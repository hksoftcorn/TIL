# JAVA2 - 생활코딩

>JAVA2 - 객체지향 | 상속 | 예외



### 객체지향프로그래밍

##### 객체

객체는 관련있는 `변수와 메소드`를 그룹핑한 것이다.

##### 추상화

좋은 객체를 만드는 법이다. 이것을 다른 말로는 설계를 잘하는 법이라고 할 수 있다. 좋은 설계는 현실을 잘 반영해야 한다. 현실은 복잡하다. 하지만 그 복잡함 전체가 필요한 것은 아니다. `복잡함 속에서 필요한 관점만을 추출하는 행위를 추상화라고 한다.`

##### 부품화

메소드는 부품화의 예라고 할 수 있다. 메소드를 사용하는 기본 취지는 연관되어 있는 로직들을 결합해서 메소드라는 완제품을 만드는 것이다. 그리고 이 메소드들을 부품으로 해서 하나의 완제품인 독립된 프로그램을 만드는 것이다. 메소드를 사용하면 코드의 양을 극적으로 줄일 수 있고, 메소드 별로 기능이 분류되어 있기 때문에 필요한 코드를 찾기도 쉽고 문제의 진단도 빨라진다.

객체 지향 프로그래밍이다. 이것의 핵심은 연관된 메소드와 그 메소드가 사용하는 변수들을 분류하고 그룹핑하는 것이다. 바로 그렇게 그룹핑 한 대상이 객체(Object)다. 비유하자면 파일과 디렉토리가 있을 때 메소드나 변수가 파일이라면 이 파일을 그룹핑하는 디렉토리가 객체라고 할 수 있다. 이를 통해서 더 큰 단위의 부품을 만들 수 있게 되었다. 객체를 만드는 법에 대해서 호기심이 생기지 않는가? 

##### 은닉화, 캡슐화

제대로된 부품이라면 그것이 어떻게 만들어졌는지 모르는 사람도 그 부품을 사용하는 방법만 알면 쓸 수 있어야 한다. 즉 내부의 동작 방법을 단단한 케이스 안으로 숨기고 사용자에게는 그 부품의 사용방법만을 노출하고 있는 것이다. 이러한 컨셉을 정보의 은닉화(Information Hiding), 또는 캡슐화(Encapsulation)라고 부른다. 

##### 인터페이스

미리 정해진 약속에 따라서 신호를 입, 출력하고, 연결점의 모양을 표준에 맞게 만들면 된다. 이러한 연결점을 인터페이스(interface)라고 한다. 즉 인터페이스는 부품들 간의 약속이다. 



### 객체화

##### 클래스와 인스턴스

```java
class Calculator{
    int left, right;
      
    public void setOprands(int left, int right){
        this.left = left;
        this.right = right;
    }
      
    public void sum(){
        System.out.println(this.left+this.right);
    }
      
    public void avg(){
        System.out.println((this.left+this.right)/2);
    }
}
  
public class CalculatorDemo4 {
      
    public static void main(String[] args) {
          
        Calculator c1 = new Calculator(); // 데이터타입 Calculator 
        c1.setOprands(10, 20);
        c1.sum();       
        c1.avg();       
          
        Calculator c2 = new Calculator();
        c2.setOprands(20, 40);
        c2.sum();       
        c2.avg();
    }
}
```

##### 클래스 멤버와 인스턴스 멤버

