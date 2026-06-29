# База знаний: Промптинг для Seedance 2

---

## ФУНДАМЕНТАЛЬНЫЕ ПРИНЦИПЫ

Закон первых слов:
Первые 20-30 слов имеют максимальный вес. SUBJECT + MOTION — всегда первыми.

Рабочий диапазон: 100-260 слов.
Меньше 100 — размытый результат. Больше 260 — модель делает только первую половину.

Физика вместо психологии:
ВЕРНО: "jaw clenches, nostrils flare, veins on neck tighten, skin pulls flat"
НЕВЕРНО: "looks angry, seems worried, feels scared"

---

## РЕЖИМЫ ГЕНЕРАЦИИ

T2V — только текст, нет файлов.
I2V — есть стартовое @Image. Описывай движение, НЕ повторяй картинку.
R2V — несколько референсов. Явно указывай роль каждого файла.
V2V — правка видео. Говори что менять, остальное "keep identical".

---

## ШАБЛОНЫ

### T2V
[SUBJECT: кто + внешность] [MOTION: действие с физикой],
in [ENVIRONMENT: форграунд / бэкграунд, время, атмосфера],
[STYLE PRESET], [CAMERA: frame size, angle, movement + lens],
[AUDIO: только если нужно].
Cinematic, photorealistic. [X]s.

### I2V
[Subject from @Image1] [does MOTION — только движение],
in [Environment], [Style], [Camera],
maintain exact appearance from @Image1.
Stable face throughout. No morphing. No deformation.

### R2V — Multi-angle Subject
Reference / Combine [subject] from @Image1 (front) and @Image2 (side),
maintaining consistent features.
Generate: [Scene + Environment + Style]. [Camera].
Stable face. No morphing. No deformation.

### V2V — Замена персонажа
@Video1 — base video. Replace [кого] with [character from @Image1].
Keep everything identical — location, lighting, camera, audio. Only the person changes.

### V2V — Продление
Extend @Video1 [forward / backward].
[Описание продолженного сегмента].
Maintain consistent style, lighting, character appearance.

---

## СИНТАКСИС РЕФЕРЕНСОВ

ВЕРНО: @Image1, @Image2, @Video1, @Audio1
НЕВЕРНО: "Image 1" / "image1" / "@image 1" / "the woman from the first image"

---

## TIMELINE ДЛЯ СЛОЖНЫХ СЦЕН

[0:00-3s]  Событие 1. Камера. SFX.
[3:00-7s]  Событие 2. Новая камера.
[7:00-15s] Финал.

Не больше 4-6 крупных событий на 15 секунд.

---

## СТАБИЛИЗАТОРЫ (обязательно для персонажей)

"Stable face throughout. No morphing. No deformation."
При нескольких шотах:
"Consistent character throughout. No drift between shots. Maintain exact appearance from @Image1 in every frame."

---

## СПЕЦИАЛЬНЫЕ ПРИЁМЫ

Непрерывный кадр: "ONE CONTINUOUS SHOT. No cuts. No edits throughout."
Match cut: "MATCH CUT. The same [motion] carries seamlessly from [Scene A] into [Scene B]."
Защита логотипа: "Do not change, alter, or replace any text, logo, or design. Must look identical to @ImageN in every frame."
Позиционирование: "appears from LEFT SIDE and stays there. Does NOT relocate."

---

## ДИАГНОСТИКА ПРОМПТА

[ ] Subject и Motion в первых 20-30 словах?
[ ] Камера: frame + angle + movement + lens?
[ ] Стабилизаторы (если персонаж)?
[ ] Референсы через @Image1?
[ ] Длина 100-260 слов?
[ ] Не больше 4-6 событий на 15с?
[ ] Физика вместо ментальных состояний?
[ ] Двойной контраст между шотами?
[ ] Лимит <= 3500 символов?

---

## ЛИМИТЫ SEEDANCE 2

- До 9 изображений / до 3 видео (суммарно <= 15с) / до 3 аудио
- Всего до 12 файлов | Вывод: 4-15 секунд, до 2K

---

## РЕШЕНИЕ ПРОБЛЕМ

Не проходит модерацию -> ZH-версия или CapCut: обрезать 1-2с, скорость 0.95x
Персонаж дрейфует -> Subject первым. Multi-angle @Image. Стабилизаторы.
Объект двоится -> "appears from LEFT SIDE. Does NOT relocate."
Лого меняется -> строка защиты лого
Модель делает половину -> убрать 30% контента, 3-4 события на 15с
Аудио не принимается -> чёрный экран с музыкой как @Video1