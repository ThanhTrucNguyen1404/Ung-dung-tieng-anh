# 🛠️ ANALYTICS TOOLS - HƯỚNG DẪN SỬ DỤNG

## 📊 HỆ THỐNG HOÀN CHỈNH

Bạn có **4 tools** để làm việc với Analytics System:

---

## 🎲 TOOL 1: Generate Sample Data

**File:** `lib/tools/generate_sample_analytics_data.dart`

### Mục đích
Tạo dữ liệu học tập THỰC TẾ và lưu vào Firebase Firestore.

### Chạy
```bash
flutter run -t lib/tools/generate_sample_analytics_data.dart -d edge
```

### Tool này tạo:
- ✅ 1 Test User (start từ 0: XP=0, Streak=0, Minutes=0)
- ✅ 20-90 Learning Sessions (30 ngày gần nhất)
- ✅ 30-90 Exercise Results với accuracy thật (30-100%)
- ✅ Tính toán XP, Streak, Progress theo công thức thực
- ✅ Lưu tất cả vào Firebase Collections:
  - `users`
  - `learning_sessions`
  - `exercise_results`

### Kết quả
- User ID mới được tạo
- Tổng XP tính từ exercise results
- Streak tính từ study pattern
- Skill Progress tính từ average accuracy

---

## 🔍 TOOL 2: Verify Analytics Data

**File:** `lib/tools/verify_analytics_data.dart`

### Mục đích
Kiểm tra xem Firebase có dữ liệu THẬT hay FAKE.

### Chạy
```bash
flutter run -t lib/tools/verify_analytics_data.dart -d edge
```

### Tool này kiểm tra:
- ✅ Users Collection (số lượng, sample data)
- ✅ Learning Sessions Collection
- ✅ Exercise Results Collection
- ✅ Vocabulary Collection (từ vựng đã import)
- ✅ Tính tổng XP THỰC TẾ từ database
- ✅ Kết luận: Có data thật hay chưa

### Kết quả
```
✅ CÓ DỮ LIỆU THẬT!
  ✓ Users profile có data
  ✓ Learning sessions có data
  ✓ Exercise results có data
  ⭐ Tổng XP tính từ database: 1,250
```

hoặc

```
⚠️ CHƯA CÓ DỮ LIỆU
Dashboard đang hiển thị dữ liệu mặc định hoặc fake.
```

---

## 📖 TOOL 3: Import Vocabulary

**File:** `lib/tools/import_vocabulary_ui.dart`

### Mục đích
Import 10,164 từ vựng (A1-C2) vào Firebase.

### Chạy
```bash
flutter run -t lib/tools/import_vocabulary_ui.dart -d edge
```

### Tool này import:
- ✅ 10,164 từ vựng từ 6 levels (A1, A2, B1, B2, C1, C2)
- ✅ Từ `vocabulary_seed_data.dart`
- ✅ Vào collection `vocabulary` trong Firestore
- ✅ Với progress bar real-time

### Kết quả
Vocabulary collection có 10,164 documents với fields:
- word, pronunciation, meaning
- partOfSpeech, level
- example, exampleTranslation
- synonyms, antonyms
- imageUrl

---

## 🎓 TOOL 4: Analytics Usage Example

**File:** `lib/examples/analytics_usage_example.dart`

### Mục đích
Demo cách sử dụng Analytics System với UI.

### Chạy
```bash
flutter run -t lib/examples/analytics_usage_example.dart -d edge
```

### Tool này có:
- ✅ 8 button để test scenarios:
  1. Start Lesson
  2. Complete Exercise
  3. End Session
  4. Show Dashboard ← **Xem dashboard với real data**
  5. Weekly Report
  6. Detect Weak Skills (AI)
  7. Export Data (GDPR)
  8. FULL LEARNING FLOW ← **Test toàn bộ flow**

### Kết quả
Hiển thị dashboard với:
- XP, Streak, Learning Hours
- Skill Progress (6 kỹ năng)
- Heatmap 30 ngày
- AI weak skill recommendations

---

## 🧪 TOOL 5: Analytics Test Suite

**File:** `lib/examples/analytics_test.dart`

### Mục đích
Test tự động toàn bộ Analytics System.

### Chạy
```bash
flutter run -t lib/examples/analytics_test.dart -d edge
```

### Tool này test:
- ✅ Create user với giá trị mặc định = 0
- ✅ Learning session tracking
- ✅ Exercise result saving
- ✅ Dashboard data generation
- ✅ Weekly report
- ✅ AI analysis

### Kết quả
Console logs real-time với status:
- ✅ PASS: Test passed
- ❌ FAIL: Test failed

---

## 📋 WORKFLOW ĐỀ XUẤT

### Scenario 1: Lần đầu setup

```bash
# Bước 1: Import từ vựng vào Firebase
flutter run -t lib/tools/import_vocabulary_ui.dart -d edge

# Bước 2: Generate sample analytics data
flutter run -t lib/tools/generate_sample_analytics_data.dart -d edge

# Bước 3: Verify data đã có trong Firebase
flutter run -t lib/tools/verify_analytics_data.dart -d edge

# Bước 4: Xem dashboard với real data
flutter run -t lib/examples/analytics_usage_example.dart -d edge
→ Bấm button "📊 Show Dashboard"
```

### Scenario 2: Kiểm tra hệ thống

