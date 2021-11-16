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
4. AUX
5. 네트워크 관리 시스템(Network Management System)

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

---

[참고 사이트]   
* https://toastfactory.tistory.com/281
* https://www.cisco.com/c/ko_kr/support/docs/ios-nx-os-software/ios-software-releases-123-mainline/46509-factory-default.html
* https://blog.naver.com/triride/220422393712
* https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=triride&logNo=220424906254
* https://jeongzzang.com/39
