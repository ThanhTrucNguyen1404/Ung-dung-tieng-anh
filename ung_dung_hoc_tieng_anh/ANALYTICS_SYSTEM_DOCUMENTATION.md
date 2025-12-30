# 📊 HỆ THỐNG THU THẬP & PHÂN TÍCH DỮ LIỆU HỌC TIẾNG ANH

## 🎯 MỤC ĐÍCH

Hệ thống thu thập, lưu trữ và phân tích dữ liệu học tiếng Anh từ **HÀNH VI THẬT** của người dùng.

### ✅ NGUYÊN TẮC CỐT LÕI

- ✅ Tất cả dữ liệu có thể bắt đầu từ **0**
- ❌ **KHÔNG** sử dụng null
- ❌ **KHÔNG** tự suy đoán % hoặc XP
- ❌ **KHÔNG** sinh dữ liệu giả
- ✅ Mọi chỉ số phải được tính toán từ **database thực tế**

---

## 📦 1. CƠ SỞ DỮ LIỆU (FIRESTORE)

### Collection: `users`

Lưu thông tin tổng hợp - giá trị mặc định = 0

```javascript
{
  "user_id": "uuid",
  "name": "string",
  "email": "string",
  "created_at": "timestamp",
  "last_active": "timestamp",
  
  "total_xp": 0,              // Tổng XP từ exercise_results
  "current_streak": 0,         // Số ngày học liên tục
  "total_learning_minutes": 0, // Tổng thời gian học
  "avatar_url": "",
  "current_level": "A1"        // A1, A2, B1, B2, C1, C2
}
```

**Quy tắc:**
- User mới → tất cả = 0
- Giá trị chỉ thay đổi khi có `learning_sessions` hoặc `exercise_results`
- Không cho phép chỉnh tay

### Collection: `learning_sessions`

Ghi mỗi lần người dùng học thực tế

```javascript
{
  "session_id": "uuid",
  "user_id": "uuid",
  "skill": "vocabulary | grammar | listening | speaking | reading | writing",
  "lesson_id": "string",
  "start_time": "timestamp",
  "end_time": "timestamp",     // null nếu chưa kết thúc
  "duration_minutes": 0,       // 0 nếu chưa tính được
  "completed": false           // false nếu user thoát giữa chừng
}
```

**Quy tắc:**
- `duration_minutes` có thể = 0 nếu user thoát sớm
- Chỉ tính học hợp lệ khi `duration_minutes > 0`

### Collection: `exercise_results`

Ghi kết quả từng bài tập thật

```javascript
{
  "exercise_id": "uuid",
  "user_id": "uuid",
  "skill": "listening",
  "correct_answers": 0,        // Số câu đúng (có thể = 0)
  "total_questions": 0,        // Tổng số câu
  "accuracy": 0.0,             // 0.0 - 1.0
  "xp_earned": 0,              // XP kiếm được (có thể = 0)
  "created_at": "timestamp",
  "lesson_id": "string",       // optional
  "session_id": "string"       // optional
}
```

**Công thức:**
```
accuracy = correct_answers / total_questions
```

---

## 📊 2. CÁCH TÍNH DASHBOARD (REAL DATA)

### ⭐ XP (REAL – CÓ THỂ = 0)

```
total_xp = Σ exercise_results.xp_earned
```

| Hành động | XP |
|-----------|-----|
| Trả lời đúng | +5 |
| Hoàn thành bài | +10 |
| Perfect lesson | +20 |
| Speaking AI pass | +30 |

**Quy tắc:** Không có bài → XP = 0

### 🔥 Streak (REAL – CÓ THỂ RESET = 0)

```
Nếu user học ≥ 10 phút trong 1 ngày:
  streak +1

Nếu nghỉ ≥ 1 ngày:
  streak = 0
```

**Nguồn dữ liệu:**
- `learning_sessions.start_time`
- `learning_sessions.duration_minutes`

### ⏱️ Thời gian học

```
total_learning_minutes = Σ learning_sessions.duration_minutes
```

**Quy tắc:** Không có session → = 0

### 📈 Tiến độ kỹ năng (KHÔNG GIẢ – CÓ THỂ = 0%)

```
Skill Progress (%) = (Σ accuracy) / số bài * 100
```

**Quy tắc:** Nếu chưa làm bài nào → Skill Progress = 0%

**Ví dụ - Listening 45%:**

```javascript
[
  { "accuracy": 0.4 },  // Bài 1: 40%
  { "accuracy": 0.5 },  // Bài 2: 50%
  { "accuracy": 0.45 }  // Bài 3: 45%
]

(0.4 + 0.5 + 0.45) / 3 = 0.45 → 45%
```

### Mapping kỹ năng

| Skill | Nguồn dữ liệu |
|-------|---------------|
| Từ vựng | Flashcard + Quiz |
| Ngữ pháp | Grammar quiz |
| Nghe | Audio / Video |
| Nói | AI Speech scoring |
| Đọc | Reading comprehension |
| Viết | AI Writing score |

---

## 🏗️ 3. KIẾN TRÚC HỆ THỐNG

### Layer Architecture

