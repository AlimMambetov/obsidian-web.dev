---
title: Вычислитель
---
## 🖥️ **ВЫЧИСЛИТЕЛЬ (COMPUTER)**

---

### **Схема**

```javascript
Computer {
  _id: ObjectId
  name: string, required
  description: string
  creator: ObjectId -> User, required

  // ========== РЕСУРСЫ ==========
  resources: {
    gpu: number
    cpu: number
    ram: number
    ssd: number
    net: number
  }
  freeResources: {
    gpu: number
    cpu: number
    ram: number
    ssd: number
    net: number
  }

  // ========== РАСПОЛОЖЕНИЕ ==========
  location: {
    address: string
    coordinates: { lat: number, lng: number }
  }

  serverUrl: string

  // ========== СТОИМОСТЬ ==========
  pricing: {
    costPerHour: number
    costPerTask: number
  }

  // ========== ВИЗУАЛ (ГОТОВЫЙ ОТ НАС) ==========
  visual: {
    '2d': {
      iconUrl: string
    }
    '3d': {
      modelUrl: string
      animations: {
        idle: string
        working: string
        loading: string
        error: string
      }
    }
  }

  // ========== СТАТУС ==========
  status: enum ['available', 'busy', 'maintenance', 'offline'], default: 'available'

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
| `_id` | ObjectId | Да | Уникальный идентификатор вычислителя |
| `name` | string | Да | Название вычислителя |
| `description` | string | Нет | Описание |
| `creator` | ObjectId -> User | Да | Кто добавил вычислитель |
| `serverUrl` | string | Нет | Ссылка на сервер вычислителя |

---

#### **Ресурсы**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `resources.gpu` | number | Нет | Всего GPU (MB) |
| `resources.cpu` | number | Нет | Всего CPU (ядер) |
| `resources.ram` | number | Нет | Всего RAM (MB) |
| `resources.ssd` | number | Нет | Всего SSD (MB) |
| `resources.net` | number | Нет | Скорость сети (Mbps) |
| `freeResources.gpu` | number | Нет | Свободно GPU |
| `freeResources.cpu` | number | Нет | Свободно CPU |
| `freeResources.ram` | number | Нет | Свободно RAM |
| `freeResources.ssd` | number | Нет | Свободно SSD |
| `freeResources.net` | number | Нет | Свободно сети |

---

#### **Расположение**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `location.address` | string | Нет | Адрес |
| `location.coordinates.lat` | number | Нет | Широта |
| `location.coordinates.lng` | number | Нет | Долгота |

---

#### **Стоимость**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `pricing.costPerHour` | number | Нет | Цена за час |
| `pricing.costPerTask` | number | Нет | Цена за задачу |

---

#### **Визуал**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `visual.2d.iconUrl` | string | Нет | Иконка сервера |
| `visual.3d.modelUrl` | string | Нет | 3D модель сервера |
| `visual.3d.animations.idle` | string | Нет | Анимация ожидания |
| `visual.3d.animations.working` | string | Нет | Анимация работы |
| `visual.3d.animations.loading` | string | Нет | Анимация загрузки |
| `visual.3d.animations.error` | string | Нет | Анимация ошибки |

---

#### **Статус**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да, default: 'available' | `'available'`, `'busy'`, `'maintenance'`, `'offline'` |

---

### **Связи с другими моделями**

```
Computer
  ├── creator → User
  ├── Task (через массив в Task)
  └── agentComputerConnections → Task
```

---

### **Индексы**

```javascript
Computer.index({ status: 1, 'resources.gpu': 1 })
Computer.index({ creator: 1 })
Computer.index({ status: 1, 'freeResources.gpu': -1 })
```
