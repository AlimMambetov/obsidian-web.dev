## 📌 Document Object Model 

DOM — это программный интерфейс, представляющий HTML-документ в виде дерева объектов, с которым может взаимодействовать JavaScript.

## 🌳 Что такое DOM?

```
document
└── html
    ├── head
    │   ├── meta
    │   └── title
    └── body
        ├── header
        │   └── nav
        ├── main
        │   ├── h1
        │   ├── p
        │   └── div.container
        └── footer
```

Каждый узел — это объект со свойствами и методами.

## 🔍 Поиск элементов в DOM

### Основные методы поиска:

```javascript
// 1. getElementById - ищет по id (возвращает ОДИН элемент)
const header = document.getElementById('header');

// 2. getElementsByClassName - по классу (возвращает КОЛЛЕКЦИЮ)
const buttons = document.getElementsByClassName('btn');

// 3. getElementsByTagName - по тегу (возвращает КОЛЛЕКЦИЮ)
const paragraphs = document.getElementsByTagName('p');

// 4. querySelector - CSS-селектор (первый найденный элемент)
const firstButton = document.querySelector('.btn');
const main = document.querySelector('#main');

// 5. querySelectorAll - все элементы по CSS-селектору (NodeList)
const allButtons = document.querySelectorAll('.btn');
const listItems = document.querySelectorAll('ul li');

// 6. Современные методы (относительный поиск)
const element = document.querySelector('.container');
const children = element.children; // только элементы
const childNodes = element.childNodes; // все узлы (включая текст)
const parent = element.parentElement;
const next = element.nextElementSibling;
const prev = element.previousElementSibling;
```

### Разница между коллекциями:
```javascript
// HTMLCollection (живая коллекция)
const liveCollection = document.getElementsByClassName('item');
console.log(liveCollection.length); // 3

// Добавляем новый элемент
document.body.innerHTML += '<div class="item">Новый</div>';
console.log(liveCollection.length); // 4 (автоматически обновилось!)

// NodeList (обычно не живая)
const nodeList = document.querySelectorAll('.item');
console.log(nodeList.length); // 4

// Добавляем еще элемент
document.body.innerHTML += '<div class="item">Еще один</div>';
console.log(nodeList.length); // 4 (НЕ обновилось!)

// Преобразование в массив
const buttonsArray = Array.from(allButtons);
// или
const buttonsArray2 = [...allButtons];
```

## ✏️ Изменение содержимого и атрибутов

### Работа с содержимым:
```javascript
const element = document.querySelector('.content');

// 1. textContent - только текст (без HTML)
element.textContent = 'Новый текст';
console.log(element.textContent);

// 2. innerHTML - с HTML-разметкой
element.innerHTML = '<strong>Жирный</strong> текст';

// 3. innerText - видимый текст (учитывает CSS)
element.innerText = 'Текст с учетом стилей';

// 4. outerHTML - весь элемент с содержимым
console.log(element.outerHTML);

// 5. value для input, textarea, select
const input = document.querySelector('input');
input.value = 'Значение по умолчанию';
console.log(input.value);
```

### Работа с атрибутами:
```javascript
const img = document.querySelector('img');

// Получение атрибутов
const src = img.getAttribute('src');
const alt = img.alt; // можно и так для стандартных атрибутов

// Установка атрибутов
img.setAttribute('src', 'new-image.jpg');
img.alt = 'Новое описание';

// Проверка наличия
if (img.hasAttribute('data-id')) {
    console.log('Есть data-атрибут');
}

// Удаление атрибута
img.removeAttribute('title');

// data-атрибуты
element.dataset.userId = '123'; // data-user-id
element.dataset.status = 'active'; // data-status
console.log(element.dataset.userId); // '123'
```

### Работа с классами:
```javascript
const div = document.querySelector('div');

// classList API (рекомендуется)
div.classList.add('active', 'highlight'); // добавить классы
div.classList.remove('hidden'); // удалить класс
div.classList.toggle('visible'); // переключить
div.classList.replace('old', 'new'); // заменить

// Проверка наличия класса
if (div.classList.contains('active')) {
    console.log('Элемент активен');
}

// Устаревший способ (не рекомендуется)
div.className = 'new-class'; // заменяет ВСЕ классы
div.className += ' additional'; // добавление класса
```

## 🎨 Изменение стилей

```javascript
const box = document.querySelector('.box');

// 1. Прямое изменение через style
box.style.backgroundColor = 'blue';
box.style.fontSize = '20px';
box.style.marginTop = '10px'; // camelCase вместо kebab-case

// 2. Установка нескольких стилей
box.style.cssText = 'color: red; font-size: 16px; border: 1px solid black;';

// 3. Получение вычисленных стилей
const computedStyle = window.getComputedStyle(box);
console.log(computedStyle.color);
console.log(computedStyle.getPropertyValue('font-size'));

// 4. Работа с CSS-переменными
box.style.setProperty('--main-color', '#ff0000');
const color = box.style.getPropertyValue('--main-color');

// 5. Добавление/удаление CSS-классов (предпочтительный способ!)
box.classList.add('highlighted'); // в CSS: .highlighted { background: yellow; }
```

