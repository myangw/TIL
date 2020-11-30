# Object_ch13

# 오브젝트 13장 정리_서브클래싱과 서브 타이핑

→ searchdev 스터디

## Intro. 상속의 두가지 용도

1. **타입계층** 구현
    - 부모 클래스는 자식 클래스의 일반화. 자식클래스는 부모 클래스의 특수화.
    - 이게 일차적인 목표여야 한다.
2. 코드 재사용
    - 애플리케이션 기능 확장
    - but 부모—자식 클래스 결합도가 강해짐 ㅠ

타입 관계를 고려하지 않은 채 단순히 코드를 재사용하기 위해 상속을 사용하면 안됨!

올바른 타입계층을 구성하는 원칙을 알아보자.

## 1. 타입이란

- 개념관점의 타입
    - 공통의 특징을 공유하는 대상들의 분류.
    - ex.  java, ruby → programming language
        - java, ruby는 programming language 라는 타입의 인스턴스다.
        - **심볼** : 타입에 이름을 붙임. (programming language)
        - **내연**: 타입이 속하는 객체들이 가지는 공통 속성이나 행동 (프로그래밍언어의 정의)
        - **외연**: 타입에 속하는 객체들의 집합. (java, ruby)
- 프로그래밍 언어 관점의 타입
    - 01010101...의 비트 묶음에 의미를 부여하기 위해 정의된 제약과 규칙.
    - 타입에 수행될 수 있는 유효한 오퍼레이션의 집합을 정의한다.
        - ex. java의 '+'는 primitive type or String만 가능

    - 타입에 수행되는 오퍼레이션에 대해 미리 약속된 문맥을 제공한다.
        - ex. java의 '+'가 int일땐 더하기이고 String일땐 문자열 합치기
- 객체지향 관점의 타입
    - 개념관점 + 프로그래밍언어 관점
    - **객체가 수신할 수 있는 메시지의 종류(퍼블릭 인터페이스)를 정의하는 것**
        - 동일한 퍼블릭 인터페이스를 제공하는 객체들은 동일한 타입으로 분류.
            - 속성(state) < 행동(public interface) !!

## 2. 타입 계층

![Object_ch13%20658df03892e246f38a4f4f02f293cdf6/E3CEDA6C-CEBE-45A2-B7B8-2226FA83A835.jpeg](Object_ch13%20658df03892e246f38a4f4f02f293cdf6/E3CEDA6C-CEBE-45A2-B7B8-2226FA83A835.jpeg)

- super type: 두 타입간 관계에서 더 일반적인 타입. 다른 집합의 모든 멤버를 포함한다.
- subtype: 더 특수한 타입. 더 큰 집합에 포함된다.

⇒ 객체지향 관점에서 타입계층

- 슈퍼타입: 서브타입이 정의한 퍼블릭 인터페이스를 일반화시켜 넓은 의미로 정의
- 서브타입: 슈퍼타입이 정의한 퍼블릭 인터페이스를 특수화시켜 구체적이고 좁은 의미로 정의

**서브타입의 인스턴스 집합은** 슈퍼타입 인스턴스의 부분집합이기 때문에 **슈퍼타입의 인스턴스로 간주될 수 있다.**

## 3. 서브클래싱과 서브타이핑

### 언제 상속을 사용해야하는가?

