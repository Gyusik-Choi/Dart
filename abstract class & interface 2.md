# Abstract Class & (Implicit) Interface Class

### 추상 클래스

extends, implements 모두 가능하다.

추상 메서드를 사용하려면 void a() {} 가 아니라 void a(); 이렇게 세미콜론(;)을 쓰라고 한다.

```dart
abstract class Vehicle {
  void getSpeed();
}

class BMW extends Vehicle {
  @override
  void getSpeed() {
    print(123);
  }
}

class Audi implements Vehicle {
  @override
  void getSpeed() {
    print(456);
  }
}

void main() {
  BMW bmw = BMW();
  bmw.getSpeed();
  
  Audi audi = Audi();
  audi.getSpeed();
}
```

<br>

공식 문서에서 얘기한 방법과 다르게 추상 메서드를 추상 클래스에서 정의해도 사용하는데는 문제 없다.

```dart
abstract class Vehicle {
  void getSpeed() {
    print(0);
  }
}

class BMW extends Vehicle {
  @override
  void getSpeed() {
    print(123);
  }
}

class Audi implements Vehicle {
  @override
  void getSpeed() {
    print(456);
  }
}

void main() {
  BMW bmw = BMW();
  bmw.getSpeed();
  // 123
  
  Audi audi = Audi();
  audi.getSpeed();
  // 456
}
```

<br>

추상 클래스, 인터페이스 클래스 모두 extends와 implements 사용 가능하다.

그리고 추상 클래스로 하지 않고 암시적인 인터페이스 클래스로 사용하더라도 extends, implements 모두 에러가 발생하지 않는다.

```dart
class Vehicle {
  void getSpeed() {
    print(0);
  }
}

class BMW extends Vehicle {
  @override
  void getSpeed() {
    print(1);
  }
}

class Audi implements Vehicle {
  @override
  void getSpeed() {
    print(2);
  }
}

void main() {
  BMW bmw = BMW();
  bmw.getSpeed();
  // 1
  
  Audi audi = Audi();
  audi.getSpeed();
  // 2
}
```

<br>

단, 두개 이상의 클래스를 extends 해서 사용하려면 에러가 발생한다.

```dart
class Vehicle2 {
  void getSpeed() {
    print(-1);
  }
}

class Vehicle {
  void getSpeed() {
    print(0);
  }
}

class BMW extends Vehicle, Vehicle2 {
  @override
  void getSpeed() {
    print(1);
  }
}

// error
// Each class definition can have at most one extends clause.
// Try choosing one superclass and define your class to implement (or mix in) the others.
```

<br>

implements 하면 에러가 발생하지 않는다.

```dart
class Vehicle2 {
  void getSpeed() {
    print(-1);
  }
}

class Vehicle {
  void getSpeed() {
    print(0);
  }
}

class BMW implements Vehicle, Vehicle2 {
  @override
  void getSpeed() {
    print(1);
  }
}

void main() {
  BMW bmw = BMW();
  bmw.getSpeed();
  // 1
}
```

<br>

추상 클래스 선언(abstract)을 하지 않고 추상 메서드를 정의하면 에러가 발생한다.

```dart
class Vehicle2 {
  void getSpeed();
}
// The non-abstract class 'Vehicle2' is missing implementations for these members: - Vehicle2.getSpeed

class Vehicle {
  void getSpeed();
}
// The non-abstract class 'Vehicle' is missing implementations for these members: - Vehicle.getSpeed

class BMW implements Vehicle, Vehicle2 {
  @override
  void getSpeed() {
    print(1);
  }
}

void main() {
  BMW bmw = BMW();
  bmw.getSpeed();
}
```

<br>

추상 클래스 선언을 하면 에러 발생하지 않는다.

```dart
abstract class Vehicle2 {
  void getSpeed();
}

abstract class Vehicle {
  void getSpeed();
}

class BMW implements Vehicle {
  @override
  void getSpeed() {
    print(1);
  }
}

class Audi implements Vehicle2 {
  @override
  void getSpeed() {
    print(2);
  }
}

void main() {
  BMW bmw = BMW();
  bmw.getSpeed();
  
  Audi audi = Audi();
  audi.getSpeed();
}
```

<br>

추상 클래스와 인터페이스의 차이는 추상 클래스는 인스턴스를 생성할 수 없다.

```dart
abstract class Vehicle {
  void getSpeed();
}

void main() {
  Vehicle v = Vehicle();
  v.getSpeed();
}

// error
// Abstract classes can't be instantiated.
```

<br>

그리고 인터페이스 클래스는 추상 메서드를 가질 수 없다.

```dart
class Vehicle {
  void getSpeed();
}

void main() {
  Vehicle v = Vehicle();
  v.getSpeed();
}

// error
// 'getSpeed' must have a method body because 'Vehicle' isn't abstract.
```

<br>

|                                         | 추상 클래스 | 인터페이스 클래스 |
| --------------------------------------- | ----------- | ----------------- |
| 추상 메서드                             | O           | X                 |
| 인스턴스                                | X           | O                 |
| implements하고 변수 및 메서드 정의 누락 | X           | X                 |
| extends하고 변수 및 메서드 정의 누락    | X           | O                 |

<br>

```dart
abstract class Vehicle {
  String name = '123';
  
  void getSpeed();
}

class BMW implements Vehicle {
  @override
  void getSpeed() {
    print(1);
  }
}

void main() {
  BMW bmw = BMW();
  bmw.getSpeed();
}

// name을 BMW 클래스에서 정의 안했다.
// The non-abstract class 'BMW' is missing implementations for these members: - Vehicle.name
```

<br>

```dart
class Vehicle {
  String name = '123';
  
  void getSpeed();
}

class BMW implements Vehicle {
  @override
  void getSpeed() {
    print(1);
  }
}

void main() {
  BMW bmw = BMW();
  bmw.getSpeed();
}

// 인터페이스 클래스에서는 getSpeed를 정의해주지 않아서 Vehicle 클래스에서 에러가 발생하고
// The non-abstract class 'Vehicle' is missing implementations for these members: - Vehicle.getSpeed
// 그리고 name을 BMW 클래스에서 정의 안해서 에러 발생한다.
// The non-abstract class 'BMW' is missing implementations for these members: - Vehicle.name
```

<br>

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
}
// 2
// 인터페이스 클래스에서는 extends 해서 사용하면 Vehicle의 name 변수를 BMW에서 정의해주지 않아도 에러 발생하지 않는다.
```



