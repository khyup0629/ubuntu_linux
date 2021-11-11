# sed 유틸리티

`sed`는 리눅스에서 텍스트 처리를 위한 유틸리티입니다.

`sed '텍스트편집' [파일명]` : 해당하는 파일에서 텍스트편집 형식에 맞게 텍스트를 편집해서 보여줍니다.     

![image](https://user-images.githubusercontent.com/43658658/141225571-3974ffcb-29d3-4fbb-877b-70ca2bc7a242.png)   
![image](https://user-images.githubusercontent.com/43658658/141225591-c74f4cfc-fe1b-4289-8cac-6879e59694ec.png)   
패턴을 입력할 때 정규표현식을 사용할 수 있습니다.

`sed` 명령어는 원본 파일의 내용에 영향을 주지 않습니다.

<sedtest.txt 파일 내용>   
![image](https://user-images.githubusercontent.com/43658658/141224141-5d6b24e5-1708-4a72-827e-23986a831a09.png)

![image](https://user-images.githubusercontent.com/43658658/141224208-2f39d3da-1867-49d8-a81d-df6b5f30df17.png)   
`linux`로 시작하는 라인을 출력   
![image](https://user-images.githubusercontent.com/43658658/141224296-e0b212ce-f5a7-4cd3-88cf-1f3724a3e062.png)   
파일의 4행을 삭제   
![image](https://user-images.githubusercontent.com/43658658/141224749-508f2453-f39a-4bea-a813-0cac827e36bb.png)   
각 라인에서 첫 번째 `linux` 문자를 `windows`로 치환   
맨 뒤에 `g`를 붙이면 전체 `linux` 문자를 `windows`로 치환   
![image](https://user-images.githubusercontent.com/43658658/141225031-1a08818c-4a00-4076-8ec8-6d288e8f43c9.png)   
각 라인의 끝에 연속된 빈 칸이 있다면 빈 칸을 삭제   
치환 형식을 응용해서 `s/pattern1/pattern2/` 중 패턴2에 null값을 주면서 삭제처럼 이용할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/141225133-f7db7ab3-c8ee-4249-b77a-e34257da24ee.png)   
`Linux`가 나오는 라인을 삭제   
![image](https://user-images.githubusercontent.com/43658658/141225346-91ca7c67-be5d-4bce-88b0-afcca35fd926.png)   
파일 내용 중 `linux` 단어를 모두 삭제합니다.   

<sedtest1.txt 파일 내용>   
![image](https://user-images.githubusercontent.com/43658658/141225715-da4f8c31-df7c-4279-b927-06c6836c64d1.png)    

![image](https://user-images.githubusercontent.com/43658658/141225991-84516576-f360-4ed6-93e8-dbb07f26328a.png)   
연속적인 모든 0을 하나의 0으로 치환   
![image](https://user-images.githubusercontent.com/43658658/141226046-21ad9dc5-ec74-4407-8958-88a74220b108.png)   
1행부터 첫 번째로 나오는 빈 줄까지 모두 삭제   
![image](https://user-images.githubusercontent.com/43658658/141226117-9f4316ce-b123-4776-99b6-7c96ea74dbac.png)   
파일 내용의 빈 줄을 모두 삭제합니다.   

