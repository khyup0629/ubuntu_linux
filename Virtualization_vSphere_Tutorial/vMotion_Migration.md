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

## EVC 설정

CPU 간의 호환성을 맞추기 위해 EVC를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/144234434-358d7f63-4800-4115-a18f-6c41cc0ca193.png)

[CPU/EVC Matrix](https://www.vmware.com/resources/compatibility/search.php?deviceCategory=cpu) 참고 사이트

`172.16.0.172`와 `172.16.0.173`의 CPU는 아래와 같이 동일하지만 ESXi 버전이 다릅니다.   
![image](https://user-images.githubusercontent.com/43658658/144233957-7fa3995f-e4b6-4c59-979d-5bb436e4a8d4.png)   
![image](https://user-images.githubusercontent.com/43658658/144233967-653d10c1-20bb-4102-85ba-6c8d3aa12c99.png)

`172.16.0.172`의 가능한 EVC 모드는 아래와 같습니다.   
![image](https://user-images.githubusercontent.com/43658658/144234134-030fddc0-f537-49aa-ba7a-3c773f486ca5.png)

`172.16.0.173`의 가능한 EVC 모드는 아래와 같습니다.   
![image](https://user-images.githubusercontent.com/43658658/144234304-5a361af3-1073-4181-8432-4fd121eee4c3.png)

인텔의 `Merom`으로 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/144234564-c46b98ad-5d7d-4c6c-8393-e671a8892512.png)




