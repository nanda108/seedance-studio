---
name: gemini-keyframe-designer
description: >
  Generates ONE storyboard JSON from a Seedance 2 shot sequence.
  Reads the Seedance prompt built by seedance-studio and outputs a single JSON
  with a ready_prompt field — one complete, ready-to-paste text for any image model
  (Gemini Imagen, DALL-E, Midjourney, Flux) that generates ONE professional storyboard
  sheet image with all panels on one page.
  The generated storyboard image is uploaded into Seedance as @ImageN reference.
  Triggered automatically when seedance-studio offers storyboard after building prompts.
  Trigger on: "раскадровка", "storyboard", "создай раскадровку", "да" после предложения.
---

# Seedance Keyframe Designer — Storyboard JSON Generator

Читает Seedance-промпт сцены → возвращает JSON с готовым промптом для ONE storyboard image.

## Пайплайн

```
seedance-studio → Seedance-промпт (с @Image[N] placeholder)
      ↓
gemini-keyframe-designer читает промпт, извлекает шоты
      ↓
возвращает storyboard.json с полем ready_prompt
      ↓
пользователь копирует ready_prompt → генерирует ОДНО изображение
      ↓
загружает его в Seedance как @Image[N]
```

## Структура JSON

```json
{
  "title": "Animated Storyboard – [Название сцены]",

  "ready_prompt": "Полный готовый промпт для вставки в image-модель — см. правила сборки ниже",

  "style": {
    "format": "professional animation storyboard",
    "layout": "[N] storyboard frames with production notes",
    "background": "clean white storyboard page",
    "visual": "[Стиль анимации]",
    "lighting": "[Описание света]",
    "camera": "realistic cinematography",
    "quality": "ultra detailed",
    "consistency": {
      "[character_id]": "same [ключевые детали внешности и одежды]"
    }
  },

  "shots": [
    {
      "panel": 1,
      "time": "0:00-0:02",
      "camera": "Тип кадра и движение",
      "action": "Физические действия",
      "dialogue": "Реплика или null",
      "frame_description": "Что нарисовано в этой панели — позиции персонажей, поза, ракурс"
    }
  ],

  "seedance_reference": "@Image[N]",

  "negative_prompt": [
    "single image",
    "comic page",
    "manga",
    "random collage",
    "different character designs",
    "missing storyboard layout",
    "missing production notes",
    "text artifacts",
    "watermark"
  ]
}
```

## Правила сборки ready_prompt

`ready_prompt` — единственное поле, которое пользователь копирует в image-модель.
Собирается из всех данных сцены. Структура:

**Часть 1 — Лист раскадровки (общее)**
"Create a professional animation storyboard on a white storyboard sheet.
[N] sequential storyboard panels arranged in a clean production layout.
Each panel includes a cinematic frame plus a production notes column with shot number,
duration, camera direction, action and dialogue.
[Стиль анимации]. Consistent [стиль] characters across all panels.
The storyboard should look like it was prepared for an animated film director."

**Часть 2 — Описание каждой панели (из shots[].frame_description)**
Собирается в единый блок:
"Panel 1 ([time], [camera]): [frame_description]. [dialogue если есть]
Panel 2 ([time], [camera]): [frame_description]. [dialogue если есть]
..."

**Часть 3 — Консистентность персонажей**
"Character consistency: [character] — [description]. [character] — [description]."

**Итоговая строка:**
"Ultra detailed, [ratio] aspect ratio, clean white background, no watermarks."

Все три части объединяются в одну строку через пробел — это и есть `ready_prompt`.

## negative_prompt

Добавляй специфические запреты под сцену если нужно.
Базовый набор всегда:
"single image, comic page, manga, random collage, different character designs,
missing storyboard layout, missing production notes, text artifacts, watermark"

## Формат вывода

### 1. Одна строка
"Генерирую storyboard.json — [N] панелей, один лист."

### 2. JSON в code block

### 3. Инструкция после JSON

```
КАК ИСПОЛЬЗОВАТЬ:
1. Скопируй поле "ready_prompt" → вставь в Gemini / DALL-E / Midjourney / Flux
2. Сгенерируй ОДНО изображение — полный лист раскадровки
3. Загрузи его в Seedance как [seedance_reference]
```

### 4. Сохранение
Write("{project_path}/storyboard.json", [json])