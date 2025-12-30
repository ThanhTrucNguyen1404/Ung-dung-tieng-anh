# 📥 Sau khi Download google-services.json

## 🎯 Các bước thực hiện:

### 1️⃣ Copy file vào project

**File đã download:** `google-services.json` (trong thư mục Downloads)

**Copy đến:**
```
C:\File Coding\ung_dung_hoc_tieng_anh\android\app\google-services.json
```

**Cách làm:**
1. Mở thư mục Downloads
2. Tìm file `google-services.json` (mới nhất)
3. Copy (Ctrl+C)
4. Mở thư mục: `C:\File Coding\ung_dung_hoc_tieng_anh\android\app\`
5. Paste và **thay thế file cũ** (Ctrl+V)

---

### 2️⃣ Verify file đúng

**Mở file để kiểm tra:**
```powershell
notepad "C:\File Coding\ung_dung_hoc_tieng_anh\android\app\google-services.json"
```

**Check nội dung:**
```json
{
  "project_info": {
    "project_number": "...",
    "project_id": "ung-dung-hoc-tieng-anh-a0880",  // ✅ Phải là a0880
    "storage_bucket": "..."
  },
  "client": [
    {
      "client_info": {
        "mobilesdk_app_id": "...",
        "android_client_info": {
          "package_name": "com.example.ung_dung_hoc_tieng_anh"  // ✅ Phải đúng
        }
      }
    }
  ]
}
```

---

### 3️⃣ Enable Authentication

**Quay lại Firebase Console:**

1. **Click X** để đóng dialog "Add Firebase to Android app"
2. **Click menu bên trái:** `Authentication`
3. **Click:** `Get started`
4. **Tab:** `Sign-in method`
5. **Click:** `Email/Password`
6. **Toggle Enable:** ON ✅
7. **Click:** `Save`

**Kết quả:**
```
Email/Password: Enabled ✅
```

---

### 4️⃣ Setup Firestore Rules

**Vào Firestore:**

1. **Click menu:** `Firestore Database`
2. **Tab:** `Rules`
3. **Paste rules này:**

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
    
    // Public read collections
    match /lessons/{lessonId} {
      allow read: if true;
    }
    
    match /vocabulary/{vocabId} {
      allow read: if true;
    }
    
    // User progress
    match /userProgress/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Test collection
    match /test/{testId} {
      allow read, write: if true;
    }
  }
}
```

4. **Click:** `Publish`

---

### 5️⃣ Clean & Rebuild Flutter App

**Mở PowerShell và chạy:**

```powershell
cd "C:\File Coding\ung_dung_hoc_tieng_anh"

# Clean project
flutter clean

# Get dependencies
flutter pub get

# Run app
flutter run
```

**Đợi app build và chạy...**

---

### 6️⃣ Test Đăng ký

**Trong app:**

1. Click `Đăng ký`
2. Nhập:
   - Họ tên: `Test User`
   - Email: `test@example.com`
   - Password: `test123456`
   - Xác nhận: `test123456`
3. Click `Đăng ký`

**✅ Kết quả mong đợi:**
```
- Loading spinner hiển thị
- Không có error
- Chuyển sang màn hình tiếp theo
```

---

### 7️⃣ Verify trên Firebase Console

**Check Authentication:**
1. Vào `Authentication` → `Users`
2. Xem user mới: `test@example.com` ✅

**Check Firestore:**
1. Vào `Firestore Database` → `Data`
2. Collection `users`
3. Xem document với data user ✅

---

## ✅ CHECKLIST:

- [ ] Download `google-services.json` từ Firebase
- [ ] Copy vào `android/app/`
- [ ] Verify file có `project_id: "ung-dung-hoc-tieng-anh-a0880"`
- [ ] Enable Authentication (Email/Password)
- [ ] Setup Firestore Rules
- [ ] Run `flutter clean`
- [ ] Run `flutter pub get`
- [ ] Run `flutter run`
- [ ] Test đăng ký thành công
- [ ] Verify user trong Firebase Console

---

## 🎉 HOÀN THÀNH!

Sau khi làm xong 7 bước trên, app sẽ:
- ✅ Đăng ký được
- ✅ Đăng nhập được
- ✅ Lưu data vào Firestore
- ✅ Không còn lỗi CONFIGURATION_NOT_FOUND

---

## 🐛 Nếu vẫn lỗi:

### Error: "CONFIGURATION_NOT_FOUND"
**Fix:**
- Verify `google-services.json` đã copy đúng
- Run `flutter clean && flutter pub get && flutter run`

### Error: "EMAIL_EXISTS"
**Fix:**
- Thử email khác: `test2@example.com`

### Error: "PERMISSION_DENIED"
**Fix:**
- Check Firestore Rules đã publish

---

Chúc bạn thành công! 🚀




