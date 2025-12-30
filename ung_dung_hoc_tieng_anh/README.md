# 🎓 Ứng Dụng Học & Ôn Tập Tiếng Anh

Ứng dụng học tiếng Anh toàn diện với AI cá nhân hóa, gamification và đồng bộ đa thiết bị.

## 🌟 Tính năng

### 📚 Học tập
- **Vocabulary** - Flashcard với Spaced Repetition
- **Grammar** - Ngữ pháp theo tình huống
- **Listening** - Luyện nghe với nhiều cấp độ
- **Speaking** - AI chấm phát âm
- **Reading** - Bài đọc phân theo level
- **Writing** - Viết đoạn văn với AI feedback

### 🎮 Gamification
- Hệ thống XP và Level
- Streak ngày học liên tiếp
- Achievement & Badges
- Rương thưởng

### 🤖 AI Cá nhân hóa
- Phân tích điểm yếu
- Gợi ý bài học phù hợp
- Lộ trình học tối ưu
- Nhắc học thông minh

### 📊 Thống kê & Phân tích
- Biểu đồ tiến độ
- Phân tích kỹ năng
- Lịch sử học tập
- Dự đoán level up

### 🔐 Người dùng
- Firebase Authentication
- Đồng bộ đa thiết bị
- Offline mode với Hive
- Dark/Light theme

## 🏗️ Kiến trúc

```
lib/
├── main.dart                 # Entry point
├── app/
│   ├── app.dart             # MaterialApp setup
│   └── di.dart              # Dependency Injection
├── core/
│   ├── constants/           # Colors, app constants
│   ├── theme/               # Light/Dark theme
│   ├── routes/              # App router
│   ├── utils/               # Validators, helpers
│   ├── widgets/             # Reusable widgets
│   └── services/            # Audio, notification, analytics
├── domain/
│   ├── entities/            # Business entities
│   ├── repositories/        # Repository interfaces
│   └── usecases/            # Business logic
├── data/
│   ├── models/              # Data models
│   ├── datasources/
│   │   ├── local/           # Hive, SharedPreferences
│   │   └── remote/          # Firebase
│   └── repositories/        # Repository implementations
├── presentation/
│   ├── bloc/                # State management
│   ├── pages/               # UI screens
│   └── widgets/             # Page-specific widgets
├── backend/
│   └── api/                 # Custom API client
└── ai/
    ├── recommendation_engine.dart
    └── error_analysis_engine.dart
```

### 📐 Design Patterns
- **Clean Architecture** - Tách biệt Domain/Data/Presentation
- **BLoC Pattern** - State management
- **Repository Pattern** - Data abstraction
- **Dependency Injection** - GetIt
- **SOLID Principles**

## 🚀 Bắt đầu

### Yêu cầu
- Flutter SDK >= 3.10.1
- Dart SDK >= 3.10.1
- Firebase project
- Android Studio / VS Code

### Cài đặt

1. **Clone repository**
```bash
git clone <your-repo-url>
cd ung_dung_hoc_tieng_anh
```

2. **Cài đặt dependencies**
```bash
flutter pub get
```

3. **Cấu hình Firebase**
```bash
# Đã cấu hình với firebase_options.dart
# Nếu cần cấu hình lại:
flutterfire configure --project=ung-dung-hoc-tieng-anh-348fd
```

4. **Chạy ứng dụng**
```bash
# Android
flutter run

# iOS
flutter run -d ios

# Web
flutter run -d chrome
```

## 📦 Dependencies chính

### State Management & DI
- `flutter_bloc: ^8.1.6` - State management
- `get_it: ^8.0.0` - Dependency injection
- `equatable: ^2.0.5` - Value equality

### Firebase
- `firebase_core: ^3.6.0`
- `cloud_firestore: ^5.4.4`
- `firebase_storage: ^12.3.2`
- `firebase_auth: ^5.3.1`

### Networking & Storage
- `dio: ^5.7.0` - HTTP client
- `hive: ^2.2.3` - Local database
- `shared_preferences: ^2.3.2`

### Audio & Media
- `just_audio: ^0.9.40` - Audio player
- `speech_to_text: ^7.0.0` - Speech recognition
- `record: ^5.1.2` - Audio recording

### UI & Animation
- `lottie: ^3.1.3` - Animations
- `fl_chart: ^0.69.2` - Charts
- `shimmer: ^3.0.0` - Loading effects
- `cached_network_image: ^3.4.1`

## 🧪 Testing

```bash
# Unit tests
flutter test

# Widget tests
flutter test test/widget_test.dart

# Integration tests
flutter test integration_test/
```

## 🔧 Build

### Android APK
```bash
flutter build apk --release
```

### iOS IPA
```bash
flutter build ios --release
```

### Web
```bash
flutter build web --release
```

## 📱 Supported Platforms
- ✅ Android 5.0+
- ✅ iOS 12.0+
- ✅ Web (Chrome, Firefox, Safari)
- ✅ Windows 10+
- ✅ macOS 10.14+

## 🎯 User Flow

```
Splash → Onboarding → Login/Register → Placement Test → Home
                                                           ↓
                                      ← Profile ← Statistics ← Lessons
```

## 🗄️ Database Schema

### Firestore Collections
- `users` - User profiles
- `lessons` - Learning content
- `vocabulary` - Word database
- `grammar` - Grammar rules
- `achievements` - Badges & rewards
- `user_progress` - Learning history

### Hive Boxes
- `vocabulary_box` - Cached words
- `lessons_box` - Offline lessons
- `progress_box` - Local progress
- `settings_box` - App preferences

## 🔐 Environment Variables

Create `.env` file:
```env
API_BASE_URL=https://api.ungdunghoctiengcanh.com
FIREBASE_API_KEY=your_api_key
```

## 🤝 Contributing

1. Fork the project
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

## 📝 License

This project is licensed under the MIT License.

## 👥 Authors

- **Your Name** - *Initial work*

## 🙏 Acknowledgments

- Flutter team
- Firebase team
- Open source community

## 📞 Contact

- Email: your.email@example.com
- Website: https://yourwebsite.com

---

⭐ **Đánh giá nếu bạn thấy hữu ích!**

Made with ❤️ using Flutter
