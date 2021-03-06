# Factory

디자인 패턴 중 하나인 싱글톤 패턴을 구현할 때 사용할 수 있는 키워드이다. 공식문서에서는 하나의 인스턴스만 생성하고 싶을 때 사용하라고 권장한다.

> Use the `factory` keyword when implementing a constructor that doesn’t always create a new instance of its class.

<br>

에러

```dart
class CommonVariable {
  late bool isDownloaded = false;
  static final CommonVariable _commonVariable = CommonVariable();
  
  factory CommonVariable() => _commonVariable;
  
  void changeIsDownloaded() => isDownloaded = !isDownloaded;
}


void main() {
  CommonVariable commonVariable = CommonVariable();
  print(commonVariable.isDownloaded);
  commonVariable.changeIsDownloaded();
  print(commonVariable.isDownloaded);
  // Uncaught RangeError: Maximum call stack size exceededError: RangeError: Maximum call stack size exceeded
}

```

<br>

정상 출력

```dart
class CommonVariable {
  late bool isDownloaded;
  // static 변수 선언(인스턴스 변수가 아닌 클래스 변수)
  // 최초로 한번만 생성되고 공용으로 사용할 수 있음
  // ! 클래스 변수는 생성자 없이 접근할 수 있다 !
  // 처음에 CommonVariable 객체 생성할때 이곳이 호출되어서 
  // CommonVaribale._internal() 을 통해 isDownloaded가 false로 세팅된다
  // 이후 또 다른 객체를 생성하려고 하더라도 factory로 호출이 되어서
  // 이미 생성한 _commonVariable 객체가 반환된다
  // 그리고 _commonVariable 객체에 담긴 isDownloaded 값은 공유된다
  static final CommonVariable _commonVariable = CommonVariable._internal();
  
  factory CommonVariable() => _commonVariable;

  CommonVariable._internal() {
      isDownloaded = false;
  }

  void changeIsDownloaded() => isDownloaded = !isDownloaded;
}


void main() {
  CommonVariable commonVariable1 = CommonVariable();
  CommonVariable commonVariable2 = CommonVariable();
  
  print(commonVariable1.isDownloaded);
  print(commonVariable2.isDownloaded);
  
  commonVariable1.changeIsDownloaded();
  
  print(commonVariable1.isDownloaded);
  print(commonVariable2.isDownloaded);
}

// false
// false

// true
// true
```

<br>

개인적으로는 전역 변수를 다룰 때 사용하기 좋을 것 같다고 생각한다. 상태관리 라이브러리(ex> 플러터의 GetX 등) 쓴다면 factory가 불필요해 보일 수 있겠지만 전역 변수만 여러개 사용되는 환경에서 클래스로 전역 변수들만 따로 묶어서 사용한다면 여러 페이지에서 해당 변수의 값에 보다 편리하게 접근하여 변경하고 이를 공유할 수 있는 장점이 있다.

<br>

위의 예시처럼 _internal() 함수 안에서 isDownloaded를 초기화하지 않아도 값은 공유된다.

```dart
class CommonVariable {
  bool isDownloaded = false;

  static final CommonVariable _commonVariable = CommonVariable._internal();
  CommonVariable._internal();
  
  factory CommonVariable() => _commonVariable;

  void changeIsDownloaded() => isDownloaded = !isDownloaded;
}


void main() {
  CommonVariable commonVariable1 = CommonVariable();
  CommonVariable commonVariable2 = CommonVariable();
  
  print(commonVariable1.isDownloaded);
  print(commonVariable2.isDownloaded);
  
  commonVariable1.changeIsDownloaded();
  
  print(commonVariable1.isDownloaded);
  print(commonVariable2.isDownloaded);
}

// false
// false

// true
// true
```



<br>

참고

https://dart.dev/guides/language/language-tour#factory-constructors

https://roseline.oopy.io/dev/flutter-tips/what-is-factory

https://another-light.tistory.com/77

https://coding-factory.tistory.com/524

https://sudarlife.tistory.com/entry/%ED%94%8C%EB%9F%AC%ED%84%B0%EB%8B%A4%ED%8A%B8-%ED%8C%A9%ED%86%A0%EB%A6%ACFactory-%ED%8C%A8%ED%84%B4-%EC%98%88%EC%A0%9C%EB%A1%9C-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0
