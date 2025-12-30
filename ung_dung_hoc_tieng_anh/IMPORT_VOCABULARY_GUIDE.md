# 📚 Hướng Dẫn Import Từ Vựng vào Firebase

## 🎉 Tổng Quan

Bạn đã có **6,757 từ vựng đầy đủ từ cấp độ A1 đến C2** được chuyển đổi thành công từ file CSV sang Dart code. Bây giờ cần import vào Firebase Firestore để ứng dụng có thể sử dụng.

## 📁 Các File Liên Quan

```
lib/
├── data/
│   └── datasources/
│       └── local/
│           └── vocabulary_seed_data.dart    ← 2,024 từ vựng đã được chuyển đổi
├── tools/
│   ├── import_vocabulary_to_firestore.dart  ← Script import (console)
│   └── import_vocabulary_ui.dart            ← Script import (có UI)
└── ...

tools/
└── csv_to_dart_converter.dart               ← Script chuyển đổi CSV sang Dart

d:\Download\
└── Từ vựng - C2.csv                         ← File CSV gốc
```

## 🚀 Cách 1: Import với UI (Khuyến nghị)

### Bước 1: Chạy ứng dụng import

```bash
flutter run -t lib/tools/import_vocabulary_ui.dart
```

### Bước 2: Sử dụng UI

1. Ứng dụng sẽ mở với giao diện đơn giản
2. Click nút **"Bắt đầu Import"**
3. Theo dõi tiến trình import:
   - Thanh progress bar
   - Số lượng từ đã import
   - Logs chi tiết
4. Chờ đến khi hoàn thành (có thể mất 5-10 phút)

### Bước 3: Kiểm tra kết quả

- Xem thống kê: Tổng, Thành công, Bỏ qua, Lỗi
- Truy cập Firebase Console để kiểm tra dữ liệu

## 🖥️ Cách 2: Import qua Console

### Chạy script

```bash
flutter run -t lib/tools/import_vocabulary_to_firestore.dart
```

Script sẽ tự động:
1. Kết nối Firebase
2. Import từng từ vào collection `vocabulary`
3. Hiển thị tiến trình trong console
4. Báo cáo kết quả cuối cùng

## 🔍 Kiểm Tra Dữ Liệu Trên Firebase

### 1. Truy cập Firebase Console

```
https://console.firebase.google.com/project/ung-dung-hoc-tieng-anh-348fd/firestore
```

### 2. Vào collection `vocabulary`

Bạn sẽ thấy các document với cấu trúc:

```javascript
{
  word: "to abate",
  pronunciation: "/əˈbeɪt/",
  meaning: "dịu đi, giảm bớt",
  partOfSpeech: "verb",
  level: "C2",
  example: "The storm showed no signs of abating.",
  exampleTranslation: "Cơn bão không có dấu hiệu dịu đi.",
  imageUrl: "",
  audioUrl: "",
  synonyms: ["subside", "decrease"],
  antonyms: ["intensify"],
  createdAt: Timestamp
}
```

## 📊 Thống Kê Dữ Liệu

### Tổng quan
- **Tổng số từ**: 6,757 từ
- **Cấp độ**: A1 → C2 (Đầy đủ tất cả cấp độ)
- **Loại từ**: Noun, Verb, Adjective, Adverb, etc.

### Phân bổ theo cấp độ
- **A1**: 1,374 từ (Beginner)
- **A2**: 895 từ (Elementary)
- **B1**: 842 từ (Intermediate)
- **B2**: 367 từ (Upper-Intermediate)
- **C1**: 1,255 từ (Advanced)
- **C2**: 2,024 từ (Proficiency)

### Tính năng đặc biệt
- ✅ Phiên âm IPA đầy đủ
- ✅ Ví dụ câu + bản dịch
- ✅ Từ đồng nghĩa (synonyms)
- ✅ Từ trái nghĩa (antonyms)
- ✅ Phân loại theo part of speech

## ⚙️ Tùy Chỉnh

### Thay đổi tốc độ import

Trong file `import_vocabulary_ui.dart`, tìm dòng:

```dart
if ((i + 1) % 50 == 0) {
  await Future.delayed(const Duration(milliseconds: 300));
}
```

