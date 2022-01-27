# abstract class vs interface (java)

자바를 기준으로 추상 클래스와 인터페이스의 차이에 대해서 정리하고자 한다

추상클래스를 **kind of**, 인터페이스를 **able** 이라는 표현으로 많이 설명한다

<br>

### 추상 클래스

- 필드(멤버변수)

- 생성자

- 메서드

  - 상속받는 클래스에서 같은 메서드명으로 정의하면 추상 클래스의 메서드가 아닌 해당 클래스의 메서드로 동작한다
  - 인터페이스의 디폴트 메서드도 마찬가지다

- 추상 메서드

  - ```java
    public abstract class Animal {
    	public abstract void sound();
    }
    
    public class Dog extends Animal {	
    	@Override
    	public void sound() {
    		System.out.println("bark");
    	}
    }
    ```

    


<br>

### 인터페이스

- 상수

  - ```java
    int min_number = 100;
    ```

  - 인터페이스를 구현한 클래스에서 동일한 변수명으로 상수를 변경할 수 있고 변경한 값으로 동작하게 된다

    - ```java
      public interface Bank {
      	public static final int max_integer = 100;
      }
      
      public class KakaoBank implements Bank{
      	public int max_integer = 101;
      	
      	@Override
      	public void withDraw() {
      		System.out.println(max_integer);
      	}	
      }
      
      public class HelloBank {
      	public static void main(String[] args) {
      		KakaoBank k = new KakaoBank();
      		k.withDraw();
               // 101
      	}
      }
      ```

      

- 추상 메서드

- 디폴트 메서드

  - 구현하는 클래스에서 재정의 가능

  - @Override 어노테이션과 함께 재정의 가능하다.

    - ```java
      public interface Bank {
      	default void checkAccount() {
      		System.out.println("계좌체크");
      	}
      }
      
      public class KakaoBank implements Bank{
      	@Override
      	public void checkAccount() {
      		System.out.println("kakao 계좌체크");
      	}
      }
      
      public class HelloBank {
      	public static void main(String[] args) {
      		KakaoBank k = new KakaoBank();
      		k.checkAccount();
               // kakao 계좌체크
      	}
      }
      ```

    - 아래 추상 클래스와 달리 super는 인터페이스를 구현하는 클래스에서 사용할 수 없음

      

    - 추상 클래스의 (일반) 메서드도 @Override 어노테이션과 함께 재정의 가능

      - ```java
        public abstract class Animal {
        	public void bark() {
        		System.out.println("bark");
        	}
        }
        
        public class Dog extends Animal {	
        	@Override
        	public void bark() {
        		super.bark();
        		System.out.println("dog bark");
        	}
        }
        
        public class HelloWorld {
        	public static void main(String[] args) {
        		Dog dog = new Dog();
        		dog.bark();
                 // bark
                 // dog bark
        	}
        }
        ```

        

- 정적 메서드

  - 인터페이스의 메서드와 동일한 메서드 이름으로 새로운 메서드를 작성하면 해당 메서드의 내용으로 동작한다

  - @Override 해서 재정의 할 수 없음

  - 인터페이스를 구현한 클래스의 인스턴스 메서드로는 호출 불가능

    - 인터페이스의 메서드로 호출 가능하다

    - ```java
      public interface Bank {
      	static void deposit() {
      		System.out.println("예금");
      	}
      }
      
      public class HelloBank {
      	public static void main(String[] args) {
      		KakaoBank k = new KakaoBank();
      		// k.deposit();
               // 위와 같은 방식으로는 호출할 수 없다
      		Bank.deposit();
      	}
      }
      ```


<br>

### 생성자

추상클래스는 생성자 가질 수 있으나 인터페이스는 생성자 가질 수 없다

<br>

### 다중상속

추상클래스는 다중상속 불가능, 인터페이스는 다중상속 가능하다

<br>

참고

https://devlog-wjdrbs96.tistory.com/370

https://limkydev.tistory.com/188

https://limkydev.tistory.com/197

https://brunch.co.kr/@kd4/6

https://medium.com/webeveloper/%EC%9E%90%EB%B0%94-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EC%99%80-%EC%B6%94%EC%83%81%ED%81%B4%EB%9E%98%EC%8A%A4-6eecbe5d6350

https://velog.io/@ednadev/%EC%9E%90%EB%B0%94-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4Interface-%EB%94%94%ED%8F%B4%ED%8A%B8-%EB%A9%94%EC%84%9C%EB%93%9C%EC%99%80-static-%EB%A9%94%EC%84%9C%EB%93%9C

