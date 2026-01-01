# 🎨 CSS Шпаргалки: От Основ до Продвинутого

## 📌 Базовый синтаксис и селекторы

### Структура CSS правила
```css
селектор {
    свойство: значение;
    другое-свойство: другое-значение;
}

/* Пример */
.container {
    width: 100%;
    color: #333;
}
```

### Основные селекторы
```css
/* По тегу */
div { color: red; }

/* По классу */
.class-name { color: blue; }

/* По ID */
#element-id { color: green; }

/* Универсальный селектор */
* { margin: 0; padding: 0; }

/* Группировка селекторов */
h1, h2, h3 { font-family: sans-serif; }

/* Селектор потомков */
div p { margin: 10px; } /* все p внутри div */

/* Дочерний селектор (непосредственный потомок) */
div > p { color: red; }

/* Соседний селектор (следующий элемент) */
h1 + p { margin-top: 0; }

/* Общий соседний селектор */
h1 ~ p { color: gray; }
```

### Селекторы атрибутов
```css
/* Элемент с атрибутом */
a[href] { color: blue; }

/* С определенным значением */
input[type="text"] { border: 1px solid #ccc; }

/* Начинается с... */
a[href^="https"] { color: green; }

/* Заканчивается на... */
a[href$=".pdf"]::after { content: " (PDF)"; }

/* Содержит... */
a[href*="example"] { font-weight: bold; }

/* Одно из значений разделенных пробелом */
div[class~="special"] { border: 2px solid red; }

/* Начинается с (для классов и т.д.) */
div[class|="lang"] { font-style: italic; }
```

### Псевдоклассы
```css
/* Состояния ссылок */
a:link { color: blue; }      /* непосещенная */
a:visited { color: purple; } /* посещенная */
a:hover { color: red; }      /* при наведении */
a:active { color: orange; }  /* при нажатии */
a:focus { outline: 2px solid blue; } /* при фокусе */

/* Структурные псевдоклассы */
:first-child { color: red; }           /* первый ребенок */
:last-child { color: blue; }          /* последний ребенок */
:nth-child(2) { font-weight: bold; }  /* конкретный ребенок */
:nth-child(odd) { background: #eee; } /* нечетные */
:nth-child(even) { background: #fff; }/* четные */
:nth-child(3n) { color: green; }      /* каждый третий */
:nth-child(3n+1) { color: red; }      /* каждый третий, начиная с первого */

/* Особые состояния */
:disabled { opacity: 0.5; }          /* отключенные элементы */
:checked { background: green; }      /* выбранные checkbox/radio */
:required { border-color: red; }     /* обязательные поля */
:valid { border-color: green; }      /* валидные значения */
:invalid { border-color: red; }      /* невалидные значения */
:empty { display: none; }            /* пустые элементы */

/* Новые псевдоклассы */
:focus-visible { outline: 2px solid blue; } /* фокус только при клавиатурной навигации */
:focus-within { background: #f0f8ff; } /* элемент содержит фокус */
:is(header, footer) { padding: 20px; } /* любой из селекторов */
:where(header, footer) { margin: 0; }  /* как :is, но с нулевой специфичностью */
:has(img) { border: 1px solid #ccc; }  /* содержит определенный элемент */
```

### Псевдоэлементы
```css
/* Добавляет контент перед элементом */
.element::before {
    content: "★ ";
    color: gold;
}

/* Добавляет контент после элемента */
.element::after {
    content: "!";
    color: red;
}

/* Первая буква */
p::first-letter {
    font-size: 2em;
    float: left;
}

/* Первая строка */
p::first-line {
    font-weight: bold;
}

/* Выделенный текст */
::selection {
    background: yellow;
    color: black;
}

/* Плейсхолдер в input */
input::placeholder {
    color: #999;
    font-style: italic;
}

/* Маркеры списка */
li::marker {
    color: red;
    font-size: 1.2em;
}
```

## 🎨 Цвета и фон

### Форматы цветов
```css
/* Именованные цвета */
color: red;
color: transparent;

/* HEX */
color: #ff0000;           /* красный */
color: #f00;              /* короткая запись */
color: #ff000080;         /* с альфа-каналом */

/* RGB/RGBA */
color: rgb(255, 0, 0);    /* красный */
color: rgba(255, 0, 0, 0.5); /* 50% прозрачности */

/* HSL/HSLA */
color: hsl(0, 100%, 50%); /* красный */
color: hsla(0, 100%, 50%, 0.5);

/* Новые системы */
color: lab(54 80 -5);     /* LAB цвет */
color: lch(54 80 308);    /* LCH цвет */
color: oklab(0.6 0.2 -0.1); /* OKLAB */
color: color(display-p3 1 0 0); /* P3 цвет */
```

