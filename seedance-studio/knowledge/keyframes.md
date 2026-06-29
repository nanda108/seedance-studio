# База знаний: Keyframes и Референсы

---

## ЧТО ТАКОЕ KEYFRAME

Keyframe — опорное изображение, которое Seedance 2 использует как визуальный якорь.
Без него модель изобретает внешность персонажей каждый раз по-разному.

Правило: если персонаж в двух и более шотах → нужен @Image.
Правило: если локация должна быть узнаваемой → нужен @Image.

---

## ИЕРАРХИЯ РЕФЕРЕНСОВ

@Image — идентичность и внешность (лицо, одежда, объект, бренд)
@Video — движение, камера, эффект
@Audio — ритм, музыка, голос

Не дублируй в тексте то, что задаёт референс:
- Есть @Image с лицом — только "@Image1 — exact face", не описывай лицо
- Есть @Video с движением — "@Video1 camera movement", не описывай подробно

---

## ТИПЫ @IMAGE РЕФЕРЕНСОВ

Multi-angle Subject (лучший способ для персонажа):
@Image1 — анфас (front view, нейтральное выражение)
@Image2 — профиль (side view или 3/4)
"Reference / Combine [subject] from @Image1 (front) and @Image2 (side), maintaining consistent features."

Logo / Brand:
"Do not change, alter, or replace any text, logo, or design. Must look identical to @Image1 in every frame."

Multi-subject:
@Image1 — первый персонаж, @Image2 — второй персонаж.
"[Character A] from @Image1 and [Character B] from @Image2..."

---

## ПРАВИЛА ОПИСАНИЯ

Есть @Image:
"@Image1 — [имя/роль]. Preserve exact likeness 100% throughout. Stable face. No morphing. No deformation."

Нет @Image — текстовый якорь:
"[Character]: [возраст], [телосложение], [кожа], [волосы], [одежда], [особенности]. Strict adherence. No deviation."

---

## КОГДА НУЖЕН KEYFRAME

Помечай шот [NEEDS KEYFRAME] если:
- Первый шот с новым персонажем
- Первый шот с брендом / продуктом
- Первый шот с уникальной локацией
- Шот с точным движением (нужен @Video)

---

## СОЗДАНИЕ KEYFRAMES

Нет @Image → используй скилл seedance-keyframe-designer.
Передай: описание, ракурс (анфас / профиль / 3/4 / full body), стиль, нейтральное студийное освещение.
Для персонажей — минимум 2 ракурса (анфас + профиль).

---

## REFERENCE MAPPING В PROJECT-BRIEF

| Переменная | Файл | Описание |
|------------|------|---------|
| @Image1 | anna_front.jpg | Анна, анфас |
| @Image2 | anna_side.jpg | Анна, профиль |
| @Image3 | logo.png | Логотип |
| @Video1 | cam_ref.mp4 | Референс камеры |
| @Audio1 | track.mp3 | Трек |

Загружай файлы в Seedance строго в порядке таблицы.

---

## КОНСИСТЕНТНОСТЬ ЧЕРЕЗ НЕСКОЛЬКО ШОТОВ

1. Единая якорная строка во всех промптах с персонажем
2. Multi-angle @Image1 (анфас) + @Image2 (профиль) в каждом шоте
3. Consistency anchor: "[CONSISTENCY: Character — [brief]. Lighting — [palette]. Lens — [focal].]"
4. Стабилизаторы: "Stable face throughout. No morphing. No deformation. Consistent character, no drift."

---

## ОБХОД ПРОБЛЕМ

Face detection блокирует → grid overlay (lumiying.com/tools/image-grid-overlay, 100% opacity)
EN не проходит модерацию → запроси ZH-версию промпта