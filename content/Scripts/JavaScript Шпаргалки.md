# 🚀 JavaScript Шпаргалки: От Новичка до Профи

## 📌 Базовый синтаксис

### Переменные и типы
```javascript
// Объявление
let variable = 'value';     // можно изменять
const CONSTANT = 'fixed';   // нельзя изменять
var oldWay = 'deprecated';  // устаревший способ

// Типы данных
typeof 42;                  // "number"
typeof "text";              // "string"
typeof true;                // "boolean"
typeof undefined;           // "undefined"
typeof null;                // "object" (особенность JS)
typeof {};                  // "object"
typeof [];                  // "object"
typeof Symbol();            // "symbol"
typeof function(){};        // "function"

// Проверки
Array.isArray([]);          // true
isNaN(NaN);                 // true
Number.isFinite(42);        // true
```

### Операторы
```javascript
// Логические
true && false;    // false (И)
true || false;    // true (ИЛИ)
!true;            // false (НЕ)

// Сравнение
10 == "10";       // true (нестрогое)
10 === "10";      // false (строгое)
10 != "10";       // false
10 !== "10";      // true

// Тернарный
condition ? trueValue : falseValue;

// Опциональная цепочка (ES2020)
user?.address?.city; // undefined если что-то отсутствует

// Нулевое слияние (ES2020)
value ?? defaultValue; // берет defaultValue если value null или undefined
```

## 🎯 Работа с данными

### Массивы
```javascript
const arr = [1, 2, 3];

// Основные методы
arr.push(4);           // [1, 2, 3, 4] - добавить в конец
arr.pop();             // [1, 2] - удалить с конца
arr.unshift(0);        // [0, 1, 2, 3] - добавить в начало
arr.shift();           // [2, 3] - удалить с начала
arr.slice(1, 3);       // [2, 3] - часть массива
arr.splice(1, 1, 99);  // [1, 99, 3] - заменить элемент
arr.concat([4, 5]);    // [1, 2, 3, 4, 5] - объединить
arr.includes(2);       // true - проверить наличие
arr.indexOf(2);        // 1 - найти индекс
arr.join('-');         // "1-2-3" - объединить в строку

// Функциональные методы
arr.map(x => x * 2);    // [2, 4, 6]
arr.filter(x => x > 1); // [2, 3]
arr.reduce((sum, x) => sum + x, 0); // 6
arr.find(x => x > 1);   // 2
arr.findIndex(x => x > 1); // 1
arr.some(x => x > 2);   // true
arr.every(x => x < 4);  // true
arr.sort((a, b) => a - b); // сортировка чисел
arr.reverse();          // [3, 2, 1]

// Деструктуризация
const [first, ...rest] = arr; // first = 1, rest = [2, 3]
```

### Объекты
```javascript
const obj = { a: 1, b: 2 };

// Работа с объектами
Object.keys(obj);      // ['a', 'b']
Object.values(obj);    // [1, 2]
Object.entries(obj);   // [['a', 1], ['b', 2]]
Object.assign({}, obj, { c: 3 }); // копирование
{...obj, c: 3};        // spread оператор (ES6)

// Деструктуризация
const { a, b: newName } = obj; // a = 1, newName = 2
const { a: alias = 0 } = obj;  // значение по умолчанию

// Динамические ключи
const key = 'name';
const dynamicObj = { [key]: 'John' }; // { name: 'John' }

// Методы объекта
const person = {
    name: 'John',
    greet() { return `Hello ${this.name}`; }
};
```

### Строки
```javascript
const str = "Hello World";

// Методы строк
str.length;                   // 11
str.toUpperCase();            // "HELLO WORLD"
str.toLowerCase();            // "hello world"
str.includes("World");        // true
str.startsWith("Hello");      // true
str.endsWith("World");        // true
str.indexOf("o");             // 4
str.lastIndexOf("o");         // 7
str.slice(0, 5);              // "Hello"
str.substring(0, 5);          // "Hello"
str.substr(6, 5);             // "World"
str.replace("World", "JS");   // "Hello JS"
str.split(" ");               // ["Hello", "World"]
str.trim();                   // удаляет пробелы с краев
"hello".padStart(10, "*");    // "*****hello"
"hello".padEnd(10, "*");      // "hello*****"

// Шаблонные строки (ES6)
const name = "John";
`Hello ${name}`; // "Hello John"
`Sum: ${1 + 2}`; // "Sum: 3"
```

