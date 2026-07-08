---
name: gemini-keyframe-designer
description: >
  Converts a Seedance 2 storyboard into a structured JSON file for storyboard visualization.
  For each shot, generates a cinematic image prompt to create a storyboard still frame using
  any image model (Gemini Imagen, DALL-E, Midjourney, Flux).
  Output JSON has four sections: characters, environment, style, frames (one image prompt per shot).
  The generated storyboard images are then uploaded back into Seedance as @ImageN references
  inside each shot prompt — giving Seedance a visual composition reference per shot.
  Use whenever the user has Seedance prompts and wants to visualize the storyboard,
  or when seedance-studio/02-prompts.md offers to create a storyboard after building prompts.
  Trigger on: "раскадровка", "storyboard", "создай раскадровку", "да" после предложения.
---

# Seedance Keyframe Designer — Storyboard JSON Generator

Принимает Seedance-промпты сцены → возвращает storyboard.json для генерации стоп-кадров.
Сгенерированные изображения загружаются обратно в Seedance как @ImageN на место плейсхолдера.

## Пайплайн

```
сценарий / правка
      ↓
seedance-studio → Seedance-промпт (с @ImageN placeholder для каждого шота)
      ↓
gemini-keyframe-designer → storyboard.json
      ↓
пользователь генерирует изображения (Gemini / DALL-E / Midjourney / Flux)
      ↓
загружает каждый кадр в Seedance на место @ImageN
      ↓
Seedance получает визуальный референс композиции для каждого шота
```

## Входные данные

Принимает готовые Seedance-промпты (из этой сессии или вставленные пользователем).
Если есть project-brief.md — Read его для контекста персонажей и стиля.

## Структура JSON

```json
{
  "project": "Название проекта",
  "format": {
    "ratio": "16:9",
    "duration": "15s",
    "style": "Полное описание стиля"
  },
  "characters": [
    {
      "id": "character_id",
      "name": "Имя",
      "reference": "@Image1",
      "description": "Полное физическое описание: возраст, телосложение, черты лица, волосы, одежда. Копируется дословно в каждый image_prompt где персонаж появляется."
    }
  ],
  "environment": {
    "reference": "@Image4",
    "description": "Полное описание окружения для вставки в каждый image_prompt"
  },
  "style": {
    "preset": "Точное название стиля",
    "lighting": "Направление, качество, источник, цветовая температура, тени",
    "color_grade": "Конкретные цвета — не настроения",
    "lens_default": "Дефолтный объектив"
  },
  "frames": [
    {
      "shot": 1,
      "time": "0:00-0:02",
      "seedance_reference": "@Image5",
      "camera": "Тип кадра и движение",
      "action": "Физические действия в этом кадре",
      "dialogue": "Реплика или null",
      "image_prompt": "Полный промпт для генерации стоп-кадра"
    }
  ]
}
```

Поле `seedance_reference` — тот самый @ImageN из плейсхолдера в Seedance-промпте.
Пользователь загружает сгенерированное изображение именно на эту позицию.

## Правила image_prompt

Каждый image_prompt = один кинематографический стоп-кадр шота. 80-150 слов. EN.

### Структура (строго в этом порядке)

**1. Style anchor — первым:**
- Pixar/CGI: `"Semi-realistic Pixar-inspired 3D animation still, cinematic 4K."`
- Фотореализм: `"Cinematic photorealistic still, 35mm film."`
- Noir: `"Cinematic 35mm noir still, high contrast."`

**2. Environment** — description из секции "environment" + детали форграунд/мидграунд/бэкграунд этого шота.

**3. Camera** — переводи Seedance-язык в фото-язык:

| Seedance | image_prompt |
|----------|-------------|
| Medium-wide, 50mm | `"Medium-wide shot, 50mm lens"` |
| ECU, 85mm | `"Extreme close-up, 85mm portrait lens, f/1.4"` |
| WS crane up | `"Wide shot, low angle, 24mm, slight upward tilt"` |
| Rack focus A→B | `"Rack focus: A sharp, B softly blurred"` |
| Slow push-in | `"Static with implied slow push-in energy"` |

**4. Characters** — для каждого в кадре:
- description из "characters" дословно
- Конкретная поза этого кадра
- Направление взгляда обязательно
- Физика эмоции: `"jaw clenches"`, не `"looks angry"`

**5. Lighting** — style.lighting дословно + уточнения кадра.

**6. Specs — последними:**
`"[ratio] aspect ratio. No text. No watermarks. Single frame. Stable faces, no morphing, no deformation."`

### Запрещено
- "digital art", "painting", "illustration"
- "moody", "sad", "happy" → конкретные физические детали
- Упоминание видео, анимации, движения

## Формат вывода

### 1. Краткое описание пайплайна (одна строка)

"Генерирую storyboard.json — [N] кадров. Загрузи каждый в Gemini/DALL-E/Midjourney, затем обратно в Seedance на место @ImageN."

### 2. JSON в code block

### 3. Таблица загрузки после JSON

| Шот | Файл для генерации | Загрузить в Seedance как |
|-----|-------------------|--------------------------|
| 1   | shot-01-storyboard.png | @Image5 |
| 2   | shot-02-storyboard.png | @Image5 |
| ... | ... | ... |

### 4. Сохранение

Write("{project_path}/storyboard.json", [json])