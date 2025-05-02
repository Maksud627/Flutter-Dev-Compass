- Future (value available in future)

```dart
Future<String> fetchData() {
  return Future.delayed(Duration(seconds: 2), () => 'Data loaded');
}

void main() {
  fetchData().then((value) {
    print(value); // After 2 seconds: "Data loaded"
  });
}

// OR

Future<int> getNumber() async => 42;
```




