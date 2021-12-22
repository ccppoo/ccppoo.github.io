---
layout: post
title: AWS Codestar로 디스코드 봇 만들기 - 3
author: ccppoo
hide_title: false
color:
feature-img:
thumbnail:
tags: [AWS, CodeStar, Discord-bot, Python]
---

Discord Developer Portal에서 봇 등록하고 돌아가는지 확인하기

**이번 장을 읽기 전에 디스코드 채널을 미리 만들어주세요!**

## 이번 글에서 진행하는 것

- 디스코드 디벨로퍼 포털에서 봇 등록하기

  - 조심하세요!

  - 디스코드 방에 봇 초대하기

    - 권한 설정과 초대 링크(URL) 만들기

      - 디스코드 용어 정리 - 채널 vs 서버

    - 봇 초대하기

- 봇 작동시키기

  - 토큰 생성하기

  - 샘플 코드로 봇 호스팅 하기

- 남은 작업들

<br>

## 디스코드 디벨로퍼 포털에서 봇 등록하기

### 조심하세요!

봇을 등록하기 위해서는 디스코드로부터 토큰을 받아야 합니다.

디스코드 포털에서는 무분별한 스팸 봇을 막기 위해서 각자 인증된 절차에 따라서 봇을 운영할 수 있는 토큰을 발급해줍니다.

봇의 운영 개수는 제한이 없지만, 잘못된 로직으로 의도치 않게 도배가 되는 등 트롤링이 발생하면 디스코드 API(봇 기능) 사용을 박탈 당할 수 있으니 조심해야 합니다.

**토큰은 같은 팀원이 아닌 이상 외부로 유출되어선 안됩니다**

---

### 디스코드 방에 봇 초대하기

#### 권한 설정과 초대 링크(URL) 만들기

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/1_register_bot.png" %}

디스코드 앱이 아닌 웹 사이트, Developer Portal에 들어갑니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/2_register_bot.png" %}

새로운 봇, 애플리케이션을 만들기 위해서 **New Application**을 클립합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/3_register_bot.png" %}

여기서 Application Name은 실제 디스코드 앱에서 보이는 **봇의 이름이 아닌** 애플리케이션의 이름입니다.

개발을 하면서 어떤 용도의 봇인지 알 수 있게끔 이름을 지어주시면 됩니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/4_register_bot.png" %}

옆에 **Bot** 메뉴에 들어가 봇을 추가합니다.

경고문이 뜨는데 큰 의미는 없는 경고문입니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/5_register_bot.png" %}

봇을 생성했습니다!

**username**이 디스코드 앱에서 보이는 이름입니다.

저는 일단 앱 이름과 동일하게 사용하겠습니다. 바꾸고 싶다면 바꾸셔도 이해하는데 문제 없습니다.

##### 디스코드 용어 정리 - 서버 vs 채널

디스코드 봇을 개발하다보면 비슷한 용어가 사용되는데 가장 중요한 두 단어,

**서버**과 **채널**에 대해서 설명드리겠습니다.

###### 서버, Server

서버는 *사람들이 속해있는 공간*입니다.

서버는 여러개의 채널을 가질 수 있으며, 서버에 속해 있는 사람들이 각자 채널에 들어가

채팅을 할 수도 있고, 음성 채팅(보이스 톡)을 할 수 있습니다.

###### 채널, Channel

채팅 채널은 카카오톡의 채팅방과 유사합니다.

음성 채널은 전화 연결과 똑같고, 음성 채팅에 연결이 되어 있으면서 동시에 채팅 채널에서 채팅을 할 수 있습니다.

단, 음성 채널은 한 번에 하나씩 연결할 수 있습니다.

#### 봇 초대하기

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/6_register_bot.png" %}

옆에 있는 메뉴, **OAuth2 - URL Generator**를 클릭하면 위와 같은 화면을 보실 수 있습니다.

**URL Generator**는 봇을 초대할 수 있는 링크를 생성하는 곳 입니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/7_register_bot.png" %}

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/8_register_bot.png" %}

**Bot**을 클릭하고, General Permissionsd에 **Administrator**를 클릭하면 URL이 생성됩니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/9_register_bot.png" %}

이 URL을 복사한 뒤 링크로 접속하시면 위와 같은 화면이 나옵니다

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/10_register_bot.png" %}

