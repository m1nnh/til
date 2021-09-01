## SQL Injection

해커에 의해 조작된 SQL 쿼리문이 데이터베이스에 그대로 전달되어 비정상적 명령을 실행시키는 공격 기법

### Error based SQL Injection

논리적 에러를 이용한 `SQL Injection`으로 가장 많이 쓰이고 대중적인 공격 기법이다.

<center><img src = "https://user-images.githubusercontent.com/78870076/131655797-7de6b31b-7cf9-4bce-983d-d1a50bd832a7.png"></center>

```
select *
from Users
where id = 'Input1' and password = 'Input2'
```

위에 구문을 통해 입력 값에 대한 검증이 없을을 확인하고, 악의적인 사용자가 임의의 SQL 구문을 주입한다.

```
select *
from Users
where id = ' ' or 1 = 1 --
```

주입한 내용으로는 `'or 1=1 --`로 where절에 있는 싱글 쿼터를 닫아주고 or 1 = 1 구문을 이용해 where절을 모두 참으로 만들어준다. 그리고 맨 뒤에 **--를 넣어 뒤의 구문을 모두 주석 처리**를 해준다.

간단한 구문이지만 Users 테이블에 있는 모든 정보를 조회하게 되고 가장 먼저 만들어진 계정으로 로그인에 성공하게 된다.

### Union based SQL Injection 

`Union` 명령어는 두 개의 쿼리문에 대한 결과를 통합해서 하나의 테이블로 보여주는 키워드이다. Union Injection을 성공하기 위해서는 두 가지 조건이 있다.

- Union 하는 두 테이블의 칼럼 수와 데이터 형이 같아야 한다.

<center><img src = "https://user-images.githubusercontent.com/78870076/131655731-a665fb69-f2aa-4c62-b3d0-01618aae8685.png"></center>

위의 그림에서 보이는 쿼리문은 Board 테이블에서 게시글을 검색하는 것이다. 입력 값을 `title`과 `contents` 칼럼의 데이터와 비교한 후 비슷한 글자가 있는 게시글을 출력한다. 여기서 입력 값으로 Union 키워드와 함께 칼럼 수를 맞춰 select 구문을 넣어주게 되면 두 쿼리문이 합쳐져 하나의 테이블로 보여진다. 인젝션 한 구문은 사용자의 id와 password를 요청하는 쿼리문인데, 인젝션이 성공하게 되면 사용자의 개인정보가 게시글과 함께 화면이 보여진다. 보통 비밀번호를 평문으로 데이터베이스에 저장하지는 않지만 인젝션이 가능하다는 점에서 이미 그 이상의 보안 위험에 노출되어 있다.

### 방어 대책

- Input 값을 받을 때, `validation` 처리하기

쿼리문으로 공격을 하였을 때의 가장 큰 특징은 **입력 값이 올바른 형식으로 입력 되었나 잘못 되었나를 체크** 해주지 않아 발생한다.

- SQL Server 에러 시, 해당하는 에러 메시지 감추기

view를 활용하여 원본 데이터베이스 테이블에는 접근 권한을 높인다. 일반 사용자는 view로만 접근하여 에러를 볼 수 없도록 만든다.

- Preparestatement 사용하기

Preparestatement를 사용하면, 특수문자를 자동으로 escaping 해준다. 그래서 이것을 활용하여 서버 측에서 필터링 과정을 통해 공격을 방어한다.

