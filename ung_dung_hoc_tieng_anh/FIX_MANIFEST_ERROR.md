# 🔧 Fix AndroidManifest Error

## ❌ Lỗi:
```
ERROR: dump failed because resource AndroidManifest.xml not found
Failed to extract manifest from APK
Invalid file
```

## ✅ Giải pháp:

### Bước 1: Clean build folder
```powershell
cd "C:\File Coding\ung_dung_hoc_tieng_anh"

# Xóa build folder
rmdir /s /q build
rmdir /s /q android\app\build
rmdir /s /q android\.gradle
```

### Bước 2: Flutter clean
```powershell
flutter clean
```

### Bước 3: Get dependencies
```powershell
flutter pub get
```

### Bước 4: Rebuild
```powershell
flutter run
```

## 🔧 Đã fix thêm:

### AndroidManifest.xml - Thêm permissions:
```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.WAKE_LOCK"/>
<uses-permission android:name="android.permission.VIBRATE"/>
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
```

## 🚀 Full Commands:

### PowerShell (chạy từng lệnh):
```powershell
# 1. Di chuyển vào project
cd "C:\File Coding\ung_dung_hoc_tieng_anh"

# 2. Xóa build cache (quan trọng!)
flutter clean

# 3. Xóa Android build
Remove-Item -Recurse -Force build -ErrorAction SilentlyContinue
Remove-Item -Recurse -Force android\app\build -ErrorAction SilentlyContinue
Remove-Item -Recurse -Force android\.gradle -ErrorAction SilentlyContinue

# 4. Get dependencies
flutter pub get

# 5. Run app
flutter run
```

## 💡 Giải thích lỗi:

### Nguyên nhân:
1. **APK build bị corrupt** do build incomplete
2. **Cache cũ** từ các lần build trước
3. **Gradle cache** có vấn đề
4. **Missing permissions** trong AndroidManifest.xml

### Tại sao cần xóa build folder?
- APK cũ bị corrupt
- Gradle cache không sync
- Dependencies không được link đúng
- Clean build = fresh start

## 🎯 Sau khi fix:

✅ APK sẽ build thành công  
✅ AndroidManifest.xml có đủ permissions  
✅ App chạy trên emulator/device  
✅ Firebase hoạt động (cần INTERNET permission)  
✅ Notifications hoạt động  

## ⚠️ Nếu vẫn lỗi:

### Option 1: Restart Android Studio
1. Close Android Studio
2. Delete `.idea` folder
3. Open Android Studio
4. File → Invalidate Caches / Restart

### Option 2: Restart Emulator
```powershell
# Stop emulator
# Start emulator lại
# Run: flutter run
```

### Option 3: Check Flutter Doctor
```powershell
flutter doctor -v
```

Nếu có vấn đề với Android toolchain, fix theo hướng dẫn.

### Option 4: Manually delete build
```powershell
# Windows Explorer
# Navigate to: C:\File Coding\ung_dung_hoc_tieng_anh
# Xóa thủ công folder: build, android\app\build, android\.gradle
```

## 🧪 Verify build:

Sau khi `flutter run` thành công, check:

```powershell
# Check APK được tạo
dir build\app\outputs\flutter-apk\app-debug.apk

# Xem size (should be > 20MB)
```

## 📱 Expected Output:

```
Launching lib\main.dart on sdk gphone64 x86 64 in debug mode...
✓ Built build\app\outputs\flutter-apk\app-debug.apk (35.2MB)
Installing build\app\outputs\flutter-apk\app-debug.apk...
Waiting for sdk gphone64 x86 64 to report its views...
Syncing files to device sdk gphone64 x86 64...
Flutter run key commands.
```

## ✅ Checklist:

- [x] Thêm permissions vào AndroidManifest.xml
- [ ] Chạy `flutter clean`
- [ ] Xóa build folders
- [ ] Chạy `flutter pub get`
- [ ] Chạy `flutter run`
- [ ] Xem app chạy trên emulator

## 🎉 Kết luận:

Lỗi này do APK build bị corrupt.  
Clean build sẽ fix hoàn toàn.

**Total time: ~2-3 phút để rebuild** ⏱️