## 🔄 Функции

### Объявление функций
```javascript
// Function Declaration
function sum(a, b) { return a + b; }

// Function Expression
const sum = function(a, b) { return a + b; };

// Arrow Function (ES6)
const sum = (a, b) => a + b;
const greet = name => `Hello ${name}`;
const noArgs = () => console.log('Hi');
const multiLine = () => {
    const result = 1 + 2;
    return result;
};

// Параметры по умолчанию
function greet(name = "Guest") { return `Hello ${name}`; }

// Rest параметры
function sum(...numbers) {
    return numbers.reduce((acc, n) => acc + n, 0);
}
sum(1, 2, 3); // 6

// Spread оператор
const nums = [1, 2, 3];
Math.max(...nums); // 3
```

### Контекст (this)
```javascript
// Правила определения this:
// 1. Обычная функция: контекст вызова
// 2. Стрелочная функция: контекст объявления
// 3. bind/call/apply: явная установка

const obj = {
    name: 'Object',
    regularFunc: function() { return this.name; },
    arrowFunc: () => this.name
};

// Изменение контекста
func.call(context, arg1, arg2);   // сразу вызывает
func.apply(context, [arg1, arg2]); // сразу вызывает
const bound = func.bind(context);  // возвращает новую функцию
```

### Замыкания
```javascript
function createCounter() {
    let count = 0;
    return function() {
        return ++count;
    };
}

const counter = createCounter();
counter(); // 1
counter(); // 2
```

## 🎭 DOM Манипуляции

### Поиск элементов
```javascript
// Поиск одного элемента
document.getElementById('id');
document.querySelector('.class');
document.querySelector('div > p');

// Поиск нескольких элементов
document.getElementsByClassName('class');   // HTMLCollection
document.getElementsByTagName('div');       // HTMLCollection
document.querySelectorAll('.class');        // NodeList

// Относительный поиск
element.closest('.parent');     // ближайший родитель
element.querySelector('.child'); // внутри элемента
element.parentElement;          // непосредственный родитель
element.children;               // дочерние элементы
element.nextElementSibling;     // следующий сосед
element.previousElementSibling; // предыдущий сосед
```

### Изменение элементов
```javascript
// Содержимое
element.textContent = 'text';   // только текст
element.innerHTML = '<b>text</b>'; // с HTML
element.outerHTML = '<div>text</div>'; // весь элемент

// Атрибуты
element.getAttribute('name');
element.setAttribute('name', 'value');
element.hasAttribute('name');
element.removeAttribute('name');
element.dataset.info = 'value'; // data-атрибуты

// Классы
element.classList.add('class');
element.classList.remove('class');
element.classList.toggle('class');
element.classList.contains('class');
element.classList.replace('old', 'new');

// Стили
element.style.color = 'red';
element.style.cssText = 'color: red; font-size: 20px';
getComputedStyle(element).color; // вычисленные стили
```

### Создание и удаление
```javascript
// Создание
document.createElement('div');
document.createTextNode('text');
document.createDocumentFragment();

// Вставка
parent.appendChild(element);        // в конец
parent.prepend(element);            // в начало
refElement.before(newElement);      // перед
refElement.after(newElement);       // после
element.replaceWith(newElement);    // замена

// Удаление
element.remove();                    // удалить элемент
parent.removeChild(element);         // удалить из родителя
element.innerHTML = '';              // очистить содержимое
```

## ⚡ События

### Обработчики событий
```javascript
// Добавление
element.addEventListener('click', handler, options);
// options: { capture: true, once: true, passive: true }

// Удаление
element.removeEventListener('click', handler);

// Объект события
element.addEventListener('click', (event) => {
    event.target;        // элемент, на котором произошло
    event.currentTarget; // элемент с обработчиком
    event.preventDefault(); // отменить действие по умолчанию
    event.stopPropagation(); // остановить всплытие
    event.stopImmediatePropagation(); // остановить все обработчики
});

// Всплытие и погружение
// событие проходит: window → document → ... → target → ... → window
```

