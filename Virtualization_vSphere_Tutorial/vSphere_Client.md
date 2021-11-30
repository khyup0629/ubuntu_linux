# vSphere Client

## vCenter에 ESXi 호스트 등록하기

vCenter에 ESXi 호스트를 등록합니다.   

![image](https://user-images.githubusercontent.com/43658658/143967825-9d7a778a-c1cc-4c1a-b168-d5a3c8fc2571.png)   
![image](https://user-images.githubusercontent.com/43658658/143968050-e4f43a43-6c75-4cdf-8fc0-4251c92b365e.png)   
![image](https://user-images.githubusercontent.com/43658658/143968085-b4b24f03-45d7-4374-9964-0736a0c78fdb.png)   

ESXi 호스트 정보들을 입력합니다.
![image](https://user-images.githubusercontent.com/43658658/143968125-8e1999ca-bc5e-48d6-907e-572fa97fa21e.png)   
![image](https://user-images.githubusercontent.com/43658658/143968217-03d8b0a4-8df8-430f-bce7-380873e81be5.png)
라이센스 확인 페이지입니다. 여기서 라이센스가 있다면 선택하면 됩니다.   
![image](https://user-images.githubusercontent.com/43658658/143968389-ea90405c-485d-48f4-9982-2826cde0a4fd.png)   
ESXi에 접속이 가능하도록 하기 위해 잠금 모드는 사용하지 않겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/143968529-11024ccb-1fff-42bd-ac4d-e6e59469b9ab.png)   
* `일반` : 로컬 콘솔이나 vCenter Server를 통해서만 ESXi 접속이 가능.
* `엄격` : vCenter Server를 통해서만 ESXi 접속 가능.

VM 위치를 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/143968704-099fc970-abcd-4c7f-9380-32a3c72f24d9.png)   
전체 설정을 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/143968740-1a845171-08ef-4035-989f-5abf48ab961f.png)   

조금만 기다리면 ESXi 호스트가 추가된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/143968820-e36896b0-3490-40bd-bff6-a59bd7b5f062.png)

같은 방법으로 나머지 호스트도 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/143969301-74db4716-6b9b-4318-b700-7a537cabebcc.png)

## iSCSI 공유 스토리지

RJ45를 통한 네트워크 환경에서 서버와 스토리지를 연결하기 위해 iSCSI 프로토콜을 이용합니다.   
(SAN 방식 FC 프로토콜을 통해 SCSI 명령어를 전달하는데, 네트워크 환경에서는 iSCSI 프로토콜을 이용해 SCSI 명령어를 전달합니다).
iSCSI 공유 스토리지를 구축하고, ESXi 호스트들을 공유 스토리지와 연결합니다.   

클러스터를 구성할 때 공유 스토리지가 없다면 클러스터 구성을 할 수 없습니다.



## 클러스터 구성하기

`클러스터` : 복수의 ESXi 호스트를 논리적으로 그룹화한 것입니다. vCenter는 동일한 클러스터에 속한 ESXi들의 리소스를 클러스터 단위의 리소스로 관리합니다.

![image](https://user-images.githubusercontent.com/43658658/143991875-e2e48f04-dd6a-4ecd-adf0-0d8b81f51669.png)   
![image](https://user-images.githubusercontent.com/43658658/143991994-bcfeacaa-1310-476e-a656-7b731bd85771.png)   
ESXi 호스트 2개를 드래그 앤 드롭하여 클러스터 안으로 넣습니다.   
![image](https://user-images.githubusercontent.com/43658658/143992141-23096bd5-e1ee-421e-834f-9e9b372e584a.png)

## 데이터스토어 생성하기

`데이터스토어` : 데이터베이스 뿐만이 아니라 모든 데이터를 저장하고 관리하기 위한 저장소

클러스터 위에서 우클릭해서 데이터스토어를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/143993998-e1af44c9-43ad-456d-b76e-385a6e3b544a.png)

![image](https://user-images.githubusercontent.com/43658658/143994027-54f93cdd-f75c-4727-962a-9293ee717368.png)   
* `VMFS` : 여러 물리적 호스트가 동일 스토리지에 대하여 동시에 읽기 및 쓰기 작업을 수행할 수 있는 클러스터 파일 시스템.

