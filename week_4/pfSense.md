# pfsense

pfsense는 개인, 기업에서 무료로 사용할 수 있는 오픈소스 라우터/방화벽입니다. 
정확히는 라우터, 스위치, 무선라우터/스위치, 방화벽으로 설정하여 사용할 수 있고, 대부분의 경우에 내·외부 경계망에서 방화벽으로 사용하고 있습니다.

# pfSense 설치

아래의 링크에서 pfsense 이미지 파일을 다운로드 받습니다.   
=> https://www.pfsense.org/download/   
![image](https://user-images.githubusercontent.com/43658658/142332690-b4ecd1c3-f3fa-46fd-881a-020666ea1e93.png)

pfsense를 위한 가상 머신을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/142333129-3f2d714e-0470-4826-8e15-2a8d9eb03bc2.png)   

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
![image](https://user-images.githubusercontent.com/43658658/142339154-3c97c19f-d9f3-4aa6-9ca2-5bee401b0167.png)   
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
모든 영역의 데이터를 백업하도록 합니다. 암호화를 체크한 후 암호화를 위한 패스워드를 입력합니다.   
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

---

[참고 사이트]   
* https://techexpert.tips/ko/pfsense-ko/pfsense-%EC%84%9C%EB%B2%84-%EC%84%A4%EC%B9%98/
* https://edushare.tistory.com/45
* https://techexpert.tips/ko/pfsense-ko/pfsense-%ea%b3%b5%ec%9e%a5-%ea%b8%b0%eb%b3%b8-%ea%b5%ac%ec%84%b1%ec%9c%bc%eb%a1%9c-%ec%9e%ac%ec%84%a4%ec%a0%95/
* https://techexpert.tips/ko/pfsense-ko/pfsense-%eb%b0%b1%ec%97%85-%eb%b0%8f-%eb%b3%b5%ec%9b%90/





