---
title: "[React] 리액트에서 뒤로가기 시 브라우저 확인 창 띄우기" 
categories:
    - Web
    - React
tags:
    - onBeforeUnload
    - setRouteLeaveHook
date: 2018-06-15 17:08:00
---

### React에서 라우터 이동 시 발생하는 이벤트
페이지를 벗어날 때(새로고침/닫기/뒤로가기)를 잡고 싶을 때가 있을 것이다. 
대부분 블로그에서는 `onBeforeUnload` 이벤트에 대해서만 설명을 하고 있다.

하지만 `onBeforeUnload` 이벤트는 React에서 뒤로가기 시에 호출되지 않는다!!!
React에서 뒤로가기 이벤트를 잡고 싶을 때에는 `setRouteLeaveHook`를 사용하면 된다.
`setRouteLeaveHook` 는 리액트에서 라우터가 바뀔 때 호출된다.

`setRouteLeaveHook` 이벤트를 사용하기 위해서는 `withRouter` 로 컴포넌트를 감싸줘야 한다.
그리고 router 는 기본으로 props 로 넘어오지만 route 는 현재 컴포넌트가 하위 레벨이라면 부모 컴포넌트로부터 전달받아야 한다.

```jsx
import { withRouter } from 'react-router';

class WriteMail extends Component {

    componentDidMount() {
        // 라우터 이동시
        this.props.router.setRouteLeaveHook(this.props.route, () => {
            // 변경사항이 없다면 그냥 브라우저를 벗어남
            if (!this.isChange()) return;
        
            // 뒤로가기 확인 창
            if (!window.confirm("이 페이지를 벗어나면 마지막 저장 후 수정된 내용은 저장되지 않습니다.")) {
                // 취소 버튼 클릭
                window.location.href = '/writeMail';
                // window.history.back();  // React 에서 주의해서 쓸 것!!
                return false;
            } else {
                // 확인 버튼 클릭
                return true;
            }
        })
    }
}

export default withRouter(WriteMail, { withRef: true });
```

브라우저를 벗어날지 말지 확인하는 창을 띄운 후 사용자가 취소 버튼을 클릭했다면, 이전 주소로 바꿔주어야 한다.
이 때 `window.history.back()` 는 React 에서 매우 조심해야 한다!!
이전 포스팅에서처럼 리액트는 SPA이기 때문에 url 을 바꾼다해도 그게 제대로 history에 들어가지 않는거 같다.
그래서 `window.location.href`로 아예 이전 url 로 바꿔준 것이다.


### Redux를 사용한다면...
위의 `WriteMail` 컴포넌트의 상위 컴포넌트에서 하위 컴포넌트에 정의된 함수를 사용하려면
`this.refs.refWriteMail.getWrappedInstance().함수이름()` 를 사용했을 것이다.
하지만 `withRouter`로 한번 더 감쌌기 때문에 
`this.refs.refWriteMail.getWrappedInstance().refs.wrappedInstance.함수이름()`으로 사용해야 한다.