### Свойства фона
```css
/* Основные свойства фона */
background-color: #f0f0f0;
background-image: url('image.jpg');
background-repeat: no-repeat; /* repeat, repeat-x, repeat-y, space, round */
background-position: center center; /* top, bottom, left, right, x% y% */
background-size: cover; /* contain, auto, 100% 100%, 200px 150px */
background-attachment: fixed; /* scroll, local */
background-clip: border-box; /* padding-box, content-box, text */
background-origin: padding-box; /* border-box, content-box */
background-blend-mode: multiply; /* normal, screen, overlay, etc */

/* Короткая запись */
background: #f0f0f0 url('image.jpg') center/cover no-repeat fixed;

/* Градиенты */
background: linear-gradient(to right, red, blue);
background: linear-gradient(45deg, red, blue, green);
background: linear-gradient(to bottom, transparent, rgba(0,0,0,0.8));
background: radial-gradient(circle at center, red, blue);
background: conic-gradient(red, yellow, lime, aqua, blue, magenta, red);
background: repeating-linear-gradient(45deg, #fff 0px, #fff 10px, #eee 10px, #eee 20px);

/* Множественные фоны */
background: 
    url('image1.jpg') top left no-repeat,
    url('image2.jpg') bottom right no-repeat,
    linear-gradient(to bottom, #fff, #000);
```

## 📏 Размеры и отступы

### Единицы измерения
```css
/* Абсолютные */
width: 10px;     /* пиксели */
width: 1in;      /* дюймы */
width: 2.54cm;   /* сантиметры */
width: 25.4mm;   /* миллиметры */
width: 72pt;     /* пункты (1pt = 1/72in) */
width: 6pc;      /* пики (1pc = 12pt) */

/* Относительные */
width: 50%;      /* проценты от родителя */
width: 10em;     /* относительно font-size элемента */
width: 10rem;    /* относительно font-size html */
width: 10vw;     /* 10% ширины viewport */
width: 10vh;     /* 10% высоты viewport */
width: 10vmin;   /* 10% меньшей стороны viewport */
width: 10vmax;   /* 10% большей стороны viewport */

/* Современные единицы */
width: 10svw;    /* small viewport width */
width: 10lvh;    /* large viewport height */
width: 10dvh;    /* dynamic viewport height */
width: 10cqw;    /* 10% от ширины контейнера */
width: 10cqi;    /* 10% от inline размера контейнера */
```

### Box Model
```css
/* Размеры */
width: 100px;
height: 100px;
min-width: 50px;
max-width: 200px;
min-height: 50px;
max-height: 200px;

/* Внутренние отступы */
padding: 10px;                  /* все стороны */
padding: 10px 20px;             /* верх/низ и лево/право */
padding: 10px 20px 30px 40px;  /* верх, право, низ, лево */
padding-top: 10px;
padding-right: 20px;
padding-bottom: 30px;
padding-left: 40px;

/* Внешние отступы */
margin: 10px;                   /* все стороны */
margin: 10px 20px;              /* верх/низ и лево/право */
margin: 10px 20px 30px 40px;    /* верх, право, низ, лево */
margin: auto;                   /* центрирование блока */
margin-top: 10px;
margin-right: 20px;
margin-bottom: 30px;
margin-left: 40px;

/* Границы */
border: 2px solid #000;         /* ширина стиль цвет */
border-width: 2px;
border-style: solid;            /* none, hidden, dotted, dashed, solid, double, groove, ridge, inset, outset */
border-color: #000;
border-top: 1px dotted red;
border-radius: 10px;            /* скругление углов */
border-radius: 10px 20px 30px 40px; /* каждый угол отдельно */
border-radius: 50%;             /* круг */
border-image: url('border.png') 30 round;

/* Box-sizing */
box-sizing: border-box;         /* width включает padding и border */
box-sizing: content-box;        /* width не включает padding и border */
```

## 📐 Позиционирование и отображение

### Свойство display
```css
display: block;          /* блочный элемент */
display: inline;         /* строчный элемент */
display: inline-block;   /* строчно-блочный */
display: none;           /* скрыть элемент */
display: flex;           /* flex контейнер */
display: inline-flex;    /* строчный flex */
display: grid;           /* grid контейнер */
display: inline-grid;    /* строчный grid */
display: flow-root;      /* новый блоковый контекст */
display: contents;       /* игнорирует собственный бокс */
display: table;          /* табличная раскладка */
display: list-item;      /* как элемент списка */
```

### Позиционирование
```css
position: static;        /* по умолчанию */
position: relative;      /* относительно своего положения */
position: absolute;      /* относительно ближайшего positioned предка */
position: fixed;         /* относительно viewport */
position: sticky;        /* липкое позиционирование */

/* Сдвиги при relative/absolute/fixed/sticky */
top: 10px;
right: 20px;
bottom: 30px;
left: 40px;

/* Z-index (работает только с positioned элементами) */
z-index: 10;

/* Пример sticky */
.sticky-element {
    position: sticky;
    top: 0;
    background: white;
}
```

