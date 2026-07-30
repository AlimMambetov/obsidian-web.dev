---
title: Vite | Landing Page
---
# Комментарии и рекомендации

## 1. В сборке не работают переменные scss в модульных стилях

![[Pasted image 20260730210318.png]]

### Решение: 

замени свой vite.config.js на этот код

```js
import { defineConfig } from 'vite'
import preact from '@preact/preset-vite'
import path from 'path'
import fs from 'fs';
const __dirname = import.meta.dirname;

  

function getGlobalStyles() {
  const globalDir = path.resolve(__dirname, './src/styles/global');
  const files = fs.readdirSync(globalDir);
  let imports = '';

  for (const file of files) {
    if (file.endsWith('.scss') && file !== 'index.scss') {
      imports += `@use "@/styles/global/${file}" as *;\n`;
    }
  }

  return imports;
}

  
export default defineConfig({
  plugins: [preact()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '&': path.resolve(__dirname, './public'),
    }
  },
  css: {
    preprocessorOptions: {
      scss: {
        additionalData: getGlobalStyles()
      }
    }
  }
})
``` 


теперь тебе не нужно указывать в каждом модульном компоненте @use '../../styles/variables' as *;
любые файлы стилей которые ты будешь создавать в папке @/styles/global будут автоматически подключаться в твои модульные стили.
![[Pasted image 20260730214315.png]]


## 2. В таких компонентах лучше просто сделать главный файл index и он будет в попадать в import по умолчанию


![[Pasted image 20260730214721.png]]


### Решение: 

![[Pasted image 20260730215020.png]]

1. Удали index.js
2. Переименуй стили в общее название для всех модульных компонентов если стилей там один файл в "styles.module.scss"
3. и так как мы подключили alias лучше делать import через @ | import { Header } from '@/components/Header'

![[Pasted image 20260730215243.png]]

Когда импортируешь стили ты называешь из styles, как по мне это длинное и не удобное слово, лучше заменить на cls, легко писать укоряет процесс верстки.