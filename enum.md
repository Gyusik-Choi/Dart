# Enum

```dart
enum Payments {
  cash, card, etc
}

void main() {
  String payWay = "card";
  List<Payments> payments = Payments.values;
  Iterable<Payments> p = payments.where((payment) {
    // return payWay == payment.toString();
    // => Payments.card
    // 아래는 Payments.card에서 Payments를 제거하는 방법이다
    return payWay == payment.toString().split('.').elementAt(1);
  });
  print(p);
  print(p.runtimeType);
}

// (Payments.card)
// WhereIterable<Payments>
```

<br>

```dart
enum Pay {
  cash, card, etc
}

String removeClassFromEnumValue(String value) {
  return value.split('.').elementAt(1);
}

void main() {
  String userPay = "card";
  Pay.values.forEach((element) {
    String payValue = removeClassFromEnumValue(element.toString());
    if (userPay == payValue) {
      print(payValue);
      print(payValue.runtimeType);
      // card
      // String
    }
  });
}
```

<br>

참고

https://techblog.woowahan.com/2527/

https://stackoverflow.com/questions/64887178/how-to-loop-over-an-enum-in-dart

https://dart.dev/guides/language/language-tour

https://dajoonee.tistory.com/30

https://velog.io/@kimbiyam/Flutter-enum-value-%EC%97%90-%EB%94%B0%EB%A5%B8-%EA%B0%92-%EB%B3%80%ED%99%98%ED%95%98%EA%B8%B0

https://masswhale.tistory.com/entry/Dart%EC%96%B8%EC%96%B4%EA%B3%B5%EB%B6%80-14Enum-Type