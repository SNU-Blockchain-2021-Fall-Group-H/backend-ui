# Mobile App에 Credentail 발급 방법

[원본 튜토리얼](https://github.com/hyperledger/aries-cloudagent-python/blob/main/demo/AliceGetsAPhone.md)

위의 링크가 원본이며, 그대로 세팅할 것을 권장함

## 큰 흐름

1. 백엔드를 켠다 
   1. ngrok / indy-tails-server / aca-py
2. 프론트앤드를 켠다
   1. Aries-reactnative
3. 연결한다
   1. Invitation url을 복사하여 자신의 핸드폰으로 가져간 후, 웹캠을 통해 Virtual Andorid가 인식할 수 있도록한다
4. Credentail 발급
   1. Backend발급 , Frontend 수락 순서임
   2. 발급을 누른 후, 앱을 껏다 켜야함



## 결과물

- 기본 BifoldApp과 발급된 증명서.

![image-20211129220419624](/home/heejinchae/.config/Typora/typora-user-images/image-20211129220419624.png)

![image-20211129220429971](/home/heejinchae/.config/Typora/typora-user-images/image-20211129220429971.png)

## 준비물

- [ngrok](https://ngrok.com/)
- [인디 테일서버(블록체인 네트워크)](https://github.com/bcgov/indy-tails-server)

- [Aries-React-Native](https://github.com/hyperledger/aries-mobile-agent-react-native)
- Ubuntu 20.04
- Android Studio(Android 가상머신 설정)
- 본인 핸드폰

위의 설정들이 필요함



# 백엔드 세팅하기



## ngrok설정

> Local포트를 외부에서 접근할 수 있도록 하는 기능임.

ngrok을 다운받은 후 

```
ngrok http 8020
```

위의 커맨드를 터미널에서 입력함. zip파일로 했다면 압축해제한 위치에서

```
./ngrok http 8020
```

실행함

![image-20211129220719853](/home/heejinchae/.config/Typora/typora-user-images/image-20211129220719853.png)

위와 같은 메세지를 볼 것임. 위 터미널은 계속 유지시키면 되고, 킨 후엔 굳이 신경쓸 일은 없음



## 터미널에서 indy-tails-server실행

> 아래 커맨드만 실행하면 됨

```
git clone https://github.com/bcgov/indy-tails-server.git
cd indy-tails-server/docker
./manage build
./manage start

```

여기도 마찬가지로 실행후 크게 신경쓸것은 없음. 튜토리얼에선 아래와 같은 메세지가 떴는지만 꼭 확인하라 함.

![image-20211129221016855](/home/heejinchae/.config/Typora/typora-user-images/image-20211129221016855.png)



## ACA-PY작동

> 데모 폴더에가서 작동함

ACA-PY를 다운받지 않은 상태라면 다음과 같이 하면됨

```
git clone https://github.com/SNU-Blockchain-2021-Fall-Group-H/aries-cloudagent-python.git
cd aries-cloudagent-python/demo
TAILS_NETWORK=docker_tails-server LEDGER_URL=http://test.bcovrin.vonx.io ./run_demo faber --aip 10 --revocation --events

```

![image-20211129221226406](/home/heejinchae/.config/Typora/typora-user-images/image-20211129221226406.png)

위의 위치에서

```
TAILS_NETWORK=docker_tails-server LEDGER_URL=http://test.bcovrin.vonx.io ./run_demo faber --aip 10 --revocation --events
```

라는 명령어를 실행한다는 뜻

![image-20211129221525658](/home/heejinchae/.config/Typora/typora-user-images/image-20211129221525658.png)

실행 후 위와같은 메세지가 뜰 것임 그리고 끝에는

![image-20211129221623370](/home/heejinchae/.config/Typora/typora-user-images/image-20211129221623370.png)

위와 같은 것이 생성됨



# 프론트앤드 설정하기

> [Aries-mobile-agent](https://github.com/hyperledger/aries-mobile-agent-react-native)이곳에서 Android기준 설정 하면 됨

Android 스튜디오를 통해 가상머신 설정을 해야함. 아마 expo에 나와있던 설정을 따라하면 될 것 같음. 가상 Android가 웹캠을 기본 카메라로 사용하도록 세팅도 해줘야함.

```bash
git clone https://github.com/hyperledger/aries-mobile-agent-react-native
cd aries-mobile-agent-react-native
npm install

```



가장 상위폴더에 .env 파일 생성

```bash
MEDIATOR_URL=http://mediator3.test.indiciotech.io:3000?c_i=eyJAdHlwZSI6ICJkaWQ6c292OkJ6Q2JzTlloTXJqSGlxWkRUVUFTSGc7c3BlYy9jb25uZWN0aW9ucy8xLjAvaW52aXRhdGlvbiIsICJAaWQiOiAiYjE5YTM2ZjctZjhiZi00Mjg2LTg4ZjktODM4ZTIyZDI0ZjQxIiwgInJlY2lwaWVudEtleXMiOiBbIkU5VlhKY1pzaGlYcXFMRXd6R3RtUEpCUnBtMjl4dmJMYVpuWktTU0ZOdkE2Il0sICJzZXJ2aWNlRW5kcG9pbnQiOiAiaHR0cDovL21lZGlhdG9yMy50ZXN0LmluZGljaW90ZWNoLmlvOjMwMDAiLCAibGFiZWwiOiAiSW5kaWNpbyBQdWJsaWMgTWVkaWF0b3IifQ==
GENESIS_URL=https://raw.githubusercontent.com/Indicio-tech/indicio-network/main/genesis_files/pool_transactions_testnet_genesis

```

위의 두가지가 끝나면 두개의 터미널을 켠 후



각각 아래의 커맨드를 입력함

```
npm run start

```

```
npm run android

```



위의 둘 중 아래 커맨드를 입력하면 앱이 설치됨

![image-20211129222418859](/home/heejinchae/.config/Typora/typora-user-images/image-20211129222418859.png)

# 연결하기 

> Back-end 중 ACA-PY의 URL을 복사하여 핸드폰에서 QR생성함. 이후 App에서 인식

## QR 생성

ACA-PY와 App의 연결은 QR을 통해서 가능하다. 현재 세팅 기준으론, 터미널에 생성되는 QR위에 URL이 뜨는데, 이것을 복사한 후 핸드폰에서 QR을 생성하면 된다.

![image-20211129223054769](/home/heejinchae/.config/Typora/typora-user-images/image-20211129223054769.png)

http:// ~ J9 까지 해당하는 부분이며, 초대 생성시마다 달라지기 때문에 주의할 것



본인의 핸드폰에서 [QR 생성기](https://www.qr-code-generator.com/)에 가서 연결용 QR을 생성한다.

![image-20211129223247938](/home/heejinchae/.config/Typora/typora-user-images/image-20211129223247938.png)



## 인식

> 카메라가 돌아가있어서, QR도 돌려서 인식시켜야 하는듯

![image-20211129223700492](/home/heejinchae/.config/Typora/typora-user-images/image-20211129223700492.png)

기본으로 카메라가 돌아가있다. 따라서 핸드폰도 돌려서 Android카메라가 보기에 수직인 방향으로 QR을 인식시키면 연결이 되는 듯 하다. 연결이 되고나면 다음과 같다

![image-20211129223815954](/home/heejinchae/.config/Typora/typora-user-images/image-20211129223815954.png)

# 발급하기

> Back-end에서 발급 -> Front-end에서 수락

 현재 세팅 기준 바로 발급되지 않고, credentail형태를 제안하면 , Front-end에서 수락해야 최종 저장된다



## 발급하기

연결이 제대로 됬다면, ACA-PY 터미널은 대략 다음과 같다

![image-20211129223958092](/home/heejinchae/.config/Typora/typora-user-images/image-20211129223958092.png)

여기서는 간략하게 1을 누르면 된다

![image-20211129224027985](/home/heejinchae/.config/Typora/typora-user-images/image-20211129224027985.png)

위와 같은 부분이 잠시 나오고 사라질 것이다.



## 수락하기

> 앱을 한번 껏다가 킨다

![image-20211129224104451](/home/heejinchae/.config/Typora/typora-user-images/image-20211129224104451.png)

위와같이 하나 도착한 것을 볼 수 있다.

![image-20211129224118919](/home/heejinchae/.config/Typora/typora-user-images/image-20211129224118919.png)

위와같은 화면이 보이며, accept를 누르면 된다. 근데 테스트 해보니, 여러개 중복 발급하면 최초에 발급한것만 accept되고 나머지는 안되는 것 같다.

