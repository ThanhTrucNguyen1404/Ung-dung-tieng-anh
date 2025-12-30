# 🚀 ANALYTICS SYSTEM - QUICK START

## ⚡ HƯỚNG DẪN NHANH 5 PHÚT

### 📋 Yêu cầu

- ✅ Firebase đã setup
- ✅ Firestore đã enable
- ✅ Flutter SDK

---

## 1️⃣ CÀI ĐẶT DEPENDENCIES

Thêm vào `pubspec.yaml`:

```yaml
dependencies:
  cloud_firestore: ^4.13.3
  equatable: ^2.0.5
  path_provider: ^2.1.1
```

Chạy:

```bash
flutter pub get
```

---

## 2️⃣ TẠO FIRESTORE COLLECTIONS

### Tạo tự động khi user đầu tiên học

Không cần làm gì! Collections sẽ tự động được tạo khi:
- User đăng ký → tạo document trong `users`
- User học bài → tạo document trong `learning_sessions`
- User làm bài tập → tạo document trong `exercise_results`

### Hoặc tạo thủ công (Optional)

Vào Firebase Console → Firestore Database → Create collection:

1. **users**
2. **learning_sessions**
3. **exercise_results**

---

## 3️⃣ KHỞI TẠO SERVICES

Trong file `main.dart` hoặc dependency injection:

```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import 'data/repositories/analytics_repository_impl.dart';
import 'domain/services/analytics_service.dart';
import 'domain/services/session_tracking_service.dart';

// Khởi tạo
final analyticsRepository = AnalyticsRepositoryImpl();
final analyticsService = AnalyticsService(analyticsRepository);
final sessionTracking = SessionTrackingService(analyticsRepository);
```

---

## 4️⃣ TRACK HỌC TẬP (3 BƯỚC ĐƠN GIẢN)

### ✅ Bước 1: Bắt đầu session

Trong `initState()` của Lesson Page:

```dart
@override
void initState() {
  super.initState();
  
  // Bắt đầu track
  sessionTracking.startSession(
    userId: currentUser.uid,
    skill: SkillType.vocabulary,  // hoặc listening, speaking, etc.
    lessonId: widget.lessonId,
  );
}
```

### ✅ Bước 2: Lưu kết quả bài tập

Khi user hoàn thành bài tập:

```dart
void onExerciseComplete() {
  sessionTracking.saveExerciseResult(
    userId: currentUser.uid,
    skill: SkillType.vocabulary,
    correctAnswers: userCorrectAnswers,  // Số câu đúng THỰC TẾ
    totalQuestions: totalQuestions,      // Tổng số câu
    completed: true,
    lessonId: widget.lessonId,
  );
}
```

### ✅ Bước 3: Kết thúc session

Trong `dispose()` của Lesson Page:

```dart
@override
void dispose() {
  // Kết thúc track
  sessionTracking.endSession();
  super.dispose();
}
```

---

## 5️⃣ HIỂN THỊ DASHBOARD

### Thêm button vào Home Page:

```dart
FloatingActionButton(
  onPressed: () {
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => DashboardPage(
          userId: currentUser.uid,
          analyticsService: analyticsService,
        ),
      ),
    );
  },
  child: const Icon(Icons.analytics),
  tooltip: 'Xem Dashboard',
)
```

**Xong! Đơn giản vậy thôi! 🎉**

---

## 📊 DASHBOARD SẼ HIỂN THỊ

✅ **Tổng XP** (từ bài tập thực tế)  
✅ **Streak** (số ngày học liên tục)  
✅ **Thời gian học** (tính từ sessions)  
✅ **Tiến độ 6 kỹ năng** (từ accuracy thực tế)  
✅ **Heatmap 30 ngày** (màu xanh đậm = học nhiều)  
✅ **AI phát hiện kỹ năng yếu** (gợi ý cải thiện)  

---

## 🧪 TEST NHANH

Chạy example app để test:

```bash
flutter run -t lib/examples/analytics_usage_example.dart
```

Hoặc test từng function:

```dart
final example = AnalyticsUsageExample();

// Test full flow (1 bài học hoàn chỉnh)
await example.exampleComplete_FullLearningFlow();
```

---

## 🎯 CÁC SKILL TYPE

