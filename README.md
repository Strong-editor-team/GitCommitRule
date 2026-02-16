# GitCommitRule
Правила создания коммитов с перечислением типов коммитов

### Внимание, алиасы для основных директорий:
* **Assets/\_\_GAME\_\_/Art** - указывается как **Art** (визуал)
* **Assets/\_\_GAME\_\_/Backend** - указывается как **Backend** (бэк)
* **Assets/Plugins** - указывается как **Plugins** (плагины)
* **Assets/\_\_GAME\_\_/Scenes** - указывается как **Scenes** (сцены)
* **Assets/\_\_GAME\_\_/Prefabs** - указывается как **Prefabs** (префабы)

### Правила оформления заголовка
Каждый заголовок коммита должен строиться по схеме: `тип(область): действие`

- `Тип (Type)` Маленькими буквами, строго из списка ниже.
- `Область (Scope)` Одно существительное в скобках, определяющее модуль (например: ui, player, net). Не пишите здесь предложения.
- `Действие (Subject)` Начинается с глагола в повелительном наклонении (что сделать? — add, fix, remove, update).
Запрещено использовать само название типа (например, в типе fix запрещено писать слово fix).
Без точки в конце.

#### Основной синтаксис коммита
```
type_of_change [(scope)]: discription

[
// list change
- Element 1.
- Element 2.
- ...
]

[Important change]
```



| Тип | Версия (SemVer)	| Описание | Правильный пример |
| :--- | :---: | :---: | ---: |
| feat |	MINOR |	Новый функционал для игрока или разработчика. |	feat(input): add gamepad support |
| fix |	PATCH |	Исправление ошибки/бага. |	fix(ui): resolve overlap in main menu |
| docs |	no |	Изменения в документации (README, Wiki). |	docs(readme): describe install process |
| edit |	no |	Универсальный тип для изменения небольших частей проекта, которые никак не влияют на оптимизацию, архитектуру и логику (изменение скорости игрока, обновление дизайна сцены) |	edit(style): redesign main menu scene |
| refactor |	no |	Изменение кода без смены логики (чистка, упрощение). |	refactor(phys): simplify raycast logic |
| perf |	no* |	Оптимизация скорости работы или потребления памяти. |	perf(gfx): reduce draw calls for trees |
| test |	no |	Добавление или исправление тестов. |	test(save): add unit test for JSON parser |
| build |	PATCH |	Изменения в системе сборки, внешних зависимостях. |	build(deps): update Addressables to 1.19 |
| ci |	no |	Настройка CI/CD (GitHub Actions, Jenkins). |	ci(git): add auto-labeler for PRs |
| chore |	no |	Рутинные задачи (перенос файлов, чистка папок). |	chore(assets): reorganize folder structure |
| resources |	no |	Добавление/обновление ассетов (модели, звуки, иконки). |	resources(items): add 3D model for healthkit |
| revert |	varies |	Отмена предыдущего коммита. |	revert: "feat(ui): add social buttons" |

*Если оптимизация (perf) ломает обратную совместимость API — это MINOR.

### Примеры:

#### Был создан файл README.md с какой-то важной информацией
```
docs(readme): create README.md file

- Important imformation added
```

#### Была изменена архитектура проекта
```
chore(UI): reorganize UI structure

- Added prefab animation button.
- Rename folder Gradient to Shaders.
- Reorganize folder Video.
- Rename CustomButton to AnimateButton
- Move AnimateButton to Backend/UI
```

### Объединение несколько типов коммитов
Иногда бывают случаи, что в одном коммите нужно и добавить новый функционал, и пофиксить старый. В таком случае можно объединить типы коммитов таким образом:
```
feat([info]) && fix([info])

feat: [the information is short]
// addition information (scripts)

fix: [the information is short]
// addition information (scripts)
```
Но крайне не рекомендуется объединять другие коммиты, кроме пары feat && fix, лучше разбейте изменения на несколько коммитов.

### Более подробное объяснение разницы между `edit`, `perf`, `refactor`, `fix`, `chore`, и `feat`
* Добавили новый функционал (именно функционал, например, инвентарь, магазин, какая-то новая система или новая функция в существующий системе. В этом случае ставится `feat`
```
feat(interact system): add new interact type

- added interact type on right mouse button (RMB)
- refactored logic interact in InteractController.cs
```

* Поменяли расположение объектов на сцене, стиль и объекты в префабе или изменена скорость анимации/объекта (или сама анимация) в коде (DOTween)/на сцене. В этом случае используется `edit`
```
edit(animation): change speed animation switch tab
```

* Поменяли расположение папок в проекте, возможно затронуто обновление ссылок на эти папки/объекты с скриптах или на сцене. В этом случае необходимо использовать `chore`. В скобках в качестве информации необходимо указать область, которая была затронута (Art, Backend, Plugins, а также можно подкатегории и другие области)
```
chore(UI): reorganize UI structure

- move Art/UI/Scripts to Backend/Script/UI
- updated links on UI scripts on MainMenu scene
```

* Отрефакторен код или изменена логика каких-то отдельных элементов, изменение которых некритично. В этих случаях используется `refactor`. Например, замена `UnityEvent` на `event Action` или функция теперь возвращает не `object`, а `string` (повышение типа, т.к. `object` является родительским типом для `string`). Также рефакторинг подразумевает под собой изменение имени объектов/типов)
```
refactor(effects): change property Value on method GetValue()

- updated links on property Value
```

* Улучшена производительность игры с использованием ранее существующих файлов/скриптов. В этом случае применяется `perf`. Например, улучшение теней для поднятия фпс, упрощение качества спрайтов, улучшение скорости работы с памятью.
```
perf(memory): replacing lists with arrays

// optimizated scripts
```
```
perf(butch): the number of butch on the gameplay scene has been reduced

// addition information
```

ВНИМАНИЕ! Если ТОЛЬКО добавлен скрипт (и обновлены ссылки на сцене на этот скрипт) на оптимизацию чего-либо, то используйте
```
feat(optimization): add chunk manager

// new scripts for optimization
```

* Исправлены баги в игре или ошибки компиляции. В таком случае используется `fix`. При фиксе багов/ошибок логика поведения может некритично поменяться (но лучше использовать совместно с `refactor`), но добавление нового функционала запрещается.

## Важные уточнения
1. **_Как избежать тавтологии в_** `fix` (и других типах). Вместо того чтобы писать `fix(enemy): fix enemy bug`, используйте глаголы-синонимы, описывающие результат:

- Resolve (Разрешить проблему) — `fix(enemy): resolve freeze on death`
- Prevent (Предотвратить) — `fix(save): prevent data loss on crash`
- Handle (Обработать) — `fix(net): handle timeout exception`
- Ensure (Гарантировать) — `fix(ui): ensure button is clickable`

2. **_Масштабируемый_** `Scope`. Не пишите название файла или слишком подробно.<br>
НЕЛЬЗЯ: `fix(PlayerControllerUpdate): ...` (Слишком детально)<br>
НУЖНО: `fix(player): ...` (Кратко и понятно)

3. **_Breaking Changes_** (Критическое изменение). Если изменение ломает проект или старые сохранения, добавьте восклицательный знак после типа и опишите причину в теле коммита. Это увеличивает **MAJOR** (после релиза) или **MINOR** (до релиза) версию.
Пример:
```
feat(save)!: migrate from binary to JSON storage
```
