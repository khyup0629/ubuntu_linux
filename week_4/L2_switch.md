# L2 스위치

## 스위치란?

허브(L1)의 단점을 개선한 것으로 패킷의 MAC 주소를 읽어 유니캐스트 방식으로 데이터를 특정 포트로만 전송합니다.   
패킷을 보내는 노드와 받는 노드를 1:1로 연결시켜 주기 때문에 충돌이 발생하지 않아 빠른 속도로 전송이 가능합니다.   

* 허브 : 기기에 꽂힌 모든 포트에 동일한 정보를 보내는 장비입니다. 브로드캐스트 방식을 취하며, 연결된 기기만큼 성능이 n분의 1로 저하되고, 패킷의 충돌이 날 가능성이 있습니다.

## 스위치 콘솔 접속

1. USB 연결 준비

pc와 스위치를 콘솔 포트로 연결(USB-LAN) 한 뒤, 장치관리자에서 포트 정보를 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/141717168-142f1d6d-c848-49f3-8ff4-0b157c0a66af.png)   
![image](https://user-images.githubusercontent.com/43658658/141716082-6f3e1b68-7823-4ae8-8148-406fb70ebd6d.png)   
* 처음에 `USB2.0-ser!` 드라이버가 없어서 포트가 뜨지 않았는데, 구글링을 통해 드라이버를 설치하니 포트가 정상적으로 나타났습니다.

2. Putty 프로그램으로 콘솔에 연결

![image](https://user-images.githubusercontent.com/43658658/141716211-30f0fd60-7d36-46c0-9162-6629a0d0889f.png)

3. 콘솔 접근

![image](https://user-images.githubusercontent.com/43658658/141716341-a57e36ea-0086-4da3-8154-1f86fec8902f.png)   
콘솔 창이 뜨면 Enter 키를 입력합니다.   

## 스위치의 4가지 모드

> <h3>User Mode</h3>

유저모드는 `>`표시가 나오는 상태로 스위치 상태 보기만 가능하고 주로 `Ping Test`를 하기 위해 사용됩니다.

> <h3>Privileged Mode</h3>

프리빌리지 모드는 관리자 모드라고도 불리며 주로 장비의 구성 및 상태를 확인 할 때 사용됩니다.   
`conf t(erminal)`을 이용해 `Global Mode`로 들어갈 수 있습니다.

> <h3>Global Mode</h3>

`(config)` 표시가 붙으며, 이 부분에서 호스트 설정 및 IP 설정 등 대부분의 작업이 처리됩니다.   
`int(erface)`를 이용해 `interface mode`로 들어갈 수 있습니다.   

> <h3>Interface Mode</h3>

`(config-if)`를 표시하며 해당 인터페이스의 상세 설정을 위해 사용됩니다.   
해당 인터페이스의 IP나 Subnet 값을 설정 할 수 있습니다.

* `exit`를 이용하면 모드를 한 단계 빠져나가고, `[Ctrl+Z]`를 누르면 바로 프리빌리지 모드로 들어갑니다.

## 스위치 초기화

> <h3>공장 초기화 진행</h3>

관리자 패스워드를 모를 때에는 공장 초기화를 진행해야 합니다.   
스위치 장비 왼쪽 하단의 `Mode` 버튼을 약 15초 정도 눌러 스위치를 초기화합니다.

![image](https://user-images.githubusercontent.com/43658658/141717261-ad510cb9-2f94-49dc-b61a-817ac9f1b002.png)   
모든 램프가 꺼지고, `SYST` 램프가 깜빡거려야 합니다.   

아래의 화면이 뜨면 `[Enter]`를 누릅니다.   
![image](https://user-images.githubusercontent.com/43658658/141724428-9e1f832e-b3fb-4c38-aad8-7db6254d5456.png)   
 
![image](https://user-images.githubusercontent.com/43658658/141724657-1e698605-c60d-401f-a839-0009cfb74fc8.png)   
`yes` : Setup 모드로 들어가서 설정에 대한 질문을 단계적으로 진행합니다.   
`no` : Setup 모드로 들어가지 않고 필요한 부분만 알아서 설정합니다.   

## 스위치 텔넷 설정

텔넷 : 원격에서 서버에 접속해 작업할 수 있도록 해주는 프로토콜.

1. VLAN 설정

텔넷으로 원격 접속이 가능하려면 서로 간에 인터넷 통신이 연결되어 있어야 합니다.   

먼저 스위치에 VLAN에 대한 IP 정보를 입력해줍니다.   
* `VLAN(Virtual Local Area Network)` : 물리적 배치와 상관없이 가상에서 논리적으로 LAN을 구성할 수 있는 기술입니다.

![image](https://user-images.githubusercontent.com/43658658/141728997-3a13c7d2-a0ec-4efe-8acf-241f4b048a9f.png)   
![image](https://user-images.githubusercontent.com/43658658/141729025-2bb869db-1ad0-4ac6-902a-f3f3af3f672d.png)   
vlan에 IP와 서브넷을 먼저 설정해줍니다.   
no sh(no shutdown)을 통해 해당 인터페이스를 끄지 않고 유지하도록 합니다.

![image](https://user-images.githubusercontent.com/43658658/141729218-8d9dd25a-12ee-4474-9d4e-610248dfa5f0.png)   
`do` : 현재 모드 상태에서 임시로 `Privileged Mode` 명령어 사용이 가능합니다.   
`sh ip int bri(show ip interface brief)` : 인터페이스의 `up`, `down` 상태(`status`) 및 `ip` 주소 정보를 간략하게 확인.

2. 비밀번호 설정

텔넷(vty)는 비밀번호가 설정되어 있어야 접속이 가능합니다.   

![image](https://user-images.githubusercontent.com/43658658/141732346-b3de8603-bdf5-4c61-83f5-68d3c6ecdcd8.png)   
`line vty 0 4` : 텔넷 회선을 0부터 4까지 5명이 동시 접속 가능하도록 한다는 의미입니다.   
`password [패스워드]` : 패스워드를 설정합니다.   
`login` : 텔넷 접속 시 패스워드를 물어봅니다(`nologin`으로 하면 패스워드를 물어보지 않습니다).

3. 윈도우 텔넷 환경 설정

`[Windows 기능 켜기/끄기]` > 텔넷 클라이언트 > 체크   
![image](https://user-images.githubusercontent.com/43658658/141730348-71780ca8-3b72-442a-8301-5a719cc9c4eb.png)

4. 원격에서 텔넷 접속

스위치를 인터넷 또는 로컬로 연결합니다.   

정상적으로 접속되는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/141740688-6b33302f-58d7-481c-8032-b70d539f355c.png)




