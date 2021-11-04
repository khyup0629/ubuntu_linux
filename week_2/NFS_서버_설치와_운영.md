# NFS 서버 설치와 운영

NFS(Network File System) : 리눅스 끼리 저장 공간을 공유하는 시스템

![image](https://user-images.githubusercontent.com/43658658/140259959-178b4d46-f7d9-44f0-8d15-5583ff2419a0.png)   

1. NFS 서버에 관련 패키지를 설치합니다.
2. NFS 서버의 `/etc/exports`에 공유할 디렉토리, 접근을 허가할 컴퓨터, 접근 권한을 지정합니다.
3. NFS 서비스를 실행합니다.
4. NFS 클라이언트에 관련 패키지를 설치합니다.
5. NFS 클라이언트에 `showmount` 명령어를 통해 NFS 서버에 `공유된 디렉토리`가 있는지 확인합니다.
6. NFS 클라이언트에 `mount` 명령어를 통해 NFS 서버에 `공유된 디렉토리를 마운트`합니다.

> <h3>NFS 서버 구축</h3>

![image](https://user-images.githubusercontent.com/43658658/140260728-ba2b57a9-f867-4337-9764-4637c4c7808b.png)   
NFS 서버 패키지 설치   
![image](https://user-images.githubusercontent.com/43658658/140262904-b51b99e3-bf8d-4eda-aeb5-409d01be06a4.png)   
`/etc/exports` 파일에 내용을 추가합니다.
* `share` 디렉토리를 공유하고, 해당 IP 주소의 접근을 허용하며, 접근 권한은 읽기(r), 쓰기(w)를 부여합니다.

![image](https://user-images.githubusercontent.com/43658658/140262087-add73e1c-7e39-48ba-847c-80e7cf51b6ed.png)   
![image](https://user-images.githubusercontent.com/43658658/140262114-175eecac-d3c4-4ae0-80b5-4ba78fd8439c.png)   
![image](https://user-images.githubusercontent.com/43658658/140262167-45a9b1dd-515c-4bfc-a186-bb1e51d0760d.png)   
![image](https://user-images.githubusercontent.com/43658658/140262199-4a99ca7f-b7c3-4ad6-9174-a202b3bfc2d7.png)   
`share` 디렉토리를 생성하고, 707 허가권을 준 뒤, 적당한 파일을 file1이라는 이름으로 복사해서 `share` 디렉토리 안에 넣습니다.

![image](https://user-images.githubusercontent.com/43658658/140262296-cdacb585-5d6a-42fb-b180-76c2cbbbfebd.png)   
NFS 서버 재시작, 상시 가동   
![image](https://user-images.githubusercontent.com/43658658/140263088-318dba3e-a479-4391-a0e0-e949c7913ad3.png)   
`exportfs -v` 명령어로 서비스가 가동하는지 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/140264070-9c4cccae-1022-4883-b573-a086c313a8d7.png)   
클라이언트에서 접속이 가능하도록 방화벽을 비활성화합니다.

> <h3>NFS 클라이언트 구축 및 NFS 서버 접속</h3>

![image](https://user-images.githubusercontent.com/43658658/140263174-55688b7d-62af-4369-bc53-51471e60db15.png)   
다른 리눅스 컴퓨터에 NFS 클라이언트 패키지를 설치합니다.   

![image](https://user-images.githubusercontent.com/43658658/140264117-feb63445-16df-429f-9229-2cce8f87d3b0.png)   
NFS 서버에 공유된 디렉토리를 확인합니다.

![image](https://user-images.githubusercontent.com/43658658/140264400-0cc2e2ba-5986-4eda-887c-1626a99874c7.png)   
`myShare` 디렉토리를 만들고, NFS 서버의 `share` 디렉토리에 마운트 시킵니다.
* `-t NFS` : NFS 서버의 공유 디렉토리를 마운트 할 때 사용합니다.
* `mount -t 파일시스템 NFS서버공유디렉토리 NFS클라이언트마운트디렉토리`
* `myShare` 디렉토리로 들어가 보면, NFS 서버의 공유 디렉토리 `Share`와 같은 파일이 들어있음을 확인할 수 있습니다.

> <h3>부팅 시 자동 마운트 설정</h3>

NFS 클라이언트가 부팅될 때마다 NFS 서버의 공유 디렉토리에 자동으로 마운트 되도록 설정해봅시다.

![image](https://user-images.githubusercontent.com/43658658/140264892-24e3b127-cbe0-4fc1-a1b1-05b527f41ca0.png)   
`/etc/fstab` 파일의 맨 아래에 내용을 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/140266243-a9c89be6-35bc-4ac3-b279-09a6d18e62ac.png)   
이제 클라이언트가 부팅될 때마다 공유 디렉토리에 자동으로 마운트 됩니다.

> <h3>윈도우 마운트 설정</h3>

[Windows 기능] > [Windows 기능 켜기/끄기]
![image](https://user-images.githubusercontent.com/43658658/140266610-c031ce45-6f86-4c31-ba43-0671a2ebac27.png)   
![image](https://user-images.githubusercontent.com/43658658/140266758-4b917f4c-4ad2-4b52-ac8b-b3c6d57c6e0e.png)   
`mount NFS서버IP주소:공유디렉토리 * `로 NFS 서버의 공유 디렉토리에 마운트합니다.   
![image](https://user-images.githubusercontent.com/43658658/140266907-c8577b21-7bad-415f-9330-00e73d936a30.png)   
파일 탐색기를 열어보면 Z 드라이브가 생성된 것을 볼 수 있습니다. NFS 서버의 공유 디렉토리와 같은 파일이 있음을 볼 수 있습니다.
