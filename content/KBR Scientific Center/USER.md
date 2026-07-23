---
title: ПОЛЬЗОВАТЕЛЬ
---

# 👤 **ПОЛЬЗОВАТЕЛЬ (USER)**

---

## 📋 **СХЕМА USER**

```js
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

## 📊 **ОПИСАНИЕ ВСЕХ ПОЛЕЙ**

---

### **1. ОСНОВНЫЕ ПОЛЯ**

| Поле           | Тип      | Обязательное        | Описание                      |
| -------------- | -------- | ------------------- | ----------------------------- |
| `_id`          | ObjectId | Да                  | Уникальный ID пользователя    |
| `fullName`     | string   | Да                  | ФИО пользователя              |
| `email`        | string   | Да, unique          | Email для входа и уведомлений |
| `passwordHash` | string   | Да                  | Хеш пароля (bcrypt)           |
| `phone`        | string   | Нет, unique, sparse | Номер телефона (для 2FA)      |

---

### **2. ЛИЧНЫЕ ДАННЫЕ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `location` | string | Нет | Город/место жительства |
| `organization` | string | Нет | Название организации/компании |
| `position` | string | Нет | Должность |
| `desiredSalary` | number | Нет | Желаемая оплата труда |

---

### **3. ВНЕШНОСТЬ (кастомизация аватара)**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `appearance.model` | string | Нет | Выбор 3D модели персонажа |
| `appearance.hair` | string | Нет | Прическа |
| `appearance.skinColor` | string | Нет | Цвет кожи |
| `appearance.headwear` | string | Нет | Головной убор |
| `appearance.accessory` | string | Нет | Аксессуар |
| `photo` | string (URL) | Нет | Фото/аватар (реальное фото) |

**Где отображается аватар пользователя:**
- Личный кабинет
- Чат с агентом
- Карточка пользователя
- Проекты (список участников)

---

### **4. ВЕРИФИКАЦИЯ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `isEmailVerified` | boolean | Да, default: false | Подтвержден ли email |
| `isPhoneVerified` | boolean | Да, default: false | Подтвержден ли телефон |
| `verificationToken` | string | Нет | Токен для подтверждения email (удаляется после) |
| `verificationTokenExpires` | Date | Нет | Срок жизни токена (24 часа) |

---

### **5. ФИНАНСЫ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `balance` | number | Да, default: 0 | Текущий баланс пользователя |
| `totalEarned` | number | Да, default: 0 | Всего заработано за всё время |

---

### **6. РОЛЬ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `role` | enum | Да, default: 'user' | `'user'` / `'creator'` / `'admin'` |

---

### **7. ПРОФИЛЬ СОЗДАТЕЛЯ (только для role='creator')**

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

### **8. НАСТРОЙКИ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `settings.displayMode` | enum | Да, default: 'auto' | `'auto'` / `'2d'` / `'3d'` |
| `settings.quality` | enum | Да, default: 'medium' | `'low'` / `'medium'` / `'high'` |
| `settings.developerMode` | boolean | Да, default: false | Режим разработчика (логи, отладка) |

---

### **9. СВЯЗИ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `competencies` | [ObjectId] | Нет | Ссылки на компетенции пользователя |

---

### **10. АУДИТ**

| Поле | Тип | Обязательное | Описание |
|------|-----|--------------|----------|
| `createdAt` | Date | Да | Дата создания аккаунта |
| `updatedAt` | Date | Да | Дата последнего обновления |
| `deletedAt` | Date | Нет | Дата удаления (soft delete) |

---

# 🔐 **АВТОРИЗАЦИЯ И АУТЕНТИФИКАЦИЯ**

---

## 🚀 **ПРОЦЕСС РЕГИСТРАЦИИ И ВХОДА**

---

### **1. РЕГИСТРАЦИЯ**

**Что делает пользователь:**
- Вводит email, пароль, имя
- Нажимает "Зарегистрироваться"

**Что делает бэк:**
1. Проверяет email (не занят)
2. Хеширует пароль (bcrypt)
3. Создает пользователя в БД:
   - `isEmailVerified: false`
   - `verificationToken: uuid`
   - `verificationTokenExpires: Date + 24h`
4. Отправляет письмо со ссылкой: `site.com/verify?token=uuid`
5. Возвращает: "Проверьте почту"

---

### **2. ПОДТВЕРЖДЕНИЕ EMAIL**

**Что делает пользователь:**
- Переходит по ссылке из письма

**Что делает бэк:**
1. Находит пользователя по `verificationToken`
2. Проверяет не истек ли токен
3. Обновляет:
   - `isEmailVerified: true`
   - `verificationToken: null`
   - `verificationTokenExpires: null`
4. Возвращает: "Email подтвержден"

---

### **3. ВХОД (LOGIN)**

**Что делает пользователь:**
- Вводит email и пароль
- Нажимает "Войти"

**Что делает бэк:**
1. Находит пользователя по email
2. Проверяет пароль (bcrypt.compare)
3. Проверяет `isEmailVerified: true`
4. Генерирует **JWT токен** (на 1 час)
5. Генерирует **Refresh токен** (на 7 дней) → в Redis
6. Возвращает: `{ accessToken, refreshToken, user }`

---

### **4. ОБНОВЛЕНИЕ ТОКЕНА (REFRESH)**

**Когда нужно:**
- JWT истек (через час)

**Что делает бэк:**
1. Принимает Refresh Token
2. Проверяет в Redis
3. Генерирует новый JWT
4. Возвращает: `{ accessToken }`

---

### **5. ВЫХОД (LOGOUT)**

**Что делает бэк:**
1. Добавляет JWT в черный список (Redis)
2. Удаляет Refresh Token из Redis
3. Возвращает: "Выход выполнен"

---

## 📊 **СХЕМА РАБОТЫ**

```
1. РЕГИСТРАЦИЯ
   User → email, пароль, имя
   Бэк → создает User (не подтвержден) + отправляет письмо

