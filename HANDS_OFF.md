# QA Test-Design Pipeline: Hands-off Description

## Короткий статус

Поточний репозиторій містить основу для напівавтоматичного test-design pipeline:

- skill для формування функціональних сценаріїв;
- skill для вибору тест-дизайн технік;
- skills для застосування окремих технік;
- skill для розподілу сценаріїв по рівнях тестової піраміди;
- Markdown-документи, які передають результати між етапами.

Однак повністю автоматичний процес виду `Jira ticket → готові автотести` ще не реалізований. Наразі немає єдиного orchestrator skill, автоматичного ticket intake, генератора коду автотестів або інтеграції з test-management системою.

## Загальна схема

```text
Ticket або вимоги
        ↓
Витягування вимог та evidence
        ↓
Інвентаризація feature/flow
        ↓
Аналіз шістьма lenses
        ↓
Вибір технік тест-дизайну
        ↓
Застосування обраних технік
        ↓
Формування функціональних сценаріїв
        ↓
Розподіл по рівнях test pyramid
        ↓
Фінальний пакет test cases
```

## Компоненти системи

### 1. `create-scenarios`

Файл:

```text
.claude/skills/create-scenarios/SKILL.md
```

Призначення:

- аналізувати feature або flow;
- збирати вимоги, acceptance criteria, документацію та доступні repository sources;
- створювати `SRC-*`, `FLOW-*`, `RULE-*` та `TC-*` IDs;
- перевіряти шість категорій сценаріїв;
- створювати traceable functional test scenarios;
- фіксувати assumptions, Questions/TBD та residual risks;
- не вигадувати application behavior.

Шість lenses:

| Lens | Діапазон ID | Що перевіряється |
| --- | --- | --- |
| Happy Path | `TC-001`–`TC-099` | Успішний business flow та completion result |
| Business Rules | `TC-100`–`TC-199` | Умови, правила, розрахунки, permissions та side effects |
| Security | `TC-200`–`TC-299` | Authentication, authorization, ownership, data exposure та manipulation |
| Negative/Error | `TC-300`–`TC-399` | Некоректні дані, rejected operations, wrong state, timeout та recovery |
| Edge Cases | `TC-400`–`TC-499` | Межі, limits, formats, timing, ordering, locale та precision |
| UI State | `TC-500`–`TC-599` | Loading, empty, error, validation, disabled, conditional та responsive states |

Результат записується у:

```text
docs/test-scenarios.md
```

Кожен сценарій має містити:

- стабільний ID;
- category;
- status;
- priority;
- requirement/source references;
- preconditions;
- test data/context;
- numbered steps;
- exact observable expected result;
- business rule;
- suggested test layer;
- technique tags;
- evidence to capture;
- cleanup/repeatability notes;
- assumptions та residual risks.

`Suggested Layer` у цьому документі є лише попередньою рекомендацією. Остаточне рішення приймає `test-strategy`.

### 2. `choose-technique`

Файл:

```text
.claude/skills/choose-technique/SKILL.md
```

Guide:

```text
docs/TestDesignAndSoftwareTestingTechniques/chooseTechnique.md
```

Призначення:

- визначати структуру вимоги;
- знаходити домінуючий ризик;
- обирати одну або декілька технік тест-дизайну;
- формувати rationale та hand-off до відповідних technique skills;
- не генерувати детальні test cases замість спеціалізованих skills.

Вибір базується не на тому, буде тест manual чи automated, а на структурі вимоги та характері ризику.

Типове зіставлення:

| Вимога або ризик | Техніка |
| --- | --- |
| Behaviorally distinct classes або categories | Equivalence Partitioning |
| Ordered limits, min/max, length, count або quota | Boundary Value Analysis |
| Joint conditions, rules, defaults, precedence або combinations | Decision Table Testing |
| Lifecycle, states, retries, expiration або event ordering | State-Transition Testing |
| Багато mostly independent factors | Pairwise Testing |
| Historical defects, legacy risks або unusual inputs | Error Guessing |
| Повний user journey | Use Case/Scenario Testing як complementary approach |
| Неповні вимоги або потреба в investigation | Exploratory Testing як complementary approach |

