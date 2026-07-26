---
title: Маркетплейс
---
## 🏪 **МАРКЕТПЛЕЙС (MARKETPLACE LISTING)**

---

### **Схема**

```javascript
MarketplaceListing {
  _id: ObjectId
  agentId: ObjectId -> Agent, required
  sellerId: ObjectId -> User, required

  // ========== ЦЕНЫ ==========
  pricePerCall: number
  subscriptionPrice: number

  // ========== СТАТИСТИКА ==========
  totalSales: number, default: 0
  totalRevenue: number, default: 0

  // ========== ОТЗЫВЫ ==========
  reviews: [{
    userId: ObjectId -> User
    rating: number, min:1, max:5
    comment: string
    createdAt: Date
  }]

  // ========== СТАТУС ==========
  status: enum ['active', 'paused', 'archived'], default: 'active'

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
| `_id` | ObjectId | Да | Уникальный идентификатор листинга |
| `agentId` | ObjectId -> Agent | Да | Какой агент продается |
| `sellerId` | ObjectId -> User | Да | Кто продает |

---

#### **Цены**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `pricePerCall` | number | Нет | Цена за один вызов |
| `subscriptionPrice` | number | Нет | Цена за подписку (месяц) |

---

#### **Статистика**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `totalSales` | number | Да, default: 0 | Количество продаж |
| `totalRevenue` | number | Да, default: 0 | Общая выручка |

---

#### **Отзывы**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `reviews[].userId` | ObjectId -> User | Да | Кто оставил отзыв |
| `reviews[].rating` | number | Да | Оценка (1-5) |
| `reviews[].comment` | string | Нет | Текст отзыва |
| `reviews[].createdAt` | Date | Да | Дата отзыва |

---

#### **Статус**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да, default: 'active' | `'active'`, `'paused'`, `'archived'` |

---

### **Связи с другими моделями**

```
MarketplaceListing
  ├── agentId → Agent
  ├── sellerId → User
  └── reviews.userId → User
```

---

### **Индексы**

```javascript
MarketplaceListing.index({ status: 1, pricePerCall: 1 })
MarketplaceListing.index({ sellerId: 1, status: 1 })
MarketplaceListing.index({ agentId: 1 }, { unique: true })
MarketplaceListing.index({ 'reviews.rating': -1 })
```