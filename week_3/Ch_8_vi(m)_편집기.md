# vim 편집기

1. Normal mode : 처음 실행한 상태
2. Insert mode : i, a, o, I, A, O를 누른 후 텍스트를 입력할 수 있는 상태
3. Command mode : Normal mode에서 `<Esc>`를 누르고 콜론(`:`)을 입력한 상태. 명령 모드에서는 방향키 위아래를 통해 이전 명령어를 불러올 수 있습니다.
4. Visual mode : 블록 선택을 위해서 `v` 또는 `<Ctrl+V>`키를 누른 상태

## 기본 편집 명령

> <h3>Undo와 Redo</h3>

* 되돌리기(Undo) : Normal mode에서 `u`
* 앞으로 돌리기(Redo) : Normal mode에서 `[Ctrl+R]`

> <h3>입력 모드</h3>

* i : 현재 커서에서 입력
* a : 현재 커서 다음부터 입력
* o : 현재 커서 다음 줄부터 입력

> <h3>커서 이동</h3>

* `상:k, 하:j, 좌:h, 우:l`
* `^` : 현재 행의 `처음`으로
* `$` : 현재 행의 `마지막`으로
* `gg` : 파일의 제일 `처음 행`으로
* `G` : 파일의 제일 `마지막 행`으로
* `b` : 단어 단위로 오른쪽으로 커서 이동
* `w` : 단어 단위로 왼쪽으로 커서 이동
* `[Ctrl+u]` : 한 페이지 위로(up)
* `[Ctrl+d]` : 한 페이지 아래로(down)
* `f문자` : 같은 행에서 특정 문자로 이동.

비주얼 모드(`v`)로 진입해 연계할 수도 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/140483604-b9b530b0-b41b-40f8-a1ae-2771bfd8bef5.png)

> <h3>삭제</h3>

* `d` : 삭제
* `dd` : 현재 커서의 행 삭제
* `dw` : 단어 삭제(d: delete, w:word)
* `3dw` : 단어 3회 삭제
* `d3w` : 단어 3개 삭제
* `df문자` : 같은 행에서 특정 `문자`가 있는 곳까지 삭제
* `x` : 커서 위치의 글자 삭제
* `.` : `가장 최근`에 수행했던 삭제 명령을 수행

