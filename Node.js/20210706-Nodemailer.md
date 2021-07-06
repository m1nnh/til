## nodemailer

로그인 시 비밀번호를 까먹어 임시 비밀번호를 발급받은 경험이 있을 것이다. node.js에서는 `nodemailer` 모듈을 통해 임시 비밀번호 발급 이메일을 쉽게 보낼 수 있다. 해당 모듈을 사용해 임시 비밀번호를 메일로 전송해주는 방법에 대해 알아보자.

### 모듈 설치

```
$ npm install nodemailer
```

### google 계정 보안 설정 변경

gmail을 통해 이메일을 보내게 했는데, 아래 두 설정을 해줘야 구글 계정에 정상적으로 접근하여 이메일을 보낼 수 있다.

- [https://myaccount.google.com/lesssecureapps](https://myaccount.google.com/lesssecureapps)
    - 보안 수준이 낮은 앱 허용 : 사용
- [https://accounts.google.com/b/0/DisplayUnlockCaptcha](https://accounts.google.com/b/0/DisplayUnlockCaptcha)
    - 내 Google 계정에 대한 액세스 허용

### Code 작성

- config/email.js

```
const nodemailer = require('nodemailer'); 
const secret_config = require('../config/secret');

const smtpTransport = nodemailer.createTransport(

    { service: "Gmail", auth: { user: "USER@gamil.com", pass: "USER_PASSWORD" }, 
    tls: { rejectUnauthorized: false } });


module.exports = smtpTransport;
```

- Router

```
app.post('/users/find-password', user.findPassword);
```

- Controller

```
const smtpTransport = require('../../../config/email');

exports.findPassword = async function (req, res) { 
    const emailOption = {
    from : "USER@gmail.com",
    to : "보내고자 하는 이메일",
    subject : "이메일 제목",
    html(또는 text) : "이메일 본문"
    }

    // 에러 처리
    let flag = 0;
    await smtpTransport.sendMail(emailOption, (err, flag) => {
        if (err) {
            flag = 1 // 에러
        }
        else flag = 0; // 정상
    });

    if (flag === 1) {
        smtpTransport.close();
        logger.error(`App - passwordSendMail Query error\n: ${JSON.stringify(err)}`);
        return res.json(response.successFalse(2054, "임시 비밀번호 발급에 실패했습니다."));
    } else {
        smtpTransport.close();
        return res.send(response(baseResponse.SUCCESS, "임시 비밀번호가 성공적으로 발급되었습니다."));
    }
}
```

AWS 서버에 올리려면 인바운드 규칙에 25번 포트를 허용시켜야 `request timeout` 에러가 뜨지 않으니 주의하자 !