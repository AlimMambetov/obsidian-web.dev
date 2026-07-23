---
title: Процесс обучения
---

## ⚙️ **TRAINING PROCESS (Процесс обучения)**

---

### **1. СХЕМА TRAINING PROCESS**

```javascript
TrainingProcess {
  _id: ObjectId
  modelId: ObjectId -> Model, required
  agentId: ObjectId -> Agent, required
  creator: ObjectId -> User, required
  
  // ========== ТИП ОБУЧЕНИЯ ==========
  type: enum ['local', 'cloud']
  
  // ========== МЕСТО ОБУЧЕНИЯ ==========
  location: {
    machineId: string
    serverId: string
  }
  
  // ========== ПАРАМЕТРЫ ОБУЧЕНИЯ ==========
  config: {
    epochs: number
    batchSize: number
    learningRate: number
    optimizer: string
    lossFunction: string
    datasetId: ObjectId -> Dataset
  }
  
  // ========== СТАТУС ==========
  status: enum ['queued', 'preparing', 'training', 'validating', 'completed', 'failed', 'cancelled']
  
  // ========== ПРОГРЕСС ==========
  progress: {
    currentEpoch: number
    totalEpochs: number
    percentage: number, min:0, max:100
    loss: number
    accuracy: number
    validationLoss: number
    validationAccuracy: number
  }
  
  // ========== РЕСУРСЫ ==========
  resources: {
    gpuUsage: number
    cpuUsage: number
    ramUsage: number
    temperature: number
  }
  
  // ========== ВРЕМЯ ==========
  startedAt: Date
  completedAt: Date
  estimatedTimeRemaining: number (seconds)
  
  // ========== ЛОГИ ==========
  logs: [{
    timestamp: Date
    message: string
    level: enum ['info', 'warning', 'error', 'debug']
    data: Mixed
  }]
  
  // ========== МЕТРИКИ ==========
  metrics: {
    finalAccuracy: number
    finalLoss: number
    f1Score: number
    precision: number
    recall: number
    confusionMatrix: [[number]]
  }
  
  // ========== РЕЗУЛЬТАТ ==========
  result: {
    modelVersion: string
    fileUrl: string (S3)
    size: number
  }
  
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
| `_id` | ObjectId | Да | Уникальный ID процесса |
| `modelId` | ObjectId -> Model | Да | Какая модель обучается |
| `agentId` | ObjectId -> Agent | Да | Для какого агента |
| `creator` | ObjectId -> User | Да | Кто запустил обучение |

---

#### **ТИП И МЕСТО ОБУЧЕНИЯ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `type` | enum | Нет | `'local'` / `'cloud'` |
| `location.machineId` | string | Нет | ID компьютера (если локально) |
| `location.serverId` | string | Нет | ID сервера (если облачно) |

---

#### **ПАРАМЕТРЫ ОБУЧЕНИЯ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `config.epochs` | number | Нет | Количество эпох |
| `config.batchSize` | number | Нет | Размер батча |
| `config.learningRate` | number | Нет | Скорость обучения |
| `config.optimizer` | string | Нет | Оптимизатор |
| `config.lossFunction` | string | Нет | Функция потерь |
| `config.datasetId` | ObjectId -> Dataset | Нет | Используемый датасет |

---

#### **СТАТУС И ПРОГРЕСС**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да | `'queued'`, `'preparing'`, `'training'`, `'validating'`, `'completed'`, `'failed'`, `'cancelled'` |
| `progress.currentEpoch` | number | Нет | Текущая эпоха |
| `progress.totalEpochs` | number | Нет | Всего эпох |
| `progress.percentage` | number | Нет | Процент выполнения (0-100) |
| `progress.loss` | number | Нет | Текущее значение потерь |
| `progress.accuracy` | number | Нет | Текущая точность |
| `progress.validationLoss` | number | Нет | Потери на валидации |
| `progress.validationAccuracy` | number | Нет | Точность на валидации |

---

#### **РЕСУРСЫ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `resources.gpuUsage` | number | Нет | Использование GPU (%) |
| `resources.cpuUsage` | number | Нет | Использование CPU (%) |
| `resources.ramUsage` | number | Нет | Использование RAM (%) |
| `resources.temperature` | number | Нет | Температура GPU (°C) |

---

#### **ВРЕМЯ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `startedAt` | Date | Нет | Время начала |
| `completedAt` | Date | Нет | Время завершения |
| `estimatedTimeRemaining` | number | Нет | Осталось времени (сек) |

---

#### **ЛОГИ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `logs[].timestamp` | Date | Да | Время записи |
| `logs[].message` | string | Да | Текст сообщения |
| `logs[].level` | enum | Да | `'info'`, `'warning'`, `'error'`, `'debug'` |
| `logs[].data` | Mixed | Нет | Дополнительные данные |

---

#### **МЕТРИКИ И РЕЗУЛЬТАТ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `metrics.finalAccuracy` | number | Нет | Итоговая точность |
| `metrics.finalLoss` | number | Нет | Итоговые потери |
| `metrics.f1Score` | number | Нет | F1-мера |
| `metrics.precision` | number | Нет | Точность |
| `metrics.recall` | number | Нет | Полнота |
| `metrics.confusionMatrix` | [[number]] | Нет | Матрица ошибок |
| `result.modelVersion` | string | Нет | Версия обученной модели |
| `result.fileUrl` | string | Нет | Ссылка на веса (S3) |
| `result.size` | number | Нет | Размер файла |

---

### **3. СВЯЗИ С ДРУГИМИ МОДЕЛЯМИ**

```
TrainingProcess
  ├── creator → User
  ├── modelId → Model
  ├── agentId → Agent
  ├── config.datasetId → Dataset
  └── LocalAppSession (через currentTraining)
```

---

### **4. ИНДЕКСЫ**

```javascript
TrainingProcess.index({ creator: 1, status: 1 })
TrainingProcess.index({ modelId: 1, status: 1 })
TrainingProcess.index({ agentId: 1, status: 1 })
TrainingProcess.index({ status: 1, 'progress.percentage': 1 })
TrainingProcess.index({ startedAt: -1 })
```
