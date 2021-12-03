# vSphere HA

VMware의 `서버 이중화 솔루션`입니다.   
ESXi 호스트 및 가상머신의 장애 발생 시 가상머신을 다른 호스트에서 자동으로 재가동함으로써 고가용성을 보장합니다.

vMotion와의 차이점은 `vMotion`은 호스트의 `전원이 켜져 있을 때` 마이그레이션이 가능한 반면, `vSphere HA`는 `호스트가 다운` 되었을 때 동작합니다.

## vSphere HA 절차

![image](https://user-images.githubusercontent.com/43658658/144362741-ac8a7564-3225-4319-82db-3363faf4c6d9.png)   
1. 하나의 ESXi 호스트에 장애가 발생해서 가상머신을 사용할 수 없게 되었습니다.
2. 가상머신의 사용권을 다른 ESXi 호스트로 넘기고 가상머신을 재가동합니다.
3. 장애가 발생한 ESXi 호스트를 복구하고 vMotion을 통해 가상머신을 다시 이동시킵니다.

## vSphere HA 아키텍처

![image](https://user-images.githubusercontent.com/43658658/144408684-9c32ac5e-fb9d-454e-af6d-98ef394711c9.png)   

vCenter에서 vSphere HA 기능을 활성화하면 각 호스트들이 `HA 클러스터`를 구성합니다.   
이때, 각 호스트들 사이에서 마스터(1대)와 슬레이브 호스트(나머지)를 선정합니다.   

각 호스트는 HA 구성에 문제가 없는지 지속적으로 상호 적합성을 체크합니다.   
또한 각 호스트는 하트비트 데이터스토어와 지속적으로 하트비트를 주고 받으면서 제대로 연결되어 있는지 체크합니다.   

마스터에 장애가 발생하면 다른 슬레이브들 사이에서 마스터를 선정합니다.   

## vSphere HA 구성

[클러스터] > [설정] > [vSphere Availability] > [편집]   
![image](https://user-images.githubusercontent.com/43658658/144363334-0a5418e7-e6a7-407a-ac33-29e7f1be2612.png)

vSphere HA 기능을 켭니다.   
![image](https://user-images.githubusercontent.com/43658658/144364252-af9385b7-e51a-4027-84d6-3dc7513d937e.png)   
* `호스트 모니터링 사용` : 클러스터 내의 호스트들 사이에서 `하트비트`를 교환합니다.
  - `하트비트` : 각 호스트가 제대로 가용되고 있는지에 대한 정보로 이를 통해 호스트의 장애를 체크할 수 있습니다.
* 아래쪽 박스는 호스트에 `각종 이슈가 발생 했을 때의 대응법`을 설정합니다.

`[승인 제어]`에서는 페일오버가 활성화되는 임계값과 가용량을 지정해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144365109-8324530d-32af-47aa-8833-f2e3d8d13889.png)

`하트비트 데이터스토어`로 사용될 데이터스토어를 선택하는 옵션입니다.   
![image](https://user-images.githubusercontent.com/43658658/144365447-45dcec75-02e5-491d-b556-0de110fbad9a.png)   
* vSphere HA는 `하트비트 데이터스토어`를 중간에 두고 클러스터 내의 호스트들과 하트비트를 주고받으면서 실패한 호스트와 네트워크에 있는 호스트를 구분합니다.

구성을 완료했는데, 분리 주소가 정의되지 않았다는 이슈가 발생했습니다.   
![image](https://user-images.githubusercontent.com/43658658/144372376-2334d36d-f665-47d7-a602-7717d45ff512.png)

조사 결과 `분리 주소`는 호스트의 기본 게이트웨이였고, 호스트가 게이트웨이가 설정되어 있지 않은 상태였기 때문에 발생한 이슈였습니다.   
![image](https://user-images.githubusercontent.com/43658658/144375277-4c46c286-2b04-4aee-b3ca-2a18cb9c2f5d.png)

각 호스트의 게이트웨이를 설정해주고 `vSphere HA`를 다시 껐다 켜니 해결되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/144376004-412e4b76-989f-491f-b850-5c158bd76d0c.png)

> <h3>vSphere HA 테스트</h3>

`172.16.0.173`을 종료 했을 때 `173` 호스트 위의 가상머신이 `172`로 옮겨 가는지 테스트하겠습니다.

현재 `HA_Test` 가상머신은 `172.16.0.173` 호스트 위에 존재합니다.   
![image](https://user-images.githubusercontent.com/43658658/144379632-ae6da56e-5cb9-41b7-99d7-49d144454cef.png)

이제 `172.16.0.173` 호스트를 종료합니다.   
![image](https://user-images.githubusercontent.com/43658658/144380191-51fbfe72-a9b6-4592-bff4-c26587ac5db4.png)

`HA_Test`의 호스트가 `172`로 넘어간 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144380422-76c78ae1-0d51-491b-adb7-f6b36f276321.png)

## vSphere FT

FT는 `무정지서버`입니다. 1년에 5분 이내의 다운 타임을 허용하는 서버입니다.   

하나의 호스트 내의 `가상머신`이 `FT 서비스를 담당`하면, 다른 호스트에는 `쉐도우 가상머신`이 생성됩니다.   
운영 가상머신에 장애가 발생할 때까지 쉐도우 가상머신은 `아무런 서비스를 수행하지 않습니다.`   
호스트에 장애가 발생하면 그 즉시 쉐도우 가상머신이 `서비스를 시작`합니다.

일반적으로 이중화는 한쪽 서비스에 장애가 발생하면 `페일오버 시간이 발생`하는데, FT 서비스는 페일오버 시간이 `거의 0`에 가깝습니다.

먼저 100GB 스토리지 2개를 만들어줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144560882-65bba9aa-cddb-42ac-b72d-95470f59fd40.png)

새로 생성한 `FT_TEST` 데이터스토어로 `FT_Test` 가상머신을 생성합니다. 이때 네트워크 어댑터 유형을 `VMXNET3`로 연결합니다.   
![image](https://user-images.githubusercontent.com/43658658/144566381-c68bca70-c846-4435-a07e-a4c8d4ff1daf.png)

이제 본격적으로 FT를 구성해보겠습니다.

먼저 FT로 동기화할 가상머신을 지정하고, [Fault Tolerance] > [Fault Tolerance 설정]을 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/144561504-fb5e5b47-fec8-423c-bdc4-c9bd3a4869b4.png)

아래의 문제가 발생했습니다.   
![image](https://user-images.githubusercontent.com/43658658/144567181-2b9b18d2-f998-42dd-9738-09379fd2c725.png)

이는 VMkernel 포트를 하나 생성해주고, `vMotion`과 `Fault Tolerance 로깅`을 체크해주면 됩니다.   

먼저 가상 스위치를 하나 생성해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144567372-edf45f8d-7fc9-430b-914f-32f66ba8baa3.png)   

VMkernel 네트워크 어댑터를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144567429-27f7cd75-dc37-491f-8fcd-a0410137f650.png)

기존 스위치에서 `vSwitch0`을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144567755-fe989233-ebc6-419a-8b40-0ea905891bee.png)

`vMotion`과 `Fault Tolerance 로깅`을 체크합니다.   
![image](https://user-images.githubusercontent.com/43658658/144567940-67a30e5c-f725-489e-8674-1c75c273ec89.png)

IP를 설정해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144568025-4fa9286c-0029-4340-b016-a5ef372c9811.png)

설정을 완료합니다.   
![image](https://user-images.githubusercontent.com/43658658/144568711-a7dad8af-3066-45ea-ba75-e06518af8dcc.png)

`173` 호스트에 대해서도 설정을 진행합니다.

다시 돌아와서 FT를 설정해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144568309-7e3d0c5d-4eec-4f34-b62d-e132c70c69c8.png)   
이번에는 대역폭 문제가 발생했습니다. 하지만 간단한 테스트 환경을 구성하는 것이기 때문에 대역폭은 중요하지 않습니다. 넘어갑니다.

방금 생성한 데이터스토어를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144568769-ce7330d1-06ff-4a6c-a0d9-804730f5ccfd.png)

Secondary 가상머신이 동작할 ESXi 호스트를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144569110-aee26d4d-2934-44c8-a4d2-d5ccb5a73279.png)

FT구성이 완료되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/144572193-a1fe643d-7c09-4c50-8aef-5b4fa3a99668.png)

Fault Tolerance 상태를 `보호되지 않음`을 `보호됨`으로 바꿔주기 위해 가상머신을 켜서 동기화를 시켜줍니다.   
부팅 중에 동기화를 시키기 때문에 부팅이 오래걸립니다.   
![image](https://user-images.githubusercontent.com/43658658/144570606-ad769c6f-5fb8-4116-b116-b8b0fe9e9c64.png)

정상적으로 FT 구성이 완료되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/144579610-2a6d1a2a-b837-4082-a7e3-708cd4922a98.png)


