# 🔥 FIRESTORE DATABASE SETUP - DASHBOARD

## ❌ VẤN ĐỀ

Dashboard không load được vì thiếu **Firestore Indexes**:

```
[cloud_firestore/failed-precondition] 
The query requires an index. You can create it here: https://...
```

---

## ✅ GIẢI PHÁP - TẠO INDEXES

### **METHOD 1: TẠO QUA FIREBASE CONSOLE (KHUYẾN NGHỊ)** ⭐

#### **Bước 1: Mở Firebase Console**

1. Vào: https://console.firebase.google.com
2. Chọn project: `ung-dung-hoc-tieng-anh-a0580`
3. Sidebar → **Firestore Database**
4. Tab → **Indexes**

#### **Bước 2: Tạo Index 1 - Learning Sessions**

1. Click **Create Index** (hoặc **+ Add Index**)
2. Điền thông tin:

```
Collection ID: learning_sessions

Fields to index:
┌──────────────┬──────────────┐
│ Field        │ Order        │
├──────────────┼──────────────┤
│ user_id      │ Ascending    │
│ start_time   │ Descending   │
└──────────────┴──────────────┘

Query scope: Collection
```

3. Click **Create**
4. Đợi status: "Building..." → "Enabled" (≈1-3 phút)

#### **Bước 3: Tạo Index 2 - Exercise Results**

1. Click **Create Index**
2. Điền thông tin:

```
Collection ID: exercise_results

Fields to index:
┌──────────────┬──────────────┐
│ Field        │ Order        │
├──────────────┼──────────────┤
│ user_id      │ Ascending    │
│ created_at   │ Descending   │
└──────────────┴──────────────┘

Query scope: Collection
```

3. Click **Create**
4. Đợi status: "Building..." → "Enabled"

#### **Bước 4: (Optional) Index 3 - Exercise Results với Skill Filter**

Nếu Dashboard filter theo skill:

```
Collection ID: exercise_results

Fields to index:
┌──────────────┬──────────────┐
│ Field        │ Order        │
├──────────────┼──────────────┤
│ user_id      │ Ascending    │
│ skill        │ Ascending    │
│ created_at   │ Descending   │
└──────────────┴──────────────┘

Query scope: Collection
```

#### **Bước 5: Verify**

1. Vào tab **Indexes**
2. Kiểm tra status: ✅ **Enabled** (3 indexes)
3. Quay lại app → Tap "Thử lại"
4. Dashboard sẽ load thành công! 🎉

---

### **METHOD 2: DEPLOY QUA FIREBASE CLI** 💻

#### **Bước 1: Install Firebase CLI**

```bash
npm install -g firebase-tools
```

#### **Bước 2: Login**

```bash
firebase login
```

#### **Bước 3: Init Firebase trong project**

```bash
cd "c:\File Coding\ung_dung_hoc_tieng_anh"
firebase init firestore
```

**Chọn:**
- Use existing project: `ung-dung-hoc-tieng-anh-a0580`
- Firestore rules file: `firestore.rules`
- Firestore indexes file: `firestore.indexes.json`

#### **Bước 4: Deploy**

```bash
firebase deploy --only firestore:indexes
```

Output:
```
✔ Deployed Firestore indexes
✔ Created index on learning_sessions (user_id ASC, start_time DESC)
✔ Created index on exercise_results (user_id ASC, created_at DESC)
```

#### **Bước 5: Verify**

```bash
firebase firestore:indexes
```

---

## 📊 INDEXES CẦN TẠO

### **Index 1: learning_sessions**

```json
{
  "collectionGroup": "learning_sessions",
  "fields": [
    { "fieldPath": "user_id", "order": "ASCENDING" },
    { "fieldPath": "start_time", "order": "DESCENDING" }
  ]
}
```

**Dùng cho queries:**
```dart
collection('learning_sessions')
  .where('user_id', isEqualTo: userId)
  .orderBy('start_time', descending: true)
```

**Mục đích:**
- Lấy danh sách sessions của user
- Sắp xếp theo thời gian (mới nhất trước)
- Tính total learning minutes
- Tính streak

---

### **Index 2: exercise_results**

```json
{
  "collectionGroup": "exercise_results",
  "fields": [
    { "fieldPath": "user_id", "order": "ASCENDING" },
    { "fieldPath": "created_at", "order": "DESCENDING" }
  ]
}
```

**Dùng cho queries:**
```dart
collection('exercise_results')
  .where('user_id', isEqualTo: userId)
  .orderBy('created_at', descending: true)
```

**Mục đích:**
- Lấy danh sách results của user
- Sắp xếp theo thời gian
- Tính total XP
- Tính accuracy

---

### **Index 3: exercise_results (với skill filter)**

```json
{
  "collectionGroup": "exercise_results",
  "fields": [
    { "fieldPath": "user_id", "order": "ASCENDING" },
    { "fieldPath": "skill", "order": "ASCENDING" },
    { "fieldPath": "created_at", "order": "DESCENDING" }
  ]
}
```

