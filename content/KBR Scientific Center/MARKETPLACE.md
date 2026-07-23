---
title: Маркетплейс
---

## 🏪 **MARKETPLACE LISTING (Маркетплейс)**

---

### **1. СХЕМА MARKETPLACE LISTING**

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

### **2. ОПИСАНИЕ ПОЛЕЙ**

---

#### **ОСНОВНЫЕ ПОЛЯ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `_id` | ObjectId | Да | Уникальный ID листинга |
| `agentId` | ObjectId -> Agent | Да | Какой агент продается |
| `sellerId` | ObjectId -> User | Да | Кто продает |

---

#### **ЦЕНЫ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `pricePerCall` | number | Нет | Цена за один вызов |
| `subscriptionPrice` | number | Нет | Цена за подписку (месяц) |

---

#### **СТАТИСТИКА**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `totalSales` | number | Да, default: 0 | Количество продаж |
| `totalRevenue` | number | Да, default: 0 | Общая выручка |

---

#### **ОТЗЫВЫ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `reviews[].userId` | ObjectId -> User | Да | Кто оставил отзыв |
| `reviews[].rating` | number | Да | Оценка (1-5) |
| `reviews[].comment` | string | Нет | Текст отзыва |
| `reviews[].createdAt` | Date | Да | Дата отзыва |

---

#### **СТАТУС**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да, default: 'active' | `'active'`, `'paused'`, `'archived'` |

---

### **3. СВЯЗИ С ДРУГИМИ МОДЕЛЯМИ**

```
MarketplaceListing
  ├── agentId → Agent
  ├── sellerId → User
  └── reviews.userId → User
```

---

### **4. ИНДЕКСЫ**

```javascript
MarketplaceListing.index({ status: 1, pricePerCall: 1 })
MarketplaceListing.index({ sellerId: 1, status: 1 })
MarketplaceListing.index({ agentId: 1 }, { unique: true })
MarketplaceListing.index({ 'reviews.rating': -1 })
```

