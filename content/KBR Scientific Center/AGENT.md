---
title: ИИ-Агент
---

## 🤖 **AGENT (ИИ-агент)**

---

### **1. СХЕМА AGENT**

```javascript
Agent {
  _id: ObjectId
  name: string, required
  description: string
  creator: ObjectId -> User, required
  
  // ========== КОМПЕТЕНЦИИ ==========
  competencies: [ObjectId -> Competency]
  
  // ========== ТРЕБОВАНИЯ К РЕСУРСАМ ==========
  requirements: {
    gpu: number
    cpu: number
    ram: number
    storage: number
  }
  
  // ========== СТОИМОСТЬ (МАРКЕТПЛЕЙС) ==========
  pricing: {
    pricePerCall: number
    subscriptionPrice: number
    commissionRate: number, default: 30
  }
  
  // ========== ВНЕШНОСТЬ (КАСТОМИЗАЦИЯ) ==========
  appearance: {
    // 2D режим
    '2d': {
      spriteUrl: string
      emotionSprites: {
        neutral: string
        happy: string
        sad: string
        angry: string
        thinking: string
      }
    }
    // 3D режим
    '3d': {
      modelUrl: string
      animations: {
        idle: string
        talking: string
        thinking: string
        working: string
        success: string
        error: string
        listening: string
      }
    }
    // Кастомизация (общая)
    customization: {
      bodyColor: string
      skinColor: string
      outfitType: string
      outfitColor: string
      hairStyle: string
      hairColor: string
      accessories: [string]
      specialElements: {
        projectorGlasses: boolean
        weldingMask: boolean
        clipboard: boolean
      }
    }
  }
  
  // ========== КОГНИТИВНЫЕ УЗЛЫ ==========
  cognitiveNodes: [{
    name: string, required
    type: enum ['input', 'processing', 'output', 'memory', 'decision']
    fileUrl: string
    config: Mixed
  }]
  
  // ========== МОДЕЛИ АГЕНТА ==========
  models: [{
    modelId: ObjectId -> Model
    version: string
    isActive: boolean, default: false
    publishedAt: Date
  }]
  
  activeModel: {
    modelId: ObjectId -> Model
    version: string
    activatedAt: Date
  }
  
  modelSource: {
    type: enum ['local', 'cloud', 'api']
    machineId: string
    serverId: string
  }
  
  // ========== ОБУЧЕНИЕ ==========
  trainingData: {
    files: [{
      name: string
      url: string
      type: string
    }]
    lastTrained: Date
  }
  
  // ========== СТАТИСТИКА ==========
  stats: {
    tasksCompleted: number, default: 0
    avgResponseTime: number
    successRate: number, default: 100
    rating: number, min:0, max:5
  }
  
  // ========== СТАТУС ==========
  status: enum ['draft', 'published', 'archived'], default: 'draft'
  version: string
  lastTrainedAt: Date
  
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
| `_id` | ObjectId | Да | Уникальный ID агента |
| `name` | string | Да | Название агента |
| `description` | string | Нет | Описание возможностей агента |
| `creator` | ObjectId -> User | Да | Создатель агента |

---

#### **КОМПЕТЕНЦИИ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `competencies` | [ObjectId -> Competency] | Нет | Список компетенций агента |

---

#### **ТРЕБОВАНИЯ К РЕСУРСАМ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `requirements.gpu` | number | Нет | Требуемая видеопамять (MB) |
| `requirements.cpu` | number | Нет | Требуемое количество ядер |
| `requirements.ram` | number | Нет | Требуемая оперативная память (MB) |
| `requirements.storage` | number | Нет | Требуемое место на диске (MB) |

---

#### **СТОИМОСТЬ (МАРКЕТПЛЕЙС)**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `pricing.pricePerCall` | number | Нет | Цена за один вызов |
| `pricing.subscriptionPrice` | number | Нет | Цена за подписку (месяц) |
| `pricing.commissionRate` | number | Да, default: 30 | Комиссия платформы (%) |

---

#### **ВНЕШНОСТЬ (КАСТОМИЗАЦИЯ)**

**2D режим:**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `appearance.2d.spriteUrl` | string | Нет | URL спрайта агента |
| `appearance.2d.emotionSprites.neutral` | string | Нет | Нейтральное выражение |
| `appearance.2d.emotionSprites.happy` | string | Нет | Счастливое выражение |
| `appearance.2d.emotionSprites.sad` | string | Нет | Грустное выражение |
| `appearance.2d.emotionSprites.angry` | string | Нет | Злое выражение |
| `appearance.2d.emotionSprites.thinking` | string | Нет | Задумчивое выражение |

**3D режим:**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `appearance.3d.modelUrl` | string | Нет | URL 3D модели |
| `appearance.3d.animations.idle` | string | Нет | Анимация ожидания |
| `appearance.3d.animations.talking` | string | Нет | Анимация разговора |
| `appearance.3d.animations.thinking` | string | Нет | Анимация размышления |
| `appearance.3d.animations.working` | string | Нет | Анимация работы |
| `appearance.3d.animations.success` | string | Нет | Анимация успеха |
| `appearance.3d.animations.error` | string | Нет | Анимация ошибки |
| `appearance.3d.animations.listening` | string | Нет | Анимация слушания |

**Кастомизация (общая):**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `appearance.customization.bodyColor` | string | Нет | Цвет тела |
| `appearance.customization.skinColor` | string | Нет | Цвет кожи |
| `appearance.customization.outfitType` | string | Нет | Тип одежды |
| `appearance.customization.outfitColor` | string | Нет | Цвет одежды |
| `appearance.customization.hairStyle` | string | Нет | Прическа |
| `appearance.customization.hairColor` | string | Нет | Цвет волос |
| `appearance.customization.accessories` | [string] | Нет | Аксессуары |
| `appearance.customization.specialElements` | object | Нет | Спецэлементы для профессий |

---

#### **КОГНИТИВНЫЕ УЗЛЫ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `cognitiveNodes[].name` | string | Да | Название узла |
| `cognitiveNodes[].type` | enum | Нет | Тип узла: `'input'`, `'processing'`, `'output'`, `'memory'`, `'decision'` |
| `cognitiveNodes[].fileUrl` | string | Нет | Ссылка на файл узла |
| `cognitiveNodes[].config` | Mixed | Нет | Конфигурация узла |

---

#### **МОДЕЛИ АГЕНТА**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `models[].modelId` | ObjectId -> Model | Да | Ссылка на модель |
| `models[].version` | string | Да | Версия модели |
| `models[].isActive` | boolean | Да, default: false | Активна ли модель |
| `models[].publishedAt` | Date | Нет | Дата публикации |
| `activeModel.modelId` | ObjectId -> Model | Нет | Активная модель |
| `activeModel.version` | string | Нет | Версия активной модели |
| `activeModel.activatedAt` | Date | Нет | Дата активации |
| `modelSource.type` | enum | Нет | Источник: `'local'`, `'cloud'`, `'api'` |
| `modelSource.machineId` | string | Нет | ID машины (локально) |
| `modelSource.serverId` | string | Нет | ID сервера (облачно) |

---

#### **ОБУЧЕНИЕ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `trainingData.files` | array | Нет | Файлы для обучения |
| `trainingData.files[].name` | string | Нет | Имя файла |
| `trainingData.files[].url` | string | Нет | URL файла |
| `trainingData.files[].type` | string | Нет | Тип файла |
| `trainingData.lastTrained` | Date | Нет | Дата последнего обучения |

---

#### **СТАТИСТИКА**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `stats.tasksCompleted` | number | Да, default: 0 | Выполнено задач |
| `stats.avgResponseTime` | number | Нет | Среднее время ответа (сек) |
| `stats.successRate` | number | Да, default: 100 | Процент успешных ответов |
| `stats.rating` | number | Нет | Рейтинг (0-5) |

---

#### **СТАТУС**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да, default: 'draft' | `'draft'` / `'published'` / `'archived'` |
| `version` | string | Нет | Версия агента |
| `lastTrainedAt` | Date | Нет | Дата последнего обучения |

---

#### **АУДИТ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `createdAt` | Date | Да | Дата создания |
| `updatedAt` | Date | Да | Дата обновления |

---

### **3. СВЯЗИ С ДРУГИМИ МОДЕЛЯМИ**

```
Agent
  ├── creator → User (создатель)
  ├── competencies → Competency (компетенции)
  ├── models.modelId → Model (модели)
  ├── activeModel.modelId → Model (активная модель)
  ├── Task (через массив в Task)
  ├── MarketplaceListing (через agentId)
  └── TrainingProcess (через agentId)
```

---

### **4. ИНДЕКСЫ**

```javascript
Agent.index({ creator: 1, status: 1 })
Agent.index({ 'models.modelId': 1 })
Agent.index({ name: 'text', description: 'text' })
Agent.index({ 'pricing.pricePerCall': 1, status: 1 })
Agent.index({ status: 1, 'stats.rating': -1 })
```

---

### **5. ВОЗМОЖНОСТИ АГЕНТА**

| Возможность      | Описание                                   |
| ---------------- | ------------------------------------------ |
| **Создание**     | Создатель может создать агента с нуля      |
| **Кастомизация** | Настройка внешности (2D/3D, цвета, одежда) |
| **Обучение**     | Привязка обученных моделей                 |
| **Публикация**   | Выставление в маркетплейс                  |
| **Статистика**   | Отслеживание производительности            |
| **Обновление**   | Обновление версий и моделей                |
