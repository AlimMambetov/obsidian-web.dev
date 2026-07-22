## 📌 Работа с событиями

События — это основа интерактивности веб-страниц. JavaScript может реагировать на действия пользователя и изменения в документе.

## 🎯 Основы событий

### Типы событий:
```javascript
// События мыши
click       // клик (mousedown + mouseup)
dblclick    // двойной клик
mousedown   // нажатие кнопки мыши
mouseup     // отпускание кнопки мыши
mousemove   // движение мыши
mouseover   // курсор над элементом
mouseout    // курсор покинул элемент
contextmenu // клик правой кнопкой

// События клавиатуры
keydown     // нажатие клавиши
keyup       // отпускание клавиши
keypress    // нажатие символьной клавиши (устарело)

// События форм
submit      // отправка формы
input       // изменение значения
change      // изменение и потеря фокуса
focus       // получение фокуса
blur        // потеря фокуса

// События загрузки
DOMContentLoaded // DOM загружен
load           // вся страница загружена
beforeunload   // перед закрытием страницы

// События окна
resize        // изменение размеров окна
scroll        // прокрутка
```

## 🎪 Добавление обработчиков событий

### 1. Свойства on* (не рекомендуется для сложных случаев)
```javascript
const button = document.querySelector('button');

// Прямое присваивание (перезаписывает предыдущие)
button.onclick = function() {
    console.log('Клик 1');
};

// Еще одно присваивание перезапишет предыдущее
button.onclick = function() {
    console.log('Клик 2'); // Будет вызвано только это
};

// Событие только всплывает
button.onclick = null; // Удаление обработчика
```

### 2. addEventListener (рекомендуется)
```javascript
const button = document.querySelector('button');

// Добавление нескольких обработчиков
button.addEventListener('click', function() {
    console.log('Клик 1');
});

button.addEventListener('click', function() {
    console.log('Клик 2'); // Выполнятся оба
});

// Удаление обработчика (нужна ссылка на функцию)
function handleClick() {
    console.log('Обработчик');
}

button.addEventListener('click', handleClick);
button.removeEventListener('click', handleClick); // Удаление

// Один раз
button.addEventListener('click', function() {
    console.log('Сработает только один раз');
}, { once: true });
```

### 3. Объект события (Event)
```javascript
element.addEventListener('click', function(event) {
    // event - объект события
    console.log(event.type);      // "click"
    console.log(event.target);    // элемент, на котором произошло событие
    console.log(event.currentTarget); // элемент с обработчиком
    console.log(event.clientX);   // координата X курсора
    console.log(event.clientY);   // координата Y курсора
    console.log(event.key);       // для клавиатурных событий
    console.log(event.preventDefault); // метод для отмены действия
    console.log(event.stopPropagation); // метод для остановки всплытия
});
```

## 🌊 Всплытие и погружение (Bubbling & Capturing)

```
       [1. Погружение (capturing phase)]
       ↓
Дед → Родитель → Ребенок [target]
       ↑
       [3. Всплытие (bubbling phase)]
```

```javascript
// HTML: <div class="grandparent"><div class="parent"><button>Клик</button></div></div>

const grandparent = document.querySelector('.grandparent');
const parent = document.querySelector('.parent');
const button = document.querySelector('button');

// По умолчанию: обработка на стадии всплытия
button.addEventListener('click', (e) => {
    console.log('Кнопка (всплытие)');
});

parent.addEventListener('click', (e) => {
    console.log('Родитель (всплытие)');
});

grandparent.addEventListener('click', (e) => {
    console.log('Дед (всплытие)');
});

document.addEventListener('click', (e) => {
    console.log('Document (всплытие)');
});

// При клике на кнопку выведется:
// Кнопка (всплытие)
// Родитель (всплытие)
// Дед (всплытие)
// Document (всплытие)
```

### Управление фазой:
```javascript
// Погружение (capture: true)
element.addEventListener('click', handler, { capture: true });
// или
element.addEventListener('click', handler, true);

// При клике на кнопку с capture: true на родителе:
// Родитель (погружение) ← сначала
// Кнопка (всплытие)
// Дед (всплытие)
```