- ⇒ (상속관계가 is-a 관계를 모델링) && (클라이언트 입장에서 부모클래스의 타입으로 자식 클래스를 사용할 수 있을때)
    - is-a 관계의 함정
        - Object-oriented language is a programming language. (O)
        - Penguin is a bird. Bird can fly (<< X)
            - 새의 정의에 날 수 있다는 행동이 포함된다면 펭귄은 새의 서브타입이 될 수 없음.
        - 어휘적 정의로만 생각할 때 잘못될 수 있으므로, 상속 사용의 예비후보 정도로만 생각하라.
    - 행동 호환성
        - 클라이언트 관점에서 두 타입의 행동이 호환될 경우에만 타입계층으로 묶어야 한다.
            - fly를 가진 Bird를 Penguin이 상속했을 때 Penguin도 fly를 가지게 됨
            - 이 때 다음과 같은 유혹들에 빠지지 말자
                1. override 메서드 비워두기

                ```java
                public class Penguin extends Bird {
                ...
                	@Override
                	public void fly() {}
                }
                ```

                2.  override 메서드에서 throw exception <<설마...ㄷㄷ

                3. 특정 타입만 제외시키기

                ```java
                public void flyBird(Bird bird) {
                	if (!(bird instance of Penguin)) {
                		bird.fly();
                	}
                }
                ```

    ### 클라이언트의 기대에 따라 계층 분리하기

    - 상속계층 분리
        - 날수 있는 새와 없는 새로 계층을 분리하자

        ![Object_ch13%20658df03892e246f38a4f4f02f293cdf6/0A602A6F-3C3F-4473-B1C4-E6508E9B0A94.jpeg](Object_ch13%20658df03892e246f38a4f4f02f293cdf6/0A602A6F-3C3F-4473-B1C4-E6508E9B0A94.jpeg)

    - 클라이언트에 따라 인터페이스 분리

        ![Object_ch13%20658df03892e246f38a4f4f02f293cdf6/56F20638-12A1-470E-AF0A-2A5318A98D9A.jpeg](Object_ch13%20658df03892e246f38a4f4f02f293cdf6/56F20638-12A1-470E-AF0A-2A5318A98D9A.jpeg)

        - 이 상황에서 Penguin이 Bird의 코드를 재사용 해야한다면?  → 합성

        ![Object_ch13%20658df03892e246f38a4f4f02f293cdf6/17579FEE-567A-4460-821C-864A7FFA1B08.jpeg](Object_ch13%20658df03892e246f38a4f4f02f293cdf6/17579FEE-567A-4460-821C-864A7FFA1B08.jpeg)

        - client1의 기대 때문에 Flyer를 변경해도, client2에는 영향 x.
        - 인터페이스 분리 원칙(Interface Segregation Principle, ISP)
            - 인터페이스를 클라이언트의 기대에 따라 분리함으로써 변경에 의한 영향을 제어
            - 클라이언트는 자신이 실제로 호출하는 메서드에만 의존해야한다. '비대한' 인터페이스를 여러개의 클라이언트에 특화된 인터페이스로 분리.
        - 주의: 현재의 요구사항이 'fly'에 관심이 없다면 굳이 상속계층에 FlyBird를 미리 추가하는건 복잡성만 늘릴 뿐. 요구사항 속에서 클라이언트가 기대하는 행동에 집중하라.

    ### 서브클래싱과 서브타이핑

    - 서브클래싱: 다른 클래스의 코드를 재사용할 목적으로 상속을 사용하는 경우.
        - 자식클래스와 부모클래스의 행동이 호환되지 않음.
        - → 구현상속, 클래스 상속.
    - 서브타이핑: 타입계층을 구성하기 위해 상속을 사용하는 경우.
        - 자식클래스와 부모클래스의 행동이 호환됨.(행동 호환성)
        - 부모클래스를 새로운 자식 클래스로 대체하더라도 시스템이 문제없이 동작(대체가능성)
        - ex. DiscountPolicy의 상속계층.
        - → 인터페이스 상속.

    ⇒  올바른 상속은 서브타이핑 관계이며, 행동 호환성과 대체가능성을 만족한다. → 리스코프 치환원칙

## 4. 리스코프 치환 원칙(Liskov Substitution Principle, LSP)

상속관계로 연결한 두 클래스가 서브타이핑 관계를 만족시키기 위해서 만족해야할 조건:

S형의 각 객체 o1에 대해 T형의 객체 o2가 하나 있고,  T에 의해 정의된 모든 프로그램 P에서 T가 S로 치환될 때, P의 동작이 변하지 않으면 S는 T의 서브타입이다. 

- 리스코프 치환원칙 위반하는 대표적 사례 : Rectangle - Square

```java
public class Rectangle {
	private int x, y, width, height;
	public Rectangle(int x, int y, int width, int height) {
	...
	}
}

public class Square extends Rectangle {
	public Square(int x, int y, int size) {
	...
	}
	@Override
	public void setWidth(int width) {
		super.setWidth(width);
		super.setHeight(width);
	}
	...
}
```

Rectangle과 협력하는 클라이언트는 width와 height가 다를 수 있다고 가정하여 다음과 같이 프로그래밍 한다.

```java
public void resize(Rectangle rectangle, int width, int height) {
	rectangle.setWidth(width);
	rectangle.setHeight(height);
	assert rectangle.getWidth() == width && rectangle.getHeight() == height;
}
```

그럼 Rectangle 대신에 Square를 전달했을 경우 메서드 실행이 실패한다.

```java
Square square = new Square(10, 10, 10);
resize(square, 50, 100);
```

- Sqaure-Rectangle은 서브클래싱 관계. 직관적으로 상속관계를 정의하지 말자.
- 자식클래스가 부모클래스를 대체하기 위해서는 부모클래스에 대한 클라이언트의 가정을 준수해야한다. 클라이언트와 떨어뜨려놓고 판단하지 말자. 대체 가능성을 결정하는 것은 클라이언트다.

### 유연한 설계

