# vim 편집기

1. Normal mode : 처음 실행한 상태
2. Insert mode : i, a, o, I, A, O를 누른 후 텍스트를 입력할 수 있는 상태
3. Command mode : Normal mode에서 `<Esc>`를 누르고 콜론(`:`)을 입력한 상태
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

* `:%s/기존문자열/새문자열` : 기존 문자열을 새 문자열로 치환합니다.

![image](https://user-images.githubusercontent.com/43658658/140489932-04903103-d75b-4fce-8231-b4862cbad100.png)   
59행부터 63행까지 `bbbbb` 문자열을 `aaaaa` 문자열로 치환.

