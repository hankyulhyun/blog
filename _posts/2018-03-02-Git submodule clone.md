---
layout: post
category : etc.
tags : [Git]
title: Git Submodule Clone 하기
---
<br/>

Github에서 관심있는 프로젝트가 생겨서 fork를 했는데 시키는대로 했는데 안된다. 이유를 보니 3개의 git submodule로 되어 있는 프로젝트가 폴더만 생성되고 내용이 없었다. Git submodule을 clone할때는 다음과 같이 하면 된다.


~~~bash
> git clone PROJECT_PATH
> git submodule init
> git submodule update
~~~


빌드 잘 된다 =)

<br/>

## 참고

1. Git 도구 - 서브모듈 [https://git-scm.com/book/ko/v1/Git-%EB%8F%84%EA%B5%AC-%EC%84%9C%EB%B8%8C%EB%AA%A8%EB%93%88](https://git-scm.com/book/ko/v1/Git-%EB%8F%84%EA%B5%AC-%EC%84%9C%EB%B8%8C%EB%AA%A8%EB%93%88) 