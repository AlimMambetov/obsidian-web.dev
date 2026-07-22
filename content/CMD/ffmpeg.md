# FFmpeg — сжатие видео (Windows cmd)

## Основные параметры

| Параметр               | Что значит                                                      |
| ---------------------- | --------------------------------------------------------------- |
| `-vcodec libx264`      | Кодек H.264 (максимальная совместимость)                        |
| `-crf 23`              | Качество: 18 (отлично) → 23 (хорошо) → 28 (средне) → 32 (плохо) |
| `-preset medium`       | Скорость: ultrafast / fast / medium / slow / veryslow           |
| `-pix_fmt yuv420p`     | Цвет (совместимость со всем)                                    |
| `-c:a aac`             | Аудиокодек                                                      |
| `-b:a 128k`            | Битрейт аудио                                                   |
| `-movflags +faststart` | Для веба — быстрое начало                                       |
| `-y`                   | Перезаписывать без вопросов                                     |

## Режимы сжатия

| Режим            | CRF | Preset   | Аудио | Размер  | Для чего         |
| ---------------- | --- | -------- | ----- | ------- | ---------------- |
| Высокое качество | 18  | slow     | 192k  | большой | архив, монтаж    |
| Средний (баланс) | 23  | medium   | 128k  | 40-50%  | YouTube, соцсети |
| Сильное сжатие   | 28  | fast     | 96k   | 15-25%  | экономия места   |
| Экстремальное    | 32  | veryfast | 64k   | ~10%    | лекции, экран    |

---
## Одиночные файлы

**Высокое качество**
```js
ffmpeg -i input.mp4 -vcodec libx264 -crf 18 -preset slow -pix_fmt yuv420p -c:a aac -b:a 192k output.mp4
```

**Среднее сжатие**
```js
ffmpeg -i input.mp4 -vcodec libx264 -crf 23 -preset medium -pix_fmt yuv420p -c:a aac -b:a 128k -movflags +faststart output.mp4
```

**Сильное сжатие**
```js
ffmpeg -i input.mp4 -vcodec libx264 -crf 28 -preset fast -pix_fmt yuv420p -c:a aac -b:a 96k output.mp4
```

**Экстремальное сжатие**
```js
ffmpeg -i input.mp4 -vcodec libx264 -crf 32 -preset veryfast -pix_fmt yuv420p -c:a aac -b:a 64k -ac 1 output.mp4
```

---
## Циклы для всех файлов (создают новые файлы)

**Высокое качество** (high_*.mp4)
```js
for %i in (*.mp4) do ffmpeg -i "%i" -vcodec libx264 -crf 18 -preset slow -pix_fmt yuv420p -c:a aac -b:a 192k "high_%~ni.mp4"
```

**Среднее сжатие** (optimized_*.mp4)
```js
for %i in (*.mp4) do ffmpeg -i "%i" -vcodec libx264 -crf 23 -preset medium -pix_fmt yuv420p -c:a aac -b:a 128k "optimized_%~ni.mp4"
```

**Сильное сжатие** (compressed_*.mp4)
```js
for %i in (*.mp4) do ffmpeg -i "%i" -vcodec libx264 -crf 28 -preset fast -pix_fmt yuv420p -c:a aac -b:a 96k "compressed_%~ni.mp4"
```

---

## Замена исходников (удаляет оригиналы)

**Среднее сжатие с заменой**
```js
for %i in (*.mp4) do (ffmpeg -i "%i" -vcodec libx264 -crf 23 -preset medium -pix_fmt yuv420p -c:a aac -b:a 128k "temp_%~ni.mp4" && del "%i" && rename "temp_%~ni.mp4" "%~ni.mp4")
```

**Сильное сжатие с заменой (⭐)**
```js
for %i in (*.mp4) do (ffmpeg -i "%i" -vcodec libx264 -crf 28 -preset fast -pix_fmt yuv420p -c:a aac -b:a 96k "temp_%~ni.mp4" && del "%i" && rename "temp_%~ni.mp4" "%~ni.mp4")
```


---
## Дополнительные полезные команды

