# Django
Python Web Framework 


## About Django
- 유지보수가 쉽고, 재사용 가능한 패턴을 사용함
- 데이터베이스 설치가 따로 필요하지 않음  
- 웹 사이트 개발시 고려해야 할 보안 설계가 많이 되어 있음     
- Python 기반으로 데이터분석/AI 활용에 유리



## MTV & MCV Pattern
MTV Pattern in Django       

### MTV Pattern 
- `Model`: **DB**
- `Template`: 사용자에게 보여지는 부분, **layout** 
- `View`: 데이터 **연산**/가공 처리 



## Install Django 
서버 접속(리눅스 **우분투** 계열 사용) 후,

1. python3 설치: `apt install python3`
2. pip3 설치: `apt install python3-pip`
3. Django 설치: `pip3 install Django`

#### ※ pip?
python 관련 패키지 설치/삭제/관리할 수 있는 프로그램



## Create Project in Django 
**1개의 Project**는 **N개 이상의 App으로 구성**되어 있음   
App은 각각의 독립된 기능을 수행할 수 있는 단위로 서로 독립적이어야 함

### Create Project
`django-admin startproject [프로젝트명]`     
- 프로젝트 폴더 내부로 진입하면, `프로젝트명과 동일한 폴더(App)`와 `manage.py 파일`이 존재
- `manage.py`: 프로젝트 관리 명령어 모음 파일 

### Create App
`django-admin startapp [App명]`      
- `settings.py`: 프로젝트 환경설정 파일로 [프로젝트명]과 동일한 [App명] 폴더 내부에 존재함! 

![image](https://user-images.githubusercontent.com/55572222/129747397-e2d193df-2e2d-48ab-bcaf-8d188497eafa.png)


## Django Server  
Django는 서버가 실행되는 동안에만 접속이 가능함 

1. 프로젝트 폴더로 이동 
2. manage.py 파일 실행: `python3 manage.py runserver 0.0.0.0:8000`
    manage.py파일을 실행하는데 누구나(0.0.0.0) 접속 가능하고, 8000번 포트로 열어놓음 
    ![image](https://user-images.githubusercontent.com/55572222/129747736-5563f180-b06d-4b0d-beef-8af70b03ebe9.png)
3. [NCP](https://www.ncloud.com/)에서 내 서버에 적용된 ACG 설정에서 지정한 포트번호(8000) 추가
    ![image](https://user-images.githubusercontent.com/55572222/129747526-d7ff7ce4-65ac-4b9b-a909-4f7517a9ac7b.png)
4. 브라우저에서 [내 서버의 IP주소]:8000 주소로 접속
    ![image](https://user-images.githubusercontent.com/55572222/129748970-4746926b-4815-4ecd-8954-ef3d33bcc796.png)    
    → 노란 배경 메세지: ALLOWED_HOSTS에 내 서버 IP 주소 추가해야 함 
5. setting.py 파일 열어서 AllOWED_HOSTS 안에 내 서버 IP 입력
    ![image](https://user-images.githubusercontent.com/55572222/129749553-7f171e32-0420-4d7e-bbd8-0700935ca1cd.png)
6. 프로젝트명 폴더로 이동후, manage.py 파일 실행: `python3 manage.py runserver 0.0.0.0:8000` 
    Django 서버 띄우기 성공!! 🙌🏻
    ![image](https://user-images.githubusercontent.com/55572222/129749420-784f2d48-94ef-4cdb-8acd-4f3de6b0d96c.png)

