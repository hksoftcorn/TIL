# JAVA1 - 생활코딩

>JAVA1 - 제어문 | 메소드

### 변수

### 조건문



### 반복문

##### While

```java
public class WhileDemo {
    public static void main(String[] args) {
        while (true) {
            System.out.println("Coding Everybody");
        }
    }
}

public class WhileDemo2 {
    public static void main(String[] args) {
        int i = 0;
        while (i < 10) {
            System.out.println("Coding Everybody" + i);
            i++;
        }
    }
}
```

##### For

> ```java
> for (초기화; 종료조건; 반복실행){
>     반복적으로 실행될 구문
> }
> ```

```java
public class ForDemo {
 
    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            System.out.println("Coding Everybody " + i);
        }
    }
}
```

##### 반복문의 제어 : break, continue

```java
public class BreakDemo {
 
    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            if (i == 5)
                break;
            System.out.println("Coding Everybody " + i);
        }
    }
}


public class ContinueDemo {
 
    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            if (i == 5)
                continue;
            System.out.println("Coding Everybody " + i);
        }
    }
}
```

##### 반복문의 중첩

```java
public class LoopDepthDemo {
 
    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            for (int j = 0; j < 10; j++) {
                System.out.println(i + "" + j);
            }
        }
    }
}
```



> ✅ TIL in JAVA-loop 
>
> - if (i == 5)  : `{ 1줄은 생략가능 }`
>
> - System.out.println`(i + "" + j)`;

 

### 배열

##### 배열의 개념

배열은 연관된 데이터를 모아서 관리하기 위해서 사용하는 데이터 타입이다. 변수가 하나의 데이터를 저장하기 위한 것이라면 배열은 여러 개의 데이터를 저장하기 위한 것이라고 할 수 있다.

```java
public class GetDemo {
 
    public static void main(String[] args) {
        String[] classGroup = { "최진혁", "최유빈", "한이람", "이고잉" };
        System.out.println(classGroup[0]);
        System.out.println(classGroup[1]);
        System.out.println(classGroup[2]);
        System.out.println(classGroup[3]);
    }
}


public class LengthDemo {
 
    public static void main(String[] args) {
        //String[] classGroup = { "최진혁", "최유빈", "한이람", "이고잉" };
        String[] classGroup = new String[4];
        classGroup[0] = "최진혁";
        System.out.println(classGroup.length); //4
        classGroup[1] = "최유빈";
        System.out.println(classGroup.length); //4
        classGroup[2] = "한이람";
        System.out.println(classGroup.length); //4
        classGroup[3] = "이고잉";
        System.out.println(classGroup.length); //4
    }
}
```

##### 배열의 제어

```java
public class ArrayLoopDemo {
 
    public static void main(String[] args) {
 
        String[] members = { "최진혁", "최유빈", "한이람" };
        for (int i = 0; i < members.length; i++) {
            String member = members[i];
            System.out.println(member + "이 상담을 받았습니다");
        }
    }
}
```

##### for-each

```java
public class ForeachDemo {
 
    public static void main(String[] args) {
        String[] members = { "최진혁", "최유빈", "한이람" };
        for (String e : members) {
            System.out.println(e + "이 상담을 받았습니다");
        }
    }
 
}
```

##### 오류

- ArrayIndexOutOfBoundsException



>  ✅ TIL in JAVA-array
>
> - String[] classGroup = { "최진혁", "최유빈", "한이람", "이고잉" };
>
> - String[] classGroup = `new` String[4];
>
>   ```java
>   // index 접근
>   String[] members = { "최진혁", "최유빈", "한이람" };
>   for (int i = 0; i < members.length; i++) {
>       String member = members[i];
>       System.out.println(member + "이 상담을 받았습니다");
>   }
>   
>   // for-each 접근
>   String[] members = { "최진혁", "최유빈", "한이람" };
>   for (String e : members) {
>       System.out.println(e + "이 상담을 받았습니다");
>   }
>   ```



