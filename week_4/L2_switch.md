# L2 스위치

## 스위치란?

허브(L1)의 단점을 개선한 것으로 패킷의 MAC 주소를 읽어 유니캐스트 방식으로 데이터를 특정 포트로만 전송합니다.   
패킷을 보내는 노드와 받는 노드를 1:1로 연결시켜 주기 때문에 충돌이 발생하지 않아 빠른 속도로 전송이 가능합니다.   

* 허브 : 기기에 꽂힌 모든 포트에 동일한 정보를 보내는 장비입니다. 브로드캐스트 방식을 취하며, 연결된 기기만큼 성능이 n분의 1로 저하되고, 패킷의 충돌이 날 가능성이 있습니다.

## 스위치 콘솔 접속

1. USB 연결 준비

pc와 스위치를 콘솔 포트로 연결(USB-LAN) 한 뒤, 장치관리자에서 포트 정보를 확인합니다.   
![KakaoTalk_20211115_121705354_01](https://user-images.githubusercontent.com/43658658/141717030-ceb7f530-5727-4602-8a37-c74a14be0718.jpg)   
![image](https://user-images.githubusercontent.com/43658658/141717168-142f1d6d-c848-49f3-8ff4-0b157c0a66af.png)   
![image](https://user-images.githubusercontent.com/43658658/141716082-6f3e1b68-7823-4ae8-8148-406fb70ebd6d.png)   
* 처음에 `USB2.0-ser!` 드라이버가 없어서 포트가 뜨지 않았는데, 구글링을 통해 드라이버를 설치하니 포트가 정상적으로 나타났습니다.

2. Putty 프로그램으로 콘솔에 연결

![image](https://user-images.githubusercontent.com/43658658/141716211-30f0fd60-7d36-46c0-9162-6629a0d0889f.png)

3. 콘솔 접근

![image](https://user-images.githubusercontent.com/43658658/141716341-a57e36ea-0086-4da3-8154-1f86fec8902f.png)   
콘솔 창이 뜨면 Enter 키를 입력합니다.   

## 스위치 초기화

> <h3>관리자 패스워드를 모를 때</h3>

