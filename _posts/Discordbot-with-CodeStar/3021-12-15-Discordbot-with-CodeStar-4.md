---
layout: post
title: AWS Codestar로 디스코드 봇 만들기 - 4 (끝)
author: ccppoo
hide_title: false
color:
feature-img:
thumbnail:
tags: [AWS, CodeStar, Discord-bot, Python]
---

수정한 코드를 AWS CodeStar에 배포하고 결과물 확인하기

## 이번 글에서 진행하는 것

- 인스턴스(AWS EC2 인스턴스)에 secret.json 만들기

  - ec2에 접속하기

  - secret.json 만들기

- 인스턴스(AWS EC2 인스턴스)에 배포하기

  - 배포하기 전에 configuration 작성하기

  - 배포 후 빌드된 결과물 확인하기

- 마치며

<br>

## 인스턴스(AWS EC2 인스턴스)에 secret.json 만들기

### ec2에 접속하기

디스코드 봇 토큰이 담긴 json 파일을 AWS에서 만든 인스턴스에 직접 생성하고 작성할 겁니다.

2번 째 목차(3번 째 글)에서 [EC2 키 페어](./Discordbot-with-CodeStar-2.html)를 만들고 잘 보이는 곳에 옮겨 뒀나요?

이제 이걸 써야합니다.

여러분이 다운로드 받은 \*.pem 파일이 있는 디렉토리 위치를 찾아서 콘솔창을 여세요

(바탕화면에 있는 경우 : 내 문서 -> 바탕화면 -> 폴더 창 주소 복사 -> 콘솔창에 cd 입력후 복붙 후 엔터)

{% include image.html src="/assets/img/aws-making-discord-bot/ch4/1_access_ec2.png" %}

EC2 키를 생성했던 AWS Console 페이지로 가서 EC2 대시보드로 갑니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch4/2_access_ec2.png" %}

실행 중인 인스턴스를 클릭합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch4/3_access_ec2.png" %}

만들어 놓은 인스턴스를 체크를 하고 연결을 클릭합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch4/4_access_ec2.png" %}

SSH 클라이언트를 클릭하고 체크 한 버튼을 클릭해 명령어를 복사합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch4/5_access_ec2.png" %}

여러분의 \*.pem 파일 위치로 이동한 콘솔창에 이 명령어를 복사해서 붙여 넣기를 합니다(우클릭을 해도 됨)

{% include image.html src="/assets/img/aws-making-discord-bot/ch4/6_access_ec2.png" %}

잘 따라 오셨다면 위와 같은 화면을 보실 수 있을 겁니다.

여러분은 지금 클라우드 컴퓨터에 접속한 상태입니다! ㅊㅋㅊㅋ

### secret.json 만들기

{% include image.html src="/assets/img/aws-making-discord-bot/ch4/8_access_ec2.png" %}

`ls` 명령어를 통해 우리가 CodeStar를 통해 만든 `python-flask-service`를 볼 수 있습니다.

이름만 flask일 뿐입니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch4/9_access_ec2.png" %}

위와 같이 명령어를 작성합니다

{% include image.html src="/assets/img/aws-making-discord-bot/ch4/10_access_ec2.png" %}

`vi` 명령어는 vim 에디터에 진입하는 의미입니다

#### 아래 순서와 같이 반드시 따라하세요

1.**'i' 를 입력합니다**

```json
{
  "token" : "
```

2.위와 같이 작성하세요

```json
{
  "token" : "<여러분이 복붙한 토큰>"
```

3.디스코드 봇 토큰(잊어버렸으면 디스코드 포탈로 가서 복사)을 복사하고 콘솔창에 우클릭을 하세요 (붙여넣기)

```json
{
  "token": "<여러분이 복붙한 토큰>"
}
```

4.괄호를 닫아주고 **esc** 그리고 **:wq**를 입력하고 **Enter**를 누르세요

{% include image.html src="/assets/img/aws-making-discord-bot/ch4/11_access_ec2.png" %}

5.**cat secret.json**을 입력해 화면과 같이 보이는지 (사진에 있는 토큰 말고 여러분이 발급한 토큰) 확인하세요

자! 그러면 여러분이 이전 장에서 만든 `secret.json` 파일과 똑같은 것을 클라우드 컴퓨터에서도 만든겁니다 ㅊㅋㅊㅋ

## 인스턴스(AWS EC2 인스턴스)에 배포하기

이제 필요한 비밀 토큰이 클라우드에도 있으니 남은일은 코드만 클라우드에 배포하면 되는 겁니다.

그럼 작성한 코드를 Github Desktop을 이용해 **커밋**하고 **푸쉬**를 합시다

{% include image.html src="/assets/img/aws-making-discord-bot/ch4/12_deploy_codestar.png" %}

사진처럼 왼쪽에 3개의 파일이 있어야합니다.

저거는 MacOS든 Linux든 뭐든간에 일단 저렇게 되어 있어야합니다.

나 저거말고 건드린거 없음

그리고 **Commit to master** 클릭합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch4/13_deploy_codestar.png" %}

여러분의 코드 변경사항은 *내 컴퓨터*에서만 된 것이므로, 깃허브 저장소에는 반영이 되도록 **Push Origin**을 클릭합시다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch4/13_deploy_codestar.png" %}

AWS CodeStar의 레포지토리를 보시면 우리가 커밋한 것을 알고 AWS는 업데이트 된 코드를 반영 

<br>

<br>

### 배포 후 빌드된 결과물 확인하기

<br>

## 마치며

새로운 걸 접한다는 건 낯설고 잘못될까 두려울 수 밖에 없는 일 입니다.

막상 낯설었던 과정을 여러번 반복하다 보면 두려움도 둔감해지고, 늘 하던 일처럼 쉽게 느껴질 겁니다.

아마 프로그래밍을 전공으로 하셨던 분이라면 **git**을 사용하는데 있어 문턱을 느끼셨을 확률이 컸을 겁니다.

검은 화면에 엄청나게 긴 줄들이...

프로그래밍은 결국 타이핑(typing)이 대부분이라는 인식이 보편적인데 막상 하다보면

돌아가는 방식에 대한 원리, 흐름을 먼저 이해하고 CLI에 명령어를 적든 코드를 적는

**하향식 접근**이 이해하기 더 좋은 방법임을 느끼실 수 있을 겁니다.

뭐... 그래도 결국 자신이 직면한 문제 상황이나 **만들고 싶은 앱**이라는 동기가 가장 중요하다고 생각합니다.

여기까지 오느라 수고하셨습니다.
