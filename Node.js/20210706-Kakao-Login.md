## Node.js를 이용한 소셜 로그인

소프트 스퀘어드에서 모의 외주를 진행하며 소셜 로그인을 구현했다. `Node.js`를 통해 개발했기 때문에 `passport`, `passport-kakao` 모듈을 어떻게 사용하는지 알아보자.

### 소셜 로그인 인증 시퀀스

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbBlXXO%2Fbtq3UtSgwBs%2F8fONc8X5v6L3orbvjqxP3k%2Fimg.png)

클라이언트가 카카오로부터 발급받은 `Access Token`을 서버에 전달하고, 서버는 그것을 다시 카카오에 검증을 요청한다. 카카오 서버는 자기가 클라이언트에게 부여한 토큰과 서버로부터 받은 토큰이 같을 경우 사용자 정보를 내어준다. 이것이 우리가 흔히 알고 있는 `OAuth` 소셜 로그인의 동작 방식이다. 이렇게 사용자 정보를 받은 서버는 데이터베이스에 저장하고, `JWT Token`을 클라이언트에게 제공한다.

### 토큰 발급

[Kakao Developers](https://developers.kakao.com)에서 애플리케이션을 등록한다.

- 내 애플리케이션 -> 애플리케이션 추가하기

![image](https://user-images.githubusercontent.com/78870076/124544034-52f64f80-de61-11eb-8f19-6e440699a7e7.png)

- 플랫폼 -> Web 플랫폼 등록

![image](https://user-images.githubusercontent.com/78870076/124544111-74efd200-de61-11eb-9aa1-51621aca32a8.png)

- 사이트 도메인을 등록하면 `Redirect URI`를 등록해야 한다고 나온다. 카카오 인증 후에 콜백받을 URI를 지정해주면 된다.

![image](https://user-images.githubusercontent.com/78870076/124544193-a2d51680-de61-11eb-8d6b-6b2b2b45404d.png)

요약 정보에 있는 `REST API Key`를 메모해두자!

### npm 모듈 적용

아래 명령어를 통해 모듈을 설치해준다. (개발 환경을 로컬과 서버나누고 있다면, 위에 리다이렉트도 두개 다 설정해주고, 모듈도 각각 설치해준다.)

```
$ npm install passport
$ npm install passport-kakao
$ npm install axios
```

### Code 작성

- Router

```
app.post('/users/kakao-login', user.kakaoLogin);
    app.get('/kakao', passport.authenticate('kakao-login'));
    app.get('/auth/kakao/callback', passport.authenticate('kakao-login', {
        successRedirect: '/',
        failureRedirect : '/',
    }), (req, res) => {res.redirect('/');});
```

- Controller

```
const passport = require('passport')
const KakaoStrategy = require('passport-kakao').Strategy

passport.use('kakao-login', new KakaoStrategy({ 
    clientID: '[REST API Key]',
    callbackURL: '[등록한 Redirect URI] -> /auth/kakao/callback', 
}, async (accessToken, refreshToken, profile, done) => 
{   
    console.log(accessToken);
    console.log(profile); 
}));
```

![image](https://user-images.githubusercontent.com/78870076/124545002-33602680-de63-11eb-9cfe-4b5b8f1c488d.png)

위에 콘솔창 체크박스에 있는 `Access Token`을 가져온 후에, 카카오 로그인 함수에 `body` 값으로 Access Token을 넘겨준다.

```
const {accessToken} = req.body;
let kakao_profile;

kakao_profile = await axios.get('https://kapi.kakao.com/v2/user/me', {
                headers: {
                    Authorization: 'Bearer ' + accessToken,
                    'Content-Type': 'application/json'
                }
```

![image](https://user-images.githubusercontent.com/78870076/124545111-64405b80-de63-11eb-9c23-bda461c27dff.png)

마지막으로 카카오 로그인 함수를 제대로 구현하면, 소셜 로그인에 성공한 화면을 볼 수 있을 것이다.