### Flexbox
```css
/* Контейнер */
.container {
    display: flex; /* или inline-flex */
    
    /* Направление основной оси */
    flex-direction: row; /* row, row-reverse, column, column-reverse */
    
    /* Перенос строк */
    flex-wrap: nowrap; /* nowrap, wrap, wrap-reverse */
    
    /* Короткая запись direction + wrap */
    flex-flow: row wrap;
    
    /* Выравнивание по основной оси */
    justify-content: flex-start; /* flex-start, flex-end, center, space-between, space-around, space-evenly */
    
    /* Выравнивание по поперечной оси */
    align-items: stretch; /* stretch, flex-start, flex-end, center, baseline */
    
    /* Многострочное выравнивание */
    align-content: stretch; /* stretch, flex-start, flex-end, center, space-between, space-around */
}

/* Элементы */
.item {
    /* Порядок отображения */
    order: 0; /* целое число */
    
    /* Способность к растяжению */
    flex-grow: 0; /* число */
    
    /* Способность к сжатию */
    flex-shrink: 1; /* число */
    
    /* Базовый размер */
    flex-basis: auto; /* размер или auto */
    
    /* Короткая запись */
    flex: 0 1 auto; /* grow shrink basis */
    flex: 1; /* flex: 1 1 0 */
    
    /* Выравнивание отдельного элемента */
    align-self: auto; /* auto, flex-start, flex-end, center, baseline, stretch */
}

/* Примеры использования */
.centered {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.equal-columns {
    display: flex;
}

.equal-columns > * {
    flex: 1;
}

.card-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
}

.card-grid > * {
    flex: 1 1 300px; /* базовый размер 300px, но может сжиматься и растягиваться */
}
```

### CSS Grid
```css
/* Контейнер */
.container {
    display: grid; /* или inline-grid */
    
    /* Определение колонок */
    grid-template-columns: 100px 1fr 2fr; /* фиксированная, гибкая, вдвое больше */
    grid-template-columns: repeat(3, 1fr); /* 3 равные колонки */
    grid-template-columns: minmax(100px, 1fr) 2fr; /* минимум 100px, максимум 1fr */
    grid-template-columns: [col-start] 1fr [col-2] 1fr [col-end]; /* с именами линий */
    
    /* Определение строк */
    grid-template-rows: 100px auto 200px;
    grid-template-rows: repeat(2, minmax(100px, auto));
    
    /* Короткая запись */
    grid-template: 
        "header header" 100px
        "sidebar main" 1fr
        "footer footer" 50px / 200px 1fr;
    
    /* Области грида */
    grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
    
    /* Промежутки */
    gap: 20px; /* row-gap column-gap */
    row-gap: 10px;
    column-gap: 15px;
    
    /* Выравнивание по горизонтали */
    justify-content: start; /* start, end, center, stretch, space-around, space-between, space-evenly */
    
    /* Выравнивание по вертикали */
    align-content: start; /* start, end, center, stretch, space-around, space-between, space-evenly */
    
    /* Автоматическое размещение */
    grid-auto-flow: row; /* row, column, row dense, column dense */
    grid-auto-columns: 100px;
    grid-auto-rows: minmax(100px, auto);
}

/* Элементы грида */
.item {
    /* Размещение по номерам линий */
    grid-column-start: 1;
    grid-column-end: 3;
    grid-row-start: 1;
    grid-row-end: 2;
    
    /* Короткая запись */
    grid-column: 1 / 3; /* или span 2 */
    grid-row: 1 / 2; /* или span 1 */
    
    /* Размещение по именам линий */
    grid-column: col-start / col-end;
    
    /* Размещение по областям */
    grid-area: header; /* или: 1 / 1 / 2 / 3 */
    
    /* Выравнивание внутри ячейки */
    justify-self: stretch; /* start, end, center, stretch */
    align-self: stretch; /* start, end, center, stretch */
    
    /* Короткая запись */
    place-self: center stretch;
}

/* Примеры */
.grid-layout {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
}

.holy-grail {
    display: grid;
    grid-template: 
        "header header header" 80px
        "nav main aside" 1fr
        "footer footer footer" 60px
        / 200px 1fr 150px;
}

header { grid-area: header; }
nav { grid-area: nav; }
main { grid-area: main; }
aside { grid-area: aside; }
footer { grid-area: footer; }
```

## ✨ Трансформации и переходы