### Делегирование событий
```javascript
// Вместо этого:
items.forEach(item => item.addEventListener('click', handler));

// Делаем так:
container.addEventListener('click', (event) => {
    if (event.target.matches('.item')) {
        handler(event.target);
    }
    // или с closest
    const item = event.target.closest('.item');
    if (item) handler(item);
});
```

### Часто используемые события
```javascript
// Мышь
click, dblclick, mousedown, mouseup, mousemove,
mouseover, mouseout, contextmenu

// Клавиатура
keydown, keyup, keypress

// Формы
submit, input, change, focus, blur

// Загрузка
DOMContentLoaded, load, beforeunload, unload

// Окно
resize, scroll

// Касания
touchstart, touchmove, touchend
```

## 🔄 Асинхронность

### Promise
```javascript
// Создание
const promise = new Promise((resolve, reject) => {
    // асинхронный код
    if (success) resolve(value);
    else reject(error);
});

// Обработка
promise
    .then(value => console.log(value))
    .catch(error => console.error(error))
    .finally(() => console.log('Завершено'));

// Статические методы
Promise.all([p1, p2, p3]);        // все должны выполниться
Promise.allSettled([p1, p2, p3]); // все завершатся (успех/ошибка)
Promise.race([p1, p2, p3]);       // первый завершенный
Promise.any([p1, p2, p3]);        // первый успешный
Promise.resolve(value);            // успешный промис
Promise.reject(error);             // отклоненный промис

// Цепочки
fetch(url)
    .then(r => r.json())
    .then(data => console.log(data))
    .catch(err => console.error(err));
```

### Async/Await
```javascript
// Объявление async функции
async function fetchData() {
    try {
        const response = await fetch(url);
        const data = await response.json();
        return data;
    } catch (error) {
        console.error(error);
        throw error;
    } finally {
        console.log('Завершено');
    }
}

// Параллельное выполнение
const [user, posts] = await Promise.all([
    fetchUser(),
    fetchPosts()
]);

// Обработка ошибок без try/catch
const [error, data] = await promise
    .then(data => [null, data])
    .catch(error => [error, null]);
```

### Fetch API
```javascript
// GET запрос
const response = await fetch('https://api.example.com/data', {
    method: 'GET',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer token'
    }
});

// POST запрос
const response = await fetch(url, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ name: 'John' })
});

// Обработка ответа
if (response.ok) {
    const data = await response.json();    // JSON
    // или
    const text = await response.text();    // текст
    // или
    const blob = await response.blob();    // бинарные данные
} else {
    throw new Error(`HTTP ${response.status}`);
}
```

## 🛠️ Полезные методы

### Массивы
```javascript
// Трансформации
arr.flat();                      // [1, 2, [3, 4]] → [1, 2, 3, 4]
arr.flatMap(x => [x, x * 2]);   // [1, 2] → [1, 2, 2, 4]
arr.fill(0, 2, 4);              // заполнить значениями

// Поиск
arr.findLast(x => x > 1);       // последний подходящий (ES2023)
arr.findLastIndex(x => x > 1);  // индекс последнего (ES2023)

// Копирование
arr.toReversed();               // обратный порядок (без мутации)
arr.toSorted();                 // сортировка (без мутации)
arr.toSpliced(1, 1, 99);       // splice (без мутации)
arr.with(1, 99);                // замена элемента (без мутации)
```

### Объекты
```javascript
// ES6+
Object.fromEntries([['a', 1], ['b', 2]]); // { a: 1, b: 2 }
Object.hasOwn(obj, 'key');                 // проверка свойства
Object.groupBy(items, ({age}) => age > 18 ? 'adult' : 'child');

// Копирование
const shallowCopy = {...obj};
const deepCopy = JSON.parse(JSON.stringify(obj));
const structuredCopy = structuredClone(obj); // ES2022
```

### Числа и даты
```javascript
// Числа
Math.round(1.5);     // 2
Math.floor(1.9);     // 1
Math.ceil(1.1);      // 2
Math.random();       // случайное число 0-1
Number.parseInt('10'); // 10
Number.parseFloat('10.5'); // 10.5
(0.1 + 0.2).toFixed(2); // "0.30"
Number.isInteger(10); // true

// Даты
const now = new Date();
now.getFullYear();     // 2024
now.getMonth();        // 0-11
now.getDate();         // день месяца
now.getDay();          // день недели (0-6)
now.getHours();        // 0-23
now.getTime();         // таймстамп
Date.now();            // текущий таймстамп
new Date().toISOString(); // "2024-01-01T12:00:00.000Z"
```

