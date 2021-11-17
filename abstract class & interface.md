# Abstract Class & (Implicit) Interface

### Abstract Class

추상 클래스는 abstract 수식어를 사용하여 정의하며 인스턴스를 생성할 수 없다. 추상 클래스는 인터페이스를 정의하는데 유용하며 때로는 구현에도 활용될 수 있다. 인스턴스를 생성할 수 있는 것처럼 보이게 하려면 factory 생성자를 정의하면 된다(팩토리 생성자를 통해서 인스턴스를 딱 하나만 생성할 수 있다).

>Use the `abstract` modifier to define an *abstract class*—a class that can’t be instantiated. Abstract classes are useful for defining interfaces, often with some implementation. If you want your abstract class to appear to be instantiable, define a [factory constructor](https://dart.dev/guides/language/language-tour#factory-constructors).
>
>Abstract classes often have [abstract methods](https://dart.dev/guides/language/language-tour#abstract-methods).

- ex>

  ```dart
  // This class is declared abstract and thus
  // can't be instantiated.
  abstract class AbstractContainer {
    // Define constructors, fields, methods...
    void updateChildren(); // Abstract method.
  }
  ```
  


<br>

### Abstract methods

추상 메서드는 추상 클래스 안에서만 존재할 수 있다.

> Instance, getter, and setter methods can be abstract, defining an interface but leaving its implementation up to other classes. Abstract methods can only exist in [abstract classes](https://dart.dev/guides/language/language-tour#abstract-classes).
>
> To make a method abstract, use a semicolon (;) instead of a method body:

- ex>

  ```dart
  abstract class Doer {
    // Define instance variables and methods...
  
    void doSomething(); // Define an abstract method.
  }
  
  class EffectiveDoer extends Doer {
    void doSomething() {
      // Provide an implementation, so the method is not abstract here...
    }
  }
  ```


<br>

### Implicit Interface

(자바와 달리 interface라는 키워드를 사용하지 않는다.)

모든 클래스가 암시적으로 인터페이스를 정의하게 된다.

> Every class implicitly defines an interface containing all the instance members of the class and of any interfaces it implements. If you want to create a class A that supports class B’s API without inheriting B’s implementation, class A should implement the B interface.
>
> A class implements one or more interfaces by declaring them in an `implements` clause and then providing the APIs required by the interfaces.

<br>

주의할점은 인터페이스 클래스를 implements 해서 클래스를 구현하려면 반드시 인터페이스 클래스의 변수와 메서드를 반드시 재정의 해야한다.

```dart
class Vehicle {
  String type = 'Vehicle';

  String printDetail() => 'Type of vehicle : $type';
}

class BMW implements Vehicle {

}

// vs code를 기준으로 이 경우에 BMW에 빨간줄이 생기고
// Missing concrete implementations of 'Vehicle.printDetail', 'getter Vehicle.type', // and 'setter Vehicle.type'. Try implementing the missing methods, or make the 
// class abstract.
// 이러한 에러를 발생시킨다
```

<br>

```dart
class Vehicle {
  String type = 'Vehicle';

  String printDetail() => 'Type of vehicle : $type';
}

class BMW implements Vehicle {
  // @override는 써도 되고 안 써도 된다
  @override
  String type = 'Car';

  @override
  String printDetail() => 'Type of vehicle : $type';
}

void main() {
  Vehicle v = Vehicle();
  BMW b = BMW();
  print(v.printDetail());
  print(b.printDetail());
}
// Type of vehicle : Vehicle
// Type of vehicle : Car
```

<br>

### Extending a Class

abstract 클래스를 implements 할 수도 있고 extends 할 수도 있다. 그러나 implements를 할 경우에는 abstract 클래스의 변수와 메서드를 모두 재정의해줘야 한다. 이때 문제가 될 수 있는 부분은 추후 abstract 클래스를 수정하게 되면 이 클래스를 implements 해서 사용하는 다른 클래스들에서 모두 에러가 발생할 수 있다. 그리고 모든 클래스들에서 재정의를 다시 해줘야하므로 유지 보수에 어려움이 발생할 수 있다.

abstract 클래스는 extends 해서 사용하거나 factory 생성자를 활용해서 사용하는게 좋을 듯 하다.

<br>

참고

https://dart.dev/guides/language/language-tour#abstract-classes

https://eunjin3786.tistory.com/408

https://limkydev.tistory.com/197

https://limkydev.tistory.com/188

https://codelabs.developers.google.com/codelabs/from-java-to-dart/#4

