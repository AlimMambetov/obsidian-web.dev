---
title: Проект
---

## 📁 **PROJECT (Проект)**

---

### **1. СХЕМА PROJECT**

```javascript
Project {
  _id: ObjectId
  name: string, required
  description: string
  creator: ObjectId -> User, required
  
  // ========== СВЯЗАННЫЕ СУЩНОСТИ ==========
  users: [ObjectId -> User]
  agents: [ObjectId -> Agent]
  computers: [ObjectId -> Computer]
  devices: [ObjectId -> Device]
  tasks: [ObjectId -> Task]
  
  // ========== СТАТУС ==========
  status: enum ['draft', 'active', 'archived'], default: 'draft'
  
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
| `_id` | ObjectId | Да | Уникальный ID проекта |
| `name` | string | Да | Название проекта |
| `description` | string | Нет | Описание проекта |
| `creator` | ObjectId -> User | Да | Владелец проекта |

---

#### **СВЯЗАННЫЕ СУЩНОСТИ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `users` | [ObjectId -> User] | Нет | Участники проекта |
| `agents` | [ObjectId -> Agent] | Нет | Агенты в проекте |
| `computers` | [ObjectId -> Computer] | Нет | Вычислители в проекте |
| `devices` | [ObjectId -> Device] | Нет | Устройства в проекте |
| `tasks` | [ObjectId -> Task] | Нет | Задачи в проекте |

---

#### **СТАТУС**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да, default: 'draft' | `'draft'`, `'active'`, `'archived'` |

---

### **3. СВЯЗИ С ДРУГИМИ МОДЕЛЯМИ**

```
Project
  ├── creator → User
  ├── users → User
  ├── agents → Agent
  ├── computers → Computer
  ├── devices → Device
  └── tasks → Task
```

---

### **4. ИНДЕКСЫ**

```javascript
Project.index({ creator: 1, status: 1 })
Project.index({ users: 1 })
Project.index({ status: 1, createdAt: -1 })
Project.index({ name: 'text', description: 'text' })
```