## 🎨 Современный JavaScript (ES6+)

### Деструктуризация
```javascript
// Массивы
const [first, second, ...rest] = [1, 2, 3, 4];
const [a = 0, b = 0] = [1]; // значения по умолчанию

// Объекты
const { name, age } = user;
const { name: userName, age: userAge } = user;
const { name = 'Guest' } = user; // значение по умолчанию

// В параметрах функции
function print({ name, age }) { console.log(name, age); }
function sum([a, b]) { return a + b; }

// Обмен значений
[a, b] = [b, a];
```

### Spread/Rest операторы
```javascript
// Spread (развернуть)
const arr1 = [1, 2];
const arr2 = [3, 4];
const combined = [...arr1, ...arr2]; // [1, 2, 3, 4]

const obj1 = { a: 1 };
const obj2 = { b: 2 };
const merged = { ...obj1, ...obj2 }; // { a: 1, b: 2 }

// Rest (собрать)
function sum(...numbers) { /* numbers = массив */ }
const [first, ...others] = [1, 2, 3];
const { name, ...rest } = user;
```

### Шорткаты
```javascript
// Опциональная цепочка
user?.address?.street;
arr?.[0];
func?.();

// Нулевое слияние
const value = input ?? 'default';

// Логическое присваивание
a ||= b; // a = a || b
a &&= b; // a = a && b
a ??= b; // a = a ?? b
```

## ⚡ Производительность и оптимизация

### Оптимизация циклов
```javascript
// Кэширование длины массива
for (let i = 0, len = arr.length; i < len; i++) {
    // вместо i < arr.length
}

// Предпочитайте for...of для массивов
for (const item of arr) {
    // вместо for...in
}

// Используйте Set для проверки уникальности
const unique = [...new Set(arr)];

// Деструктуризация в циклах
for (const { id, name } of users) {
    console.log(id, name);
}
```

### Мемоизация
```javascript
function memoize(fn) {
    const cache = new Map();
    return function(...args) {
        const key = JSON.stringify(args);
        if (cache.has(key)) return cache.get(key);
        const result = fn(...args);
        cache.set(key, result);
        return result;
    };
}

const memoizedSum = memoize((a, b) => a + b);
```

### Отложенная загрузка
```javascript
// Динамический импорт
const module = await import('./module.js');

// Ленивая загрузка изображений
const img = new Image();
img.loading = 'lazy';
img.src = 'image.jpg';

// Intersection Observer для lazy loading
const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.src = entry.target.dataset.src;
            observer.unobserve(entry.target);
        }
    });
});
```

## 🛡️ Обработка ошибок

### Try/Catch/Finally
```javascript
try {
    // код, который может выбросить ошибку
    throw new Error('Что-то пошло не так');
} catch (error) {
    console.error('Ошибка:', error.message);
    console.error('Стек:', error.stack);
} finally {
    console.log('Выполняется всегда');
}

// Создание кастомных ошибок
class ValidationError extends Error {
    constructor(message) {
        super(message);
        this.name = 'ValidationError';
    }
}

throw new ValidationError('Неверные данные');
```

### Глобальная обработка ошибок
```javascript
// Непойманные ошибки Promise
window.addEventListener('unhandledrejection', (event) => {
    console.error('Непойманный промис:', event.reason);
});

// Глобальные ошибки
window.addEventListener('error', (event) => {
    console.error('Глобальная ошибка:', event.error);
});

// Ошибки в fetch
fetch(url).catch(error => {
    if (error.name === 'TypeError') {
        console.error('Проблемы с сетью');
    }
    throw error;
});
```

## 📦 Модули ES6

### Экспорт
```javascript
// Именованный экспорт
export const name = 'value';
export function func() {}
export class ClassName {}

// Экспорт по умолчанию
export default function() {}

// Групповой экспорт
export { name, func, ClassName };
export { name as newName };
```

