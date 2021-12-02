# vMotion Migration

`vMotion` : 가상머신을 현재 동작하고 있는 호스트에서 다른 호스트로 실시간으로 이동하는 기능(가상머신을 `온라인 마이그레이션`한다).

## vMotion 절차

1. 유저는 현재 `ESXi 호스트 1`의 `가상머신 A`에 접속 중입니다. 가상머신의 동작 중에 `ESXi 호스트 1`에서 `ESXi 호스트 2`로 vMotion을 실시합니다.
2. 메모리를 `ESXi 호스트 1`에서 `ESXi 호스트 2`로 사전 카피합니다. 카피 진행 중에 발생하는 메모리 변경분은 메모리 비트맵에 저장됩니다.
3. `ESXi 호스트 1`의 `가상머신 A`를 정지합니다. 아주 순간적이라 유저는 알 수 없습니다. `메모리 비트맵`을 `ESXi 호스트 1`에서 `ESXi 호스트 2`로 카피합니다.
4. `ESXi 호스트 2`에서 `가상머신 A`를 가동합니다. 유저는 `ESXi 호스트 2`로 넘어가 `가상머신 A`에 접속됩니다.
5. 이동이 완료되고 나면 `ESXi 호스트 1`의 `가상머신 A`가 삭제됩니다.

## vMotion 실습

`172.16.0.172` 위의 윈도우 서버 가상머신을 `172.16.0.173`으로 옮기는 `마이그레이션` 실습을 해보겠습니다.   

먼저 `172` ESXi 호스트에 가상머신을 생성합니다. 이때 스토리지는 `공유 스토리지를 선택`합니다.   
![image](https://user-images.githubusercontent.com/43658658/144239360-045be610-8217-4ad7-9332-c7e8db57ae0b.png)

마이그레이션을 시작합니다.   
![image](https://user-images.githubusercontent.com/43658658/144239598-4ace6ee2-8028-4eb7-87e5-55b544e2a4b4.png)

vMotion을 수행할 것이므로 `계산 리소스만 변경`을 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/144229181-0dc26f6b-6ea1-4f08-827e-c413d07b1229.png)
* `스토리지만 변경` : Storage vMotion을 수행.
* `계산 리소스 및 스토리지 모두 변경` : vMotion + Storage vMotion

목적지 호스트를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144238162-a81c6fbd-103f-4c94-87a1-fcd8c9c67cf5.png)   
(이 과정에서 만약 생성한 가상머신이 공유 스토리지에 설치되지 않았다면 목적지 호스트가 나타나지 않습니다)

마이그레이션할 네트워크를 선택합니다. 목적지 호스트의 VM 네트워크를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144238817-69b634ab-8c02-4b75-96c9-0771c5608ead.png)

마이그레이션을 수행하면 아래와 같이 호스트가 `173`으로 넘어간 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144238988-24fc273b-f52d-4764-b5b1-6250ddd8306e.png)

## DRS

`DRS` : 여러 개의 호스트가 하나의 거대한 리소스 풀을 형성하고, 하나의 호스트에서 특정 가상머신에 많은 부하가 발생할 경우 그 가상머신을 다른쪽 호스트로 강제 마이그레이션하는 기능을 처리합니다.

즉, `전체 ESXi 호스트의 리소스를 로드밸런싱`할 수 있습니다.

> <h3>DRS 구성</h3>

