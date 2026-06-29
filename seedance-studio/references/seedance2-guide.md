# Seedance 2.0 — Official Guide Reference
Source: BytePlus ModelArk + рабочая практика Клуб Нейротворцов

---

## OUTPUT FORMAT (обязательный формат вывода)

Для каждого промпта выводить строго в таком порядке:

1. **Краткое описание на русском** — 1-2 предложения: что происходит на видео
2. **EN:** — полный английский промпт
3. **ZH:** — нативный рерайт на китайском (НЕ перевод, пишется как китайский кинорежиссёр). Макс. 3300 символов с пробелами.

> Лимит символов на весь блок: **3500 символов с пробелами**
> Бонус ZH: китайские промпты часто проходят модерацию там, где EN режется

---

## БАЗОВЫЕ ПРИНЦИПЫ

### Вес первых слов
Первые **20–30 слов** несут максимальный вес. Subject и Motion — ВСЕГДА в начало. Иначе модель «теряет» персонажа.

### Длина промпта
**100–260 слов** — рабочий диапазон. Короче → «вата». Длиннее → модель делает только половину.

### Роли входов (не дублируй!)
| Тип входа | Роль |
|-----------|------|
| TEXT | весь мир, среда, действие |
| IMAGE | идентичность и внешность |
| VIDEO | движение, камера, эффект |
| AUDIO | ритм, музыка, голос |

Каждому входу — своя роль. Не описывай текстом то, что уже даёт референс.

---

## СИНТАКСИС РЕФЕРЕНСОВ

**Единственный правильный синтаксис:**
```
@Image1, @Image2, @Image3  — для изображений
@Video1, @Video2, @Video3  — для видео
@Audio1, @Audio2, @Audio3  — для аудио
```

Нумерация независимая для каждого типа. @Image1 = первый загруженный image.

**ПРАВИЛЬНО:**
- `"Reference the appearance from @Image1"`
- `"Follow the action from @Video1"`
- `"@Image1 — exact face and body. Preserve likeness 100% throughout."`

**НЕПРАВИЛЬНО:**
- `"Image 1"` / `"Image1"` / `"@image 1"` / `"the woman from the first image"`

---

## ПОЛНАЯ СТРУКТУРА ПРОМПТА (сложные сцены)

```
[REFERENCES] → [FORMAT] → [STYLE] → [COLOR] → [ENVIRONMENT] → [TIMELINE]
```

- **REFERENCES** — описание ассетов с ролями
- **FORMAT** — длина, BPM, соотношение сторон, число шотов
- **STYLE** — пресет стиля
- **COLOR** — цветовая палитра, грейдинг
- **ENVIRONMENT** — локация, атмосфера, освещение
- **TIMELINE** — события по секундам

**Звук:** по умолчанию только sound design, без музыки. Спрашивать у пользователя.

**Текст в кадре:** только по запросу. По умолчанию — без субтитров и заголовков.

---

## ФИНАЛЬНЫЕ СТРОКИ-СТАБИЛИЗАТОРЫ

В конце ЛЮБОГО промпта с персонажем:
```
Stable face throughout. No morphing. No deformation.
```

Дополнительно при необходимости:
- `"Maintain exact appearance from @Image1"`
- `"Consistent character throughout, no deformation or drift"`

---

## РЕЖИМЫ ГЕНЕРАЦИИ

| Ситуация | Режим |
|----------|-------|
| Только идея → тестовый кадр | T2V |
| Есть финальное изображение → оживить | I2V (описывай движение, не повторяй картинку) |
| Стабильная внешность через несколько сцен | R2V + Multi-angle Subject Reference |
| Сцена из готовых элементов (бренд, лого, персонаж) | R2V + Multi-image Reference |
| Точная пластика / тайминг движения | R2V + Action Reference |
| Конкретное движение камеры | R2V + Camera Movement Reference |
| Дописать / склеить ролики | V2V |

---

## ЛИМИТЫ ВХОДОВ

- до 9 изображений
- до 3 видео (суммарно ≤ 15 секунд)
- до 3 аудио (mp3, суммарно ≤ 15 секунд)
- всего до 12 файлов на генерацию
- длительность вывода: **4–15 секунд**, до 2K, со встроенным аудио

---

## РЕФЕРЕНСЫ: ИЗОБРАЖЕНИЯ

### Multi-angle Subject Reference (один объект с разных ракурсов)
```
Reference / Extract / Combine + @ImageN's [Subject],
generate [Scene Description],
maintaining consistent [Subject] features.
```
Когда: нужно сохранить точную внешность товара или персонажа.

### Multi-image Reference (несколько разных изображений)
Подвиды:
- **Logo Reference** — лого из @Image1 появляется в кадре
- **Multi-subject** — кот из @Image1 и собака из @Image2 в одной сцене
- **Multi-element** — персонаж + одежда + локация + лого из разных изображений
- **Storyboard** — раскадровка как изображение → генерация по панелям