2. ПОДТВЕРЖДЕНИЕ
   User → переходит по ссылке
   Бэк → isEmailVerified: true

3. ВХОД
   User → email, пароль
   Бэк → JWT (1 час) + Refresh Token (7 дней)

4. ЗАПРОСЫ
   User → JWT в заголовке
   Бэк → проверяет JWT → пропускает

5. ОБНОВЛЕНИЕ
   User → Refresh Token
   Бэк → новый JWT

6. ВЫХОД
   User → выход
   Бэк → удаляет токены
```

---

## 🔐 **ГДЕ ЧТО ХРАНИТСЯ**

| Что | Где |
|-----|-----|
| Пользователь | MongoDB (`User`) |
| JWT токен | В памяти фронта (localStorage) |
| Refresh токен | В памяти фронта + Redis |
| Черный список JWT | Redis |
| Verification токен | MongoDB (временное поле, удаляется) |

---

# 👨‍💻 **ЛОГИКА РАБОТЫ С СОЗДАТЕЛЕМ**

---

## 📌 **НАЧАЛО: пользователь зарегистрирован**

```javascript
// После регистрации и подтверждения email
User {
  fullName: 'Иван Петров',
  email: 'ivan@mail.com',
  isEmailVerified: true,
  role: 'user',                // ← пока обычный пользователь
  creatorProfile: null
}
```

---

## 🔄 **КАК СТАТЬ СОЗДАТЕЛЕМ (4 шага)**

---

### **Шаг 1. Подача заявки**

**Что делает пользователь:**
- Заходит в личный кабинет → "Стать создателем"
- Заполняет анкету:
  - Навыки (Python, PyTorch, TensorFlow)
  - Опыт работы с ML
  - Портфолио
- Нажимает "Отправить заявку"

**Что делает бэк:**
```javascript
PUT /api/users/role
{
  role: 'creator',
  applicationData: { skills, experience, portfolio }
}

// Бэк обновляет пользователя
User {
  role: 'creator',
  creatorProfile: {
    verificationStatus: 'pending'   // ← ждет проверки
  }
}

