# awk 프로그래밍

## awk 란?

데이터를 조작하고 리포트를 생성하기 위해 사용하는 언어입니다.

```
awk 'pattern' filename
awk '{action}' filename
awk 'pattern {action}' filename
```

<awkfile 파일 내용>   
![image](https://user-images.githubusercontent.com/43658658/141226946-8b76825a-0312-4111-aa62-52f13aedf28a.png)   

![image](https://user-images.githubusercontent.com/43658658/141227155-7481869c-f86c-4fa8-807e-5a9b75eaf45d.png)   
`Gildong`을 포함하고 있는 라인 출력   
![image](https://user-images.githubusercontent.com/43658658/141227188-f59ec542-c2f2-4f35-8fad-259a620166c9.png)   
공백 또는 탭을 기준으로 필드를 나누었을 때 왼쪽에서 첫 번째로 나오는 필드(`$1`)를 출력합니다.   
![image](https://user-images.githubusercontent.com/43658658/141227265-597016bc-647c-4cfc-9535-b4b4e1083712.png)   
`Hong`이 들어간 라인의 1, 2번째 레코드를 출력합니다.   

![image](https://user-images.githubusercontent.com/43658658/141227656-80e7bf70-ab96-48eb-9ea2-ec2fca547811.png)   
`df` 명령으로 나오는 테이블에서 네 번째 필드의 레코드가 1000000을 초과하는 라인 출력

> <h3>print 함수</h3>

print 함수는 `{print}` 형식으로 awk의 액션 부분에 사용될 수 있습니다.   
문자열은 큰 따옴표로 감싸주고, 아규먼트들 사이에는 콤마로 구분지어 줍니다.   
콤마를 사용하지 않으면 아규먼트들은 서로 연결되어버립니다.   

![image](https://user-images.githubusercontent.com/43658658/141228763-2501b19a-e2d1-47be-8c22-d257d498447e.png)
필드 변수가 연결되어 있으면 콤마를 사용해서 구분지어 주고, 문자열과 변수가 연결되어 있으면 띄어쓰기를 기준으로 구분하면 됩니다.   

![image](https://user-images.githubusercontent.com/43658658/141228936-60f90ec4-5db2-44f3-8f4b-142308178dc2.png)   
![image](https://user-images.githubusercontent.com/43658658/141228921-40245da4-f2d3-4752-ada9-7829a11df534.png)   
`Jung`과 매칭되는 라인 중 1, 2번째 필드값을 이용해 print합니다.   

> <h3>OFMT 변수</h3>

숫자를 출력할 때 숫자의 포맷을 제어하기 위한 역할을 합니다.

![image](https://user-images.githubusercontent.com/43658658/141229454-6c984451-b36e-4b85-8a43-5bc0cabf9df0.png)   
`%.2f` : 소수점 아래 둘째 자리까지 나타내고 그 뒤는 버림   
또한 큰 수는 지수 형태(e)로 나타낼 수 있습니다.   

> <h3>printf 함수</h3>

print 함수와 다르게 printf 함수는 포매팅된 깔끔한 출력을 지원합니다.   
