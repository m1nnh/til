## 한방쿼리

### 카테고리

<img width="720" alt="image" src="https://user-images.githubusercontent.com/78870076/116816257-25a8cf00-ab9c-11eb-97d4-47686b4e4bbb.png">

### 게시판

<img width="1201" alt="image" src="https://user-images.githubusercontent.com/78870076/116816269-38230880-ab9c-11eb-9495-340d8e099335.png">

게시판 같은 경우는 일반 조인을 사용하게 되면 별점이 아예 없는 음식점의 경우는 데이터에 뜨지 않기 때문에 레프트 조인을 사용했다.

### 음식점 기본 정보

<img width="1171" alt="image" src="https://user-images.githubusercontent.com/78870076/116816301-5ee13f00-ab9c-11eb-816c-831cc392ad39.png">

별점과, 리뷰, 댓글, 찜 같은 경우 테이블을 조인하여 나타냈다.

### 메뉴

<img width="871" alt="image" src="https://user-images.githubusercontent.com/78870076/116816352-889a6600-ab9c-11eb-9076-4a5772e85d4b.png">

### 원산지 표기 및 가게 소개

<img width="645" alt="image" src="https://user-images.githubusercontent.com/78870076/116816372-a10a8080-ab9c-11eb-930d-d20643506867.png">

### 음식점 정보

<img width="1073" alt="image" src="https://user-images.githubusercontent.com/78870076/116816409-c39c9980-ab9c-11eb-9c38-f824d5feeb36.png">

<img width="998" alt="image" src="https://user-images.githubusercontent.com/78870076/116815691-ae723b80-ab99-11eb-82f5-a197345a031a.png">

### 리뷰1

<img width="1770" alt="image" src="https://user-images.githubusercontent.com/78870076/116816439-dfa03b00-ab9c-11eb-97df-1171830b0194.png">

리뷰 쿼리는 수정이 필요하다. 일단 가상 테이블을 이용하여 최근 6개월 별점 표기를 했고, 별점 1점부터 5점 까지의 개수도 모두 가상테이블을 하나하나 만들어서 표기했다. 이 부분은 수정할 예정이다.

### 리뷰2

<img width="994" alt="image" src="https://user-images.githubusercontent.com/78870076/116816484-0eb6ac80-ab9d-11eb-8ea1-f359fd63d10d.png">

리뷰2는 최근 한 달 간의 리뷰수와 총 사장님 댓글을 표시했다.

### 리뷰3

<img width="1050" alt="image" src="https://user-images.githubusercontent.com/78870076/116816530-3efe4b00-ab9d-11eb-9afa-1b57adb28c58.png">

리뷰3는 일반 사용자들의 리뷰이다. 화면에 보이는 것들을 표기했으며 날짜 같은 경우 배달의 민족에서 표기하는 방식을 사용해서 쿼리문을 작성했다.

### 찜

<img width="995" alt="image" src="https://user-images.githubusercontent.com/78870076/116816546-4faec100-ab9d-11eb-9b00-b3ab6d198040.png">

찜 같은 경우는 게시판과 비슷하다. 하지만 isLiked 플래그를 이용하여 'Y'에 해당하는 음식점들만 데이터들을 표시하게 했다.

### 장바구니

<img width="850" alt="image" src="https://user-images.githubusercontent.com/78870076/116816560-6b19cc00-ab9d-11eb-9517-69a0366f95c8.png">

장바구니 같은 경우는 두 개의 쿼리문을 작성했다. 우선 전체적인 메뉴 수량 및 가격들을 표시했고, 토탈 가격을 따로 쿼리를 작성했다.

### 검색

<img width="854" alt="image" src="https://user-images.githubusercontent.com/78870076/116816605-96042000-ab9d-11eb-834e-2684920078ba.png">

검색 같은 경우는 최근 일주일 검색 기록을 표시했고, 1시간마다 검색 순위가 바뀌는 것 같아 누적 검색량을 1시간 단위로 설정하였다.

## 한마디

사용자 쿼리문도 작성하였지만, 개인정보가 표시되어있어 올리지 않았다. 하지만 사용자 같은 경우는 쉽게 구현할 수 있을 것이다. 처음 작성한 쿼리문들이었다. 일주일간 많은 것을 배운 느낌이었다.