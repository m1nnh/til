## 컴파일 설치하는 이유
소프트 스퀘어드에서 내준 첫 과제는 `Linux`에 `Apache` / `PHP` / `MySQL` 세 가지를 설치하여 서버를 구축하는 것이다. 대신 Package Management를 이용하지 않고 소스 설치를 요구했다. 그렇다면 왜 수동 설치를 해야 하는지 알아보자.
`Ubuntu`를 예로 들면 `apt` 혹은 `apt-get`이라는 Package Management가 있다. 이것을 통해 프로그램을 설치하게 되면 OS 종류나 버전에 맞춰 의존성 있는 프로그램들을 설치해준다. 개인적인 목적으로 설치하는 프로그램인 경우에는 패키지 매니지먼트를 이용해 쉽게 설치하면 되나, **회사에서 업무적인 목적으로 설치하는 경우** 이것을 막을 수 도 있다. 그래서 수동 설치를 해야한다.

## Parallels + Ubuntu 20.04.2 설치
노트북을 Window를 쓰다가 최근에 M1 Chip이 내장된 맥북을 구매했다. 이 과제를 수행하기 전에 Virtual Box를 설치해보려 했지만, Virtual Box가 해당 제품을 지원하지 않는다는 글을 보았고, 다른 것을 찾아보다가 맥에서는 Vmware를 더 많이 사용한다는 글을 보았었고, 해당 제품을 사용하려고 찾아 보았다. 하지만 최근에 나온 제품들은 전부 유료였고 가장 최신 버전의 Fusion 12.0 제품이 무료로 나와 있길래 설치하려 했다.

![스크린샷 2021-04-12 오후 2 54 23](https://user-images.githubusercontent.com/78870076/114346835-058a7f00-9b9f-11eb-9c69-3122def95e19.png)

하지만 이것 또한 Intel 기반의 맥용만이었다. 그래서 찾아 보다가 Parallels가 지원한다는 글을 보았고 설치하게 되었다. 

Parallels 설치 : [https://cpuu.postype.com/post/9184363](https://cpuu.postype.com/post/9184363)

또한 위에 블로그에 설치 방법에 보면 알겠지만 Window용 우분투 설치 파일은 amd이고, 맥용은 arm을 이용한다.

![스크린샷 2021-04-12 오전 12 05 26](https://user-images.githubusercontent.com/78870076/114347087-67e37f80-9b9f-11eb-8406-e19da1636fbf.png)

## Apache 2.4.46 소스(수동) 설치

### 컴파일 설치 관례
- /usr/local에 설치한다.
- 소스파일은 /usr/local/src에 보관한다.

```
$ ./configure
$ make && make install
```

### 의존성 패키지(APR, PCRE) 설치

- APR(Apache Portalbe Runtime) : Apache 웹 서버를 지원하는 라이브러리이다.
- PCRE(Perl Compatible Regular Expressions) : 펄 호환 정규 표현식은 펄 프로그래밍 언어의 정규 표현식 기능에 착안하여 만든, 정규 표현식 C 라이브러리이다.

```
$ sudo su
$ cd /usr/local
$ mkdir apache
```

#### /usr/local에 apr과 apr-util 설치
- wget : 웹 서버로부터 파일을 다운받는다.
- tar xvfz : `tar.gz` 압축을 한 번에 풀어준다.
- make : 소스를 컴파일한다.
- make install : make를 통해 만들어진 설치 파일을 설치하는 과정이다.

```
# 필수 설치
$ apt-get install gcc zlibc zlib1g zlib1g-dev libssl-dev openssl \
libxml12-dev ncurses-dev libexpat1-dev

$ wget http://mirror.navercorp.com/apache//apr/apr-1.7.0.tar.gz
$ wget http://mirror.navercorp.com/apache//apr/apr-util-1.6.1.tar.gz
$ tar xvfz apr-1.7.0.tar.gz
$ tar xvfz apr-util-1.6.1.tar.gz

$ cd /usr/local/apr-1.7.0
$ ./configure --prefix=/usr/local/apr

$ make && make install

$ cd usr/local/apr-util-1.6.1
$ ./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr

$ make && make install
```

#### /usr/local에 pcre 설치

```
$ cd /usr/local
$ wget ftp://ftp.pcre.org/pub/pcre/pcre-8.43.tar.gz
$ tar xvfz pcre-8.43.tar.gz

$ cd /usr/local/pcre-8.43
$ ./configure --prefix=/usr/local/pcre

$ make && make install
```

### Apache 2.4.46 설치

```
$ cd /usr/local 
$ wget http://apache.tt.co.kr//httpd/http-2.4.46.tar.gz 
$ tar xvfz httpd-2.4.46.tar.gz 

$ cd httpd-2.4.46 
$ ./configure --prefix=/usr/local/apache2.4 \ --enable-module=so --enable-rewrite --enable-so \ --with-apr=/usr/local/apr \ --with-apr-util=/usr/loacl/apr-util \ --with-pcre=/usr/local/pcre \ --enable-mods-shared=all $ make 
&& make install
```

### Apache 실행
- 실행 : httpd -k start
- 종료 : httpd -k stop

```
$ sudo /usr/local/apache2.4/bin/httpd -k start 
$ ps -ef | grep httpd | grep -v grep 
$ sudo netstat -anp | grep httpd 
$ sudo curl http://127.0.0.1
```

```
$ sudo netstat -anp | grep httpd 
```

![스크린샷 2021-04-12 오후 2 14 39](https://user-images.githubusercontent.com/78870076/114350532-4769f400-9ba4-11eb-83ed-31741b12b662.png)

위와 같이 오류가 뜬다면

```
sudo apt-get update
sudo apt-get install net-tools
```

위의 명령어를 치고 다시 실행해보면

![스크린샷 2021-04-12 오후 2 16 01](https://user-images.githubusercontent.com/78870076/114350775-a16ab980-9ba4-11eb-899c-8c520c1791b3.png)

netstat 명령어는 잘 수행 되지만 그 아래와 같이 curl 명렁어 오류가 뜬다면

```
sudo apt install curl
```

위의 명령어를 치고 실행 해보면

![스크린샷 2021-04-12 오후 2 17 04](https://user-images.githubusercontent.com/78870076/114351002-f7d7f800-9ba4-11eb-9068-8db21fc41c09.png)

위의 사진처럼 `Apache`가 정상적으로 실행된 모습을 볼 수 있다.

## 📂 Reference

- [https://sangminlog.tistory.com/entry/how-to-install-apache?category=871225](https://sangminlog.tistory.com/entry/how-to-install-apache?category=871225)



