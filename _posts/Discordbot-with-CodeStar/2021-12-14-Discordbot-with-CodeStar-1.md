---
layout: post
title: AWS Codestar로 디스코드 봇 만들기 - 1
author: ccppoo
hide_title: false
color:
feature-img:
thumbnail:
tags: [AWS, CodeStar, Discord-bot, Python]
---

AWS에서 프로젝트를 생성하고 내 컴퓨터에서 확인하기

## 이번 글에서 진행하는 것

- AWS CodeStar 프로젝트 생성하기

  - AWS Console 들어가기

  - CodeStar 프로젝트 생성하기

- AWS를 통해서 생성된 Github 프로젝트 내 컴퓨터로 가져오기

- 키페어 만들기

## AWS CodeStar 프로젝트 생성하기

### AWS Console 들어가기

구글에 AWS를 검색하고 메인 사이트에 들어가면 다음과 같은 화면이 보이실 겁니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/1_go_to_console.png" %}

그리고 로그인을 하시면 **내 계정** 밑에 **AWS Management Console**을 클릭합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/2_go_to_console.png" %}

위와 같은 화면이 보이시는데 이번 실습에 사용하는 AWS CodeStar에 들어가기 위해 검색을 합니다

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/3_go_to_console.png" %}

보이시는 CodeStar 서비스를 클릭합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/4_go_to_console.png" %}

CodeStar에 들어오시면 **프로젝트 시작하기**를 클릭합니다.

### CodeStar 프로젝트 생성하기

여기서는 프로젝트를 시작하기 위한 기본적인 세팅, 템플릿을 선택하는 과정입니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/5_make_project.png" %}

보시다 시피 여러 탬플릿이 있고, 나중에 익숙해지시면 다양한 언어로 프로젝트를 생성할 수 있습니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/6_make_project.png" %}

Python(Flask) 템플릿을 선택하고 **Next**를 클릭합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/7_make_project.png" %}

원하시는 프로젝트 이름을 설정하시고, 프로젝트 리포지토리에 Github을 선택합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/8_make_project.png" %}

Github에 연결하기 위해서 AWS가 여러분의 Github 계정에 접근할 수 있는 권한이 필요하기 때문에 **Github에 연결**을 합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/9_make_project.png" %}

위와 같이 창이 뜨는데, AWS가 코드 탬플릿을 여러분의 Github 계정에 직접 만들어주고 github의 issue 내용도 연동하기 위한 것입니다.

연결이름은 단순히 연결에 대한 이름일 뿐이고, 여러분의 계정 이름과 상관 없는 것이니 마음대로 지으셔도 됩니다.

그리고 **Github에 연결**을 클릭해주세요

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/10_make_project.png" %}

새 앱 설치를 클릭해주세요.

여기서 말하는 '앱'은 .exe(OS : Windows) 또는 .app(OS : MacOS)과 같이 컴퓨터에 설치하는 프로그램이 아니라

Github에서 다양한 웹 서비스와 연동할 수 있는 앱을 의미합니다.

예를 들자면, 여러분이 웹 사이트를 가입할 때 해당 사이트에 직접 가입하는 것 대신

"Google 계정을 이용해 가입", "카카오톡 계정을 이용해 가입"과 같은 맥락입니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/11_make_project.png" %}

그리고 **install**를 클릭해주세요

모든 레포지토리(Repository)에 접근 권한을 AWS가 요구하는 이유는 CodeStar 프로젝트를 Github과 연동하면

AWS가 직접 레포지토리를 새롭게 만들기 때문입니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/12_make_project.png" %}

그러면 자동으로 앱 ID가 첨부되고, **연결**을 클릭해서 진행하면 됩니다.

---

**저는 자동으로 ID가 안채워졌는데요?**

만약 자동으로 첨부가 안된다면 Github 사이트에 들어가 본인 계정의 **Settings**에 들어갑니다

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/13_make_project.png" %}

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/14_make_project.png" %}

스크롤해서 내려가면 Application이 보이실 겁니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/15_make_project.png" %}

여기까지 정상적으로 위 과정까지 진행되었다면 아까 말한 설치된 AWS 앱이 보이실 겁니다.

Configure를 클릭해주세요

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/16_make_project.png" %}

들어 오시고, 여러분의 보이시는 페이지의 주소창의 마지막 부분에 있는 숫자들이 설치한 앱의 ID입니다.

이 숫자를 복사해서

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/12_make_project.png" %}

빨간색으로 가려진 부분에 붙여 넣기를 해주시면 됩니다.

---

여기까지 오셨다면 아래와 같은 화면을 보실 수 있습니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/17_make_project.png" %}

레포지토리 설명은 나중에 AWS가 탬플릿 코드가 포함된 레포지토리를 만들어줄 때 첨부되는 설명입니다.

선택사항이니 마음대로 작성해주세요.

**퍼블릭**과 **프라이빗**이 있는데 여러분의 프로젝트가 외부로 공개되는지 안되는지 여부를 선택하는 겁니다.

저는 당당하니깐 누구나 볼 수 있도록 퍼블릭을 선택하겠습니다.

사실 여러분의 코드는 누가 볼 가능성은 매우 낮습니다.

부끄러워하지 마시고 당당하게 여러분의 코드를 공개하십쇼

{% include image.html src="/assets/img/aws-making-discord-bot/ch1/18_make_project.png" %}

다음으로 여러분의 봇이 작동할 인스턴스, '클라우드'을 선택하는 과정입니다.

인스턴스 유형은 컴퓨터 성능과 같은 의미인데 여기서 세부적으로 다루지 않을 거지만,

이번 실습에서 만드는 봇은 기본 선택 옵션 **t2.micro**로 충분합니다.

가격이 걱정되신다면 **t2.nano**를 선택하셔도 상관없습니다.

제가 선택하는 t2.micro의 요금은 시간당 0.0116 USD, **시간 당 13.73원**... 입니다.

VPC와 서브넷은 선택 항목에서 아무거나 고르셔도 크게 상관이 없습니다.

## 키 페어 만들기

키 페어는 여러분이 나중에 콘솔(CLI)을 통해서 AWS에 접속하기 위한 용도로 쓰입니다.

분량이 많은 것 같으니 다음 장에 이어서 하겠습니다.

지금 켜놓은 창 나가지 말고 바로 다음장으로!!