Формула:
```
Reference / Extract / Combine / Follow / Generate +
@ImageN's [Referenced Element Description],
generate [Scene Description],
maintaining consistent [Referenced Element] features.
```

---

## РЕФЕРЕНСЫ: ВИДЕО

Явно указывай ЧТО именно копировать из видео.

### Action Reference
```
Reference @VideoN's [Action Description],
generate [Scene Description],
maintaining consistent action details.
```

### Camera Movement Reference
```
Reference @VideoN's [Camera Movement Description],
generate [Scene Description],
maintaining consistent camera movement.
```

### Effects Reference
```
Reference @VideoN's [Effects Description],
generate [Scene Description],
maintaining consistent effects.
```

---

## ВИДЕО-РЕДАКТИРОВАНИЕ (V2V)

### Add / Remove / Replace
```
ДОБАВИТЬ: At [Time Position] + [Spatial Position] of @VideoN, add [Element].
УДАЛИТЬ:  Remove [Element] from @VideoN, keep everything else unchanged.
ЗАМЕНИТЬ: Replace [Original] in @VideoN with [New Element].
```

Замена персонажа:
```
@Video1 — base video. Replace [кого] with @Image1.
Keep everything else identical — location, lighting, camera angles,
cuts, transitions, background, audio. Only the person changes.
```

### Video Extension
```
Extend @VideoN forward/backward + [Description of extended content].
```

### Track Completion (склейка)
```
@Video1 + [Transition] + connect to @Video2 + [Transition] + connect to @Video3
```
Лимит: до 3 видео, суммарно ≤ 15 секунд.

---

## КАМЕРА — ПОЛНЫЙ ЯЗЫК

### Типы камер
| Тип | Описание |
|-----|----------|
| handheld | органичная дрожь, документальность |
| static / locked-off | полностью статичная |
| stabilized tracking | плавное движение рядом |
| robotic arm | точные дуги, crash zooms |
| crane | подъём / опускание |
| aerial / drone | сверху |
| helmet cam / GoPro | широкий угол, POV |

### Размеры кадра
`ECU → CU → MCU → MS → WS → EWS`

### Движения камеры
dolly-in / dolly-out, push-in / pull-back, tracking shot, follow shot,
pan left/right, tilt up/down, orbit / arc shot, crane shot,
drone aerial, handheld, steadicam, zoom-in / zoom-out

### Переходы
hard cut / match cut / whip pan / crash zoom / dutch angle

### Правило двойного контраста на каждом cut
Каждый cut меняет И размер кадра И тип камеры одновременно.
Пример: `WS handheld → ECU static → MS crane → CU tracking`

---

## СТИЛИ-ПРЕСЕТЫ (вставлять в [STYLE] / [AESTHETICS])

| Стиль | Блок |
|-------|------|
| Кинематографический | `Photorealistic. 35mm film grain. 8K cinematic. Volumetric rendering. ARRI camera. Professional color grading.` |
| Реалистичный телефон / UGC | `Shot on phone — no color grade, no stabilization, natural grain, auto-exposure flickering, slight motion blur. Not cinematic. Not polished. Real.` |
| Старый телефон / утечка | `Looks like filmed on an old Nokia or cheap Android — low quality, grainy, slightly overexposed, handheld shake. Raw and authentic. No filters.` |
| GoPro / экшн-камера | `GoPro wide angle aesthetic. Slight barrel distortion. Auto-exposure adjusting. No stabilization. Raw authentic footage feel.` |
| NASA / документальный космос | `Hasselblad sharp focus — no bokeh anywhere. Pure black sky. Kodak Ektachrome film emulation. 16mm grain. Harsh directional sunlight. Deep black shadows.` |
| Музыкальный клип / мрачный | `Deep desaturation. Near monochrome. Blacks crushed to zero. High contrast. Film grain. Single source fluorescent rim light from behind.` |
| UGC обзор продукта | `Smartphone footage. Natural window light. Casual handheld selfie angle. No ring light, no filters. Natural phone quality, not color graded, authentic.` |
| Игровой трейлер / CGI | `Cinematic video game trailer. Photorealistic CGI. Unreal Engine 5 quality. Epic orchestral soundtrack. Fast combat cuts. Slow-motion key moments.` |
| Motion design / анимация | `Motion design. Black background throughout. Seamless continuous evolution — no cuts, no jumps. Every transition fluid and mathematical.` |
| 2D анимация | `Hand-drawn 2D cel animation. Fast-paced comedy montage.` |

---

## TIMELINE — РАЗБИВКА ПО ВРЕМЕНИ

Формат:
```
[0:00–3s]  Что происходит. Камера. Детали.
           SFX: звуки если нужны.
[3:00–7s]  Что дальше. Камера меняется.
[7:00–15s] Финал.
```

**Правило:** не больше **4–6 крупных событий** на 15 секунд. Больше → модель обрезает конец.

Для beat-sync: `FORMAT: 15s / 145 BPM / 15 SHOTS / beat-synced`