DRS를 구성해봅시다.   
![image](https://user-images.githubusercontent.com/43658658/144337975-c8f765ca-3956-41db-91dd-872c36772279.png)   

`vSphere DRS` 기능을 켭니다.
![image](https://user-images.githubusercontent.com/43658658/144338256-f6704b00-1e74-4945-aa5f-458efea42c18.png)   
* `자동화 수준`
  - `완전히 자동화됨` : 가상머신의 전원을 켜면 가상머신이 적당한 ESXi 서버에 자동으로 배치. 이후 작동 중에는 클러스터 내의 부하를 확인하여 자동 배치.
  - `부분적으로 자동화됨` :  가상머신의 전원을 켜면 적당한 ESXi 서버에 자동 배치. 이후 작동 중에는 클러스터 내의 부하를 확인하여 관리자에게 배치 추천.
  - `수동` : 가상머신의 전원을 켤 때 관리자가 ESXi 서버 선택. 이후 클러스터 내의 부하를 확인하여 관리자에게 배치 추천.

![image](https://user-images.githubusercontent.com/43658658/144338608-a3939dab-cb74-4986-8d52-c82fcc31f0e6.png)   
* `마이그레이션 임계값` : 클러스터 내 부하의 변동폭이 크다면 `적극적`으로 해두고, 그렇지 않은 경우에는 `보수적` 쪽을 선택.

![image](https://user-images.githubusercontent.com/43658658/144339284-67c608a8-4b71-4fcb-887f-093d2ce53c39.png)   
`DRS`는 최근 5분 동안의 가상 시스템 요구량을 참고하여 클러스터 내 호스트의 로드를 조정하는 반면, 
`Predictive DRS`는 `vRealize Operations Manager`가 제공한 데이터를 기반으로 작업을 수행합니다.   
vRealize Operations Manager는 vCenter Server에서 실행 중인 가상 시스템을 모니터링하고, 장기적인 기간별 데이터를 분석하여, 예측 가능한 패턴의 리소스 사용량에 대한 예상 데이터를
Predictive DRS에 제공합니다.

![image](https://user-images.githubusercontent.com/43658658/144339250-3262552f-4e8d-4402-aa81-23c2b7629198.png)   
* DPM : ESXi 호스트의 리소스가 사용되고 있지 않은 경우, DRS 기능을 이용해 가상머신을 자동으로 다른 호스트로 옮기고 호스트의 전원을 끄는 기능입니다.

클러스터의 DRS가 구성 되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/144339869-88fdfea0-9373-4219-86a2-16f5f6d5f1ad.png)

> <h3>DRS 수동 설정 테스트</h3>

DRS의 경우 자동으로 테스트하는 것이 쉽지 않으므로, 수동으로 설정하고 실습을 진행해보겠습니다.

`vSphere DRS` 설정에서 다시 `[편집]`을 누릅니다. 자동화 기능을 수동으로 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/144340238-e21094b8-fff0-465f-a17d-ea354fecd280.png)

`172.16.0.173` 호스트 위의 가상머신 하나를 선택해서 전원을 켭니다.   
![image](https://user-images.githubusercontent.com/43658658/144340723-b0b2aeb5-38c1-4f05-9533-c02c6ca78c00.png)

DRS를 `수동`으로 설정했기 때문에 가상머신의 전원을 켤 때 클러스터 내 적당한 호스트를 추천해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144340778-bcbdda97-af8e-4738-bf3b-916c90af24d4.png)

`172` 호스트를 선택하고 [확인]을 누르면 호스트가 이동됩니다.   
![image](https://user-images.githubusercontent.com/43658658/144340903-07aecba4-8167-494a-b911-ba1b8a6f947d.png)

> <h3>DRS 자동 설정 테스트</h3>

비록 자동모드로 부하가 증가 했을 때, 자동으로 마이그레이션 되는지는 확인할 수 없지만, 다른 방식으로 DRS가 작동하고 있는지 확인할 수 있습니다.

다시 클러스터의 `vSphere DRS`로 들어가 [편집]을 클릭합니다. DRS를 `완전히 자동화됨`으로 바꿔줍니다.   
![image](https://user-images.githubusercontent.com/43658658/144341052-5464e726-eedf-43d9-b48e-2035645b2197.png)

가상머신을 생성해봅니다. 진행하다보면 기존에 가상머신을 생성할 때와 차이점을 발견할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144341194-d86db4a8-144e-42a8-82cd-628b98e8cccd.png)
* 기존에는 가상머신을 생성할 때 `클러스터가 아닌 호스트`를 지정해주어야 했습니다.
* 하지만 DRS가 설정되어 있는 상황에서는 `DRS가 알아서 호스트를 지정`해주기 때문에 선택할 필요가 없습니다.

가상머신을 생성하고 호스트를 확인해보면 `173`에 생성된 것을 확인할 수 있습니다.   
(`172` 호스트에는 총 4개의 가상머신이 있었기 때문에 `173`에 로드밸런싱되어 배치되었습니다)
![image](https://user-images.githubusercontent.com/43658658/144341743-9e1aeb5e-10a2-4929-bc41-4929176ae3de.png)

## Storage vMotion Migration

`Storage vMotion` : 가상머신을 다른 데이터스토어로 마이그레이션합니다(가상머신은 데이터스토어 내에 파일 형식으로 저장되어 있습니다).

> <h3>Storage vMotion 실습</h3>

먼저 50GB 정도의 신규 데이터스토어를 생성합니다.
![image](https://user-images.githubusercontent.com/43658658/144348633-74e58dbb-41fe-4fa5-add4-ba4b20de275a.png)

현재 첫 번째 공유 스토리지에 `Migration_Test` 가상머신 파일이 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144348794-a8fc16b3-ae6a-47b5-ab7c-ad9f50adb25c.png)

`Migration_Test` 가상머신의 스토리지를 마이그레이션 하겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/144348941-8c896d80-99b4-4bfc-8a19-2dec17cc41e7.png)

`스토리지만 변경`을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144349018-2589849d-4f35-4e73-b0c9-257c159e9a21.png)

50GB의 두 번째 공유 스토리지를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144349084-32388e0d-d959-4e03-b24e-eefd7a7c4f3d.png)

마이그레이션이 완료되면 두 번째 공유 스토리지에 정상적으로 이동이 된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/144349203-c0ecc6a0-97e1-47eb-a24b-3d91520ea857.png)

마이그레이션이 끝나고 이러한 메시지가 나타났는데,   
![image](https://user-images.githubusercontent.com/43658658/144350234-67a34ba2-4377-4f39-b213-51c9c336333f.png)

재부팅을 하면 해결됩니다.   
![image](https://user-images.githubusercontent.com/43658658/144350842-623c90bd-8d46-4b19-b009-330e0a509a18.png)
![image](https://user-images.githubusercontent.com/43658658/144350873-73d7fc64-1737-4219-8a41-23db06483349.png)

## Storage DRS

`Storage DRS` : 데이터스토어에서 가상머신의 디스크인 VMDK를 `자동으로 이동`시키는 기능입니다.

> <h3>Storage DRS 구성</h3>

`Storage DRS`는 데이터스토어 클러스터를 통해 구성합니다.   

[데이터스토어 탭] > [Datacenter] > [우클릭] > [스토리지] > [새 데이터스토어 클러스터]   
![image](https://user-images.githubusercontent.com/43658658/144351828-d3a16b30-6b23-4719-8c27-093b4a78cd86.png)

`[자동화 안 함(수동 모드)]`를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144352073-f7369379-4811-49a8-bbb4-ee5e4cdaa46a.png)   
밑의 각 옵션은 세부적으로 자동/수동 모드를 지정하는 부분입니다.   
* `공간 조정 자동화 수준` : 클러스터 내 공간 불균형의 로드밸런싱 자동화 여부
* `I/O 조정 자동화 수준` : 인풋/아웃풋 부하 불균형의 로드밸런싱 자동화 여부
* `규칙 적용 자동화 수준` : 클러스터의 선호도 규칙 위반 수정 자동화 여부
* `정책 적용 자동화 수준` : 스토리지 및 VM 정책 위반 수정 자동화 여부
* `VM 제거 자동화 수준` : VM 제거 자동화 여부

고급 기능 설정 부분입니다.   
![image](https://user-images.githubusercontent.com/43658658/144358389-957ec591-71c7-4a26-bedb-07ce5d04128c.png)
`I/O 메트릭` : 가상머신 및 전체 데이터스토어의 I/O를 모니터링하다가 설정값보다 높아지면 자동으로 마이그레이션합니다.   
`Storage DRS 임계값` : 사용된 공간이 일정 수준 이상이 되면 마이그레이션을 권장합니다.

DRS 기능이 적용되는 클러스터를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144358782-c7c06a75-671e-4dba-a201-3e6198966202.png)

DRS 기능이 적용되는 데이터스토어를 모두 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/144358830-307bf9d5-21b9-4311-b9db-cc3f0e354ae0.png)

전체 설정을 확인하고 데이터스토어 클러스터를 구성합니다.   
![image](https://user-images.githubusercontent.com/43658658/144359066-fd8b8978-483f-449d-8b60-90a4f3c305c5.png)

이렇게 데이터스토어를 구성하면 Storage DRS가 활성화 됩니다.   
![image](https://user-images.githubusercontent.com/43658658/144359368-5bde7dbc-8a56-4b90-90b4-54da9a9884c0.png)