### Остановка всплытия:
```javascript
button.addEventListener('click', (e) => {
    e.stopPropagation(); // Остановить всплытие
    console.log('Кнопка');
    // Родитель и дед не получат событие
});

// stopImmediatePropagation - остановить и другие обработчики
button.addEventListener('click', (e) => {
    e.stopImmediatePropagation(); // Этот и другие обработчики не сработают
    console.log('Первый обработчик');
});

button.addEventListener('click', () => {
    console.log('Второй обработчик'); // Не выполнится
});
```

### Отмена стандартного поведения:
```javascript
// Отмена отправки формы
form.addEventListener('submit', (e) => {
    e.preventDefault();
    // форма не отправится
});

// Отмена контекстного меню
element.addEventListener('contextmenu', (e) => {
    e.preventDefault();
});

// Отмена перехода по ссылке
link.addEventListener('click', (e) => {
    if (!confirm('Перейти?')) {
        e.preventDefault();
    }
});
```

## 🎭 Делегирование событий

Паттерн для работы с динамическими элементами:

```javascript
// HTML: <ul id="list"><li>Элемент 1</li><li>Элемент 2</li></ul>

const list = document.getElementById('list');

// ПЛОХО: обработчик на каждый элемент
document.querySelectorAll('#list li').forEach(li => {
    li.addEventListener('click', () => {
        console.log(li.textContent);
    });
});

// Проблема: новые li не получат обработчик

// ХОРОШО: делегирование (один обработчик на родителе)
list.addEventListener('click', (e) => {
    // Проверяем, кликнули ли на li
    if (e.target.tagName === 'LI') {
        console.log(e.target.textContent);
    }
});

// Еще лучше с closest
list.addEventListener('click', (e) => {
    const listItem = e.target.closest('li');
    if (listItem && list.contains(listItem)) {
        console.log(listItem.textContent);
    }
});

// Динамическое добавление элементов работает!
const newLi = document.createElement('li');
newLi.textContent = 'Новый элемент';
list.appendChild(newLi); // Будет работать с делегированием
```

### Практический пример: Таблица с действиями
```html
<table id="users">
    <thead>
        <tr><th>Имя</th><th>Email</th><th>Действия</th></tr>
    </thead>
    <tbody>
        <tr data-id="1">
            <td>Иван</td>
            <td>ivan@example.com</td>
            <td>
                <button class="edit-btn">✏️</button>
                <button class="delete-btn">🗑️</button>
            </td>
        </tr>
    </tbody>
</table>
```

```javascript
const table = document.getElementById('users');

table.addEventListener('click', (e) => {
    const row = e.target.closest('tr[data-id]');
    if (!row) return;
    
    const userId = row.dataset.id;
    
    if (e.target.classList.contains('edit-btn')) {
        editUser(userId);
    } else if (e.target.classList.contains('delete-btn')) {
        deleteUser(userId);
    }
});
```

## ⏳ События загрузки страницы

```javascript
// 1. DOMContentLoaded - DOM построен, но ресурсы могут еще грузиться
document.addEventListener('DOMContentLoaded', () => {
    console.log('DOM готов');
    // Можно безопасно обращаться к элементам
});

// 2. load - вся страница загружена (включая картинки, стили)
window.addEventListener('load', () => {
    console.log('Страница полностью загружена');
});

// 3. beforeunload - перед закрытием страницы
window.addEventListener('beforeunload', (e) => {
    // Отмена закрытия
    e.preventDefault();
    e.returnValue = ''; // Для совместимости
    return 'Вы точно хотите уйти?';
});

// 4. Порядок событий
// DOMContentLoaded → load → beforeunload
```

## 🌟 Пользовательские события

