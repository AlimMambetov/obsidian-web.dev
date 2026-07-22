## **Теория:**
---
### **Объекты**

**Объекты** - коллекция пар `ключ: значение`:
```js
const user = {
    name: "Иван",
    age: 25,
    isActive: true
};
```

### **localStorage**

**localStorage** - хранилище в браузере, позволяет хранить в памяти браузера данные:
- `localStorage.setItem(key, value)` - сохранить
- `localStorage.getItem(key)` - получить
- localStorage хранит только строки    

### **JSON**

**Формат JSON** (JavaScript Object Notation) — текстовый формат для обмена данными. Похож на объекты JavaScript, но это строка:
- `JSON.stringify()` - объект в строку
- `JSON.parse()` - строку в объект

### **Data-атрибуты и dataset:**

Data-атрибуты — специальные HTML-атрибуты, начинающиеся с `data-`, которые позволяют хранить дополнительную информацию в элементах.

**В HTML:** `<button data-id="123" data-action="delete">`  
**В JavaScript:** `элемент.dataset.id` вернёт "123"

**Преимущества:**
- Не мешают стандартным атрибутам
- Легко получать значения через JavaScript
- Позволяют связывать данные с элементами

###  **Работа с датами**

`Date.now()` возвращает текущее время в миллисекундах (количество миллисекунд с 1 января 1970 года). Это удобно для создания уникальных идентификаторов.

`new Date().toLocaleDateString()` возвращает текущую дату в формате, понятном пользователю (например, "01.01.2024").

### **Диалоговые окна confirm() и alert()**

`confirm("сообщение")` — показывает диалоговое окно с кнопками "OK" и "Отмена". Возвращает `true`, если пользователь нажал "OK", и `false`, если "Отмена".

`alert("сообщение")` — простое информационное окно с кнопкой "OK".


## **Практика с DOM:**
---
### HTML
```html
<body>
	<h1>📝 Заметки</h1>
	<div class="notes-container">
	<input type="text" id="noteInput" placeholder="Введите заметку...">
	<button id="addNoteBtn">Добавить</button>
	<div id="notesList"></div>
	</div>
	<button id="clearBtn">Очистить всё</button>
</body>
```

### CSS
```css
body {
    font-family: Arial, sans-serif;
    max-width: 500px;
    margin: 0 auto;
    padding: 20px;
}

.notes-container {
    margin-bottom: 20px;
}

#noteInput {
    width: 70%;
    padding: 8px;
    margin-right: 10px;
}

button {
    padding: 8px 16px;
    cursor: pointer;
}

#addNoteBtn {
    background: #4CAF50;
    color: white;
    border: none;
}

#clearBtn {
    background: #f44336;
    color: white;
    border: none;
    margin-top: 20px;
}

.note {
    background: #f9f9f9;
    padding: 10px;
    margin: 10px 0;
    border-left: 4px solid #4CAF50;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.delete-btn {
    background: #ff9800;
    color: white;
    border: none;
    padding: 5px 10px;
    cursor: pointer;
}
```

### JS
```js
// Элементы
const noteInput = document.getElementById('noteInput');
const addNoteBtn = document.getElementById('addNoteBtn');
const notesList = document.getElementById('notesList');
const clearBtn = document.getElementById('clearBtn');

// Загружаем заметки из localStorage или создаём пустой массив
let notes = JSON.parse(localStorage.getItem('notes')) || [];

// Показываем заметки при загрузке
renderNotes();

// Добавление заметки
addNoteBtn.addEventListener('click', addNote);

// Enter тоже добавляет заметку
noteInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') addNote();
});

// Очистка всех заметок
clearBtn.addEventListener('click', () => {
    if (confirm('Удалить все заметки?')) {
        notes = [];
        saveNotes();
        renderNotes();
    }
});

function addNote() {
    const text = noteInput.value.trim();
    if (!text) return; // Если пусто - выходим
    
    // Создаём объект заметки
    const newNote = {
        id: Date.now(), // Уникальный ID по времени
        text: text,
        date: new Date().toLocaleDateString() // Текущая дата
    };
    
    notes.push(newNote); // Добавляем в массив
    noteInput.value = ''; // Очищаем поле ввода
    saveNotes(); // Сохраняем в localStorage
    renderNotes(); // Обновляем список
}

function renderNotes() {
    notesList.innerHTML = ''; // Очищаем список
    
    // Если заметок нет
    if (notes.length === 0) {
        notesList.innerHTML = '<p style="color: #666;">Заметок пока нет</p>';
        return;
    }
    
    // Создаём элементы для каждой заметки
    notes.forEach(note => {
        const noteElement = document.createElement('div');
        noteElement.className = 'note';
        
        noteElement.innerHTML = `
            <div>
                <strong>${note.text}</strong>
                <br>
                <small style="color: #666;">${note.date}</small>
            </div>
            <button class="delete-btn" data-id="${note.id}">Удалить</button>
        `;
        
        notesList.appendChild(noteElement);
    });
    
    // Вешаем обработчики на кнопки удаления
    document.querySelectorAll('.delete-btn').forEach(btn => {
        btn.addEventListener('click', (e) => {
            const id = Number(e.target.dataset.id);
            deleteNote(id);
        });
    });
}

function deleteNote(id) {
    // Удаляем заметку по ID
    notes = notes.filter(note => note.id !== id);
    saveNotes();
    renderNotes();
}

function saveNotes() {
    // Сохраняем массив в localStorage
    localStorage.setItem('notes', JSON.stringify(notes));
}
```