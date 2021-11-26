# backend-ui



# QR 생성 및 증명서 발급을 위한 UI

> ACA-PY와 Aries-React-Native를 DIDComm으로 연결한 후, 증명서를 발급합니다.



# 실행순서

> Von-network 실행 -> ACA-PY 중 Faber 실행 -> UI 실행



## Von-network실행

[리포지토리](https://github.com/SNU-Blockchain-2021-Fall-Group-H/von-network)

root폴더 위치에 가서

```
./manage start --logs
```

위 명령어를 통해 네트워크를 실행함. 데모를 돌리는동안은 켜놓으면 됨.

모두다 끝난 후에는 

```
./manage down
```

으로 종료함



## ACA-PY Faber 실행

[리포지토리](https://github.com/SNU-Blockchain-2021-Fall-Group-H/aries-cloudagent-python)

Root>demo 위치로 이동후

```
./run_demo faber
```

를 통해 실행 함. 



## UI실행

[root위치](.)

여기서에서 다음 커맨드를 실행함

```
npm run lite
```

lite서버가 실행되며 [UI](localhost:3000)가 실행됨. 가운데 Click QR을 클릭하면  QR코드가 생성될 것임. 생성되지 않는다면, 앞에 켜져야 하는 것들이 잘 안켜졌을 수도 있음. F12를 눌러 로그를 참조바람 