## 🧩 Создание и удаление элементов

### Создание элементов:
```javascript
// 1. createElement
const newDiv = document.createElement('div');
newDiv.textContent = 'Новый элемент';
newDiv.className = 'my-class';

// 2. createTextNode (чистый текст)
const textNode = document.createTextNode('Простой текст');

// 3. createDocumentFragment (оптимизация для множественных вставок)
const fragment = document.createDocumentFragment();

for (let i = 0; i < 100; i++) {
    const item = document.createElement('li');
    item.textContent = `Элемент ${i}`;
    fragment.appendChild(item);
}

document.querySelector('ul').appendChild(fragment);
```

### Вставка элементов:
```javascript
const container = document.querySelector('.container');
const newElement = document.createElement('p');
newElement.textContent = 'Новый параграф';

// 1. append - в конец (после всех детей)
container.append(newElement);
container.append('Текст', anotherElement); // можно несколько

// 2. prepend - в начало (перед всеми детьми)
container.prepend(newElement);

// 3. before - перед элементом
container.before(newElement);

// 4. after - после элемента
container.after(newElement);

// 5. replaceWith - заменить элемент
oldElement.replaceWith(newElement);

// Устаревшие методы (но все еще используются)
container.appendChild(newElement); // аналог append
container.insertBefore(newElement, referenceElement);
```

### Удаление элементов:
```javascript
const element = document.querySelector('.item');

// 1. remove() - удалить элемент
element.remove();

// 2. removeChild() - удалить дочерний элемент
const parent = element.parentElement;
parent.removeChild(element);

// 3. Очистка всех дочерних элементов
const list = document.querySelector('ul');

// Быстро, но не безопасно для событий
list.innerHTML = '';

// Более безопасно
while (list.firstChild) {
    list.removeChild(list.firstChild);
}

// Через массив
Array.from(list.children).forEach(child => child.remove());
```

## 🎯 Практика: Создание динамического интерфейса

### Проект 1: Динамический список дел
```html
<div class="todo-app">
    <input type="text" id="todo-input" placeholder="Новая задача...">
    <button id="add-btn">Добавить</button>
    <ul id="todo-list"></ul>
</div>
```

```javascript
// Получаем элементы
const input = document.getElementById('todo-input');
const addButton = document.getElementById('add-btn');
const list = document.getElementById('todo-list');

// Массив задач (state)
let todos = [];

// Функция добавления задачи
function addTodo() {
    const text = input.value.trim();
    
    if (text) {
        // Создаем объект задачи
        const todo = {
            id: Date.now(),
            text: text,
            completed: false
        };
        
        // Добавляем в массив
        todos.push(todo);
        
        // Очищаем поле ввода
        input.value = '';
        
        // Отрисовываем список
        renderTodos();
    }
}

// Функция отрисовки списка
function renderTodos() {
    // Очищаем список
    list.innerHTML = '';
    
    // Создаем элементы для каждой задачи
    todos.forEach(todo => {
        const li = document.createElement('li');
        li.className = `todo-item ${todo.completed ? 'completed' : ''}`;
        li.dataset.id = todo.id;
        
        li.innerHTML = `
            <input type="checkbox" ${todo.completed ? 'checked' : ''}>
            <span>${todo.text}</span>
            <button class="delete-btn">×</button>
        `;
        
        list.appendChild(li);
    });
}

// Функция удаления задачи
function deleteTodo(id) {
    todos = todos.filter(todo => todo.id !== id);
    renderTodos();
}

// Функция переключения статуса
function toggleTodo(id) {
    todos = todos.map(todo => 
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
    );
    renderTodos();
}

// Обработчики событий
addButton.addEventListener('click', addTodo);
input.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') addTodo();
});

// Делегирование событий для динамических элементов
list.addEventListener('click', (e) => {
    const li = e.target.closest('.todo-item');
    if (!li) return;
    
    const id = Number(li.dataset.id);
    
    if (e.target.classList.contains('delete-btn')) {
        deleteTodo(id);
    } else if (e.target.type === 'checkbox') {
        toggleTodo(id);
    }
});

// Инициализация
renderTodos();
```

### Проект 2: Модальное окно
```javascript
// HTML для модального окна
const modalHTML = `
    <div class="modal-overlay">
        <div class="modal">
            <div class="modal-header">
                <h2>Заголовок</h2>
                <button class="close-btn">&times;</button>
            </div>
            <div class="modal-body">
                <p>Содержимое модального окна</p>
            </div>
            <div class="modal-footer">
                <button class="btn cancel">Отмена</button>
                <button class="btn confirm">ОК</button>
            </div>
        </div>
    </div>