![Object_ch13%20658df03892e246f38a4f4f02f293cdf6/6E58C39D-A503-474B-A808-3D6F7DCE03F7.jpeg](Object_ch13%20658df03892e246f38a4f4f02f293cdf6/6E58C39D-A503-474B-A808-3D6F7DCE03F7.jpeg)

- 의존성 역전 원칙(DIP) : 상위수준 모듈인 Movie와 하위수준 모듈인 OverlappedDiscountPolicy 모두 추상클래스인 DiscountPolicy에 의존한다.
- 리스코프 치환 원칙(LSP): Movie의 관점에서 DiscountPolicy 대신 OverlappedDiscountPolicy와 협력하더라도 아무 문제가 없다.
- 개방-폐쇄 원칙(OCP) : 새로운 기능 추가를 위해 OverlappedDiscountPolicy를 추가해도 Movie 코드를 수정할 필요가 없다.

## 5. 계약에 의한 설계와 서브타이핑

- 클라이언트 관점에서 자식 클래스가 부모 클래스를 대체할 수 있다는 것은 무엇을 의미하는가?

### 계약에 의한 설계(Design By Contract, DBC)

- 클라이언트-서버 협력을 의무와 이익으로 구성된 계약의 관점에서 표현.
- 3가지 요소
    - 사전조건(precondition) : 정상적으로 메서드를 실행하기 위해 **클라이언트가** 만족해야하는 조건
    - 사후조건(postcondition): 메서드가 실행된 후에 **서버가** 클라이언트에게 보장해야하는 조건
    - 클래스 불변식(class invariant): 메서드 실행 전과 후에 인스턴스가 만족시켜야하는것

서브타입이 리스코프치환원칙을 만족시키기 위해서는 클라이언트와 슈퍼타입 간에 체결된 '계약'을 준수해야한다.

예제로 살펴보기

```java
public abstract class DiscountPolicy {
    private List<DiscountCondition> conditions = new ArrayList<>();
...
    public Money calculateDiscountAmount(Screening screening) {
        checkPrecondition(screening); // 사전조건

        Money amount = Money.ZERO;
        for(DiscountCondition each : conditions) {
            if (each.isSatisfiedBy(screening)) {
                amount = getDiscountAmount(screening);
                checkPostcondition(amount);  // 사후조건
                return amount;
            }
        }

        amount = screening.getMovieFee();
        checkPostcondition(amount);  // 사후조건
        return amount;
    }
		// 클라이언트(Movie)가 전달하는 screening이 어때야한다 라는 조건. Movie의 책임
    protected void checkPrecondition(Screening screening) {
        assert screening != null &&
                screening.getStartTime().isAfter(LocalDateTime.now());
    }
		// 서버가 메서드 실행 후의 리턴값에 대해 만족시켜야하는 조건.
    protected void checkPostcondition(Money amount) {
        assert amount != null && amount.isGreaterThanOrEqual(Money.ZERO);
    }

    abstract protected Money getDiscountAmount(Screening Screening);
}
```

```java
public class BrokenDiscountPolicy extends DiscountPolicy {
...

    @Override
    public Money calculateDiscountAmount(Screening screening) {
        checkPrecondition(screening);                 // 기존의 사전조건
        checkStrongerPrecondition(screening);         // 더 강력한 사전조건

        Money amount = screening.getMovieFee();
        checkPostcondition(amount);                   // 기존의 사후조건
        checkStrongerPostcondition(amount);           // 더 강력한 사후조건
        return amount;
    }

    private void checkStrongerPrecondition(Screening screening) {
        assert screening.getEndTime().toLocalTime()
                .isBefore(LocalTime.MIDNIGHT);
    }

    private void checkStrongerPostcondition(Money amount) {
        assert amount.isGreaterThanOrEqual(Money.wons(1000));
    }
...
}
```

- 더 강력한 사전조건을 붙이면 어떻게 될까?
    - Movie는 DiscountPolicy와 협력한다. DiscountPolicy에서 요구하는 사전조건만 충족시키면 될것이라고 생각하고 screening을 줄것임. 그리고나서 에러....
    - BrokenDiscountPolicy는 클라이언트 관점에서 DiscountPolicy를 대체할 수 없으므로, 서브타입이 아니다.
- 더 약한 사전조건은?  가능
- 더 강력한 사후조건?  0보다 큰 값을 반환한다는 계약을 위반하지 않으므로 가능.
- 더 약한 사후조건?
    - 기존의 사후조건을 제거해야함. 마이너스를 반환할 수 있게 되는데, 클라이언트 입장에서 예기치 못한 결과 발생.

서브타입에 더 강력한 사전조건을 정의할 수 없고, 더 약한 사후조건을 정의할 수 없다.

⇒ 부록A 계약에 의한 설계