**Информация о видео**
```js
ffmpeg -i input.mp4 -hide_banner
```

**Только поменять контейнер (без перекодирования)**
```js
ffmpeg -i input.mp4 -c copy output.mkv
```

**Извлечь аудио**
```js
ffmpeg -i input.mp4 -q:a 0 -map a output.mp3
```

**Убрать аудио (оставить только видео)**
```js
ffmpeg -i input.mp4 -c copy -an output.mp4
```

**Обрезать видео (с 00:30 до 01:30)**
```js
ffmpeg -i input.mp4 -ss 00:00:30 -to 00:01:30 -c copy output.mp4
```

**Уменьшить разрешение до 720p**
```js
ffmpeg -i input.mp4 -vf scale=1280:720 -vcodec libx264 -crf 23 -preset medium -c:a copy output.mp4
```

**Все видео в подпапках (рекурсивно)**
```js
for /r %i in (*.mp4) do ffmpeg -i "%i" -vcodec libx264 -crf 28 -preset fast -pix_fmt yuv420p "%~dpni_compressed.mp4"
```

**Несколько форматов сразу (mp4, mov, avi)**
```js
for %i in (*.mp4 *.mov *.avi) do ffmpeg -i "%i" -vcodec libx264 -crf 28 -preset fast -pix_fmt yuv420p "compressed_%~ni.mp4"
```


---
## Советы

- Всегда тестируйте на одном файле перед массовой обработкой
- При замене оригиналов сделайте резервную копию
- CRF 23 — лучший выбор для большинства случаев
- Если спешите — используйте preset fast
- Если нужен минимальный размер — CRF 30-32 + аудио 64k моно


---

## Другие возможности ffmpeg

### Конвертировать форматы

Любое видео в любой формат (mp4, avi, mkv, mov, webm и т.д.)

```js
ffmpeg -i input.mp4 -c copy output.avi
```

```js
ffmpeg -i input.mkv -vcodec libx264 -acodec aac output.mp4
```

```js
ffmpeg -i input.mov -vcodec libx264 -crf 23 output.mp4
```

### Извлечь аудио

Вытащить звук в mp3, aac, wav

```js
ffmpeg -i input.mp4 -q:a 0 -map a output.mp3
```

```js
ffmpeg -i input.mp4 -c:a aac -b:a 128k output.aac
```

```js
ffmpeg -i input.mp4 -c:a pcm_s16le output.wav
```

### Убрать звук

Оставить только видео (без аудио)

```js
ffmpeg -i input.mp4 -c copy -an output.mp4
```

```js
ffmpeg -i input.mp4 -vcodec libx264 -crf 23 -an output.mp4
```

### Субтитры

Извлечь субтитры из видео

```js
ffmpeg -i input.mp4 -map 0:s:0 output.srt
```

Вшить субтитры в видео

```js
ffmpeg -i input.mp4 -i subtitles.srt -c copy -c:s mov_text output.mp4
```

```js
ffmpeg -i input.mp4 -vf "subtitles=subtitles.srt" output.mp4
```

### Замедлить или ускорить видео

Замедлить в 2 раза (0.5 скорость)

```js
ffmpeg -i input.mp4 -vf "setpts=2.0*PTS" -af "atempo=0.5" slow.mp4
```

Ускорить в 2 раза

```js
ffmpeg -i input.mp4 -vf "setpts=0.5*PTS" -af "atempo=2.0" fast.mp4
```

Ускорить только видео (аудио без изменений)

```js
ffmpeg -i input.mp4 -vf "setpts=0.5*PTS" -an fast_video_only.mp4
```

### Создать GIF из видео

Обычный GIF

```js
ffmpeg -i input.mp4 -vf "fps=10,scale=320:-1" output.gif
```

GIF с палитрой (лучше качество)

```js
ffmpeg -i input.mp4 -vf "fps=10,scale=320:-1:flags=lanczos,palettegen" palette.png
ffmpeg -i input.mp4 -i palette.png -lavfi "fps=10,scale=320:-1[x];[x][1:v]paletteuse" output.gif
```

Из определенного отрезка (с 00:30 длительностью 5 секунд)

