### 1. 상속 개요

---

```java
public class Coupon {
  private int discountAmount; // 하위 클래스에서는 접근 불가
  public Coupon(int discountAmount) {
    this.discountAmount = discountAmount;
  }
  public int getDiscountAmount(){ // 자식 클래스에서도 사용 가능
    return discountAmount;
  }
  public int calculateDiscountedPrice(int price) {
    if(price < discountAmount)
      return 0;
    return price - discountAmount;
  }
}
```

Coupon 클래스를 상속받아 새로운 클래스를 만드는 예

```java
public class LimitPriceCoupon extends Coupon {
  private int limitPrice;
  public LimitPriceCoupon(int limitPrice, int discountAmount) {
    super(discountAmount);
    this.limitPrice = limitPrice;
  }
  public int getLimitPrice(){
    return limitPrice;
  }

  @Override
  public int calculateDiscountedPrice(int price) { // 재정의
    if(price < limitPrice)
      return price;
    return super.calculateDiscountedPrice(price);
  }
}
```

- 상위(super) 클래스 또는 부모 클래스: 상속 대상이 되는 클래스(여기에서는 Coupon 클래스)

- 하위 클래스 또는 자식 클래스: 상속을 받는 클래스

- 재정의: 상위 클래스에서 정의된 메서드를 새롭게 구현

### 2. 다형성과 상속

---

- 다형성: 한 객체가 여러 가지 모습(타입)을 갖는다는 것을 의미, 한 객체가 여러 타입을 가질 수 있다는 것을 뜻한다.

- 객체는 여러 타입에 정의된 기능을 제공할 수 있고 자바와 같은 정적 타입 언어에서는 타입 상속을 통해 구현한다.

타입 상속을 통한 다형성 구현

```java
public class Plane {
  public void fly(){
    // 비행
  }
}

interface Turbo {
  public void boost();
}

class TurboPlane extends Plane implements Turbo{ // 상속, 상속
  public void boost(){
    // 가속
  }
}
```

TurboPlane 타입의 객체를 Plane 타입이나 Turbo 타입에 할당하는 것도 가능하다.

```java
TurboPlane tp = new TurboPlane();

Plane p = tp;
p.fly();

Turbo t = tp;
t.boost();
```

타입 상속

1. 인터페이스 상속: 타입 정의만을 상속받는 것

   ex) 추상 함수만을 가진 추상 클래스를 상속받는 경우

   TurboPlane 클래스는 상속받은 인터페이스에 정의된 메서드를 구현한다.

2. 구현 상속: 상위 클래스에 정의된 기능을 재사용하기 위한 목적으로 사용된다.

#### 🤔 Plane p = new TurboPlane();과 TurboPlane p = new TurboPlane();의 차이는?

```java
class Plane {
    public void fly() {
        System.out.println("Plane is flying");
    }
}

class TurboPlane extends Plane {
    public void fly() {
        System.out.println("TurboPlane is flying with extra speed!");
    }

    public void turboBoost() {
        System.out.println("TurboBoost activated!");
    }
}
```

```java
Plane p = new TurboPlane();
p.turboBoost();  // 컴파일 오류: Plane 클래스에 turboBoost()가 없음

TurboPlane p = new TurboPlane();  // p의 타입이 TurboPlane
p.turboBoost();  // TurboPlane 클래스에 정의된 turboBoost() 메서드를 호출할 수 있음
```

### 3. 추상 타입과 유연함

---

- 추상화: 데이터나 프로세스 등을 의미가 비슷한 개념이나 표현으로 정의하는 과정

  예를 들어

  1.  FTP에서 파일 다운

  2.  소켓에서 데이터 읽기

  3.  DB 테이블의 데이터를 조회

위 세 기능을 추상화하면 `로그 수집`이라는 개념을 정의 가능

추상화된 타입은 오퍼레이션의 시그니터만 정의할 뿐, 실제 구현을 제공하지는 못한다.

(추상화를 한다고 해서 반드시 추상 타입을 만들어야 하는 것은 아님! 오해하지 말기)

