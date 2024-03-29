# Ch15. 디자인패턴과 프레임워크

협력을 일관성있게 만들기 위한 방법



## Design Pattern

* 다양한 변경을 다루기 위해 반복적으로 재사용할 수 있는 설계의 묶음.
* 특정 상황에 적용 가능한 패턴을 알고있다면 객체들의 역할, 책임, 협력관계를 쉽게 구성할 수 있다.
* **패턴의 구성요소는 클래스가 아니라 '역할' 이다**.
  * 역할은 동일한 오퍼레이션에 대해 응답할 수 있는 책임의 집합.
  * 하나의 객체가 n개의 역할을 수행할 수도 있음.
* 주의 - 패턴이 설계의 목표가 되지 않도록. 명확한 목적 없이 패턴을 남용하면 복잡도만 증가한다.



## Framework

* '추상 클래스나 인터페이스를 정의하고 인스턴스 사이의 상호작용을 통해 시스템 전체 혹은 일부를 구현해놓은 **재사용 가능한 설계**'

* 애플리케이션 영역에 걸쳐 공통의 클래스들을 정의해서 일반적인 설계 결정을 미리 내려둔다.

* 좋은 객체지향 설계는 의존성이 역전된 설계다.

  * 전통적 구조: 상위 정책이 구체적 세부사항에 의존. 애플리케이션 코드가 라이브러리나 tookit을 호출 etc.

  * 의존성이 역전된 객체지향 구조: 프레임워크가 애플리케이션에 속하는 서브클래스의 메서드를 호출한다. 프레임워크로 제어 흐름의 주체가 이동(Inversion of Control)

    * 협력을 제어하는 것은 프레임워크이고, 개발자는 프레임워크가 적절한 시점에 실행할 것으로 예상되는 코드를 작성할 뿐. 프레임워크에 맞춘 오퍼레이션을 작성해야하지만 결정해야하는 설계 개념은 줄어들고 애플리케이션별로 구체적 오퍼레이션의 구현만 남게된다.

      