비주얼 모드(`v`)로 진입해 연계할 수도 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/140484395-60f2520a-c52b-43b3-b802-feebde403fb4.png)   
![image](https://user-images.githubusercontent.com/43658658/140484433-0a71f9a8-437d-497b-9f83-f0c3449ffd25.png)   

> <h3>복사와 붙여넣기</h3>

* `yy` : 현재 커서의 행 전체 복사
* `yw` : 현재 커서의 단어 복사(y: copy, w:word)
* `2yy` : 현재 커서의 행부터 2줄 복사
* `y2w` : 현재 커서의 단어부터 2단어 복사
* `y0` : 커서 위치부터 줄의 처음까지 복사
* `y$` : 커서 위치부터 줄의 끝까지 복사
* `p` : 현재 커서의 다음 위치에 붙여넣기
* `P(대문자)` : 현재 커서의 위치에 붙여넣기

비주얼 모드(`v`)로 진입해 블록으로 선택한 다음 `y`를 눌러 복사하고 원하는 위치이 `p`를 눌러 붙여넣기 가능.   
![image](https://user-images.githubusercontent.com/43658658/140484090-5a8d3ce0-445c-4ef5-ad66-668aaf178076.png)   
![image](https://user-images.githubusercontent.com/43658658/140484180-71028a01-aa21-40a7-a29b-5778a7a46f01.png)   

> <h3>비주얼 모드에서 유용한 키</h3>

비주얼 모드에서 블록을 씌우고 해당 커멘드를 실행합니다.

* `~` : 대소문자 전환
* `y` : 복사
* `d` : 삭제

> <h3>반복 실행</h3>

`반복할 숫자`를 써주고 커맨드를 입력하게 되면, 그 커맨드를 `숫자`만큼 반복합니다.

![image](https://user-images.githubusercontent.com/43658658/140485590-f0f240f6-2709-4cec-b518-57632930b686.png)   
Normal Mode에서 `5ishell`을 입력하고 `esc`를 누르면 `shell` 단어가 5번 복사됩니다.

![image](https://user-images.githubusercontent.com/43658658/140485718-eb194d79-b6db-43cb-a849-2dcac3feb607.png)   
`5o` + `esc`는 5줄의 빈 줄을 추가해줍니다.

> <h3>저장/종료</h3>

`:`로 Command mode에 진입할 수 있습니다.   

* `:w test.txt` : test.txt 파일로 저장.
* `:w >> test.txt` : test.txt 파일에 덧붙여서 저장.
* `:e test.txt` : test.txt 파일을 불러오기.

> <h3>행 번호 표시</h3>

* `:set nu` : 행에 번호 표시

> <h3>매크로</h3>

`q` 명령으로 등록 시작 `q`로 등록 완료

1. `qa`: `a`라는 매크로 동작
2. `^`: 두 번째 라인의 시작으로 이동
3. `f<` : "<" 문자로 이동
4. `df>` : ">" 문자까지 삭제
5. `q` : 매크로 등록 마침

> <h3>덮어쓰기</h3>

* `r문자` : 현재 커서의 문자를 `r 뒤의 문자`로 덮어쓰기
* `R` : `esc`키를 누르기 전까지 입력하는 모든 문자를 덮어쓰기

> <h3>문자열 검색</h3>

* `/문자열` : 문자열을 검색합니다. `n`키를 눌러 다음 문자열로 넘어갈 수 있습니다.   

> <h3>문자열 치환</h3>

* `:%s/기존문자열/새문자열` : 기존 문자열을 새 문자열로 치환합니다. 한 라인에 바꿔야 할 문자열이 여러 개 있다면 `제일 처음 문자열`만 치환합니다.   
![image](https://user-images.githubusercontent.com/43658658/140490876-54d7d4ce-ff6a-4437-90ef-0c97cbf7b0fd.png)   
`:%s/Shell/aaaaa` 실행

* `:%s/기존문자열/새문자열/g` : 기존 문자열을 새 문자열로 치환합니다. 한 라인에 바꿔야 할 문자열이 여러 개 있다면 `모두` 치환합니다.   
![image](https://user-images.githubusercontent.com/43658658/140490940-58873a2a-25c5-4db6-bec9-211e81796f1d.png)   
`:%s/Shell/aaaaa/g` 실행

![image](https://user-images.githubusercontent.com/43658658/140491119-3bc6da21-781c-4c0d-b4ab-9c5d80c06b66.png)   
`:2, 6s/Shell/aaaaa` : 2행부터 6행까지 `Shell` 문자열을 `aaaaa` 문자열로 치환.

![image](https://user-images.githubusercontent.com/43658658/140491247-b2b4a67e-ee0d-4d5e-a21d-1e351d51ca4a.png)   
`:2, 6s/Shell/aaaaa/g` 실행

![image](https://user-images.githubusercontent.com/43658658/140491383-351384f7-ec2d-4ec9-9183-e50e39ddbca6.png)   
`:-1, +3s/Shell/aaaaa/g` : 초기 커서 위치로부터 위로 1행, 아래로 3행까지 `Shell` 문자열을 `aaaaa` 문자열로 전체 치환.

![image](https://user-images.githubusercontent.com/43658658/140491659-c2520e05-671e-4e35-b749-8d4d75353d0c.png)   
`:%s/Shell/aaaaa/gc` : 매칭되는 문자열을 하나씩 탐색하며 치환할 것인지 물어봅니다(`a`를 누를 경우 전체 문자열을 치환, `y`는 해당 문자열 치환, `n`는 치환하지 않음)

## 여러 개의 편집창 사용하기

`:vs [파일명]` : 현재의 편집창을 세로로 분리   
![image](https://user-images.githubusercontent.com/43658658/140492244-00a66a16-bc0f-4eba-b3f3-15fba9424226.png)   

`:sp [파일명]` : 현재의 편집창을 가로로 분리   
![image](https://user-images.githubusercontent.com/43658658/140492321-9b2e04c1-b461-43ef-9e57-eabe18de4f7d.png)

창 분리는 여러 번 할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/140492829-646d84e1-d2a1-4bd2-bf4d-bc9352f4c3d6.png)

`[Ctrl+w]`를 2회 입력해서 창을 이동할 수 있습니다.

각 창에서 종료(`:q`)를 하면 해당 창이 사라집니다.   
![image](https://user-images.githubusercontent.com/43658658/140492545-a00fe784-ee51-4b6e-b4f7-c82352b75636.png)   
현재 창에서 빨간 박스 창을 지웁니다.   
![image](https://user-images.githubusercontent.com/43658658/140492647-b67fdd54-c4ea-4550-8e94-eb1f6d915afd.png)
![image](https://user-images.githubusercontent.com/43658658/140492689-485d257b-f9e6-4676-85dd-bdbddc376e36.png)   

## vi 실습

`vimtutor` 명령어를 통해 나오는 튜토리얼 실습.

![image](https://user-images.githubusercontent.com/43658658/140638245-b25ae6da-54cb-407d-94d3-5726cf554df0.png)   
![image](https://user-images.githubusercontent.com/43658658/140638252-49fe916e-3632-4f7f-ba9a-c25dbbe1a7a5.png)   
![image](https://user-images.githubusercontent.com/43658658/140638263-c468e0f6-6735-4765-a91f-4b3ddea81daf.png)   
![image](https://user-images.githubusercontent.com/43658658/140638272-2c132d3c-7c54-4f25-8f9e-afa3675466ef.png)   
![image](https://user-images.githubusercontent.com/43658658/140638299-2f1329ff-b4e1-4f69-9567-d15fef990e64.png)   
![image](https://user-images.githubusercontent.com/43658658/140638309-6b6d3c60-5c7d-4b98-9881-9dec7017fca8.png)
   
![image](https://user-images.githubusercontent.com/43658658/140638314-fdb0b064-b0a7-4ad8-bf19-2252b1b6d1ea.png)   
![image](https://user-images.githubusercontent.com/43658658/140638320-cc015023-5388-4ea1-b2a4-c43302a8db00.png)   
![image](https://user-images.githubusercontent.com/43658658/140638349-e688fe8c-d9cc-4bf4-bf6a-62ec9cb600e4.png)   
![image](https://user-images.githubusercontent.com/43658658/140638383-4df833e8-312d-47a5-9c62-3a8fac99ec78.png)   
![image](https://user-images.githubusercontent.com/43658658/140638402-f948a8e8-0af4-478f-956f-e60946403a03.png)   
![image](https://user-images.githubusercontent.com/43658658/140638473-66e5cd1e-d141-4f12-9754-132e81e33e79.png)   

![image](https://user-images.githubusercontent.com/43658658/140639413-884eb581-f087-4249-ac78-6d2519c309c3.png)   
![image](https://user-images.githubusercontent.com/43658658/140639462-401555e6-fe11-4955-9eb3-8e21453240c7.png)   
![image](https://user-images.githubusercontent.com/43658658/140639531-47db0cce-9690-45c5-875e-632262d15a45.png)   
![image](https://user-images.githubusercontent.com/43658658/140639557-f95d3187-6f15-4cc8-b70c-46ef27025939.png)   

4.1 : 파일의 특정 행으로 이동   
![image](https://user-images.githubusercontent.com/43658658/140639646-cf73e6bc-8427-4aa1-a502-9d00c81a2bd9.png)   
4.2 : 찾기 명령   
![image](https://user-images.githubusercontent.com/43658658/140639707-14892f8b-cf16-4841-8b5e-10e9ce8bf28c.png)   
4.3 : 괄호 짝 찾기   
![image](https://user-images.githubusercontent.com/43658658/140639740-397c21cf-b66b-4b8d-8ef4-817046d2705a.png)   
4.4 : 문자열 치환   
![image](https://user-images.githubusercontent.com/43658658/140639865-d0a95811-9e72-41e7-b073-fb6ef83b6a53.png)   

5.1 : 외부 명령 실행법   
![image](https://user-images.githubusercontent.com/43658658/140639896-3adc602d-50f1-49bb-b5e5-87c797608167.png)   
5.2 : 다른 이름으로 저장   
![image](https://user-images.githubusercontent.com/43658658/140639956-f00546bc-b747-4cb1-9380-326aac9f0217.png)   
5.3 파일의 일부를 저장   
![image](https://user-images.githubusercontent.com/43658658/140640127-018c9e33-4a30-44aa-9c9c-3822c781e5c6.png)   
5.4 저장한 파일의 내용을 읽어들이기
![image](https://user-images.githubusercontent.com/43658658/140640212-35a86499-f930-4655-9535-19fa92dc0f72.png)   