```java
class Calculator2 {
    // static을 맴버(변수,메소드) 앞에 붙이면 클래스의 맴버가 된다.
    static double PI = 3.14;
    // 클래스 변수인 base가 추가되었다.
    static int base = 0;
    int left, right;
 
    public void setOprands(int left, int right) {
        this.left = left;
        this.right = right;
    }
 
    public void sum() {
        // 더하기에 base의 값을 포함시킨다.
        System.out.println(this.left + this.right + base);
    }
 
    public void avg() {
        // 평균치에 base의 값을 포함시킨다.
        System.out.println((this.left + this.right + base) / 2);
    }
}
 
public class CalculatorDemo2 {
 
    public static void main(String[] args) {
 
        Calculator2 c1 = new Calculator2();
        c1.setOprands(10, 20);
        c1.sum(); // 30 출력
 
        Calculator2 c2 = new Calculator2();
        c2.setOprands(20, 40);
        c2.sum(); // 60 출력
 
        // 클래스 변수 base의 값을 10으로 지정했다.
        Calculator2.base = 10;
 
        c1.sum(); // 40 출력
        c2.sum(); // 70 출력
 
    }
 
}
```

>  ✅ TIL in JAVA-OOP
>
>  - static을 맴버(변수,메소드) 앞에 붙이면 클래스의 맴버가 된다.
>  - this. 을 멤버 앞에 붙이면 인스턴스의 멤버가 된다.

##### 클래스 메소드

```java
class Calculator3{
  
    public static void sum(int left, int right){
        System.out.println(left+right);
    }
    public static void avg(int left, int right){
        System.out.println((left+right)/2);
    }
}
 
public class CalculatorDemo3 {
     
    public static void main(String[] args) {
        Calculator3.sum(10, 20);
        Calculator3.avg(10, 20);
         
        Calculator3.sum(20, 40);
        Calculator3.avg(20, 40);
    }
}
```

##### 클래스 맴버와 인스턴스 맴버 관계

1. **인스턴스 메소드는 클래스 맴버에 접근 할 수 있다.**
2. **클래스 메소드는 인스턴스 맴버에 접근 할 수 없다.**
   인스턴스 변수는 인스턴스가 만들어지면서 생성되는데, 클래스 메소드는 인스턴스가 생성되기 전에 만들어지기 때문에 클래스 메소드가 인스턴스 맴버에 접근하는 것은 존재하지 않는 인스턴스 변수에 접근하는 것과 같다.

> ✅ TIL in JAVA- class&instance
>
> - 인스턴스 변수 -> Non-Static Field
>
> - 클래스 변수 -> Static Field
>
>   필드(field)라는 것은 클래스 전역에서 접근 할 수 있는 변수를 의미하는데 이에 대한 자세한 내용은 유효범위 수업에서 알아보겠다.

### 유효범위

```java
// 무한 루프
public class ScopeDemo2 {
    static int i;
     
    static void a() {
     /*int*/ i = 0;
    }
    public static void main(String[] args) {
        for (/*int*/ i = 0; i < 5; i++) {
            a();
            System.out.println(i);
        }
    }
}

// int i 를 선
```

##### This

```java
class C3 {
    int v = 10;
 
    void m() {
        int v = 20;
        System.out.println(v)  //20
        System.out.println(this.v);  //10
    }
}
 
public class ScopeDemo9 {
 
    public static void main(String[] args) {
        C3 c1 = new C3();
        c1.m();
    }
}
```

##### 생성자

```java
class Calculator {
    int left, right;
	// 생성자,  constructor
    public Calculator(int left, int right) {
        this.left = left;
        this.right = right;
    }
 
    public void sum() {
        System.out.println(this.left + this.right);
    }
 
    public void avg() {
        System.out.println((this.left + this.right) / 2);
    }
}
 
public class CalculatorDemo1 {
 
    public static void main(String[] args) {

        Calculator c1 = new Calculator(10, 20);
        c1.sum();
        c1.avg();
 
        Calculator c2 = new Calculator(20, 40);
        c2.sum();
        c2.avg();
    }
}
```

✅ TIL in JAVA-scope

> - static int i 는` 전연벽수`가 된다.
>- 함수 혹은 반복문 에서 새롭게 정의한 i는 새로운 `지역변수`로 재탄생한다. 
> - this를 이용하여 전역변수를 불러올 수 있다. (생략 가능)



### 상속

상속(Inheritance)이란 물려준다는 의미다. 어떤 객체가 있을 때 그 객체의 필드(변수)와 메소드를 다른 객체가 물려 받을 수 있는 기능을 상속이라고 한다.