Приклад:

```text
Password length      → Equivalence Partitioning + Boundary Value Analysis
Discount conditions  → Decision Table Testing
Order lifecycle      → State-Transition Testing
Browser/OS/locale   → Pairwise Testing
Legacy malformed data→ Error Guessing
Full registration   → Use Case/Scenario Testing
```

Важливі залежності:

- EP зазвичай є основою для BVA;
- EP та BVA можуть постачати рівні для Decision Table або Pairwise;
- Decision Table може описувати складні guards для State-Transition;
- Error Guessing доповнює, але не замінює формальні техніки;
- exploratory, security, performance, accessibility, compatibility, reliability та usability testing не зводяться до вибору однієї black-box техніки.

### 3. Детальні technique skills та guides

Skills:

```text
.claude/skills/equivalence-partitioning/SKILL.md
.claude/skills/boundary-value-analysis/SKILL.md
.claude/skills/decision-table/SKILL.md
.claude/skills/pairwise-testing/SKILL.md
.claude/skills/state-transition-testing/SKILL.md
.claude/skills/error-guessing/SKILL.md
```

Guides:

```text
docs/TestDesignAndSoftwareTestingTechniques/BlackBox/equivalencePartitioning.md
docs/TestDesignAndSoftwareTestingTechniques/BlackBox/boundaryValueAnalysis.md
docs/TestDesignAndSoftwareTestingTechniques/BlackBox/decisionTable.md
docs/TestDesignAndSoftwareTestingTechniques/BlackBox/pairwiseTesting.md
docs/TestDesignAndSoftwareTestingTechniques/BlackBox/stateTransition.md
docs/TestDesignAndSoftwareTestingTechniques/BlackBox/errorGuessing.md
```

Обраний technique skill читає відповідний guide і застосовує його правила для створення:

- equivalence partitions;
- boundary values;
- decision-table rules;
- pairwise rows;
- state transitions;
- error hypotheses.

Результати мають зберігати traceability до `REQ-*`, `SRC-*`, `RULE-*`, `SEL-*` та `TC-*` IDs.

### 4. `test-strategy`

Файл:

```text
.claude/skills/test-strategy/SKILL.md
```

Призначення:

- читати `docs/test-scenarios.md`;
- перевіряти scenario IDs, категорії, references та exact oracles;
- знаходити реальні application sources та test harnesses у repository;
- призначати найнижчий adequate test-pyramid layer;
- пояснювати contested decisions;
- виявляти pyramid anti-patterns;
- описувати defense-in-depth для критичних правил.

Результат записується у:

```text
docs/test-strategy.md
```

## Як розподіляються тести по test pyramid

| Layer | Коли застосовується |
| --- | --- |
| Unit | Pure deterministic function, parser, mapper, calculation або rule без I/O, persistence, network, clock чи external dependencies |
| API/Integration | Backend/service rule, validator, serializer, persistence, authorization, event contract або API status/body/side-effect |
| Component | Ізольований UI rendering, interaction, validation display, loading/empty/error state або local UI state |
| E2E | Multi-page, full-stack, browser, external integration або critical business journey, де потрібно перевірити wiring між межами |

Правило вибору:

```text
Визначити потрібний oracle та boundaries
        ↓
Знайти найнижчий рівень, який може його перевірити
        ↓
Опустити pure logic, validation та contracts вниз
        ↓
Залишити обмежену кількість E2E для критичних cross-boundary flows
        ↓
Додати defense-in-depth лише для окремої додаткової перевірки
```

Приклади:

- Input validation зазвичай перевіряється на Unit або API/Integration.
- API status codes та response body перевіряються на API/Integration.
- UI-specific validation message може додатково перевірятися на Component.
- Повний registration або checkout journey може мати E2E тест.
- Calculation може мати Unit тест, а persistence та serialized representation — API/Integration тест.

Остаточний layer не визначається автоматично лише за назвою категорії. Він залежить від required oracle, меж поведінки, architecture evidence та наявності відповідного test harness.

## Повний процес для одного ticket

### Крок 1. Intake ticket

На вхід можуть подаватися:

