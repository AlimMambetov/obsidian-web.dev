---
title: Архитектура проекта
---
## 📁 **ПАПКИ В `src/` — ПРОСТО И ПОНЯТНО**

---

### **1. `config/` — настройки**

**Что тут:** Подключение к базам, внешним сервисам.

**Как выглядит:**

```
config/
├── db.ts              # Подключение к MongoDB
├── redis.ts           # Позже (кэширование)
└── cors.ts            # Настройки CORS (разрешённые домены)
```

**Как класть:**

```typescript
// config/db.ts
import mongoose from 'mongoose';

const connectDB = async () => {
  await mongoose.connect(process.env.MONGO_URI);
};

export default connectDB;
```

---

### **2. `models/` — схемы данных (что храним в БД)**

**Что тут:** Описание таблиц/коллекций MongoDB.

**Как выглядит:**

```
models/
├── User.ts            # Пользователь
├── Agent.ts           # ИИ-агент
├── Task.ts            # Задание
├── Computer.ts        # Вычислитель
├── Device.ts          # Робот
├── Transaction.ts     # Транзакция
└── ...
```

**Как класть:**

```typescript
// models/User.ts
const UserSchema = new Schema({
  fullName: String,
  email: String,
  passwordHash: String
});

export default model('User', UserSchema);
```

---

### **3. `controllers/` — логика обработки запросов**

**Что тут:** Функции, которые обрабатывают запросы (регистрация, создание агента).

**Как выглядит:**

```
controllers/
├── authController.ts   # Регистрация, вход
├── userController.ts   # Профиль, обновление
├── agentController.ts  # Создание, редактирование агента
└── taskController.ts   # Создание, статус задания
```

**Как класть:**

```typescript
// controllers/userController.ts
export const getUsers = async (req, res) => {
  const users = await User.find();
  res.json(users);
};
```

---

### **4. `routes/` — маршруты (URL-адреса API)**

**Что тут:** Связывают URL с контроллерами.

**Как выглядит:**

```
routes/
├── authRoutes.ts    # /api/auth/register, /api/auth/login
├── userRoutes.ts    # /api/users
├── agentRoutes.ts   # /api/agents
└── taskRoutes.ts    # /api/tasks
```

**Как класть:**

```typescript
// routes/userRoutes.ts
router.get('/', getUsers);
router.get('/:id', getUserById);
```

---

### **5. `middleware/` — промежуточные обработчики**

**Что тут:** Функции, которые выполняются до контроллера (проверка токена, логирование).

**Как выглядит:**

```
middleware/
├── auth.ts           # Проверка JWT
├── errorHandler.ts   # Обработка ошибок
└── logger.ts         # Логирование запросов (позже)
```

**Как класть:**

```typescript
// middleware/auth.ts
export const auth = (req, res, next) => {
  const token = req.headers.authorization;
  if (!token) return res.status(401).json({ error: 'No token' });
  // проверка токена...
  next();
};
```

---

### **6. `utils/` — вспомогательные функции**

**Что тут:** Полезные функции, которые используются в разных местах.

**Как выглядит:**

```
utils/
├── tokenService.ts   # Генерация JWT
├── emailService.ts   # Отправка писем
├── hashPassword.ts   # Хеширование пароля
└── validators.ts     # Проверка email, телефона
```

**Как класть:**

```typescript
// utils/tokenService.ts
export const generateToken = (userId) => {
  return jwt.sign({ userId }, process.env.JWT_SECRET);
};
```

---

### **7. `types/` — TypeScript типы**

**Что тут:** Описания типов для TypeScript (чтобы код был безопасным).

**Как выглядит:**

```
types/
└── index.ts          # Все типы в одном файле
```

**Как класть:**

```typescript
// types/index.ts
export interface IUser {
  fullName: string;
  email: string;
  passwordHash: string;
}
```

---

### **8. `app.ts` — главный файл**

**Что тут:** Собирает всё вместе — подключает БД, middleware, маршруты, запускает сервер.

**Как выглядит:**

```typescript
// app.ts
import express from 'express';
import connectDB from './config/db';
import userRoutes from './routes/userRoutes';

const app = express();
connectDB();
app.use(express.json());
app.use('/api/users', userRoutes);
app.listen(5000);
```

---

## 📊 **КАК ВСЁ СВЯЗАНО (ПОТОК ЗАПРОСА):**

```
1. Запрос приходит на /api/users
         ↓
2. routes/userRoutes.ts → направляет в контроллер
         ↓
3. controllers/userController.ts → обрабатывает логику
         ↓
4. models/User.ts → работает с базой данных
         ↓
5. Ответ возвращается пользователю
```

---

## 🗂️ **ЧТО КУДА КЛАСТЬ (ПРАВИЛО):**

| Что создаёшь | Куда кладёшь |
|--------------|--------------|
| Схему БД | `models/` |
| Логику запроса | `controllers/` |
| URL-маршрут | `routes/` |
| Проверку токена | `middleware/` |
| Вспомогательную функцию | `utils/` |
| TypeScript тип | `types/` |
| Настройку подключения | `config/` |

---

## ✅ **КОРОТКО:**

| Папка | Содержит |
|-------|----------|
| `config/` | Настройки (БД, CORS) |
| `models/` | Схемы данных |
| `controllers/` | Логика запросов |
| `routes/` | URL-адреса |
| `middleware/` | Проверки до запроса |
| `utils/` | Вспомогательные функции |
| `types/` | TypeScript типы |
| `app.ts` | Главный файл |

---

**Теперь понятно?** Если да — переходим к созданию **модели User** и **GET-запроса**. 👇
--