# 🚨 LỖI DASHBOARD - FIX NGAY (3 PHÚT)

## ❌ Lỗi hiện tại

```
Không thể tải dữ liệu: [cloud_firestore/failed-precondition] 
The query requires an index.
```

## ✅ Giải pháp nhanh nhất

### **Bước 1: Mở link trong error message**

1. Nhìn vào error message trong app
2. Copy link bắt đầu bằng: `https://console.firebase.google.com...`
3. Paste vào browser → Enter
4. Click **"Create Index"** → Đợi 1-3 phút
5. Quay lại app → Tap **"Thử lại"**

**✅ XONG!**

---

## 📋 Hoặc tạo thủ công

### **Vào Firebase Console:**

https://console.firebase.google.com/project/ung-dung-hoc-tieng-anh-a0580/firestore/indexes

### **Tạo 2 indexes:**

#### **Index 1:**
```
Collection: learning_sessions
Fields:
  - user_id: Ascending ⬆️
  - start_time: Descending ⬇️
```

#### **Index 2:**
```
Collection: exercise_results
Fields:
  - user_id: Ascending ⬆️
  - created_at: Descending ⬇️
```

### **Đợi status = "Enabled" (1-3 phút)**

### **Test app → Dashboard sẽ load! ✅**

---

## 🖥️ Hoặc dùng CLI (nếu biết)

```powershell
cd "c:\File Coding\ung_dung_hoc_tieng_anh"
.\deploy_firestore.ps1
```

---

## 📖 Hướng dẫn chi tiết

Xem: [FIRESTORE_SETUP_GUIDE.md](./FIRESTORE_SETUP_GUIDE.md)

---

## 🎯 Kết quả

**Khi thành công:**
- ✅ Dashboard hiển thị XP
- ✅ Hiển thị Streak
- ✅ Hiển thị Learning Minutes
- ✅ Hiển thị Skill Progress
- ✅ Không còn lỗi "requires an index"

**Dashboard sẽ hoạt động bình thường! 🎉**

