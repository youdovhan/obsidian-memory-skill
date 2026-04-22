---
name: obsidian-memory
description: >
  Set up Obsidian as Claude Code's long-term memory vault. Use when the user
  says "install Obsidian as my memory", "set up my second brain", "configure
  Obsidian for Claude", "make Claude remember things in Obsidian", or wants a
  structured knowledge vault that Claude reads across sessions. Creates the
  full folder architecture, navigation files, templates, session protocols,
  and CLAUDE.md integration so Claude treats the vault as durable memory.
---

# Obsidian Memory Vault — Setup Skill

Налаштовує Obsidian vault як **довгострокову пам'ять** для Claude Code. Після виконання цього скіла у користувача буде:

- Структурована vault з правильною ієрархією (не хаос нотаток)
- Навігаційні файли (INDEX + MOC) які Claude читає першими
- Темплейти для людей, проєктів, клієнтів, щоденних звітів
- Протокол коли зберігати і коли читати
- `CLAUDE.md` з інтеграцією vault у кожну сесію
- (Опціонально) MCP Obsidian сервер для прямого доступу
- (Опціонально) Хуки для автоматичних записів

**Це не просто папка з замітками — це операційна система пам'яті.** Vault стає джерелом правди про людей, проєкти, рішення, уроки.

---

## Як використовувати цей скіл

Коли користувач просить "налаштуй Obsidian як пам'ять" — йди по кроках нижче. Не пропускай Крок 0: якщо не зібрати базові параметри, скіл створить vault не там, де треба.

### Крок 0 — Зібрати параметри

Запитай у користувача (одним повідомленням, не по одному):

1. **Шлях до vault** — куди створювати? Якщо є існуючий Obsidian vault — використати його. Якщо ні — запропонувати шлях (наприклад `~/ObsidianVault` на Mac/Linux, `C:\ObsidianVault` на Windows).
2. **Мова контенту** — якою мовою писати темплейти та навігацію? (EN / UA / RU / ES / інша)
3. **Основне використання** — особиста пам'ять / робота з клієнтами (CRM) / технічні проєкти / все разом?
4. **MCP Obsidian** — встановлювати MCP сервер для прямого читання/запису через Obsidian API? (Y/N — якщо користувач не знає що це, за замовчуванням **ні**, vault буде читатися через файлову систему, цього достатньо)
5. **Хуки автоматики** — налаштувати session-start та session-stop хуки для автозавантаження контексту? (Y/N — за замовчуванням **ні**, це складніше і не всім треба)

Якщо користувач не відповів на частину — став розумні дефолти і повідом про них.

### Крок 1 — Створити структуру папок

Адаптуй за обраним use case (з Кроку 0.3):

