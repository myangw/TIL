# @Transactional 원리에 대한 간단한 메모



TranscationRequiredException을 만나 문제 해결 후 `@Transactional` 의 원리에 대해 더 알고싶어졌다.

- 요약: `@Transactional` 을 붙인 class or method는 AOP proxy 객체로 wrapping된다.
- 어노테이션을 붙이지 않았을 때는 caller → target Method로 바로 이어지지만, 어노테이션을 붙이고난 이후의 동작은 아래 그림처럼.

![@Transactional%20%E1%84%8B%E1%85%AF%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A6%20%E1%84%83%E1%85%A2%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%80%E1%85%A1%E1%86%AB%E1%84%83%E1%85%A1%E1%86%AB%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%86%E1%85%A6%E1%84%86%E1%85%A9%208b110debd7b7422eb26c60f30391f599/Untitled.png](@Transactional%20%E1%84%8B%E1%85%AF%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A6%20%E1%84%83%E1%85%A2%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%80%E1%85%A1%E1%86%AB%E1%84%83%E1%85%A1%E1%86%AB%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%86%E1%85%A6%E1%84%86%E1%85%A9%208b110debd7b7422eb26c60f30391f599/Untitled.png)

- 주의할 점은 외부 method call만 intercept되어 transaction이 동작한다는 것이다. target object 안에서 invoke하는 또다른 target object의 메서드는 proxy로 감싸진 메서드가 아니게된다. 그래서 transaction advisor의 영향을 받지 않는다. Transcation이 적용되기를 원한다면 self-invocation을 피하도록 리팩토링 해야함.

- 참고

[https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#tx-decl-explained](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#tx-decl-explained)  👍

[https://cheese10yun.github.io/spring-transacion-same-bean/](https://cheese10yun.github.io/spring-transacion-same-bean/) 

[https://stackoverflow.com/questions/1099025/spring-transactional-what-happens-in-background](https://stackoverflow.com/questions/1099025/spring-transactional-what-happens-in-background)