### Transform
```css
/* 2D трансформации */
transform: translate(100px, 50px);    /* смещение */
transform: translateX(100px);
transform: translateY(50px);
transform: scale(2);                  /* масштаб */
transform: scale(1.5, 0.5);           /* по X и Y */
transform: scaleX(2);
transform: scaleY(0.5);
transform: rotate(45deg);             /* поворот */
transform: skew(20deg, 10deg);        /* наклон */
transform: skewX(20deg);
transform: skewY(10deg);

/* 3D трансформации */
transform: translate3d(100px, 50px, 0);
transform: scale3d(2, 2, 2);
transform: rotate3d(1, 1, 1, 45deg);
transform: rotateX(45deg);
transform: rotateY(45deg);
transform: rotateZ(45deg);
transform: perspective(500px) rotateY(45deg);

/* Множественные трансформации */
transform: translate(50px, 50px) rotate(45deg) scale(1.5);

/* Центр трансформации */
transform-origin: center center;      /* по умолчанию */
transform-origin: 0 0;                /* левый верхний угол */
transform-origin: 100% 100%;          /* правый нижний угол */
transform-origin: 20px 40px;          /* конкретные координаты */

/* Стиль 3D */
transform-style: flat;                /* по умолчанию */
transform-style: preserve-3d;         /* сохраняет 3D пространство */
backface-visibility: visible;         /* видимость обратной стороны */
backface-visibility: hidden;          /* скрыть обратную сторону */
```

### Transition
```css
/* Какие свойства анимировать */
transition-property: all;             /* все свойства */
transition-property: opacity, transform; /* конкретные свойства */
transition-property: none;            /* без анимации */

/* Длительность анимации */
transition-duration: 0.3s;
transition-duration: 500ms;

/* Функция времени */
transition-timing-function: ease;     /* по умолчанию */
transition-timing-function: linear;   /* постоянная скорость */
transition-timing-function: ease-in;  /* медленно начинает */
transition-timing-function: ease-out; /* медленно заканчивает */
transition-timing-function: ease-in-out; /* медленно начинает и заканчивает */
transition-timing-function: cubic-bezier(0.42, 0, 0.58, 1); /* кастомная */
transition-timing-function: steps(4, jump-start); /* ступенчатая */

/* Задержка перед началом */
transition-delay: 0.2s;

/* Короткая запись */
transition: all 0.3s ease 0.2s;
transition: opacity 0.5s, transform 0.3s;
transition: 0.3s transform; /* property=duration, остальное по умолчанию */

/* Пример использования */
.button {
    background: blue;
    transition: background 0.3s, transform 0.2s;
}

.button:hover {
    background: darkblue;
    transform: translateY(-2px);
}
```

### Animation
```css
/* Определение ключевых кадров */
@keyframes slide-in {
    from {
        transform: translateX(-100%);
        opacity: 0;
    }
    to {
        transform: translateX(0);
        opacity: 1;
    }
}

/* Или с процентами */
@keyframes bounce {
    0%, 100% {
        transform: translateY(0);
    }
    50% {
        transform: translateY(-20px);
    }
}

/* Применение анимации */
.element {
    /* Название анимации */
    animation-name: slide-in;
    
    /* Длительность */
    animation-duration: 0.5s;
    
    /* Функция времени */
    animation-timing-function: ease-out;
    
    /* Задержка */
    animation-delay: 0.2s;
    
    /* Количество повторений */
    animation-iteration-count: 1; /* или infinite */
    
    /* Направление */
    animation-direction: normal; /* normal, reverse, alternate, alternate-reverse */
    
    /* Состояние после завершения */
    animation-fill-mode: none; /* none, forwards, backwards, both */
    
    /* Проигрывание/пауза */
    animation-play-state: running; /* running, paused */
    
    /* Короткая запись */
    animation: slide-in 0.5s ease-out 0.2s 1 normal forwards;
}

/* Примеры популярных анимаций */
@keyframes fade-in {
    from { opacity: 0; }
    to { opacity: 1; }
}

@keyframes fade-out {
    from { opacity: 1; }
    to { opacity: 0; }
}

@keyframes slide-up {
    from { transform: translateY(100px); opacity: 0; }
    to { transform: translateY(0); opacity: 1; }
}

@keyframes slide-down {
    from { transform: translateY(-100px); opacity: 0; }
    to { transform: translateY(0); opacity: 1; }
}

@keyframes pulse {
    0% { transform: scale(1); }
    50% { transform: scale(1.1); }
    100% { transform: scale(1); }
}

@keyframes spin {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
}
```

## 🎯 Типографика и текст

