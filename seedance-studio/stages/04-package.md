# Этап 4 — Финальный Пакет

Собери и сохрани полный пакет для генерации в Seedance.

Формат FINAL_PACKAGE.md:

---
SEEDANCE STUDIO — ФИНАЛЬНЫЙ ПАКЕТ
ПРОЕКТ: [из project-brief]
ШОТОВ: [N] | ДЛИНА: [суммарно] | ФОРМАТ: [ratio]
---

## 1. REFERENCE MAPPING
Загружай файлы в Seedance строго в этом порядке:
@Image1 = [файл] — [роль]
@Image2 = [файл] — [роль]
@Video1 = [файл] — [роль]

## 2. VISUAL BIBLE ANCHOR
[Anchor-строка из project-brief]

## 3. ПРОМПТЫ
---
ШОТ [N] — [ТИП] | [режим] | [X]s
[Описание на русском]
EN:
[Промпт]
---

## 4. KEYFRAMES NEEDED
[NEEDS KEYFRAME] ШОТ [N]: [что нужно]
-> Используй seedance-keyframe-designer

## 5. ЛИМИТЫ SEEDANCE 2
- До 9 изображений / до 3 видео (<=15с) / до 3 аудио
- Всего до 12 файлов | Вывод: 4-15 секунд, до 2K

Сохранение:
Write("{project_path}/FINAL_PACKAGE.md", [весь пакет])