Thay đổi `milliseconds: 300` để điều chỉnh tốc độ:
- `100` = nhanh hơn
- `500` = chậm hơn (ổn định hơn)

### Skip từ đã tồn tại

Mặc định, script sẽ **bỏ qua** các từ đã tồn tại trong database. Nếu muốn **cập nhật** (update), thay đổi:

```dart
if (docSnapshot.exists) {
  // Bỏ qua
  skipCount++;
  continue;
}
```

Thành:

```dart
if (docSnapshot.exists) {
  // Cập nhật
  await docRef.update(data);
  successCount++;
  continue;
}
```

## 🔧 Xử Lý Lỗi

### Lỗi: Firebase not initialized

**Nguyên nhân**: Firebase chưa được cấu hình

**Giải pháp**:
```bash
# Cài đặt FlutterFire CLI
dart pub global activate flutterfire_cli

# Cấu hình Firebase
flutterfire configure
```

### Lỗi: Permission denied

**Nguyên nhân**: Firestore rules chưa cho phép ghi dữ liệu

**Giải pháp**: Cập nhật `firestore.rules`:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /vocabulary/{vocabId} {
      allow read: if true;
      allow write: if request.auth != null; // Hoặc true cho development
    }
  }
}
```

### Lỗi: Quota exceeded

**Nguyên nhân**: Đã vượt quá giới hạn Firebase free tier

**Giải pháp**:
1. Chờ 24h để quota reset
2. Hoặc nâng cấp lên Firebase Blaze plan
3. Giảm tốc độ import (tăng delay)

## 🧪 Test Dữ Liệu

### Kiểm tra trong ứng dụng

```dart
// Trong vocabulary_page.dart hoặc test file
void testVocabularyData() async {
  final firestore = FirebaseFirestore.instance;
  
  // Lấy 10 từ đầu tiên
  final snapshot = await firestore
      .collection('vocabulary')
      .limit(10)
      .get();
  
  for (var doc in snapshot.docs) {
    print('Word: ${doc['word']}');
    print('Meaning: ${doc['meaning']}');
    print('---');
  }
}
```

## 📝 Bước Tiếp Theo

Sau khi import thành công:

1. ✅ **Test dữ liệu** trong ứng dụng
2. ✅ **Thêm từ vựng các cấp độ khác** (A1, A2, B1, B2, C1)
3. ✅ **Thêm audio URL** cho pronunciation
4. ✅ **Thêm image URL** cho từ phù hợp
5. ✅ **Tạo bài học** (lessons) từ các từ vựng

## 🎓 Import Thêm Từ Vựng Các Cấp Độ Khác

Nếu bạn có file CSV cho các cấp độ khác (A1, A2, B1, B2, C1):

1. **Chuyển đổi CSV sang Dart**:
```bash
# Sửa đường dẫn file trong tools/csv_to_dart_converter.dart
# Thay đổi: final csvFile = File(r'd:\Download\Từ vựng - [LEVEL].csv');
dart run tools/csv_to_dart_converter.dart
```

2. **Import vào Firebase**:
```bash
flutter run -t lib/tools/import_vocabulary_ui.dart
```

## 💡 Tips & Tricks

### 1. Backup dữ liệu

Trước khi import, nên backup dữ liệu hiện tại:

```bash
# Export data từ Firestore
firebase firestore:export backup-$(date +%Y%m%d)
```

### 2. Monitor chi phí

- Theo dõi Firestore usage tại Firebase Console
- Free tier: 50K reads/day, 20K writes/day
- Import 6,757 từ = 6,757 writes (vẫn nằm trong giới hạn free tier!)

### 3. Optimize queries

Khi query trong app, nên:
- Cache dữ liệu với Hive (đã có)
- Limit số lượng kết quả
- Sử dụng pagination

## 📞 Hỗ Trợ

Nếu gặp vấn đề:
1. Kiểm tra logs trong console/UI
2. Xem Firebase Console → Firestore → Data
3. Kiểm tra Firebase Console → Functions → Logs (nếu có)

## ✨ Kết Luận

Bạn đã sẵn sàng để import 2,024 từ vựng C2 vào Firebase! Chúc bạn thành công! 🎉

---

**Cập nhật lần cuối**: 2024-12-25
**Tổng số từ đã chuyển đổi**: 6,757 từ (A1-C2)