- ticket summary;
- description;
- acceptance criteria;
- comments;
- linked issues;
- attached files;
- API contracts;
- domain documentation;
- code та existing tests, якщо вони доступні.

Потрібно відокремити:

- Confirmed facts;
- Assumptions;
- Questions/TBD;
- Residual risks.

Якщо ticket не містить достатньо інформації для exact oracle, не можна вигадувати очікувану поведінку.

### Крок 2. Scenario generation

Запускається `create-scenarios`.

Skill:

1. визначає scope;
2. інвентаризує sources;
3. створює `SRC-*`, `FLOW-*`, `RULE-*` IDs;
4. застосовує шість lenses;
5. формує `TC-*` scenarios;
6. додає priorities, preconditions, steps та exact oracles;
7. фіксує gaps, assumptions та residual risks;
8. записує результат у `docs/test-scenarios.md`.

### Крок 3. Technique selection

Для кожної вимоги або окремого risk area запускається `choose-technique`.

Skill аналізує:

- ranges та boundaries;
- classes of values;
- combinations of conditions;
- states та transitions;
- contextual/configuration factors;
- defect history;
- dominant risks.

Результатом є selection та hand-off до одного або декількох technique skills.

### Крок 4. Technique application

Обраний technique skill читає відповідний BlackBox guide і формує конкретну модель:

- EP partitions;
- BVA values;
- decision rules;
- pairwise combinations;
- state-transition paths;
- error-guessing hypotheses.

Детальна процедура не повинна дублюватися в `create-scenarios` або `choose-technique`.

### Крок 5. Scenario consolidation

Результати технік об’єднуються у `docs/test-scenarios.md`.

Потрібно:

- не створювати дублікати;
- зберігати стабільні IDs;
- пов’язувати scenarios з requirements та technique selections;
- відокремлювати legal positive cases від invalid/forbidden cases;
- зберігати exact oracle;
- не перетворювати невідоме на Confirmed behavior.

### Крок 6. Test-pyramid strategy

Запускається `test-strategy` після формування сценаріїв.

Він:

1. читає `docs/test-scenarios.md`;
2. перевіряє структуру scenario records;
3. сканує фактичні repository sources;
4. визначає required boundaries та oracle;
5. призначає Unit/API/Integration/Component/E2E;
6. перевіряє можливість push-down;
7. фіксує contested decisions;
8. аналізує defense-in-depth;
9. формує distribution table;
10. записує результат у `docs/test-strategy.md`.

### Крок 7. Final test-case package

Після двох документів доступна test-design інформація:

```text
docs/test-scenarios.md
        +
 docs/test-strategy.md
        +
technique-specific models
```

Вона визначає:

- що тестувати;
- якими даними;
- які кроки виконувати;
- який exact result очікувати;
- якою технікою спроєктовано case;
- на якому рівні виконувати тест.

## Що працює зараз

У поточному вигляді pipeline можна використовувати як напівавтоматичний процес:

```text
1. Передати Claude текст ticket або requirements.
2. Запустити create-scenarios.
3. Передати окремі requirement areas у choose-technique.
4. Запустити відповідні technique skills.
5. Оновити docs/test-scenarios.md.
6. Запустити test-strategy.
7. Отримати docs/test-strategy.md.
8. На основі документів сформувати final test cases.
```

Уже працюють правила та контракти для:

- шести lenses;
- стабільних scenario IDs;
- source traceability;
- exact oracles;
- no-invention policy;
- technique selection;
- application of formal black-box techniques;
- test-pyramid layer assignment;
- blocked state за відсутності evidence.

## Що ще не реалізовано

### 1. Єдиний orchestrator

Немає skill, який автоматично запускає весь процес однією командою.

Можливий майбутній файл:

```text
.claude/skills/test-pipeline/SKILL.md
```

### 2. Автоматичний ticket intake

Немає реалізованого адаптера, який автоматично:

- приймає Jira issue key;
- отримує issue details;
- читає comments, links та attachments;
- збирає пов’язану документацію;
- знаходить code/test evidence;
- передає нормалізований контекст у наступні skills.

Ticket можна передати текстом або отримати через доступний Jira connector/MCP, але автоматичний запуск повного ланцюга ще не налаштований.

