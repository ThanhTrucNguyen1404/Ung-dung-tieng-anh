# 🔥 Hướng dẫn Setup Firebase THỦ CÔNG

## ⚠️ Tình huống:
- Project ID trong code: `ung-dung-hoc-tieng-anh-348fd` ❌
- Project ID trên Firebase: `ung-dung-hoc-tieng-anh-a0880` ✅
- FlutterFire CLI không tìm thấy project

## ✅ GIẢI PHÁP: Setup thủ công (5 phút)

---

## 📋 BƯỚC 1: Vào Project trên Firebase Console

1. **Mở:** https://console.firebase.google.com
2. **Click vào project:** **"ung-dung-hoc-tieng-anh"** 
   - (ID: ung-dung-hoc-tieng-anh-a0880)

---

## 🔐 BƯỚC 2: Enable Authentication

1. **Click menu bên trái:** `Authentication`
2. **Click nút:** `Get started` (nếu lần đầu)
3. **Tab:** `Sign-in method`
4. **Click:** `Email/Password`
5. **Toggle Enable:** ON ✅
6. **Click:** `Save`

### ✅ Kết quả:
```
Email/Password: Enabled ✅
```

---

## 💾 BƯỚC 3: Create Firestore Database

1. **Click menu:** `Firestore Database`
2. **Click:** `Create database`
3. **Chọn mode:**
   - ✅ `Start in test mode` (cho development)
4. **Cloud Firestore location:**
   - Chọn: `asia-southeast1 (Singapore)`
5. **Click:** `Enable`

### ⏳ Đợi 1-2 phút để database được tạo...

### ✅ Kết quả:
```
Database created ✅
Mode: Test mode
Location: asia-southeast1
```

---

## 📝 BƯỚC 4: Setup Firestore Rules

1. **Tab:** `Rules`
2. **Xóa tất cả** và paste rules này:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users - own data only
    match /users/{userId} {
      allow read: if request.auth != null && request.auth.uid == userId;
      allow create: if request.auth != null && request.auth.uid == userId;
      allow update: if request.auth != null && request.auth.uid == userId;
    }
    
    // Lessons - public read
    match /lessons/{lessonId} {
      allow read: if true;
    }
    
    // Vocabulary - public read
    match /vocabulary/{vocabId} {
      allow read: if true;
    }
    
    // User progress - own data only
    match /userProgress/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Test collection (development only)
    match /test/{testId} {
      allow read, write: if true;
    }
  }
}
```

3. **Click:** `Publish`

### ✅ Kết quả:
```
Rules published ✅
```

---

## 📱 BƯỚC 5: Download google-services.json

### Kiểm tra xem Android app đã được add chưa:

1. **Click biểu tượng bánh răng ⚙️** (góc trái trên)
2. **Click:** `Project settings`
3. **Scroll xuống** phần **"Your apps"**

### Nếu CHƯA CÓ Android app:

4. **Click:** `Add app`
5. **Chọn:** Android icon (🤖)
6. **Android package name:** `com.example.ung_dung_hoc_tieng_anh`
7. **App nickname (optional):** `Ung Dung Hoc Tieng Anh`
8. **Click:** `Register app`
9. **Click:** `Download google-services.json`
10. **Click:** `Next` → `Next` → `Continue to console`

### Nếu ĐÃ CÓ Android app:

4. **Tìm app:** `com.example.ung_dung_hoc_tieng_anh`
5. **Click icon download** (hoặc `google-services.json`)

---

## 📂 BƯỚC 6: Thay thế file google-services.json

1. **Lưu file đã download** từ Firebase Console

2. **Mở thư mục project:**
   ```
   C:\File Coding\ung_dung_hoc_tieng_anh\android\app
   ```

3. **Xóa file cũ:** `google-services.json` ❌

4. **Copy file mới vào:** `google-services.json` ✅

5. **Verify:** Mở file và check:
   ```json
   {
     "project_info": {
       "project_id": "ung-dung-hoc-tieng-anh-a0880"  // ✅ Phải đúng ID này!
     }
   }
   ```

---

## 🔄 BƯỚC 7: Update .firebaserc

**Mở file:** `C:\File Coding\ung_dung_hoc_tieng_anh\.firebaserc`

**Thay đổi:**
```json
{
  "projects": {
    "default": "ung-dung-hoc-tieng-anh-a0880"
  }
}
```

**Lưu file** ✅

---

## 🔧 BƯỚC 8: Update firebase_options.dart

**Option 1: Tự động (nếu FlutterFire CLI hoạt động)**

Chạy trong PowerShell:
```powershell
cd "C:\File Coding\ung_dung_hoc_tieng_anh"
firebase login
flutterfire configure
```

**Option 2: Thủ công**

Nếu FlutterFire không hoạt động, bạn có thể giữ nguyên file `firebase_options.dart` hiện tại, vì:
- File `google-services.json` mới sẽ được Gradle sử dụng cho Android
- iOS sẽ dùng `GoogleService-Info.plist` (nếu có)

---

## 🧹 BƯỚC 9: Clean & Rebuild

Chạy trong PowerShell:

```powershell
cd "C:\File Coding\ung_dung_hoc_tieng_anh"

