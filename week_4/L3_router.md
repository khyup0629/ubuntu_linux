# L3 라우터

## 라우터란?

컴퓨터 네트워크 간에 데이터 패킷을 전송하는 네트워크 장치입니다. 
패킷의 위치를 추출하여, 그 위치에 대한 최적의 경로를 지정하며, 이 경로에 따라 데이터 패킷을 다음 장치로 전달합니다. 
특징은 서로 다른 네트워크 간에 중계 역할을 한다는 것입니다.

## 라우터 접근 방법

라우터에 접근하는 방법은 총 5가지가 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/141886406-a4fd2e7e-bd49-4acf-898e-8647f0fb1985.png)

1. 콘솔
2. 텔넷
3. TFTP
4. AUX : 모뎀을 통해 원격 접근
5. 네트워크 관리 시스템(Network Management System) : GUI 환경을 통한 접근

실제로 실습해 볼 방법은 `콘솔, 텔넷, TFTP`입니다.

## 라우터 콘솔 접근

pc와 라우터를 콘솔로 연결합니다.   
![image](https://user-images.githubusercontent.com/43658658/141877073-fc2144c7-f745-410c-9124-7f161d62c2a6.png)   

[장치 관리자]를 열고 포트를 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/141876619-b54108b4-a7b2-47d2-905a-9aa9cb23d89d.png)    

PuTTy를 열고 `Serial` 방식으로 연결합니다.   
![image](https://user-images.githubusercontent.com/43658658/141877145-ae774f10-9f4b-470d-845b-81abf9b44827.png)   

콘솔 창이 뜨면 [Enter]키를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/141877268-c3f148b5-1272-4373-926d-05b3c28d7924.png)   

## 라우터 공장 초기화

글로벌 모드에서 컨피그레이션 레지스터를 `0x2102`로 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/141878996-1b831211-d138-483b-aecb-be87351fd28c.png)   
* `0x2102` : 라우터의 IOS 이미지를 flash 메모리로부터 로딩합니다. 만약 flash 메모리에 IOS이미지가 없다면 TFTP, ROM의 순서로 이미지를 찾아 부팅하고 NVRAM에서 설정값을 읽어옵니다.
* `0x2141` : 손실 복구. 라우터의 IOS 이미지를 ROM으로부터 로딩합니다. NVRAM은 무시합니다.
* `0x2142` : 패스워드 복구. 라우터의 IOS 이미지를 FLASH로부터 로딩합니다. NVRAM은 무시합니다.
* `시스코 IOS` : 시스코 시스템즈의 대부분의 라우터와 현행 모든 스위치에 사용되고 있는 소프트웨어. IOS는 멀티태스킹 OS와 통합된 라우팅, 스위칭, 인터네트워킹, 텔레커뮤니케이션 기능의 패키지입니다.
* `NVRAM, FLASH, ROM` : 모두 비휘발성 메모리입니다. `ROM`은 사용자가 임의로 건드릴 수 없고, `FLASH`는 IOS 이미지파일을 저장하는데 사용됩니다. `NVRAM`은 설정파일(부팅과정 중 환경설정파일을 로드)을 저장하는데 사용됩니다. `RAM`은 장비의 전원이 켜져있는 동안 설정되는 모든 설정파일을 저장하는데, 이를 `NVRAM`에 저장하지 않으면 모두 사라져버립니다.
  * 장비를 복구할 때(비밀번호 분실) ROM과 FLASH는 운영체제가 로딩되기 전, RAM, NVRAM은 운영체제가 로딩된 후의 단계입니다. 

`show version` 명령을 통해 제일 마지막 줄에 해당 내용이 추가되었는지 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/141879755-58a48b84-d752-468f-ba63-e39b0d80ab40.png)

`write erase` 명령을 사용하여 라우터의 현재 시작 컨피그레이션을 지웁니다.   
![image](https://user-images.githubusercontent.com/43658658/141879859-70913be4-5283-41f6-abef-402ae76a80ed.png)

`reload`를 통해 재부팅합니다.   
![image](https://user-images.githubusercontent.com/43658658/141879933-3e96ed3d-a959-4e65-b264-40c3b7e78da2.png)   
컨피그레이션을 저장하겠냐는 메시지가 나오면 `no`를 입력합니다.

라우터가 다시 로드되면 `System Configuration Dialog` 대화상자가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/141880538-9be035bb-996f-486d-bab1-374d1e503572.png)   

이 과정을 거치면 라우터의 공장 초기화가 완료됩니다.

## 라우터 텔넷 설정

`do sh ip int bri` 명령을 통해 사용가능한 인터페이스의 목록을 봅니다.   
![image](https://user-images.githubusercontent.com/43658658/141884645-ee8d0ba1-5158-4e13-9bf8-de8eccea5fd7.png)

인터페이스 하나를 골라 IP를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/141884813-b4df284e-7494-4a71-9ff2-45fb1b22cc2f.png)

IP 설정이 잘 되었는지 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/141884781-80a9be60-6bb8-4dfe-b1df-a2f938861682.png)

