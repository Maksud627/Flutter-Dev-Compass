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

- async/await

```dart
Future<void> loadData() async {
  print('Loading...');
  String result = await fetchData(); // wait until done
  print(result);
}

// Loading...
// Data loaded
```
- Stream (handling multiple async values)

```dart
Stream<int> countStream() async* {
  for (int i = 1; i <= 3; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}
```