### 메소드

##### 메소드의 정의와 호출

```java
public class MethodDemo1 {
    // 정의
    public static void numbering() {
        int i = 0;
        while (i < 10) {
            System.out.println(i);
            i++;
        }
    }
 	// 호출
    public static void main(String[] args) {
        numbering();
    }
}
```

##### main 메소드

main 메소드는 규칙이다. 여러분이 만들고 싶은 프로그램이 있다면 여러분은 반드시 `public static void main(String[] args)`가 이끄는 중괄호 안에 실행되기를 기대하는 로직을 위치시켜야 한다. 이것은 약속이기 때문에 여러분은 약속을 지켜야 한다. 그렇게 코드를 작성하면 자바를 실행할 때 자바는 여러분이 작성한 main 메소드를 실행하게 되는 것이다. 여러분은 main 메소드를 작성하고, 자바는 main 메소드를 실행하는 관계라고 할 수 있다.

##### 메소드의 입력과 출력

*매개변수, (복수)인자*

```java
public class MethodDemo4 {
    public static void numbering(int init, int limit) { //int limit 매개변수(parameter)
        int i = init;
        while (i < limit) {
            System.out.println(i);
            i++;
        }
    }
 
    public static void main(String[] args) {
        numbering(1, 5); //5 인자(argument)
    }
}
```

*return*

```java
public class MethodDemo6 {
    public static String numbering(int init, int limit) {
        int i = init;
        // 만들어지는 숫자들을 output이라는 변수에 담기 위해서 변수에 빈 값을 주었다.
        String output = "";
        while (i < limit) {
            // 숫자를 화면에 출력하는 대신 변수 output에 담았다.
            output += i;
            i++;
        }
        // 중요!!! output에 담겨 있는 문자열을 메소드 외부로 반환하려면 
        // 아래와 같이 return 키워드 뒤에 반환하려는 값을 배치하면 된다.
        return output;
    }
 
    public static void main(String[] args) {
        // 메소드 numbering이 리턴한 값이 변수 result에 담긴다.
        String result = numbering(1, 5);
        // 변수 result의 값을 화면에 출력한다.
        System.out.println(result);
        try { // 무시
            // 다음 행은 out.txt 라는 파일에 numbering이라는 메소드가 반환한 값을 저장합니다.
            BufferedWriter out = new BufferedWriter(new FileWriter("out.txt"));
            out.write(result);
            out.close();
        } catch (IOException e) {
        } // 무시
    }
}
```



> ✅ TIL in JAVA-method
>
> - return 방법을 기억합시다!! 정장을 입은 듯한 Java Style ~
>
>   - public static String numbering() : 이 함수는 String으로 나옵니다.
>   - String result = numbering(1, 5); : 이 함수의 결과를 String으로 result 변수에 담습니다.
>   - public static String[] numbering() : 이 함수는 String array으로 나옵니다.
>   - String[] result = numbering(1, 5); : 이 함수의 결과를 String array으로 result 변수에 담습니다.
>
>   ```java
>   public static String numbering(int init, int limit) {
>       int i = init;
>       String output = "";
>       while (i < limit) {
>           output += i;
>           i++;
>       }
>       return output;
>       
>   public static void main(String[] args) {
>       String result = numbering(1, 5);
>       System.out.println(result);
>   }
>   ```



### 입력과 출력

##### String[] args

```java
class InputDemo{
    public static void main(String[] args){
        System.out.println(args.length);
    }
}

class InputForeachDemo{
    public static void main(String[] args){
        for(String e : args){
            System.out.println(e);
        }
    }
}
```

##### scanner & while(sc.hasNextInt())

```java
import java.util.Scanner;
 
public class ScannerDemo {
 
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int i = sc.nextInt();
        System.out.println(i*1000);
        sc.close();
    }
}
```
