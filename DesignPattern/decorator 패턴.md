# decorator 패턴

생성일: 2020년 12월 26일 오전 11:45

## 1. 기능 재사용을 위한 상속의 폐해

![decorator%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%20c3d29c77d33140c0a97747e39406d6f9/ADE2E429-872D-4FF8-86FB-45B0C4273668.jpeg](decorator%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%20c3d29c77d33140c0a97747e39406d6f9/ADE2E429-872D-4FF8-86FB-45B0C4273668.jpeg)

- 위 그림의 문제점
    - 첨가물의 가격이 바뀔 때 beverage클래스를 수정해야함
    - 첨가물 종류가 더 생길 때 beverage클래스를 수정해야함
    - 특정 첨가물이 들어가면 안되는 경우에도 저걸 다 상속받게됨
    - 더블모카라면 곤란해짐

### 단순히 기능 재사용을 위해 상속을 사용할 경우

- 모든 서브클래스에서 똑같은 행동을 상속받아야 함.
- 하나 인터페이스 바꾸면 기존 코드의 수정이 많이 일어남
- (cf. 오브젝트- 서브클래싱, 서브타이핑 챕터)

**⇒ 기존코드를 수정하지 않도록 합성을 통해 행동을 확장하자.** 

** 모든 설계에 무조건 OCP를 적용하라는 뜻은 아님. 바뀔 확률이 높은 부분에 중점을 둬서 원칙을 적용하자. 

## 2. 데코레이터 패턴이란

### 개념

![decorator%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%20c3d29c77d33140c0a97747e39406d6f9/A2A7E687-965A-4B23-954E-59E26D670B8A.jpeg](decorator%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%20c3d29c77d33140c0a97747e39406d6f9/A2A7E687-965A-4B23-954E-59E26D670B8A.jpeg)

- 한 객체를 여러개의 데코레이터로 감쌀 수 있다.
    - how? 데코레이터 클래스의 타입(형식)은 그 클래스가 감싸고 있는 클래스의 타입을 반영하도록 만들어서 (이것은 상속을 통해 일어남)
- 데코레이터에서는 자기가 감싸고 있는 구성요소의 메서드를 호출한 결과 + 새로운 기능을 더해서 행동을 확장한다.

![decorator%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%20c3d29c77d33140c0a97747e39406d6f9/A6F59784-41B7-4F6C-AC94-C48A21D24227.jpeg](decorator%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%20c3d29c77d33140c0a97747e39406d6f9/A6F59784-41B7-4F6C-AC94-C48A21D24227.jpeg)

![decorator%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%20c3d29c77d33140c0a97747e39406d6f9/8361B73E-99BE-475C-967D-0A855FD98865.jpeg](decorator%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%20c3d29c77d33140c0a97747e39406d6f9/8361B73E-99BE-475C-967D-0A855FD98865.jpeg)

### 코드레벨에서 살펴보자

```java
// 음료 (컴포넌트)
public abstract class Beverage {
        String description = "";
        public abstract double cost();

        public String getDescription() {
            return description;
        }
}

    // 첨가물 (데코레이터 추상 클래스)
    public abstract class CondimentDecorator extends Beverage {
        public abstract String getDescription();
    }

    // 음료 구현체 (구현된 컴포넌트)
    public class Espresso extends Beverage {
        public Espresso() {
            description = "에스프레소";
        }
        public double cost() {
            return 1.99;
        }
    }
    
    // 첨가물 구현체 (구상 데코레이터)
    public class Whip extends CondimentDecorator {
        Beverage beverage; // decorator는 감싸고자 하는 음료를 저장하기 위한 인스턴스 변수를 필드로 가짐

        public Whip(Beverage beverage) {
            this.beverage = beverage; // 생성자를 통해 인스턴스변수에 감싸고자 하는 객체를 넣어줌
        }

        public String getDescription() {
            return beverage.getDescription() + ", + 휘핑크림";
        }

        public double cost() {
            return .20 + beverage.cost();
        }
    }

public class Mocha extends CondimentDecorator {
    Beverage beverage; // decorator는 감싸고자 하는 음료를 저장하기 위한 인스턴스 변수를 필드로 가짐

    public Mocha(Beverage beverage) {
        this.beverage = beverage; // 생성자를 통해 인스턴스변수에 감싸고자 하는 객체를 넣어줌
    }

    public String getDescription() {
        return beverage.getDescription() + ", 모카";
    }

    public double cost() {
        return .40 + beverage.cost();
    }
}

public static void main(String[] args) {
        Beverage beverage = new Espresso();
        System.out.println(beverage.getDescription() + " $" + beverage.cost());

        Beverage beverage2 = new Whip(beverage);
        beverage2 = new Whip(beverage2);
        System.out.println(beverage2.getDescription() + " $" + beverage2.cost());

        Beverage beverage3 = new Mocha(beverage);
        beverage3 = new Mocha(beverage3);
        beverage3 = new Whip(beverage3);
        System.out.println(beverage3.getDescription() + " $" + beverage3.cost());
				
//결과
//에스프레소 $1.99
//에스프레소, 휘핑크림, 휘핑크림 $2.39
//에스프레소, 모카, 모카, 휘핑크림 $2.99

// client가 beverage3의 cost를 요청했을때 cost계산 로직을 따라가보자.
// 감싸고 있는 객체를 합성해서 필드로 가지고 있기 때문에 
// 감싸고있는 객체의 cost를 가져와 감싸는 객체의 cost를 더할 수 있다.
}
```

