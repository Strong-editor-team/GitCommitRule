# GitCommitRule
Правила создания коммитов с перечислением типов коммитов

### Внимание, алиасы для основных директорий:
* **Assets/\_\_GAME\_\_/Art** - указывается как **Art** (визуал)
* **Assets/\_\_GAME\_\_/Backend** - указывается как **Backend** (бэк)
* **Assets/Plugins** - указывается как **Plugins** (плагины)
* **Assets/\_\_GAME\_\_/Scenes** - указывается как **Scenes** (сцены)
* **Assets/\_\_GAME\_\_/Prefabs** - указывается как **Prefabs** (префабы)

### Основной синтаксис коммита
```
type(scope): subject 

[
// list of changes
- change 1
- change 2
- ...
]
```

#### Правила оформления заголовка
Каждый заголовок коммита должен строиться по схеме: `тип(область): действие`

- `Тип (Type)` Маленькими буквами, строго из списка ниже.
- `Область (Scope)` Одно существительное в скобках, определяющее модуль (например: ui, player, net). Не пишите здесь предложения.
- `Действие (Subject)` Начинается с глагола в повелительном наклонении (что сделать? — add, fix, remove, update).
Запрещено использовать само название типа (например, в типе fix запрещено писать слово fix).
Без точки в конце.

#### Правила времен для коммитов
ВСЕ части коммита указывается только в `Imperative Mood`, где глагол чаще всего выносится на первое место. Примеры:

#### Правила оформления тела
В теле коммита должно содержаться более подробное описание каждого из изменений в этом коммите. Элемент начинается с `- ` и с маленькой буквы, в конце точки нет, время Present Simple.
```
chore(structure): reorganize assets and packages

- add important information
- delete unused assets
- move packages to Assets/Packages
```

### Типы коммитов
| Тип | Версия (SemVer)	| Описание | Правильный пример |
| :--- | :---: | :---: | ---: |
| feat |	MAJOR/MINOR |	Новый функционал для игрока или разработчика. |	feat(input): add gamepad support |
| fix |	PATCH |	Исправление ошибки/бага. |	fix(ui): resolve overlap in main menu |
| docs |	no |	Изменения в документации (README, Wiki). |	docs(readme): describe install process |
| edit |	no |	Универсальный тип для изменения файлов-ассетов, которые никак не влияют на оптимизацию, архитектуру и логику (изменение скорости игрока, обновление дизайна сцены) |	edit(style): redesign main menu scene |
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

- add important information
```

#### Была изменена архитектура проекта
```
chore(UI): reorganize UI structure

- add prefab animation button
- rename folder Gradient to Shaders
- reorganize folder Video
- rename CustomButton to AnimateButton
- move AnimateButton to Backend/UI
```

### Объединение несколько типов коммитов
Иногда бывают случаи, что в одном коммите нужно и добавить новый функционал, и пофиксить старый. В таком случае можно объединить типы коммитов таким образом:
```
feat(scope) && fix(scope)

feat: subject 1
[ feat commit body ]

fix: subject 2
[ fix commit body ]
```
```
chore(scope) && edit(scope)

chore: subject 1
[ chore commit body ]

edit: subject 2
[ edit commit body ]
```

Но крайне не рекомендуется объединять другие коммиты, кроме пары `feat && fix` или `chore && edit`, лучше разбейте изменения на несколько коммитов.

### Более подробное объяснение разницы между `edit`, `perf`, `refactor`, `fix`, `chore`, и `feat`
* Добавили новый функционал (именно функционал, например, инвентарь, магазин, какая-то новая система или новая функция в существующий системе. В этом случае ставится `feat`
```
feat(interact system): add new interact type

- add interact type on right mouse button (RMB)
- refactor logic interact in InteractController.cs
```

* Поменяли расположение объектов на сцене, стиль и объекты в префабе или изменена скорость анимации/объекта (или сама анимация) в коде (DOTween)/на сцене. В этом случае используется `edit`
```
edit(animation): change switch tab animation speed
```

* Поменяли расположение папок в проекте, возможно затронуто обновление ссылок на эти папки/объекты с скриптах или на сцене. В этом случае необходимо использовать `chore`. В скобках в качестве информации необходимо указать область, которая была затронута (Art, Backend, Plugins, UI и другие области)
```
chore(UI): reorganize UI structure

- move Art/UI/Scripts to Backend/Script/UI
- update UI script references in MainMenu scene
```

* Отрефакторен код или изменена логика каких-то отдельных элементов, изменение которых некритично. В этих случаях используется `refactor`. Например, замена `UnityEvent` на `event Action`. Также рефакторинг подразумевает под собой изменение имени объектов/типов)
```
refactor(effects): change property Value on method GetValue()

- update references on property Value
```

* Улучшение производительности без добавления новой функциональности. Если вы изменяете существующие файлы/скрипты исключительно для повышения скорости работы или снижения потребления памяти, используйте тип `perf`.
```
perf(memory): replace lists with arrays
```
```
perf(bench): reduce draw calls in gameplay scene
```
```
perf(shadows): optimize shadow distance
```

* Добавление системы, которая одновременно реализует новую механику и повышает производительность.
Например, chunk manager может как обеспечивать бесшовную подгрузку мира (новая механика), так и оптимизировать рендеринг. В таких случаях решение принимается по главной цели:

  * Если основная задача — реализовать новую игровую механику (например, бесконечную генерацию мира), используйте `feat`, даже если система также улучшает производительность.

  * Если же механика уже существовала, и вы перерабатываете её исключительно для оптимизации (ускорения работы, уменьшения лагов), используйте `perf`.
```
perf(world): optimize chunk loading for existing streaming
```

* Исправлены баги в игре или ошибки компиляции. В таком случае используется `fix`. При фиксе багов/ошибок логика поведения может некритично поменяться (но лучше использовать совместно с `refactor`), но добавление нового функционала нежелательно (используется с `feat` принудительно, если такое имеется).

## Важные уточнения
1. **_Как избежать тавтологии в_** `fix` (и других типах). Вместо того чтобы писать `fix(enemy): fix enemy bug`, используйте глаголы-синонимы, описывающие результат:

- Resolve (Разрешить проблему) — `fix(enemy): resolve freeze on death`
- Prevent (Предотвратить) — `fix(save): prevent data loss on crash`
- Handle (Обработать) — `fix(net): handle timeout exception`
- Ensure (Гарантировать) — `fix(ui): ensure button is clickable`

2. **_Масштабируемый_** `Scope`. Не пишите название файла или слишком подробно.<br>
НЕЛЬЗЯ: `fix(PlayerControllerUpdate): ...` (Слишком детально)<br>
НУЖНО: `fix(player): ...` (Кратко и понятно)<br>
Допускается использовать `player movement`, `main scene` и так далее

3. **_Breaking Changes_** (Критическое изменение). Если изменение ломает проект или старые сохранения, добавьте восклицательный знак после типа и опишите причину в теле коммита. Это увеличивает **MAJOR** (после релиза) или **MINOR** (до релиза) версию.
Пример:
```
feat(save)!: migrate from binary to JSON storage
```
4. **_Использование AI_**. Если затрудняетесь подобрать тип коммита и перечень значимых изменений, то опишите все изменения нейросети и получите отформатированный вариант, но проследите, чтобы коммит соответствовал всем вышеперечисленным правилам.