![Image](https://github.com/user-attachments/assets/d08da30a-1748-4a37-b295-dbf96b908d45)

상속을 이용해 추상 타입을 실제 구현 클래스로 연결하면 다음과 같이 추상 타입을 이용해서 코드 작성 가능

각 하위 타입들을 모두 상위 타입인 LogCollector 인터페이스에 정의된 기능을 실제로 구현하는데, 이들 클래스들은 실제 구현을 제공한다는 의미에서 `콘크리트 클래스`라고 부른다.

#### 🤔 왜 추상 타입을 사용하는 걸까?

암호화 프로그램에서 콘크리트 클래스를 직접 사용하는 경우, 요구 사항 변화가 생길 때마다 FlowController(흐름 제어)의 코드 변경이 이루어진다.

예를 들어,

기존의 파일에서 바이트 데이터 읽기에서 소켓에서 바이트 데이터 읽기가 요구 사항이 추가됐다면

```java
import java.net.Socket;

public class FlowController {
  private boolean useFile;

  public FlowController(boolean useFile) {
    this.useFile = useFile;
  }

  public void process(){
    byte[] data = null;
    if(useFile) {
      FileDataReader fileReader = new FileDataReader();
      data = fileReader.read();
    } else {
      SocketDataReader SocketReader = new SocketDataReader();
      data = socketReader.read();
    }

    Encryptor encryptor = new Encryptor();
    byte[] encryptedData = encryptor.encrypt(data);

    FileDataWriter  writer = new FileDataWriter();
    writer.write(encryptedData);
  }
}
```

`어떤 곳으로부터 바이트 데이터 읽기`로 추상화한다.

추상 타입 만들기

```java
public interface ByteSource {
  public byte[] read();
}
```

ByteSource를 상속받도록 변경해주기

```java
public class FileDataReader implements ByteSource {
	public byte[] read(){

    }
}

public class SocketDataReader implements ByteSource {

}
```

바이트 데이터 읽기 추상화 적용

```java
ByteSource source = null;
if(useFile)
   source = new FileDataReader();
else
   source = new SocketDataReader();

byte[] data = source.read();
```

ByteSource 타입 객체 생성 부분이 동일하기 때문에 ByteSoureFactory 클래스를 만들어서 ByteSource 타입 객체 생성하는 책임을 부여!

```java
public class ByteSourceFactory {
  public ByteSource create() {
  	if(useFile())
    	return new FileDataReader();
    else
        return new SocketDataReader();
  }
  // ...
}
```

이제 이 클래스도 반영해서 FlowController를 수정하면...

```java
public class FlowController {
  public void process(){
    ByteSource source = ByteSourceFactory.getInstance().create();
    byte[] data = source.read();

    Encryptor encryptor = new Encryptor();
    byte[] encryptedData = encryptor.encrypt(data);

    FileDataWriter  writer = new FileDataWriter();
    writer.write(encryptedData);
  }
}
```

이제 추가 요구 사항이 들어와도 FlowController 코드는 영향을 받지 않고 ByteSourceFactory만 변경된다.

#### 추상화 과정을 통해 얻은 유연함 정리

1. ByteSource 종류 변경되면, ByteSourceFactory만 변경됨

2. FlowController 제어 흐름 변경 시, ByteSource 객체 생성 부분은 영향을 주지 않음

객체를 생성하는 책임과 흐름을 제어하는 책임을 분리하였다.

추상화는 공통된 개념을 도출해 추상 타입을 정의해 주기도 하지만, 많은 책임을 가진 객체로부터 책임을 분리하는 촉매제가 되기도 함, 변경의 유연함을 증가시켜 준다.

#### 경험하지 않은 분야라 하더라도 추상화하는 방법

- 변경되는 부분을 추상화한다. (변화되는 부분이 추상화 후보)

- 추상화가 되어 있지 않은 코드는 주로 동일 구조를 갖는 if-else 블록으로 드러남

추상화를 하면 추상 타입을 사용하는 코드에 영향을 주지 않으면서 추상 타입의 실제 구현을 변경할 수 있다는 것을 뜻한다.

#### 인터페이스에 대고 프로그래밍하기

- 여기서 말하는 인터페이스란 오퍼레이션을 정의한 인터페이스

- 인터페이스는 새롭게 발견된 추상 개념을 통해서 도출되는 것

`주의할 점`) 유연함을 얻는 과정에서 타입(추상 타입)이 증가하고 구조도 복잡해지기 때문에 모든 곳에서 인터페이스를 사용해서는 안 된다. 인터페이스를 사용할 때는 변화 가능성이 높은 경우에 한해서 사용해야 함

인터페이스를 작성할 때, 그 인터페이스를 사용하는 코드 입장에서 작성해야 한다.

#### 인터페이스와 테스트

테스트할 때도, FileDataReader 클래스 구현이 완성되지 않았더라도 FlowController 클래스 테스트 가능

```java
public void testProcess() {
  ByteSource mockSource = new MockByteSource();
  // ByteSource 객체로 FileDataReader 대신 MockByteSource 사용

  FlowController fc = new FlowController();
  fc.process();

  // 결과가 정상적으로 만들어졌는지 확인하는 코드
  ...
}

class MockByteSource implements ByteSource {
  public byte[] read() {
    byte[] data = new byte[128];
    // data를 테스트 목적의 데이터로 초기화
    return data;
  }
}
```

- Mock 객체를 사용함으로써 실제 사용할 콘크리트 클래스의 구현 없이 테스트 가능하다.

- 사용할 대상을 인터페이스로 추상화하면 좀 더 쉽게 Mock 객체를 만들 수 있고 사용할 코드의 완성을 기다릴 필요 없이 내가 만든 코드를 먼저 빠르게 테스트 가능하다.

#### 테스트 주도 개발 (TDD, Test Driven Development)

- 테스트 코드를 먼저 작성하고 실제 코드를 작성하는 방법으로 개발하는 기법

테스트를 강제함으로써 알맞은 책임을 가진 객체를 도출하도록 유도한다.