### 주의할점 (QnA 등)

- 특정 구상 컴포넌트인지 확인한 다음 어떤 작업을 처리하는 경우, 데코레이터를 쓸 수 없지 않냐 (하우스블렌드 커피만 특별할인 등)
    - ⇒ ㅇㅇ 쓰지마라. 추상 컴포넌트 형식을 바탕으로 돌아가는 코드에만 데코레이터를 써라
- 모카, 휘핑, 모카.. 말고 휘핑, 더블모카 로 출력하게 하고싶다. 다른 데코레이터들을 전부 알아야할텐데 이렇게 해도 되는가
    - ⇒ 여러 단계의 데코레이터를 파고 들어가서 어떤 작업을 해야한다면, 원래 데코레이터 패턴이 만들어진 의도와 어긋난다. **데코레이터는 그 데코레이터가 감싸는 객체에 행동을 추가하기 위한 용도로 만들어진 것!**
    - (but 마지막으로 만들어진 객체의 description을 파싱해서 최종 문구를 pretty print할수는 있음)
- 구성요소의 클라이언트 입장에서는, 데코레이터 존재를 알 수 없다. 구체적인 형식에 의존하는 경우에 데코레이터 패턴을 쓰면 문제가 된다.
- 자잘한 객체들이 매우 많이 추가될 수 있어 복잡해질 수도 있다.

## 3. java 에 사용된 데코레이터 패턴

![decorator%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%20c3d29c77d33140c0a97747e39406d6f9/E0806D33-3567-482A-A370-BF4D65A88E10.jpeg](decorator%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%20c3d29c77d33140c0a97747e39406d6f9/E0806D33-3567-482A-A370-BF4D65A88E10.jpeg)

```java
public class LowerCaseInputStream extends FilterInputStream {

    public LowerCaseInputStream(InputStream in) {
        super(in);
    }
    
    public int read() throws IOException {
        int c = super.read();
        return (c == -1 ? c : Character.toLowerCase((char)c));
    }
    public int read(byte[] b, int offset, int len) throws IOException {
        int result = super.read(b, offset, len);
        for (int i = offset; i < offset + result; i++) {
            b[i] = (byte)Character.toLowerCase((char)b[i]);
        }
        return result;
    }
}
```

## 참고.

### spring에 사용된 데코레이터 패턴

토비의 스프링 6장 AOP 

타깃에 부가적인 기능을 런타임 시에 다이내믹하게 부여해주기 위해 프록시를 사용함.

![decorator%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%20c3d29c77d33140c0a97747e39406d6f9/Untitled.png](decorator%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%20c3d29c77d33140c0a97747e39406d6f9/Untitled.png)

[https://jongmin92.github.io/2018/04/15/Spring/toby-6/#프록시와-프록시-패턴-데코레이터-패턴](https://jongmin92.github.io/2018/04/15/Spring/toby-6/#%ED%94%84%EB%A1%9D%EC%8B%9C%EC%99%80-%ED%94%84%EB%A1%9D%EC%8B%9C-%ED%8C%A8%ED%84%B4-%EB%8D%B0%EC%BD%94%EB%A0%88%EC%9D%B4%ED%84%B0-%ED%8C%A8%ED%84%B4) 

### 참고자료

[https://johngrib.github.io/wiki/decorator-pattern/](https://johngrib.github.io/wiki/decorator-pattern/)