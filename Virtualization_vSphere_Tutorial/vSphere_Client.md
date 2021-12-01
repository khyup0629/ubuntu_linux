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

RJ45를 통한 네트워크 환경에서 서버와 스토리지를 연결하기 위해 `iSCSI 프로토콜`을 이용합니다.   
(SAN 방식 FC 프로토콜을 통해 SCSI 명령어를 전달하는데, NAS 환경에서는 iSCSI 프로토콜을 이용해 SCSI 명령어를 전달합니다).
iSCSI 공유 스토리지를 구축하고, ESXi 호스트들을 공유 스토리지와 연결합니다.   

클러스터를 구성할 때 `공유 스토리지가 없으면` 클러스터가 제대로 구성되지 않습니다.

> <h3>Window Server 2016 설치</h3>

가상머신을 하나 생성하고 윈도우 서버 OS를 설치합니다.   

iso 파일로 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/144007731-b1a5c49c-26dd-4933-95f5-5b11ff44b222.png)   
![image](https://user-images.githubusercontent.com/43658658/144007904-c6087e6c-8040-4da2-aa3f-1aea7da6d009.png)   
GUI로 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/144008046-e66e7263-529b-42a5-97d1-cc0179b514b4.png)   
윈도우만 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/144008169-7963ebfa-e8f6-40c2-addf-cfc17e1a75ce.png)   
설치가 완료되면 비밀번호를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/144009137-5367eb59-b36d-4fec-a9e9-6c85c5cd3adb.png)
`Ctrl+Alt+Del`을 누르라고 나와있는데, 실제로 키보드로 누르면 눌러지지 않습니다.   
![image](https://user-images.githubusercontent.com/43658658/144009207-b3251da2-3ad0-4bb1-bcb2-a4954a0d3c59.png)   
해결 방법은 아래와 같습니다.   
![image](https://user-images.githubusercontent.com/43658658/144009305-467c441c-3dbb-4d18-a7a6-65a2876045f1.png)   

> <h3>Window Server 한글화</h3>

[Settings] > [Time & Language] > [Region & Language]에서 `한국어`를 선택하고 `Options`를 선택합니다.   
(만약 한국어가 없다면 `Add a language`를 클릭해서 한국어를 추가합니다)   
![image](https://user-images.githubusercontent.com/43658658/144009745-e7156b8e-e116-4b90-870f-b58cab4607b9.png)

언어 팩을 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/144009905-b553d5e5-98cf-40d9-9207-adfd48f99214.png)

설치를 완료하고 재부팅을 하면 언어가 한글로 바뀌어 있음을 알 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144010107-54ae8eab-6ace-4b05-b7bc-538d1effe74b.png)

> <h3>iSCSI 공유 스토리지 구성</h3>

이 윈도우 서버에 `iSCSI 공유 스토리지`를 구성할 것입니다.   

먼저 [서버 관리자]로 접속합니다.   
iSCSI를 이용하기 위해 필요한 것들을 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/144021568-202c969f-6479-4b55-b7c9-b61459ed3c23.png)
* iSCSI에서 연결하려는 주체는 초기자(initiator), 접속 디바이스 대상을 타겟(Target)이라고 부릅니다.
* VMware 환경에서는 연결의 주체(`초기자`)가 ESXi 호스트, 연결 대상 디바이스(`타겟`)는 스토리지입니다.

