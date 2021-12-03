# vSphere Client

## vCenter에 ESXi 호스트 등록하기

vCenter에 ESXi 호스트를 등록해보겠습니다.   

먼저 데이터센터를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/143967825-9d7a778a-c1cc-4c1a-b168-d5a3c8fc2571.png)   
간단하게 이름만 지정해서 생성이 가능합니다.   
![image](https://user-images.githubusercontent.com/43658658/143968050-e4f43a43-6c75-4cdf-8fc0-4251c92b365e.png)   

다음으로 호스트를 추가합니다.   
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

## iSCSI 공유 스토리지 구축 후 ESXi와 연결

RJ45를 통한 네트워크 환경에서 서버와 스토리지를 연결하기 위해 `iSCSI 프로토콜`을 이용합니다.   
(SAN 방식 FC 프로토콜을 통해 SCSI 명령어를 전달하는데, NAS 환경에서는 iSCSI 프로토콜을 이용해 SCSI 명령어를 전달합니다).   

iSCSI 공유 스토리지를 구축하고, ESXi 호스트들을 공유 스토리지와 연결해보겠습니다.   

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

먼저 [서버 관리자]를 실행합니다.   
iSCSI를 이용하기 위해 필요한 것들을 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/144347499-b7e1ac29-e610-4da7-a18f-aec74612fc41.png)   
`iSCSI Target Server`를 선택하면 `File Server`도 선택하도록 되어있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144347614-1fecd09e-59df-4baf-8ea3-2a0308217b19.png)   

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
캐시에 ESXi 호스트가 뜨게 하려면 `vSphere Client`의 ESXi 호스트에서 iSCSI 유형의 스토리지 어댑터를 추가해 스토리지 서버를 가리키도록 설정해야 합니다.   
![image](https://user-images.githubusercontent.com/43658658/144043644-2f0eb5b0-56ad-4306-bd91-150b6852c24b.png)   
![image](https://user-images.githubusercontent.com/43658658/144043682-c64241ab-3208-49fd-b6c3-d71ff5317cec.png)   
![image](https://user-images.githubusercontent.com/43658658/144043852-6869526f-2262-44b5-b179-25c6c55a898f.png)   
![image](https://user-images.githubusercontent.com/43658658/144043891-2b82ed8f-21af-4ed6-9e6f-9d05b00c3ee5.png)   
같은 방법으로 다른 ESXi 호스트도 설정해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144047276-600d4782-2b0f-4106-92c4-14eb6b4f4394.png)   
그리고 다시 스토리지로 돌아와보면, 캐시에 식별자가 추가된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144047321-e17dbc68-f34e-4142-b207-653a1b04ab02.png)   
전체 설정을 확인하고 iSCSI 공유 스토리지를 생성합니다.   
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
![image](https://user-images.githubusercontent.com/43658658/144576526-c4157e27-9e6a-4ad5-8850-6bdb2a170357.png)   

EVC는 클러스터 내의 호스트들의 CPU 호환을 맞춰주는 기술입니다.   
=> [CPU/EVC Matrix](https://www.vmware.com/resources/compatibility/search.php?deviceCategory=cpu) 참고 사이트

`172.16.0.172`와 `172.16.0.173`의 CPU는 아래와 같이 동일하지만 ESXi 버전이 다릅니다.   
![image](https://user-images.githubusercontent.com/43658658/144233957-7fa3995f-e4b6-4c59-979d-5bb436e4a8d4.png)   
![image](https://user-images.githubusercontent.com/43658658/144233967-653d10c1-20bb-4102-85ba-6c8d3aa12c99.png)

`172.16.0.172`의 가능한 EVC 모드는 아래와 같습니다.   
![image](https://user-images.githubusercontent.com/43658658/144234134-030fddc0-f537-49aa-ba7a-3c773f486ca5.png)

`172.16.0.173`의 가능한 EVC 모드는 아래와 같습니다.   
![image](https://user-images.githubusercontent.com/43658658/144234304-5a361af3-1073-4181-8432-4fd121eee4c3.png)

인텔의 `Merom`으로 설정합니다.   

vCenter가 클러스터 바깥에 위치해있다면 위와 같은 설정은 클러스터를 생성하고 이후에 해도 상관없지만,   
vCenter가 클러스터 내의 호스트 위에 가상머신으로 올려져 있다면, 사전에 설정을 해주어야 합니다.   
(나중에 여러 실습을 할 때, CPU의 호환이 맞지 않아 결국 EVC 설정을 해주어야 할 때, 굉장히 번거로워질 수 있습니다)

ESXi 호스트 2개를 드래그 앤 드롭하여 클러스터 안으로 넣습니다.   
![image](https://user-images.githubusercontent.com/43658658/143992141-23096bd5-e1ee-421e-834f-9e9b372e584a.png)

## 데이터스토어

`데이터스토어` : 데이터베이스 뿐만이 아니라 모든 데이터를 저장하고 관리하기 위한 저장소

우리는 앞서 윈도우 서버를 iSCSI 공유 스토리지로 생성했고, ESXi 호스트들과 연결지었습니다.   
이제 VMware에서 사용할 수 있도록 `데이터스토어(가상 파일시스템)로 변환`시키겠습니다.

> <h3>데이터스토어 생성하기</h3>

클러스터 위에서 우클릭해서 데이터스토어를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/143993998-e1af44c9-43ad-456d-b76e-385a6e3b544a.png)

![image](https://user-images.githubusercontent.com/43658658/143994027-54f93cdd-f75c-4727-962a-9293ee717368.png)   
* `VMFS` : 여러 물리적 호스트가 동일 스토리지에 대하여 동시에 읽기 및 쓰기 작업을 수행할 수 있는 클러스터 파일 시스템.

이전에 생성한 공유 스토리지를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144049208-90a4833c-ed15-4c73-8b98-707d6840b5d0.png)

모든 디스크를 사용하도록 하고 데이터스토어를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/144049347-44b28a16-4d03-40dc-a097-870e4abf3cd9.png)   

스토리지 탭으로 들어가면 공유 스토리지가 데이터스토어로 생성된 것을 확인할 수 있습니다.   
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

## VMware 표준 가상 네트워크 연결 및 관리하기

> <h3>vSphere 표준 가상화 스위치의 개념</h3>

최초 ESXi를 설치하면 가장 먼저 가상 표준 스위치(vSwitch)가 생성됩니다.   
![image](https://user-images.githubusercontent.com/43658658/144157411-9566361e-5edf-4d1b-92aa-f3ccd56cccf3.png)   
* vSwitch : 단순히 L2 트래픽이 흐르기만 하는 스위치입니다. 다른 네트워크 스위치처럼 OS가 있지도 않고, telnet으로 접속할 수도 없습니다.

포트 그룹 : 같은 역할을 하는 포트의 논리적인 그룹입니다. vSwitch에는 2종류의 포트 그룹이 있습니다.   
* `VMkernel 포트(Management 포트)` : ESXi와 vCenter 관리를 위한 전용 포트.
* `VM 포트` : VM이 가지는 일반적인 포트.

![image](https://user-images.githubusercontent.com/43658658/144160993-11366215-1c9f-4ff0-b205-7463399fac39.png)   
현재는 테스트 환경이므로, `Management 포트`와 `VM 포트`가 같은 환경에 있습니다.   
이렇게 되면 같은 라인으로 ESXi 트래픽과 VM 트래픽이 동시에 흐르게 되어 효율적이지 못하고 서로 영향을 줄 수 있습니다.   
그래서 실무에서는 두 포트를 서로 분리합니다.

그리고 현재 물리적인 DELL 서버 장비는 4개의 포트가 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144161913-11bf8c60-cad0-47c1-ab05-352cbbecc5d9.png)   
`vSphere Client`를 통해 서버 장비의 pNIC(physical NIC)를 볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144162065-1b4c4a81-7cca-430a-9a53-d2dfa05e8988.png)   
현재 1개의 포트에만 vSwitch가 연결되어 있는 것을 확인할 수 있습니다.

> <h3>별도의 가상 스위치에 VM 네트워크 생성</h3>

앞서 Management 네트워크와 VM 네트워크가 하나의 가상 스위치에 구성되어 있고, 가상 스위치와 물리 서버가 하나의 라인으로 연결되면 발생할 수 있는 문제점에 대해 설명해드렸습니다.   

그래서 별도의 `가상 스위치를 생성`해주고, `가상머신의 네트워크`를 새로 생성한 가상 스위치로 바꿔서 
Management 네트워크와 VM 네트워크의 `역할을 서로 다른 스위치에 분리`해주도록 하겠습니다.

별도의 `가상 스위치를 생성`합니다.   
![image](https://user-images.githubusercontent.com/43658658/144163211-f816c096-5c2f-492f-987d-80dbb21d0953.png)

VM 네트워크를 옮길 것이므로 `표준 스위치용 가상 시스템 포트 그룹`을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144163441-ce773a14-f3fe-42fb-ac47-1114168178be.png)

새로운 스위치를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/144163633-07ccd7b3-ef66-4dfd-9a08-d405e88ee174.png)   
* MTU : 패킷의 최대 용량.

가상 스위치에 할당할 어댑터를 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/144163901-35f99510-a0d8-45e0-a492-8faa387889c3.png)   

`vmnic2`를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144163948-2ae6cbda-bb7b-47db-b1df-413dc202aa46.png)   
* `1`이 아닌 `2`를 선택하는 이유는 이후에 `vmnic0`과 `vmnic1`은 `Teaming`을 구성할 것이기 때문입니다.

활성 어댑터 및으로 올 수 있도록 화살표를 통해서 옮겨줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144164172-d4ee4ed2-ca3d-468e-be2d-16d7f31acb95.png)

전체 설정을 확인하고 가상 스위치를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/144164319-fb59c6d6-0245-4790-8e3a-81231384b30c.png)   
![image](https://user-images.githubusercontent.com/43658658/144164544-d936fe71-917f-49bd-a8bd-8fa48743759f.png)

`vSwitch0`의 VM 네트워크가 아닌 `vSwitch1`의 `1층 VM 네트워크`로 가상머신의 네트워크를 변경해주겠습니다.

테스트를 위해 호스트 `172`에 윈도우 서버 가상머신을 하나 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/144178039-7c2bc3ba-1354-4b03-bb11-d0ba09d15171.png)

윈도우 서버 VM의 네트워크 어댑터를 `vSwitch1`의 `1층 VM 네트워크`로 변경해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144178110-db154f2e-79e2-43d3-a9ee-108b07766a87.png)   
![image](https://user-images.githubusercontent.com/43658658/144178158-bfd2c13e-288f-4be5-811e-72debb1550a1.png)   
![image](https://user-images.githubusercontent.com/43658658/144178191-07038750-c182-413e-847f-f3d506edfe35.png)

> <h3>Teaming을 통한 NIC 이중화</h3>

`Teaming` : 2개 이상의 물리 NIC를 묶어서 하나의 이더넷 포트로 사용하는 기술입니다.   

이렇게 티밍을 하는 것을 `이중화`라고 부릅니다.   

`이중화`를 하는 이유는 가용성 때문입니다. VM 네트워크 담당 NIC가 1개 밖에 없다면 1개의 라인에 문제가 생겼을 때 그 즉시 네트워크가 끊기게 됩니다.   
이중화를 하면 한 라인이 끊겨도 다른 라인을 통해 연결이 가능하기 때문에 `가용성이 높아`집니다.

서버 이중화, 네트워크 스위치 이중화, 네트워크 라인 이중화, 스토리지 이중화 등 `모든 구성요소의 이중화는 필수`입니다.

`[물리적 어댑터 관리]`를 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/144180674-ad744a79-1916-4ca3-87c7-53b6f780e5ba.png)   

어댑터를 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/144180934-0d3def8a-270c-4fe6-a3f3-5150db5b67af.png)

`vmnic2`와 `vmnic3`을 티밍할 것이므로 `vmnic3`를 선택해주고, `대기 어댑터`로 화살표를 이용해 내립니다.   
![image](https://user-images.githubusercontent.com/43658658/144181039-cc1dda48-dab7-4c58-876a-e87e9be79b8d.png)

설정을 모두 완료하면 `vmnic2`와 `vmnic3`가 묶인 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144181146-406b257e-6539-4521-8d11-f24a2a45c0d3.png)
* 지금 활성화된 어댑터는 `vmnic2`이고, 대기 중인 어댑터는 `vmnic3`입니다.
* `vmnic2`에 장애가 발생하면, 트래픽이 `vmnic3`로 페일오버(failover)되어 전송됩니다.

vmnic2, vmnic3로 모두 트래픽이 전송되도록 하고 싶다면 둘 다 활성화하고 로드 밸런싱을 하면 됩니다.   
![image](https://user-images.githubusercontent.com/43658658/144181692-c2ca4072-3ce7-46ab-9c4f-bae6f2e6ec53.png)

`vmnic0`과 `vmnic1`도 마찬가지로 티밍해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144184492-1585b12f-9306-4a37-9b28-38fadb03802e.png)

## 가상머신 관리

> <h3>가상머신 생성</h3>

vSphere Client에서 `172.16.0.172` 호스트 위에 가상머신을 생성해보겠습니다.   

호스트 위에서 우클릭해 [새 가상 시스템]으로 가상머신 생성 마법사를 시작합니다.   
![image](https://user-images.githubusercontent.com/43658658/144191903-d1b2fbe9-5e34-4cd4-95f3-898362d3612d.png)

데이터센터 선택.   
![image](https://user-images.githubusercontent.com/43658658/144191945-cb103b00-8c1b-47db-a1f6-3de7f5e55fc8.png)

호스트 지정.   
![image](https://user-images.githubusercontent.com/43658658/144192037-b81bb27b-d619-44bb-8f3f-9c15c5fce950.png)

스토리지 선택.   
![image](https://user-images.githubusercontent.com/43658658/144192089-36a8c5ed-237e-45d4-b52f-3f24c2a1be1b.png)

모든 설정을 확인하고 가상머신을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/144192316-accf0e61-fcbf-4625-8410-379733146b0b.png)

가상머신이 생성되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/144192382-83867b21-d0a6-49fa-bb08-4e54c9224aa3.png)

가상머신에 iso 파일을 연결하고 전원을 켭니다.   
![image](https://user-images.githubusercontent.com/43658658/144192644-0affba57-cc6d-44a5-8ee8-8f7681fb4266.png)

`[웹 콘솔 시작]` 버튼을 누르면 웹으로 화면을 볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144192721-b6687d99-cbb5-4b31-ab45-787b105e3c90.png)

> <h3>가상머신 삭제</h3>

가상머신을 삭제하는 방법은 2가지가 있습니다.   
1. 인벤토리에서 삭제하기
2. 디스크에서 삭제하기

(삭제할 때는 가상머신을 종료한 상태여야 합니다)

#### 인벤토리에서 삭제

인벤토리에서 삭제합니다.   
![image](https://user-images.githubusercontent.com/43658658/144192900-bc5e5a00-8c2d-48f5-8394-80ed37783c7b.png)

인벤토리에서는 삭제 되었지만,   
![image](https://user-images.githubusercontent.com/43658658/144193030-5a7fcecd-0204-43d7-9d6b-9e7b4c90f644.png)   

데이터스토어에는 여전히 존재합니다.   
![image](https://user-images.githubusercontent.com/43658658/144193146-a9284168-9c1a-4aaf-bbc8-66c98e3cd160.png)

#### 인벤토리에 다시 등록하기

`.vmx` 파일을 다시 등록합니다.   
![image](https://user-images.githubusercontent.com/43658658/144193288-97658bac-1df4-4db5-8130-f6f63af44efa.png)

가상머신을 생성할 때와 비슷한 화면이 나타납니다. 데이터센터와 호스트를 지정해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144193380-236a9f5e-ad3b-4e6c-9463-a5bea7c44653.png)   
![image](https://user-images.githubusercontent.com/43658658/144193450-1561a671-eba9-45a2-a927-d46e77e72b6c.png)

짜잔!   
![image](https://user-images.githubusercontent.com/43658658/144193523-9496c9b0-4474-4bce-945e-d7e979392e6c.png)

#### 데이터스토어에서 삭제

데이터스토어에서 삭제하는 방법 역시 간단합니다.   
![image](https://user-images.githubusercontent.com/43658658/144193681-5e6dc5d5-4265-4004-8196-1a40c6f8f170.png)

데이터스토어에서 아예 사라졌습니다.   
![image](https://user-images.githubusercontent.com/43658658/144193838-14a9f2e1-22f5-4cb8-9dc8-b1a98ac0549a.png)

## 스냅샷 관리

스냅샷 : 가상머신 상태를 포착하여 보존하는 기능입니다. 관리자가 원하는 경우에 언제든지 스냅샷을 생성한 시점으로 돌아갈 수 있습니다.   
- 가상머신의 on/off 유무와 상관 없이 생성이 가능합니다.
- 스냅샷을 생성할 경우 가상머신의 `모든 데이터와 메모리 값`은 그대로 유지됩니다.

> <h3>스냅샷 생성</h3>

스냅샷 생성 버튼을 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/144194846-d5e4cb05-f9ee-41d4-af48-18dc9a9ef68a.png)

스냅샷 이름과 설명을 지정합니다.   
![image](https://user-images.githubusercontent.com/43658658/144195030-71ad9700-9820-43fb-aa78-de85214ccc66.png)   
* `가상 시스템 메모리 스냅샷` : 가상머신의 메모리 데이터를 스냅샷에 포함시킬지 여부.
* `게스트 파일 시스템 일시정지` : 게스트 OS의 작동을 일시정지시켜 정확하게 스냅샷을 하기 위함.

스냅샷을 생성하면 데이터스토어에 스냅샷 관련 파일이 추가됩니다.   
![image](https://user-images.githubusercontent.com/43658658/144199437-989c047c-49f9-4ecf-abae-5f4461171745.png)   
* `.vmdk` : 새로 생성되는 파일로 델타디스크 파일입니다. 스냅샷을 생성하면 이후로 가상머신의 데이터는 가상 디스크가 아닌 델타디스크에 보관됩니다. 
이후에 스냅샷을 복원하면 델타 디스크에 저장된 내용은 사라집니다.
* `.vmsn` : 새로 생성되는 파일로 스냅샷 생성 당시 가상머신의 메모리 데이터를 보관하는 파일.
* `.vmsd` : 스냅샷의 히스토리를 보관하는 파일. 기존에 존재하는 파일로 스냅샷을 삭제하더라도 파일은 삭제되지 않습니다.

> <h3>스냅샷을 통한 복원</h3>

스냅샷 관리로 들어갑니다.   
![image](https://user-images.githubusercontent.com/43658658/144198144-c060bacc-3aba-4159-9fcb-28f4f35004d8.png)

생성한 스냅샷을 선택하고 복원합니다.   
![image](https://user-images.githubusercontent.com/43658658/144198253-e5fe16e4-2eb3-4f1f-add5-a5c68dc1ec56.png)

## 클론과 템플릿

동일한 역할을 하는 가상머신을 손쉽게 여러 개 만들어낼 수 있는 기능입니다.   
OS 뿐만이 아닌 어플리케이션까지 복제할 수 있습니다.

> <h3>클론 가상머신 생성</h3>

`클론` : 기존 가상머신을 완전히 똑같은 가상머신으로 만들어 내는 기능입니다.   
- OS 내에 있는 호스트 이름, IP 주소, MAC 주소까지 모두 복제됩니다.

![image](https://user-images.githubusercontent.com/43658658/144202255-232f25db-e26e-43dc-8eca-d98b13623b82.png)   
![image](https://user-images.githubusercontent.com/43658658/144202428-d9514292-54ed-4ef4-8d71-81c7f6bb1f2c.png)   
![image](https://user-images.githubusercontent.com/43658658/144202452-fbf00c09-a60c-4275-8150-5b3a743e87d6.png)   
가상머신이 저장될 데이터스토어 선택.   
![image](https://user-images.githubusercontent.com/43658658/144202569-570d8265-7e17-49d3-b8ad-3961ef29d190.png)   

복제 옵션을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144202799-3398f9a9-561e-4cc7-b6a8-3ff9f0854d1a.png)   
* `운영체제 사용자 지정` : 운영체제의 설정(호스트 이름, 네트워크 설정 등)을 변경.
* `가상 시스템 하드웨어 사용자 지정` : 가상머신의 하드웨어 스펙을 변경.
* `생성 후 가상 시스템 전원 켜기` : 생성 후에 다른 설정 없이 바로 전원 가동.

여기서 운영체제 설정 변경을 위한 사용자 규격이 나타나 있지 않습니다.   
![image](https://user-images.githubusercontent.com/43658658/144205775-2daa06b7-dd46-455f-9b34-723c3d828772.png)

이를 위해 `사용자 지정 규격 관리자`로 들어가서 규격을 만들어줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144205827-54d3a5c0-74cd-4cc8-95bc-c19943f5d1da.png)

`사용자 지정 규격 이름`을 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/144206198-84238518-cc5d-4c10-8b71-052ba52c1921.png)   
`게스트OS 이름`을 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/144206599-3b75a056-c734-4208-a3ab-97331b4f11a6.png)   
`제품 키` 입력은 생략해도 됩니다.   
![image](https://user-images.githubusercontent.com/43658658/144206733-c801850a-46ed-4eae-8d53-1b24dac89540.png)   
`암호`를 지정합니다.   
![image](https://user-images.githubusercontent.com/43658658/144206826-d3451304-b2e5-4d8c-a4e0-48e172714140.png)   
`IP 주소`를 수동으로 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/144207129-03a28940-4758-46df-b669-b8ab6dbee061.png)   
![image](https://user-images.githubusercontent.com/43658658/144207459-6bfe3a5a-5ce9-4010-8960-4ca85f35fb35.png)   
전체 설정을 확인하고 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/144207685-0db28f1b-673c-4856-9dc5-15e7eba98c27.png)

다시 클론 생성 화면으로 돌아옵니다.   
`사용자 지정 규격`을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/144207913-cf81091a-599d-4317-8b6b-c0ba29f280fc.png)   
전체 설정을 확인하고 클론 가상머신을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/144208116-e84c1e23-f591-4012-8aa0-e35d84f7ec97.png)

복제된 가상머신이 생성되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/144208197-af0138cd-c8ce-4693-aaa9-e72ed239249a.png)

> <h3>템플릿을 통한 가상머신 생성</h3>

`템플릿` : 템플릿은 완벽하게 복제하지 않습니다. OS 및 어플리케이션이 설치되어 있다면 그 큰 틀만 복제를 해줍니다.

템플릿을 생성하려는 가상머신을 종료하고 템플릿을 만듭니다.   
![image](https://user-images.githubusercontent.com/43658658/144210266-64cf081a-4f58-4dc0-bc6b-f824f07bcd5f.png)

`[VM 및 템플릿]` 탭으로 가면 생성된 템플릿을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144210546-2388ed53-5a6e-47c3-9300-d823bae454be.png)

템플릿으로 새 VM을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/144210688-57159331-26a9-4210-bd28-407bbb83a427.png)

클론 복제와 비슷한 환경이 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/144210773-d43c5754-e0ca-42a3-ab00-fb393bf20ff3.png)

클론 복제와 같이 옵션을 선택할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144211050-c5701e2f-bcff-4682-baf3-8ccd85bf00c9.png)

클론 실습 당시 생성했던 `사용자 지정 규격`을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144211105-2b821108-03fe-4cff-9a98-f0163f83b027.png)

마침 버튼을 누르면 템플릿으로 생성된 가상머신을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144211324-0ad4670e-64b0-4a93-b63b-71a49791dec9.png)   



