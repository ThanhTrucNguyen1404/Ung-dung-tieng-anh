# Testing Guide

## Cấu trúc Test

```
test/
├── widget_test.dart          # Basic widget tests
├── unit/                     # Unit tests
│   ├── validators_test.dart
│   └── date_time_helper_test.dart
└── integration/              # Integration tests (future)
```

## Chạy Tests

### Chạy tất cả tests
```bash
flutter test
```

### Chạy một file test cụ thể
```bash
flutter test test/unit/validators_test.dart
```

### Chạy với coverage
```bash
flutter test --coverage
```

### Xem coverage report
```bash
# Install lcov (Windows - choco)
choco install lcov

# Generate HTML report
genhtml coverage/lcov.info -o coverage/html

# Open report
start coverage/html/index.html
```

## Test Categories

### 1. Unit Tests ✅
Test các functions và utilities độc lập:
- ✅ `validators_test.dart` - Email, password validation
- ✅ `date_time_helper_test.dart` - Date formatting, streak calculation

### 2. Widget Tests (TODO)
Test UI components:
- `splash_page_test.dart`
- `login_page_test.dart`
- `vocabulary_card_test.dart`

### 3. BLoC Tests (TODO)
Test state management:
- `auth_bloc_test.dart`
- `vocabulary_bloc_test.dart`

### 4. Integration Tests (TODO)
Test end-to-end flows:
- Login flow
- Complete a lesson flow

## Mocking

### Firebase Mocking
```dart
// For testing with Firebase
import 'package:firebase_core_platform_interface/firebase_core_platform_interface.dart';
import 'package:flutter_test/flutter_test.dart';

void setupFirebaseAuthMocks() {
  TestWidgetsFlutterBinding.ensureInitialized();
  // Setup mock
}
```

### Network Mocking
```dart
// For testing with Dio
import 'package:mockito/mockito.dart';
import 'package:dio/dio.dart';

class MockDio extends Mock implements Dio {}
```

## Best Practices

1. **Arrange-Act-Assert Pattern**
```dart
test('description', () {
  // Arrange
  final input = 'test@example.com';
  
  // Act
  final result = Validators.validateEmail(input);
  
  // Assert
  expect(result, null);
});
```

2. **Group Related Tests**
```dart
group('Email Validation', () {
  test('valid email', () { ... });
  test('invalid email', () { ... });
});
```

3. **Use Descriptive Names**
```dart
// Good
test('Valid email should return null', () { ... });

// Bad
test('test1', () { ... });
```

## Coverage Goals

- Unit tests: 80%+
- Widget tests: 60%+
- Integration tests: Key flows

## CI/CD Integration

```yaml
# .github/workflows/test.yml
name: Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: subosito/flutter-action@v2
      - run: flutter pub get
      - run: flutter test --coverage
```

## Troubleshooting

### Lỗi: Firebase not initialized
```dart
// Add to test setup
setUpAll(() async {
  setupFirebaseAuthMocks();
});
```

### Lỗi: Platform binding not initialized
```dart
TestWidgetsFlutterBinding.ensureInitialized();
```

## Resources

- [Flutter Testing Guide](https://docs.flutter.dev/testing)
- [BLoC Testing](https://bloclibrary.dev/#/testing)
- [Mockito Documentation](https://pub.dev/packages/mockito)