**Dùng cho queries:**
```dart
collection('exercise_results')
  .where('user_id', isEqualTo: userId)
  .where('skill', isEqualTo: 'vocabulary')
  .orderBy('created_at', descending: true)
```

**Mục đích:**
- Filter results theo skill (vocabulary, grammar, etc.)
- Skill progress tracking
- Weak skill detection

---

## 🔒 FIRESTORE SECURITY RULES

File `firestore.rules` đã được tạo với security rules:

### **Key Rules:**

```javascript
// Users collection
- User chỉ đọc/write data của chính mình
- Không cho phép delete

// Learning Sessions
- User chỉ đọc/write sessions của mình
- Immutable khi đã tạo (không delete)

// Exercise Results
- User chỉ đọc/write results của mình
- Immutable (không update/delete)

// Vocabulary
- Tất cả users có thể đọc
- Chỉ admin (console) có thể write
```

### **Deploy Rules:**

```bash
firebase deploy --only firestore:rules
```

---

## 🧪 TESTING

### **Test 1: Check Indexes Status**

```bash
firebase firestore:indexes
```

Expected output:
```
┌─────────────────────┬────────┬─────────────┬──────────────┐
│ Collection          │ Fields │ Status      │ Created      │
├─────────────────────┼────────┼─────────────┼──────────────┤
│ learning_sessions   │ 2      │ ✅ ENABLED  │ 2025-12-27   │
│ exercise_results    │ 2      │ ✅ ENABLED  │ 2025-12-27   │
│ exercise_results    │ 3      │ ✅ ENABLED  │ 2025-12-27   │
└─────────────────────┴────────┴─────────────┴──────────────┘
```

### **Test 2: Verify trong App**

1. Mở app
2. Navigate to Dashboard
3. Kiểm tra:
   - ✅ Total XP hiển thị
   - ✅ Current Streak hiển thị
   - ✅ Learning Minutes hiển thị
   - ✅ Skill Progress hiển thị
   - ✅ Không có error "requires an index"

### **Test 3: Check Firestore Console**

1. Vào: Firestore Database → **Indexes** tab
2. Verify:
   - ✅ 3 indexes hiển thị
   - ✅ Status: "Enabled" (màu xanh)
   - ✅ Không có "Building" hay "Error"

---

## ⚠️ COMMON ISSUES

### **Issue 1: "Building" quá lâu**

**Symptom:**
```
Index status: Building... (> 10 minutes)
```

**Solution:**
- Đợi thêm (có thể 30 phút nếu database lớn)
- Nếu > 1 giờ: Delete index và tạo lại

---

### **Issue 2: "Permission Denied"**

**Symptom:**
```
[permission-denied] Missing or insufficient permissions
```

**Solution:**
1. Check Firebase Console → Authentication
2. Verify user đã login
3. Check `firestore.rules` đã deploy chưa:
   ```bash
   firebase deploy --only firestore:rules
   ```

---

### **Issue 3: "Index already exists"**

**Symptom:**
```
Error: Index already exists
```

**Solution:**
- Bỏ qua (index đã có sẵn)
- Hoặc check trong Console → Indexes → Verify status

---

### **Issue 4: Query vẫn lỗi sau khi tạo index**

**Symptom:**
```
Still shows "requires an index" error
```

**Solution:**
1. Force refresh app (close & reopen)
2. Check index status: "Enabled" chưa?
3. Verify collection names chính xác:
   - `learning_sessions` (not `learning_session`)
   - `exercise_results` (not `exercise_result`)

---

## 📝 FILES CREATED

```
Created:
✅ firestore.indexes.json
   - Composite indexes config
   
✅ firestore.rules
   - Security rules
   
✅ FIRESTORE_SETUP_GUIDE.md
   - Complete setup guide (this file)
```

---

## 🚀 QUICK START

### **Fastest way (3 minutes):**

1. **Open Firebase Console**
   - Go to: https://console.firebase.google.com
   - Project: `ung-dung-hoc-tieng-anh-a0580`
   - Firestore Database → Indexes

2. **Create 2 indexes:**
   - Index 1: `learning_sessions` (user_id ASC, start_time DESC)
   - Index 2: `exercise_results` (user_id ASC, created_at DESC)

3. **Wait for "Enabled" status** (1-3 minutes)

4. **Test app**
   - Open Dashboard
   - Tap "Thử lại"
   - ✅ Should work!

---

## 📞 SUPPORT

**If still not working:**

1. Check indexes in Firebase Console
2. Verify user is authenticated
3. Check Firestore rules
4. Read error message carefully
5. Create index from error link

**Firebase Console:**
https://console.firebase.google.com/project/ung-dung-hoc-tieng-anh-a0580/firestore/indexes

---

## 🎉 SUCCESS

**When everything works:**

✅ 3 indexes created and enabled
✅ Dashboard loads data successfully
✅ XP, Streak, Minutes display correctly
✅ Skill Progress shows real data
✅ No "requires an index" errors

**Dashboard is now LIVE! 🎊**

