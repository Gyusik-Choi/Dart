# abstract class - dart vs java

### 인스턴스

dart와 java 모두 추상클래스의 인스턴스 생성할 수 없다.

<br>

### 한 파일 vs 여러 파일

dart는 한 파일에 여러 (퍼블릭한) 클래스 선언 가능하다.

java는 한 파일에 public class 를 하나만 쓸 수 있어서 public 추상 클래스와 해당 추상 클래스를 extends 하는 클래스를 별도의 파일에 써야 한다.

<br>

### 멤버변수

#### dart

dart는 초기화를 해주거나 late 키워드를 앞에 붙여서 곧 초기화 될 것이라고 명시해줘야 한다. 혹은 ?를 붙여서 nullable 하다고 선언해줄 수 있다(그러나 kind를 활용하는 곳이 있는데 null이면 말이 안되기 때문에 주의해서 사용해야 한다).

```dart
abstract class Animal {
  late String kind;
}
```

```dart
abstract class Animal {
  String kind = '';
}
```

```dart
abstract class Animal {
  String? kind;
}
```

<br>

late를 붙이지 않거나 초기화하지 않으면 에러 발생한다.

```dart
abstract class Animal {
  String kind;
}
// Error: Field 'kind' should be initialized because its type 'String' doesn't allow null.
```

<br>

late

> enforce this variable’s constraints at runtime instead of at compile time

<br>

```dart
abstract class Animal {
  late String kind;
}

class Dog extends Animal {

  Dog(String kind) {
    this.kind = kind;
  }
}

void main() {
  Dog d = Dog("포유류");
  print(d.kind);
  // 포유류
}
```

- dart에서 멤버변수에 this를 반드시 붙여야 하는 것은 아니지만 파라미터와 같을 경우에는 구분을 위해 this를 사용해줘야 한다. 이때는 안쓰면 에러 발생한다.

<br>

#### java

접근 제어자(public, default, protected, private) 를 붙이거나 붙이지 않을 수 있으며 초기화하지 않아도 된다. ~~이건 편하네~~.

접근 제어자를 생략하면 default 를 의미하게 되며 default를 동일한 패키지 안에서 접근 가능하다. 접근 제어자별 접근 가능 범위는 [이곳](https://csw7432.tistory.com/entry/Java-%EC%A0%91%EA%B7%BC%EC%A0%9C%EC%96%B4%EC%9E%90-Access-Modifier)을 참고했다.

```java
public abstract class Animal {
	public String kind;
}
```

<br>

### 메서드

dart와 java 모두 추상 클래스에 정의한 메서드(추상 메서드 아님)를 상속받은 클래스에서 사용 가능하다.

#### dart

```dart
abstract class Animal {
  void breath() {
    print("숨쉰다");
  }
}

class Dog extends Animal {

}

void main() {
  Dog d = Dog();
  d.breath();
  // 숨쉰다
}
```

<br>

#### java

```java
// Animal.java
public abstract class Animal {
	public void breath() {
		System.out.println("숨쉰다");
	}
}
```

```java
// Dog.java
public class Dog extends Animal {
    
}
```

```java
// Main.java
public class Main {
	
	public static void main(String[] args) {
		Dog dog = new Dog();
		dog.breath();
	}
}
```

<br>

### 추상 메서드

dart와 java 모두 추상 메서드는 반드시 재정의 해줘야 한다. 

java와 다르게 dart 에서는 추상 메서드에 abstract 키워드를 붙이지 않는다(붙이면 에러 발생한다).

#### dart

```dart
abstract class Animal {
  void sound();
}

class Dog extends Animal {
  @override
  void sound() {
    print("bark");
  }
}

void main() {
  Dog d = Dog();
  d.sound();
  // bark
}
```

<br>

#### java

```java
// Animal.java
public abstract class Animal {
	public abstract void sound();
}

```

```java
// Dog.java
public class Dog extends Animal {
	@Override
	public void sound() {
		System.out.println("bark");
	}
}
```

```java
// Main.java
public class HelloWorld {
	public static void main(String[] args) {
		Dog dog = new Dog();
		dog.sound();
         // bark
	}
}
```

<br>

### 타입의 다형성

dart와 java 모두 다형성을 지원한다.

추상 클래스 파라미터 변수에 해당 클래스를 상속(extends)한 클래스의 객체를 주입하면 해당 파라미터 변수는 타입 변환이 자동으로 발생한다.

#### dart

```dart
abstract class Animal {
  void sound();
}

class Dog extends Animal {
  @override
  void sound() {
    print("bark");
  }
}

class Cat extends Animal {
  @override
  void sound() {
    print("meow");
  }
}

void showWhoYouAre(Animal animal) {
  animal.sound();
}

void main() {
  Dog d = Dog();
  showWhoYouAre(d);
  // bark

  Cat c = Cat();
  showWhoYouAre(c);
  // meow
}
```

<br>

#### java

```java
// Animal.java
public abstract class Animal {
	public abstract void sound();
}
```

```java
// Dog.java
public class Dog extends Animal {
	@Override
	public void sound() {
		System.out.println("bark");
	}
}
```

```java
// Cat.java
public class Cat extends Animal {
	@Override
	public void sound() {
		System.out.println("meow");
	}
}
```

```java
// Main.java
public class Main {
	
	public static void main(String[] args) {
		Dog dog = new Dog();
		dog.sound();
		
		Cat cat = new Cat();
		cat.sound();
		
		animalSound(dog);
		animalSound(cat);
        
        // bark
        // meow
        // bark
        // meow
	}
	
	private static void animalSound(Animal animal) {
		animal.sound();
	}

}
```



<br>

참고

https://dart.dev/guides/language/language-tour#abstract-classes

https://dart.dev/null-safety/understanding-null-safety#late-variables

https://limkydev.tistory.com/188

https://csw7432.tistory.com/entry/Java-%EC%A0%91%EA%B7%BC%EC%A0%9C%EC%96%B4%EC%9E%90-Access-Modifier

https://tecoble.techcourse.co.kr/post/2020-10-27-polymorphism/