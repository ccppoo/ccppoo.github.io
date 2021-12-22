---
layout: post
title: AWS Codestar로 디스코드 봇 만들기 - 2
author: ccppoo
hide_title: false
color:
feature-img:
thumbnail:
tags: [AWS, CodeStar, Discord-bot, Python]
---

EC2 키페어 만들고, 만들어진 프로젝트 탬플릿 확인하기

## 이번 글에서 진행하는 것

- EC2 키 페어 만들기

- 프로젝트 생성하기

  - Github에 만들어진 레포지토리 확인하기

  - **Github Desktop**을 이용해 내 컴퓨터로 **Clone**해서 개발 환경 만들기

<br>

### EC2 키 페어 만들기

인스턴스, 즉 여러분이 만든 클라우드에 접근하기 위한 키(key)를 발급 받아야 됩니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch2/1_make_key.png" %}

**키 페어**를 검색해 클릭합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch2/2_make_key.png" %}

**키 페어 생성**을 클릭합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch2/3_make_key.png" %}

여러분이 만든 EC2 인스턴스에 접근할 수 있는 **자격**을 만드는 겁니다.

AWS 입장에서는 인스턴스에 접근할 때 이 사람이 진짜 과연 인증된 사람인지를 확인하기 위해서

사용자를 생성하고 주어진 키가 있어야 접근을 허가는 맥락입니다.

여러분의 계정 이름과 상관 없는 열쇠의 이름을 지어주는 것이니 편하신대로 하시면 됩니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch2/4_make_key.png" %}

키 생성을 하면 위 화면과 같이 보이실 겁니다.

다운로드된 파일은 프로젝트 폴더에 **넣지 마시고** 일단은 바탕화면 같이 잘 보이는 곳으로 이동시켜주세요

{% include image.html src="/assets/img/aws-making-discord-bot/ch2/5_make_key.png" %}

다시 ec2 생성 화면, [AWS Codestar로 디스코드 봇 만들기 - 1](./2021-12-14-Discordbot-with-CodeStar-1.md)
마지막에 잠시 멈춘 화면으로 돌아오시면 새로고침 아이콘을 클릭하시면 선택이 됩니다.

다 했으면 **NEXT** 클릭!!

<br>

### 프로젝트 생성하기

{% include image.html src="/assets/img/aws-making-discord-bot/ch2/6_create_project.png" %}

지금까지 설정한 사항을 검토하는 마지막 장입니다.

특별한 점이 없다면 **프로젝트 생성**을 클릭해 진행합시다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch2/7_create_project.png" %}

이제 클라우드 인스턴스가 만들어지기까지 10분 정도 소요되는 동안 AWS가 만들어 놓은 것을 확인합시다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch2/8_create_project.png" %}

레포지토리와 이슈는 Github에 만들어진 레포지토리와 연동 된 것으로, Github에서 작성한 이슈를 AWS Console에서도 확인할 수 있습니다.

<br>

#### Github에 만들어진 레포지토리 확인하기

{% include image.html src="/assets/img/aws-making-discord-bot/ch2/9_create_project.png" %}

여러분의 깃허브 페이지에 들어가보면 생성된 프로젝트의 이름과 설명이 일치하는 것을 볼 수 있습니다.

수 많은 폴더와 코드들은 AWS가 만들어준 템플릿입니다.

<br>

#### Github Desktop을 이용해 내 컴퓨터로 Clone해서 개발 환경 만들기

{% include image.html src="/assets/img/aws-making-discord-bot/ch2/10_clone_project.png" %}

여러분의 레포지토리에 들어가 초록색 **Code** 버튼을 클릭하고 URL을 복사합니다.

아니면 직접 웹 페이지 URL을 복사해도 상관없습니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch2/11_clone_project.png" %}

Github Desktop을 열어 좌측 상단의 File 메뉴를 클릭, **Clone Repository**를 클릭합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch2/12_clone_project.png" %}

복사한 URL를 붙여넣기를 하고 **Clone**을 클릭합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch2/13_clone_project.png" %}

VSCode를 설치하셨다면 **Crtl+Shift+A**를 눌러 VSCode를 여시고,

이외 다른 IDE를 사용하신다면 클론한 레포지토리 폴더를 여시면 됩니다.

위 사진과 같이 AWS가 만들어준 템플릿 파일들이 있는 것을 볼 수 있습니다.

<br>

### 마치며

이제 초기 세팅이 끝났습니다.

프로그래밍이 처음이시라면 지금까지 과정이 힘들 수 있습니다.

근데 솔직히 처음하는데 처음부터 잘 할 수 없잖아요?

마저 기본적인 코드를 작성해봅시다!
