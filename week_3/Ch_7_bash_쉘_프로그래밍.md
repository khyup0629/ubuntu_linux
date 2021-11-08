앞서 우분투를 공부하면서 익혔던 내용은 스킵하였습니다.

---

# bash 쉘 프로그래밍

## 사용자 입력 읽기

> <h3>read 명령</h3>

![image](https://user-images.githubusercontent.com/43658658/140669934-f61db0df-f952-43bb-bb78-8e0927bdc918.png)   
![image](https://user-images.githubusercontent.com/43658658/140669919-37c85d1d-5249-4234-a432-2f0841a40cc2.png)   
* `echo -e` : 이스케이프 문자를 큰 따옴표(")로 묶어 인식하도록 하는 옵션.
* `echo -n` : 마지막에 개행을 하지 않는 옵션.
* `\c` : 개행을 하지 않는 이스케이프 문자.
* 명령어 read만 쓰면 입력 문자는 REPLY라는 `빌트인 변수`에 자동으로 들어갑니다.
* `read -p 문자` : 문자를 출력하면서 입력을 받습니다.
* `read -a 리스트` : 띄어쓰기를 기준으로 리스트로 입력 받습니다. "${리스트[2]}"의 형식으로 리스트를 불러옵니다.

## 산술 연산

> <h3>정수형 산술 연산</h3>

`declare -i 변수` : 변수를 정수형으로 선언합니다. `변수=숫자`를 통해서 변수 선언을 할 수 있습니다. 만약 문자가 들어오면 0으로 인식합니다.

![image](https://user-images.githubusercontent.com/43658658/140670285-e9d714ff-8346-452c-978b-520ef66071d7.png)   
* `num=hello` : 정수형 변수로 선언된 num 변수에 문자 hello가 들어왔으므로 0으로 인식합니다.

![image](https://user-images.githubusercontent.com/43658658/140670381-882e512b-0b71-4a31-9da2-db05ad7a9c3f.png)   
* `num=10+10` : 정수형으로 변수를 선언하고, 수식을 적으면 계산된 값이 변수로 들어갑니다. 이때 수식 사이에는 띄어쓰지 않습니다.

![image](https://user-images.githubusercontent.com/43658658/140670472-d9f1603d-ba84-4c3d-a18b-cac42a237f98.png)   
* 정수형으로 선언하고 실수를 넣으면 구문 에러가 발생합니다.

![image](https://user-images.githubusercontent.com/43658658/140670533-00eb25e3-4d52-440b-9287-f6907400487c.png)   
* 변수 없이 declare -i만 명령하면 정수형으로 선언된 변수들의 목록과 값을 볼 수 있습니다.

`let 수식(변수포함)` : 수식을 계산합니다.

![image](https://user-images.githubusercontent.com/43658658/140671122-7d8fe473-fa8c-46e5-9d1b-4166322b4807.png)   
* `((수식))` : `let 수식` 명령과 같이 인식합니다.

> <h3>진수 표기와 사용</h3>

![image](https://user-images.githubusercontent.com/43658658/140670718-0acca8ba-2a08-44f6-a281-95f64781f8e7.png)   
* `x=016` : 0으로 시작하는 정수가 들어오면 8진수로 인식합니다.
* `n#num` : num 숫자를 n진수로 인식합니다.

> <h3>실수형 산술 연산</h3>

![image](https://user-images.githubusercontent.com/43658658/140671874-f8fa3e26-6bcd-41b3-af47-16f38b5bd771.png)   
* echo와 bc 명령어를 파이프로 연결. bc 명령으로 `20 / 3`을 계산.
* scale=2 : 소숫점 아래 2자리.

## 위치 파라미터와 명령라인 아규먼트

> <h3>위치 파라미터</h3>

위치 파라미터가 10 이상이 되면 `${10}`과 같이 작성합니다.

![image](https://user-images.githubusercontent.com/43658658/140672528-88ce0156-0ee0-49c1-bf16-c6ef3965cf3c.png)   
![image](https://user-images.githubusercontent.com/43658658/140672657-b3edf7d9-0b24-49b0-85e5-09d1fec27360.png)   
* `$#` : 파라미터의 개수(1~)
* `$0` : 실행파일
* `$3` 파라미터를 입력하지 않으면, null로 인식합니다.

> <h3>set 명령</h3>

set : 위치 파라미터를 재설정 할 수 있습니다.

![image](https://user-images.githubusercontent.com/43658658/140673225-3f4344ce-042d-4647-956a-e5862c1d5080.png)   
![image](https://user-images.githubusercontent.com/43658658/140673185-27d6821f-97f0-419e-a999-5b3f1aa44550.png)   

![image](https://user-images.githubusercontent.com/43658658/140673430-f4888e5b-23b2-40bf-a2fe-266cbafa0534.png)   
![image](https://user-images.githubusercontent.com/43658658/140673490-2aee8f56-ba01-4b91-93d3-1ee13d9949a4.png)   
* `${1:?"메시지"}` : `파라미터1`을 입력하지 않으면 `메시지`를 출력하고, 입력되면 아래로 넘어갑니다.

> <h3>`$*`와 `$@`의 차이</h3>

![image](https://user-images.githubusercontent.com/43658658/140673695-399a89b3-458a-4198-8b06-c72bb2cd1e8f.png)   
![image](https://user-images.githubusercontent.com/43658658/140673873-d07446c3-c128-4eb7-aea1-94381d2ef220.png)   

파라미터들을 한 줄의 문자열로 나타내고 싶으면 `"$*"` 형식을 사용합니다.   
파라미터들의 따옴표 형식을 인식하기 위해서는 `"$@"` 형식을 사용합니다.

## 조건문

> <h3>종료 상태 변수</h3>

![image](https://user-images.githubusercontent.com/43658658/140674365-1c703f01-0493-47d1-9013-5e7656ea405f.png)   
* name이 bllu일 경우 `/etc/passwd` 파일에서 검색했을 때 목록이 있으므로 정상적 종료인 `0`이 출력
* name이 multi일 경우 `/etc/passwd` 파일에서 검색했을 때 목록이 없으므로 비정상적 종료인 `1`이 출력

> <h3>test 명령</h3>

![image](https://user-images.githubusercontent.com/43658658/140675244-2f50fbcd-4818-4ad4-88ae-9b7a86b1ae70.png)   
* `test 조건` : 조건이 참이면 `$?`가 0, 거짓이면 1이 출력된다.

![image](https://user-images.githubusercontent.com/43658658/140675497-6f5ef65b-1f6c-42e8-9241-7e69ddcf30e1.png)   
* `[ 조건 ]` : `test 조건`과 같습니다. 더블 브라켓(`[[ 조건 ]]`)을 사용해도 무방합니다.
* `[Mm]????` : test 명령은 와일드 카드를 허용하지 않습니다. ?는 상수 문자로 취급됩니다. 따라서 1이 출력됩니다.
* 싱글 브라켓 안에 조건을 넣을 땐 부등호가 아닌 문자(-gt, -eq 등)로 비교해야 합니다.
* 부등호를 쓰고 싶다면, let 명령(`(( 조건 ))`)으로 입력해야 합니다.

![image](https://user-images.githubusercontent.com/43658658/140676025-b2b7923e-6552-4386-879c-4a8014d10332.png)   
![image](https://user-images.githubusercontent.com/43658658/140678245-e4cb40f0-6368-4a72-a7f3-02410b3ef90b.png)   
* `[[ 조건 ]]` : 복합적인 패턴 매칭이 있다면 더블 브라켓으로 조건식을 묶습니다. 와일드 카드, 패턴 매칭을 사용 가능하도록 합니다.
* 파일 테스트(`-f 파일명, -x 파일명`) 조건에 사용합니다.

![image](https://user-images.githubusercontent.com/43658658/140676812-f8438d55-e9fa-4c90-a115-289c98f7f149.png)   
* `shopt -s extglob` : 좀 더 고차원적인 패턴 매칭을 가능하게 하는 명령어
* `[Gg]+(o)d` : 맨 앞에 G 또는 g가 오고 바로 뒤에 o 문자가 있는지 검사합니다(o가 여러 개여도 상관 없습니다). 마지막에는 d가 있는지 검사합니다.

> <h3>let 명령</h3>

let 명령은 이중 괄호(`(( 조건 ))`)와 같습니다.

![image](https://user-images.githubusercontent.com/43658658/140677214-bda39167-e094-4360-955b-f29ccb83162e.png)   
let 명령에서는 equal을 `==`로 표현 해야 합니다.   
부등호(>=, < 등), 기호(&&, || 등)를 사용하는 조건식에 사용합니다.

> <h3>if 문</h3>

![image](https://user-images.githubusercontent.com/43658658/140677845-9ba49a16-f8a7-4b10-961b-4c714a4e30ee.png)   
![image](https://user-images.githubusercontent.com/43658658/140677868-4d42cd97-6e5d-4961-91f1-c702e0548672.png)   
* `[Nn]o?(way|t really)` : 처음에 N 또는 n이 오고, 그 다음에 o가 오고, 그 다음 문자열이 way 또는 t really가 오는지를 검사합니다.

> <h3>exit 문과 ? 변수</h3>

![image](https://user-images.githubusercontent.com/43658658/140680805-fc411889-52b1-4eb0-95cc-ba817b566c98.png)   
![image](https://user-images.githubusercontent.com/43658658/140680597-6ad20bbf-d09c-404f-b333-25b44c87ae68.png)   
각 조건문에 걸리면 exit 1, 2, 3으로 종료되도록 했습니다. 그리고 `?` 변수에 exit 숫자가 대입됩니다.

> <h3>null 값 체크하기</h3>

![image](https://user-images.githubusercontent.com/43658658/140686655-6a849cc7-8601-49f4-9ef7-86a5d1c7d15b.png)   
![image](https://user-images.githubusercontent.com/43658658/140686845-3301fcfe-814c-423e-9c4e-7d18aa38faca.png)   
null 값은 빈 값("")을 의미하고, 문자열 패턴 매칭을 통해 체크합니다.

> <h3>ifelse 문</h3>

![image](https://user-images.githubusercontent.com/43658658/140687492-372c38f5-8edd-4fa6-91a9-dd01df5d2cc6.png)   
* `>& /dev/null` : grep 명령의 결괏값을 출력하지 않습니다.   
![image](https://user-images.githubusercontent.com/43658658/140687566-0695f44c-1e51-4863-bb5a-ea5000a21c65.png)   
* `>& /dev/null`가 없다면 아래와 같이 grep 명령의 결괏값이 출력되고 echo가 출력됩니다.   
![image](https://user-images.githubusercontent.com/43658658/140687659-a3318744-5b56-4377-b003-3cc7169dee39.png)   

![image](https://user-images.githubusercontent.com/43658658/140688397-2f8f7459-1600-489e-be7d-76ec2f020da7.png)
`expr 표현식` : 표현식이 올바른지 검사합니다.   
`$number + 0`의 식이 참이 되려면 number가 정수여야 합니다. 실수로 입력되면 계산을 할 수 없기 때문에 거짓이 됩니다.

> <h3>case 문</h3>

![image](https://user-images.githubusercontent.com/43658658/140689196-0ec2d6b6-0e1b-4a3b-ac24-5db01e682b4c.png)   
![image](https://user-images.githubusercontent.com/43658658/140689438-a19dfac7-2d36-4ad2-b543-41522062819e.png)   
* `cat <<- ENDIT` ~ `ENDIT` : ENDIT 사이의 내용을 출력합니다.

## 반복문

> <h3>백업 파일 만들기</h3>

백업 디렉토리(backup)를 만들고, if로 시작하는 파일들을 백업해봅시다.   
![image](https://user-images.githubusercontent.com/43658658/140690856-d4bdd002-90c6-4339-90c4-6e12cca696c3.png)   
![image](https://user-images.githubusercontent.com/43658658/140690805-4d5bd946-f3de-47a8-b792-35ca0a23960a.png)   

> <h3>실행 허가권 추가하기</h3>

![image](https://user-images.githubusercontent.com/43658658/140691606-f7331c75-7cdb-4c5c-856a-97757539a49a.png)
![image](https://user-images.githubusercontent.com/43658658/140691750-adfcee2e-09c9-4e5f-a2d6-85355a7afc81.png)   
단어 목록을 지정하지 않았지만, 파일이 실행되면 위치 파라미터를 자동으로 file에 할당합니다.   
파일이 일반 파일(-f)이고, 실행 허가권(-x)을 가지고 있지 않다(!)면, 실행 허가권을 줍니다.

> <h3>0~9까지 출력하기</h3>

![image](https://user-images.githubusercontent.com/43658658/140692799-522adada-72ec-4c1d-a3de-f6b6b456e04c.png)   
![image](https://user-images.githubusercontent.com/43658658/140692813-2cd1db6f-09d0-4e1d-bbce-6628a41d8060.png)   

> <h3>q 누를 때 까지 무한 루프</h3>

![image](https://user-images.githubusercontent.com/43658658/140693856-18d1c7ac-94f1-41c6-9776-fe15c5177c33.png)   
![image](https://user-images.githubusercontent.com/43658658/140694420-d7d1fc6f-07fd-45b1-a2c7-3aa62ab7bf12.png)   
* `[ -n 변수 ]` : 변수가 null이 아니면 참.
* `go=` : 임의로 go에 null값을 줍니다.

> <h3>시간 순서대로 나타내기</h3>

![image](https://user-images.githubusercontent.com/43658658/140694746-eba5f312-b47f-4b3f-8fef-05b2a685528a.png)    
![image](https://user-images.githubusercontent.com/43658658/140694777-0561e7cb-2afe-472b-b297-7415ad3539fb.png)    

> <h3>select 문</h3>

![image](https://user-images.githubusercontent.com/43658658/140695244-5fe02475-21c2-4b75-bcf4-2ac28b20974d.png)   
![image](https://user-images.githubusercontent.com/43658658/140695208-643efdd6-dfdf-4357-857d-50a1ad024db2.png)   

* select문은 PS3 프롬프트를 가장 먼저 작성합니다. 출력은 가장 아래에 출력됩니다.   
* 변수는 실행 가능한 명령문이나 파일 또는 문자열을 적습니다.   
* 입력받은 번호는 `$REPLY`에 들어가고, `$program`에는 명령문이 들어갑니다.   
* 별다른 종료를 취하지 않으면 무한 반복됩니다.

![image](https://user-images.githubusercontent.com/43658658/140696751-aa925917-3ff6-449f-a58d-7a519c0f1759.png)   
![image](https://user-images.githubusercontent.com/43658658/140696678-84006bcf-5d48-4f15-a79c-ea7107f1304e.png)   

> <h3>shift</h3>

![image](https://user-images.githubusercontent.com/43658658/140697279-8a609651-cf04-4496-bed1-d0740625e6d4.png)   
![image](https://user-images.githubusercontent.com/43658658/140697332-65d6d128-9b89-4faf-bf90-7c454b7bcebf.png)   
* shift 3 : 좌측으로 3번 파라미터 이동

![image](https://user-images.githubusercontent.com/43658658/140697416-c5d311e0-5d11-4c1c-abef-f4a9d8c8b2b5.png)   
![image](https://user-images.githubusercontent.com/43658658/140697466-9472de46-f90e-401e-8bc1-0f2ad5f691fa.png)   

> <h3>중첩 루프</h3>

continue n : n번째 루프의 시작으로 이동. 가장 안쪽 루프가 1, 그 다음 바깥쪽 루프가 2, ...

![image](https://user-images.githubusercontent.com/43658658/140698202-f56d715a-60eb-4926-b3c2-808826cb3a72.png)   
![image](https://user-images.githubusercontent.com/43658658/140698290-94a2c9c2-07a0-45c6-ac92-ab1ee003cfca.png)   


> <h3>루프의 결과를 파일로 리다이렉트하기</h3>

<file.txt> 파일 생성   
![image](https://user-images.githubusercontent.com/43658658/140699887-89160ff2-55bf-4734-8b04-326b36027f5c.png)   

![image](https://user-images.githubusercontent.com/43658658/140700137-dfcbd71b-c0da-45e6-ab3b-ab28d6da39cc.png)   
![image](https://user-images.githubusercontent.com/43658658/140700379-d58e5f62-8780-4851-93ca-02fd9e9015be.png)   
* `cat $1 | while read line` : file.txt 파일에서 내용을 한 라인씩 `line` 변수로 읽어 들입니다.
* `(( count == 1 )) && echo "Processing file $1" > /dev/tty` : count가 1이면, 현재 모니터(/dev/tty)로 "Processing file $1" 메시지가 보이도록 합니다.
* `echo -e "$count\t$line"` : count와 line에 대한 내용을 `tmp$$`로 리다이렉션해서 저장합니다.
* `mv tmp$$ $2` : `tmp$$` 파일명을 두 번째 파라미터(file2.txt)로 바꿉니다.

> <h3>루프의 결과를 명령어와 파이프하기</h3>

![image](https://user-images.githubusercontent.com/43658658/140701143-f4f6d021-8002-4487-b50c-a6588e61a93e.png)   
![image](https://user-images.githubusercontent.com/43658658/140701247-3fed2b44-5c3d-4829-b317-feda3c6877a8.png)   

> <h3>IFS와 루프</h3>

`IFS` : 변수로 단어를 분리하는 기준이 되는 문자(공백, 탭, newline 문자 등)가 적혀 있습니다. 기본값은 공백입니다. IFS를 기준으로 단어를 구분합니다.

![image](https://user-images.githubusercontent.com/43658658/140702548-b72a61fa-a681-4ab5-9358-9b152c8d6475.png)   
![image](https://user-images.githubusercontent.com/43658658/140702629-8e32e47e-a603-4098-87cf-cdb7b003e560.png)   
* `oldifs=$IFS; IFS=":"` : IFS의 초기값을 잠시 oldifs에 저장해놓고, 단어를 구분하는 새로운 기준을 `:`로 설정합니다.
* names의 변수를 `:`를 기준으로 단어를 구분합니다.
* `IFS=$oldifs` : IFS에 다시 초기값(공백)을 넣습니다.
* `a b c d`를 공백을 기준으로 4개의 단어로 구분합니다.

## 함수

> <h3>함수 선언</h3>

```
function 함수명 { 명령; 명령 }
```

![image](https://user-images.githubusercontent.com/43658658/140703561-3c3f4102-07d2-4166-ad92-758e75dc7937.png)

> <h3>함수 설정 제거</h3>

```
unset -f 함수명
```

![image](https://user-images.githubusercontent.com/43658658/140703714-b2ce7069-c83f-4bd0-845d-63756d0a220f.png)

> <h3>실행 파일의 위치 파라미터를 함수에서 사용하기</h3>

![image](https://user-images.githubusercontent.com/43658658/140705298-a0923658-1fd5-458b-b7f1-85dfa11f2349.png)   
![image](https://user-images.githubusercontent.com/43658658/140706217-61c47d63-3d96-4420-84af-9b8e1520586a.png)   

> <h3>지역 변수와 리턴</h3>

지역 변수 : 함수 내에서 선언되는 변수로 local과 함께 변수를 선언합니다. 함수가 끝나면 메모리에서 사라집니다.

![image](https://user-images.githubusercontent.com/43658658/140706808-f1823f98-0995-4858-b2c0-9578ef9d047f.png)   
![image](https://user-images.githubusercontent.com/43658658/140706919-5ae45717-44da-4456-994a-b629c76baa21.png)   
* 함수의 리턴값은 `?`로 들어가기 때문에 `$?`를 호출해야 리턴값을 볼 수 있습니다.
* `sum`은 지역 변수로 선언 해주었기 때문에 함수 바깥에서는 메모리에 없습니다. 따라서, `$sum`를 해도 빈 값이 출력됩니다.

![image](https://user-images.githubusercontent.com/43658658/140707692-6a185392-354d-4a2d-baa9-d72a7eef24ce.png)   
![image](https://user-images.githubusercontent.com/43658658/140707832-fefda5df-a445-471d-9610-524c06e9622a.png)   

> <h3>source 명령</h3>

<myfunction 파일 내용>   
![image](https://user-images.githubusercontent.com/43658658/140708909-2cebd6a9-5026-4dfd-adbf-a723f504ac3a.png)   

![image](https://user-images.githubusercontent.com/43658658/140708857-29dac465-d4b7-4284-ad33-7a878e1d6ae4.png)
`source 파일명`을 통해 파일 내의 함수를 읽어 들여 다른 함수에서 사용할 수 있도록 합니다.
`$(함수)`를 통해 함수의 결괏값을 이용할 수 있습니다.

source 대신 `.`을 사용할 수도 있습니다.

## 트래핑 시그널

프로그램이 실행되는 동안 `[Ctrl+C]` 또는 `[Ctrl+\]`를 누르면 프로그램이 종료됩니다. trap 명령은 시그널이 도착했을 때 프로그램이 어떤 반응을 할지 관리하도록 하는 명령어입니다.

`trap -l` : 모든 시그널의 목록을 볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/140710754-0690300b-a8ce-4f5c-9a7f-2bb3ec3102af.png)

```
trap '명령; 명령' 시그널번호
trap '명령; 명령' 시그널이름

trap 'rm tmp*; exit 1' 1 2 15
trap 'rm tmp*; exit 1' HUP INT TERM
```

1(HUP), 2(INT), 15(TERM) 시그널이 도착했을 때 tmp로 시작하는 모든 파일을 삭제하고 종료하도록 한 것입니다.

> <h3>시그널 재설정</h3>

시그널을 디폴트로 설정하기 위해서는 `trap 시그널번호/시그널이름`을 입력합니다.

`trap 'trap 2' 2` : 프로그램을 종료하기 위해서 `[Ctrl+C]`를 두 번 눌러야 합니다.

> <h3>시그널 무시</h3>

`trap " " 1 2` 또는 `trap " " HUP INT` : 시그널 1과 2는 프로세스에 의해 무시됩니다.

> <h3>trap 목록</h3>

![image](https://user-images.githubusercontent.com/43658658/140713204-b809c5d9-8969-46c5-900f-4af7d25ceda9.png)   
* `trap`만 입력하면 모든 trap 목록을 볼 수 있습니다.
* `trap '명령' 2` : `[Ctrl+C]`키를 누르면 해당 명령이 실행되도록 합니다.

![image](https://user-images.githubusercontent.com/43658658/140714596-375b00cb-1aaf-48ae-9884-1f24da09959d.png)   
![image](https://user-images.githubusercontent.com/43658658/140714562-7becb90a-9fae-462b-8344-0d96a670e97e.png)   
* trap을 통해서 `[Ctrl+C]`, `[Ctrl+\]`, `[Ctrl+Z]`로 종료하는 것을 막았습니다.
* 오직 stop을 입력해서 종료할 수 있도록 했습니다.

> <h3>trap과 함수</h3>

trap을 사용한 함수가 한 번 호출되고 나면 스크립트 전체에 영향을 미칩니다.   
![image](https://user-images.githubusercontent.com/43658658/140717308-0f4c5ce7-d188-4707-ae23-1b023a3221a8.png)   
![image](https://user-images.githubusercontent.com/43658658/140717704-7b112779-4719-47d0-ba83-f507240a994b.png)   
![image](https://user-images.githubusercontent.com/43658658/140718026-6fdf11bc-8448-45b2-8f68-a9c4a211ff53.png)   
`[Ctrl+C]`는 막아 놓았기 때문에, `[Ctrl+Z]`로 백그라운드로 실행되도록 한 다음 `kill -9 프로세스ID` 명령으로 종료해야 합니다.

## 스크립트 디버깅

> <h3>스크립트 디버깅</h3>

![image](https://user-images.githubusercontent.com/43658658/140719094-00bc4b1a-7e8a-4ae4-95da-5761102a69c0.png)   
![image](https://user-images.githubusercontent.com/43658658/140719155-dada13f5-edb6-4589-af08-be8ddfc08d64.png)   
* `-x` : 변수 치환 > 스크립트를 따라가며 에코(+) 출력 > 실행

![image](https://user-images.githubusercontent.com/43658658/140719560-d0d9f787-13c6-486a-8bf9-7633fc65b9e9.png)   
* `-v` : 스크립트의 각 라인을 출력 > 실행

![image](https://user-images.githubusercontent.com/43658658/140720184-9b53cfe5-d1c2-4ef5-ad2a-3cf441bdc854.png)   
* `-n` : 해석만 하고 실행은 하지 않습니다.

## 명령라인

> <h3>getopts</h3>

![image](https://user-images.githubusercontent.com/43658658/140720715-4b1faeb8-869b-4020-b9b4-017034bca9f9.png)   
![image](https://user-images.githubusercontent.com/43658658/140720816-c65c93e2-9ad0-4623-bd20-7eb6a3a3074b.png)   
* `getopts xy options` : getopts 다음에 x와 y 옵션이 있는지 체크하기 위해 `xy`가 들어갔고, 옵션들은 `options`에 할당됩니다.

> <h3>OPTARG, OPTIND</h3>

* `OPTARG` : argument가 할당됩니다.
* `OPTIND` : argument의 수(스크립트명도 포함)를 나타냅니다.

![image](https://user-images.githubusercontent.com/43658658/140722134-97eced0a-b95b-425a-8900-5000301eebda.png)   
![image](https://user-images.githubusercontent.com/43658658/140727573-929ac61c-6e09-4985-91ce-526213fd3425.png)   
* `ab:` : a와 b의 옵션 중 b 옵션은 argument가 필요하도록 설정합니다.

![image](https://user-images.githubusercontent.com/43658658/140728148-fc76974f-0cd2-4d6e-9971-64aaab21da4a.png)
![image](https://user-images.githubusercontent.com/43658658/140728450-e68654bd-ad65-4697-bd0d-1c7a607765ee.png)   

## Text GUI

dialog 패키지를 이용해 TUI를 구현해봅시다.

> <h3>메시지 박스</h3>

![image](https://user-images.githubusercontent.com/43658658/140734631-422a8af6-a348-45fe-89a6-440355fcbbd7.png)   
![image](https://user-images.githubusercontent.com/43658658/140734651-d1486f7d-c884-4f23-bc19-27140ecd1ba5.png)   

> <h3>yes/no 박스</h3>

![image](https://user-images.githubusercontent.com/43658658/140735116-2cae3b5f-f20c-4090-a80d-a0cf697343a2.png)   
![image](https://user-images.githubusercontent.com/43658658/140735256-fae40dc4-9ba8-4dbd-a5fd-9a4bf7850890.png)   
* `sel=$?` : yesno 박스에서 yes를 선택하면 0, no를 선택하면 1, esc 키를 누르면 255가 할당됩니다.

> <h3>입력 박스</h3>

![image](https://user-images.githubusercontent.com/43658658/140736943-ad6c9c7a-2f79-47a3-9494-68f48583ab2e.png)   
![image](https://user-images.githubusercontent.com/43658658/140736875-5e5f5c50-c1c8-40b5-8279-82cb5e97761a.png)   
* `2>/tmp/input.$$` : 입력값을 `/tmp/input.$$` 파일에 임시로 저장합니다.
* 마지막에 `/tmp/input.$$` 파일을 지웁니다.

> <h3>라디오 리스트</h3>

![image](https://user-images.githubusercontent.com/43658658/140738113-7c6517e6-004b-46c3-b03f-47dfcd3affb9.png)   
![image](https://user-images.githubusercontent.com/43658658/140738602-52bb0187-9e0c-460f-82ed-10b6290f5d23.png)   

