# 우분투(20.04 LTS) 리눅스 학습 자료

우분투 리눅스 학습 / 「이것이 우분투 리눅스다 (개정판)」 참조

목차
- [Ch 1. 실습 환경 구축](https://github.com/khyup0629/ubuntu_linux/blob/main/week_1/Ch_1_%EC%8B%A4%EC%8A%B5_%ED%99%98%EA%B2%BD_%EA%B5%AC%EC%B6%95.md#ch-1-%EC%8B%A4%EC%8A%B5-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95)   
- [Ch 2. 우분투 리눅스 소개](https://github.com/khyup0629/ubuntu_linux/blob/main/week_1/Ch_2_%EC%9A%B0%EB%B6%84%ED%88%AC_%EB%A6%AC%EB%88%85%EC%8A%A4_%EC%86%8C%EA%B0%9C.md#ch-2-%EC%9A%B0%EB%B6%84%ED%88%AC-%EB%A6%AC%EB%88%85%EC%8A%A4-%EC%86%8C%EA%B0%9C)   
- [Ch 3. 우분투 리눅스 설치](https://github.com/khyup0629/ubuntu_linux/blob/main/week_1/Ch_3_%EC%9A%B0%EB%B6%84%ED%88%AC_%EB%A6%AC%EB%88%85%EC%8A%A4_%EC%84%A4%EC%B9%98.md#ch-3-%EC%9A%B0%EB%B6%84%ED%88%AC-%EB%A6%AC%EB%88%85%EC%8A%A4-%EC%84%A4%EC%B9%98)   
- [Ch 4. 서버 구축시 알아야 할 필수 개념과 명령어](https://github.com/khyup0629/ubuntu_linux/blob/main/week_1/Ch_4_%EC%84%9C%EB%B2%84%EA%B5%AC%EC%B6%95%EC%8B%9C%EC%95%8C%EC%95%84%EC%95%BC%ED%95%A0%ED%95%84%EC%88%98%EA%B0%9C%EB%85%90%EA%B3%BC%EB%AA%85%EB%A0%B9%EC%96%B4.md#ch-4-%EC%84%9C%EB%B2%84-%EA%B5%AC%EC%B6%95%EC%8B%9C-%EC%95%8C%EC%95%84%EC%95%BC-%ED%95%A0-%ED%95%84%EC%88%98-%EA%B0%9C%EB%85%90%EA%B3%BC-%EB%AA%85%EB%A0%B9%EC%96%B4)   
- [Ch 7. 쉘 스크립트 프로그래밍](https://github.com/khyup0629/ubuntu_linux/blob/main/week_2/%EC%85%B8_%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D.md#%EC%85%B8-%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)   
- [Ch 8. 원격지 시스템 관리](https://github.com/khyup0629/ubuntu_linux/blob/main/week_2/%EC%9B%90%EA%B2%A9%EC%A7%80_%EC%8B%9C%EC%8A%A4%ED%85%9C_%EA%B4%80%EB%A6%AC.md#%EC%9B%90%EA%B2%A9%EC%A7%80-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B4%80%EB%A6%AC)   
- [Ch 9. 네임 서버 설치와 운영](https://github.com/khyup0629/ubuntu_linux/blob/main/week_2/%EB%84%A4%EC%9E%84_%EC%84%9C%EB%B2%84_%EC%84%A4%EC%B9%98%EC%99%80_%EC%9A%B4%EC%98%81.md#%EB%84%A4%EC%9E%84-%EC%84%9C%EB%B2%84-%EC%84%A4%EC%B9%98%EC%99%80-%EC%9A%B4%EC%98%81)   
- [Ch 10. 메일 서버 설치와 운영](https://github.com/khyup0629/ubuntu_linux/blob/main/week_2/%EB%A9%94%EC%9D%BC_%EC%84%9C%EB%B2%84_%EC%84%A4%EC%B9%98%EC%99%80_%EC%9A%B4%EC%98%81.md#%EB%A9%94%EC%9D%BC-%EC%84%9C%EB%B2%84-%EC%84%A4%EC%B9%98%EC%99%80-%EC%9A%B4%EC%98%81)   
- [Ch 11. 데이터베이스 서버 설치와 운영](https://github.com/khyup0629/ubuntu_linux/blob/main/week_2/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4_%EC%84%9C%EB%B2%84_%EA%B5%AC%EC%B6%95%EA%B3%BC_%EC%9A%B4%EC%98%81.md#%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%84%9C%EB%B2%84-%EA%B5%AC%EC%B6%95%EA%B3%BC-%EC%9A%B4%EC%98%81)   
- [Ch 12. 웹 서버 설치와 운영](https://github.com/khyup0629/ubuntu_linux/blob/main/week_2/%EC%9B%B9_%EC%84%9C%EB%B2%84_%EC%84%A4%EC%B9%98%EC%99%80_%EC%9A%B4%EC%98%81.md#%EC%9B%B9-%EC%84%9C%EB%B2%84-%EC%84%A4%EC%B9%98%EC%99%80-%EC%9A%B4%EC%98%81)   
- [Ch 14. NFS 서버 설치와 운영](https://github.com/khyup0629/ubuntu_linux/blob/main/week_2/NFS_%EC%84%9C%EB%B2%84_%EC%84%A4%EC%B9%98%EC%99%80_%EC%9A%B4%EC%98%81.md#nfs-%EC%84%9C%EB%B2%84-%EC%84%A4%EC%B9%98%EC%99%80-%EC%9A%B4%EC%98%81)   
- [Ch 15. Samba 서버 설치와 운영](https://github.com/khyup0629/ubuntu_linux/blob/main/week_2/Samba_%EC%84%9C%EB%B2%84_%EC%84%A4%EC%B9%98%EC%99%80_%EC%9A%B4%EC%98%81.md#samba-%EC%84%9C%EB%B2%84-%EC%84%A4%EC%B9%98%EC%99%80-%EC%9A%B4%EC%98%81)   

