# 🏗️ CSS Grid - Краткий справочник

## 🎯 Основы

### Создание сетки:
```css
.container {
  display: grid; /* или inline-grid */
}
```

## 📐 Создание колонок и строк

### Колонки:
```css
.container {
  grid-template-columns: 100px 200px 300px; /* 3 колонки */
  grid-template-columns: 1fr 2fr 1fr;       /* пропорции */
  grid-template-columns: repeat(3, 1fr);    /* 3 равные колонки */
  grid-template-columns: 1fr minmax(200px, 1fr); /* минимальная ширина */
}
```

### Строки:
```css
.container {
  grid-template-rows: 100px auto 200px;     /* 3 строки */
  grid-template-rows: repeat(4, 100px);     /* 4 строки по 100px */
}
```

## 📏 Единицы измерения

| Единица | Описание | Пример |
|---------|----------|--------|
| `fr` | Доля свободного пространства | `1fr 2fr` |
| `auto` | По размеру контента | `auto 1fr` |
| `minmax()` | Минимальная и максимальная | `minmax(200px, 1fr)` |
| `repeat()` | Повторение | `repeat(3, 1fr)` |
| `fit-content()` | По контенту | `fit-content(200px)` |

## 🎯 Размещение элементов

### Явное размещение:
```css
.item {
  grid-column: 2 / 4; /* от 2 до 4 колонки */
  grid-row: 1 / 3;    /* от 1 до 3 строки */
}
```

### Сокращённая запись:
```css
.item {
  grid-area: 1 / 2 / 3 / 4; /* row-start / col-start / row-end / col-end */
}
```

## 📍 Области сетки (Grid Areas)

### Определение областей:
```css
.container {
  display: grid;
  grid-template-areas:
    "header header header"
    "sidebar content content"
    "footer footer footer";
}

.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.content { grid-area: content; }
.footer { grid-area: footer; }
```

## ↔️ Выравнивание

### Выравнивание внутри ячеек:
```css
.container {
  justify-items: center;    /* по горизонтали */
  align-items: center;      /* по вертикали */
  place-items: center;      /* оба сразу */
}

/* Для конкретного элемента: */
.item {
  justify-self: start;
  align-self: end;
  place-self: center;
}
```

### Выравнивание всей сетки:
```css
.container {
  justify-content: space-between; /* по горизонтали */
  align-content: center;          /* по вертикали */
  place-content: center;          /* оба сразу */
}
```

## 🔄 Авторазмещение

### Направление:
```css
.container {
  grid-auto-flow: row;        /* по строкам (по умолчанию) */
  grid-auto-flow: column;     /* по колонкам */
  grid-auto-flow: dense;      /* заполнять пустоты */
}
```

### Авторазмеры:
```css
.container {
  grid-auto-rows: 100px;      /* высота новых строк */
  grid-auto-columns: 1fr;     /* ширина новых колонок */
}
```

## 📏 Отступы

### Свойство `gap`:
```css
.container {
  gap: 20px;              /* отступы между рядами и колонками */
  row-gap: 10px;          /* только между строками */
  column-gap: 15px;       /* только между колонками */
}
```

## 🎯 Практические примеры

### 1. Базовая сетка:
```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}
```

### 2. Макет страницы:
```css
.page {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 250px 1fr;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}
```

### 3. Адаптивная сетка:
```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}
```

### 4. Карточка с гридом:
```css
.card {
  display: grid;
  grid-template-rows: auto 1fr auto;
  gap: 10px;
  height: 300px;
}
```

### 5. Выравнивание по центру:
```css
.center {
  display: grid;
  place-items: center; /* и по горизонтали и по вертикали */
  height: 100vh;
}
```

## 📱 Медиазапросы для Grid

```css
.container {
  display: grid;
  grid-template-columns: 1fr; /* мобильные */
}

@media (min-width: 768px) {
  .container {
    grid-template-columns: repeat(2, 1fr); /* планшеты */
  }
}

@media (min-width: 1024px) {
  .container {
    grid-template-columns: repeat(3, 1fr); /* десктоп */
  }
}
```

## 🔧 Полезные комбинации

### Адаптивные карточки:
```css
.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 15px;
}
```

### Форма с метками:
```css
.form {
  display: grid;
  grid-template-columns: 150px 1fr;
  gap: 10px;
  align-items: center;
}
```

### Колонки с сайдбаром:
```css
.layout {
  display: grid;
  grid-template-columns: 300px 1fr;
  gap: 30px;
}
```

### Сетка фото:
```css
.photos {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 200px;
  gap: 10px;
}

.photo-wide {
  grid-column: span 2; /* занимает 2 колонки */
}

.photo-tall {
  grid-row: span 2;    /* занимает 2 строки */
}
```

## 📝 Шпаргалка

### Основные свойства:
- `grid-template-columns` — колонки
- `grid-template-rows` — строки
- `grid-template-areas` — именованные области
- `gap` — отступы между ячейками

### Размещение элементов:
- `grid-column` / `grid-row` — позиция
- `grid-area` — область или позиция
- `justify-items` / `align-items` — выравнивание в ячейках
- `justify-content` / `align-content` — выравнивание сетки

### Авторазмещение:
- `grid-auto-flow` — направление
- `grid-auto-rows` / `grid-auto-columns` — размеры новых строк/колонок

## 🚀 Быстрые шаблоны

### Классическая сетка 12 колонок:
```css
.grid-12 {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 20px;
}

.col-4 { grid-column: span 4; }
.col-6 { grid-column: span 6; }
.col-8 { grid-column: span 8; }
```

### Мозаичная сетка:
```css
.masonry {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  grid-auto-rows: 150px;
  gap: 15px;
}
```

### Панель инструментов:
```css
.toolbar {
  display: grid;
  grid-auto-flow: column;
  gap: 10px;
  justify-content: start;
}
```

