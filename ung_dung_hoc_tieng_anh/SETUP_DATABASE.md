# 🗄️ Hướng Dẫn Setup Database

## 1. Firebase Firestore Setup

### Bước 1: Truy cập Firebase Console
1. Mở [Firebase Console](https://console.firebase.google.com)
2. Chọn project: `ung-dung-hoc-tieng-anh-348fd`

### Bước 2: Kích hoạt Firestore
1. Vào menu **Firestore Database**
2. Click **Create database**
3. Chọn **Start in test mode** (cho development)
4. Chọn location: **asia-southeast1** (Singapore)
5. Click **Enable**

### Bước 3: Setup Firestore Rules
1. Vào tab **Rules**
2. Copy nội dung từ file `firestore.rules` 
3. Paste vào editor
4. Click **Publish**

### Bước 4: Tạo Collections cơ bản

#### Collection: users
```javascript
// Document structure
{
  email: "user@example.com",
  displayName: "User Name",
  level: "A1",
  xp: 0,
  currentLevel: 1,
  streakDays: 0,
  lastStudyDate: null,
  createdAt: Timestamp
}
```

#### Collection: lessons
```javascript
{
  title: "Bài học 1",
  description: "Mô tả bài học",
  type: "vocabulary", // vocabulary, grammar, listening, etc.
  level: "A1",
  durationMinutes: 15,
  xpReward: 20,
  topics: ["family", "daily_life"],
  isPremium: false,
  createdAt: Timestamp
}
```

#### Collection: vocabulary
```javascript
{
  word: "apple",
  meaning: "quả táo",
  pronunciation: "/ˈæp.əl/",
  audioUrl: "https://...",
  imageUrl: "https://...",
  example: "I eat an apple every day.",
  exampleTranslation: "Tôi ăn một quả táo mỗi ngày.",
  partOfSpeech: "noun",
  level: "A1",
  synonyms: [],
  antonyms: [],
  createdAt: Timestamp
}
```

### Bước 5: Thêm dữ liệu mẫu

**Option 1: Thủ công qua Firebase Console**
1. Vào collection tương ứng
2. Click **Add document**
3. Nhập dữ liệu theo structure trên

**Option 2: Import từ file JSON**
```bash
# Tạo file import_data.json với dữ liệu mẫu
# Sử dụng Firebase CLI
firebase firestore:import import_data.json
```

## 2. Firebase Storage Setup

### Bước 1: Kích hoạt Storage
1. Vào menu **Storage**
2. Click **Get started**
3. Chọn **Start in test mode**
4. Click **Done**

### Bước 2: Tạo folder structure
```
storage/
├── audio/
│   ├── vocabulary/
│   └── listening/
├── images/
│   ├── vocabulary/
│   └── lessons/
└── user_uploads/
    └── speaking/
```

### Bước 3: Setup Storage Rules
```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    // Audio files - Public read
    match /audio/{allPaths=**} {
      allow read: if true;
      allow write: if false;
    }
    
    // Images - Public read
    match /images/{allPaths=**} {
      allow read: if true;
      allow write: if false;
    }
    
    // User uploads - Only owner can access
    match /user_uploads/{userId}/{allPaths=**} {
      allow read, write: if request.auth != null && 
                            request.auth.uid == userId;
    }
  }
}
```

## 3. Firebase Authentication Setup

### Bước 1: Kích hoạt Auth
1. Vào menu **Authentication**
2. Click **Get started**

### Bước 2: Enable Auth Providers
1. Vào tab **Sign-in method**
2. Enable **Email/Password**
3. (Optional) Enable **Google** nếu cần

### Bước 3: Authorized domains
Thêm domain của bạn vào **Authorized domains** nếu deploy lên web

## 4. Test Database Connection

### Chạy test script
```bash
# Test Firestore và Hive
flutter run lib/test_database.dart
```

### Kết quả mong đợi
```
✅ Kết nối Firebase thành công
✅ Tạo document thành công
✅ Đọc document thành công
✅ Cập nhật document thành công
✅ Query thành công
✅ Xóa document thành công
✅ FIRESTORE: Tất cả tests đều PASS!

✅ Mở box thành công
✅ Ghi dữ liệu thành công
✅ Đọc dữ liệu thành công
✅ Cập nhật thành công
✅ Xóa dữ liệu thành công
✅ HIVE: Tất cả tests đều PASS!
```

## 5. Troubleshooting

### Lỗi: Permission denied
**Nguyên nhân:** Firestore Rules chưa được setup
**Giải pháp:** 
1. Vào Firestore Rules
2. Copy rules từ `firestore.rules`
3. Publish

### Lỗi: Network error
**Nguyên nhân:** Không có internet hoặc Firebase chưa được init
**Giải pháp:**
1. Kiểm tra kết nối internet
2. Kiểm tra `firebase_options.dart`
3. Chạy lại `flutterfire configure`

### Lỗi: Hive box not found
**Nguyên nhân:** Hive chưa được init
**Giải pháp:**
```dart
await Hive.initFlutter();
```

## 6. Dữ liệu mẫu để test

### Tạo user test
```dart
// Qua app: Register với email
Email: test@example.com
Password: test123456
```

### Thêm vocabulary mẫu (Firebase Console)
```javascript
{
  "word": "hello",
  "meaning": "xin chào",
  "pronunciation": "/həˈloʊ/",
  "example": "Hello, how are you?",
  "exampleTranslation": "Xin chào, bạn khỏe không?",
  "partOfSpeech": "interjection",
  "level": "A1",
  "createdAt": new Date()
}
```

### Thêm lesson mẫu
```javascript
{
  "title": "Chào hỏi cơ bản",
  "description": "Học cách chào hỏi trong Tiếng Anh",
  "type": "vocabulary",
  "level": "A1",
  "durationMinutes": 10,
  "xpReward": 15,
  "topics": ["greetings"],
  "isPremium": false,
  "createdAt": new Date()
}
```

## 7. Monitoring

### Kiểm tra usage
1. Vào Firebase Console
2. Check **Usage** tab trong Firestore/Storage
3. Monitor số lượng reads/writes

### Setup alerts
1. Vào **Budget & alerts**
2. Setup ngưỡng cảnh báo
3. Nhận email khi vượt quota

## 8. Production Considerations

### Security
- [ ] Update Firestore Rules từ test mode sang production
- [ ] Enable App Check
- [ ] Setup backup tự động

### Performance
- [ ] Create indexes cho queries phức tạp
- [ ] Enable caching
- [ ] Optimize read/write operations

### Cost
- [ ] Monitor usage thường xuyên
- [ ] Implement pagination
- [ ] Cache dữ liệu với Hive

---

✅ **Setup xong! Giờ có thể bắt đầu dev app.**