**Мінімальна (особиста пам'ять):**
```
vault/
├── 00-INDEX.md
├── MANIFEST.md
├── Memory/
│   ├── Active Tasks.md
│   ├── Session Log.md
│   ├── Lessons Learned.md
│   └── Daily Reports/
├── People/
├── Projects/
├── Rules/
└── Templates/
```

**Повна (робота + CRM + проєкти):**
```
vault/
├── 00-INDEX.md                     ← Точка входу, читається ПЕРШИМ
├── MANIFEST.md                     ← Повний індекс vault
├── Memory/                         ← Ядро пам'яті
│   ├── Active Tasks.md             — незакінчені задачі
│   ├── Session Log.md              — що робили в кожній сесії
│   ├── Lessons Learned.md          — помилки + висновки
│   ├── Skills Registry.md          — коли який скіл/агент викликати
│   └── Daily Reports/              — щоденні звіти
├── Rules/                          ← Операційні правила
│   ├── Rules MOC.md                — індекс правил
│   ├── Session Protocol.md         — як починати/завершувати сесію
│   ├── Language Rules.md           — мова відповідей та контенту
│   └── Content Rules.md            — правила створення контенту
├── People/                         ← CRM
│   ├── Clients MOC.md              — точка входу в CRM
│   ├── Clients/                    — активні клієнти (🟡🟢🔴)
│   ├── Partners/                   — партнери
│   └── Team/                       — команда
├── Projects/                       ← Технічні / робочі проєкти
│   ├── Projects MOC.md             — індекс проєктів
│   └── Dashboard.md                — живий статус всіх проєктів
├── Business/                       ← (Опціонально) продукти, ціни, офери
│   └── Business MOC.md
├── Templates/                      ← Шаблони для Obsidian Template plugin
│   ├── Person.md
│   ├── Client.md
│   ├── Project.md
│   └── Daily Report.md
└── Credentials/                    ← (Опціонально) плейсхолдери для токенів
```

Створи всі папки через `mkdir -p` (Unix) або `New-Item -ItemType Directory` (PowerShell). **Папки без `.gitkeep` Obsidian приховує — не проблема, контент з'явиться коли створиш файли.**

### Крок 2 — Створити навігаційні файли

Це **найважливіший крок**. Без цих файлів Claude не знатиме що читати першим.

Створи ці файли з контенту темплейтів у `templates/`:

1. **`00-INDEX.md`** — копія з [templates/00-INDEX.md](templates/00-INDEX.md), адаптована під обрану структуру. Це точка входу.
2. **`MANIFEST.md`** — повний індекс усіх файлів vault з коротким описом кожного (автоматично заповнюється, далі оновлюється вручну).
3. **`Memory/Active Tasks.md`** — з [templates/Active-Tasks.md](templates/Active-Tasks.md). Порожній шаблон з секціями Гарячі/Продажі/Продукт/Технічне.
4. **`Memory/Session Log.md`** — з [templates/Session-Log.md](templates/Session-Log.md). Порожній журнал сесій.
5. **`Memory/Lessons Learned.md`** — з [templates/Lessons-Learned.md](templates/Lessons-Learned.md). Порожній список уроків.
6. **MOC-файли** (Maps of Content) — індекси для кожної категорії: `Clients MOC.md`, `Projects MOC.md`, `Rules MOC.md`, `Business MOC.md`. Беруть з [templates/MOC-template.md](templates/MOC-template.md).

Важливо: **переклади заголовки і описи на мову з Кроку 0.2**, але структуру не міняй.

### Крок 3 — Скопіювати темплейти

Скопіюй у `vault/Templates/`:

- `Person.md` — універсальний шаблон контакту
- `Client.md` — CRM-картка клієнта зі статусами
- `Project.md` — картка проєкту з архітектурними рішеннями
- `Daily Report.md` — щоденний звіт

Усі — з папки `templates/` цього скіла.

Підкажи користувачу як активувати Obsidian Template plugin: Settings → Core plugins → Templates → enable → встановити template folder location = `Templates`.

### Крок 4 — Створити `CLAUDE.md` інтеграції

Це критично. `CLAUDE.md` — це project instructions які Claude Code читає при старті кожної сесії в цій папці. Він має містити посилання на vault.

**Де створювати:**

- Якщо vault **це і є** робоча папка користувача → `CLAUDE.md` в корені vault
- Якщо vault **окремо** від робочих папок → `CLAUDE.md` у кожній робочій папці з яких треба мати доступ до пам'яті, АБО глобальний `~/.claude/CLAUDE.md`

Візьми [examples/CLAUDE-md-example.md](examples/CLAUDE-md-example.md), підстав фактичний шлях до vault користувача, адаптуй мову.

Мінімальний зміст:
```markdown
## Пам'ять — Obsidian vault

- **Шлях:** `<повний шлях до vault>`
- **Карта vault:** `00-INDEX.md` ← читай ПЕРШИМ при старті сесії
- **Незакінчені задачі:** `Memory/Active Tasks.md`
- **Уроки з помилок:** `Memory/Lessons Learned.md`

## Протокол пам'яті

### Коли ЧИТАТИ (старт сесії):
1. `00-INDEX.md` → огляд структури
2. `Memory/Active Tasks.md` → що в роботі
3. Останній Daily Report (якщо потрібен контекст "що було вчора")

### Коли ЗБЕРІГАТИ (без питань):
- Нова людина → `People/{Name}.md` + оновити MOC
- Нове бізнес-рішення → `Business/{Name}.md`
- Нове правило → `Rules/{Name}.md`
- Технічне рішення → `Projects/{Name}.md`
- Помилка + висновок → `Memory/Lessons Learned.md`
- Кінець сесії → оновити `Memory/Active Tasks.md` + `Memory/Session Log.md`
```

### Крок 5 (опціонально) — MCP Obsidian сервер

Якщо користувач в Кроці 0.4 обрав Y — встанови MCP сервер. Є два популярні:

**Варіант A — `obsidian-mcp` (рекомендується, простіший):**
```bash
claude mcp add obsidian npx -y obsidian-mcp <шлях_до_vault>
```

**Варіант B — `mcp-obsidian` (через REST API плагін):**
1. В Obsidian: Settings → Community plugins → пошук "Local REST API" → install + enable
2. Згенеруй API ключ у налаштуваннях плагіну
3. `claude mcp add obsidian -e OBSIDIAN_API_KEY=<key> -- npx -y mcp-obsidian`

Перевір встановлення: `claude mcp list` → має бути `obsidian` зі статусом connected.

Плюси MCP: прямі API запити до vault (швидше за `cat` для великих операцій), розуміння wikilinks, пошук по тегах. Мінуси: ще один процес, залежність від Node.js.

**Без MCP теж працює.** Claude читає vault через Read/Grep/Glob — цього достатньо для 90% задач.

### Крок 6 (опціонально) — Хуки автоматики

Якщо користувач у Кроці 0.5 обрав Y — налаштуй session-start хук який автоматично вставляє в контекст:
- Поточні Active Tasks
- Останній Daily Report
- Останні 5 lessons

Детальна інструкція: [references/hooks-setup.md](references/hooks-setup.md).

**Не нав'язуй хуки.** Вони корисні при активному використанні, але додають складності. Краще запропонувати на пізніше, коли користувач розбереться з базовим vault.

### Крок 7 — Фінальна перевірка

1. Перевір що всі папки створені: `ls -la <vault>`
2. Перевір що `00-INDEX.md` відкривається і має правильні посилання
3. Якщо встановлений MCP — перевір `claude mcp list`
4. Покажи користувачу **що робити далі**:
   - Відкрити vault в Obsidian (File → Open vault → обрати папку)
   - Увімкнути Community plugins якщо треба
   - Почати сесію з Claude Code у папці з `CLAUDE.md` — Claude має автоматично прочитати vault

### Крок 8 — Завершити

Виведи короткий підсумок:
- ✅ Створено: N папок, M файлів
- ✅ Шлях до vault: `<path>`
- ✅ CLAUDE.md в: `<path>`
- ⚠️ Що зробити вручну (якщо є): відкрити vault в Obsidian, увімкнути плагіни
- 📖 Наступні кроки: почати заповнювати People/, Projects/

---

## Принципи архітектури (чому саме так)

### 3 рівні пам'яті

| Рівень | Система | Що зберігає | Життєвий цикл |
|--------|---------|-------------|---------------|
| Швидкий | Claude Memory (`~/.claude/.../memory/`) | Поведінкові правки, bridge | Між сесіями |
| Довгостроковий | **Obsidian vault** | Люди, правила, проєкти, уроки | Назавжди |
| Операційний | Notion / Todoist / Linear | Задачі, статуси, контент | Робочий |

Obsidian — саме **довгостроковий**. Туди йдуть речі які мають пережити десятки сесій: хто клієнт, що вирішили, які помилки не повторювати.

### Чому MOC, а не теги

MOC (Map of Content) — явний індексний файл. Теги — неявні фільтри.

Claude швидше орієнтується по явних посиланнях: `[[Clients MOC]]` → `[[John Doe]]`, ніж по тегах `#client`. Теги додаються додатково, не замість.

### Чому `00-INDEX.md` а не `README.md`

Префікс `00-` підіймає файл на верх сортування. Obsidian бічна панель завжди показує його першим. Claude теж завжди читає як першу точку входу.

### Чому `People/Clients/` а не `Clients/`

Все про людей — в одному місці. CRM, партнери, команда — всі під `People/`. Так Claude не плутається коли треба знайти контакт, і немає дублювання (людина одночасно клієнт і партнер — один файл).

### Чому Active Tasks не в Notion

**Active Tasks — короткий мутабельний список "що зараз в роботі"** для Claude. Він читається при старті кожної сесії. Якщо покласти в Notion — буде зайвий API-запит і зайва синхронізація. У vault — `cat Memory/Active\ Tasks.md` і готово.

Notion лишається для розгорнутого project management з командою. Active Tasks — для AI.

---

## Типові помилки

**Помилка 1: Плоска структура без MOC**
> Користувач створює vault і кидає туди 200 файлів у корінь. Claude не може знайти що читати.

Фікс: завжди створювати MOC-файли. Навіть якщо зараз 2 клієнти — `Clients MOC.md` має існувати.

**Помилка 2: CLAUDE.md без шляху до vault**
> `CLAUDE.md` написано "у мене є Obsidian vault з пам'яттю", але без шляху. Claude не знає де він.

Фікс: завжди вказувати абсолютний шлях. Мінімум: `Obsidian vault: /Users/x/ObsidianVault/`.

**Помилка 3: Темплейти в корені vault**
> Темплейт-файли змішуються з реальними нотатками. Плутанина.

Фікс: всі темплейти строго в `Templates/`. Obsidian Templates plugin треба налаштувати на цю папку.

**Помилка 4: Дублювання між Claude Memory і Obsidian**
> Одна й та ж інформація зберігається і в `~/.claude/.../memory/`, і в vault.

Фікс: чіткий розподіл. Швидкі поведінкові правила + bridge — Claude Memory. Довгостроковий контент — Obsidian. Поріг: якщо запис переживе 10+ сесій — Obsidian.

**Помилка 5: Vault без Git**
> Користувач рік працює з vault, жодного бекапу. Один неправильний MCP-write все стирає.

Фікс: `git init` в vault одразу ж. Періодичні комміти. Навіть локально без push.

---

## Що НЕ робити

- **Не клади секрети в vault.** API-токени, паролі — в `Credentials/` тільки як плейсхолдери ("API-ключ Stripe — шукай в 1Password") АБО взагалі не в vault.
- **Не створюй файли "ProjectX - Copy 2 final (2).md".** Vault — жива система. Якщо треба версія — це git.
- **Не роби vault публічним** поки не вичистиш імена людей і бізнес-деталі.
- **Не став MCP Obsidian** якщо не плануєш ним користуватися — лишні налаштування.
- **Не створюй 50 тегів одразу.** Теги додаються по мірі потреби, не наперед.

---

## Додаткові ресурси

- [references/architecture.md](references/architecture.md) — детально про 3-рівневу пам'ять
- [references/session-protocol.md](references/session-protocol.md) — правила старту/завершення сесії
- [references/hooks-setup.md](references/hooks-setup.md) — налаштування автоматичних хуків
- [references/mcp-setup.md](references/mcp-setup.md) — детально про MCP Obsidian
- [examples/CLAUDE-md-example.md](examples/CLAUDE-md-example.md) — приклад `CLAUDE.md` з інтеграцією vault
- [examples/example-vault-tree.md](examples/example-vault-tree.md) — приклад заповненого vault

---

## Інтеграція з іншими скілами

Цей скіл створює **фундамент пам'яті**. Інші скіли можуть писати у vault:

- Скіли CRM → пишуть в `People/Clients/`
- Скіли звітування → пишуть в `Memory/Daily Reports/`
- Скіли lessons learned → пишуть в `Memory/Lessons Learned.md`

Структура vault гарантує що ці записи не конфліктуватимуть.
