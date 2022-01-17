# Mixin

> Mixins are a way of reusing a class’s code in multiple class hierarchies.
>
> To *use* a mixin, use the `with` keyword followed by one or more mixin names.

믹스인은 다양한 계층에 걸친 클래스의 코드를 재사용할 수 있는 방법이다. 믹스인을 사용하려면 with 키워드를 믹스인할 클래스 이름 앞에 붙여준다.

<br>

### 다중상속 지원 X

다트에서는 자바와 마찬가지로 다중상속을 지원하지 않는다. 

<br>

### 믹스인 메서드 명이 중복될 경우

여러 믹스인을 사용하더라도 동일한 메서드의 경우에 마지막 믹스인 클래스의 메서드를 따른다. 믹스인은 클래스를 상속하지 않으면서 상속한 것처럼 메서드를 사용할 수 있게 해준다.

```dart
class A {
  String getMessage() => 'A';
}
 
class B {
  String getMessage() => 'B';
}
 
class P {
  String getMessage() => 'P';
}
 
class AB extends P with A, B {}
 
class BA extends P with B, A {}
 
void main() {
  String result = '';
 
  AB ab = AB();
  result += ab.getMessage();
 
  BA ba = BA();
  result += ba.getMessage();
 
  print(result);
  // BA
}
```

AB, BA 클래스는 P 인터페이스 클래스를 extends 하여 사용하면서 A, B 클래스를 mixin 하고 있다.

P, A, B 클래스는 모두 같은 이름인 getMessage 메서드를 포함하고 있다. 이럴때는 맨 마지막 믹스인 클래스를 따르게 된다.

<br>

### with 은 implements 앞에 나와야 한다

with 키워드는 implements 뒤에 나올 수 없다. 반드시 implements 앞에 나와야 한다.

```dart
class A {
  String getMessage() => 'A';
}
 
class B {
  String getMessage() => 'B';
}
 
class P {
  String getMessage() => 'P';
}
 
class AB implements P with A, B {}
// 위의 코드는 아래와 같은 에러를 발생시킨다
// The with clause must be before the implements clause. Try moving the with clause before the implements clause.
```

<br>

### 추상 메서드와 믹스인 메서드

추상 클래스의 메서드(추상 메서드)와 믹스인 클래스 메서드가 같은 이름을 갖지 않으면 추상 클래스의 메서드인 getMessage를 재정의하라는 에러 발생한다.

```dart
class A {
  String getMessageFromA() => 'A';
}
 
class B {
  String getMessageFromB() => 'B';
}
 
abstract class P {
  String getMessage();
}
 
class AB extends P with A, B {}
// 추상 메서드와 믹스인 클래스 메서드가 같은 이름을 갖지 않으면 추상 클래스의 메서드인 getMessage를 재정의하라는 에러 발생한다.
// Missing concrete implementation of 'P.getMessage'. Try implementing the missing method, or make the class abstract.
 
class BA extends P with B, A {}
// 추상 메서드와 믹스인 클래스 메서드가 같은 이름을 갖지 않으면 추상 클래스의 메서드인 getMessage를 재정의하라는 에러 발생한다.
// Missing concrete implementation of 'P.getMessage'. Try implementing the missing method, or make the class abstract.
 
void main() {

}
```

<br>

믹스인 클래스 메서드 중 하나라도 추상 메서드와 이름이 같으면 에러는 발생하지 않고 추상 메서드와 동일한 믹스인 클래스 메서드의 기능으로 동작한다.

```dart
class A {
  String getMessages() => 'A';
}
 
class B {
  String getMessage() => 'B';
}
 
abstract class P {
  String getMessage();
}
 
class AB extends P with A, B {}
 
class BA extends P with B, A {}
 
void main() {
  String result = '';
 
  AB ab = AB();
  result += ab.getMessage();
 
  BA ba = BA();
  result += ba.getMessage();
 
  print(result);
  // BB
}
```

<br>

```dart
abstract class Car {
  void drive();
}

mixin ExhaustSound {
  void drive() {
    print("부아앙");
  }
}

class Lamborghini extends Car with ExhaustSound {
  drive();
}

void main() {
  Lamborghini l = Lamborghini();
  l.drive();
  // 부아앙
}
```

<br>

### mixin 키워드

일반 클래스로서 사용하지 않고 믹스인 용도로만 사용하려면 class 키워드 대신 mixin 키워드를 사용하면 된다.

```dart
abstract class Car {
  void drive();
}

mixin ExhaustSound {
  void makesExhaustSound() {
    print("부아앙");
  }
}

class Lamborghini extends Car with ExhaustSound {
  @override
  void drive() {
    makesExhaustSound();
    print("달린다");
  }

}

void main() {
  Lamborghini l = Lamborghini();
  l.drive();
  // 부아앙
  // 달린다
}
```

<br>

참고

https://omty.tistory.com/43
