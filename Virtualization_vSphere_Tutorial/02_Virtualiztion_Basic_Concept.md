# 가상화 기초 지식

AWS, Azure, GCP 등 현존하는 클라우드 컴퓨팅은 대부분 `x86 가상화`를 기반으로 하고 있습니다.

`x86 가상화(x86 virtualization)` : x86 기반의 게스트 OS(윈도우, 리눅스 등)이 `하이퍼바이저`라는 가상화 레이어 위에서 구동하는 방식입니다.
* `x86` : 80x86 이라는 인텔이 개발한 마이크로프로세서 계열을 통칭하여 부르는 말
* 우리가 알고 있는 대중화된 Intel CPU 혹은 AMD는 모두 x86 계열입니다.

CPU의 성능이 점점 증가하면서 가상화를 통해 하나의 서버에 여러 대의 OS를 동시에 운영하더라도 성능 저하 없이 운영할 수 있는 파워를 얻게 되면서 가상화 기술이 각광 받게 되었습니다.

## 서버 가상화

한 대의 물리적인 서버에 여러 대의 논리적인 가상머신(VM)을 작동시키는 것을 말합니다.

각 가상머신에는 OS가 설치되고, 완전히 독립되어 있습니다. 가상머신들은 물리적인 서버에 설치된 `하이퍼바이저`에 의해 관리됩니다.

> <h3>서버 가상화의 특징</h3>

1. 각 가상머신은 서로 완전히 독립되어 있습니다.
2. 물리적인 호스트 서버에 종속되어 있지 않습니다.

> <h3>서버 가상화의 장점</h3>

1. 서버 리소스를 공유하여 `비용을 절감`할 수 있습니다.
2. 유지보수의 `편의성이 증대`됩니다.   
* 호스트 서버에 유지보수 및 장애가 발생하면, 가상화 클러스터를 이용해 기존 가상머신을 다른 서버로 이동시킨 다음 장애가 발생한 호스트 서버를 손쉽게 조치할 수 있습니다.
3. `시스템 가용성이 증대`됩니다.   
* 호스트 서버에 장애가 발생하면 OS의 가용성이 떨어지는데, 이때 클러스터로 구성된 가상머신 팜이 호스트 서버 위의 가상머신들을 `다른 쪽으로 이동`시켜 `가용성을 보장`할 수 있습니다.

## 하이퍼바이저

호스트 컴퓨터에서 다수의 운영 체제를 동시에 실행하기 위한 논리적 플랫폼을 말합니다.

서버라는 하드웨어에 `얇은 막을 하나 설치`하고 그 막을 통해 `물리적인 서버를 통제`합니다.   
그리고 그 위에 여러 개의 OS가 동작할 수 있는 가상머신을 만들 수 있는 바탕을 이루는 역할을 합니다.

> <h3>하이퍼바이저 Type</h3>

#### 하이버파이저 방식(베어메탈 방식)

![image](https://user-images.githubusercontent.com/43658658/143515242-02bb0d99-edd4-454d-8db7-b012de8733eb.png)   
서버에 하이퍼바이저 제품을 직접 설치하는 방식입니다.   
VMware의 ESXi Server, Citrix의 XenServer, MS의 Hyper-V 서버 등이 `베어메탈 방식`의 하이퍼바이저입니다.   
`ISO 이미지 파일`을 통해 부팅해서 설치합니다.

#### 호스티드 방식

![image](https://user-images.githubusercontent.com/43658658/143515261-8ec4d616-c1b8-41f0-8d15-c331e74e378f.png)   
기존에 설치된 OS(윈도우, 리눅스) 위에 응용 애플리케이션으로 가상화 제품을 설치하고 그 위에 가상머신을 만들어서 운영하는 방식입니다.   
VMware의 VMware Workstation Pro, VMware Player Pro, Oracle의 VirtualBox 등이 대표적인 프로그램입니다.

개인용 개발 및 테스트를 할 경우 많이 활용하고, 개인의 데스크톱이나 노트북에 설치해 사용합니다.

## 가상화 아키텍쳐 구현 방식

> <h3>유저 모드와 커널 모드</h3>

`커널 모드` : 하드웨어를 직접 다루는 모드   
`유저 모드` : 어플리케이션을 다루는 모드

전통적인 방법에서는 유저 모드가 어플리케이션에 대한 콜에 대응하고, 커널 모드가 OS에 대한 콜에 각각 대응했습니다.   

만약 여러 대의 OS가 하나의 하드웨어를 동시에 컨트롤하려고 한다면 문제가 발생할 수 있는데, Intel-VT와 AMD-V가 이 부분을 해결합니다.   

Intel-VT, AMD-V 환경(`Root 모드`)에서는 OS로부터의 콜과 어플리케이션으로부터의 콜을 한 번에 대응하고 관리합니다.