// Отправляет уведомление админу
→ Notification: 'Новая заявка на создателя от Ивана Петрова'
```

---

### **Шаг 2. Проверка админом**

**Что делает админ:**
- Видит заявку в админ-панели
- Проверяет навыки, портфолио
- Принимает или отклоняет

**Если приняли:**
```javascript
PUT /api/admin/verify-creator/:userId
{ status: 'verified' }

// Бэк обновляет
User {
  creatorProfile: {
    verificationStatus: 'verified',
    verifiedAt: new Date()
  }
}

// Отправляет уведомление создателю
→ Notification: 'Поздравляем! Вы стали создателем'
```

**Если отклонили:**
```javascript
PUT /api/admin/verify-creator/:userId
{ status: 'rejected', reason: 'Недостаточно опыта' }

User {
  creatorProfile: {
    verificationStatus: 'rejected'
  }
}

→ Notification: 'Ваша заявка отклонена. Причина: ...'
```

---

### **Шаг 3. Генерация API Key**

**Что делает создатель:**
- Получает уведомление об одобрении
- Переходит в раздел "API Ключи"
- Нажимает "Создать новый ключ"

**Что делает бэк:**
```javascript
POST /api/creator/generate-api-key
{ name: 'Мой домашний ПК' }

// Генерирует ключ (показывает 1 раз!)
{
  id: 'api_key_123',
  key: 'kbr_live_a1b2c3d4e5f6...',  // ← ПОКАЗАТЬ ТОЛЬКО СЕЙЧАС!
  prefix: 'kbr_live_a1b2c3d4',
  name: 'Мой домашний ПК',
  createdAt: new Date()
}

// Сохраняет в отдельную коллекцию (хешированный)
ApiKey {
  userId: ObjectId,
  keyHash: 'bcrypt_hash...',  // Сам ключ НЕ храним!
  name: 'Мой домашний ПК',
  isActive: true,
  createdAt: new Date()
}
```

**Важно!** Ключ показывается только 1 раз! Сохранить его нужно сразу!

---

### **Шаг 4. Установка локального приложения**

**Что делает создатель:**
- Скачивает десктопное приложение (Windows/Mac/Linux)
- Устанавливает на свой компьютер
- Открывает приложение → вводит API Key

**Что делает локальное приложение:**
```javascript
POST /api/training/connect
Headers: { 'X-API-Key': 'kbr_live_a1b2c3d4...' }
{
  machineId: 'uuid-компьютера',
  hardware: { gpu: 'RTX 3090', ram: '64GB' }
}

// Бэк проверяет ключ
// 1. Находит хеш ключа в ApiKey
// 2. Сравнивает: bcrypt.compare(ключ, хеш)
// 3. Если совпало → активирует приложение

