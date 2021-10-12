## 날짜와 시간 & 형식화

### Calendar와 GregorianCalender

`Calendar`는 추상 클래스이기 때문에 직접 객체를 생성할 수 없고, 메소드를 통해서 완전히 구현된 클래스의 인스턴스를 얻어야 한다.

```
Calendar cal = new Calendar(); // 에러

// getInstance() 메소드는 Calendar 클래스를 구현한 클래스의 인스턴스를 반환한다.
Calendar cal = Calendar.getInstance();
```

Calendar를 상속받아 완전히 구현한 클래스로는 `GregorianCalendar`와 `BuddhistCalendar`이 있는데 getInstance()는 시스템의 국가와 지역설정을 확인해서 태국인 경우에는 BuddhistCalendar의 인스턴스를 반환하고, 그 외에는 GregorianCalendar의 인스턴스를 반환한다.

인스턴스를 직접 생성해서 사용하지 않고 이처럼 메소드를 통해서 인스턴스를 반환받게 하는 이유는 최소한의 변경으로 프로그램이 동작할 수 있도록 하기 위한 것이다.

```
class MyApplication {
    public Static void main(String[] args) {
        Calendar cal = new GregorianCalendar();
    }
}
```

Calendar가 새로 추가되면서 Date는 대부분의 메소드가 `deprecated` 되었으므로 잘 사용되지 않는다. 그럼에도 불구하고 여전히 Date를 필요로 하는 메소드들이 있기 때문에 Calendar를 Date로 또는 그 반대로 반환하는 일이 생긴다.

- Calendar를 Date로 반환

```
Calendar cal = Calendar.getInstance();
Date d = new Date(cal.getTimeInMillis()); // = Date(long date)
```

- Date를 Calendar로 반환

```
Date d = new Date();
Calendar cal = Calendar.getInstance();
cal.setTime(d)
```

### 형식화 클래스

형식화 클래스는 형식화에 사용될 패턴을 정의하는데, 데이터를 정의된 패턴에 맞춰 형식화할 수 있을 뿐만 아니라 역으로 형식화된 데이터에서 원래의 데이터를 얻어낼 수도 있다. 이것은 마치 "123"과 같은 문자열을 Integer.parseInt()를 사용해서 123이라는 숫자로 변환하는 것과 같은 일이 가능하다는 것을 의미한다.

#### DecimalFormat

형식화 클래스 중에서 숫자를 형식화 하는데 사용되는 것이 `DecimalFormat`이다. DecimalFormat을 이용하면 숫자 데이터를 정수, 부동소수점, 금액 등의 다양한 형식으로 표현할 수 있으며, 반대로 일정한 형식의 텍스트 데이터를 숫자로 쉽게 변환하는 것도 가능하다.

기호 | 의미 
:--- | :---
0 | 10진수(값이 없을 때는 0)
\# | 10진수
. | 소수점
\- | 음수부호
E | 지수 기호
; | 패턴 구분자
% | 퍼센트

DecimalFormat을 사용하는 방법은 간단하다. 먼저 원하는 출력형식의 패턴을 작성하여 DecimalFormat 인스턴스를 생성한 다음, 출력하고자 하는 문자열로 `format` 메소드를 호출하면 원하는 패턴에 맞게 변환된 문자열을 얻게 된다.

```
double number = 1234567.89;
DecimalFormat df = new DecimalFormat("#.#E0");
String result = df.format(number) 
```

#### ChoiceFormat

`ChoiceFormat`은 특정 범위에 속하는 값을 문자열로 변환해준다. 연속적 또는 불연속적인 범위의 값들을 처리하는 데 있어서 if문이나 switch문은 적절하지 못한 경우가 많다.

```
class ChoiceFormatEx {
    public static void main(String[] args) {
        double[] limits = {60, 70, 80, 90}; // 오름차순 정렬해야 한다.
        String[] grades = {"D", "C", "B", "A"}; // grades와 limits 개수가 동일해야함
        int[] scores = {100, 95, 88, 70, 52, 60, 70};

        ChoiceFormat form = new ChoiceFormat(limits, grades);

        for (int i = 0; i < scores.length; i++) {
            System.out.println(scores[i] + " : " + for.format(scores[i])); // 100 : A \ 95 : A ...
        }
    }
}
```

예제에서는 4개의 경계값에 의해 '60 ~ 69', '70 ~ 79', '80 ~ 89', '90~'의 범위가 정의되었다.

### java.time 패키지

날짜와 시간을 하나로 표현하는 Calendar 클래스와 달리, `java.time` 패키지에서는 날짜와 시간을 별도의 클래스로 분리해 놓았다. 시간을 표현할 때는 `LocalTime` 클래스를 사용하고, 날짜를 표현할 때는 `LocalDate` 클래스를 사용한다. 그리고 날짜와 시간이 모두 필요할 때는 `LocalDateTime` 클래스를 사용하면 된다.

```
LocalDate date = LocalDate.now(); // 현재 날짜(2021-07-20)
LocalTime time = LocalTime.now(); // 현재 시간(17:17:40.875)
LocalDateTime dateTime = LocalDateTime.now(); // 2021-07-20T17:17:40.875
ZonedDateTime dateTimeInKr = ZonedDateTime.now(); // 2021-07-20T17:17:40.875+09:00[Asia/Seoul]
```

LocalDate와 LocalTime은 java.time 패키지의 가장 기본이 되는 클래스이며, 나머지 클래스들은 이들의 확장이므로 이 두 클래스만 잘 이해하고 나면 나머지는 아주 쉬워진다. 객체를 생성하는 방법은 현재의 날짜와 시간을 LocalDate와 LocalTime으로 각각 반환하는 now()와 지정된 날짜와 시간으로 LocalDate와 LocalTime 객체를 생성하는 of()가 있다. 둘 다 static 메소드이다.

```
LocalDate today = LocalDate.now(); // 현재 날짜
LocalTime now = LocalTime.now(); // 현재 시간

LocalDate birthDate = LocalDate.of(1996, 10, 10); // 1996년 10월 10일
LocalTime birthTime = LocalTime.of(23, 59, 59); // 23시 59분 59초
```