`;

class Modal {
    constructor(content) {
        this.content = content;
        this.createModal();
        this.bindEvents();
    }
    
    createModal() {
        this.modal = document.createElement('div');
        this.modal.className = 'modal-overlay';
        this.modal.innerHTML = `
            <div class="modal">
                <div class="modal-content">
                    ${this.content}
                </div>
            </div>
        `;
        document.body.appendChild(this.modal);
    }
    
    bindEvents() {
        // Закрытие по клику на оверлей
        this.modal.addEventListener('click', (e) => {
            if (e.target === this.modal) {
                this.close();
            }
        });
        
        // Закрытие по ESC
        document.addEventListener('keydown', (e) => {
            if (e.key === 'Escape') {
                this.close();
            }
        });
    }
    
    open() {
        this.modal.style.display = 'block';
        document.body.style.overflow = 'hidden'; // блокируем скролл
    }
    
    close() {
        this.modal.style.display = 'none';
        document.body.style.overflow = 'auto';
    }
}

// Использование
const myModal = new Modal('<h3>Привет!</h3><p>Это модальное окно</p>');
// myModal.open();
```

## 🛠️ Полезные паттерны и техники

### 1. Шаблонизация с template
```html
<template id="user-template">
    <div class="user-card">
        <img class="avatar" src="" alt="">
        <h3 class="name"></h3>
        <p class="email"></p>
    </div>
</template>
```

```javascript
function createUserCard(user) {
    const template = document.getElementById('user-template');
    const clone = template.content.cloneNode(true);
    
    clone.querySelector('.avatar').src = user.avatar;
    clone.querySelector('.name').textContent = user.name;
    clone.querySelector('.email').textContent = user.email;
    
    return clone;
}
```

### 2. Делегирование событий
```javascript
// ПЛОХО (для каждого элемента)
document.querySelectorAll('.item').forEach(item => {
    item.addEventListener('click', handleClick);
});

// ХОРОШО (одно событие на родителе)
document.querySelector('.container').addEventListener('click', (e) => {
    if (e.target.classList.contains('item')) {
        handleClick(e.target);
    }
});

// С использованием closest
document.querySelector('.container').addEventListener('click', (e) => {
    const item = e.target.closest('.item');
    if (item) {
        handleClick(item);
    }
});
```

### 3. Оптимизация производительности
```javascript
// Троттлинг (не чаще чем раз в N мс)
function throttle(func, delay) {
    let lastCall = 0;
    return function(...args) {
        const now = Date.now();
        if (now - lastCall >= delay) {
            lastCall = now;
            func.apply(this, args);
        }
    };
}

window.addEventListener('scroll', throttle(handleScroll, 100));

// Дебаунсинг (последний вызов через N мс)
function debounce(func, delay) {
    let timeout;
    return function(...args) {
        clearTimeout(timeout);
        timeout = setTimeout(() => func.apply(this, args), delay);
    };
}

input.addEventListener('input', debounce(handleInput, 300));
```

### 4. Работа с формами
```javascript
const form = document.querySelector('form');

// Получение данных формы
form.addEventListener('submit', (e) => {
    e.preventDefault();
    
    // 1. Через FormData
    const formData = new FormData(form);
    const data = Object.fromEntries(formData);
    
    // 2. Через элементы
    const data = {
        name: form.querySelector('[name="name"]').value,
        email: form.querySelector('[name="email"]').value
    };
    
    // Валидация
    if (!validateForm(data)) {
        showErrors();
        return;
    }
    
    // Отправка данных
    sendData(data);
});

// Динамическая валидация
form.addEventListener('input', (e) => {
    const input = e.target;
    const errorElement = input.nextElementSibling;
    
    if (!input.checkValidity()) {
        errorElement.textContent = input.validationMessage;
        input.classList.add('invalid');
    } else {
        errorElement.textContent = '';
        input.classList.remove('invalid');
    }
});
```

## 🎯 Упражнения для закрепления

### Задание 1: Галерея изображений
Создайте галерею, где:
1. Показывается одно большое изображение
2. Под ним — миниатюры
3. При клике на миниатюру большое изображение меняется
4. Добавьте кнопки "вперед/назад"

### Задание 2: Аккордеон (раскрывающийся список)
```html
<div class="accordion">
    <div class="accordion-item">
        <button class="accordion-header">Раздел 1</button>
        <div class="accordion-content">Содержимое 1</div>
    </div>
    <!-- больше элементов -->
</div>
```

### Задание 3: Динамический фильтр товаров
Создайте список товаров с фильтрами по:
- Цене (диапазон)
- Категории
- Наличию на складе

## 📚 Что дальше?

После освоения DOM:
1. **События (Events)** — детальное изучение Event API
2. **Асинхронность** — работа с API, Promise, async/await
3. **Модули** — организация кода в ES6 модули
4. **Браузерные API** — LocalStorage, History, Geolocation

---

**Совет:** Создавайте реальные мини-проекты! Начните с простого To-Do приложения, затем сделайте модальное окно, слайдер, форму с валидацией. Практика — ключ к пониманию DOM.