User {
  creatorProfile: {
    localApp: {
      installed: true,
      version: '1.0.0',
      lastActive: new Date(),
      machineId: 'uuid-компьютера',
      hardware: { gpu: 'RTX 3090', ram: '64GB' }
    }
  }
}
```

---

## 🔑 **ПОЧЕМУ API KEY В ОТДЕЛЬНОЙ КОЛЛЕКЦИИ?**

| Что делаем | Зачем |
|------------|-------|
| **Отдельная коллекция** | Можно создать несколько ключей на разные ПК |
| **Храним хеш, а не ключ** | Если украдут БД → ключи не украдут |
| **isActive: true/false** | Можно отозвать ключ, не удаляя |
| **lastUsedAt** | Видно, используется ли ключ |

---

## 🎯 **ЧТО МОЖЕТ ДЕЛАТЬ СОЗДАТЕЛЬ**

| Действие | Как |
|----------|-----|
| **Создать модель** | Настраивает архитектуру через интерфейс |
| **Загрузить датасет** | Загружает файлы для обучения |
| **Обучить модель** | Через локальное приложение (использует свою GPU) |
| **Создать агента** | Создает ИИ-агента с нуля |
| **Кастомизировать агента** | Настраивает внешность созданного агента |
| **Опубликовать агента** | Выставляет в маркетплейс, устанавливает цену |
| **Зарабатывать** | Получает деньги от пользователей |

---

## 🎨 **КАСТОМИЗАЦИЯ АВАТАРА ПОЛЬЗОВАТЕЛЯ**

Пользователь может настроить свой аватар в личном кабинете:

```javascript
User {
  appearance: {
    model: string          // Выбор 3D модели
    hair: string           // Прическа
    skinColor: string      // Цвет кожи
    headwear: string       // Головной убор
    accessory: string      // Аксессуар
  }
  photo: string (URL)      // Реальное фото
}
```

**Где отображается:**
- Личный кабинет
- Чат с агентом (аватар пользователя)
- Карточка пользователя
- Проекты (список участников)

---

## 🤖 **КАСТОМИЗАЦИЯ АГЕНТА (для создателя)**

Создатель может настроить внешность созданного агента:

```javascript
Agent {
  appearance: {
    // 2D режим
    '2d': {
      spriteUrl: string
      emotionSprites: { neutral, happy, sad, angry, thinking }
    }
    // 3D режим
    '3d': {
      modelUrl: string
      animations: { idle, talking, thinking, working, success, error }
    }
    // Кастомизация
    customization: {
      bodyColor: string       // Цвет тела
      skinColor: string       // Цвет кожи
      outfitType: string      // Одежда (инженер, ученый, и т.д.)
      outfitColor: string     // Цвет одежды
      hairStyle: string       // Прическа
      hairColor: string       // Цвет волос
      accessories: [string]   // Аксессуары (очки, шлем, и т.д.)
      specialElements: {      // Спец. элементы для профессий
        projectorGlasses: boolean
        weldingMask: boolean
        clipboard: boolean
      }
    }
  }
}
```

**Как работает:**
1. Создатель создает агента
2. Переходит в раздел "Настройка внешности"
3. Выбирает 2D или 3D режим
4. Настраивает все параметры
5. Агент отображается в чате и карточке с выбранной внешностью

---

## 📊 **СРАВНЕНИЕ ВОЗМОЖНОСТЕЙ**

| Возможность | Обычный user | Создатель |
|-------------|--------------|-----------|
| Кастомизация своего аватара | ✅ | ✅ |
| Использовать готовых агентов | ✅ | ✅ |
| Чат с агентом | ✅ | ✅ |
| Создавать задачи | ✅ | ✅ |
| Заказывать физические детали | ✅ | ✅ |
| Создавать агентов | ❌ | ✅ |
| Кастомизировать агентов | ❌ | ✅ |
| Обучать модели | ❌ | ✅ |
| API Key | ❌ | ✅ (в отдельной коллекции) |
| Локальное приложение | ❌ | ✅ |
| Публиковать в маркетплейс | ❌ | ✅ |
| Зарабатывать | ❌ | ✅ |

---

## 🔄 **ПОЛНЫЙ ЦИКЛ РАБОТЫ СОЗДАТЕЛЯ**

```
1. Регистрация + подтверждение email
            ↓
2. Подача заявки на создателя (pending)
            ↓
3. Админ проверяет и одобряет (verified)
            ↓
4. Генерация API Key (показываем 1 раз!)
            ↓
5. Установка локального приложения + ввод API Key
            ↓
6. Загрузка датасета в систему
            ↓
7. Обучение модели (локально на своей GPU)
            ↓
8. Создание агента (внешность + когнитивные узлы)
            ↓
9. Привязка модели к агенту
            ↓
10. Публикация в маркетплейсе
            ↓
11. Получение дохода от продаж
```

---

## 🔐 **ГЛАВНОЕ: API Key ТОЛЬКО ДЛЯ СОЗДАТЕЛЕЙ!**

```javascript
if (user.role === 'creator' && creatorProfile.verificationStatus === 'verified') {
  // ✅ Может генерировать API Key
} else {
  // ❌ НЕТ
}
```
