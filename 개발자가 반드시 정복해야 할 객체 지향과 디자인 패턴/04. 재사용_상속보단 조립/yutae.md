# 4. 재사용: 상속보단 조립

# 상속

상속을 사용하면 기존(상위) 클래스의 기능을 유지하면서 쉽게 다른 클래스(하위)의 기능을 재사용하면서 추가 기능을 확장 시킬 수 있음.

하지만 많은 단점이 존재함

## 1. 상위 클래스의 변경을 어렵게 만듬

어떠한 클래스를 상속받는다는건 그 클래스를 의존 한다는것임.

만약 의존 클래스(상위)가 변경되면 자기 자신(하위)와 자기 자신을 의존하는 또 다른 하위 클래스 까지 변경 해야하는 경우도 발생할 수 있음

## 2. 클래스의 불필요한 증가

상속만 사용하다보면 유사한 기능을 확장하는 과정에서 클래스의 개수가 불필요하게 증가 할 수 있음

상속만 사용한다면 예를 들어 Root라는 클래스를 B,C가 상속을 받았는데 B를 실행하고 C를 실행해야하는 상황과 C를 실행하고 B를 실행해야하는 상황이 나올 수 있음

## 3. 상속의 오용

상속 자체를 잘못 사용하는 경우가 발생 할 수 있음

정말 엉뚱한 클래스를 상속 할 경우 상속 클래스를 사용하는 입장에서 클래스를 사용할 때 난해 할 수 있음 ( 엉뚱한 클래스의 메서드 까지 함께 보여지기 떄문 )

# 조립을 이용한 재사용

객체 조립(composition)은 여러 객체를 묶어서 더 복잡한 기능을 제공하는 객체를 만드는것

객체 지향에서 조립이란 필드에서 다른 객체를 참조하는 방식으로 구현됨

```java
public class FlowController{
	private final Encryptor encryptor = new Encrytpto();

	public void process() {
		// ...
		byte[] encryptData = encryptor.encrypt(data);
		// ...
	}
}
```

조립을 통한 재사용은 상속을 통한 재사용에서 발생된 문제를 해결해줌

예를들어 위에서 말한 Root라 B,C를 상속 받은 후 기능에 따라 불필요한 클래스가 추가되는 상황을 언급한적이 있는데 조립을 이용하면 불필요한 클래스 방지가 가능함

```java
public class Root {
	private final B b;
	private final C c;
}
```

또 상속은 코드를 작성할 때 관계가 형성되는데 조립은 런타임도중에 상위 클래스를 교체할 수 있음

```java
//상속의 예
//스토리지가 A라는 알고리즘을가진 압축 클래스를 사용할떄
public class Stroage extends ACompressor() { }
// 이런식으로 상속을 하게되면 런타임 도중 변경 불가능

//반면 조립을 이용 한다면

public interface Compresor { }

public class AComporess implements Compressor {}

public class BCompress implements Compressor {}

public class Storage {
	private final Compressor compressor
	// 상황에 따라 원하는걸 사용할 수 있음
	public void doSomething() {
		if(something) {
			this.compressor = new ACompressor();
		} else {
			this.compressor = new BCompressor();
		}
	}
}
```

이렇듯 상속보단 조립을 사용하는게 좋음

# 위임

위임(delegation)은 내가 할 일을 다른 객체에게 넘긴다는 의미를 가지며 보통 조립 방식을 이용해서 위임을 구현함

# 그렇다면 상속은 언제 사용할까?

재사용이라는 관점이 아닌 기능의 확장이라는 관점에서는 상속을 사용해도됨

또한 추가로 `IS-A` 관계가 성립되어야함

## IS-A 관계

하위 클래스가 상위 클래스의 특성과 동작을 이어 받아, “하위 클래스는 상위클래스의 일종”이 된다는 관계

예시)

`Cat` 클래스가 `Animal` 클래스를 상속받는다면 고양이는 동물이다 (Cat is an Animal)이 되므로 관계가 성립되지만 펭귄 is fly은 펭귄은 날 수 없으니 상속을 받으면 안됨
