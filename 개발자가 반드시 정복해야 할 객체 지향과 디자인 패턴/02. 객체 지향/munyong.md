### 1. 절차 지향과 객체 지향

---

#### 절차 지향 프로그래밍

- 프로시저로 프로그램을 구성하는 기법
- 데이터를 중심으로 한 프로시저로 구성된다.
- 데이터를 사용하는 프로시저를 수정할 경우에는 이 데이터를 사용하는 모든 프로시저도 함께 수정해 주어야 함
- 새로운 요구 사항에 따른 코드 수정이 어렵고 새로운 기능 추가 시에 많은 구현 시간(개발 비용)이 든다.

#### 객체 지향 프로그래밍

- 데이터 및 데이터와 관련된 프로시저들을 객체 단위로 묶는다.
- 각 객체들을 자신만의 데이터와 프로시저를 갖고 다른 객체에 기능을 제공하기 위해 프로시저를 사용하는데, 이때 프로시저는 자신이 속한 객체의 데이터에만 접근 가능
- 요구사항 변화 시, 더 쉽게 변경 가능

### 2. 객체

---

객체를 정의할 때 사용되는 것은 객체가 제공해야 할 기능
예를 들어 소리 크기 제어 객체라고 하면 `1)소리 크기 증가` `2)소리 크기 감소` `3)음 소거` 라는 세 개의 기능을 제공한 다는 것이 중요할 뿐이다.

- 오퍼레이션: 객체가 제공하는 기능
- 시그니처: 오퍼레이션의 사용법 (기능 식별 이름, 파라미터 및 파라미터 타입, 기능 실행 결과 값)
- 인터페이스: 객체가 제공하는 모든 오퍼레이션의 집합 (타입으로 구분), 객체가 제공하는 기능에 대한 명세서
- 클래스: 실제 객체의 구현을 정의하는 것
- 인스턴스: 메모리에 생성된 객체
- 메시지: 오퍼레이션의 실행을 요청하는 것

### 3. 객체의 책임과 크기

---

각 객체는 자신만의 책임이 있다.

- 타입/인터페이스: 한 객체가 갖는 책임을 정의한 것
  책임 결정하기

1. 기능 목록 정리
2. 기능들을 객체들에게 분배하기 (책임 분배)

- 객체가 갖는 책임의 크기는 작을 수록 좋다. (단일 책임의 원직 SRP)
- 객체가 갖는 책임의 크기가 커질수록 절차 지향적으로 구조가 변질되며, 기능 변경 어려움의 문제(경직성 문제) 발생
- 책임의 크기를 작게 할수록 변화에 유리하다.

### 4. 의존

---

- 다른 타입에 의존하다는 것은 의존하는 타입 변경 발생 시에 자신도 함께 변경 가능성이 높다는 뜻
- 순환 의존은 변경의 여파가 나 자신에게 다시 영향을 줄 수 있다.

### 5. 캡슐화

---

- 객체가 내부적으로 기능을 어떻게 구현하는지를 감추는 것
- 내부 구현 변경의 유연함을 주는 기법

ex) 서비스 만료 날짜 여부 확인 코드
만료 여부 확인 구현을 캡슐화한다. (Member 클래스에 isExpired() 구현)

```java
if (member.isExpired()){
 // 만료에 따른 처리
}
```

위의 isExpired() 메서드를 사용한 곳은 수정하지 않아도 된다. (캡슐화된 기능을 사용하는 코드의 영향 없음)

#### [캡슐화를 위한 두 개의 규칙]

1. Tell, Don't Ask: 데이터를 물어보지 않고, 기능을 실행해 달라고 말하기
2. 데미테르의 법칙

- 메서드에서 생성한 객체의 메서드만 호출
- 파라미터로 받은 객체의 메서드만 호출
- 필드로 참조하는 객체의 메서드만 호출

### 6. 객체 지향 설계 과정

---

1. 제공해야 할 기능을 찾고 또는 세분화하고 그 기능을 알맞게 객체에 할당

- 기능을 구현하는데 필요한 데이터를 객체에 추가
- 기능은 최대한 캡슐화해서 구현

2. 객체 간의 어떻게 메시지를 주고받을 지 결정

- 구현 과정에서 한 클래스에 여러 책임이 섞여 있으면 객체를 새로 만들어 책임을 분리하게 됨

캡슐화는 내부 구현에 유연함을 제공해 주는 기법