```bash
# Verify data hiện tại
flutter run -t lib/tools/verify_analytics_data.dart -d edge

# Nếu chưa có data → Generate
flutter run -t lib/tools/generate_sample_analytics_data.dart -d edge

# Test toàn bộ system
flutter run -t lib/examples/analytics_test.dart -d edge
```

### Scenario 3: Demo cho giáo viên

```bash
# Chạy example app
flutter run -t lib/examples/analytics_usage_example.dart -d edge

# Scenarios để demo:
1. Bấm "🎓 FULL LEARNING FLOW" → Xem toàn bộ flow
2. Bấm "📊 Show Dashboard" → Xem dashboard
3. Bấm "6. Detect Weak Skills (AI)" → Xem AI analysis
```

---

## 📊 KẾT QUẢ MONG ĐỢI

### Sau khi chạy Tool 1 + 2:

```
Firebase Firestore:
├── users (1+ documents)
│   └── total_xp: 500-2000
│   └── current_streak: 0-30 days
│   └── total_learning_minutes: 100-1000
│
├── learning_sessions (20-90 documents)
│   └── duration_minutes: 10-50
│   └── completed: true
│
├── exercise_results (30-90 documents)
│   └── accuracy: 0.3-1.0
│   └── xp_earned: 25-80
│
└── vocabulary (10,164 documents)
    └── All A1-C2 words
```

### Dashboard hiển thị:

```
┌─────────────────────────────────┐
│  ⭐ 1,250 XP                     │
│  🔥 5 Streak                     │
│  ⏱️ 12h Learning                 │
└─────────────────────────────────┘

Từ vựng    ████████░░  75%
Ngữ pháp   ██████░░░░  60%
Nghe       ████░░░░░░  45%
Nói        ███░░░░░░░  30%
Đọc        █████░░░░░  55%
Viết       ████░░░░░░  40%
```

---

## 🎯 TÍCH HỢP VÀO APP CHÍNH

Sau khi verify data OK, integrate Analytics vào app:

### 1. Lesson Page

```dart
@override
void initState() {
  super.initState();
  
  // Bắt đầu track
  sessionTracking.startSession(
    userId: currentUser.uid,
    skill: SkillType.vocabulary,
    lessonId: widget.lessonId,
  );
}

@override
void dispose() {
  // Kết thúc track
  sessionTracking.endSession();
  super.dispose();
}
```

### 2. Exercise Complete

```dart
void onExerciseComplete() {
  sessionTracking.saveExerciseResult(
    userId: currentUser.uid,
    skill: SkillType.vocabulary,
    correctAnswers: userCorrectAnswers,
    totalQuestions: totalQuestions,
    completed: true,
  );
}
```

### 3. Home Page - Add Dashboard Button

```dart
FloatingActionButton(
  onPressed: () => Navigator.push(
    context,
    MaterialPageRoute(
      builder: (context) => DashboardPage(
        userId: currentUser.uid,
        analyticsService: analyticsService,
      ),
    ),
  ),
  child: Icon(Icons.analytics),
)
```

---

## 🔥 TIPS & TRICKS

### Tip 1: Reset Data
Nếu muốn xóa data và tạo lại:
1. Vào Firebase Console
2. Xóa collections: `users`, `learning_sessions`, `exercise_results`
3. Chạy lại Tool 1

### Tip 2: Multiple Test Users
Tool 1 tạo user ID unique mỗi lần chạy:
```
test_user_1735200000000
test_user_1735200100000
test_user_1735200200000
```

### Tip 3: Verify After Changes
Sau mỗi lần thay đổi code, chạy:
```bash
flutter run -t lib/examples/analytics_test.dart -d edge
```

---

## ✅ CHECKLIST

- [ ] Tool 1: Generate data ✅
- [ ] Tool 2: Verify data có trong Firebase ✅
- [ ] Tool 3: Import vocabulary (nếu chưa) ✅
- [ ] Tool 4: Test với example app ✅
- [ ] Tool 5: Run test suite ✅
- [ ] Integrate vào lesson pages
- [ ] Add dashboard button
- [ ] Test với real user flow

---

## 📞 TROUBLESHOOTING

### Vấn đề: Tool không chạy

**Nguyên nhân:** Thiếu Firebase config

**Giải pháp:**
```bash
flutterfire configure
```

### Vấn đề: Data không lưu vào Firebase

**Nguyên nhân:** Firestore rules chặn

**Giải pháp:** Update Firestore rules:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true; // Test only!
    }
  }
}
```

### Vấn đề: Dashboard hiển thị 0

**Nguyên nhân:** Chưa có data

**Giải pháp:**
1. Chạy Tool 1 để generate data
2. Chạy Tool 2 để verify
3. Refresh dashboard

---

## 🎉 KẾT LUẬN

Bạn có đủ tools để:

✅ Tạo dữ liệu THỰC (Tool 1)  
✅ Verify dữ liệu (Tool 2)  
✅ Import vocabulary (Tool 3)  
✅ Demo system (Tool 4)  
✅ Test tự động (Tool 5)  

**HỆ THỐNG HOÀN CHỈNH - SẴN SÀNG SỬ DỤNG! 🚀**

---

**Version:** 1.0.0  
**Last Updated:** December 26, 2025  
**Status:** ✅ Production Ready