### Свойства шрифта
```css
/* Семейство шрифтов */
font-family: Arial, sans-serif;
font-family: "Times New Roman", serif;
font-family: "Segoe UI", system-ui, sans-serif;
font-family: var(--font-primary); /* CSS переменные */

/* Размер шрифта */
font-size: 16px;
font-size: 1rem; /* относительно html */
font-size: 1.5em; /* относительно родителя */
font-size: 120%; /* относительно родителя */
font-size: clamp(1rem, 2vw, 1.5rem); /* адаптивный */

/* Насыщенность */
font-weight: normal; /* normal, bold, bolder, lighter */
font-weight: 400; /* от 100 до 900 */
font-weight: var(--font-weight-bold);

/* Стиль */
font-style: normal; /* normal, italic, oblique */
font-style: italic;

/* Растяжение */
font-stretch: normal; /* ultra-condensed, extra-condensed, condensed, semi-condensed, normal, semi-expanded, expanded, extra-expanded, ultra-expanded */

/* Размер заглавных букв */
font-variant-caps: normal; /* small-caps, all-small-caps, petite-caps, all-petite-caps, unicase, titling-caps */

/* Варианты цифр */
font-variant-numeric: normal; /* lining-nums, oldstyle-nums, proportional-nums, tabular-nums, diagonal-fractions, stacked-fractions */

/* Короткая запись */
font: italic small-caps bold 16px/1.5 Arial, sans-serif;
/* style variant weight size/line-height family */
```

### Свойства текста
```css
/* Выравнивание */
text-align: left; /* left, right, center, justify, start, end */
text-align-last: auto; /* auto, left, right, center, justify */
text-justify: auto; /* auto, inter-word, inter-character, none */

/* Высота строки */
line-height: 1.5; /* безразмерное число */
line-height: 24px; /* абсолютное значение */
line-height: 150%; /* процентное */

/* Межсимвольный интервал */
letter-spacing: 0.1em; /* нормальное расстояние между буквами */
letter-spacing: -0.05em; /* отрицательное для сжатия */

/* Межсловный интервал */
word-spacing: 0.2em;

/* Декорация текста */
text-decoration: underline; /* none, underline, overline, line-through */
text-decoration-line: underline;
text-decoration-style: solid; /* solid, double, dotted, dashed, wavy */
text-decoration-color: red;
text-decoration-thickness: 2px;
text-underline-offset: 4px; /* расстояние от текста */
text-underline-position: auto; /* auto, under, left, right */

/* Трансформация текста */
text-transform: none; /* none, capitalize, uppercase, lowercase, full-width */
text-transform: capitalize; /* Каждое Слово С Заглавной */

/* Отступ первой строки */
text-indent: 2em;

/* Обрезка текста */
text-overflow: clip; /* clip, ellipsis, string */
text-overflow: ellipsis; /* троеточие при переполнении */

/* Перенос слов */
word-wrap: normal; /* normal, break-word */
overflow-wrap: normal; /* normal, break-word, anywhere */
word-break: normal; /* normal, break-all, keep-all, break-word */

/* Перенос длинных слов */
hyphens: manual; /* none, manual, auto */

/* Пробелы и отступы */
white-space: normal; /* normal, nowrap, pre, pre-wrap, pre-line, break-spaces */

/* Направление текста */
direction: ltr; /* ltr, rtl */
writing-mode: horizontal-tb; /* horizontal-tb, vertical-rl, vertical-lr */
text-orientation: mixed; /* mixed, upright, sideways */

/* Тень текста */
text-shadow: 2px 2px 4px rgba(0,0,0,0.5); /* смещениеX смещениеY размытие цвет */
text-shadow: 0 0 10px #fff, 0 0 20px #fff; /* множественные тени */

/* Обводка текста */
-webkit-text-stroke: 1px black; /* ширина цвет */
-webkit-text-fill-color: transparent; /* прозрачная заливка */
```

## 🎭 Эффекты и фильтры

### Filter
```css
/* Простые фильтры */
filter: blur(5px);                 /* размытие */
filter: brightness(150%);          /* яркость */
filter: contrast(200%);            /* контраст */
filter: grayscale(100%);           /* черно-белый */
filter: hue-rotate(90deg);         /* сдвиг цвета */
filter: invert(100%);              /* инверсия */
filter: opacity(50%);              /* прозрачность */
filter: saturate(200%);            /* насыщенность */
filter: sepia(100%);               /* сепия */

/* Тень */
filter: drop-shadow(5px 5px 10px rgba(0,0,0,0.5));

/* Множественные фильтры */
filter: brightness(150%) contrast(120%) saturate(200%);

/* SVG фильтры */
filter: url(#myFilter);
```

### Backdrop-filter
```css
/* Размытие фона за элементом */
backdrop-filter: blur(10px);
backdrop-filter: brightness(150%);
backdrop-filter: contrast(120%);
backdrop-filter: grayscale(50%);
backdrop-filter: hue-rotate(90deg);
backdrop-filter: invert(100%);
backdrop-filter: opacity(50%);
backdrop-filter: saturate(200%);
backdrop-filter: sepia(100%);

/* Множественные фильтры */
backdrop-filter: blur(5px) brightness(120%);
```

