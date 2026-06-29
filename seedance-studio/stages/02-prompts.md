# Этап 2 — Сборка Промптов

Данные — из project-brief.md: Visual Bible anchor, camera rules, character anchors, reference mapping.

## Режим шота

T2V — только текст | I2V — есть @Image | R2V — несколько референсов | V2V — правка видео
Определяй по Reference Mapping из project-brief.

## Шаблоны

T2V:
[SUBJECT + внешность] [MOTION с физикой],
in [ENVIRONMENT: форграунд / бэкграунд, время, атмосфера],
[STYLE: Visual Bible anchor из project-brief],
[CAMERA: frame + angle + movement + lens],
[AUDIO: только если нужно].
Cinematic, photorealistic. [X]s.

I2V:
[Subject from @Image1] [MOTION — движение, не повтор картинки],
in [Environment], [Style anchor], [Camera],
maintain exact appearance from @Image1.
Stable face throughout. No morphing. No deformation.

R2V:
Reference [appearance] of [subject] from @Image1
(and @Image2 for multi-angle if in project-brief).
Generate: [Environment + Style anchor]. [Camera].
Stable face. No morphing. No deformation.

## Критические правила

Subject + Motion — первые 20-30 слов.
100-260 слов. Физика: "jaw clenches" НЕ "looks angry".
Не больше 4-6 событий на 15 секунд.

## Объективы: автовыбор

Если camera: auto — читай knowledge/cinematography.md, выбирай под жанр и тон сцены.

## Формат вывода

---
ШОТ [N] — [ТИП] | [T2V/I2V/R2V/V2V] | [X]s | [ratio]
[Описание на русском]

EN:
[Промпт — 100-260 слов]

РЕЖИССЁРСКАЯ ЗАМЕТКА: [что делает шот нарративно]
---

По умолчанию только EN. ZH — только по явному запросу.

## Сохранение

Write("{project_path}/prompts/shot-{N}.md", [промпт])

После всех шотов: "QA-проверка?"