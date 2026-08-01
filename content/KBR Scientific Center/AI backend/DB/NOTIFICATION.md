---
title: Уведомление
---
## 🔔 **УВЕДОМЛЕНИЕ (NOTIFICATION)**

---

### **Схема**

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

### **Описание полей**

---

#### **Основные поля**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `_id` | ObjectId | Да | Уникальный идентификатор уведомления |
| `userId` | ObjectId -> User | Да | Кому отправлено |

---

#### **Тип**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `type` | enum | Да | `'task_update'`, `'payment'`, `'agent_published'`, `'system'` |

---

#### **Контент**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `title` | string | Да | Заголовок уведомления |
| `message` | string | Да | Текст уведомления |
| `link` | string | Нет | Ссылка для перехода |

---

#### **Статус**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `read` | boolean | Да, default: false | Прочитано ли |

---

### **Связи с другими моделями**

```
Notification
  └── userId → User
```

---

### **Индексы**

```javascript
Notification.index({ userId: 1, read: 1, createdAt: -1 })
Notification.index({ userId: 1, createdAt: -1 })
Notification.index({ read: 1, createdAt: -1 })
```