### Blend modes
```css
/* Смешивание фона */
background-blend-mode: normal; /* normal, multiply, screen, overlay, darken, lighten, color-dodge, color-burn, hard-light, soft-light, difference, exclusion, hue, saturation, color, luminosity */

/* Смешивание с фоном страницы */
mix-blend-mode: normal; /* те же значения */

/* Примеры */
.blend-multiply {
    background: url('image.jpg'), linear-gradient(red, blue);
    background-blend-mode: multiply;
}

.text-blend {
    color: white;
    mix-blend-mode: difference;
}
```

### Clip-path
```css
/* Базовые формы */
clip-path: circle(50% at 50% 50%);
clip-path: ellipse(50% 40% at 50% 50%);
clip-path: inset(10% 20% 30% 40%); /* top right bottom left */
clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
clip-path: path('M10,10 L100,10 L100,100 Z');

/* URL к SVG */
clip-path: url(#myClipPath);

/* Примеры */
.diamond {
    clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
}

.speech-bubble {
    clip-path: polygon(
        0% 0%, 100% 0%, 100% 75%, 
        75% 75%, 75% 100%, 50% 75%, 
        0% 75%
    );
}
```

## 🎨 CSS Custom Properties (Переменные)

### Определение и использование
```css
/* Глобальные переменные */
:root {
    --primary-color: #3498db;
    --secondary-color: #2ecc71;
    --font-size: 16px;
    --spacing: 1rem;
    --border-radius: 8px;
    --transition: all 0.3s ease;
    --shadow: 0 2px 10px rgba(0,0,0,0.1);
}

/* Локальные переменные (в пределах селектора) */
.element {
    --local-color: #ff0000;
}

/* Использование переменных */
.button {
    background: var(--primary-color);
    font-size: var(--font-size, 16px); /* значение по умолчанию */
    padding: calc(var(--spacing) * 2); /* вычисления */
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
    transition: var(--transition);
}

/* Динамическое изменение через JavaScript */
document.documentElement.style.setProperty('--primary-color', '#ff0000');
```

### Полезные системные переменные
```css
/* Цвета системы */
color: Canvas;                /* цвет фона окон */
color: CanvasText;           /* цвет текста окон */
color: LinkText;             /* цвет ссылок */
color: VisitedText;          /* цвет посещенных ссылок */
color: ActiveText;           /* цвет активных ссылок */
color: ButtonFace;           /* цвет фона кнопок */
color: ButtonText;           /* цвет текста кнопок */
color: Field;                /* цвет фона полей ввода */
color: FieldText;            /* цвет текста полей ввода */

/* Пример темной/светлой темы */
:root {
    color-scheme: light dark;
    --bg-color: light-dark(#fff, #1a1a1a);
    --text-color: light-dark(#333, #fff);
}
```

## 📱 Адаптивный дизайн

### Media Queries
```css
/* По ширине viewport */
@media (max-width: 768px) { /* до 768px */ }
@media (min-width: 769px) and (max-width: 1024px) { /* от 769px до 1024px */ }
@media (min-width: 1025px) { /* от 1025px и больше */ }

/* По высоте viewport */
@media (max-height: 600px) { }

/* По ориентации */
@media (orientation: portrait) { /* портретная */ }
@media (orientation: landscape) { /* альбомная */ }

/* По разрешению экрана */
@media (min-resolution: 2dppx) { /* ретина дисплеи */ }
@media (resolution: 192dpi) { }

/* По типу устройства */
@media screen { /* экраны */ }
@media print { /* печать */ }
@media speech { /* скринридеры */ }

/* По особенностям устройства */
@media (hover: hover) { /* есть hover */ }
@media (hover: none) { /* нет hover (тач устройства) */ }
@media (pointer: fine) { /* точный указатель (мышь) */ }
@media (pointer: coarse) { /* крупный указатель (палец) */ }
@media (prefers-color-scheme: dark) { /* темная тема */ }
@media (prefers-color-scheme: light) { /* светлая тема */ }
@media (prefers-reduced-motion: reduce) { /* уменьшенная анимация */ }
@media (prefers-contrast: high) { /* высокий контраст */ }
@media (forced-colors: active) { /* режим высокой контрастности Windows */ }

/* Современный синтаксис */
@media (width <= 768px) { /* до 768px */ }
@media (768px <= width <= 1024px) { /* от 768px до 1024px */ }

/* Примеры */
.container {
    width: 1200px;
    margin: 0 auto;
}

@media (max-width: 1200px) {
    .container {
        width: 100%;
        padding: 0 20px;
    }
}

@media (max-width: 768px) {
    .container {
        padding: 0 10px;
    }
}
```

