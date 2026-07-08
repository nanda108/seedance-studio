# Этап 2 — Сборка Промптов

Данные — из project-brief.md: Visual Bible anchor, camera rules, character anchors, reference mapping.

## Режим шота

T2V — только текст | I2V — есть @Image | R2V — несколько референсов | V2V — правка видео
Определяй по Reference Mapping из project-brief.

## Шаблоны

### R2V (основной режим при наличии референсов персонажей)

Reference @Image1 as [character] — exact appearance throughout.
Reference @Image2 as [character] — exact appearance throughout.
[...остальные персонажи и окружение...]
Reference @ImageN as storyboard frame for this shot — use as visual composition reference.

[SUBJECT + MOTION с физикой],
in [ENVIRONMENT: форграунд / мидграунд / бэкграунд, время, атмосфера],
[STYLE: Visual Bible anchor из project-brief],
[CAMERA: frame + angle + movement + lens].
[AUDIO: только если нужно].
Cinematic. [X]s.

Stable faces throughout. No morphing. No deformation.
Consistent character appearance from @Image1, @Image2 [...] in every shot.

### T2V (нет референсов)

[SUBJECT + внешность] [MOTION с физикой],
in [ENVIRONMENT],
[STYLE anchor], [CAMERA: frame + angle + movement + lens].
Cinematic, photorealistic. [X]s.

### I2V (одно стартовое изображение)

[Subject from @Image1] [MOTION — только движение, не повтор картинки],
in [Environment], [Style anchor], [Camera],
maintain exact appearance from @Image1.
Stable face throughout. No morphing. No deformation.

## Placeholder для раскадровки

В каждый промпт добавляй строку под референсы персонажей:

  Reference @ImageN as storyboard frame for this shot — use as visual composition reference.

Где N = следующий свободный номер после персонажей и окружения.
Пример: если персонажи @Image1-3, окружение @Image4 → раскадровка @Image5.

Пользователь загрузит сгенерированный стоп-кадр раскадровки на это место в Seedance.
Если раскадровка ещё не готова — строку оставить, пользователь добавит файл позже.

## Критические правила

Subject + Motion — первые 20-30 слов.
100-260 слов. Физика: "jaw clenches" НЕ "looks angry".
Не больше 4-6 событий на 15 секунд.

## Объективы: автовыбор

Если camera: auto — читай knowledge/cinematography.md, выбирай под жанр и тон сцены.

## Формат вывода

---
ШОТ [N] — [ТИП] | [T2V/I2V/R2V] | [X]s | [ratio]
[Описание на русском]

EN:
[Промпт — 100-260 слов]

STORYBOARD REFERENCE: @Image[N] — загрузи сгенерированный кадр раскадровки сюда
РЕЖИССЁРСКАЯ ЗАМЕТКА: [что делает шот нарративно]
---

По умолчанию только EN. ZH — только по явному запросу.

## Сохранение

Write("{project_path}/prompts/shot-{N}.md", [промпт])

## После всех шотов — всегда предлагай раскадровку

После того как все промпты готовы, выведи:

---
Промпты готовы.

Следующий шаг — раскадровка:
Я создам storyboard.json с промптами для каждого кадра. Ты генерируешь изображения
в Gemini / DALL-E / Midjourney, загружаешь в Seedance как @Image[N] — и каждый шот
получает визуальный референс композиции.

Создать раскадровку? (да / нет)
---

Если "да" — загрузи gemini-keyframe-designer и передай ему все промпты текущей сцены.