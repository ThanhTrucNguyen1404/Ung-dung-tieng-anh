# 📚 HƯỚNG DẪN SETUP HỆ THỐNG TỪ VỰNG

## 🚀 Cách Upload 1000+ Từ Vựng Lên Firebase

### **BƯỚC 1: Chạy Script Upload**

```powershell
# Trong PowerShell
cd "C:\File Coding\ung_dung_hoc_tieng_anh"

# Chạy script upload
flutter run lib/scripts/upload_vocabulary_to_firebase.dart
```

### **BƯỚC 2: Nhấn "Start Upload" Trong App**

- App sẽ mở ra
- Nhấn nút **"Start Upload"**
- Đợi khoảng 2-3 phút để upload hoàn tất
- Sẽ có **1000+ từ vựng** được upload lên Firestore

### **BƯỚC 3: Kiểm Tra Trên Firebase Console**

1. Vào https://console.firebase.google.com
2. Chọn project **ung-dung-hoc-tieng-anh-a0580**
3. Vào **Firestore Database**
4. Kiểm tra collection **vocabulary** - phải có ~1000 documents

---

## ✅ SAU KHI UPLOAD XONG

### **Vocabulary Page Sẽ Tự Động:**

1. ✅ **Lấy từ Firebase** theo cấp độ (A1, A2, B1, B2)
2. ✅ **Cache offline** vào Hive
3. ✅ **Tracking tiến độ** khi người dùng học
4. ✅ **Spaced Repetition** - từ sẽ xuất hiện lại theo thuật toán
5. ✅ **XP Rewards** - mỗi từ học xong +10 XP

### **Chức Năng Đã Hoạt Động:**

- 📱 Flashcard với animation lật thẻ
- 🎚️ Chọn cấp độ từ menu (A1 → B2)
- 🔄 Tải lại danh sách từ mới
- ✅ Đánh dấu "Đã nhớ" / "Chưa nhớ"
- 📊 Tracking progress real-time
- 🏆 Hiển thị XP khi hoàn thành

---

## 🔍 TROUBLESHOOTING

### **Lỗi: "Chưa có từ vựng nào"**

➡️ **Nguyên nhân:** Chưa upload dữ liệu lên Firebase
➡️ **Giải pháp:** Chạy script upload (Bước 1)

### **Lỗi: "Permission denied"**

➡️ **Nguyên nhân:** Firestore rules chặn
➡️ **Giải pháp:** 
1. Vào Firebase Console → Firestore → Rules
2. Paste rules sau:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Vocabulary - public read, admin write
    match /vocabulary/{vocabId} {
      allow read: if true; // Everyone can read
      allow write: if request.auth != null; // Only authenticated users can write
    }
    
    // User vocabulary progress - private
    match /user_vocabulary_progress/{progressId} {
      allow read, write: if request.auth != null && request.auth.uid == resource.data.userId;
    }
  }
}
```

3. Click **"Publish"**

### **Lỗi kết nối Firebase**

➡️ Chạy lại:
```powershell
flutter clean
flutter pub get
flutter run
```

---

## 📈 NEXT FEATURES

- [ ] Audio pronunciation (just_audio)
- [ ] Image for each word
- [ ] Daily new words notification
- [ ] Spaced repetition advanced algorithm
- [ ] Vocabulary quiz mode
- [ ] Word categories (Animals, Food, Travel, etc.)

---

## 🎯 KẾT QUẢ MONG ĐỢI

Sau khi setup xong, **Vocabulary Page** sẽ:

1. ✅ Hiển thị từ vựng THẬT từ Firebase (không phải hardcode)
2. ✅ Có thể chọn cấp độ A1-B2
3. ✅ Tracking tiến độ học của user
4. ✅ Hoạt động offline với Hive cache
5. ✅ Tính XP và cập nhật lên database