### Современные единицы для адаптивности
```css
/* Clamp для адаптивных размеров */
font-size: clamp(1rem, 2vw, 1.5rem); /* минимум 1rem, предпочтительно 2vw, максимум 1.5rem */
width: clamp(300px, 50%, 800px);

/* Container queries */
@container (min-width: 500px) {
    .card {
        flex-direction: row;
    }
}

@container card (min-width: 700px) {
    .card__image {
        width: 300px;
    }
}
```

### Responsive Images
```css
/* Адаптивные фоновые изображения */
.hero {
    background-image: url('image-small.jpg');
}

@media (min-width: 768px) {
    .hero {
        background-image: url('image-medium.jpg');
    }
}

@media (min-width: 1200px) {
    .hero {
        background-image: url('image-large.jpg');
    }
}

/* Адаптивные свойства для картинок */
img {
    max-width: 100%;
    height: auto;
}

.picture-example {
    width: 100%;
    height: 300px;
    object-fit: cover; /* cover, contain, fill, none, scale-down */
    object-position: center center;
}
```

## 🎯 CSS Методологии и БЭМ

### БЭМ (Block Element Modifier)
```css
/* Block - самостоятельный компонент */
.button { }
.menu { }
.card { }

/* Element - часть блока, не имеет смысла отдельно */
.button__icon { }
.menu__item { }
.card__title { }

/* Modifier - модификатор блока или элемента */
.button--primary { }
.button--disabled { }
.menu--vertical { }
.card__title--large { }

/* Пример */
<button class="button button--primary">
    <span class="button__icon">🎯</span>
    <span class="button__text">Click me</span>
</button>

<style>
.button {
    display: inline-flex;
    align-items: center;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.button--primary {
    background: #3498db;
    color: white;
}

.button--disabled {
    opacity: 0.5;
    cursor: not-allowed;
}

.button__icon {
    margin-right: 8px;
}

.button__text {
    font-weight: bold;
}
</style>
```

## 🔧 Полезные однострочники и решения

### Центрирование
```css
/* Горизонтальное центрирование блока */
margin: 0 auto;

/* Абсолютное центрирование (старый способ) */
position: absolute;
top: 50%;
left: 50%;
transform: translate(-50%, -50%);

/* Flexbox центрирование */
display: flex;
justify-content: center;
align-items: center;

/* Grid центрирование */
display: grid;
place-items: center;

/* Текст по центру */
text-align: center;

/* Вертикальное выравнивание inline элементов */
vertical-align: middle;
```

### Обрезка текста
```css
/* Однострочная обрезка с троеточием */
.truncate {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

/* Многострочная обрезка (2 строки) */
.multiline-truncate {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
}
```

### Скрытие элементов
```css
/* Полное скрытие с сохранением места */
visibility: hidden;

/* Полное скрытие без сохранения места */
display: none;

/* Скрытие, но доступное для скринридеров */
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
}
```

### Clearfix
```css
/* Стандартный clearfix */
.clearfix::after {
    content: '';
    display: table;
    clear: both;
}

/* Современный способ */
.container {
    display: flow-root;
}
```

### Кастомный скроллбар
```css
/* WebKit браузеры (Chrome, Safari, Edge) */
::-webkit-scrollbar {
    width: 12px;
}

::-webkit-scrollbar-track {
    background: #f1f1f1;
    border-radius: 10px;
}

::-webkit-scrollbar-thumb {
    background: #888;
    border-radius: 10px;
}

::-webkit-scrollbar-thumb:hover {
    background: #555;
}

/* Firefox */
html {
    scrollbar-width: thin;
    scrollbar-color: #888 #f1f1f1;
}
```

### Плейсхолдер
```css
/* Стилизация плейсхолдера */
::-webkit-input-placeholder { /* Chrome/Opera/Safari */
    color: #999;
    font-style: italic;
}

::-moz-placeholder { /* Firefox 19+ */
    color: #999;
    font-style: italic;
}

:-ms-input-placeholder { /* IE 10+ */
    color: #999;
    font-style: italic;
}

:-moz-placeholder { /* Firefox 18- */
    color: #999;
    font-style: italic;
}

/* Современный синтаксис */
::placeholder {
    color: #999;
    font-style: italic;
    opacity: 1; /* Firefox уменьшает opacity */
}
```

### Selection (выделение текста)
```css
/* Цвет выделения */
::selection {
    background: #ffeb3b;
    color: #000;
}

::-moz-selection {
    background: #ffeb3b;
    color: #000;
}
```

### Печать
```css
/* Стили для печати */
@media print {
    /* Скрыть элементы */
    .no-print {
        display: none !important;
    }
    
    /* Убрать фоны */
    * {
        background: transparent !important;
        color: black !important;
        box-shadow: none !important;
        text-shadow: none !important;
    }
    
    /* Ссылки */
    a {
        text-decoration: underline;
    }
    
    a[href]:after {
        content: " (" attr(href) ")";
    }
    
    /* Разрыв страницы */
    .page-break {
        page-break-before: always;
    }
    
    /* Не разрывать внутри */
    .no-break {
        page-break-inside: avoid;
    }
}
```