텔넷 접속 패스워드와 enable 패스워드를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/141886139-2612996b-408a-499d-b5c9-71cde1f1679b.png)   
![image](https://user-images.githubusercontent.com/43658658/141886174-532d980e-5211-4129-97b9-aaca54e6d638.png)   
* `transport input telnet` : 해당 0~4 라인을 텔넷으로 접속한다고 설정.

텔넷으로 설정한 IP로 접속해보면 접속이 되는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/141885076-07c96ef5-3f92-4b30-99c9-c2d29971f85b.png)

## AUX 포트

콘솔 포트 밑에 `AUX` 포트라는 것이 있습니다.
이 포트에는 모뎀을 연결할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/141877720-7236accd-bdca-45ed-b79e-25836040c7b9.png)   

모뎀을 연결해 놓으면 원격지에서도 모뎀을 통해 라우터에 명령어를 입력할 수 있습니다.
모뎀을 이용한 라우터 구성은 기존의 네트워크에 문제가 발생해 텔넷으로 접근이 불가능할 때, 그리고 콘솔을 연결하자니 너무 거리가 먼 곳일 때 원격지에서 라우터에 접근을 가능하게 합니다.

## 라우터 로컬 백업과 복원

현재 `running-config`(현재 작동중인 설정에 관한 모든 것이 적힌 파일)를 플래시 메모리에 `aa.txt`라는 이름으로 복사합니다.   
복사가 끝나면 `more flash:aa.txt`로 잘 복사되었는지 파일을 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/141887366-a048add4-6693-404f-9e13-4e724ba18f03.png)   

`hostname test`로 호스트이름을 `test`로 바꿉니다.   
![image](https://user-images.githubusercontent.com/43658658/141887596-eac95b38-fce9-44fd-bed8-7f87e1c8fbef.png)   

`configure replace flash aa.txt`를 통해 백업을 시작하고, 중간에 `y`를 입력해 백업을 진행시킵니다.   
![image](https://user-images.githubusercontent.com/43658658/141887728-86bc27fd-f83a-4d0e-aecd-a745847f48bf.png)   
다시 `hostname`이 `Router`로 돌아간 것을 확인할 수 있습니다.

## TFTP를 통한 리모트 백업과 복원

원격 pc에 TFTP 서버를 구축하고 라우터의 설정 파일을 백업해 보겠습니다.

먼저, 원격 pc에 TFTP 서버를 구축하기 위해 TFTP 툴을 다운로드 받습니다.   
=> https://tftp.kr.uptodown.com/windows/download

[설치 완료]   
![image](https://user-images.githubusercontent.com/43658658/141925429-2cfb52d5-4c4a-439f-91e4-aef9c9bad176.png)

라우터에서 `running-config`를 TFTP 전송합니다.   
![image](https://user-images.githubusercontent.com/43658658/141925616-9600d907-d8ab-43b9-a240-6ff6bb995b23.png)   
`copy running-config tftp` : `running-config`를 TFTP 전송.   
원격 pc의 IP와 백업 파일의 이름을 설정합니다.

`TFTP 툴`로 가서 `[Show Dir]`를 선택하면 백업 파일이 전송된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/141926203-2afeaf75-34fe-4e92-b8a6-63f5d445d901.png)

`파일 탐색기`로 TFTP가 설치된 경로로 들어가면 백업 파일이 존재하는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/141926068-759723fa-73b9-4a48-8593-f80911227ecb.png)

복원 테스트를 위해서 라우터를 초기화합니다.   

원격 pc에 있는 백업 파일을 라우터의 `running-config` 파일로 다시 가져옵니다.   
![image](https://user-images.githubusercontent.com/43658658/141928102-b3d37e49-bc60-4c9a-9e85-fc0e6a52a064.png)   
* `copy tftp running-config` : tftp 서버에 있는 파일을 running-config로 가져옵니다.
* 가져올 원격 pc의 IP를 입력하고, 백업 파일의 이름을 작성한 뒤, `Destination filename`은 `running-config`가 맞으므로 엔터를 누릅니다.

백업 파일을 만들 당시 텔넷 접속 설정을 모두 완료한 상태이기 때문에 초기화를 하면 텔넷 접속이 불가능하고,   
백업 파일을 통해 복원을 완료한다면 텔넷 접속이 가능할 것입니다.

복원을 완료하고 텔넷 서버를 통해 접속해 보면 접속이 원활히 되는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/141928393-99705941-182c-41fb-b625-4e3ea0dc6b73.png)

---

[참고 사이트]   
* https://toastfactory.tistory.com/281
* https://www.cisco.com/c/ko_kr/support/docs/ios-nx-os-software/ios-software-releases-123-mainline/46509-factory-default.html
* https://blog.naver.com/triride/220422393712
* https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=triride&logNo=220424906254
* https://jeongzzang.com/39
