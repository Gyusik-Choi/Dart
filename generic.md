# Dart - generic

> 제네릭은 타입에 대한 제약과 유연성을 줄 수 있다.

<br>

### 제네릭의 타입에 대한 제약

"Generics are often required for type safety"

"Properly specifying generic types results in better generated code"

```dart
List<String> names = [];
names.add("dart");
// "dart"
```

<br>

```dart
List<String> names = [];
names.add(1);
// Error: The argument type 'int' can't be assigned to the parameter type 'String'.
```

<br>

### 제네릭의 타입에 대한 유연성

"Another reason for using generics is to reduce code duplication"

"코드에 대한 재사용성이 좋아진다"

<br>

#### 클래스 

생성자로 넣어준 param을 리턴하는 함수의 타입만 다르다. 두 개의 별도 클래스를 작성할 필요가 없다.

```dart
class Person {
  T getName<T>(T param) {
    return param;
  }
}

void main() {
  var person = Person();
  print(person.getName<String>('Kim'));
  // "kim"
  print(person.getName<int>(1));
  // 1
}
```

<br>

#### 추상 클래스

```dart
abstract class Cache<T> {
  T getByKey(String key);
  void setByKey(String key, T value);
}

class AnotherCache extends Cache<int> {
  @override
  int getByKey(String key) {
    return 1;
  }

  @override
  int setByKey(String key, int value) {
    return value;
  }
}

class AnotherCache2 extends Cache<String> {
  @override
  String getByKey(String key) {
    return key;
  }
  
  // 추상 클래스를 extends 할 경우 반드시 재정의를 해줘야 하는데, 리턴 타입을 바꿀 수 있다. void에서 List로 바꿨음.
  @override
  List setByKey(String key, String value) {
    return [];
  }
}

void main() {
  AnotherCache a = AnotherCache();
  int k = a.setByKey("abc", 1);
  print(k);
  // 1

  AnotherCache2 a2 = AnotherCache2();
  List emptyList = a2.setByKey("def", "ghi");
  print(emptyList);
  // []
}
```

<br>

### 제네릭의 타입에 대한 제약과 유연성

#### extends

extends는 클래스 파라미터 타입에 대한 제약을 주면서 동시에 유연성도 줄 수 있다.

<br>

##### - 제약

"When implementing a generic type, you might want to limit the types that can be provided as arguments, so that the argument must be a subtype of a particular type. You can do this using `extends`."

<br>

dart 에서 [Object?](https://dart.dev/null-safety/understanding-null-safety#top-and-bottom) 와 Object는 다르다. Object?는 nullable 한데 Object는 non-nullable이면서 Object?의 하위 타입이다.

Object를 extends하여 T를 non-nullable하게 만들었다.

```dart
class Foo<T extends Object> {
  // Any type provided to Foo for T must be non-nullable.
}
```

<br>

##### - 유연성

"You can use `extends` with other types besides `Object`. Here’s an example of extending `SomeBaseClass`, so that members of `SomeBaseClass` can be called on objects of type `T`"

"It’s OK to use `SomeBaseClass` or any of its subtypes as the generic argument"

<br>

 Foo의 클래스 파라미터 타입으로 SomeBaseClass 뿐만 아니라 SomeBaseClass를 extends 하는 Extender 클래스도 지정할 수 있다. 그래서 인스턴스를 생성할때 제네릭 변수(generic argument)로 SomeBaseClass와 함께 Extender도 지정 가능하다.

```dart
class Foo<T extends SomeBaseClass> {
  // Implementation goes here...
  String makeString() => "I'm instance of 'Foo<$T>'";
}

abstract class SomeBaseClass {
  String sayHello();
}

class Extender extends SomeBaseClass {
  @override
  String sayHello() => "hi";

  int sayNumber() => 1;
}

void main() {
  Foo<SomeBaseClass> someBaseClassFoo = Foo<SomeBaseClass>();
  String val = someBaseClassFoo.makeString();
  print(val);

  Foo<Extender> extenderFoo = Foo<Extender>();
  String value = extenderFoo.makeString();
  print(value);
}
```

<br>

Foo의 인스턴스를 생성할때 제네릭 변수를 지정해주지 않아도 된다. 이때 foo의 타입은 Foo< SomeBaseClass >가 된다.

```dart
  var foo = Foo();
  print(foo.makeString());
  print(foo);
  // I'm instance of 'Foo<SomeBaseClass>'
  // Instance of 'Foo<SomeBaseClass>'
```

<br>

단, 무조건 Foo < SomeBaseClass >로 강제 되는 것은 아니다. var 대신 타입을 지정하면 SomeBaseClass, Extender 모두 가능하다.

```dart
Foo<SomeBaseClass> foo = Foo();
print(foo);
// Instance of 'Foo<SomeBaseClass>'
```

```dart
Foo<Extender> foo = Foo();
print(foo);
// Instance of 'Foo<Extender>'
```

<br>

### 제네릭 메서드

> "Initially, Dart’s generic support was limited to classes. A newer syntax, called *generic methods*, allows type arguments on methods and functions"

```dart
T first<T>(T ts) {
  T a = ts;
  return a;
}

void main() {
  print(first("a"));
  // "a"
  print(first(1));
  // 1
}
```



<br>

참고

https://dart.dev/guides/language/language-tour#generics

https://iosroid.tistory.com/50

https://st-lab.tistory.com/153

https://jeong-pro.tistory.com/100
