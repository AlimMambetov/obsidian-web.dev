---
title: Уведомление
---

## 🔔 **NOTIFICATION (Уведомление)**

---

### **1. СХЕМА NOTIFICATION**

```javascript
Notification {
  _id: ObjectId
  userId: ObjectId -> User, required
  
  // ========== ТИП ==========
  type: enum ['task_update', 'payment', 'agent_published', 'system']
  
  // ========== КОНТЕНТ ==========
  title: string, required
  message: string, required
  link: string
  
  // ========== СТАТУС ==========
  read: boolean, default: false
  
  // ========== АУДИТ ==========
  createdAt: Date
}
```

---

### **2. ОПИСАНИЕ ПОЛЕЙ**

---

#### **ОСНОВНЫЕ ПОЛЯ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `_id` | ObjectId | Да | Уникальный ID уведомления |
| `userId` | ObjectId -> User | Да | Кому отправлено |

---

#### **ТИП**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `type` | enum | Да | `'task_update'`, `'payment'`, `'agent_published'`, `'system'` |

---

#### **КОНТЕНТ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `title` | string | Да | Заголовок уведомления |
| `message` | string | Да | Текст уведомления |
| `link` | string | Нет | Ссылка для перехода |

---

#### **СТАТУС**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `read` | boolean | Да, default: false | Прочитано ли |

---

### **3. СВЯЗИ С ДРУГИМИ МОДЕЛЯМИ**

```
Notification
  └── userId → User
```

---

### **4. ИНДЕКСЫ**

```javascript
Notification.index({ userId: 1, read: 1, createdAt: -1 })
Notification.index({ userId: 1, createdAt: -1 })
Notification.index({ read: 1, createdAt: -1 })
```

