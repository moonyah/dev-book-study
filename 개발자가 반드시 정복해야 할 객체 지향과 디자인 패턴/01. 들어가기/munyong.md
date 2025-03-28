#### 소프트웨어 설계가 엉망일 경우

시간이 지날수록 코드 변경이 어렵고 간단한 요구 사항조차 개발이 안된다.
그래서 개발자는 변화에 대응할 수 있는 구조 설계를 해야 한다.

## 1. 지저분해지는 코드

<p align="center">
  <img src="https://github.com/user-attachments/assets/0d8c8b6c-7f06-432e-906e-7839e042990e" width="500">
</p>

메뉴 영역에 1, 2, .... 이렇게 있고 공통 영역 버튼 1, 2, ... 이렇게 있는 경우에

```java
public class Main {
  public static void main(String[] args) {
    Application app = new Application();

    // menu1 클릭
    app.menu1.click();

    // button1 클릭
    app.button1.click();

    // menu2 클릭
    app.menu2.click();

    // button1 클릭
    app.button1.click();
  }
}

// 클릭 이벤트를 처리할 인터페이스
interface OnClickListener {
  void clicked(Component eventSource);
}

// 공통 컴포넌트 클래스
class Component {
  private String id;
  private OnClickListener listener;

  public Component(String id) {
    this.id = id;
  }

  public String getId() {
    return id;
  }

  public void setOnClickListener(OnClickListener listener) {
    this.listener = listener;
  }

  public void click() {
    if (listener != null) {
      listener.clicked(this);
    }
  }
}

// 메뉴 클래스
class Menu extends Component {
  public Menu(String id) {
    super(id);
  }
}

// 버튼 클래스
class Button extends Component {
  public Button(String id) {
    super(id);
  }
}

// Application 클래스
class Application implements OnClickListener {
  public Menu menu1 = new Menu("menu1");
  public Menu menu2 = new Menu("menu2");
  public Button button1 = new Button("button1");

  private String currentMenu = null;

  public Application(){
    menu1.setOnClickListener(this);
    menu2.setOnClickListener(this);
    button1.setOnClickListener(this);
  }

  public void clicked(Component eventSource){
    if(eventSource.getId().equals("menu1")) {
      changeUIToMenu1();
    } else if(eventSource.getId().equals("menu2")) {
      changeUIToMenu2();
    } else if(eventSource.getId().equals("button1")) {
      if(currentMenu == null)
        return;
      if (currentMenu.equals("menu1"))
        processButton1WhenMenu1();
      else if(currentMenu.equals("menu2"))
        processButton1WhenMenu2();
    }
  }

  private void changeUIToMenu1(){
    currentMenu = "menu1";
    System.out.println("메뉴1 화면으로 전환");
  }

  private void changeUIToMenu2(){
    currentMenu = "menu2";
    System.out.println("메뉴2 화면으로 전환");
  }

  private void processButton1WhenMenu1(){
    System.out.println("메뉴1 화면의 버튼1 처리");
  }

  private void processButton1WhenMenu2(){
    System.out.println("메뉴2 화면의 버튼1 처리");
  }
}
```

여기에서 Application 클래스만 살펴 볼 것이다.

### 여기에서 menu추가, button 추가 요구 사항이 들어올 경우

```java
public void clicked(Component eventSource){
  if(eventSource.getId().equals("menu1")) {
    changeUIToMenu1();
  } else if(eventSource.getId().equals("menu2")) {
    changeUIToMenu2();
  } else if(eventSource.getId().equals("menu3")) {
    changeUIToMenu3();
  } else if(eventSource.getId().equals("button1")) {
    if(currentMenu == null)
      return;
    if (currentMenu.equals("menu1"))
      processButton1WhenMenu1();
    else if(currentMenu.equals("menu2"))
      processButton1WhenMenu2();
    else if(currentMenu.equals("menu3"))
      processButton1WhenMenu3();
  } else if(eventSource.getId().equals("button2")) {
    // ...
  } else if(eventSource.getId().equals("button3")) {
    // ...
  }
}
```

if-else 블록이 점점 커지면서 복잡해지고 수정이 오래걸린다.

> 🚨 "초기에는 새로운 요구 사항을 빠르게 개발해 주었는데, 시간이 지날수록 간단한 요구 사항 조차도 제 때 개발이 안 되는" 상황 발생

## 2. 수정하기 좋은 구조를 가진 코드

#### 각 메뉴를 선택했을 때 동일하게 동작되는 것 정리

1. 메뉴가 선택되면 해당 화면을 보여준다.
2. 버튼1을 클릭하면 선택된 메뉴 화면에서 알맞은 처리를 한다.

### 공통된 동작 정의 (ScreenUI 인터페이스 정의)

```java
public interface ScreenUI {
  public void show();
  public void handleButton1Click(); // 버튼1이 눌렀을 때 실행됨
}
```

메뉴 별로 ScreenUI 인터페이스를 구현한 클래스 작성

> ### [참고]
>
> 자바는 하나의 .java 파일 안에는 public 클래스를 하나만 허용한다.
> 테스트용이거나 한 파일에 다 넣고 싶다면, public 키워드를 제거하자.
> 실제 프로젝트처럼 구조화하고 싶다면 파일 나누기
> 예를 들면,
>
> - ScreenUI.java → public interface ScreenUI
> - Menu1ScreenUI.java → public class Menu1ScreenUI implements ScreenUI
> - Application.java → public class Application

### 수정 후의 코드