## 🚀 Современные фичи CSS

### Container Queries
```css
/* Определение контейнера */
.container {
    container-type: inline-size;
    container-name: card-container;
}

/* Альтернативная запись */
.container {
    container: card-container / inline-size;
}

/* Запрос к контейнеру */
@container (min-width: 500px) {
    .card {
        flex-direction: row;
    }
}

@container card-container (min-width: 700px) {
    .card__image {
        width: 300px;
    }
}

/* Использование cqw/cqh единиц */
.card {
    font-size: clamp(1rem, 3cqw, 1.5rem);
    padding: 2cqh 3cqw;
}
```

### CSS Nesting
```css
/* Старый способ */
.card { }
.card:hover { }
.card .title { }
.card.active .title { }

/* Новый способ (CSS Nesting) */
.card {
    background: white;
    border-radius: 8px;
    
    &:hover {
        box-shadow: 0 4px 12px rgba(0,0,0,0.15);
    }
    
    & .title {
        font-size: 1.2rem;
        
        .active & {
            color: #3498db;
        }
    }
    
    @media (min-width: 768px) {
        max-width: 400px;
    }
}
```

### Scroll Snap
```css
/* Контейнер с привязкой */
.container {
    scroll-snap-type: y mandatory; /* x, y, block, inline, both */
    scroll-snap-type: x proximity;
    overflow-y: scroll;
    height: 300px;
}

/* Элементы для привязки */
.item {
    scroll-snap-align: start; /* start, end, center, none */
    scroll-snap-stop: always; /* always, normal */
    scroll-margin: 20px; /* отступ при привязке */
}

/* Плавная прокрутка */
html {
    scroll-behavior: smooth;
}
```

### Адаптивная типографика
```css
/* Fluid типографика */
:root {
    --min-font: 16px;
    --max-font: 24px;
    --min-width: 320px;
    --max-width: 1920px;
}

html {
    font-size: clamp(
        var(--min-font),
        calc(var(--min-font) + (var(--max-font) - var(--min-font)) * 
            ((100vw - var(--min-width)) / (var(--max-width) - var(--min-width))),
        var(--max-font)
    );
}

/* Упрощенный вариант */
body {
    font-size: clamp(1rem, 2vw, 1.5rem);
    line-height: clamp(1.4, 1.5, 1.8);
}
```

## 🎨 CSS Анимации готовые к использованию

### Loading Spinners
```css
/* Простой спиннер */
.spinner {
    width: 40px;
    height: 40px;
    border: 4px solid #f3f3f3;
    border-top: 4px solid #3498db;
    border-radius: 50%;
    animation: spin 1s linear infinite;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

/* Точки */
.loading-dots::after {
    content: ' .';
    animation: dots 1.5s steps(5, end) infinite;
}

@keyframes dots {
    0%, 20% { content: ' .'; }
    40% { content: ' ..'; }
    60% { content: ' ...'; }
    80%, 100% { content: ''; }
}
```

### Плавное появление
```css
.fade-in {
    animation: fadeIn 0.5s ease-in;
}

@keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
}

.slide-up {
    animation: slideUp 0.3s ease-out;
}

@keyframes slideUp {
    from {
        transform: translateY(20px);
        opacity: 0;
    }
    to {
        transform: translateY(0);
        opacity: 1;
    }
}
```

## 📚 Ресурсы для изучения CSS

### Интерактивные обучалки
- [CSS-Tricks](https://css-tricks.com/) - статьи и руководства
- [MDN Web Docs](https://developer.mozilla.org/ru/docs/Web/CSS) - документация
- [CSS Grid Generator](https://cssgrid-generator.netlify.app/) - генератор сеток
- [Flexbox Froggy](https://flexboxfroggy.com/) - игра для изучения Flexbox

### Инструменты
- [Autoprefixer](https://autoprefixer.github.io/) - автоматические префиксы
- [PostCSS](https://postcss.org/) - обработка CSS
- [PurgeCSS](https://purgecss.com/) - удаление неиспользуемого CSS
- [CSS Minifier](https://cssminifier.com/) - минификация

### Практика
- [Frontend Mentor](https://www.frontendmentor.io/) - реальные проекты
- [CSS Battle](https://cssbattle.dev/) - CSS игра
- [CodePen Challenges](https://codepen.io/challenges) - челленджи

---

**Совет:** Лучший способ выучить CSS — практиковаться. Создавайте реальные проекты, экспериментируйте с разными техниками и следите за новыми возможностями CSS. Удачи в изучении! 🎨