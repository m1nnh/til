## ERD 설계

- 본 ERD는 실제 배달의민족 ERD와는 관계가 없습니다.

저번 주까지의 과제는 서버 구축 위주의 과제들이었다. 이제 정말로 하나의 앱을 가지고 분석을 하는 과제들이 나오기 시작했다. 바로 ERD 설계와 한방 쿼리문 작성이다. 데이터베이스를 잘 모르고 쿼리문도 잘 몰랐기에 이번 과제에 조금 힘이 들었지만, 이제야 뭔가 배우는 기분이 들었다.

### 배달의 민족 ERD 설계

나는 평상시 관심이 있던 배달의 민족 서비스에 대해서 조사를 했다. 학교 중간고사 과제로는 [배달의 민족 STRIDE 분석](https://github.com/m1nnh/DKU-minh/blob/master/시큐어코딩/MidReport.pdf)을 했었다. 이번엔 배달의 민족 데이터들에 대해서 분석을 하기 시작했다.

### 전체적인 ERD

![스크린샷 2021-05-02 오후 10 31 51](https://user-images.githubusercontent.com/78870076/116815052-bf6d7d80-ab96-11eb-8784-fb3beefa58a7.png)

잘 보이진 않지만 대략적인 ERD이다. 하나하나 다시 살펴보겠다.

### User

<img width="582" alt="image" src="https://user-images.githubusercontent.com/78870076/116815089-e75ce100-ab96-11eb-8d79-f9f85ecad32e.png">

<img width="492" alt="image" src="https://user-images.githubusercontent.com/78870076/116815116-052a4600-ab97-11eb-95a8-93a173cb173d.png">

두 번째 사진을 보고 ERD를 분석하였다. 우선 눈으로 확인할 수 있는 데이터들이 있고, 등급 같은 경우는 Flag를 두어 구분하였다. 또한 사용자의 주소를 알려면 경도와 위도도 필요하다. 경도와 위도의 데이터 타입은 데시말을 이용하면 된다.

### Coupon & Point & CouponUser

<img width="482" alt="image" src="https://user-images.githubusercontent.com/78870076/116815167-49b5e180-ab97-11eb-8be2-8e0925f09d40.png">

<img width="505" alt="image" src="https://user-images.githubusercontent.com/78870076/116815188-64885600-ab97-11eb-8df5-72419c01d96a.png">

세 개의 테이블이 있다. 우선 쿠폰 정보를 가지고 있는 쿠폰 테이블과 쿠폰을 사용하는 쿠폰 유저 테이블로 나누었다. 그리고 포인트 테이블은 적립과 사용내역만 넣어두고 보유 포인트 같은 경우는 적립금 - 사용요금을 통해 나타낼 수 있으므로 따로 칼럼으로 지정해주진 않았다.

### LikeStore & Search

<img width="518" alt="image" src="https://user-images.githubusercontent.com/78870076/116815242-9ef1f300-ab97-11eb-892c-aa76f69b83e8.png">

<img width="481" alt="image" src="https://user-images.githubusercontent.com/78870076/116815262-b335f000-ab97-11eb-914b-0366bfd7ef6f.png">

다음은 검색 테이블과 찜 테이블이다. 찜 같은 경우는 음식점 인덱스와 사용자 인덱스를 받아오는 것이 포인트이고, 검색 같은 경우는 한방 쿼리를 짤 때 등록시간이 매우 중요하게 사용되었다.

### Basket & Order & Category

<img width="506" alt="image" src="https://user-images.githubusercontent.com/78870076/116815309-dc568080-ab97-11eb-94c2-701e08d788ef.png">

<img width="1032" alt="image" src="https://user-images.githubusercontent.com/78870076/116815445-90580b80-ab98-11eb-829f-c06dba56d5ce.png">

다음은 장바구니 테이블, 주문 테이블, 카테고리 테이블이다. 장바구니 같은 경우는 메뉴 수량 부분을 나중에 토탈 비용을 구할 때 사용되기 때문에 칼럼명으로 넣었다. 그리고 주문 테이블은 장바구니 인덱스를 그대로 들고 오고 나머지는 보이는 대로 넣었다. 결제수단 같은 경우는 플래그로 나누어서 표현하였다. 카테고리는 카테고리 창에 보이는 그대로를 칼럼에 넣었다.

### Store

<img width="472" alt="image" src="https://user-images.githubusercontent.com/78870076/116815512-d90fc480-ab98-11eb-9282-98e4bed6cc9d.png">

<img width="998" alt="image" src="https://user-images.githubusercontent.com/78870076/116815691-ae723b80-ab99-11eb-82f5-a197345a031a.png">

음식점 테이블은 많아보이지만 간단하다. 하지만 여기서 더 세분화할 예정이다. 고정 데이터들을 따로 테이블화 시키고 유동적으로 변하는 테이블들은 따로 구분해 데이터들이 변할 때마다 모든 데이터가 수정이 되지 않도록 정규화할 예정이다.

### Menu

<img width="509" alt="image" src="https://user-images.githubusercontent.com/78870076/116815736-d5c90880-ab99-11eb-88c2-0e93526c9fcb.png">

![image](https://user-images.githubusercontent.com/78870076/116815743-dd88ad00-ab99-11eb-8897-69705d79ae86.jpeg)

메뉴 테이블도 딱히 어려울 것은 없었다. 보이는 그대로를 표시해줬고, 기본적으로 대표 메뉴와 1인분 메뉴가 기본적으로 다들 있었다. 그것을  메뉴 플래그로 나누어 주었다. 하지만 가게별로 태그 되는 것들이 바뀌기 때문에 이 부분은 좀 더 수정을 해야 될 것 같다.

### Review & BossComment & BossNotice

<img width="508" alt="image" src="https://user-images.githubusercontent.com/78870076/116815776-0ad55b00-ab9a-11eb-827d-d10264eb1a8a.png">

<img width="490" alt="image" src="https://user-images.githubusercontent.com/78870076/116815810-322c2800-ab9a-11eb-815b-6fdb95d3f222.png">

리뷰도 세 부분으로 나누었다. 우선 대댓글은 사장님 밖에 달지 못하였다. 그리고 나머지는 화면에 보이는 대로 칼럼명을 작성했다.

## 한마디

처음으로 ERD 설계를 진행하는데, 어려웠다. 우선 감이 잡히질 않았었고 뭔가 제대로 짜는 기분이 들지 않았다. 앞으로 계속 쿼리를 작성하면서 수정을 진행할 예정이다. 또한 많이 어려웠다.