```java
//  공통된 동작 정의 (ScreenUI 인터페이스 정의)
interface ScreenUI {
  public void show();
  public void handleButton1Click(); // 버튼1이 눌렀을 때 실행됨
}
class Menu1ScreenUI implements ScreenUI {
  public void show() { System.out.println("메뉴1 화면으로 전환");}
  public void handleButton1Click() {
    System.out.println("메뉴1 화면의 버튼1 처리");
  }
}
class Menu2ScreenUI implements ScreenUI {
  public void show() { System.out.println("메뉴2 화면으로 전환");}
  public void handleButton1Click() {
    System.out.println("메뉴2 화면의 버튼1 처리");
  }
}

// ScreenUI 인터페이스, Menu1ScreenUI & Menu2ScreenUI 클래스를 이용해서
// Application 클래스 수정
class Application implements OnClickListener {
  public Menu menu1 = new Menu("menu1");
  public Menu menu2 = new Menu("menu2");
  public Button button1 = new Button("button1");

  private ScreenUI currentScreen = null; // ScreenUI 타입으로 변경

  public Application(){
    menu1.setOnClickListener(this);
    menu2.setOnClickListener(this);
    button1.setOnClickListener(this);
  }

  public void clicked(Component eventSource){
    String sourceId = eventSource.getId();
    if(sourceId.equals("menu1")) {
      currentScreen = new Menu1ScreenUI(); // 객체 할당됨
      currentScreen.show();
    } else if(sourceId.equals("menu2")) {
      currentScreen = new Menu2ScreenUI();
      currentScreen.show();
    }else if(sourceId.equals("button1")) {
      if(currentScreen == null)
        return;
      currentScreen.handleButton1Click(); // 할당된 객체의 메서드가 호출됨
    }
  }
}
```

> 💡 button1 처리 코드에서 현재 어쩐 메뉴 화면인지 상관없이 currentScreen.handleButton1Click()을 실행한다는 것!

### 메뉴 클릭 처리 코드와 버튼 클릭 처리 코드 분리하기

> 두 작업을 더 잘 구분하기 위해

```java
private OnClickListener menuListener =
  new OnClickListener() {
    public void clicked(Component eventSource){
      String sourceId = eventSource.getId();
      if(sourceId.equals("menu1")) {
        currentScreen = new Menu1ScreenUI();
      }else if(sourceId.equals("menu2")) {
        currentScreen = new Menu2ScreenUI();
      }
      currentScreen.show();

    }
  };

  private OnClickListener buttonListener =
  new OnClickListener() {
    public void clicked(Component eventSource){
      if(currentScreen == null)
        return;
      String sourceId = eventSource.getId();
      if(sourceId.equals("button1")) {
        currentScreen.handleButton1Click();
      }
    }
  };
```

### 버튼2 추가하기

```java
interface ScreenUI {
  public void show();
  public void handleButton1Click();
  public void handleButton2Click(); // 추가
}

class Menu1ScreenUI implements ScreenUI {
  public void show() { System.out.println("메뉴1 화면으로 전환");}
  public void handleButton1Click() {
    System.out.println("메뉴1 화면의 버튼1 처리");
  }
   public void handleButton2Click() { // 추가
    System.out.println("메뉴1 화면의 버튼2 처리");
  }
}
class Menu2ScreenUI implements ScreenUI {
  public void show() { System.out.println("메뉴2 화면으로 전환");}
  public void handleButton1Click() {
    System.out.println("메뉴2 화면의 버튼1 처리");
  }
   public void handleButton2Click() { // 추가
    System.out.println("메뉴2 화면의 버튼2 처리");
  }
}

// buttonListener만 수정해주면 됨
 private OnClickListener buttonListener =
  new OnClickListener() {
    public void clicked(Component eventSource){
      if(currentScreen == null)
        return;
      String sourceId = eventSource.getId();
      if(sourceId.equals("button1")) {
        currentScreen.handleButton1Click();
      } else if(sourceId.equals("button2")) { // 추가
        currentScreen.handleButton2Click();
      }
    }
  };
```

### 메뉴3 추가하기

```java
// Menu3ScreenUI 추가
class Menu3ScreenUI implements ScreenUI {
  public void show() { System.out.println("메뉴3 화면으로 전환");}
  public void handleButton1Click() {
    System.out.println("메뉴3 화면의 버튼2 처리");
  }
   public void handleButton1Click() {
    System.out.println("메뉴3 화면의 버튼2 처리");
  }
}
```

```java
private Menu menu3 = new Menu("menu3");
```

```java
memu3.setOnClickListener(menuListener);
```

```java
// menuListener만 수정해주면 됨
private OnClickListener menuListener =
  new OnClickListener() {
    public void clicked(Component eventSource){
      String sourceId = eventSource.getId();
      if(sourceId.equals("menu1")) {
        currentScreen = new Menu1ScreenUI();
      }else if(sourceId.equals("menu2")) {
        currentScreen = new Menu2ScreenUI();
      } else if(sourceId.equals("menu3")) { // 추가
        currentScreen = new Menu3ScreenUI();
      }
      currentScreen.show();
    }
  };
```

#### 수정한 코드의 장점 정리

1. 새로운 메뉴 추가 시에 버튼 처리 코드가 영향을 받지 않는다.
2. 한 메뉴 관련 코드가 한 개의 클래스로 모여서 코드 분석/수정이 용이
3. 서로 다른 메뉴에 대한 처리 코드가 섞여 있지 않다.

## 3. 소프트웨어의 가치

- 새로운 요구 사항을 적용하기 어려우면 소프트웨어는 죽음으로 이어질 수 있다.
- 변화 가능한 유연한 구조를 만들어 주는 핵심 기법 중 하나는 객체 지향(Object Oriented)이다.