```js
ffmpeg -i input.mp4 -ss 00:00:30 -t 5 -vf "fps=10,scale=320:-1" output.gif
```

## Цикл для конвертации всех видео в папке

```js
for %i in (*.mp4) do ffmpeg -i "%i" -c copy "%~ni.avi"
```

```js
for %i in (*.mkv) do ffmpeg -i "%i" -vcodec libx264 -acodec aac "%~ni.mp4"
```

```js
for %i in (*.mp4) do ffmpeg -i "%i" -q:a 0 -map a "%~ni.mp3"
```

Вот дополнение для вашего файла — вставьте в конец, после раздела "Другие возможности ffmpeg":

---

## Работа с MP3 (сжатие и конвертация)

### Параметры для аудио

| Параметр          | Что значит                                     |
| ----------------- | ---------------------------------------------- |
| `-b:a 128k`       | битрейт аудио (чем меньше, тем сильнее сжатие) |
| `-ac 1`           | моно (вместо стерео)                           |
| `-ac 2`           | стерео                                         |
| `-ar 44100`       | частота дискретизации                          |
| `-c:a libmp3lame` | кодировщик MP3                                 |
| `-q:a 0`          | лучшее качество MP3 (0-9, где 0 лучше)         |

### Битрейты для MP3

| Битрейт | Качество | Для чего |
|---------|----------|----------|
| 320 kbps | отличное | музыка, архив |
| 192 kbps | хорошее | повседневное слушание |
| 128 kbps | среднее | стандарт, подкасты |
| 96 kbps | приемлемо | речь, аудиокниги |
| 64 kbps | низкое | лекции, экономия места |

### Сжать MP3 (уменьшить битрейт)

```js
ffmpeg -i input.mp3 -b:a 128k output.mp3
```

```js
ffmpeg -i input.mp3 -b:a 96k output.mp3
```

```js
ffmpeg -i input.mp3 -b:a 64k output.mp3
```

### Сжать и перевести в моно (сильная экономия)

```js
ffmpeg -i input.mp3 -ac 1 -b:a 64k output.mp3
```

### Конвертировать между аудиоформатами

**WAV → MP3**
```js
ffmpeg -i input.wav -b:a 192k output.mp3
```

**FLAC → MP3**
```js
ffmpeg -i input.flac -b:a 320k output.mp3
```

**OPUS → MP3**
```js
ffmpeg -i input.opus -b:a 128k output.mp3
```

**M4A → MP3**
```js
ffmpeg -i input.m4a -b:a 128k output.mp3
```

### Сжать все MP3 в папке

```js
for %i in (*.mp3) do ffmpeg -i "%i" -b:a 128k "compressed_%~ni.mp3"
```

```js
for %i in (*.mp3) do ffmpeg -i "%i" -ac 1 -b:a 64k "small_%~ni.mp3"
```

### Конвертировать все файлы в MP3

```js
for %i in (*.wav *.flac *.m4a) do ffmpeg -i "%i" -b:a 192k "%~ni.mp3"
```

---

## Работа с изображениями

### Параметры для изображений

| Параметр | Что значит |
|----------|------------|
| `-q:v 2` | качество JPG (2 лучше, 31 хуже) |
| `-compression_level 9` | сжатие PNG (0-9, 9 максимум) |
| `-vf "scale=W:H"` | изменить размер |
| `-vf "scale=W:-1"` | изменить ширину, высота пропорционально |
| `-vf "fps=1"` | 1 кадр в секунду (для извлечения из видео) |
| `-vframes 1` | взять 1 кадр |

### Сжать изображения

**Сжать JPG (уменьшить качество)**
```js
ffmpeg -i input.jpg -q:v 5 compressed.jpg
```

**Сжать PNG**
```js
ffmpeg -i input.png -compression_level 9 compressed.png
```

**Сжать и изменить размер**
```js
ffmpeg -i input.jpg -vf "scale=1920:-1" -q:v 5 resized.jpg
```

### Конвертировать в WebP (современный формат, сильное сжатие)

**JPG/PNG → WebP**
```js
ffmpeg -i input.jpg -c:v libwebp -q:v 80 output.webp
```

