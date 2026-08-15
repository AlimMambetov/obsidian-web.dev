---
title: API КЛЮЧ
---
## 🔑 **API КЛЮЧ (APIKEY)**

---

### **Схема**

```javascript
ApiKey {
  _id: ObjectId
  userId: ObjectId -> User, required

  // ========== КЛЮЧ ==========
  keyHash: string, required
  prefix: string, required

  // ========== ИНФОРМАЦИЯ ==========
  name: string

  // ========== ПРАВА ДОСТУПА ==========
  permissions: [enum], default: ['training']

  // ========== СТАТУС ==========
  isActive: boolean, default: true
  lastUsedAt: Date
  expiresAt: Date

  // ========== СТАТИСТИКА ==========
  usageStats: {
    totalRequests: number, default: 0
    totalTrainingHours: number, default: 0
    lastRequestAt: Date
  }

  // ========== АУДИТ ==========
  createdAt: Date
  updatedAt: Date
}
```

---

### **Описание полей**

---

#### **Основные поля**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `_id` | ObjectId | Да | Уникальный идентификатор ключа |
| `userId` | ObjectId -> User | Да | Владелец ключа (создатель) |

---

#### **Ключ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `keyHash` | string | Да | Хеш ключа (bcrypt). Сам ключ не хранится |
| `prefix` | string | Да | Префикс ключа для отображения (например, `kbr_live_a1b2c3`) |

---

#### **Информация**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `name` | string | Нет | Название ключа (например, "Мой домашний ПК") |

---

#### **Права доступа**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `permissions` | [enum] | Да, default: ['training'] | Права: `'training'`, `'sync'`, `'models'`, `'dataset'`, `'all'` |

---

#### **Статус**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `isActive` | boolean | Да, default: true | Активен ли ключ |
| `lastUsedAt` | Date | Нет | Дата последнего использования |
| `expiresAt` | Date | Нет | Дата истечения (опционально) |

---

#### **Статистика использования**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `usageStats.totalRequests` | number | Да, default: 0 | Количество запросов |
| `usageStats.totalTrainingHours` | number | Да, default: 0 | Часов обучения |
| `usageStats.lastRequestAt` | Date | Нет | Время последнего запроса |

---

### **Как работает**

1. **Создатель** генерирует ключ через интерфейс
2. Бэк генерирует случайный ключ: `kbr_live_<random>`
3. Сохраняет **хеш** ключа в `ApiKey`
4. **Показывает ключ 1 раз** (сохранить сразу!)
5. Создатель вводит ключ в локальное приложение
6. Приложение отправляет ключ в заголовке: `X-API-Key: kbr_live_...`
7. Бэк проверяет хеш и пропускает запрос

---

### **Связи с другими моделями**

```
ApiKey
  └── userId → User
```

---

### **Индексы**

```javascript
ApiKey.index({ userId: 1, isActive: 1 })
ApiKey.index({ keyHash: 1 })
ApiKey.index({ expiresAt: 1 })
```