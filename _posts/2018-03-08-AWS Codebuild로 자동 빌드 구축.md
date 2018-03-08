---
layout: post
category: AWS 
tags : [AWS]
title: AWS Codebuild로 자동 빌드 구축
---

<br/>

AWS S3로 정적 웹 호스팅을 시작했지만 매번 블로그 글을 쓸 때마다 빌드해서 손으로 옮기는건 매우 귀찮은 일이다. 따라서 github에 push 할 때마다 자동으로 빌드하여 배포가 되도록한다. AWS의 codebuild 서비스를 사용할 것이다.

##  빌드 프로젝트 생성

먼저 AWS 콘솔에 로그인하여 codebuild에 들어간 후 프로젝트를 만든다.

![cb_console](https://s3.ap-northeast-2.amazonaws.com/image.hankyul.io/2018030801.jpg){: .center-image }

아래와 같이 내용을 채워 넣는다.

### 1. 프로젝트 구성
* 프로젝트 이름 : 빌드 프로젝트의 이름을 기입한다.

### 2. 소스: 빌드 대상
* 소스 공급자 : Github
* 리포지토리 : 내 계정의 리포지토리 사용
* 리포지토리 선택 : Blog 소스가 있는 리포지토리 선택
* Git Clone 깊이 : 1
* Webhook : 체크, 체크시 푸시 할 때 마다 자동으로 이 빌드 프로젝트가 실행된다.
* 배지 : 체크 안함, 내용을 잘 모름

### 3. 환경: 빌드 방법
* 환경 이미지 : AWS CodeBuild에서 관리하는 이미지 사용
* 운영 체제 : Ubuntu
* 런타임 : Ruby (Jekyll은 ruby로 되어 있다)
* 런타임 버전 : aws/codebuild/ruby:2.3.1
* 권한이 있음 : 체크 안함
* 빌드 사양 : 소스 코드 루트 디렉터리에서 buildspec.yml 사용
* buildspec 이름 : buildspec.yml
* 인증서 : 인증서 설치 안함

### 4. 아티팩트: 이 빌드 프로젝트의 아티팩트를 넣을 위치
* 유형 : 아티팩트 없음, 일반적인 다른 어플리케이션에서는 아티팩트를 넣을 위치를 선택하게 되는데 jekyll 프로젝트를 s3에 넣을 경우 아티팩트 설정시 s3 루트 아래에 _site 폴더 생성 후 그 아래에 정적 웹 페이지가 들어간다고 하여 사용하지 않는다.

### 5. 캐시
* 유형 : 캐시 없음

### 6. 서비스의 역할
* 계정에 서비스 역할 생성 및 적당한 역할 이름 배정

### 7. VPC
* VPC : No VPC

### 8. 고급설정
s3에 빌드 결과를 넣는 과정에서 aws console 명령어를 사용해야 한다. 따라서 명령어 사용시 인증에 필요한 설정을 해주어야 한다. 환경 변수에 다음과 같은 이름을 추가하고 자신의 값을 넣는다.
* AWS_ACCESS_KEY_ID
* AWS_SECRET_ACCESS_KEY 


##  buildspec.yml 생성

블로그 프로젝트의 root 바로 아래에 buildspec.yml 파일을 만들어서 build 방법을 명시해주어야 한다.

<br/>

**buildspec.yml**

~~~ruby
version: 0.2
   
phases:
  install:
    commands:
      - gem install jekyll jekyll-paginate jekyll-sitemap jekyll-gist jekyll-feed
  pre_build:
    commands:
      - export LC_ALL="en_US.UTF-8"
      - locale-gen en_US en_US.UTF-8
      - dpkg-reconfigure locales
  build:
    commands:
      - echo "******** Building Jekyll site ********"
      - jekyll build
      - echo "******** Uploading to S3 ********"
      - aws s3 sync _site/ s3://blog.hankyul.io
~~~


* **version** : 현재 0.2가 최신 설정이다. 0.1도 빌드 자체는 호환이 되지만 locale 설정이 안먹는다.
* **phases > install > commands** : 빌드 실행 전 설치해야 하는 gem을 설치한다.
* **phases > pre_build > commands** : 블로그에 영어 이외의 문자가 들어가 있어서 추가로 locale 설정이 필요하다.
* **phases > build > commands** : 실제 빌드 수행하고 s3에 빌드 결과를 동기화 한다.

<br/>

이제 소스를 수정하고 github에 push 하면 자동으로 빌드가 되고 블로그 배포가 진행된다


* * * 

## 참고 자료

1. [Using AWS CodePipeline and CodeBuild to update a Jekyll website](https://alexbilbie.com/2016/12/codebuild-codepipeline-update-jekyll-website)
2. [AWS CodeBuild 문제 해결](https://docs.aws.amazon.com/ko_kr/codebuild/latest/userguide/troubleshooting.html)
3. [AWS CodeBuild의 빌드 사양 참조](https://docs.aws.amazon.com/ko_kr/codebuild/latest/userguide/build-spec-ref.html)



















