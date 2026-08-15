---
title: Робот
---
## 🛠️ **РОБОТ/УСТРОЙСТВО (DEVICE)**

---

### **Схема**

```javascript
Device {
  _id: ObjectId
  name: string, required
  description: string
  creator: ObjectId -> User, required

  // ========== КОМПЕТЕНЦИИ ==========
  competencies: [ObjectId -> Competency]

  // ========== ТИП ==========
  type: enum ['3d_printer_plastic', '3d_printer_metal', 'welder', 'miller', 'cnc']

  // ========== РАСПОЛОЖЕНИЕ ==========
  location: {
    address: string
    coordinates: { lat: number, lng: number }
  }

  ipAddress: string

  // ========== ПРОТОКОЛ ==========
  protocol: enum ['gcode', 'modbus', 'opcua', 'custom'], default: 'gcode'
  protocolConfig: Mixed

  // ========== СТОИМОСТЬ ==========
  pricing: {
    costPerHour: number
    costPerGram: number
  }

  // ========== УЗЛЫ ==========
  nodes: [{
    name: string, required
    url: string
    type: enum ['joint', 'actuator', 'sensor', 'controller', 'tool']
    location: { x: number, y: number, z: number }
    dimensions: { width: number, height: number, depth: number }
    orientation: { pitch: number, yaw: number, roll: number }
    parentNode: string
    modelUrl: string
    fileUrl: string
  }]

  // ========== ХАРАКТЕРИСТИКИ ==========
  specs: {
    maxSize: string
    precision: string
    materials: [string]
    power: number
    temperature: number
    speed: number
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
        printing: string
        welding: string
        error: string
        maintenance: string
      }
    }
  }

  // ========== СТАТУС ==========
  status: enum ['idle', 'busy', 'maintenance', 'offline'], default: 'idle'
  currentTask: ObjectId -> Task

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
| `_id` | ObjectId | Да | Уникальный идентификатор устройства |
| `name` | string | Да | Название устройства |
| `description` | string | Нет | Описание |
| `creator` | ObjectId -> User | Да | Кто добавил устройство |
| `ipAddress` | string | Нет | IP адрес робота |

---

#### **Компетенции и тип**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `competencies` | [ObjectId -> Competency] | Нет | Компетенции устройства |
| `type` | enum | Нет | `'3d_printer_plastic'`, `'3d_printer_metal'`, `'welder'`, `'miller'`, `'cnc'` |

---

#### **Расположение**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `location.address` | string | Нет | Адрес |
| `location.coordinates.lat` | number | Нет | Широта |
| `location.coordinates.lng` | number | Нет | Долгота |

---

#### **Протокол**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `protocol` | enum | Да, default: 'gcode' | `'gcode'`, `'modbus'`, `'opcua'`, `'custom'` |
| `protocolConfig` | Mixed | Нет | Конфигурация протокола |

---

#### **Стоимость**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `pricing.costPerHour` | number | Нет | Цена за час работы |
| `pricing.costPerGram` | number | Нет | Цена за грамм (для 3D печати) |

---

#### **Узлы**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `nodes[].name` | string | Да | Название узла |
| `nodes[].url` | string | Нет | Ссылка на конкретный узел прибора |
| `nodes[].type` | enum | Нет | `'joint'`, `'actuator'`, `'sensor'`, `'controller'`, `'tool'` |
| `nodes[].location` | object | Нет | Координаты: x, y, z |
| `nodes[].dimensions` | object | Нет | Размеры: width, height, depth |
| `nodes[].orientation` | object | Нет | Ориентация: pitch, yaw, roll |
| `nodes[].parentNode` | string | Нет | Ссылка на родительский узел |
| `nodes[].modelUrl` | string | Нет | 3D модель узла |
| `nodes[].fileUrl` | string | Нет | Ссылка на файл узла |

---

#### **Характеристики**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `specs.maxSize` | string | Нет | Максимальный размер детали |
| `specs.precision` | string | Нет | Точность |
| `specs.materials` | [string] | Нет | Поддерживаемые материалы |
| `specs.power` | number | Нет | Мощность (кВт) |
| `specs.temperature` | number | Нет | Рабочая температура |
| `specs.speed` | number | Нет | Скорость работы |

---

#### **Визуал**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `visual.2d.iconUrl` | string | Нет | Иконка устройства |
| `visual.3d.modelUrl` | string | Нет | 3D модель |
| `visual.3d.animations.idle` | string | Нет | Ожидание |
| `visual.3d.animations.working` | string | Нет | Работа |
| `visual.3d.animations.printing` | string | Нет | Печать |
| `visual.3d.animations.welding` | string | Нет | Сварка |
| `visual.3d.animations.error` | string | Нет | Ошибка |
| `visual.3d.animations.maintenance` | string | Нет | Обслуживание |

---

#### **Статус**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да, default: 'idle' | `'idle'`, `'busy'`, `'maintenance'`, `'offline'` |
| `currentTask` | ObjectId -> Task | Нет | Текущее задание |

---

### **Связи с другими моделями**

```
Device
  ├── creator → User
  ├── competencies → Competency
  ├── currentTask → Task
  ├── Task (через массив в Task)
  └── agentDeviceConnections → Task
```

---

### **Индексы**

```javascript
Device.index({ status: 1, type: 1 })
Device.index({ competencies: 1 })
Device.index({ creator: 1 })
Device.index({ 'location.coordinates': '2dsphere' })
```