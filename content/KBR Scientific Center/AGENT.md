---
title: ИИ-Агент
---
## 🤖 **АГЕНТ (AGENT)**

---

### **Схема**

```javascript
Agent {
  _id: ObjectId
  name: string, required
  description: string
  creator: ObjectId -> User, required

  // ========== КОМПЕТЕНЦИИ ==========
  competencies: [ObjectId -> Competency]

  url: string

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

### **Описание полей**

---

#### **Основные поля**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `_id` | ObjectId | Да | Уникальный идентификатор агента |
| `name` | string | Да | Название агента |
| `description` | string | Нет | Описание возможностей агента |
| `creator` | ObjectId -> User | Да | Создатель агента |
| `url` | string | Нет | Ссылка на самого агента (API endpoint) |

---

#### **Компетенции**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `competencies` | [ObjectId -> Competency] | Нет | Список компетенций агента |

---

#### **Требования к ресурсам**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `requirements.gpu` | number | Нет | Требуемая видеопамять (MB) |
| `requirements.cpu` | number | Нет | Требуемое количество ядер |
| `requirements.ram` | number | Нет | Требуемая оперативная память (MB) |
| `requirements.storage` | number | Нет | Требуемое место на диске (MB) |

---

#### **Стоимость (маркетплейс)**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `pricing.pricePerCall` | number | Нет | Цена за один вызов |
| `pricing.subscriptionPrice` | number | Нет | Цена за подписку (месяц) |
| `pricing.commissionRate` | number | Да, default: 30 | Комиссия платформы (%) |

---

#### **Внешность (кастомизация)**

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

#### **Когнитивные узлы**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `cognitiveNodes[].name` | string | Да | Название узла |
| `cognitiveNodes[].type` | enum | Нет | `'input'`, `'processing'`, `'output'`, `'memory'`, `'decision'` |
| `cognitiveNodes[].fileUrl` | string | Нет | Ссылка на файл узла |
| `cognitiveNodes[].config` | Mixed | Нет | Конфигурация узла |

---

#### **Обучение**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `trainingData.files` | array | Нет | Файлы для обучения |
| `trainingData.files[].name` | string | Нет | Имя файла |
| `trainingData.files[].url` | string | Нет | URL файла |
| `trainingData.files[].type` | string | Нет | Тип файла |
| `trainingData.lastTrained` | Date | Нет | Дата последнего обучения |

---

#### **Статистика**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `stats.tasksCompleted` | number | Да, default: 0 | Выполнено задач |
| `stats.avgResponseTime` | number | Нет | Среднее время ответа (сек) |
| `stats.successRate` | number | Да, default: 100 | Процент успешных ответов |
| `stats.rating` | number | Нет | Рейтинг (0-5) |

---

#### **Статус**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да, default: 'draft' | `'draft'`, `'published'`, `'archived'` |
| `version` | string | Нет | Версия агента |
| `lastTrainedAt` | Date | Нет | Дата последнего обучения |

---

#### **Аудит**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `createdAt` | Date | Да | Дата создания |
| `updatedAt` | Date | Да | Дата обновления |

---

### **Связи с другими моделями**

```
Agent
  ├── creator → User
  ├── competencies → Competency
  ├── Task (через массив в Task)
  ├── MarketplaceListing (через agentId)
  └── agentDeviceConnections → Task
  └── agentComputerConnections → Task
```

---

### **Индексы**

```javascript
Agent.index({ creator: 1, status: 1 })
Agent.index({ name: 'text', description: 'text' })
Agent.index({ 'pricing.pricePerCall': 1, status: 1 })
Agent.index({ status: 1, 'stats.rating': -1 })
```
