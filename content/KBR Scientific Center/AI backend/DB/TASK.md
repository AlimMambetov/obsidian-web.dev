---
title: Задание
---
## 📋 **ЗАДАНИЕ (TASK)**

---

### **Схема**

```javascript
Task {
  _id: ObjectId
  name: string, required
  description: string
  creator: ObjectId -> User, required

  // ========== СТАТУС ==========
  status: enum ['created', 'in_progress', 'completed', 'failed', 'cancelled'], default: 'created'

  // ========== УЧАСТНИКИ ==========
  users: [ObjectId -> User]
  agents: [ObjectId -> Agent]
  computers: [ObjectId -> Computer]
  devices: [ObjectId -> Device]

  // ========== РЕСУРСЫ ==========
  requiredResources: {
    gpu: number
    cpu: number
    ram: number
    storage: number
  }
  allocatedResources: {
    gpu: number
    cpu: number
    ram: number
    storage: number
  }

  // ========== СВЯЗИ АГЕНТОВ С УСТРОЙСТВАМИ ==========
  agentDeviceConnections: [{
    agentId: ObjectId -> Agent
    deviceId: ObjectId -> Device
    status: enum ['pending', 'active', 'completed', 'failed']
  }]

  // ========== СВЯЗИ АГЕНТОВ С ВЫЧИСЛИТЕЛЯМИ ==========
  agentComputerConnections: [{
    agentId: ObjectId -> Agent
    computerId: ObjectId -> Computer
    status: enum ['pending', 'active', 'completed', 'failed']
  }]

  // ========== ЧАТ ==========
  chat: {
    messages: [{
      senderId: ObjectId -> User
      senderType: enum ['user', 'agent', 'system']
      text: string
      fileUrl: string
      audioUrl: string
      videoUrl: string
      timestamp: Date
    }]
  }

  // ========== ТАЙМЛАЙН ==========
  timeline: [{
    event: string
    userId: ObjectId -> User
    timestamp: Date
  }]

  // ========== РЕЗУЛЬТАТ ==========
  result: {
    type: enum ['digital', 'physical']
    url: string
    fileUrl: string
    physicalStatus: {
      status: enum ['pending', 'printing', 'done', 'shipped']
      deviceId: ObjectId -> Device
      estimatedCompletion: Date
    }
  }

  // ========== 3D СЦЕНА ==========
  scene3d: {
    url: string
    animations: {
      idle: string
      active: string
    }
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
| `_id` | ObjectId | Да | Уникальный идентификатор задания |
| `name` | string | Да | Название задания |
| `description` | string | Нет | Описание задания |
| `creator` | ObjectId -> User | Да | Кто создал задание |

---

#### **Статус**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да, default: 'created' | `'created'`, `'in_progress'`, `'completed'`, `'failed'`, `'cancelled'` |

---

#### **Участники**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `users` | [ObjectId -> User] | Нет | Участники-пользователи |
| `agents` | [ObjectId -> Agent] | Нет | Участники-агенты |
| `computers` | [ObjectId -> Computer] | Нет | Используемые вычислители |
| `devices` | [ObjectId -> Device] | Нет | Используемые устройства |

---

#### **Ресурсы**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `requiredResources.gpu` | number | Нет | Требуется GPU |
| `requiredResources.cpu` | number | Нет | Требуется CPU |
| `requiredResources.ram` | number | Нет | Требуется RAM |
| `requiredResources.storage` | number | Нет | Требуется место |
| `allocatedResources.gpu` | number | Нет | Выделено GPU |
| `allocatedResources.cpu` | number | Нет | Выделено CPU |
| `allocatedResources.ram` | number | Нет | Выделено RAM |
| `allocatedResources.storage` | number | Нет | Выделено место |

---

#### **Связи агентов с устройствами**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `agentDeviceConnections[].agentId` | ObjectId -> Agent | Да | Агент |
| `agentDeviceConnections[].deviceId` | ObjectId -> Device | Да | Устройство |
| `agentDeviceConnections[].status` | enum | Да | `'pending'`, `'active'`, `'completed'`, `'failed'` |

---

#### **Связи агентов с вычислителями**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `agentComputerConnections[].agentId` | ObjectId -> Agent | Да | Агент |
| `agentComputerConnections[].computerId` | ObjectId -> Computer | Да | Вычислитель |
| `agentComputerConnections[].status` | enum | Да | `'pending'`, `'active'`, `'completed'`, `'failed'` |

---

#### **Чат**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `chat.messages[].senderId` | ObjectId -> User | Да | Отправитель |
| `chat.messages[].senderType` | enum | Да | `'user'`, `'agent'`, `'system'` |
| `chat.messages[].text` | string | Нет | Текст сообщения |
| `chat.messages[].fileUrl` | string | Нет | Ссылка на файл |
| `chat.messages[].audioUrl` | string | Нет | Ссылка на аудио |
| `chat.messages[].videoUrl` | string | Нет | Ссылка на видео |
| `chat.messages[].timestamp` | Date | Да | Время отправки |

---

#### **Таймлайн**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `timeline[].event` | string | Да | Событие |
| `timeline[].userId` | ObjectId -> User | Да | Кто совершил |
| `timeline[].timestamp` | Date | Да | Время события |

---

#### **Результат**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `result.type` | enum | Нет | `'digital'`, `'physical'` |
| `result.url` | string | Нет | Ссылка на результат |
| `result.fileUrl` | string | Нет | Файл результата |
| `result.physicalStatus.status` | enum | Нет | `'pending'`, `'printing'`, `'done'`, `'shipped'` |
| `result.physicalStatus.deviceId` | ObjectId -> Device | Нет | Какое устройство |
| `result.physicalStatus.estimatedCompletion` | Date | Нет | Ожидаемое завершение |

---

#### **3D сцена**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `scene3d.url` | string | Нет | URL 3D сцены |
| `scene3d.animations.idle` | string | Нет | Анимация ожидания |
| `scene3d.animations.active` | string | Нет | Активная анимация |

---

### **Связи с другими моделями**

```
Task
  ├── creator → User
  ├── users → User
  ├── agents → Agent
  ├── computers → Computer
  ├── devices → Device
  ├── agentDeviceConnections.agentId → Agent
  ├── agentDeviceConnections.deviceId → Device
  ├── agentComputerConnections.agentId → Agent
  ├── agentComputerConnections.computerId → Computer
  ├── result.physicalStatus.deviceId → Device
  └── Transaction (через taskId)
```

---

### **Индексы**

```javascript
Task.index({ creator: 1, status: 1 })
Task.index({ users: 1, status: 1 })
Task.index({ agents: 1, status: 1 })
Task.index({ status: 1, createdAt: -1 })
Task.index({ 'agentDeviceConnections.status': 1 })
Task.index({ 'agentComputerConnections.status': 1 })
```