# Clean project
flutter clean

# Get dependencies
flutter pub get

# Run app
flutter run
```

---

## 🧪 BƯỚC 10: Test Đăng ký

1. **Mở app trên emulator**
2. **Click:** `Đăng ký`
3. **Nhập:**
   - Họ tên: `Test User`
   - Email: `test@example.com`
   - Password: `test123456`
   - Xác nhận: `test123456`
4. **Click:** `Đăng ký`

### ✅ Kết quả mong đợi:
```
✅ Loading spinner hiển thị
✅ Không có error
✅ Chuyển sang Placement Test page
```

---

## 🔍 BƯỚC 11: Verify trên Firebase Console

### Check Authentication:

1. **Vào:** `Authentication` → `Users`
2. **Xem:** User mới được tạo ✅
   ```
   test@example.com
   Created: just now
   ```

### Check Firestore:

1. **Vào:** `Firestore Database` → `Data`
2. **Collection:** `users`
3. **Document ID:** (uid của user)
4. **Xem data:**
   ```json
   {
     "uid": "...",
     "email": "test@example.com",
     "displayName": "Test User",
     "currentLevel": 1,
     "totalXP": 0,
     "currentStreak": 0,
     "createdAt": "2025-12-19..."
   }
   ```

---

## ✅ CHECKLIST HOÀN THÀNH:

### Firebase Console:
- [ ] Project: `ung-dung-hoc-tieng-anh-a0880` được chọn ✅
- [ ] Authentication: Email/Password **Enabled** ✅
- [ ] Firestore: Database **Created** ✅
- [ ] Firestore Rules: **Published** ✅
- [ ] Android app: **Registered** ✅
- [ ] `google-services.json`: **Downloaded** ✅

### Local Project:
- [ ] File `android/app/google-services.json`: **Updated** ✅
- [ ] File `.firebaserc`: **Updated** ✅
- [ ] Run: `flutter clean` ✅
- [ ] Run: `flutter pub get` ✅
- [ ] Run: `flutter run` ✅

### Testing:
- [ ] Đăng ký: **Thành công** ✅
- [ ] User trong Authentication: **Có** ✅
- [ ] User data trong Firestore: **Có** ✅

---

## 🐛 Troubleshooting:

### Lỗi 1: "CONFIGURATION_NOT_FOUND"
**Fix:** 
- Verify `google-services.json` đã được thay đúng
- Run `flutter clean && flutter pub get && flutter run`

### Lỗi 2: "EMAIL_EXISTS"
**Fix:** 
- Email đã được dùng
- Thử email khác: `test2@example.com`

### Lỗi 3: "WEAK_PASSWORD"
**Fix:** 
- Password phải >= 6 ký tự

### Lỗi 4: "PERMISSION_DENIED" (Firestore)
**Fix:** 
- Check Firestore Rules đã publish chưa
- Verify rules cho phép user write vào `/users/{userId}`

---

## 🎯 TÓM TẮT:

1. ✅ Vào Firebase Console
2. ✅ Enable Authentication (Email/Password)
3. ✅ Create Firestore Database
4. ✅ Publish Firestore Rules
5. ✅ Download `google-services.json`
6. ✅ Thay file trong `android/app/`
7. ✅ Update `.firebaserc`
8. ✅ `flutter clean && flutter run`
9. ✅ Test đăng ký

**Tổng thời gian: 5-10 phút** ⏱️

---

## 📞 Nếu vẫn lỗi:

1. **Share screenshot** của Firebase Console:
   - Authentication page
   - Firestore Database page

2. **Share log** từ Flutter:
   ```powershell
   flutter run --verbose
   ```

3. **Check file:**
   ```powershell
   type android\app\google-services.json | findstr project_id
   ```

---

Chúc bạn setup thành công! 🎉

