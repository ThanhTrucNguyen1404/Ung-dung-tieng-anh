# 🔥 Fix Firebase Registration Error

## ❌ Lỗi:
```
Exception: An internal error has occurred. 
[CONFIGURATION_NOT_FOUND]
```

## 🎯 Nguyên nhân:

Lỗi này **KHÔNG PHẢI** do database, mà do:
1. ❌ Firebase Authentication chưa được enable
2. ❌ google-services.json chưa được sync
3. ❌ Cần rebuild app sau khi config Firebase

## ✅ CÁCH FIX (3 bước):

### BƯỚC 1: Enable Firebase Authentication

1. **Vào Firebase Console:**
   - Mở: https://console.firebase.google.com
   - Chọn project: `ung-dung-hoc-tieng-anh-348fd`

2. **Enable Authentication:**
   - Click menu bên trái: **Authentication**
   - Click **Get started**
   - Vào tab **Sign-in method**
   - Click **Email/Password**
   - Toggle **Enable** → ON ✅
   - Click **Save**

3. **Verify:**
   - Bạn sẽ thấy "Email/Password" status = **Enabled** ✅

### BƯỚC 2: Enable Firestore (nếu chưa)

1. **Vào Firestore Database:**
   - Click menu: **Firestore Database**
   - Click **Create database**

2. **Chọn mode:**
   - Chọn: **Start in test mode** (cho development)
   - Location: **asia-southeast1** (Singapore)
   - Click **Enable**

3. **Setup Rules:**
   - Vào tab **Rules**
   - Paste rules từ file `firestore.rules` (đã tạo trước đó)
   - Click **Publish**

### BƯỚC 3: Rebuild App

```powershell
cd "C:\File Coding\ung_dung_hoc_tieng_anh"

# Clean build
flutter clean

# Get dependencies
flutter pub get

# Rebuild app (QUAN TRỌNG!)
flutter run
```

## 🔍 Kiểm tra Firebase Setup:

### Check 1: google-services.json
```powershell
# File phải tồn tại tại:
dir android\app\google-services.json
```

**Nội dung phải có:**
- `project_id`: "ung-dung-hoc-tieng-anh-348fd"
- `client` → `oauth_client` → có API keys
- Không được để trống hoặc template

### Check 2: Firebase Console

**Authentication:**
- ✅ Email/Password: **Enabled**
- ❌ Không được "Not enabled"

**Firestore:**
- ✅ Database: **Created**
- ✅ Rules: **Published**

## 🧪 Test Registration:

Sau khi fix, test lại:

1. **Mở app**
2. **Click "Đăng ký"**
3. **Nhập thông tin:**
   - Họ tên: Test User
   - Email: test@example.com
   - Password: test123456
   - Xác nhận password: test123456
4. **Click "Đăng ký"**

### Kết quả mong đợi:

✅ **Thành công:**
```
- Hiển thị loading spinner
- Chuyển sang Placement Test page
- Không có error
```

❌ **Nếu vẫn lỗi:**
```
Xem log để biết lỗi cụ thể:
flutter run --verbose
```

## 🐛 Debug Steps:

### 1. Check Flutter console log:
```
Exception: [firebase_auth/...]
```

### 2. Common Errors & Fixes:

#### Error: "EMAIL_NOT_FOUND"
**Fix:** Enable Email/Password trong Authentication

#### Error: "WEAK_PASSWORD"  
**Fix:** Password phải >= 6 ký tự

#### Error: "EMAIL_EXISTS"
**Fix:** Email đã được dùng, thử email khác

#### Error: "CONFIGURATION_NOT_FOUND"
**Fix:** 
1. Check google-services.json tồn tại
2. Rebuild app: `flutter clean && flutter run`

#### Error: "PERMISSION_DENIED" (Firestore)
**Fix:**
1. Vào Firestore Rules
2. Paste nội dung từ `firestore.rules`
3. Publish

## 📝 Firestore Rules (nếu cần):

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
    
    // Test collection (development)
    match /test/{testId} {
      allow read, write: if true;
    }
  }
}
```

## ✅ Checklist:

### Firebase Console:
- [ ] Authentication → Email/Password: **Enabled** ✅
- [ ] Firestore Database: **Created** ✅
- [ ] Firestore Rules: **Published** ✅

### Local:
- [ ] google-services.json: **Tồn tại** ✅
- [ ] Chạy: `flutter clean`
- [ ] Chạy: `flutter pub get`
- [ ] Chạy: `flutter run`

### Test:
- [ ] Đăng ký mới: **Thành công** ✅
- [ ] Đăng nhập: **Thành công** ✅
- [ ] Chuyển sang home page: **OK** ✅

## 🎯 Kết quả sau khi fix:

✅ Đăng ký thành công  
✅ Tạo user trong Firebase Authentication  
✅ Lưu user data vào Firestore  
✅ Chuyển sang Placement Test  
✅ Login sau đó cũng hoạt động  

## 📱 User Flow sau khi fix:

```
Register
  ↓
Firebase Auth: Tạo account ✅
  ↓
Firestore: Lưu user data ✅
  ↓
Navigate to Placement Test ✅
  ↓
Complete test → Home ✅
```

## 💡 Tips:

### 1. Test với nhiều accounts:
```
test1@example.com
test2@example.com
test3@example.com
```

### 2. Check Firebase Console để verify:
```
Authentication → Users → Xem danh sách users mới
Firestore → users collection → Xem documents
```

### 3. Reset nếu muốn test lại:
```
Authentication → Users → Xóa users
Firestore → users → Xóa documents
```

## 🔒 Production Checklist:

Trước khi release:

- [ ] Change Firestore Rules từ test mode → production
- [ ] Enable App Check
- [ ] Setup email verification
- [ ] Add password reset flow
- [ ] Setup security monitoring

---

## 🚀 QUICK FIX:

```
1. Firebase Console → Authentication → Enable Email/Password ✅
2. Firebase Console → Firestore → Create database ✅
3. flutter clean && flutter pub get && flutter run ✅
```

**Tổng thời gian: 5 phút** ⏱️

Sau khi làm 3 bước trên, đăng ký sẽ hoạt động 100%! 🎉

