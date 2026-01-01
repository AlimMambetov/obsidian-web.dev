## **Теория:**

**Работа с DOM (Document Object Model):**
- DOM — это представление HTML-документа в виде дерева объектов    
- JavaScript может менять этот документ

**Методы для поиска элементов:**
- `document.querySelector('селектор')` — найти первый элемент по CSS-селектору
- `document.getElementById('id')` — найти элемент по id
- `document.querySelectorAll('селектор')` — найти все элементы по селектору

**Работа с содержимым элемента:**
- `element.textContent` — получить или изменить текстовое содержимое
- `element.innerHTML` — получить или изменить HTML-содержимое

**Работа со стилями:**
- `element.style.свойство` — изменить CSS-свойство элемента
- Например: `element.style.color = 'red'`

**Создание элементов:**
- `document.createElement('тег')` — создать новый HTML-элемент
- `element.appendChild(новыйЭлемент)` — добавить элемент в конец другого элемента

**Методы для классов:**
- `element.classList.add('класс')` — добавить класс
- `element.classList.remove('класс')` — удалить класс
- `element.classList.toggle('класс')` — переключить класс

**Вывод в консоль:**
- `console.log(значение)` — вывести информацию в консоль разработчика

---
## **Практика с DOM:**

```html
<body>
    <!-- Создаём заголовок первого уровня -->
    <h1>title</h1>
</body>
```


```css
body {
  font-family: Arial, sans-serif; /* Устанавливаем шрифт для всего документа */
  background: #f0f0f0; /* Задаем цвет фона всей страницы */
}  

h1 {
  color: #333; /* Цвет текста заголовка первого уровня */
  border-bottom: 2px solid #4CAF50; /* Добавляем линию под заголовком*/
  padding-bottom: 4px; /* Внутренний отступ снизу (чтобы текст не прилипал к линии) */
  margin: 50px; /* Внешний отступ со всех сторон заголовка */
}

p {
  color: #666;
}
```


```javascript
// Находим первый элемент <h1> на странице
const title = document.querySelector('h1');

// Выводим найденный элемент в консоль (для проверки)
console.log(title);

// Меняем текст внутри заголовка
title.textContent = 'Привет, JavaScript!';

// Меняем CSS-стили через JavaScript
title.style.color = 'green';

// Создаем новый элемент <p>
const newElement = document.createElement('p');

// Добавляем текст в созданный элемент
newElement.textContent = 'Этот элемент создан с помощью JavaScript!';

// Добавляем созданный элемент в конец <body>
document.body.appendChild(newElement);
```


