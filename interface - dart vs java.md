# interface - dart vs java

### 키워드

#### dart

interface 키워드를 사용하지 않는다. 모든 클래스는 암시적으로 interface를 정의한다고 간주한다.

#### java

java는 interface 키워드를 사용한다.

<br>

### implements

#### dart & java

implements로 구현한다.

<br>

### extends

#### dart

dart는 implements 하여 클래스를 구현할 수 있으면서 extends 를 사용하여 클래스를 생성할 수 있다. extends를 당하는(?) 클래스는 추상 클래스가 아니기 때문에 추상 메서드를 가질 수는 없다.

<br>

상속받는 클래스에서 상속할 클래스에 작성된 메서드를 @override 어노테이션을 사용하여 재정의할 수 있는데 @override를 사용하지 않아도 상속받는 클래스에서 작성한 메서드로 동작하므로 굳이 작성하지 않아도 된다.

```dart
class Vehicle {
  String name = '123';
  
  void getSpeed() {
    print(1);
  }
}

class BMW extends Vehicle {
  @override
  void getSpeed() {
    print(2);
  }
}

void main() {
  BMW bmw = BMW();
  bmw.getSpeed();
  print(bmw.name);
}
```

<br>

상속할 클래스의 메서드는 super로 호출 가능하다. 단, 바로 사용할 수는 없고 같은 이름의 메서드를 만들고 그 안에서 super로 상속할 클래스의 메서드를 호출할 수 있다.

```dart
class Vehicle {
  void getSpeed() {
    print(1);
  }
}

class BMW extends Vehicle {
	// super.getSpeed();
    // 위와 같은 방법으로 호출할 수 없다
}
```

<br>

```dart
class Vehicle {
  void getSpeed() {
    print(1);
  }
}

class BMW extends Vehicle {
  void getSpeed() {
    super.getSpeed();
  }
}
```

<br>

#### java

extends 할 수 없다.

<br>

### 상수

#### dart

@override 어노테이션은 생략해도 되지만 구현할 클래스의 모든 상수를 재정의해줘야 한다. 하나라도 생략하면 오류 발생한다. 

```dart
class Vehicle {
  int number = 1;
  String name = 'Vehicle';
}

class BMW implements Vehicle {
  @override
  int number = 2;

  @override
  String name = 'BMW';
}
```

<br>

상수 뿐만 아니라 메서드도 모두 재정의해야 한다.

```dart
class Vehicle {
  int number = 1;
  String name = 'Vehicle';
  
  void getSpeed() {
    print(1);
  }
}

class BMW implements Vehicle {
  @override
  int number = 1;

  @override
  String name = 'BMW';

  @override
  void getSpeed() {
	print(2);
  }
}

void main() {
  BMW bmw = BMW();
  bmw.getSpeed();
  print(bmw.name);
}
```

<br>

#### java

구현하는 클래스에서 구현할 클래스에 정의한 상수에 대해서 @Override 어노테이션 사용할 수 없다. 그리고 구현하는 클래스에서 재정의하면 구현하는 클래스에서는 재정의한 값으로 동작한다.

```java
public interface Bank {
	public static final int max_integer = 100;
}

public class KakaoBank implements Bank{
	public int max_integer = 101;
}

public class HelloBank {
	public static void main(String[] args) {
		KakaoBank k = new KakaoBank();
		System.out.println(k.max_integer);
        // 101
	}
}
```

