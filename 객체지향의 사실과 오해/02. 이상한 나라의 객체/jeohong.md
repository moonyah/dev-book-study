# 02. 이상한 나라의 객체

리뷰기간: 2024년 10월 14일 → 2024년 10월 20일
태그: 1회독, 작성완료

## ✨ 내용 요약

- 객객체지향 패러다임의 목적
    - 현실세계를 모방하는것이 아니라 현실 세계를 기반으로 `새로운 세계를 창조`
- 상태 와 행동은 상호 의존적이다
    - 상태를 알아야 행동을 할 수 있음
    - 행동의 결과는 상태에 따라 달라짐
- 서로 다른 객체가 협력을 하기 위해서는 `연결(링크)` 되어 있어야 함
- 객체의 상태
    - 프로퍼티(property) : 변경되지 않는 고정되어 있는것 ⇒ 정적(Static)
        - 단순한 값인 `속성`과 다른 객체를 가리키는 `링크`의 조합으로 표현
        - 키, 음료의 양, 위치
    - 프로퍼티 값 (property value) : 변경될수 있는 “값” ⇒ 동적(Dynamic)
        - 175cm, 50ml, 대한민국
- 캡슐화의 개념
    - 자신의 상태를 스스로 관리하는 자율적인 객체
    - 객체는 상태를 캡슐안에 감춰둔 채 외부로 노출하지 않음
    - 객체의 행동을 유발하는 것은 외부로부터 전달된 메시지 이지만, 상태를 변경할지 여부는 객체 스스로 결정
- 객체와 값의 차이
    - 객체 : 고유 `식별자`를 이용해 동등함 판별
        - 객체의 상태가 달라도 `식별자`가 같으면 동등
    - 값 : `상태`를 이용해 동등함 판별
        - 식별자가 다르더라도 `상태`가 같으면 동등
- 블랙박스의 개념
    - 조회를 의미하는 “쿼리”
    - 행동을 지시하는 “명령”
    - 명령을 할때 쿼리를 바로 보여주지 않아야함, 각각 독립적으로 존재
- 상태를 먼저 결정하고 행동을 나중에 결정하면 안된다
    - 상태를 먼저 결정할 경우 캡슐화 저해
    - 객체를 협력자가 아닌 고립된 섬으로 만듦
    - 객체의 재사용성 저하

## 📝 감상 및 리뷰

- 이상한 나라의 앨리스의 이야기로 객체의 특성 ( 상태, 행동, 책임, 상태와 행동의 의존성, 링크 등등을 설명하여 이해가 잘됨 )
- 상태와 행동은 상호 의존적임을 알게됨
- 사이드이펙트(Side Effect) 라는 단어를 알게모르게 쓰고있었지만 정확이 어떤뜻인지는 몰랐는데 이번 장을 읽으며 어떤 현상인지 알게됨
- 

## 🛠️ 실무/프로젝트 적용

- 캡슐화 + 블랙박스 공부
    
    ```swift
    class Calcurator {
        private var first: Double = 0
        private var second: Double = 0
        
        private var number: Double = 0
        
        func add(_ first: Double, _ second: Double) {
            self.first = first
            self.second = second
            
            self.number = first + second
        }
        
        func queryNumber() -> Double {
            return self.number
        }
    }
    
    let calcurator = Calcurator()
    
    print(calcurator.queryNumber()) // 0.0
    calcurator.add(1, 2)
    print(calcurator.queryNumber()) // 3.0
    ```
    
    ⇒ first, second, number 는 외부에 노출될 수 없음
    
    ⇒ add 함수를 이용해 (요청) 행동 요청 → 행동 요청의 결과를 바로 볼 수 없음
    
    ⇒ queryNumber 함수를 통해서만 number의 `상태` 를 조회 할 수 있음
    

- 객체와 값의 비교
    
    ```swift
    class Calcurator {
        private var number: Double = 0
        
        func queryNumber() -> Double {
            return self.number
        }
    }
    
    let calcurator = Calcurator()
    let calcurator2 = Calcurator()
    
    print(calcurator === calcurator2) 
    // false => "식별자"가 다름
    
    print(calcurator.queryNumber() == calcurator2.queryNumber())
    // true => "식별자"가 다르지만 "상태"가 같음
    
    ```
    

### 실무에서 적용된 예시

CustomPopup을 띄우기 위해 하기 코드를 작성 한 경험이 있음

```swift
// customPopup띄우는 한 객체의 일부 코드
class PopupView {
// ... BlackBox

func showPopupSingleButton(on view: UIView, message: String, confirmAction: @escaping () -> Void = { return }) {
        if singleButtonPopup == nil {
            singleButtonPopup = SingleButtonPopupView(frame: .zero)
        }
        
        singleButtonPopup?.messageLabelSetting(message: message)
        singleButtonPopup?.show(on: view)
        singleButtonPopup?.confirmAction = confirmAction
    }
    
  // ... BlackBox
}
    
    
    
// 사용하는 쪽
class MainViewController {
// ... BlackBox

PopupView.showPopupSingleButton(on: self?.view ?? UIView(), message: "다시 시도해 주세요.")

// ...BlackBox

}
```

- MainViewController 는 함수(메세지)를 통해 popupView객체에게 “다시 시도해 주세요.” 라는 `행동을 요청함`
- showPopupSingleButton() 함수는 메세지를 수신한 후 `상태를 결정`(팝업을 띄움) 함
- 위의 과정에서 MainViewController는 “다시 시도해 주세요.” 문구의 팝업을 띄워줘 라고 요청을 한 후, 그에 대한 결과(상태)에 대해 관여를 하지 않음 PopupView는 요청메세지를 수신한 후 내부에서 자율적으로 자신의 상태를 변경하고 요청에 대한 상태를 결정함

## 🔍 추가 자료

- 함수에서 리턴값으로 변경된 “상태”를 바로 읽어드리는 것은 올바른 객체지향적 설계가 아닌것인가?
- RDD(책임-주도 설계_Responsibility-Driven Design)