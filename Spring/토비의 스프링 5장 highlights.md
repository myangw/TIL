# 토비의 스프링 5장 highlights

### Transaction 서비스 추상화

* UserService 테스트하기: 테스트에서 작업중간에 예외를 강제로 발생시키는게 필요하다면 -> UserService를 상속해서 테스트에 필요한 기능을 추가하도록 일부 메서드 override.
* DB가 하나의 SQL명령을 처리할 때는 그 자체로 완벽한 트랜잭션을 보장함.
* 트랜잭션 경계설정 : setAutoCommit(false)로 시작 선언 - commit() or rollback으로 종료
* 로컬 트랜잭션: 하나의 db커넥션 안에서 만들어짐 / 글로벌 트랜잭션: 여러 개의 db와의 작업을 하나의 트랜잭션으로
* **트랜잭션 동기화** - UserService에서 트랜잭션을 시작하기 위해 만든 Connection객체를 특별한 저장소에 보관해두고, 이후에 호출되는 dao 메서드에서는 저장된 Connection을 사용
  * `DataSourceUtils.getConnection(dataSource)`  : Connection객체를 저장소에 바인딩
* hibernate는 Connection을 직접 사용하지 않고 Session을 사용. 독자적인 트랜잭션관리 API를 사용
* 트랜잭션 경계설정을 담당하는 코드처럼, 일정한 패턴을 갖는 유사한 구조를 가진 코드 -> 추상화 !!
  * 애플리케이션 서비스로직과 서로 다른 계층
    * UserService, UserDao 등 애플리케이션 계층 / TransactionManager 등 서비스 추상화계층 / JDBC, Connection Pooling, ... 기술 서비스 계층