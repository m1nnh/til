## AWS 서버 구축

비트나미로 Local 서버를 구축했다. 이번엔 AWS로 서버를 구축할 것이다. `Linux` + `Nginx` + `PHP` + `MySQL`를 이용하여 서버를 구축한다.

### 서버 구축

우선 [AWS](https://aws.amazon.com/ko/)홈페이지를 가서 회원가입을 진행한다. 회원가입을 진행한 후에 EC2 서비스로 들어가 인스턴스를 생성해준다. 인스턴스는 1개만 무료이니 여러 개 만들지 않도록 주의한다. 만든 후에 탄력적 IP를 설정해주면 끝난다.

### 터미널로 서버 실행

<img width="926" alt="image" src="https://user-images.githubusercontent.com/78870076/116805360-080a4400-ab61-11eb-9f9d-13df1890ec20.png">

인스턴스에서 연결을 클릭 후 ssh -i 부분을 복사해둔다. 그리고 인스턴스를 만들었을 때 다운로드한 프라이빗 키 파일을 찾는다. 해당 파일이 있는 디렉토리로 이동후 아까 ssh -i 부분 복사해둔 것을 실행하고 아래와 같은 창이 뜨면 연동이 완료된 것이다.

<img width="673" alt="image" src="https://user-images.githubusercontent.com/78870076/116805381-3ab43c80-ab61-11eb-92c5-dbfcf3e709c9.png">

### Nginx + PHP + MySQL

서버가 연결이 되었으면 터미널 창을 이용해 `Nginx` / `PHP` / `MySQL` 을 다운 받는다.

```
$ sudo apt update

# install nginx
$ sudo apt install nginx

# install mysql
$ sudo apt install mysql –server

#install php
$ sudo apt install php-fpm php-mysql
```

## 외부에서 접속하기

### Nginx
다운을 완료하고 AWS 인바운드 설정을 해주어야 한다. 인바운드를 다음과 같이 설정한다.

<img width="319" alt="image" src="https://user-images.githubusercontent.com/78870076/116805428-80710500-ab61-11eb-84f4-6d8b703057ac.png"> 

인바운드 설정을 해주고 퍼블릭 주소를 검색창에 입력하고 아래와 같이 창이 뜨면 정상적으로 다운이 완료된 것이다.

<img width="528" alt="image" src="https://user-images.githubusercontent.com/78870076/116805458-b8784800-ab61-11eb-8cbe-b96337dda848.png">


### PHP

또한 PHP 연동도 확인하여야 하니 phpinfo.php 파일을 작성 후 확인해 준다.

<img width="380" alt="image" src="https://user-images.githubusercontent.com/78870076/116805501-f4aba880-ab61-11eb-96f8-36739110a0fc.png">

PHP도 다음과 같이 연동이 확인되면 정상적으로 연동이된 것이다.

### MySQL

MySQL은 `Datagrip`을 사용한다. 

<img width="402" alt="image" src="https://user-images.githubusercontent.com/78870076/116805535-20c72980-ab62-11eb-9e54-d16f6a4d91ba.png">

아까 전 인바운드 설정에서 3306 포트 번호를 열어 주었다. 또한 퍼블릭 주소를 복사 해와 Host에 입력을 하고 Test를 클릭하면 연결이 됐다고 뜰 것이다.