# future, async & await, whenComplete

await 사용유무에 따른 Future.delayed의 결과 차이와 whenComplete를 다루려고 한다. 그 전에 핵심 개념들을 먼저 정리해본다.

<br>

### future

Dart에서 future란 공식문서에 따르면

> A future (lower case “f”) is an instance of the [Future](https://api.dart.dev/stable/dart-async/Future-class.html) (capitalized “F”) class. A future represents the result of an asynchronous operation, and can have two states: uncompleted or completed.

Future 클래스의 인스턴스다. 비동기 요청에 대한 결과를 나타내고, uncompleted 혹은 completed 두 가지 상태를 가질 수 있다.

<br>

### async & await

>  The `async` and `await` keywords provide a declarative way to define asynchronous functions and use their results. Remember these two basic guidelines when using `async` and `await`:
>
> - To define an async function, add `async` before the function body:
> - The `await` keyword works only in `async` functions.

async & await는 비동기 함수에 대한 선언적 방법과 그 결과 값을 활용하는 선언적 방법을 제공한다.

<br>

### whenComplete

Future클래스의 메서드다.

> This is the asynchronous equivalent of a "finally" block.

이게 가장 whenComplete를 이해하는 중요한 문장이라고 생각한다. try-catch-finally에서 finally에 대응하는 개념이 whenComplete다.

<br>

### Future.delayed

> Creates a future that runs its computation after a delay.
>
> The `computation` will be executed after the given `duration` has passed, and the future is completed with the result of the computation.

<br>

### 예시 코드

```dart
Future futurePrint(Duration dur, String msg) async {
  return Future.delayed(dur).then((onValue) => msg);
}

void main() async {
  try {
    print("A");
    await futurePrint(Duration(seconds: 1), "B")
      .then((status) => print(status));
    print("C");
  } catch(err) {
    print(err); 
  }
}

// A
// B
// C
```

<br>

```dart
Future futurePrint(Duration dur, String msg) async {
  return Future.delayed(dur).then((onValue) => msg);
}

void main() async {
  try {
    print("A");
    futurePrint(Duration(seconds: 1), "B")
      .then((status) => print(status));
    print("C");
  } catch(err) {
    print(err); 
  }
}

// A
// C
// B
```

<br>

```dart
Future futurePrint(Duration dur, String msg) async {
  return await Future.delayed(dur).then((onValue) => msg);
}

void main() async {
  try {
    print("A");
    futurePrint(Duration(seconds: 1), "B")
      .then((status) => print(status));
    print("C");
  } catch(err) {
    print(err); 
  }
}

// A
// C
// B
```

<br>

1 VS 2, 3으로 보면 된다. 

가장 큰 차이는 main 함수에서 futurePrint 함수를 호출할 때 await 사용 유무다. Future.delayed가 Future를 생성하는 것은 맞지만 그렇다고 이게 비동기 요청 다음 코드의 실행을 막아주지는 못한다. 한줄 한줄 순서대로 실행 하고 싶다면 await가 반드시 사용되어야 한다. futurePrint에서 await 키워드를 사용하는 것(3번째 예시 코드)은 원하는 효과를 얻지 못하고 함수를 호출하는 부분에서 await를 실행해줘야 한다.

<br>

참고

https://dart.dev/codelabs/async-await

https://api.flutter.dev/flutter/dart-async/Future-class.html

https://api.flutter.dev/flutter/dart-async/Future/whenComplete.html

https://api.flutter.dev/flutter/dart-async/Future/Future.delayed.html

https://pcseob.tistory.com/3