### 3. Автоматичний запуск skills

Немає окремої orchestration logic, яка послідовно викликає:

```text
create-scenarios
→ choose-technique
→ detailed technique skill(s)
→ test-strategy
```

### 4. Генерація automation code

У repository немає окремого `generate-tests` skill або exporter-ів для:

- Playwright;
- Jest/Vitest;
- API test frameworks;
- Postman;
- Gherkin/Cucumber;
- TestRail;
- Zephyr;
- Jira Xray;
- CSV/JSON test-management formats.

Поточні документи є test-design та strategy artifacts, а не готовим executable automation code.

### 5. Machine-readable artifact storage

Наразі основний формат — Markdown. Для повністю автоматичного pipeline бажано додати нормалізовані артефакти, наприклад:

```text
artifacts/
  requirements.json
  technique-selection.json
  test-scenarios.json
  test-strategy.json
```

Markdown може залишатися human-readable presentation layer.

## Безпечна поведінка при відсутності джерел

У поточному repository немає application backend, frontend, API implementation або test suite. Тому початкові documents працюють у blocked state:

- `docs/test-scenarios.md` не містить вигаданих `TC-*` cases;
- `docs/test-strategy.md` не містить вигаданих function, endpoint, component або test references;
- невідомі джерела позначаються як `Question/TBD`;
- невідомі ризики фіксуються як `Residual risk`;
- execution time позначається як `Unknown`;
- layer assignment без достатнього evidence залишається `Pending`;
- zero cases не означає, що application не має behavior або defects.

## Важливі обмеження

1. Один ticket може містити кілька flows, risks, techniques та test layers.
2. Одна техніка не покриває всю feature автоматично.
3. `Suggested Layer` зі scenario document не є остаточним рішенням.
4. Test count не є доказом повноти.
5. 100% coverage у вибраній моделі не доводить відсутність усіх defects.
6. Звичайний Unit/API/Component/E2E assignment не доводить security, performance, accessibility, reliability, compatibility або usability coverage.
7. E2E не повинен використовуватися для всіх перевірок.
8. Security або non-functional risks можуть вимагати окремих спеціалізованих follow-ups.
9. За відсутності exact oracle scenario не може бути безпечно підтверджений.
10. За відсутності application source неможливо правдиво послатися на функцію, endpoint, component або test harness.

## Бажаний майбутній orchestrator

Для повністю автоматичного pipeline потрібен окремий `test-pipeline` skill з приблизною логікою:

```text
1. Прийняти Jira ticket key або текст ticket.
2. Отримати ticket та доступні linked evidence.
3. Нормалізувати requirements, actors, rules, risks та oracle.
4. Запустити create-scenarios.
5. Для кожної requirement area запустити choose-technique.
6. Передати hand-offs у відповідні technique skills.
7. Об'єднати результати у docs/test-scenarios.md.
8. Запустити test-strategy.
9. Об'єднати layer decisions у docs/test-strategy.md.
10. Сформувати фінальний test-case package.
11. Опційно експортувати cases у Jira/Xray/TestRail або automation framework.
```

Один ticket може мати такий результат:

```text
Ticket
  ├── FLOW-001
  │     ├── TC-001 Happy Path → E2E
  │     ├── TC-100 Business Rule → API/Integration
  │     ├── TC-300 Negative → API/Integration
  │     └── TC-500 UI State → Component
  ├── FLOW-002
  │     ├── TC-101 Rule boundary → Unit
  │     └── TC-401 Limit case → API/Integration
  └── Residual risks / Questions / Follow-ups
```

Це означає, що ticket не “відноситься” лише до одного рівня. Він розкладається на окремі перевірювані behaviors, і кожен behavior отримує відповідний test layer.

## Підсумок

Поточна архітектура вже задає необхідну логіку test design:

```text
Requirements
→ scenarios
→ technique selection
→ technique application
→ scenario consolidation
→ pyramid strategy
```

Але сьогодні це набір узгоджених skills та документів, які потрібно запускати послідовно. Для режиму `ticket → повний test package` необхідно додати orchestrator, ticket integration, machine-readable schemas та, за потреби, automation/test-management exporters.
