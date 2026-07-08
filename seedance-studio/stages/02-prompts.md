# Этап 2 — Сборка Промптов

Данные — из project-brief.md: Visual Bible anchor, camera rules, character anchors, reference mapping.

## Режим шота

T2V — только текст | I2V — есть @Image | R2V — несколько референсов | V2V — правка видео
Определяй по Reference Mapping из project-brief.

## Placeholder для раскадровки

Перед сборкой промптов — прочитай Reference Mapping из project-brief.md.
Найди последний занятый @ImageN. Стоп-кадр раскадровки = следующий номер.

Пример:
  @Image1 — Макс, @Image2 — Моника, @Image3 — Сэр, @Image4 — лес → раскадровка @Image5
  @Image1 — персонаж, @Image2 — лого → раскадровка @Image3

Добавляй строку в каждый промпт после референсов персонажей:
  Reference @Image[N] as storyboard reference for this scene — visual composition guide.

Если project-brief.md не найден или Reference Mapping пустой — пиши @Image[?] и предупреди:
  "Уточни номер @Image для раскадровки после инициализации проекта."

## Шаблоны

### R2V (основной при наличии референсов)

Reference @Image1 as [character] — exact appearance throughout.
Reference @Image2 as [character] — exact appearance throughout.
[...остальные персонажи и окружение...]
Reference @Image[N] as storyboard reference for this scene — visual composition guide.

[SUBJECT + MOTION с физикой],
in [ENVIRONMENT],
[STYLE anchor из project-brief],
[CAMERA: frame + angle + movement + lens].
[AUDIO: только если нужно].
Cinematic. [X]s.

Stable faces throughout. No morphing. No deformation.
Consistent character appearance from @Image1, @Image2 [...] in every shot.

### T2V

[SUBJECT + внешность] [MOTION с физикой],
in [ENVIRONMENT],
[STYLE anchor], [CAMERA].
Cinematic, photorealistic. [X]s.

### I2V

[Subject from @Image1] [MOTION — только движение],
in [Environment], [Style anchor], [Camera],
maintain exact appearance from @Image1.
Stable face throughout. No morphing. No deformation.

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

STORYBOARD REFERENCE: @Image[N] — загрузи готовый лист раскадровки сюда
РЕЖИССЁРСКАЯ ЗАМЕТКА: [что делает шот нарративно]
---

По умолчанию только EN. ZH — только по явному запросу.

## Сохранение

Write("{project_path}/prompts/shot-{N}.md", [промпт])

## После всех шотов — всегда предлагай раскадровку

---
Промпты готовы. Раскадровка: я читаю эти промпты и собираю один JSON
с готовым промптом для генерации листа раскадровки.
Ты генерируешь одно изображение и загружаешь его в Seedance как @Image[N].

Создать раскадровку? (да / нет)
---

Если "да" — загрузи gemini-keyframe-designer.
Промпты уже в контексте разговора — передавать отдельно не нужно.
Также сообщи: @Image[N] для раскадровки и путь {project_path} для сохранения.