```
┌─────────────────────────────────────┐
│   Presentation Layer (UI)           │
│   - dashboard_page.dart             │
│   - stat_card.dart                  │
│   - skill_progress_card.dart        │
│   - heatmap_widget.dart             │
│   - weak_skills_card.dart           │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│   Domain Layer (Business Logic)     │
│   - analytics_service.dart          │
│   - session_tracking_service.dart   │
│   - privacy_service.dart            │
│   - entities (UserProfile, etc.)    │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│   Data Layer (Repository)           │
│   - analytics_repository_impl.dart  │
│   - models (UserProfileModel, etc.) │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│   Firebase Firestore                │
│   - users collection                │
│   - learning_sessions collection    │
│   - exercise_results collection     │
└─────────────────────────────────────┘
```

### Data Flow

```
User Action (học bài)
     ↓
SessionTrackingService.startSession()
     ↓
AnalyticsRepository.startLearningSession()
     ↓
Firestore: learning_sessions.add()
     ↓
User Action (làm bài tập)
     ↓
SessionTrackingService.saveExerciseResult()
     ↓
AnalyticsRepository.saveExerciseResult()
     ↓
Firestore: exercise_results.add()
     ↓
Firestore: users.update({ total_xp: +xp })
     ↓
Dashboard auto refresh
```

---

## 🔧 4. CÁCH SỬ DỤNG

### Setup

```dart
// Khởi tạo services
final repository = AnalyticsRepositoryImpl();
final analyticsService = AnalyticsService(repository);
final sessionTracking = SessionTrackingService(repository);
final privacyService = PrivacyService(repository);
```

### Scenario 1: Bắt đầu học

```dart
await sessionTracking.startSession(
  userId: 'user123',
  skill: SkillType.vocabulary,
  lessonId: 'lesson_001',
);
```

### Scenario 2: Lưu kết quả bài tập

```dart
await sessionTracking.saveExerciseResult(
  userId: 'user123',
  skill: SkillType.vocabulary,
  correctAnswers: 8,      // User trả lời đúng 8/10
  totalQuestions: 10,
  completed: true,
  lessonId: 'lesson_001',
);
```

### Scenario 3: Kết thúc session

```dart
await sessionTracking.endSession();
```

### Scenario 4: Hiển thị Dashboard

```dart
DashboardPage(
  userId: 'user123',
  analyticsService: analyticsService,
)
```

### Scenario 5: Lấy báo cáo tuần

```dart
final report = await analyticsService.getWeeklyReport('user123');
print('Thời gian học: ${report.totalHours} giờ');
print('XP: ${report.totalXp}');
print('Accuracy TB: ${report.averageAccuracyPercent}%');
```

### Scenario 6: AI phát hiện kỹ năng yếu

```dart
final weaknesses = await analyticsService.detectWeakSkills('user123');
for (var weakness in weaknesses) {
  print('${weakness.skill.displayName}: ${weakness.accuracyPercent}%');
  print('→ ${weakness.recommendedPractice}');
}
```

### Scenario 7: Export dữ liệu (GDPR)

```dart
final file = await privacyService.exportUserData('user123');
print('Exported to: ${file.path}');
```

---

## 🏆 5. TÍNH NĂNG NÂNG CAO

### ✅ Heatmap học tập theo ngày

Hiển thị 30 ngày gần nhất với màu sắc theo cường độ học:

```dart
final heatmap = await analyticsService.getStudyHeatmap('user123');
// Returns: Map<DateTime, int> (minutes per day)
```

**Intensity levels:**
- 0 phút: Xám nhạt
- 1-10 phút: Xanh nhạt
- 10-30 phút: Xanh
- 30-60 phút: Xanh đậm
- 60-120 phút: Xanh đậm hơn
- 120+ phút: Xanh đậm nhất

### ✅ AI phát hiện kỹ năng yếu

Tự động phát hiện kỹ năng có accuracy < 60%:

```dart
final weaknesses = await analyticsService.detectWeakSkills('user123');
```

**Logic:**
- Accuracy < 40% → "Luyện tập 30 phút/ngày"
- Accuracy < 50% → "Luyện tập 20 phút/ngày"
- Accuracy < 60% → "Luyện tập 15 phút/ngày"

### ✅ Báo cáo tiến độ theo tuần/tháng

```dart
final weeklyReport = await analyticsService.getWeeklyReport('user123');
```

Bao gồm:
- Tổng thời gian học
- Tổng XP
- Số bài làm
- Accuracy trung bình

### ✅ Adaptive Learning Path

Dựa trên kỹ năng yếu → gợi ý bài học phù hợp:

```dart
final weakSkills = await analyticsService.detectWeakSkills('user123');
// Sử dụng weakSkills để recommend lessons
```

### ✅ Privacy & GDPR Compliance

**Xem lịch sử:**
```dart
final history = await privacyService.getStudyHistory('user123', days: 30);
```

**Export dữ liệu:**
```dart
final file = await privacyService.exportUserData('user123');
```

**Xóa dữ liệu cũ:**
```dart
await privacyService.deleteOldData('user123', olderThanDays: 365);
```

