# 🚀 DASHBOARD - SETUP NHANH 5 PHÚT

## 🎯 Mục đích

Tạo sample data và test Dashboard ngay lập tức!

---

## ✅ CÁCH 1: TỰ ĐỘNG (KHUYẾN NGHỊ) ⭐

### **1 lệnh duy nhất:**

```powershell
.\setup_and_test_dashboard.ps1
```

Script sẽ tự động:
- ✅ Tạo data trong Firestore (1 user + 7 sessions + 18 results)
- ✅ Kiểm tra indexes
- ✅ Launch app
- ✅ Hướng dẫn test Dashboard

**Tổng thời gian: 3-5 phút**

---

## ⚡ CÁCH 2: TỪNG BƯỚC

### **Bước 1: Tạo data**

```powershell
flutter run -t lib/tools/setup_dashboard_data.dart -d edge
```

Đợi thấy:
```
🎉 DASHBOARD DATA SETUP COMPLETE!
✅ 1 User Profile (ID: demo_user)
✅ 7 Learning Sessions
✅ 18 Exercise Results
```

### **Bước 2: Tạo Firestore Indexes**

**QUAN TRỌNG:** Dashboard cần 2 indexes để hoạt động!

1. Vào: https://console.firebase.google.com/project/ung-dung-hoc-tieng-anh-a0580/firestore/indexes

2. Tạo 2 indexes:

**Index 1:**
```
Collection: learning_sessions
Fields:
  - user_id: Ascending ⬆️
  - start_time: Descending ⬇️
```

**Index 2:**
```
Collection: exercise_results
Fields:
  - user_id: Ascending ⬆️
  - created_at: Descending ⬇️
```

3. Đợi status = **"Enabled"** (1-3 phút)

### **Bước 3: Test app**

```powershell
flutter run -t lib/main_ui_demo.dart -d edge
```

Trong app:
1. Click **Dashboard** tab (icon bar chart)
2. Đợi data load
3. Thấy stats:
   - ✅ Total XP: 250
   - ✅ Current Streak: 7 days
   - ✅ Learning Minutes: 180 mins
   - ✅ Skill Progress: 6 skills

---

## 🔍 VERIFY DATA

### **Firebase Console:**

https://console.firebase.google.com/project/ung-dung-hoc-tieng-anh-a0580/firestore/data

**Check 3 collections:**

```
✅ users
   └─ demo_user
      ├─ total_xp: 250
      ├─ current_streak: 7
      └─ total_learning_minutes: 180

✅ learning_sessions
   └─ 7 documents

✅ exercise_results
   └─ 18+ documents
```

---

## ❌ TROUBLESHOOTING

### **Lỗi: "requires an index"**

**Nguyên nhân:** Chưa tạo Firestore Indexes

**Giải pháp:**
1. Copy link trong error message
2. Paste vào browser → Click "Create Index"
3. Đợi "Enabled"
4. Quay lại app → Thử lại

**Chi tiết:** [`FIRESTORE_INDEXES_QUICKFIX.md`](./FIRESTORE_INDEXES_QUICKFIX.md)

---

### **Lỗi: "Permission denied"**

**Nguyên nhân:** Firestore rules chưa deploy

**Giải pháp:**
```powershell
firebase deploy --only firestore:rules
```

Hoặc update rules trong Firebase Console:
https://console.firebase.google.com/project/ung-dung-hoc-tieng-anh-a0580/firestore/rules

---

### **Dashboard hiển thị 0/null**

**Nguyên nhân:** Data chưa sync hoặc sai user ID

**Giải pháp:**
1. Check Firebase Console → Data có chưa?
2. Check user_id = `demo_user` (không phải `test_user...`)
3. Đợi 5-10 giây cho sync
4. Tap "Refresh" trong app

---

## 📚 DOCUMENTATION

**Quick Guides:**
- [`DASHBOARD_QUICK_SETUP.md`](./DASHBOARD_QUICK_SETUP.md) ← **Đang đọc**
- [`FIRESTORE_INDEXES_QUICKFIX.md`](./FIRESTORE_INDEXES_QUICKFIX.md) - Fix lỗi indexes

**Full Documentation:**
- [`FIRESTORE_SETUP_GUIDE.md`](./FIRESTORE_SETUP_GUIDE.md) - Setup đầy đủ
- [`README_ANALYTICS.md`](./README_ANALYTICS.md) - Analytics system
- [`ANALYTICS_QUICK_START.md`](./ANALYTICS_QUICK_START.md) - Getting started

---

## 🎉 SUCCESS

**Khi Dashboard hoạt động:**

```
┌─────────────────────────────────────────┐
│            DASHBOARD                    │
│  ┌─────────┬─────────────┬───────────┐  │
│  │   250   │   7 days    │ 180 mins  │  │
│  │ Total XP│   Streak    │  Minutes  │  │
│  └─────────┴─────────────┴───────────┘  │
│                                         │
│  Skill Progress:                        │
│  📚 Vocabulary    ████████░░ 80%        │
│  📖 Grammar       ███████░░░ 70%        │
│  🎧 Listening     ██████░░░░ 60%        │
│  🗣️ Speaking      ██████░░░░ 60%        │
│  📄 Reading       ██████░░░░ 60%        │
│  ✍️ Writing       ██████░░░░ 60%        │
└─────────────────────────────────────────┘
```

**✅ Dashboard sẵn sàng sử dụng!** 🎊

---

## 🚀 NEXT STEPS

1. **Tích hợp vào flow học:**
   - Mỗi lần user hoàn thành bài → Tạo `exercise_result`
   - Mỗi lần user học → Tạo `learning_session`
   - Dashboard tự động cập nhật real-time

2. **Customize Dashboard:**
   - Edit: `lib/presentation/pages/dashboard/dashboard_page.dart`
   - Add thêm widgets
   - Thay đổi colors/styles

3. **Add more features:**
   - Weekly report
   - Monthly statistics
   - AI recommendations
   - Leaderboard integration

---

**Có vấn đề? Hỏi ngay!** 💬