> 기존의 객체를 그대로 유지하면서 어떤 기능을 추가하는 방법이 없을까? 이런 맥락에서 등장하는 것이 상속이다. 즉 기존의 객체를 수정하지 않으면서 새로운 객체가 기존의 객체를 기반으로 만들어지게 되는 것이다. 이때 기존의 객체는 기능을 물려준다는 의미에서 부모 객체가 되고 새로운 객체는 기존 객체의 기능을 물려받는다는 의미에서 자식 객체가 된다. 부모 클래스와 자식 클래스의 관계를 상위(super) 클래스와 하위(sub) 클래스라고 표현하기도 한다. 또한 기초 클래스(base class), 유도 클래스(derived class)라고도 부른다. 

```java
class SubstractionableCalculator extends Calculator {
    public void substract() {
        System.out.println(this.left - this.right);
    }
}
```

##### 상속과 생성자

```java
public class ConstructorDemo {
    public ConstructorDemo(){}
    public ConstructorDemo(int param1) {}
    public static void main(String[] args) {
        ConstructorDemo  c = new ConstructorDemo();
    }
}

class SubstractionableCalculator extends Calculator {
    public SubstractionableCalculator(int left, int right) {
        super(left, right);
    }
```

✅ TIL in JAVA-inheritance

> - 코드 중복의 제거, 재활용성, 간결성
> - 기본 생성자를 명시해야 한다. `public ConstructorDemo(){}`
> - 부모의 생성자가 없어져도 super를 통해서 해결할 수 있다. 즉 자식 클래스의 생성자가 상속을 받을 때 `super(left, rigt);` 를 이용한다.



##### Overriding

> 하위 클래스에서 상의 클래스와 동일한 메소드를 정의하면 부모 클래스로부터 물려 받은 기본 동작 방법을 변경하는 효과를 갖게 된다. 이것을 메소드 오버라이딩(overriding)이라고 한다. 오버라이딩을 하기 위해서는 아래의 조건을 충족시켜야 한다.
>
> - 메소드의 이름
> - 메소드 매개변수의 숫자와 데이터 타입 그리고 순서
> - 메소드의 리턴 타입
>
> 위와 같이 메소드의 형태를 정의하는 사항들을 통털어서 메소드의 서명(signature)라고 한다. 
>
> `super.` 를 이용하여 부모클래스의 메소드를 상속받을 수 있다.

```java
public int avg() {
    return super.avg();
}
```

##### Overloading

>매개변수의 숫자에 따라서 같은 이름의, 서로 다른 메소드를 호출하고 있다는 것을 알 수 있다. 이름은 같지만 시그니처는 다른 메소드를 중복으로 선언 할 수 있는 방법을 메소드 오버로딩(overloading)이라고 한다.
>
>- 매개변수가 달라지면 서로 다른 메소드라고 오버로딩된다.
>
>- 하지만, return 형식이 달라지면 오류를 발생시킨다.

```java
public class OverloadingDemo {
    void A (){System.out.println("void A()");}
    void A (int arg1){System.out.println("void A (int arg1)");}
    void A (String arg1){System.out.println("void A (String arg1)");}
    //int A (){System.out.println("void A()");}
    public static void main(String[] args) {
        OverloadingDemo od = new OverloadingDemo();
        od.A();
        od.A(1);
        od.A("coding everybody");
    }
}
```

✅ TIL in JAVA-overriding vs overloading

> - 오버라이딩과 오버로딩은 용어가 참으로 헷갈린다. 당연하다. 중요한 것은 오버라이딩이 무엇이고 오버로딩이 무엇인가를 구분하는 것은 아니다. riding(올라탄다)을 이용해서 부모 클래스의 메소드의 동작방법을 변경하고, loading을 이용해서 같은 이름, 다른 매개변수의 메소드들을 여러개 만들 수 있다는 사실을 아는 것이 중요하다. 







