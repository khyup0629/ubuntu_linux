# pfsense

pfsense는 개인, 기업에서 무료로 사용할 수 있는 오픈소스 라우터/방화벽입니다. 
정확히는 라우터, 스위치, 무선라우터/스위치, 방화벽으로 설정하여 사용할 수 있고, 대부분의 경우에 내·외부 경계망에서 방화벽으로 사용하고 있습니다.

# pfsense 설치

아래의 링크에서 pfsense 이미지 파일을 다운로드 받습니다.   
=> https://www.pfsense.org/download/   
![image](https://user-images.githubusercontent.com/43658658/142332690-b4ecd1c3-f3fa-46fd-881a-020666ea1e93.png)

pfsense를 위한 가상 머신을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/142333129-3f2d714e-0470-4826-8e15-2a8d9eb03bc2.png)   
* `freeBSD` : 버클리 대학에서 개발된 오픈소스 OS. 리눅스와 달리 전통적인 유닉스 방식에 따라 온전한 OS 개발을 목표로 개발되었습니다.

pfsense `iso` 파일을 넣고 가상 머신을 실행하면 자동으로 설치가 시작됩니다.   
![image](https://user-images.githubusercontent.com/43658658/142333896-f7d78122-6400-4ea9-b827-c7447e9745a6.png)   
pfsense 최종 사용자 라이선스 계약을 수락합니다.   
![image](https://user-images.githubusercontent.com/43658658/142334010-8585b35a-dd76-4358-ba62-8438733cb074.png)   
설치 옵션을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/142334054-20dbac44-e47b-4b44-8d23-541875e85ccb.png)   
키보드 레이아웃을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/142334204-a655f2b9-5377-4dd9-88e5-6bde400dc348.png)   
![image](https://user-images.githubusercontent.com/43658658/142334384-a0163f21-3bf3-4ca1-8c4d-0a31000440d1.png)   
자동 옵션을 선택해서 디스크 분할을 자동으로 수행합니다.   
![image](https://user-images.githubusercontent.com/43658658/142334954-db2f51b0-2bf6-45b9-ab43-fce2da95f237.png)   
pfsense 서버 설치를 시작합니다.   
![image](https://user-images.githubusercontent.com/43658658/142334996-6fbca3a4-f3b4-495a-9b0b-273438763802.png)   
수동 화면 구성을 하지 않습니다.   
![image](https://user-images.githubusercontent.com/43658658/142335098-51023807-2bee-471a-b1f8-3b03fb8ddee5.png)   
재부팅합니다.   
![image](https://user-images.githubusercontent.com/43658658/142335125-2a110f9b-d740-4f19-8c33-70ea9c7a0f19.png)   

내부에서 네트워크를 나눌 필요는 없으므로 vlan을 구성하지 않고, 시스템의 외부 인터페이스를 `vmx0`로 구성합니다.   
![image](https://user-images.githubusercontent.com/43658658/142335738-65abb620-7461-4ee5-a1ed-653ec7b05d3a.png)   
시스템의 내부 인터페이스를 `vmx1`로 구성합니다.   
![image](https://user-images.githubusercontent.com/43658658/142335770-f8bc7c1d-2fe6-4f69-ba90-609a66174e1a.png)   
지정한 인터페이스를 확인하고 진행합니다.   
![image](https://user-images.githubusercontent.com/43658658/142335829-cc4f7550-66d6-40d9-95d2-7cc4acd7293d.png)   

pfsense 서버를 성공적으로 설치했습니다.   
![image](https://user-images.githubusercontent.com/43658658/142337620-7ca0d0f7-5edb-4b56-a9bc-96b49edffd59.png)   
설치 이후의 설정은 할당 받은 IP로 웹 브라우저로 접속해 진행합니다.

## pfsense 서버 웹 접근 후 기본 설정

할당 받은 IP로 웹으로 접근하기 위해서 8번 옵션을 선택해서 `pfctl -d`를 명령합니다.   
![image](https://user-images.githubusercontent.com/43658658/142338604-663c34ee-168a-46f1-8818-569bb9447dbb.png)   
* `pfctl -d` : pfsense의 방화벽을 비활성화 한다는 의미입니다.

웹 브라우저를 열고 할당 받은 IP로 pfsense 서버에 접속합니다.   
![image](https://user-images.githubusercontent.com/43658658/142338807-b08c086e-e019-4ca4-aef8-eb79c5000ef0.png)

pfsense 기본 암호 로그인 정보를 입력합니다(admin/pfsense).   
![image](https://user-images.githubusercontent.com/43658658/142338915-e7f42a84-c282-4132-bbe8-8e52d20ec518.png)

pfsense 셋업 마법사가 표시됩니다.   
![image](https://user-images.githubusercontent.com/43658658/142338963-dacdd3d0-5a8b-4e4e-8a4c-35e0775f1886.png)   
호스트이름, 도메인, DNS 구성을 수행합니다.   
(DNS 서버 8.8.8.8 은 구글 DNS 서버를 의미)   
![image](https://user-images.githubusercontent.com/43658658/142358110-3d792d01-9f4b-4eb0-88f0-cd61e57283ab.png)   
표준 시간대를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/142339598-d7c32dbd-caf6-4d39-b433-3679aced9fdd.png)   
내부 인터페이스의 ip와 서브넷마스크를 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/142340434-0a040b44-2d07-4967-ad11-2e91fdababc6.png)   
원하는 비밀번호로 바꿔줍니다.   
![image](https://user-images.githubusercontent.com/43658658/142340569-ae839c58-0cf4-4682-934c-880a7bf7f03c.png)   
pfsense를 재부팅합니다.   
![image](https://user-images.githubusercontent.com/43658658/142340632-b4c9db56-260d-452e-a936-9cdeba641762.png)   
모든 기본 설정이 모두 완료되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/142340723-6818f2fd-dfe5-4446-a66a-57c5aac73eb8.png)

pfsense GUI 환경의 대시보드가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/142341908-71ac5f87-60ca-40c0-b99f-b4a1e1e4f831.png)

## pfsense 공장 초기화

상단 메뉴에 `Factory Defaults` 버튼을 누릅니다.   
![image](https://user-images.githubusercontent.com/43658658/142343711-cd8deacd-2421-4d9c-b298-d882995d866e.png)   

`Factory Reset` 버튼을 누르면 pfsense가 재부팅되며 공장 초기화됩니다.   
![image](https://user-images.githubusercontent.com/43658658/142343864-f180ccf7-e321-443a-908b-d2028c7ba974.png)   

## pfsense 백업 및 복원

> <h3>pfsense 백업</h3>

상단의 [백업 및 복원] 메뉴로 들어갑니다.   
![image](https://user-images.githubusercontent.com/43658658/142342265-4d2b96de-298e-4e97-8d57-4db3fd4e8d1b.png)   
모든 영역의 데이터를 백업하도록 합니다. 백업 파일 암호화를 체크한 후 패스워드를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/142342587-5b0ce482-344a-44da-be41-dfc1f21fd859.png)   
XML 파일로 백업 파일을 다운로드 받습니다.

> <h3>pfsense 복원</h3>

상단의 [백업 및 복원] 메뉴로 들어가서 페이지의 아래쪽에 복원 항목을 봅니다.   
![image](https://user-images.githubusercontent.com/43658658/142342843-b389a0f9-e2f1-44d6-bbf6-bb91df33cc78.png)   
모든 영역의 데이터를 복원하도록 합니다. 백업 파일(XML)을 선택하고, 암호화가 되어 있으므로 체크한 뒤, 백업 파일을 생성할 때 설정한 패스워드를 입력합니다.   

`[설정 복원]` 버튼을 누르면 백업 파일을 복원한 뒤 pfsense가 재부팅됩니다.

## 언어 변경 - 한글

상단의 `[General Setup]` 메뉴로 들어갑니다.   
![image](https://user-images.githubusercontent.com/43658658/142344467-6332b2db-beb1-4828-8d72-005ce0e06b3d.png)   

로컬라이제이션 항목에서 언어를 한국어로 변경합니다.   
![image](https://user-images.githubusercontent.com/43658658/142344515-8bc7c025-57c6-4a6a-b7f5-9a1aedecb223.png)   

페이지 하단에 `[Save]`를 누르고, 가상 머신에서 pfsense를 재부팅하면 언어가 변경됩니다.   
![image](https://user-images.githubusercontent.com/43658658/142344923-042351f5-b120-4152-8547-bda3baf16608.png)   

## IP 충돌로 인한 WAN IP 변경

DHCP로 WAN IP를 동적으로 할당 받으니 IP 충돌이 일어나서 호스트 `141`로 변경하였습니다.   
![image](https://user-images.githubusercontent.com/43658658/142353975-3696d4a3-4185-48d3-83bc-20475db754f1.png)   
![image](https://user-images.githubusercontent.com/43658658/142354083-70b3c5b2-172a-4909-a826-ed484ab64f20.png)   

## openVPN 구축

> <h3>VPN 인증 설정</h3>

openVPN을 사용하기 위해서는 인증서가 필요합니다.   
먼저 인증 기관을 생성합니다.

[시스템] > [인증서 관리자]   
![image](https://user-images.githubusercontent.com/43658658/142351509-9c2430fd-c6e5-4a18-a975-532262e29b44.png)   

[CAs] > [추가] 버튼을 눌러줍니다.   
인증 기관 이름과 인증 기관 생성 방법을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/142354556-927869ea-8911-4f64-bc03-8dd7e25ff4bb.png)   
* `내부 인증 기관 생성` : pfsense 자체에서 관리하는 인증 기관 생성

나머지 내부 인증 기관 항목은 인증 기관에 대한 설정을 해주는 것인데, 기본값으로 설정되어 있는 것을 그대로 적용합니다.   
![image](https://user-images.githubusercontent.com/43658658/142355171-45912af5-f642-4bc9-a16d-f75517a98aa8.png)

인증 기관이 생성된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/142355304-083b664a-63ea-4555-b00e-41f6002701dc.png)

다음으로 인증서를 생성합니다.   

[인증서] > [추가] 버튼을 눌러줍니다.   
![image](https://user-images.githubusercontent.com/43658658/142356173-5cb22873-3978-4295-840e-04a2eb928932.png)   
* 인증서 역시 pfsense에서 사용될 것이므로 내부 인증서로 선택합니다.
* 인증 기관은 아까 생성했던 인증 기관을 선택합니다.
* 공통 이름에는 인증서의 도메인을 적습니다.
* 인증서 유형은 서버를 위한 인증서이므로 `Server Certificate`를 선택합니다.

인증서가 생성되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/142356371-91118097-464c-48a8-b74d-0640ef68e9cc.png)

> <h3>VPN 유저 생성</h3>

VPN을 사용할 사용자를 생성해보겠습니다.

[시스템] > [사용자 관리자]   
![image](https://user-images.githubusercontent.com/43658658/142356486-f050828a-bf9b-42cb-8a15-8a037da91020.png)   

[사용자] > [추가] 버튼을 눌러줍니다.   
사용자를 생성하는 부분인데, 앞서 인증 설정 단계를 거쳤다면 대부분의 항목이 이미 채워져 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/142356823-daa0dd89-15db-4123-9c48-27b84df7d42b.png)   
* 인증서 항목을 체크하여 사용자 인증서를 만들어줍니다. 인증 기관은 이전에 생성한 인증 기관을 선택합니다.

사용자가 생성되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/142356921-81373bcc-37e7-463b-a32f-44107512019f.png)   

> <h3>VPN 클라이언트 패키지 설치</h3>

openVPN에서 사용할 클라이언트 패키지를 설치합니다.

[시스템] > [패키지 관리자]   
![image](https://user-images.githubusercontent.com/43658658/142357280-5beab16c-e6fb-446e-b3b6-e6ef14c611ef.png)   

[사용 가능한 패키지] > 검색어 : openvpn   
![image](https://user-images.githubusercontent.com/43658658/142357380-f56ac6c2-5b0a-45d9-94df-2955556ba962.png)

[Install]을 눌러 설치를 진행합니다.   
![image](https://user-images.githubusercontent.com/43658658/142357565-fa724d9c-907b-4f26-b9bc-52aad05bd6bb.png)

설치가 완료되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/142362527-a9cf6da9-4576-4610-99b7-ca113ab362e1.png)

> <h3>VPN 설정</h3>

VPN 설정을 위해서 [VPN] > [openVPN]으로 들어갑니다.   
![image](https://user-images.githubusercontent.com/43658658/142362613-6767c93c-d70d-4038-863d-bf6b7c008c8d.png)

클라이언트 패키지를 설치하였기 때문에 [마법사] 이후의 탭 항목 [Client Export], [Shared key Export]이 생성되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/142362816-4c629ee0-e768-4e59-8dbf-ead12097187e.png)

openVPN을 설정하는데, 단계적으로 시행하기 위해 [마법사]를 통해서 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/142362993-2b139906-5541-44b3-b86a-14d3e7f01303.png)   

서버 타입은 인증을 어떻게 할 것인지를 선택하는 것인데, pfsense에서 자체적으로 인증할 것이기 때문에 `Local User Access`로 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/142363221-64eb6199-ea31-4e11-8ab4-3296d0d08a1d.png)   
![image](https://user-images.githubusercontent.com/43658658/142363248-175eab28-d934-4893-86a2-d2ced25f3c40.png)      

VPN은 외부에서 접속이 들어오기 때문에 `WAN`을 사용합니다.   
![image](https://user-images.githubusercontent.com/43658658/142366111-09eab81e-075a-47cd-ae34-c4f49aaf44a4.png)   

`터널 네트워크` : openVPN은 클라이언트가 서버에 접속할 때 `터널`을 생성하고 `터널`을 통해서 서버에 접속합니다. 이 터널의 네트워크 ID를 할당해줍니다.   
`Local Network` : 원격에서 접속할 수 있는 로컬 네트워크 ID를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/142366001-566db883-6d95-4611-8b69-76b593325be7.png)

방화벽, openVPN 규칙을 자동으로 생성하도록 체크합니다.   
![image](https://user-images.githubusercontent.com/43658658/142365050-c43320dd-922f-453c-8563-95f39ba5d2d2.png)

openVPN 서버가 생성되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/142365206-8b1ce9fd-3a73-4b6d-b311-d0eae4417317.png)

> <h3>클라이언트에게 배포</h3>

openVPN 탭에서 Client Export 탭을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/142366394-29c37804-88af-4675-ab2c-b18f8b3711ae.png)

페이지의 아래쪽에 openVPN Client 소프트웨어를 자신의 운영체제에 맞게 다운로드 할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/142366345-785faa64-cf38-4963-845e-60dffb8c1185.png)

다운로드한 파일을 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/142367441-1e94c60e-4371-457a-a5f9-cba4a7968bef.png)   

> <h3>openVPN GUI 실행</h3>

이전에 생성한 사용자 정보를 통해 로그인합니다.   
![image](https://user-images.githubusercontent.com/43658658/142367507-6f981ad1-1330-450c-9136-64a206e80f0c.png)

openVPN을 설정할 때 생성했던 터널 IP가 할당되고, 연결이 되는 것을 볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/142367541-2f60073a-e8d1-43b5-b589-d36afa649b09.png)

`ipconfig`로 연결 정보를 살펴보면, openVPN에 대해 `192.168.2.2`를 IP 주소로 가진 것을 볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/142368418-6cdec131-9386-4a82-8228-d647c277b32c.png)

> <h3>VPN 상태 확인</h3>

![image](https://user-images.githubusercontent.com/43658658/142368748-756a94ca-e8bd-4c96-a5e3-e4ad9ea06de4.png)   
상태가 초록색 체크 표시가 떠 있으면, openVPN이 활성화 되어 있는 것을 의미합니다.   
실제 IP인 `172.16.0.85`의 `1194`번 포트로 접속하면 터널 IP인 `192.168.2.2`를 통해 서버에 접속됩니다.

로그를 보기 위해 [상태] > [시스템 로그]로 들어갑니다.   
![image](https://user-images.githubusercontent.com/43658658/142369327-e057c7a0-5e4f-43bb-9a16-5966e9f3afe9.png)

이곳에서 openVPN에 대한 로그를 볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/142369400-b306fae9-6016-4b14-a48e-e1f4f1a4c27b.png)


---

[참고 사이트]   
* [설치 방법 가이드 메뉴얼](https://techexpert.tips/ko/pfsense-ko/pfsense-%EC%84%9C%EB%B2%84-%EC%84%A4%EC%B9%98/)
* [설치 방법 영상](https://edushare.tistory.com/45)
* [공장 초기화](https://techexpert.tips/ko/pfsense-ko/pfsense-%ea%b3%b5%ec%9e%a5-%ea%b8%b0%eb%b3%b8-%ea%b5%ac%ec%84%b1%ec%9c%bc%eb%a1%9c-%ec%9e%ac%ec%84%a4%ec%a0%95/)
* [백업 및 복원](https://techexpert.tips/ko/pfsense-ko/pfsense-%eb%b0%b1%ec%97%85-%eb%b0%8f-%eb%b3%b5%ec%9b%90/)
* [언어 한글화](https://techexpert.tips/ko/pfsense-ko/pfsense-%ec%9b%b9-%ec%9d%b8%ed%84%b0%ed%8e%98%ec%9d%b4%ec%8a%a4-%ec%96%b8%ec%96%b4-%eb%b3%80%ea%b2%bd/)
* [openVPN 구축 영상](https://www.youtube.com/watch?v=NABC4XeRBWQ&list=PL_1Pc_OMhiCg16-QSMXUoxDZJRUo2lRHM&index=2)





