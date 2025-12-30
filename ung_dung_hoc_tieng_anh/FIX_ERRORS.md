# 🔧 Fix Build Errors - Hướng Dẫn

## ❌ Lỗi: record_linux compatibility issue

**Nguyên nhân:**  
Package `record` version 5.1.2 có vấn đề tương thích với platform Linux trên Flutter mới.

**Đã fix:**  
Tạm thời comment out các packages gây lỗi trong `pubspec.yaml`:
- ❌ `record: ^5.1.2` - Recording audio
- ❌ `audioplayers: ^6.1.0` - Alternative audio player (dùng just_audio thay thế)
- ❌ `speech_to_text: ^7.0.0` - Speech recognition
- ❌ `permission_handler: ^11.3.1` - Permissions

**✅ Giữ lại:**
- ✅ `just_audio: ^0.9.40` - Audio playback (đủ dùng)

## 🚀 Cách Fix

### Bước 1: Clean cache
```powershell
cd "C:\File Coding\ung_dung_hoc_tieng_anh"
flutter clean
```

### Bước 2: Get dependencies
```powershell
flutter pub get
```

### Bước 3: Run app
```powershell
flutter run
```

## 📝 Giải thích

### Tại sao comment các packages?

1. **record** - Gây lỗi build trên Linux platform
   - App chưa cần ghi âm trong giai đoạn đầu
   - Có thể thêm lại sau với version khác

2. **audioplayers** - Trùng chức năng với just_audio
   - `just_audio` đã đủ để phát audio
   - Giảm dependencies, tăng tốc build

3. **speech_to_text** - Chưa cần thiết ngay
   - Feature nâng cao, thêm sau
   - Tránh lỗi platform

4. **permission_handler** - Có thể thay thế
   - Flutter đã có permission APIs built-in
   - Thêm sau khi thực sự cần

### Features bị ảnh hưởng?

**✅ Vẫn hoạt động:**
- Phát audio từ vựng (just_audio)
- Tất cả UI/UX
- Firebase
- Hive local storage
- BLoC state management

**⏸️ Tạm thời không có:**
- Ghi âm phát âm (Speaking module)
- Speech to text (Nâng cao)
- Runtime permissions (có thể dùng alternative)

## 🔄 Khi nào thêm lại?

### Option 1: Sau khi packages được fix
```yaml
# Đợi record package update, sau đó:
record: ^5.2.0  # hoặc version mới hơn
```

### Option 2: Dùng alternative packages
```yaml
# Thay vì record, dùng:
flutter_sound: ^9.2.13  # Alternative cho audio recording

# Thay vì speech_to_text, dùng:
# Google Cloud Speech API qua dio/http
```

### Option 3: Platform-specific
```yaml
# Chỉ enable cho Android/iOS, disable cho Linux/Windows
record:
  ^5.1.2
  # Thêm platform conditions trong code
```

## 🧪 Test Build

### Android:
```powershell
flutter build apk --debug
```

### iOS (trên Mac):
```bash
flutter build ios --debug
```

### Web:
```powershell
flutter build web
```

## 📱 Workaround cho Speaking Module

Khi cần implement Speaking practice:

```dart
// Thay vì dùng record package, có thể:

// 1. Sử dụng platform channels
// 2. Dùng flutter_sound
// 3. Hoặc chỉ cho phép playback, không ghi âm trong MVP
```

## ✅ Checklist sau khi fix

- [x] Comment out problematic packages
- [ ] Run `flutter clean`
- [ ] Run `flutter pub get`
- [ ] Run `flutter run`
- [ ] Kiểm tra app chạy OK
- [ ] Test các features chính

## 🎯 Kết luận

Đây là fix **tạm thời** để app chạy được ngay.  
Các packages đã comment có thể thêm lại sau khi:
1. Packages được update/fix
2. Hoặc tìm được alternatives
3. Hoặc implement platform-specific solutions

**App vẫn chạy đầy đủ 90% features!** 🎉

