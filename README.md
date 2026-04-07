# `.bid` tutorial (Zilda)

`.bid` описывает блок: его ключ, физику, рендер, текстуры, цвета и иконку.
Файлы лежат в `assets/packs/<Pack_name>/blocks/`.

## 1) Минимальный рабочий `.bid`

```ini
[bid]
version = 1
key = "zilda:my_block"
name = "My Block"

[props]
render = "opaque"
solid = true
```

Этого уже достаточно, чтобы блок парсился.

## 2) Секции и поля

### `[bid]`

- `version` (обязательно): сейчас поддерживается только `1`
- `key` (обязательно): строка в кавычках, например `"zilda:stone"`
- `name` (опционально): отображаемое имя, если не указать, используется `key`

### `[props]`

- `render` (обязательно): один из
  - `"opaque"`
  - `"cutout"`
  - `"transparent"`
  - `"water"`
- `solid` (обязательно): `true` или `false`
- `collision` (опционально): `true` или `false`, по умолчанию = `solid`
- `creative_visible` (опционально): `true` или `false`, по умолчанию `true`

### `[textures]` (опционально)

Можно не указывать вообще.

Поддерживаются варианты:

- `all` - одна текстура на все стороны
- `top/side/bottom` - классическая схема
- `top/bottom/north/south/east/west` - полный контроль по граням
- `particle` - отдельная текстура для частиц

Правила подстановки:

- `top` и `bottom` берутся из своих полей, иначе из `all`
- `north/south/east/west` берутся из своих полей, иначе из `side`, иначе из `all`
- если указаны только `top/side/bottom` (без `north/south/east/west`) - используется режим `TopSideBottom`

### `[colors]` (опционально)

Аналогично `textures`, но вместо путей указываются цвета:

- формат `#RRGGBB`, например `"#7fd35a"`
- или `r,g,b`:
  - в диапазоне `0..1` (например `0.5,0.8,0.3`)
  - или `0..255` (например `128,200,80`)

### `[icon]` (опционально)

- `texture` - путь к текстуре иконки
- `color` - цвет иконки

Если указаны оба, приоритет у `texture`.

## 3) Примеры

### Пример A: блок с текстурами `top/side/bottom`

```ini
[bid]
version = 1
key = "zilda:grass"
name = "Grass"

[props]
render = "opaque"
solid = true

[textures]
top = "textures/grass_up.jpeg"
side = "textures/grass_bottom.jpeg"
bottom = "textures/dirt.jpeg"
```

### Пример B: блок только с цветами

```ini
[bid]
version = 1
key = "zilda:color_test"
name = "Color Test"

[props]
render = "opaque"
solid = true
collision = true

[colors]
top = "#ff0000"
side = "#00ff00"
bottom = "#0000ff"

[icon]
color = "#ffc400"
```

## 4) Синтаксис и ограничения

- Комментарии начинаются с `#`
- Пустые строки разрешены
- Строковые значения должны быть в двойных кавычках
- Булевы только `true`/`false`
- Числа (например `version`) без кавычек
- Все `key = value` должны находиться внутри секции `[section]`

Неизвестные секции и неизвестные поля игнорируются.

## 5) Частые ошибки

- `unsupported .bid version` - указана версия не `1`
- `missing [bid].key` или `missing [props].render` - пропущено обязательное поле
- `expected string in quotes` - строка без `"`
- `expected true/false` - неверный булев тип
- `expected key = value` - сломан синтаксис строки
- `key/value outside section` - поле написано до первой секции

## 6) Чеклист перед запуском

- Файл лежит в `assets/packs/<Pack_name>/blocks/`
- Есть `[bid].version = 1`, `[bid].key`, `[props].render`, `[props].solid`
- Пути текстур корректны
- Цвета в формате `#RRGGBB` или `r,g,b`
