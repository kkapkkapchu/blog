---
title: "[GIT] GIT 명령어"
date: 2019-02-21 14:21:00
categories:
  - Web
  - GIT
tags:
  - git
  - command
---

## 원격 저장소 URL 변경하기

기존 원격 저장소 URL을 변경하기 위해 `git remote set-url` 명령어를 사용합니다.

```sh
$ git remote -v
# View existing remotes
origin  https://github.com/user/repo.git (fetch)
origin  https://github.com/user/repo.git (push)

$ git remote set-url origin https://github.com/user/repo2.git
# Change the 'origin' remote's URL

$ git remote -v
# Verify new remote URL
origin  https://github.com/user/repo2.git (fetch)
origin  https://github.com/user/repo2.git (push)
```

## 원격 저장소 복제하기

원격 저장소를 복제해서 가져올때는 `git clone`을 사용한다.

```sh
$ git clone `git 주소`
```

## 원격 저장소 추가하기

원격 저장소를 추가할때는 `git remote add`를 사용한다.

```sh
$ git remote add `저장소 이름` `git 주소`
```