---

## ФИЗИКА И ЭМОЦИИ

**Правильно — физика вместо ментальных состояний:**
- ✓ `jaw clenches, nostrils flare, veins bulge, skin pulls flat`
- ✗ `looks angry, feels scared, seems worried`

**Правильно — намерение действия, без биомеханики:**
- ✓ `spinning back kick connects`
- ✗ `left forearm rotates 45 degrees to deflect the incoming right hook`

**Интенсивность:** усиливай наречиями — `slowly, abruptly, gently, violently, slightly, dramatically`

---

## АРХЕТИПЫ СЦЕН

**Экшн:** Pursuit (преследование) / Duel (доминирование чередуется) / Impact (нарастание → удар → aftermath)

**Общие:** Journey (движение через пространство) / Atmosphere (настроение IS контент) / Reveal (скрытое открывается движением камеры)

**Диалог:** Confrontation / Interrogation / Negotiation (симметричный фрейминг)

---

## СПЕЦИАЛЬНЫЕ ЭФФЕКТЫ И ПРИЁМЫ

| Приём | Формула |
|-------|---------|
| Непрерывный кадр | `ONE CONTINUOUS SHOT. No cuts. No edits throughout.` |
| Beat-sync монтаж | `FORMAT: 15s / 145 BPM / 15 SHOTS / beat-synced` |
| Match cut | `MATCH CUT. The same [действие] carries seamlessly into [новая сцена].` |
| Выход за чёрные полосы | `FORMAT: 2.39:1 cinematic bars throughout. BREAKING BARS EFFECT: In key moments — shoulders, hands, head physically breach the black bars.` |
| Бинокль ночного видения | `OUTSIDE binoculars = dark phone footage. INSIDE binoculars = green phosphor night vision, circular vignette frame. These two views NEVER appear simultaneously.` |
| Защита логотипа | `Do not change, alter, or replace any text, logo, or design on the packaging. Must look identical to @Image2 in every frame.` |
| Отражение в часах | `He raises his left wrist — watch face fills part of frame. In the convex watch crystal: his face reflected clearly.` |

---

## ЛОКАЦИИ — СПЕЦИФИКАЦИИ

| Локация | Формула |
|---------|---------|
| Россия / постсовет | `Cyrillic signage on all buildings. No English text. Post-Soviet architecture. Soviet-era aesthetic.` |
| Невесомость | `True microgravity throughout. Every object floats — slow drift, gentle rotation. Hair floats slightly. Water forms perfect wobbling spheres.` |
| Луна | `Pure black sky. No atmosphere. No stars in daylight. Harsh single-source sunlight. Deep black shadows. Fine grey regolith — footprints compress and hold perfectly. Lunar gravity physics.` |

---

## РЕШЕНИЕ ПРОБЛЕМ

| Проблема | Решение |
|----------|---------|
| Не проходит модерацию | Перегнать через CapCut, обрезать 1–2с, скорость 0.95x. Попробовать ZH-промпт. |
| Фото блокирует face detection | Grid overlay на lumiying.com/tools/image-grid-overlay (100% opacity) |
| Аудио не принимает | Загрузи чёрный экран с музыкой как @Video1. Пиши: "@Video1 — audio reference only. Ignore visual content." |
| Объект появляется в двух местах | `"appears from LEFT SIDE and stays there. Does NOT relocate."` |
| Модель делает только половину | Убери 30% контента. Уменьши beats до 3–4 на 15 секунд. |
| Логотипы меняются | Добавь строку защиты логотипа |
| Персонаж дрейфует по внешности | Subject в первые 20–30 слов. Добавь стабилизаторы. Используй multi-angle reference. |

---

## НЕГАТИВНЫЕ ПОДСКАЗКИ

```
no distortion, no stretching
no grain, no shadows          (для чистого CGI)
no historical drama feel      (если уводит в эпоху)
no morphing, no deformation
no character drift between cuts
```

---

## МИНИ-ШАБЛОНЫ (быстрый старт)

### I2V
```
[Subject from @Image1] [does Motion],
in [Environment],
[Aesthetics: lighting + style + color],
[Camera: shot size + movement],
[Audio: music + sfx],
maintain exact appearance from @Image1.
Stable face throughout. No morphing. No deformation.
```

### R2V
```
Reference the [appearance/identity] of [subject] from @Image1
(and @Image2 if multi-angle).
Reference the [camera movement / action / effect] from @Video1.
Generate: [scene description with Environment + Aesthetics + Audio].
Maintain consistent [subject features / motion / camera] throughout.
Stable face. No morphing. No deformation.
```

### Быстрая формула (рабочая практика)
```
@Image1 — [кто это].

[FORMAT + STYLE в 2-3 строки]

[ENVIRONMENT в 2-3 строки]

[0:00–5s]  [что происходит + камера]
[5:00–10s] [что происходит + камера]
[10:00–15s][что происходит + камера]

Stable face. No morphing. No deformation.
```