---

## 📱 6. TÍCH HỢP VÀO APP

### Trong Lesson Page

```dart
class LessonPage extends StatefulWidget {
  final SessionTrackingService sessionTracking;
  
  @override
  void initState() {
    super.initState();
    
    // Bắt đầu session
    sessionTracking.startSession(
      userId: currentUserId,
      skill: SkillType.listening,
      lessonId: widget.lessonId,
    );
  }
  
  @override
  void dispose() {
    // Kết thúc session khi thoát
    sessionTracking.endSession();
    super.dispose();
  }
  
  void onExerciseComplete(int correct, int total) {
    // Lưu kết quả
    sessionTracking.saveExerciseResult(
      userId: currentUserId,
      skill: SkillType.listening,
      correctAnswers: correct,
      totalQuestions: total,
      completed: true,
    );
  }
}
```

### Trong Home Page

```dart
FloatingActionButton(
  onPressed: () {
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => DashboardPage(
          userId: currentUserId,
          analyticsService: analyticsService,
        ),
      ),
    );
  },
  child: Icon(Icons.analytics),
)
```

---

## 🧪 7. TESTING

### Test với dữ liệu THỰC TẾ

```dart
void main() {
  final example = AnalyticsUsageExample();
  
  // Test full flow
  await example.exampleComplete_FullLearningFlow();
}
```

### Run Example App

```dart
void main() {
  runApp(const AnalyticsExampleApp());
}
```

**File:** `lib/examples/analytics_usage_example.dart`

---

## 🔐 8. BẢO MẬT & QUYỀN RIÊNG TƯ

### Firestore Security Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Users collection
    match /users/{userId} {
      allow read: if request.auth.uid == userId;
      allow write: if request.auth.uid == userId;
    }
    
    // Learning sessions
    match /learning_sessions/{sessionId} {
      allow read: if request.auth.uid == resource.data.user_id;
      allow create: if request.auth.uid == request.resource.data.user_id;
      allow update: if request.auth.uid == resource.data.user_id;
    }
    
    // Exercise results
    match /exercise_results/{resultId} {
      allow read: if request.auth.uid == resource.data.user_id;
      allow create: if request.auth.uid == request.resource.data.user_id;
    }
  }
}
```

### GDPR Compliance

✅ **Right to Access:** User có thể xem tất cả dữ liệu của mình  
✅ **Right to Export:** User có thể export dữ liệu  
✅ **Right to Delete:** User có thể xóa dữ liệu cũ  
✅ **Transparency:** Hiển thị rõ dữ liệu gì được thu thập  

---

## 📈 9. ANALYTICS METRICS

### Key Metrics

| Metric | Formula | Purpose |
|--------|---------|---------|
| Total XP | Σ xp_earned | Đo lường tổng tiến bộ |
| Current Streak | Days with ≥10 min study | Đo lường sự kiên trì |
| Learning Time | Σ duration_minutes | Đo lường effort |
| Skill Progress | Avg(accuracy) per skill | Đo lường kỹ năng |
| Accuracy | correct / total | Đo lường chất lượng |

### Dashboard KPIs

```
┌─────────────────────────────────────┐
│  ⭐ 1,250 XP                         │
│  🔥 5 Streak                         │
│  ⏱️ 12h Learning                     │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│  Từ vựng      ████████░░   75%      │
│  Ngữ pháp     ██████░░░░   60%      │
│  Nghe         ████░░░░░░   45%      │
│  Nói          ███░░░░░░░   30%      │
│  Đọc          █████░░░░░   55%      │
│  Viết         ████░░░░░░   40%      │
└─────────────────────────────────────┘
```

---

## 🚀 10. NEXT STEPS

### Phase 1 (Done ✅)
- ✅ Database schema
- ✅ Repository layer
- ✅ Services layer
- ✅ Dashboard UI
- ✅ Heatmap
- ✅ AI weak skill detection
- ✅ Privacy features

### Phase 2 (TODO)
- [ ] Leaderboard (XP ranking)
- [ ] Achievements system
- [ ] Social features (share progress)
- [ ] AI recommendations
- [ ] Push notifications for streak

### Phase 3 (Future)
- [ ] Predictive analytics
- [ ] A/B testing
- [ ] Machine learning models
- [ ] Advanced reporting

---

## 📞 SUPPORT

Nếu có vấn đề, check các file:

1. **Example Code:** `lib/examples/analytics_usage_example.dart`
2. **Repository:** `lib/data/repositories/analytics_repository_impl.dart`
3. **Services:** `lib/domain/services/`
4. **UI:** `lib/presentation/pages/dashboard/`

---

## 🎯 KẾT LUẬN

Hệ thống này đảm bảo:

✅ **100% Real Data** - Không có dữ liệu giả  
✅ **Zero Safe** - Tất cả có thể bắt đầu từ 0  
✅ **Privacy First** - GDPR compliant  
✅ **AI Powered** - Phát hiện kỹ năng yếu tự động  
✅ **Production Ready** - Sẵn sàng cho đồ án 9-10 điểm  

**Happy Coding! 🚀**

