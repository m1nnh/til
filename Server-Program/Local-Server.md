## Local 서버 구축

저번 주에 컴파일을 통한 APM 설치를 진행했다. 이번 주차는 컴파일 설치가 아닌 `Bitnami`를 이용한 패키징 설치를 진행하려고 한다. `Bitnami MAMP`를 이용하여 맥 환경에 `Apache` / `MySQL` / `PHP`를 한 번에 설치해준다.

### 설치 및 실행

[비트나미 사이트](https://bitnami.com)에서 설치 후 `localhost:8080` 혹은 `127.0.0.1:8080`로 접속하여 아래와 같이 화면이 뜨면 정상적으로 실행된 것이다.

<img width="431" alt="image" src="https://user-images.githubusercontent.com/78870076/116804796-ae077f80-ab5c-11eb-8dbf-879525d0eac2.png">


### phpinfo 띄우기

설치한 MAMP 폴더에 Apache2 -> htdocs 폴더에 phpinfo.php 파일을 다음과 같이 작성한다.

```
<?php
phpinfo();
?>
```

그러고 나서 `localhost:8080/phpinfo.php` 혹은 `127.0.0.1:8080/phpnifo.php`를 검색하면 다음과 같이 실행되면 정상적으로 실행이 완료된 것이다.

<img width="438" alt="image" src="https://user-images.githubusercontent.com/78870076/116804856-30903f00-ab5d-11eb-911d-5bcb9aba2a35.png">

### 포트포워딩

지금까지는 단순히 로컬 서버를 구축한 것이다. 외부에서 접속하기 위해 포트포워딩이 필요하다. [포트포워딩](https://minhyeok-rithm.tistory.com/entry/20210418-Network?category=854208)에 대한 내용은 전에 내용 정리하였다.

공유기 주소를 알아내고 주소를 검색하면 포트포워딩 설정을 할 수 있다. 그리고 나서 포트포워딩 설정을 다음과 같이 설정해주면 된다.

<img width="594" alt="image" src="https://user-images.githubusercontent.com/78870076/116804958-e3f93380-ab5d-11eb-9e4f-4168a7f23d9d.png">

- 외부 포트 : 외부에서 어떤 포트로 접속하였을 때 지정된 컴퓨터로 연결할 것인지 설정
- 내부 포트 : 외부 포트에서 연결해주었을 때, 내부에서는 어떤 포트를 사용할 것인지 설정

포트포워딩 설정이 완료되면 `http://[외부 IP]:[포트 번호]`로 로컬 서버에 접근할 수 있다. 네이버에 [내 IP 주소](https://search.naver.com/search.naver?where=nexearch&sm=top_hty&fbm=0&ie=utf8&query=내+ip) 검색하면 외부 IP가 나온다.

### 접속 확인

이로써 외부 클라이언트에서 로컬 서버로의 접근이 가능하다. 단 같은 인터넷을 사용하고 있는 것이 아닌 외부 기기로 연결을 해야한다.

<img width="409" alt="image" src="https://user-images.githubusercontent.com/78870076/116805018-5ff37b80-ab5e-11eb-87a2-535fd7c3f5e7.png">