# 03. 타입과 추상화

리뷰기간: 2024년 10월 21일 → 2024년 10월 27일
태그: 1회독, 작성완료

## ✨ 내용 요약

- 사실적인 내용이 포함된 지하철 노선도는 오히려 승객들에게 불편함을 야기
    
    ⇒ 추상화를 통해 필요한 정보만 전달 ( 역과 역 사이의 순서, 환승 등등 )
    
- 추상화란?
    - 좀더 명확한 정보를 이해하기 위해 특정 절차나 물체를 의도적으로 생략하거나 감춤으로써 복잡성을 극복
    - 추상화는 사물의 본질에 집중하여 복잡성을 극복
    - 첫 번째 차원 : 구체적인 사물들 간의 공통점은 취하고 차이점을 버리는 `일반화`하여 단순화
    - 두번째 차원 : 중요한 부분을 강조하기 위해 `불필요한 세부 사항을 제거함`으로써 단순화
- 개념 이란?
    - 공통점을 기반으로 객체들을 묶기 위한 그릇
    - 객체를 여러 그룹으로 분류
    - 개념의 세가지 관점
        - 심볼 : 개념을 가리키는 간략한 이름 또는 명칭
        - 내연 : 개념의 완전한 정의, 객체가 개념에 속하는지 여부 파악
        - 외연 : 개념에 속하는 모든 객체의 집합
- 객체의 분류
    - 분류란?
        - 객체의 특정한 개념을 적용하는 작업
        - 첫번째 차원 (일반화) : 하트 왕, 하트 왕비, 정원사, 병사 등을 트럼프라는 개념으로 묶어 객체 간의 차이를 무시하고 공통점만 취함
        - 두번째 차원 (불필요한 세부사항 제거) : 트럼프 객체 집합의 공통점 중에서도 중요한 특징은 몸이 납작하고 네모난 모서리에 팔 다리가 달려있다는 것이다. 그 외의 사항들은 문맥상 어떤 도움도 되지 않기 때문에 불필요한 세부 사항을 제거
- 타입
    - 타입 = 개념
    - 데이터 타입
        - 메모리 내에 저장된 데이터의 종류를 분류하는데 사용하는 메모리 집합에 관한 메타데이터
        - 데이터에 대한 분류는 암시적으로 어떤 종류의 연산이 해당 데이터에 대해 수행될 수 있는지 결정
    - 객체의 타입
        - 어떤 객체가 어떤 타입에 속하는지 결정하는 것은 객체의 행동, 어떤 객체들이 동일한 행동을 수행할 수 있다면 동일 타입으로 분류
        - 객체의 내부적인 표현은 외부로부터 철저하게 감춰진다, 객체의 행동을 가장 효과적으로 수행할 수 있다면 객체 내부의 상태를 어떤 방식으로 표현해도 무방
- 행동
    - `다형성` : 동일한 타입에 속한 객체는 내부의 데이터 표현 방식이 다르더라도 동일한 메시지를 수신하고 이를 처리할 수 있다. 다형성이란 동일한 요청에 대해 서로 다른 방식으로 응답할 수 있는 능력을 뜻한다. 동일한 메시지를 서로 다른 방식으로 처리하기 위해서는 객체들은 동일한 메시지를 수신할 수 있어야하기 때문에 결과적으로 다형적인 객체들은 동일한 타입(또는 타입 계층)에 속하게 된다.
    - `캡슐화` : 데이터 내부 표현 방식과 무관하게 행동만이 고려 대상이라는 사실은 외부에 행동만을 제공하고 데이터는 감춰야 한다는 것을 의미한다. 이 원칙을 흔히 캡슐화라고 한다. 공용 인터페이스 뒤로 데이터를 캡슐화하라는 격언은 객체를 행동에 따라 분류하기 위해 지켜야 하는 기본적인 원칙이다. 데이터가 캡슐의 벽을 뚫고 객체의 인터페이스를 오염시키는 순간 객체의 분류 체계는 급격히 위험에 노출되고 결과적으로 유연하지 못한 설계를 낳는다.
    - `책임 주도 설계(Responsibility-Driven Design)` : 객체가 외부에 제공해야 하는 행동을 먼저 생각해야 한다. 이를 위해서는 객체가 외부에 제공해야 하는 책임을 결정하고 그 책임을 수행하는 데 적합한 데이터를 나중에 결정한 후, 데이터를 책임을 수행하는 데 필요한 외부 인터페이스 뒤로 캡슐화해야 한다. 이러한 설계 방법에 대해 프레임워크를 제공하는 것이 책임 주도 설계이다.
- 타입의 계층
    - 일반화 : 포괄적인 의미를 내품는 일반적인 개념 ( 트럼프 )
    - 특수화 : 일반적인 개념보다 범위가 더 좁은 특수한 개념 ( 트럼프 인간 )
    - 슈퍼타입과 서브타입
        - 슈퍼타입 : 일반적인 타입
        - 서브타입 : 특수한 타입
- 정적 모델
    - 타입은 시간에 따라 동적으로 변하는 객체의 상태를 시간과 무관한 정적인 모습으로 다룰 수 있게 해줌
    - 객체의 상태에 복잡성을 부과하는 시간이라는 요소를 제거하여 시간에 독립적인 정적 모습으로 객체를 생각할 수 있게 해줌
    - 타입은 시간에 따른 객체의 상태 변경이라는 복잡성을 단순화할 수 있는 효과적인 방법
- 동적 모델과 정적모델
    - 동적모델 : 객체가 특정 시점에 구체적으로 어떤 상태를 가지는 지 표현하는 스냅샷처럼, 객체가 살아 움직이는 동안 상태가 어떻게 변하고 어떻게 행동하는지를 포착하는 것
    - 정적모델 ( 타입 모델 ) :  객체가 가질 수 있는 모든상태와 모든 행동을 시간에 독립적으로 표현하는 것
- 클래스와 타입은 동일하지 않다
- 타입은 객체 분류를 위해 사용되는 개념이고, 클래스는 타입을 구현할 수 있는 여러 매커니즘 중 하나일 뿐

## 📝 감상 및 리뷰

- 내용이 점점 딥해진다고 생각 들음
- 타입을 행동 중심으로 바라보며 클래스와의 차이를 명확히 이해할 수 있었.
- 객체지향에서 설계의 근본을 다시 생각하게 해주었습니다

## 🛠️ 실무/프로젝트 적용

- 행동 중심의 타입 설계를 통해 프로젝트의 유지보수를 용이하게 하고, 코드의 유연성과 확장성을 확보

## 🔍 추가 자료

- 더 알아보고 싶은 내용은?