```javascript
// Создание пользовательского события
const event = new Event('myEvent', {
    bubbles: true,    // будет всплывать
    cancelable: true, // можно отменить
    composed: true    // пройдет через Shadow DOM
});

// Событие с данными (CustomEvent)
const customEvent = new CustomEvent('userAction', {
    detail: {         // дополнительные данные
        userId: 123,
        action: 'login'
    },
    bubbles: true
});

// Диспетчеризация события
element.dispatchEvent(customEvent);

// Подписка на пользовательское событие
element.addEventListener('userAction', (e) => {
    console.log(e.detail.userId); // 123
});

// Пример: система уведомлений
class EventBus {
    constructor() {
        this.listeners = {};
    }
    
    on(event, callback) {
        if (!this.listeners[event]) {
            this.listeners[event] = [];
        }
        this.listeners[event].push(callback);
    }
    
    off(event, callback) {
        if (!this.listeners[event]) return;
        this.listeners[event] = this.listeners[event].filter(cb => cb !== callback);
    }
    
    emit(event, data) {
        if (!this.listeners[event]) return;
        this.listeners[event].forEach(callback => callback(data));
    }
}

// Использование
const bus = new EventBus();
bus.on('notification', (message) => {
    showNotification(message);
});
bus.emit('notification', 'Новое сообщение!');
```

## 📊 События форм

```javascript
const form = document.querySelector('form');
const input = document.querySelector('input');

// input - при каждом изменении
input.addEventListener('input', (e) => {
    console.log('Текущее значение:', e.target.value);
    // Идеально для live-валидации
});

// change - после изменения и потери фокуса
input.addEventListener('change', (e) => {
    console.log('Финальное значение:', e.target.value);
});

// focus/blur
input.addEventListener('focus', () => {
    input.style.borderColor = 'blue';
});

input.addEventListener('blur', () => {
    input.style.borderColor = '#ccc';
    validateInput(input);
});

// submit
form.addEventListener('submit', async (e) => {
    e.preventDefault();
    
    // Сбор данных формы
    const formData = new FormData(form);
    const data = Object.fromEntries(formData);
    
    // Валидация
    if (!validateForm(data)) {
        showErrors();
        return;
    }
    
    // Показать индикатор загрузки
    showLoader();
    
    try {
        // Отправка данных
        const response = await fetch('/api/submit', {
            method: 'POST',
            body: JSON.stringify(data),
            headers: { 'Content-Type': 'application/json' }
        });
        
        if (response.ok) {
            showSuccess();
            form.reset();
        } else {
            throw new Error('Ошибка сервера');
        }
    } catch (error) {
        showError(error.message);
    } finally {
        hideLoader();
    }
});

// Валидация в реальном времени
input.addEventListener('input', debounce(() => {
    validateField(input);
}, 300));
```

## 🔄 События клавиатуры

```javascript
document.addEventListener('keydown', (e) => {
    console.log(`Клавиша: ${e.key}, Код: ${e.code}`);
    
    // Комбинации клавиш
    if (e.ctrlKey && e.key === 's') {
        e.preventDefault(); // Отменяем сохранение страницы
        saveDocument();
    }
    
    if (e.key === 'Escape') {
        closeModal();
    }
    
    if (e.key === 'Enter' && e.ctrlKey) {
        submitForm();
    }
    
    // Навигация стрелками
    if (e.key === 'ArrowUp') {
        moveUp();
    } else if (e.key === 'ArrowDown') {
        moveDown();
    }
});

// Пример: горячие клавиши в приложении
class KeyboardShortcuts {
    constructor() {
        this.shortcuts = new Map();
        this.setup();
    }
    
    setup() {
        document.addEventListener('keydown', this.handleKeydown.bind(this));
    }
    
    register(combination, callback, description = '') {
        this.shortcuts.set(combination, { callback, description });
    }
    
    handleKeydown(e) {
        const combination = this.getCombination(e);
        const shortcut = this.shortcuts.get(combination);
        
        if (shortcut && !this.isInputFocused()) {
            e.preventDefault();
            shortcut.callback();
        }
    }
    
    getCombination(e) {
        const parts = [];
        if (e.ctrlKey) parts.push('Ctrl');
        if (e.altKey) parts.push('Alt');
        if (e.shiftKey) parts.push('Shift');
        parts.push(e.key.toUpperCase());
        return parts.join('+');
    }
    
    isInputFocused() {
        const active = document.activeElement;
        return active.tagName === 'INPUT' || 
               active.tagName === 'TEXTAREA' ||
               active.isContentEditable;
    }
}

// Использование
const shortcuts = new KeyboardShortcuts();
shortcuts.register('Ctrl+S', () => save());
shortcuts.register('Ctrl+Z', () => undo());
shortcuts.register('Ctrl+Shift+P', () => print());
```

