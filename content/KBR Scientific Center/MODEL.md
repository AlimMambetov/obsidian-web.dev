---
title: Модель
---

## 🧠 **MODEL (ML модель для обучения)**

---

### **1. СХЕМА MODEL**

```javascript
Model {
  _id: ObjectId
  name: string, required
  description: string
  creator: ObjectId -> User, required
  agentId: ObjectId -> Agent
  
  // ========== ТИП МОДЕЛИ ==========
  type: enum ['llm', 'vision', 'audio', 'recommendation', 'regression', 'classification', 'generative', 'custom']
  
  // ========== АРХИТЕКТУРА ==========
  architecture: {
    framework: enum ['pytorch', 'tensorflow', 'onnx', 'custom']
    config: Mixed
    size: number (MB)
    parameters: number
  }
  
  // ========== ДАТАСЕТ ==========
  dataset: {
    name: string
    size: number (samples)
    type: string
    url: string
    version: string
    lastUpdated: Date
  }
  
  // ========== ВЕРСИИ ==========
  versions: [{
    version: string, required
    fileUrl: string
    size: number
    metrics: {
      accuracy: number
      loss: number
      f1Score: number
      trainingTime: number (hours)
    }
    status: enum ['draft', 'training', 'trained', 'published', 'archived']
    createdAt: Date
  }]
  
  currentVersion: string
  
  // ========== СТАТУС ==========
  status: enum ['draft', 'training', 'ready', 'published', 'archived'], default: 'draft'
  
  // ========== КОНФИГ ОБУЧЕНИЯ ==========
  trainingConfig: {
    epochs: number
    batchSize: number
    learningRate: number
    optimizer: string
    lossFunction: string
    earlyStopping: boolean
    validationSplit: number
  }
  
  // ========== ЛОКАЛЬНОЕ ОБУЧЕНИЕ ==========
  localTraining: {
    enabled: boolean, default: false
    machineId: string
    status: enum ['idle', 'training', 'completed', 'failed']
    progress: number
    startedAt: Date
    completedAt: Date
    logs: [{
      timestamp: Date
      message: string
      level: string
    }]
  }
  
  // ========== БЭКАПЫ ==========
  backups: [{
    version: string
    fileUrl: string
    createdAt: Date
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
| `_id` | ObjectId | Да | Уникальный ID модели |
| `name` | string | Да | Название модели |
| `description` | string | Нет | Описание модели |
| `creator` | ObjectId -> User | Да | Создатель модели |
| `agentId` | ObjectId -> Agent | Нет | К какому агенту привязана |

---

#### **ТИП МОДЕЛИ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `type` | enum | Нет | Тип модели: `'llm'`, `'vision'`, `'audio'`, `'recommendation'`, `'regression'`, `'classification'`, `'generative'`, `'custom'` |

---

#### **АРХИТЕКТУРА**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `architecture.framework` | enum | Нет | Фреймворк: `'pytorch'`, `'tensorflow'`, `'onnx'`, `'custom'` |
| `architecture.config` | Mixed | Нет | Конфигурация модели (слои, параметры) |
| `architecture.size` | number | Нет | Размер модели в MB |
| `architecture.parameters` | number | Нет | Количество параметров |

---

#### **ДАТАСЕТ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `dataset.name` | string | Нет | Название датасета |
| `dataset.size` | number | Нет | Количество сэмплов |
| `dataset.type` | string | Нет | Тип данных (текст, изображения, аудио) |
| `dataset.url` | string | Нет | Ссылка на датасет (S3) |
| `dataset.version` | string | Нет | Версия датасета |
| `dataset.lastUpdated` | Date | Нет | Дата обновления |

---

#### **ВЕРСИИ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `versions[].version` | string | Да | Номер версии |
| `versions[].fileUrl` | string | Да | Ссылка на веса модели (S3) |
| `versions[].size` | number | Нет | Размер файла |
| `versions[].metrics.accuracy` | number | Нет | Точность |
| `versions[].metrics.loss` | number | Нет | Потери |
| `versions[].metrics.f1Score` | number | Нет | F1-мера |
| `versions[].metrics.trainingTime` | number | Нет | Время обучения (часы) |
| `versions[].status` | enum | Да | `'draft'`, `'training'`, `'trained'`, `'published'`, `'archived'` |
| `versions[].createdAt` | Date | Да | Дата создания версии |
| `currentVersion` | string | Нет | Текущая активная версия |

---

#### **СТАТУС**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да, default: 'draft' | `'draft'` / `'training'` / `'ready'` / `'published'` / `'archived'` |

---

#### **КОНФИГ ОБУЧЕНИЯ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `trainingConfig.epochs` | number | Нет | Количество эпох |
| `trainingConfig.batchSize` | number | Нет | Размер батча |
| `trainingConfig.learningRate` | number | Нет | Скорость обучения |
| `trainingConfig.optimizer` | string | Нет | Оптимизатор (Adam, SGD) |
| `trainingConfig.lossFunction` | string | Нет | Функция потерь |
| `trainingConfig.earlyStopping` | boolean | Нет | Ранняя остановка |
| `trainingConfig.validationSplit` | number | Нет | Доля валидации (0-1) |

---

#### **ЛОКАЛЬНОЕ ОБУЧЕНИЕ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `localTraining.enabled` | boolean | Да, default: false | Включено ли локальное обучение |
| `localTraining.machineId` | string | Нет | ID компьютера |
| `localTraining.status` | enum | Нет | `'idle'`, `'training'`, `'completed'`, `'failed'` |
| `localTraining.progress` | number | Нет | Прогресс (0-100) |
| `localTraining.startedAt` | Date | Нет | Дата начала |
| `localTraining.completedAt` | Date | Нет | Дата завершения |
| `localTraining.logs` | array | Нет | Логи обучения |

---

#### **БЭКАПЫ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `backups[].version` | string | Нет | Версия бэкапа |
| `backups[].fileUrl` | string | Нет | Ссылка на файл бэкапа (S3) |
| `backups[].createdAt` | Date | Нет | Дата создания |

---

#### **АУДИТ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `createdAt` | Date | Да | Дата создания |
| `updatedAt` | Date | Да | Дата обновления |

---

### **3. СВЯЗИ С ДРУГИМИ МОДЕЛЯМИ**

```
Model
  ├── creator → User (создатель)
  ├── agentId → Agent (к какому агенту привязана)
  ├── TrainingProcess (через modelId)
  └── Agent.models (через массив моделей в Agent)
