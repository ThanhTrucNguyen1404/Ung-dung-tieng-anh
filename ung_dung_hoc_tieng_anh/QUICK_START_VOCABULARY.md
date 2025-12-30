# ⚡ HƯỚNG DẪN NHANH - IMPORT TỪ VỰNG

## 🎉 Bạn đã có 6,757 từ vựng A1-C2!

### 📊 Tóm tắt
- **A1**: 1,374 từ
- **A2**: 895 từ
- **B1**: 842 từ
- **B2**: 367 từ
- **C1**: 1,255 từ
- **C2**: 2,024 từ
- **TỔNG**: **6,757 từ** ✨

---

## 🚀 IMPORT VÀO FIREBASE (3 BƯỚC)

### Bước 1: Mở Terminal

```bash
cd "c:\File Coding\ung_dung_hoc_tieng_anh"
```

### Bước 2: Chạy công cụ Import

**Chọn 1 trong 2 cách:**

#### Cách 1: Import với UI (⭐ Khuyến nghị)
```bash
flutter run -t lib/tools/import_vocabulary_ui.dart
```
- ✅ Giao diện đẹp
- ✅ Xem tiến trình real-time
- ✅ Nút Start/Stop

#### Cách 2: Import qua Console (Nhanh hơn)
```bash
flutter run -t lib/tools/import_vocabulary_to_firestore.dart
```
- ⚡ Nhanh
- 📝 Logs trong terminal

### Bước 3: Đợi hoàn thành
- ⏱️ Thời gian: ~5-10 phút
- 📊 Import: 6,757 từ
- ✅ Xong!

---

## 📚 TÀI LIỆU CHI TIẾT

- 📄 **VOCABULARY_SUMMARY.md** - Thống kê chi tiết
- 📘 **IMPORT_VOCABULARY_GUIDE.md** - Hướng dẫn đầy đủ

---

## 🔍 KIỂM TRA SAU KHI IMPORT

### 1. Vào Firebase Console
```
https://console.firebase.google.com/project/ung-dung-hoc-tieng-anh-348fd/firestore
```

### 2. Mở collection `vocabulary`
- Bạn sẽ thấy 6,757 documents
- Mỗi document là 1 từ vựng

### 3. Test trong app
```dart
// Lấy 10 từ A1
final words = await firestore
    .collection('vocabulary')
    .where('level', isEqualTo: 'A1')
    .limit(10)
    .get();
```

---

## ❓ GẶP VẤN ĐỀ?

### Lỗi Firebase not initialized
```bash
flutterfire configure
```

### Lỗi Permission denied
Cập nhật `firestore.rules`:
```javascript
match /vocabulary/{vocabId} {
  allow read: if true;
  allow write: if request.auth != null;
}
```

### Import bị dừng giữa chừng
- Chạy lại script
- Script tự động bỏ qua từ đã import

---

## 🎯 SỬ DỤNG TRONG APP

### Lấy từ theo level
```dart
VocabularySeedData.getWordsByLevel('A1');
```

### Lấy từ ngẫu nhiên
```dart
VocabularySeedData.getDailyWords(20);
```

### Tìm kiếm
```dart
VocabularySeedData.searchWords('hello');
```

---

## ✨ XONG RỒI!

Bây giờ bạn có thể:
- ✅ Xây dựng tính năng học từ vựng
- ✅ Tạo flashcards
- ✅ Làm bài test/quiz
- ✅ Tích hợp vào ứng dụng

**Chúc bạn thành công! 🎉**