## 🎮 События мыши

```javascript
const element = document.querySelector('.draggable');

element.addEventListener('mousedown', startDrag);

function startDrag(e) {
    e.preventDefault();
    
    const startX = e.clientX;
    const startY = e.clientY;
    const elementX = element.offsetLeft;
    const elementY = element.offsetTop;
    
    function onMouseMove(e) {
        const dx = e.clientX - startX;
        const dy = e.clientY - startY;
        
        element.style.left = `${elementX + dx}px`;
        element.style.top = `${elementY + dy}px`;
    }
    
    function onMouseUp() {
        document.removeEventListener('mousemove', onMouseMove);
        document.removeEventListener('mouseup', onMouseUp);
    }
    
    document.addEventListener('mousemove', onMouseMove);
    document.addEventListener('mouseup', onMouseUp);
}

// Координаты
element.addEventListener('click', (e) => {
    console.log('Client:', e.clientX, e.clientY); // относительно окна
    console.log('Page:', e.pageX, e.pageY);       // относительно документа
    console.log('Screen:', e.screenX, e.screenY); // относительно экрана
    console.log('Offset:', e.offsetX, e.offsetY); // относительно элемента
});
```

## 🚀 Паттерны и лучшие практики

### 1. Пассивные обработчики (для производительности)
```javascript
// Без passive: true браузер ждет, не вызовется ли preventDefault()
window.addEventListener('scroll', () => {
    // Прокрутка может быть не такой плавной
});

// С passive: true - говорим браузеру, что не будем отменять прокрутку
window.addEventListener('scroll', () => {
    // Более плавная прокрутка
}, { passive: true });

// Особенно важно для touch событий на мобильных
element.addEventListener('touchstart', handler, { passive: true });
```

### 2. Оптимизация с requestAnimationFrame
```javascript
let ticking = false;

window.addEventListener('scroll', () => {
    if (!ticking) {
        requestAnimationFrame(() => {
            doSomething(); // Выполнится на следующем кадре анимации
            ticking = false;
        });
        ticking = true;
    }
});
```

### 3. Управление памятью
```javascript
// Удаление обработчиков для предотвращения утечек памяти
class Component {
    constructor(element) {
        this.element = element;
        this.handleClick = this.handleClick.bind(this);
        this.element.addEventListener('click', this.handleClick);
    }
    
    handleClick() {
        console.log('Клик');
    }
    
    destroy() {
        this.element.removeEventListener('click', this.handleClick);
        this.element = null;
    }
}
```

### 4. Отложенная инициализация
```javascript
// Инициализация компонентов только когда они в viewport
const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            initializeComponent(entry.target);
            observer.unobserve(entry.target);
        }
    });
});

document.querySelectorAll('.lazy-component').forEach(el => {
    observer.observe(el);
});
```


## 📚 Что дальше?

После освоения событий:
1. **Асинхронность** — Promise, async/await, работа с API
2. **Работа с сервером** — Fetch API, XMLHttpRequest, WebSocket
3. **Хранение данных** — LocalStorage, SessionStorage, IndexedDB
4. **Браузерные API** — Geolocation, Notification, Canvas

---

**Важно:** События — это основа интерактивности. Практикуйтесь, создавая интерактивные компоненты: слайдеры, аккордеоны, модальные окна, drag-and-drop. Чем больше реальных проектов сделаете, тем лучше поймете работу событий в JavaScript.