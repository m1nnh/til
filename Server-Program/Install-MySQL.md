## MySQL 8.0.19 소스(수동) 설치
`Ubuntu 20.04.2` 위에 `Apache 2.4.46` 설치를 완료했다. 

[Apache 설치 방법](https://minhyeok-rithm.tistory.com/entry/Install-Apache?category=854409)

`MySQL`은 버전 8.0.19를 수동 설치했다. `MySQL` 설치가 오래 걸린다고 하여 스냅샷을 찍어 백업을 해두었다.

### 의존성 패키지 설치
[MySQL 공식 문서](https://dev.mysql.com/doc/refman/8.0/en/source-installation-prerequisites.html)에 꼭 설치해야 하는 패키지들이 명시되어 있다. `apt-get update`후 필요한 패키지들을 설치하면 된다.

### MySQL Community Server 8.0.19 설치
`Apache` 설치할 때와 동일하게 `tar.gz`파일을 다운받아 압축 해제한다.

```
$ cd /usr/local
$ wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.19.tar.gz
$ tar xvfz MySQL-8.0.19.tar.gz
```

### MySQL 설치
- cmake : 설치 옵션을 부여한다.
- make : build
- make install : 설치

```
$ cd /usr/local/mysql-8.0.19 
$ mkdir <DIRNAME> 
$ cd <DIRNAME> 
$ cmake .. \ 

-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \ 
-DMYSQL_DATADIR=/usr/local/mysql/data \ 
-DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock \ -DMYSQL_TCP_PORT=3306 \ -DDEFAULT_CHARSET=utf8 \ -DDEFAULT_COLLATION=utf8_general_ci \ -DSYSCONFDIR=/etc \ -DWITH_EXTRA_CHARSETS=all \ -DWITH_INNOBASE_STORAGE_ENGINE=1 \ -DWITH_ARCHIVE_STORAGE_ENGINE=1 \ -DWITH_BLACKHOLE_STORAGE_ENGINE=1 \ 
-DDOWNLOAD_BOOST=1 \ 

-DWITH_BOOST=/usr/local/mysql/boost 

$ make && make test && make install
```

![스크린샷 2021-04-12 오후 2 36 33](https://user-images.githubusercontent.com/78870076/114356481-a1ba8300-9bab-11eb-9956-89cb3d2c05e7.png)

다음과 같이 에러가 뜬다면

```
sudo apt install cmake
```

명령어를 실행한 후 다시 cmake 과정을 수행한다. 또한 설치 도중에

![스크린샷 2021-04-12 오후 3 09 31](https://user-images.githubusercontent.com/78870076/114356750-ed6d2c80-9bab-11eb-8831-60a190994633.png)

위와 같은 에러가 떴다. 구글링을 해보니 **코드의 에러**가 아니라 **메모리 에러인 경우**가 많다고 한다. 그래서 메모리를 조정해주었다.

```
$ sudo swapoff -a
$ sudo dd if=/dev/zero of=/swapfile bs=1G count=8
$ sudo mkswap /swapfile
$ sudo swapon /swapfile
$ grep SwapTotal /proc/meminfo
```

- Swap 프로세스 종료
- Swap Resize 실행
- 사용가능한 파일을 swap으로 만들어준다.
- 해당 Swap 파일을 활성화한다.
- 마지막으로 이용가능한 swap의 정도를 보는 코드는 마지막 코드와 같다.

![스크린샷 2021-04-12 오후 3 13 41](https://user-images.githubusercontent.com/78870076/114357206-708e8280-9bac-11eb-8da9-86ea661afc53.png)

위의 명령어들을 실행하고 다시 설치를 진행하니 에러 없이 진행이 잘 됐지만 시간이 상당히 오래걸렸다.

### MySQL 데이터베이스 초기화

#### mysql 그룹 및 유저 생성

```
$ groupadd mysql
$ useradd -r -g mysql -s /bin/false mysql
```

#### mysql-files 디렉토리 생성

```
$ cd /usr/local/mysql
$ mkdir mysql-files
```

#### 권한 설정

- chown : change own, 파일의 소유권자 변경
- chmod : change mode, 파일과 디렉토리의 사용 권한 변경

```
$ chown -R mysql:mysql /usr/local/mysql
$ chown mysql:mysql mysql-files
$ chmod 750 mysql-files
```
#### 기본 데이터베이스 설정

```
$ bin/mysqld --initialize --user=mysql \
--basedir=/usr/local/mysql \
--datadir=/usr/local/mysql/data
```

위에 명령어를 입력하면 `root@localhost: password`라는 문구가 맨 아래쪽에 뜨게 되는데 `passward` 부분이 임시 비밀번호이니 기억하자.

### 비밀번호 초기화

#### mysql 서버 실행
- mysqld_safe : mysql 실행

```
$ bin/mysqld_safe --user=mysql &
```

& 명령어를 통해 백그라운드에서 프로세스를 실행시켜준다.

#### 서버 연결 및 종료

```
$ bin/mysql -u root -p
```
위에 명령어를 입력하면 패스워드를 입력해야 하는데 아까 기억해둔 임시 비밀번호를 입력하면 된다.

![스크린샷 2021-04-12 오후 4 14 36](https://user-images.githubusercontent.com/78870076/114358177-99fbde00-9bad-11eb-8ce1-78602827c231.png)

그런데 백그라운드에 서버가 연결이 되지 않았다는 에러가 떴다. 그래서 서버 연결을 직접 해주었다.

![스크린샷 2021-04-12 오후 4 15 41](https://user-images.githubusercontent.com/78870076/114358254-abdd8100-9bad-11eb-949f-70dfbc874e8d.png)

위와 같이 서버 연결을 다시 해주고 앞에 명령어를 다시 입력 해주면

![스크린샷 2021-04-12 오후 4 16 27](https://user-images.githubusercontent.com/78870076/114358464-e9420e80-9bad-11eb-985a-cc5ddc57bee5.png)

잘 실행이 된다. 그리고 쉘 화면이 `mysql>`로 변한다. 아래의 명령어를 통해 `root` 비밀번호를 변경할 수 있다. `root-password` 부분은 사용자가 원하는 대로 변경이 가능하다.

```
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'root-password';
```

MySQL 서버는 `shutdown` 명령어를 통해 종료할 수 있다.

```
$ bin/mysqladmin -u root -p shutdown
```

![스크린샷 2021-04-12 오후 4 16 51](https://user-images.githubusercontent.com/78870076/114358506-f4953a00-9bad-11eb-9aee-b4540dc595d1.png)

## 📂 Reference

- [https://sangminlog.tistory.com/entry/how-to-install-mysql?category=871225](https://sangminlog.tistory.com/entry/how-to-install-mysql?category=871225)