**WebP с разным качеством**
```js
ffmpeg -i input.png -c:v libwebp -q:v 90 output.webp
```

```js
ffmpeg -i input.png -c:v libwebp -q:v 70 output.webp
```

```js
ffmpeg -i input.png -c:v libwebp -q:v 50 output.webp
```

### Конвертировать между форматами

**PNG → JPG**
```js
ffmpeg -i input.png -q:v 2 output.jpg
```

**JPG → PNG**
```js
ffmpeg -i input.jpg output.png
```

**BMP → JPG**
```js
ffmpeg -i input.bmp -q:v 2 output.jpg
```

**WebP → PNG**
```js
ffmpeg -i input.webp output.png
```

**WebP → JPG**
```js
ffmpeg -i input.webp -q:v 2 output.jpg
```

### Изменить размер изображения

**До ширины 1280px (высота пропорционально)**
```js
ffmpeg -i input.jpg -vf "scale=1280:-1" output.jpg
```

**До точного размера 1920x1080 (игнорируя пропорции)**
```js
ffmpeg -i input.jpg -vf "scale=1920:1080" output.jpg
```

**Сделать миниатюру 300px шириной**
```js
ffmpeg -i input.jpg -vf "scale=300:-1" thumb.jpg
```

### Извлечь кадры из видео

**Один кадр (скриншот)**
```js
ffmpeg -i input.mp4 -ss 00:00:30 -vframes 1 -q:v 2 frame.jpg
```

**Кадр в PNG (лучше качество)**
```js
ffmpeg -i input.mp4 -ss 00:00:30 -vframes 1 frame.png
```

**Кадр каждые 10 секунд**
```js
ffmpeg -i input.mp4 -vf "fps=0.1" frame_%04d.jpg
```

### Создать видео из изображений

**Из последовательности кадров (IMG_001.jpg, IMG_002.jpg...)**
```js
ffmpeg -framerate 24 -i IMG_%03d.jpg -c:v libx264 -crf 18 -pix_fmt yuv420p output.mp4
```

**Из всех JPG в папке**
```js
ffmpeg -framerate 24 -pattern_type glob -i "*.jpg" -c:v libx264 -crf 20 output.mp4
```

**Из одной картинки сделать видео (5 секунд)**
```js
ffmpeg -loop 1 -i image.jpg -t 5 -c:v libx264 -pix_fmt yuv420p output.mp4
```

### Циклы для массовой обработки изображений

**Все PNG → JPG**
```js
for %i in (*.png) do ffmpeg -i "%i" -q:v 2 "%~ni.jpg"
```

**Все JPG → WebP**
```js
for %i in (*.jpg) do ffmpeg -i "%i" -c:v libwebp -q:v 80 "%~ni.webp"
```

**Все PNG → WebP (сильное сжатие)**
```js
for %i in (*.png) do ffmpeg -i "%i" -c:v libwebp -q:v 70 "%~ni.webp"
```

**Все JPG сжать и уменьшить до ширины 1920px**
```js
for %i in (*.jpg) do ffmpeg -i "%i" -vf "scale=1920:-1" -q:v 5 "compressed_%~ni.jpg"
```

**Все изображения (JPG, PNG, BMP) → WebP**
```js
for %i in (*.jpg *.png *.bmp) do ffmpeg -i "%i" -c:v libwebp -q:v 75 "%~ni.webp"
```

### Сравнение форматов для изображений

| Формат | Когда использовать |
|--------|-------------------|
| JPG | Фотографии, сложные изображения (хорошее сжатие) |
| PNG | Скриншоты, логотипы, прозрачность (без потерь) |
| WebP | Современный формат, лучше сжатие чем JPG и PNG |
| BMP | Никогда (огромный размер) |

### Советы по изображениям

- Для фотографий в интернете используйте WebP (q:v 75-85)
- Для скриншотов с текстом используйте PNG
- Для архивных фото используйте JPG (q:v 2-5)
- WebP дает на 25-35% меньший размер чем JPG при том же качестве
- Всегда тестируйте на одном изображении перед массовой обработкой