[파일 및 저장소 서비스]   
![image](https://user-images.githubusercontent.com/43658658/144010949-f41eb87f-b5a0-4525-8d5c-95d9e16151d9.png)

iSCSI 탭에서 iSCSI 가상 스토리지를 만듭니다.   
![image](https://user-images.githubusercontent.com/43658658/144022620-a44f7177-6ffd-4514-a933-837997dca720.png)   
![image](https://user-images.githubusercontent.com/43658658/144022747-66c7a027-5195-4509-9530-16f8c6019a67.png)   
![image](https://user-images.githubusercontent.com/43658658/144023516-da018aae-3ff3-4549-ac1e-5823995cc8e7.png)   
![image](https://user-images.githubusercontent.com/43658658/144023185-0f5fa422-e066-40d2-a94b-2a5b9b334404.png)   
![image](https://user-images.githubusercontent.com/43658658/144023564-c90fb10e-fda4-4f6b-894e-5f083e1affae.png)   
![image](https://user-images.githubusercontent.com/43658658/144042593-c9574d46-f0f3-4d8b-802e-03da8194951f.png)   
이 항목에서 초기자를 선택해야 합니다. 초기자는 ESXi 호스트들을 가리킵니다.   
![image](https://user-images.githubusercontent.com/43658658/144043101-35cbc1c8-6200-498c-b8b7-b2d0499cf095.png)   
캐시에 ESXi 호스트가 뜨게 하려면 `vSphere Client`의 ESXi 호스트에서 iSCSI 유형의 스토리지 어댑터를 추가해 스토리지와 연결시켜주어야 합니다.   
![image](https://user-images.githubusercontent.com/43658658/144043644-2f0eb5b0-56ad-4306-bd91-150b6852c24b.png)   
![image](https://user-images.githubusercontent.com/43658658/144043682-c64241ab-3208-49fd-b6c3-d71ff5317cec.png)   
![image](https://user-images.githubusercontent.com/43658658/144043852-6869526f-2262-44b5-b179-25c6c55a898f.png)   
![image](https://user-images.githubusercontent.com/43658658/144043891-2b82ed8f-21af-4ed6-9e6f-9d05b00c3ee5.png)   
같은 방법으로 다른 ESXi 호스트도 설정해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144047276-600d4782-2b0f-4106-92c4-14eb6b4f4394.png)   
그리고 다시 스토리지로 돌아와보면, 캐시에 식별자가 추가된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144047321-e17dbc68-f34e-4142-b207-653a1b04ab02.png)   
전체 설정을 확인하고 iSCSI 가상 스토리지를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/144047464-b0044297-ee4d-48b5-bb67-8b508fd155dc.png)   

> <h3>ESXi와 공유 스토리지 연결</h3>

스토리지를 생성했다고 자동으로 연결되지 않습니다.   
다시 `vSphere Client`로 돌아가서 `스토리지 다시 검색`을 클릭해 새로 추가된 스토리지 볼륨이 있는지 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/144046049-c732644f-5523-4864-9a38-0b94bc4e2c34.png)   
두 ESXi 호스트 모두 스토리지가 연결되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/144047689-a2ba1a8c-0ae7-4fce-9384-61bcc690512c.png)   
![image](https://user-images.githubusercontent.com/43658658/144047748-79c2c735-e408-4ce0-8068-2240d9d752ee.png)   

## 클러스터 구성하기

`클러스터` : 복수의 ESXi 호스트를 논리적으로 그룹화한 것입니다. vCenter는 동일한 클러스터에 속한 ESXi들의 리소스를 클러스터 단위의 리소스로 관리합니다.

![image](https://user-images.githubusercontent.com/43658658/143991875-e2e48f04-dd6a-4ecd-adf0-0d8b81f51669.png)   
![image](https://user-images.githubusercontent.com/43658658/143991994-bcfeacaa-1310-476e-a656-7b731bd85771.png)   
ESXi 호스트 2개를 드래그 앤 드롭하여 클러스터 안으로 넣습니다.   
![image](https://user-images.githubusercontent.com/43658658/143992141-23096bd5-e1ee-421e-834f-9e9b372e584a.png)

## 데이터스토어

`데이터스토어` : 데이터베이스 뿐만이 아니라 모든 데이터를 저장하고 관리하기 위한 저장소

우리는 앞서 윈도우 서버를 iSCSI 공유 스토리지로 생성했고, ESXi 호스트들과 연결지었습니다.   
이제 VMware에서 사용할 수 있도록 `데이터스토어(가상 파일시스템)로 변환`시켜 사용 하겠습니다.

> <h3>데이터스토어 생성하기</h3>

클러스터 위에서 우클릭해서 데이터스토어를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/143993998-e1af44c9-43ad-456d-b76e-385a6e3b544a.png)

![image](https://user-images.githubusercontent.com/43658658/143994027-54f93cdd-f75c-4727-962a-9293ee717368.png)   
* `VMFS` : 여러 물리적 호스트가 동일 스토리지에 대하여 동시에 읽기 및 쓰기 작업을 수행할 수 있는 클러스터 파일 시스템.

이전에 생성한 공유 스토리지를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144049208-90a4833c-ed15-4c73-8b98-707d6840b5d0.png)

모든 디스크를 사용하도록 하고 데이터스토어를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/144049347-44b28a16-4d03-40dc-a097-870e4abf3cd9.png)   

스토리지 탭으로 들어가면 데이터스토어가 생성된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144049620-49008041-d55d-4d6b-bd72-42eef7051615.png)

> <h3>데이터스토어 언마운트 및 마운트하기</h3>

생성한 데이터스토어에서 마운트 해제 버튼을 누릅니다.   
![image](https://user-images.githubusercontent.com/43658658/144153556-5fa3283d-e0ad-460a-9e56-e4df3f28bd70.png)

마운트 해제할 ESXi 호스트를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144153634-5258785a-2e15-41db-a14b-9822d2a3e9c9.png)

ESXi 호스트와 연결이 해제되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/144153765-aefe3f62-3f47-44b5-9546-78c94281d2ac.png)

다시 마운트를 위해 마운트 버튼을 누릅니다.   
![image](https://user-images.githubusercontent.com/43658658/144153835-d730b10f-e996-414f-b93d-f1220eea1e5a.png)

호스트를 선택하고 마운트를 합니다.   
![image](https://user-images.githubusercontent.com/43658658/144153919-e814e6d4-4add-4002-8e6a-7cac410448b2.png)   

다시 정상적으로 연결됩니다.   
![image](https://user-images.githubusercontent.com/43658658/144153952-58299f23-5495-49ab-a6bd-cc212fdcc291.png)

> <h3>데이터스토어 탐색</h3>

데이터스토어 내부의 파일을 확인하겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/144154219-1909509c-abe3-4754-af8e-34a03b96b32f.png)

가상머신 역시 파일로 나타납니다. 현재는 가상머신이 없습니다.   
![image](https://user-images.githubusercontent.com/43658658/144154325-ec233687-cd57-4fa1-8475-5f84a0a1bc6d.png)

ISO라는 이름의 폴더를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/144154433-ab1c5ec0-dcbd-4ef2-b197-d1b004a8335a.png)

ISO 파일을 업로드 해봅니다.   
![image](https://user-images.githubusercontent.com/43658658/144154822-66438266-aadf-4b35-b4f7-d2cc91bacdcf.png)

데이터스토어에 업로드한 iso 파일을 통해서 손쉽게 가상머신에 OS를 설치할 수 있습니다.

> <h3>데이터스토어 용량 증가</h3>

데이터스토어의 용량을 증가시키기 위해서는 2가지 방법이 있습니다.   
1. 새로운 데이터스토어를 만듭니다.
2. 기존 데이터스토어에 새로운 LUN을 추가하여 용량을 확장합니다.

현재 데이터스토어의 용량은 아래와 같습니다.   
![image](https://user-images.githubusercontent.com/43658658/144155422-3741f6d8-e934-4f44-ae20-559f65dcdef3.png)

데이터스토어의 용량을 증가시켜 보겠습니다.

먼저 새로운 가상 디스크를 만듭니다.   
![image](https://user-images.githubusercontent.com/43658658/144155261-87be0cd8-be54-4ed0-a911-9daa09127279.png)

사용 가능한 공간에서 적당한 양을 할당합니다.   
![image](https://user-images.githubusercontent.com/43658658/144155361-df8e0934-6087-44f5-ad05-ed1f13398878.png)

전체 설정을 확인하고 만듭니다.   
![image](https://user-images.githubusercontent.com/43658658/144155699-a133c73b-5268-4367-8bbe-24c267098724.png)

2개의 ESXi 호스트에서 각각 `스토리지 다시 검색`을 실행합니다.   
![image](https://user-images.githubusercontent.com/43658658/144155809-f91b86f5-baf7-44a0-a71d-dd2c51dc66b6.png)

새로 생성한 가상 디스크가 추가되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/144155902-82fdf4eb-786a-4d2a-b6d8-304301e5c917.png)

이제 기존 데이터스토어에 새로 생성한 가상 디스크를 추가하겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/144155996-5b2dadd9-3093-43f6-b9b7-8a70203f5383.png)

새로 생성한 디스크를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144156044-917e60ec-c24c-4202-8c03-9d22b83785ac.png)

용량을 증가시키면 아래와 같이 용량이 1GB 확장된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144156095-d5dafcce-98cb-4428-b3f9-5b117806821f.png)
