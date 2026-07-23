---
title: Транзакция
---

## 💰 **TRANSACTION (Транзакция)**

---

### **1. СХЕМА TRANSACTION**

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

### **2. ОПИСАНИЕ ПОЛЕЙ**

---

#### **ОСНОВНЫЕ ПОЛЯ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `_id` | ObjectId | Да | Уникальный ID транзакции |
| `userId` | ObjectId -> User | Да | Пользователь, совершивший транзакцию |

---

#### **ТИП**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `type` | enum | Да | `'purchase'`, `'subscription'`, `'commission'`, `'withdrawal'`, `'refund'` |

---

#### **СУММА**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `amount` | number | Да | Сумма транзакции |
| `currency` | string | Да, default: 'RUB' | Валюта |

---

#### **СВЯЗАННЫЕ СУЩНОСТИ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `agentId` | ObjectId -> Agent | Нет | Какой агент (если покупка) |
| `taskId` | ObjectId -> Task | Нет | Какая задача (если оплата) |

---

#### **СТАТУС**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да, default: 'pending' | `'pending'`, `'completed'`, `'failed'`, `'refunded'` |

---

#### **ПЛАТЕЖ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `paymentMethod` | string | Нет | Способ оплаты |
| `transactionId` | string | Нет | ID в платежной системе |

---

### **3. СВЯЗИ С ДРУГИМИ МОДЕЛЯМИ**

```
Transaction
  ├── userId → User
  ├── agentId → Agent
  └── taskId → Task
```

---

### **4. ИНДЕКСЫ**

```javascript
Transaction.index({ userId: 1, status: 1 })
Transaction.index({ agentId: 1 })
Transaction.index({ createdAt: -1 })
Transaction.index({ status: 1, type: 1 })
```

