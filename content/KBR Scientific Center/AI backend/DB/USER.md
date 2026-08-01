---
title: ПОЛЬЗОВАТЕЛЬ
---
## 👤 **ПОЛЬЗОВАТЕЛЬ (USER)**

---

### **Схема**

```javascript
User {
  _id: ObjectId
  fullName: string, required
  email: string, required, unique
  passwordHash: string, required
  phone: string, unique, sparse

  // ========== ЛИЧНЫЕ ДАННЫЕ ==========
  location: string
  organization: string
  position: string
  desiredSalary: number

  computerUrl: string

  // ========== ВНЕШНОСТЬ (кастомизация) ==========
  appearance: {
    model: string
    hair: string
    skinColor: string
    headwear: string
    accessory: string
  }

  photo: string (URL)

  // ========== ВЕРИФИКАЦИЯ ==========
  isEmailVerified: boolean, default: false
  isPhoneVerified: boolean, default: false
  verificationToken: string
  verificationTokenExpires: Date

  // ========== ФИНАНСЫ ==========
  balance: number, default: 0
  totalEarned: number, default: 0

  // ========== РОЛЬ ==========
  role: enum ['user', 'creator', 'admin'], default: 'user'

  // ========== ПРОФИЛЬ СОЗДАТЕЛЯ ==========
  creatorProfile: {
    verificationStatus: enum ['pending', 'verified', 'rejected'], default: 'pending'
    verifiedAt: Date
    localApp: {
      installed: boolean, default: false
      version: string
      lastActive: Date
      machineId: string
    }
    totalModelsCreated: number, default: 0
    totalAgentsPublished: number, default: 0
    totalEarnings: number, default: 0
    rating: number, default: 0
    reviewsCount: number, default: 0
  }

  // ========== НАСТРОЙКИ ==========
  settings: {
    displayMode: enum ['auto', '2d', '3d'], default: 'auto'
    quality: enum ['low', 'medium', 'high'], default: 'medium'
    developerMode: boolean, default: false
  }

  // ========== СВЯЗИ ==========
  competencies: [ObjectId -> Competency]

  // ========== АУДИТ ==========
  createdAt: Date
  updatedAt: Date
  deletedAt: Date
}
```

---

### **Описание полей**

---

#### **Основные поля**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `_id` | ObjectId | Да | Уникальный идентификатор пользователя |
| `fullName` | string | Да | Полное имя пользователя |
| `email` | string | Да, unique | Email для входа и уведомлений |
| `passwordHash` | string | Да | Хеш пароля (bcrypt) |
| `phone` | string | Нет, unique, sparse | Номер телефона (для 2FA) |

---

#### **Личные данные**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `location` | string | Нет | Город/место жительства |
| `organization` | string | Нет | Название организации/компании |
| `position` | string | Нет | Должность |
| `desiredSalary` | number | Нет | Желаемая оплата труда |
| `computerUrl` | string | Нет | Ссылка на компьютер пользователя (может быть динамический IP) |

---

#### **Внешность (кастомизация)**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `appearance.model` | string | Нет | Выбор 3D модели |
| `appearance.hair` | string | Нет | Прическа |
| `appearance.skinColor` | string | Нет | Цвет кожи |
| `appearance.headwear` | string | Нет | Головной убор |
| `appearance.accessory` | string | Нет | Аксессуар |
| `photo` | string (URL) | Нет | Фото/аватар |

---

#### **Верификация**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `isEmailVerified` | boolean | Да, default: false | Подтвержден ли email |
| `isPhoneVerified` | boolean | Да, default: false | Подтвержден ли телефон |
| `verificationToken` | string | Нет | Токен для подтверждения email (удаляется после) |
| `verificationTokenExpires` | Date | Нет | Срок действия токена (24 часа) |

---

#### **Финансы**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `balance` | number | Да, default: 0 | Текущий баланс пользователя |
| `totalEarned` | number | Да, default: 0 | Всего заработано за всё время |

---

#### **Роль**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `role` | enum | Да, default: 'user' | `'user'` / `'creator'` / `'admin'` |

---

#### **Профиль создателя (только для role='creator')**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `creatorProfile.verificationStatus` | enum | Да, default: 'pending' | `'pending'` / `'verified'` / `'rejected'` |
| `creatorProfile.verifiedAt` | Date | Нет | Дата прохождения верификации |
| `creatorProfile.localApp.installed` | boolean | Да, default: false | Установлено ли локальное приложение |
| `creatorProfile.localApp.version` | string | Нет | Версия установленного приложения |
| `creatorProfile.localApp.lastActive` | Date | Нет | Дата последней активности |
| `creatorProfile.localApp.machineId` | string | Нет | Уникальный ID компьютера |
| `creatorProfile.totalModelsCreated` | number | Да, default: 0 | Количество созданных моделей |
| `creatorProfile.totalAgentsPublished` | number | Да, default: 0 | Количество опубликованных агентов |
| `creatorProfile.totalEarnings` | number | Да, default: 0 | Заработано от продаж агентов |
| `creatorProfile.rating` | number | Да, default: 0 | Рейтинг создателя (0-5) |
| `creatorProfile.reviewsCount` | number | Да, default: 0 | Количество отзывов |

---

#### **Настройки**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `settings.displayMode` | enum | Да, default: 'auto' | `'auto'` / `'2d'` / `'3d'` |
| `settings.quality` | enum | Да, default: 'medium' | `'low'` / `'medium'` / `'high'` |
| `settings.developerMode` | boolean | Да, default: false | Режим разработчика (логи, отладка) |

---

#### **Связи**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `competencies` | [ObjectId] | Нет | Ссылки на компетенции пользователя |

---

#### **Аудит**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `createdAt` | Date | Да | Дата создания аккаунта |
| `updatedAt` | Date | Да | Дата последнего обновления |
| `deletedAt` | Date | Нет | Дата удаления (soft delete) |

---

### **Связи с другими моделями**

```
User
  ├── competencies → Competency
  ├── Agent (как creator)
  ├── Computer (как creator)
  ├── Device (как creator)
  ├── Task (как creator и участник)
  ├── MarketplaceListing (как sellerId)
  ├── Transaction (как userId)
  └── Notification (как userId)
```

---

### **Индексы**

```javascript
User.index({ email: 1 }, { unique: true })
User.index({ role: 1 })
User.index({ 'creatorProfile.verificationStatus': 1 })
User.index({ fullName: 'text', email: 'text' })
```
