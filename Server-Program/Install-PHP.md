## PHP 8.0.3 소스(수동) 설치
현재 `Ubuntu 20.04.2` 위에 `Apache 2.4.46` 과 `MySQL 8.0.19`까지 설치했다. `PHP`는 버전 7.4.1 설치를 마지막으로 우분투 위에 설치했다.

[Apache 2.4.46 설치 방법](https://minhyeok-rithm.tistory.com/entry/Install-Apache?category=854409)

[MySQL 8.0.19 설치 방법](https://minhyeok-rithm.tistory.com/entry/Install-MySQL?category=854409)

### 의존성 패키지 설치

```
$ apt-get install libxml2-dev
$ apt-get install libjpeg-dev
$ apt-get install libpng-dev
```

### PHP 설치
`Apache`와 `MySQL` 설치할 때와 동일하게 `tar.gz` 파일을 다운 받아 압축 해제한다.

```
$ cd /usr/local/
$ wget https://www.php.net/distributions/php-7.4.1.tar.gz
$ tar xvfz php-7.4.1.tar.gz
```

#### configure & make

```
$ cd php-7.4.1 
$ ./configure \ 
--with-apxs2=/usr/local/apache2.4/bin/apxs \ 
--enable-mysqlnd \ 
--with-mysql-sock=mysqlnd \ 
--with-mysqli=mysqlnd \ 
--with-pdo-mysql=mysqlnd \ 
--with-imap-ssl \ 
--with-iconv \ 
--enable-gd \ 
--with-jpeg \ 
--with-libxml \ 
--with-openssl 

$ make && make test && make install
```
위에 ./configure 명령어를 입력하면 

![스크린샷 2021-04-12 오후 6 08 04](https://user-images.githubusercontent.com/78870076/114370393-64112680-9bba-11eb-8ae0-01a687717834.png)

다음과 같이 오류가 떠서

```
sudo apt install libsqlite3-dev
```

설치를 해주고 다시 실행하면

![스크린샷 2021-04-12 오후 6 09 01](https://user-images.githubusercontent.com/78870076/114370401-670c1700-9bba-11eb-9de5-944ce290f74d.png)

![스크린샷 2021-04-12 오후 6 12 05](https://user-images.githubusercontent.com/78870076/114379760-274a2d00-9bc4-11eb-9483-d732cd61a054.png)

### Apache와 PHP 연동
아파치 설정 파일인 `httpd.conf` 파일을 열어 PHP 모듈이 설치되어 있는지 확인한다.

```
$ cd /usr/local/apache2.4/conf
$ gedit httpd.conf
```

![스크린샷 2021-04-12 오후 7 22 48](https://user-images.githubusercontent.com/78870076/114379983-7db76b80-9bc4-11eb-8f34-2bb3175142b8.png)

모듈이 잘 설치되어 있다면 똑같은 `httpd.conf` 에서 `mime_module`에 아래 사진과 같이 작성한다.

![스크린샷 2021-04-12 오후 7 29 44](https://user-images.githubusercontent.com/78870076/114380946-9411f700-9bc5-11eb-88fb-47d50fa05b9f.png)

#### PHP.ini 파일 세팅
- php.ini : PHP 설정 파일
`php-7.4.1` 디렉토리로 가면 `php.ini-development`와 `php.ini-production` 두 개의 파일이 있다. 전자는 개발용, 후자는 프로덕션 시스템용 버전으로 개발용 같은 경우 더 많은 오류와 경고를 표시해주지만 보안상 문제가 생길 수 있으므로 개발 환경에서만 사용해야 한다. production 파일을 `/usr/local/lib` 디렉토리에 복사한다.

```
$ cd /usr/local/php-7.4.1 
$ cp php.ini-production /usr/local/lib/php.ini
```

#### 테스트용 php 파일 작성

```
$ cd /usr/local/apache2.4/htdocs $ vi phpinfo.php

 <?php phpinfo(); ?>
```

#### 연결 확인
아파치를 재실행 시킨 후 `http://127.0.1.1/phpinfo.php`로 접속하여 PHP 설치 정보가 출력되면 성공적으로 연동된 것이다. `ps -ef | grep httpd` 명령어로 아파치가 정상적으로 실행 중인지도 확인한다.

```
$ sudo /usr/local/apache2.4/bin/httpd -k start 
$ ps -ef | grep httpd | grep -v grep 
$ sudo netstat -anp | grep httpd 
$ sudo curl http://127.0.1.1
```

![스크린샷 2021-04-12 오후 6 24 13](https://user-images.githubusercontent.com/78870076/114381551-3631df00-9bc6-11eb-9578-052b2812903f.png)

위와 같이 창이 뜬다면 연결에 성공한 것이다.

## 설치를 마치며
그래도 블로그에 정리가 잘 되있는 글을 확인해서 할 수 있었다. 그래도 정리가 되있었어도 설치 과정에서 가상 머신이 다른점, 에러가 다르게 나타나는 점 등 시간이 많이 투자됐다. 그래도 다양한 명령어를 접할 수 있었고, 예전에 리눅스를 썼던 기억들을 다시 복기 할수 있었다.

## 📂 Reference

- [https://sangminlog.tistory.com/entry/how-to-install-php](https://sangminlog.tistory.com/entry/how-to-install-php)
