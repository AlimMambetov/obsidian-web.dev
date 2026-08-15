---
title: Структура компонентов
---
# 📁 Архитектура компонентов в проекте

## Общая структура

```
components/
├── common/      # Базовые атомарные компоненты
├── ui/          # Элементы интерфейса и формы
├── core/        # Стандартные UI-компоненты
├── modules/     # Бизнес-модули со сложной логикой
├── layout/      # Структурные компоненты
└── templates/   # Секции и большие блоки
```

---

## 📦 Что где лежит

| Папка | Что хранит | Примеры |
|-------|------------|---------|
| **common/** | Базовые кирпичики, без логики | Container, Icon, Typography, Spinner, Image |
| **ui/** | Элементы форм и ввода | Button, Input, Checkbox, Radio, Select, Link |
| **core/** | Стандартные UI-компоненты | Card, Modal, Alert, Toast, Accordion, Slider |
| **modules/** | Бизнес-модули со сложной JS-логикой | AuthBlock, ContactForm, ProductCard, ShoppingCart |
| **layout/** | Структурные компоненты | Header, Footer, Sidebar, Navigation |
| **templates/** | Секции и большие блоки | HeroSection, AboutSection, FeaturesSection |

---

## 🔗 Правила импорта (иерархия)

```
templates/  ← может импортировать всё (common, ui, core, modules, layout)
    ↑
layout/     ← может импортировать common, ui, core, modules
    ↑
modules/    ← может импортировать common, ui, core
    ↑
core/       ← может импортировать common, ui
    ↑
ui/         ← может импортировать только common
    ↑
common/     ← не может импортировать ничего
```

### Таблица зависимостей

| Папка | Можно импортировать | Нельзя импортировать |
|-------|-------------------|---------------------|
| **common/** | ❌ ничего | всё |
| **ui/** | ✅ common | core, modules, layout, templates, ui |
| **core/** | ✅ common, ui | modules, layout, templates, core |
| **modules/** | ✅ common, ui, core | layout, templates, modules |
| **layout/** | ✅ common, ui, core, modules | templates, layout |
| **templates/** | ✅ common, ui, core, modules, layout | templates |

---

## 📝 Примеры использования

### ✅ Правильно

```jsx
// common/ — не импортирует ничего
export function Container({ children }) {
  return <div className="container">{children}</div>;
}
```

```jsx
// ui/ — импортирует только common
import { Typography, Icon } from '@/components/common';

export function Button({ children, icon }) {
  return (
    <button>
      {icon && <Icon name={icon} />}
      <Typography>{children}</Typography>
    </button>
  );
}
```

```jsx
// core/ — импортирует common + ui
import { Container, Typography } from '@/components/common';
import { Button } from '@/components/ui';

export function Card({ title, children, action }) {
  return (
    <Container>
      <Typography variant="h4">{title}</Typography>
      {children}
      {action && <Button>{action}</Button>}
    </Container>
  );
}
```

```jsx
// modules/ — импортирует common + ui + core
import { Container, Typography } from '@/components/common';
import { Button, Input } from '@/components/ui';
import { Card, Alert } from '@/components/core';

export function AuthBlock() {
  const [error, setError] = useState(null);
  
  const handleSubmit = async () => {
    // Сложная бизнес-логика
    try {
      await login();
    } catch (err) {
      setError(err.message);
    }
  };

  return (
    <Card title="Вход">
      {error && <Alert type="error" message={error} />}
      <Input placeholder="Email" />
      <Input type="password" placeholder="Пароль" />
      <Button onClick={handleSubmit}>Войти</Button>
    </Card>
  );
}
```

```jsx
// layout/ — импортирует common + ui + core + modules
import { Container, Typography } from '@/components/common';
import { Button } from '@/components/ui';
import { Card } from '@/components/core';
import { AuthBlock } from '@/components/modules';

export function Header() {
  return (
    <header>
      <Container>
        <Typography variant="h1">Лого</Typography>
        <Button>Войти</Button>
        <AuthBlock />
      </Container>
    </header>
  );
}
```

```jsx
// templates/ — импортирует всё!
import { Container, Typography } from '@/components/common';
import { Button } from '@/components/ui';
import { Card } from '@/components/core';
import { Header, Footer } from '@/components/layout';
import { AuthBlock } from '@/components/modules';

export function HeroSection() {
  return (
    <>
      <Header />
      <Container>
        <Typography variant="h1">Добро пожаловать</Typography>
        <Button>Начать</Button>
        <Card>
          <AuthBlock />
        </Card>
      </Container>
      <Footer />
    </>
  );
}
```

---

### ❌ Неправильно (запрещено)

```jsx
// ❌ common не может импортировать ничего
import { Button } from '@/components/ui'; // ЗАПРЕЩЕНО!
```

```jsx
// ❌ ui не может импортировать core
import { Card } from '@/components/core'; // ЗАПРЕЩЕНО!
```

```jsx
// ❌ core не может импортировать modules
import { AuthBlock } from '@/components/modules'; // ЗАПРЕЩЕНО!
```

```jsx
// ❌ modules не может импортировать layout
import { Header } from '@/components/layout'; // ЗАПРЕЩЕНО!
```

```jsx
// ❌ layout не может импортировать layout
import { Footer } from '@/components/layout'; // ЗАПРЕЩЕНО!
```

```jsx
// ❌ templates не может импортировать templates
import { AboutSection } from '@/components/templates'; // ЗАПРЕЩЕНО!
```

---

## 🎯 Краткое резюме

### Иерархия (от простого к сложному):
```
common → ui → core → modules → layout → templates
```

### Золотые правила:
1. **common** — НЕ импортирует ничего
2. **ui** — импортирует только common
3. **core** — импортирует common + ui
4. **modules** — импортирует common + ui + core
5. **layout** — импортирует common + ui + core + modules
6. **templates** — импортирует всё!

### Важно:
- ✅ Двигаться можно только **СНИЗУ ВВЕРХ**
- ✅ Нельзя импортировать компоненты из той же папки
- ✅ Нельзя импортировать из папок, которые находятся выше по иерархии

---

## 💡 Итог

Эта архитектура обеспечивает:
- ✅ **Чистоту кода** — каждый компонент знает своё место
- ✅ **Переиспользуемость** — компоненты легко использовать повторно
- ✅ **Масштабируемость** — легко добавлять новые компоненты
- ✅ **Поддерживаемость** — понятно, где что искать и как использовать

**Главное правило**: компоненты могут использовать только те, что находятся **НИЖЕ** по иерархии! 🚀