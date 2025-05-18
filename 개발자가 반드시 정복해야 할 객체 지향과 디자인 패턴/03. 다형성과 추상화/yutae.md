# 상속

상속은 한타입을 그대로 사용하면서 구현을 추가 할 수 있도록 해주는 방법을 제공함

# Coupon 클래스르 활용한 상속의 예제 

```java
public class Coupon {
    private int discountAmount; 
    public Coupon(int discountAmount) {
        this.discountAmount = discountAmount;
    }
    public int getDiscountAmount() { 
        return discountAmount;
    }
    public int calculateDiscountedPrice(int price) {
        if(price < discountAmount) {
        return 0;
        }
        return price - discountAmount;
    }
}
```

해당 Coupon 클래스 기반으,로상품 그액이 지정 한 금액 이상인 경우에만 할인 받을 수 있도록 하는 쿠폰을 구현하고 싶을 때 기존 쿠폰 클래스를 상속받아 새로운 기능의 클래스를 정의 할 수 있음

```java 
public class LimitPriceCoupon extends Coupon {
    private int limitPrice;

    public LimitPriceCoupon(int limitPrice, int discountAmount) {
        super(discountAmount);
        this.limitPrice = limit
    }

    public int getLimitPrice() {
        return limitPrice;
    }
}
```

이 때 상속이 되는 클래스를 상위 (super) 클래스 또는 부모 (parent) 클래스라 부르며 상속받는 클래스는 하위(sub) 클래스 또는 자식 (child) 클래스라 부름.

자식 클래스는 부모 클랫으ㅔ 정의된 구현을 물려받게됨.

또 하위 클래스는 상위 클래스에서 정의된 메서드를 `오버라이딩(overrding)`이라는 기법을 이용해 새롭게 정의 가능 


```java 
public class LimitPriceCoupon extends Coupon {
    private int limitPrice;

    public LimitPriceCoupon(int limitPrice, int discountAmount) {
        super(discountAmount);
        this.limitPrice = limit
    }

    public int getLimitPrice() {
        return limitPrice;
    }

    @Override
    public int calculateDiscountedPrice(int price) {
        if(price < limitPrice) { // 제품 가격이 제한 금액보다 작으면
            return price;
        }
        return super.calculateDiscountedPrice(price);
    }
}
```
위 처럼 메서드 오버라이딩을 하게되면 상위 타입에 메서드가아닌 하위 타입의 메서드가 실행도미

# 다형성과 상속

다형성(Polymorphism)은 한객체가 여러가지 모습을 갖는다는 것을 의미함.
즉 다형성이란 한 객체가 여러 탕비을 가질 수 있는 것을 뜻함.

자바와 같은 정적 타입 언어에서는 타입 상속을 통해서 다형성을 구성함.

```java
public class Plane() {
    public void fly() {
        // 비행
    }
}

public class Turbo {
    public void boost();
}

public class TurboPlane extends Plane implements Turbo  {
    public void boost() {

    }
}
```

Turboplane 탑이의 객체에 Plane 타입이나 Turbo 타입에 정의된 메서드의 샐행 요청가능

```java 
TurboPlane tp = new TurboPlane();
tp.fly();
tp.boost();
```

반대로 TurboPlane 타입의 객체를 Plane 타입이나 Turbo 타입에 할당하는것도 가능

```java
TurboPlane tp = new TurboPlane();

Plane p = tp; // TurboPlane 객체는 Plane 타입도 됨
p.fly;

Turbo t = tp; // TurboPlane 객체는 Turbo 타입도 됨
t.boost; 
```

# 추상 타입과 유연함 

추상화는 데이터나 프로세스 등을 의미가 비슷한 개념이나 표현으로 정의하는 과정을 말함.

컴퓨터 분야에서는 이런 추상화는 매우 광범위하게 사용되며 타입도 추상화 대상이 된다.

추상화를 한다해서 반드시 추상 타입을 만들어야하는것은 절 떄 아님


// TODO 추후 작성 ㅠㅠ
