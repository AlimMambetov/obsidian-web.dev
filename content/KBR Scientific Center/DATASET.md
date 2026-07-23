---
title: Датасет
---

## 📊 **DATASET (Датасет для обучения)**

---

### **1. СХЕМА DATASET**

```javascript
Dataset {
  _id: ObjectId
  name: string, required
  description: string
  creator: ObjectId -> User, required
  
  // ========== ТИП ДАННЫХ ==========
  type: enum ['text', 'image', 'audio', 'video', 'tabular', '3d_model', 'custom']
  
  // ========== РАЗМЕР ==========
  size: {
    samples: number
    totalSize: number (MB)
    compressedSize: number
  }
  
  // ========== ИСТОЧНИК ==========
  source: {
    type: enum ['uploaded', 'generated', 'external']
    url: string
    license: string
  }
  
  // ========== ФОРМАТ ==========
  format: {
    fileType: string
    schema: Mixed
    labels: [string]
  }
  
  // ========== ФАЙЛЫ ==========
  files: [{
    name: string
    url: string (S3)
    size: number
    type: string
    checksum: string
  }]
  
  // ========== СПЛИТЫ ==========
  splits: {
    trainRatio: number, default: 0.7
    valRatio: number, default: 0.15
    testRatio: number, default: 0.15
  }
  
  // ========== СТАТУС ==========
  status: enum ['uploading', 'processing', 'ready', 'archived']
  
  // ========== ВЕРСИИ ==========
  versions: [{
    version: string
    url: string
    size: number
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
| `_id` | ObjectId | Да | Уникальный ID датасета |
| `name` | string | Да | Название датасета |
| `description` | string | Нет | Описание датасета |
| `creator` | ObjectId -> User | Да | Создатель датасета |

---

#### **ТИП ДАННЫХ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `type` | enum | Нет | `'text'`, `'image'`, `'audio'`, `'video'`, `'tabular'`, `'3d_model'`, `'custom'` |

---

#### **РАЗМЕР**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `size.samples` | number | Нет | Количество сэмплов |
| `size.totalSize` | number | Нет | Общий размер (MB) |
| `size.compressedSize` | number | Нет | Сжатый размер (MB) |

---

#### **ИСТОЧНИК**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `source.type` | enum | Нет | `'uploaded'`, `'generated'`, `'external'` |
| `source.url` | string | Нет | Ссылка на источник |
| `source.license` | string | Нет | Лицензия датасета |

---

#### **ФОРМАТ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `format.fileType` | string | Нет | Тип файлов (csv, json, images) |
| `format.schema` | Mixed | Нет | Схема данных (структура) |
| `format.labels` | [string] | Нет | Список меток классов |

---

#### **ФАЙЛЫ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `files[].name` | string | Нет | Имя файла |
| `files[].url` | string | Нет | Ссылка на файл (S3) |
| `files[].size` | number | Нет | Размер файла (MB) |
| `files[].type` | string | Нет | MIME-тип |
| `files[].checksum` | string | Нет | Контрольная сумма |

---

#### **СПЛИТЫ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `splits.trainRatio` | number | Да, default: 0.7 | Доля обучающей выборки |
| `splits.valRatio` | number | Да, default: 0.15 | Доля валидационной выборки |
| `splits.testRatio` | number | Да, default: 0.15 | Доля тестовой выборки |

---

#### **СТАТУС**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `status` | enum | Да | `'uploading'`, `'processing'`, `'ready'`, `'archived'` |

---

#### **ВЕРСИИ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `versions[].version` | string | Нет | Номер версии |
| `versions[].url` | string | Нет | Ссылка на файл версии |
| `versions[].size` | number | Нет | Размер версии |
| `versions[].createdAt` | Date | Нет | Дата создания версии |

---

### **3. СВЯЗИ С ДРУГИМИ МОДЕЛЯМИ**

```
Dataset
  ├── creator → User
  ├── Model (через dataset в Model)
  └── TrainingProcess (через datasetId в config)
```

---

### **4. ИНДЕКСЫ**

```javascript
Dataset.index({ creator: 1, status: 1 })
Dataset.index({ type: 1, status: 1 })
Dataset.index({ name: 'text', description: 'text' })
Dataset.index({ 'size.samples': -1 })
```