만들어 놓은 디스코드 채널을 선택하고 초대를 하면 위와 같이 나옵니다.

현재는 봇을 **호스팅**하고 있지 않은 상태이므로 오프라인입니다.

<br>

## 봇 작동시키기

이제 봇이 동작할 수 있도록 간단한 샘플 코드를 보도록 합시다.

### 토큰 생성하기

먼저 봇이 디스코드에 접근하기 위해서 맨 처음에 언급했듯 **토큰**이 필요합니다.

다시 디스코드 애플리케이션 사이드 메뉴 **Bot**으로 돌아가면 아래와 같은 화면을 볼 수 있습니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/5_register_bot.png" %}

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/11_register_bot.png" %}

Reveal Token을 클릭하면 화면과 같이 봇이 보이는데

이 토큰이 바로 여러분이 외부로 유출되어선 안되는 토큰입니다.

토큰은 언제나 디스코드 포탈로 오면 다시 볼 수 있으니 보는 방법만 확인하고 샘플 코드로 넘어가도록 합시다.

### 샘플 코드로 봇 호스팅 하기

```python
# helloworld/application.py

import os.path
import json
import discord


class MyClient(discord.Client):

    async def on_ready(self):
        print('Logged in as')
        print(self.user.name)
        print(self.user.id)
        print('------')

    async def on_message(self, message):
        if message.author.id == self.user.id:
            return

        if message.content.startswith('!hello'):
            await message.reply('Hello!', mention_author=True)


client = MyClient()

my_path = os.path.dirname(__file__) + '../../secret.json'

with open(my_path, mode='r') as fp:
    secret = json.load(fp)
    token = secret['token']

client.run(token)
```

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/11_register_bot.png" %}

이 코드를 처음에 AWS가 만들어 줬던 탬플릿 파일에서 위와 같이

`helloworld/application.py`에 붙여넣기를 합니다.

`helloworld/flaskrun.py`는 사용하지 않으므로 삭제합니다.

여러분의 토큰은 레포지토리에 공개되면 안되기 때문에 임시로 `secret.json` 파일을 만들어 간접적으로 가져오도록 할 겁니다.

`discord-bot` 폴더 아이콘이 보이는 위치에 `secret.json` 파일을 만들어 아래와 같이 작성합니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/14_run_bot.png" %}

그리고 `requirements.txt` 내용을 모두 지우고 아래와 같이 작성합니다.

```txt
discord.py==1.7.3
```

나중에 AWS에 배포가 되어서 클라우드에서 실행될 때 현재 컴퓨터와 같은 환경을 가지도록

`requirements.txt` 파일에 있는 모듈을 기록해두는 것입니다.

여기까지 왔으면 파일 구성은 이렇게 됩니다

{% include aligner.html images="/aws-making-discord-bot/ch3/16_run_bot.png,/aws-making-discord-bot/ch3/20_run_bot.png" column=2 %}

#### AWS CodeStar에 배포하기 전에 내 컴퓨터에서 테스트하기

코드를 AWS CodeStar에 배포하기 전에 먼저 작성한 코드가 제대로 작동하는지 확인해야합니다.

그러기 위해서 실행할 수 있도록

VSCode를 사용하신다면 **Ctrl + `(back tick)** 또는

커맨드 창(cmd)을 열어 실행해 봅시다.

둘 중에 아무거나 선택해도 상관 없습니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/13_run_bot.png" %}

코드를 실행하기 전에 `python -m pip install discord`를 통해 패키지를 설치하세요!

<br>
<p style="text-align:center; font-size:30px">이제 실행합시다</p>
<br>

`python application.py`

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/17_run_bot.png" %}

위와 같이 콘솔이 보일 것이고...

디스코드 앱을 보시면 아래와 같이 봇이 로그인이 된 것을 확인하실 수 있습니다.

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/18_run_bot.png" %}

테스트를 하기 위해서 `!hello`를 보내봅시다!

{% include image.html src="/assets/img/aws-making-discord-bot/ch3/19_run_bot.png" %}

만세~~!

## 남은 작업들

`.gitignore`에서 우리가 'secret.json`을 썼기 때문에 AWS CodeStar에 배포를 해도

AWS CodeStar에는 secret.json 파일이 뭔지 모르겠죠?

거의 다 왔습니다...