```dart
SkillType.vocabulary  // Từ vựng
SkillType.grammar     // Ngữ pháp
SkillType.listening   // Nghe
SkillType.speaking    // Nói
SkillType.reading     // Đọc
SkillType.writing     // Viết
```

---

## 💡 TIPS & BEST PRACTICES

### ✅ DO:

1. **Luôn gọi `startSession()` khi bắt đầu học**
2. **Luôn gọi `endSession()` khi kết thúc**
3. **Lưu kết quả THỰC TẾ** (không làm tròn, không chỉnh sửa)
4. **Refresh streak mỗi ngày** (có thể dùng background job)

### ❌ DON'T:

1. **Không tự suy đoán XP** (dùng `XpRewards.calculateXp()`)
2. **Không fake accuracy** (luôn dùng `correct / total`)
3. **Không hard-code progress** (luôn tính từ database)
4. **Không skip session tracking** (quan trọng cho streak)

---

## 🔥 REFRESH STREAK HÀNG NGÀY

### Option 1: Manual Refresh (trong Dashboard)

```dart
ElevatedButton(
  onPressed: () async {
    await analyticsService.refreshStreak(currentUser.uid);
    setState(() {}); // Refresh UI
  },
  child: const Text('Refresh Streak'),
)
```

### Option 2: Auto Refresh (khi app start)

```dart
// Trong main.dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  
  // Auto refresh streak
  final prefs = await SharedPreferences.getInstance();
  final lastRefresh = prefs.getString('last_streak_refresh');
  final today = DateTime.now().toIso8601String().substring(0, 10);
  
  if (lastRefresh != today) {
    // Refresh streak cho current user
    await analyticsService.refreshStreak(currentUser.uid);
    await prefs.setString('last_streak_refresh', today);
  }
  
  runApp(MyApp());
}
```

---

## 📱 INTEGRATION CHECKLIST

- [ ] Dependencies đã cài
- [ ] Firebase đã setup
- [ ] Services đã khởi tạo
- [ ] `startSession()` trong lesson page
- [ ] `saveExerciseResult()` khi hoàn thành bài
- [ ] `endSession()` khi thoát
- [ ] Dashboard button đã thêm
- [ ] Test với example app
- [ ] Streak refresh đã setup

---

## 🆘 TROUBLESHOOTING

### Vấn đề: Dashboard hiển thị 0 XP

**Nguyên nhân:** Chưa có exercise results trong database

**Giải pháp:**
1. Kiểm tra `saveExerciseResult()` đã được gọi chưa
2. Check Firebase Console → `exercise_results` collection
3. Xem logs: `print(result.xpEarned)`

### Vấn đề: Streak luôn = 0

**Nguyên nhân:** Session duration < 10 phút hoặc chưa refresh

**Giải pháp:**
1. Học ít nhất 10 phút liên tục
2. Gọi `analyticsService.refreshStreak(userId)`
3. Check `learning_sessions` có `duration_minutes > 0`

### Vấn đề: Skill Progress = 0%

**Nguyên nhân:** Chưa có exercise results cho skill đó

**Giải pháp:**
1. Làm ít nhất 1 bài tập cho skill đó
2. Đảm bảo `skill` parameter đúng
3. Check `exercise_results` với filter `skill == 'vocabulary'`

---

## 📖 TÀI LIỆU CHI TIẾT

Đọc thêm: **[ANALYTICS_SYSTEM_DOCUMENTATION.md](./ANALYTICS_SYSTEM_DOCUMENTATION.md)**

---

## 🎓 EXAMPLE CODE

File: **[lib/examples/analytics_usage_example.dart](./lib/examples/analytics_usage_example.dart)**

Chứa 8+ scenarios chi tiết!

---

## ✨ KẾT QUẢ MONG ĐỢI

Sau khi integrate:

✅ Dashboard hiển thị dữ liệu THỰC TẾ  
✅ XP tăng khi làm bài  
✅ Streak tăng khi học hàng ngày  
✅ Progress tăng theo accuracy  
✅ Heatmap hiển thị lịch học  
✅ AI gợi ý kỹ năng yếu  

**Chúc bạn thành công! 🚀**

---

**Có vấn đề?** Check documentation hoặc example code!

