---
name: gemini-keyframe-designer
description: >
  Generates a structured storyboard JSON from Seedance 2 prompts. For each shot in a Seedance
  project, produces a cinematic image prompt that can be sent to any image generation model
  (Gemini Imagen, DALL-E, Midjourney, Flux) to create a photorealistic storyboard frame.
  Output is a single JSON file with three sections: characters (consistent descriptions),
  style (lighting, color grade, lens), and frames (one image prompt per shot).
  Use whenever the user has Seedance prompts and wants to visualize the storyboard,
  create reference frames, or pre-visualize a video project before generating it.
  Trigger on: "раскадровка", "storyboard", "визуализируй проект", "создай кейфреймы",
  "промпт для изображения", "покажи как будет выглядеть", "сгенерируй референсы".
---

# Seedance Keyframe Designer — Storyboard JSON Generator

Принимает Seedance-промпты проекта и возвращает один JSON-файл для генерации раскадровки.

## Входные данные

Читай из project-brief.md (если есть) и промптов шотов:
- Персонажи и их описания → секция "characters"
- Visual Bible anchor, стиль, свет → секция "style"
- Каждый шот → один элемент в "frames"

Если project-brief.md не найден — попроси вставить промпты напрямую.

## Структура JSON

```json
{
  "project": "Название проекта",
  "characters": [
    {
      "id": "character_id",
      "name": "Имя / роль",
      "description": "Полное физическое описание для image-модели: возраст, телосложение, черты лица, волосы (цвет, длина, стиль), одежда (каждый элемент: материал, цвет, посадка). Копируется дословно в каждый фрейм где персонаж появляется."
    }
  ],
  "style": {
    "preset": "Название стиля (Cinematic Photorealistic / Neo-Noir / CGI Animated / etc.)",
    "lighting": "Полное описание света: направление, качество, источник, цветовая температура, тени, контраст",
    "color_grade": "Цветовая палитра: конкретные цвета, не настроения. Пример: desaturated olive greens, burnt amber practicals, deep navy shadows",
    "lens": "Объектив и характер: focal length, aperture feel, crop ratio"
  },
  "frames": [
    {
      "shot": 1,
      "time": "0:00-0:05",
      "camera": "Тип кадра и движение (Medium-wide, slow push-in)",
      "action": "Что происходит в кадре — физические действия, не ментальные состояния",
      "dialogue": "Реплика если есть, или null",
      "image_prompt": "Полный промпт для генерации изображения — см. правила ниже"
    }
  ]
}
```

## Правила image_prompt

Каждый image_prompt — это один кинематографический стоп-кадр этого шота.
Структура: STYLE ANCHOR → CHARACTERS → ACTION/POSE → ENVIRONMENT → LIGHTING → CAMERA → SPECS

### STYLE ANCHOR (первым, всегда)
Фотореализм: "Cinematic photorealistic still,"
CGI/анимация: "Cinematic CGI animated still, Pixar/Disney quality,"
Noir: "35mm cinematic still, high contrast,"

### CHARACTERS
- Копируй description из секции "characters" дословно
- Добавь конкретную позу и выражение для этого фрейма
- Направление взгляда — обязательно
- Физические сигналы эмоции: "jaw clenches", не "looks angry"

### ENVIRONMENT
Три плана:
- Foreground: что между камерой и персонажем
- Midground: персонаж + основное действие
- Background: контекст, атмосфера, глубина

### LIGHTING
Копируй из секции "style.lighting" дословно + уточнения для этого фрейма.

### CAMERA
Переводи Seedance-язык в фото-язык:
- MS slow push-in → "Medium shot, 50mm, f/2.0 shallow depth"
- ECU static → "Extreme close-up, 85mm, f/1.4, eyes razor-sharp"
- WS crane up → "Wide shot from low angle, 24mm, slight upward tilt"
- EWS → "Establishing wide, 18mm, full environment"

### SPECS (последними, всегда)
"Photorealistic, ultra-detailed, [ratio] aspect ratio, sharp focus on [subject], no text, no watermarks, single image."

### Запрещено в image_prompt
- "digital art", "illustration", "painting" — ломает фотореализм
- Vague: "dark and moody" → конкретные цвета
- "happy", "sad", "angry" → физические сигналы
- Упоминание Seedance, видео, анимации

## Вывод

1. Выведи JSON полностью в code block
2. После JSON — краткая инструкция:

```
КАК ИСПОЛЬЗОВАТЬ:
1. Скопируй каждый "image_prompt" в Gemini / DALL-E / Midjourney / Flux
2. Сгенерируй по одному изображению на шот
3. Собери в сетку (PowerPoint, Figma, Canva) в порядке номеров шотов
4. Добавь текстовые блоки: shot number, time, camera, action, dialogue

ФОРМАТ СЕТКИ: 2-3 колонки, подписи справа от каждого кадра
```

3. Сохрани JSON:
Write("{project_path}/storyboard.json", [json])