```

---

### **4. ИНДЕКСЫ**

```javascript
Model.index({ creator: 1, status: 1 })
Model.index({ agentId: 1, status: 1 })
Model.index({ type: 1, status: 1 })
Model.index({ 'versions.status': 1 })
Model.index({ name: 'text', description: 'text' })
```

---

### **5. ЖИЗНЕННЫЙ ЦИКЛ МОДЕЛИ**

```
1. СОЗДАНИЕ
   Создатель → создает модель (status: 'draft')
            ↓
2. НАСТРОЙКА
   → Загружает датасет
   → Настраивает архитектуру
   → Настраивает trainingConfig
            ↓
3. ОБУЧЕНИЕ
   → Запускает обучение через локальное приложение
   → localTraining.status: 'training'
   → Отслеживает прогресс
            ↓
4. ЗАВЕРШЕНИЕ
   → Обучение завершено → status: 'ready'
   → Создается версия (versions[].status: 'trained')
   → Веса модели сохраняются в S3
            ↓
5. ПУБЛИКАЦИЯ
   → Модель привязывается к агенту
   → Agent.models добавляется запись
            ↓
6. АРХИВАЦИЯ
   → status: 'archived' (если не используется)
```

---

### **6. КЛЮЧЕВЫЕ ОСОБЕННОСТИ**

| Особенность | Описание |
|-------------|----------|
| **Версионирование** | Каждая обученная модель сохраняется как отдельная версия |
| **Локальное обучение** | Обучение на своем ПК через локальное приложение |
| **Метрики** | Сохраняются accuracy, loss, f1Score для каждой версии |
| **Бэкапы** | Автоматическое сохранение бэкапов моделей |
| **Привязка к агенту** | Модель можно привязать к конкретному агенту |
