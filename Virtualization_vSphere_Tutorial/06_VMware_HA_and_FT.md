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

![image](https://user-images.githubusercontent.com/43658658/144362795-79023fc5-a01e-4f76-ad46-a5256d4bf9ea.png)   

vCenter에서 vSphere HA 기능을 활성화하면 각 호스트에 `FDM 에이전트`가 설치되면서 `HA 클러스터`를 구성합니다.   
이때, 각 호스트들 사이에서 마스터(1대)와 슬레이브 호스트(나머지)를 선정합니다.   

각 호스트는 HA 구성에 문제가 없는지 지속적으로 상호 적합성을 체크합니다.   
마스터에 장애가 발생하면 다른 슬레이브들 사이에서 마스터를 선정합니다.   

## vSphere HA 구성

[클러스터] > [설정] > [vSphere Availability] > [편집]   
![image](https://user-images.githubusercontent.com/43658658/144363334-0a5418e7-e6a7-407a-ac33-29e7f1be2612.png)

vSphere HA 기능을 켭니다.   
![image](https://user-images.githubusercontent.com/43658658/144364252-af9385b7-e51a-4027-84d6-3dc7513d937e.png)   
* `호스트 모니터링 사용` : 클러스터 내의 호스트들 사이에서 `하트비트`를 교환합니다.
  - `하트비트` : 각 호스트가 제대로 가용되고 있는지에 대한 정보로 이를 통해 호스트의 장애를 체크할 수 있습니다.
* 아래쪽 박스는 호스트에 `각종 이슈가 발생 했을 때의 대응법`을 설정합니다.

`[승인 제어]`에서는 페일오버 활성화의 임계값과 가용량을 지정해줍니다.   
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

FT는 무정지서버입니다. 1년에 5분 이내의 다운 타임을 허용하는 서버입니다.   

하나의 호스트가 내의 가상머신이 FT 서비스를 담당하면, 다른 호스트에는 쉐도우 가상머신이 생성됩니다.   
운영 가상머신에 장애가 발생할 때까지 쉐도우 가상머신은 아무런 서비스를 수행하지 않습니다.   
호스트에 장애가 발생하면 그 즉시 쉐도우 가상머신이 서비스를 시작합니다.

일반 이중화는 한쪽 서비스에 장애가 발생하면 페일오버 시간이 발생하는데, FT와 같은 서비스는 페일오버 시간이 거의 0에 가깝습니다.




