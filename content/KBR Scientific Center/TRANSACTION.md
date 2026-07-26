---
title: Транзакция
---
## 💰 **ТРАНЗАКЦИЯ (TRANSACTION)**

---

### **Схема**

```javascript
Transaction {
  _id: ObjectId
  userId: ObjectId -> User, required

  // ========== ТИП ==========
  type: enum ['purchase', 'subscription', 'commission', 'withdrawal', 'refund']

  // ========== СУММА ==========
  amount: number, required
  currency: string, default: 'RUB'

  // ========== СВЯЗАННЫЕ СУЩНОСТИ ==========
  agentId: ObjectId -> Agent
  taskId: ObjectId -> Task

  // ========== СТАТУС ==========
  status: enum ['pending', 'completed', 'failed', 'refunded'], default: 'pending'

  // ========== ПЛАТЕЖ ==========
  paymentMethod: string
  transactionId: string

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
| `_id` | ObjectId | Да | Уникальный идентификатор транзакции |
| `userId` | ObjectId -> User | Да | Пользователь, совершивший транзакцию |

---

#### **Тип**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `type` | enum | Да | `'purchase'`, `'subscription'`, `'commission'`, `'withdrawal'`, `'refund'` |

---

#### **Сумма**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `amount` | number | Да | Сумма транзакции |
| `currency` | string | Да, default: 'RUB' | Валюта |

---

#### **Связанные сущности**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `agentId` | ObjectId -> Agent | Нет | Какой агент (если покупка) |
| `taskId` | ObjectId -> Task | Нет | Какая задача (если оплата) |

---

#### **Статус**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да, default: 'pending' | `'pending'`, `'completed'`, `'failed'`, `'refunded'` |

---

#### **Платеж**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `paymentMethod` | string | Нет | Способ оплаты |
| `transactionId` | string | Нет | ID в платежной системе |

---

### **Связи с другими моделями**

```
Transaction
  ├── userId → User
  ├── agentId → Agent
  └── taskId → Task
```

---

### **Индексы**

```javascript
Transaction.index({ userId: 1, status: 1 })
Transaction.index({ agentId: 1 })
Transaction.index({ createdAt: -1 })
Transaction.index({ status: 1, type: 1 })
```