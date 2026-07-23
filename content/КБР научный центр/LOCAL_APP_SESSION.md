---
title: Сессия локального приложения
---

## 💻 **LOCAL APP SESSION (Сессия локального приложения)**

---

### **1. СХЕМА LOCAL APP SESSION**

```javascript
LocalAppSession {
  _id: ObjectId
  userId: ObjectId -> User, required
  machineId: string, required
  sessionId: string, unique
  
  // ========== СОСТОЯНИЕ ==========
  status: enum ['idle', 'training', 'syncing', 'error'], default: 'idle'
  
  // ========== ОБОРУДОВАНИЕ ==========
  hardware: {
    gpu: {
      name: string
      memory: number (MB)
      cudaCores: number
      temperature: number
      utilization: number (0-100)
    }
    cpu: {
      name: string
      cores: number
      utilization: number
    }
    ram: {
      total: number (MB)
      used: number
    }
  }
  
  // ========== ТЕКУЩЕЕ ОБУЧЕНИЕ ==========
  currentTraining: {
    agentId: ObjectId -> Agent
    modelId: ObjectId -> Model
    progress: number, min:0, max:100
    epoch: number
    loss: number
    accuracy: number
    estimatedTimeRemaining: number (seconds)
    startedAt: Date
  }
  
  // ========== ОЧЕРЕДЬ ==========
  trainingQueue: [{
    agentId: ObjectId -> Agent
    modelId: ObjectId -> Model
    priority: enum ['low', 'medium', 'high'], default: 'medium'
    requestedAt: Date
  }]
  
  // ========== СИНХРОНИЗАЦИЯ ==========
  sync: {
    lastSync: Date
    pendingUploads: [{
      file: string
      type: enum ['model', 'weights', 'dataset']
      size: number
      status: enum ['pending', 'uploading', 'completed', 'failed']
    }]
  }
  
  // ========== ЛОГИ ==========
  logs: [{
    timestamp: Date
    level: enum ['info', 'warning', 'error', 'debug']
    message: string
    data: Mixed
  }]
  
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
| `_id` | ObjectId | Да | Уникальный ID сессии |
| `userId` | ObjectId -> User | Да | Владелец сессии |
| `machineId` | string | Да | Уникальный ID компьютера |
| `sessionId` | string | Да, unique | ID текущей сессии |

---

#### **СОСТОЯНИЕ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да, default: 'idle' | `'idle'`, `'training'`, `'syncing'`, `'error'` |

---

#### **ОБОРУДОВАНИЕ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `hardware.gpu.name` | string | Нет | Название GPU |
| `hardware.gpu.memory` | number | Нет | Объем видеопамяти (MB) |
| `hardware.gpu.cudaCores` | number | Нет | Количество ядер CUDA |
| `hardware.gpu.temperature` | number | Нет | Температура GPU (°C) |
| `hardware.gpu.utilization` | number | Нет | Загрузка GPU (%) |
| `hardware.cpu.name` | string | Нет | Название CPU |
| `hardware.cpu.cores` | number | Нет | Количество ядер CPU |
| `hardware.cpu.utilization` | number | Нет | Загрузка CPU (%) |
| `hardware.ram.total` | number | Нет | Объем RAM (MB) |
| `hardware.ram.used` | number | Нет | Используемая RAM (MB) |

---

#### **ТЕКУЩЕЕ ОБУЧЕНИЕ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `currentTraining.agentId` | ObjectId -> Agent | Нет | Какой агент обучается |
| `currentTraining.modelId` | ObjectId -> Model | Нет | Какая модель обучается |
| `currentTraining.progress` | number | Нет | Прогресс (0-100) |
| `currentTraining.epoch` | number | Нет | Текущая эпоха |
| `currentTraining.loss` | number | Нет | Текущие потери |
| `currentTraining.accuracy` | number | Нет | Текущая точность |
| `currentTraining.estimatedTimeRemaining` | number | Нет | Осталось времени (сек) |
| `currentTraining.startedAt` | Date | Нет | Время начала обучения |

---

#### **ОЧЕРЕДЬ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `trainingQueue[].agentId` | ObjectId -> Agent | Да | Агент в очереди |
| `trainingQueue[].modelId` | ObjectId -> Model | Да | Модель в очереди |
| `trainingQueue[].priority` | enum | Да, default: 'medium' | `'low'`, `'medium'`, `'high'` |
| `trainingQueue[].requestedAt` | Date | Да | Время запроса |

---

#### **СИНХРОНИЗАЦИЯ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `sync.lastSync` | Date | Нет | Время последней синхронизации |
| `sync.pendingUploads[].file` | string | Нет | Имя файла |
| `sync.pendingUploads[].type` | enum | Нет | `'model'`, `'weights'`, `'dataset'` |
| `sync.pendingUploads[].size` | number | Нет | Размер файла |
| `sync.pendingUploads[].status` | enum | Нет | `'pending'`, `'uploading'`, `'completed'`, `'failed'` |

---

#### **ЛОГИ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `logs[].timestamp` | Date | Да | Время записи |
| `logs[].level` | enum | Да | `'info'`, `'warning'`, `'error'`, `'debug'` |
| `logs[].message` | string | Да | Текст сообщения |
| `logs[].data` | Mixed | Нет | Дополнительные данные |

---

### **3. СВЯЗИ С ДРУГИМИ МОДЕЛЯМИ**

```
LocalAppSession
  ├── userId → User
  ├── currentTraining.agentId → Agent
  ├── currentTraining.modelId → Model
  ├── trainingQueue[].agentId → Agent
  └── trainingQueue[].modelId → Model
```

---

### **4. ИНДЕКСЫ**

```javascript
LocalAppSession.index({ userId: 1, status: 1 })
LocalAppSession.index({ machineId: 1, sessionId: 1 })
LocalAppSession.index({ status: 1, 'currentTraining.startedAt': 1 })
LocalAppSession.index({ 'sync.pendingUploads.status': 1 })
```

