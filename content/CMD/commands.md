
Для сжатия изображений на винду устанавливаются pngquant и jpegoptim через Chocolatey

```js
for /r %i in (*.png) do pngquant --quality=60-61 --force --verbose --output "%i" -- "%i"
```

```js
for /r %i in (*.jpg) do jpegoptim --max=50 --overwrite --verbose "%i"
```

## ffmpeg

для сжатия видео
```js
for %i in (*.mp4) do (ffmpeg -i "%i" -vcodec libx264 -crf 28 -preset fast -pix_fmt yuv420p -c:a aac -b:a 96k "temp_%~ni.mp4" && del "%i" && rename "temp_%~ni.mp4" "%~ni.mp4")
```