### Импорт
```javascript
// Именованный импорт
import { name, func } from './module.js';
import { name as newName } from './module.js';

// Импорт всего
import * as module from './module.js';

// Импорт по умолчанию
import defaultFunc from './module.js';

// Комбинированный
import defaultFunc, { name } from './module.js';

// Импорт для side effects
import './styles.css';
```

## 🎯 Полезные однострочники

```javascript
// Генерация случайного ID
const id = Math.random().toString(36).substring(2);

// Глубокое копирование объекта
const copy = JSON.parse(JSON.stringify(obj));

// Удаление дубликатов из массива
const unique = [...new Set(arr)];

// Проверка на пустой объект
const isEmpty = obj => Object.keys(obj).length === 0;

// Перемешивание массива
const shuffled = arr.sort(() => Math.random() - 0.5);

// Группировка массива объектов
const grouped = arr.reduce((acc, item) => {
    (acc[item.category] ||= []).push(item);
    return acc;
}, {});

// Форматирование числа с разделителями
const formatted = num.toLocaleString();

// Пауза выполнения
const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));

// Получение параметров URL
const params = new URLSearchParams(window.location.search);
const id = params.get('id');

// Проверка на мобильное устройство
const isMobile = /iPhone|iPad|iPod|Android/i.test(navigator.userAgent);

// Копирование в буфер обмена
const copyToClipboard = text => navigator.clipboard.writeText(text);
```

## 🔧 Отладка и инструменты

### Console методы
```javascript
console.log('обычный вывод');
console.info('информация');
console.warn('предупреждение');
console.error('ошибка');
console.debug('отладка');
console.table([{a:1, b:2}, {a:3, b:4}]);
console.dir(element, { depth: null });
console.time('timer'); console.timeEnd('timer');
console.trace('стек вызовов');
console.group('группа'); console.groupEnd();
console.assert(condition, 'сообщение');
console.count('метка'); // счетчик
```

### Дебаггинг
```javascript
// Брейкпоинты
debugger; // остановка выполнения

// Проверка производительности
const start = performance.now();
// код
const end = performance.now();
console.log(`Выполнено за ${end - start}ms`);
```

## 🚀 Продвинутые концепции

### Generators
```javascript
function* generator() {
    yield 1;
    yield 2;
    yield 3;
}

const gen = generator();
gen.next(); // { value: 1, done: false }
gen.next(); // { value: 2, done: false }
gen.next(); // { value: 3, done: false }
gen.next(); // { value: undefined, done: true }

// Бесконечная последовательность
function* infiniteSequence() {
    let i = 0;
    while (true) yield i++;
}
```

### Proxy
```javascript
const handler = {
    get(target, prop) {
        return prop in target ? target[prop] : 'default';
    },
    set(target, prop, value) {
        if (prop === 'age' && value < 0) {
            throw new Error('Возраст не может быть отрицательным');
        }
        target[prop] = value;
        return true;
    }
};

const person = new Proxy({}, handler);
person.name = 'John';
console.log(person.name); // 'John'
console.log(person.age);  // 'default'
```

### WeakMap/WeakSet
```javascript
// WeakMap (ключи - только объекты, автоматическая сборка мусора)
const weakMap = new WeakMap();
const obj = {};
weakMap.set(obj, 'value');
weakMap.get(obj); // 'value'

// WeakSet (только объекты, уникальные значения)
const weakSet = new WeakSet();
weakSet.add(obj);
weakSet.has(obj); // true
```

## 📚 Ресурсы для продолжения обучения

### Документация
- [MDN Web Docs](https://developer.mozilla.org/ru/docs/Web/JavaScript) - лучшая документация
- [JavaScript.info](https://javascript.info/) - современный учебник
- [ECMAScript спецификация](https://tc39.es/ecma262/) - для глубокого понимания

### Практика
- [Codewars](https://www.codewars.com/) - задачи по JavaScript
- [LeetCode](https://leetcode.com/) - алгоритмические задачи
- [Frontend Mentor](https://www.frontendmentor.io/) - реальные проекты

---

**Запомните:** Лучший способ выучить JavaScript — писать код. Начните с простых проектов, постепенно увеличивая сложность. Не бойтесь экспериментировать и разбирать чужой код. Удачи в изучении! 🚀