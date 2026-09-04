# QA Test-Design Pipeline: Hands-off Description

## Короткий статус

Поточний репозиторій містить основу для напівавтоматичного test-design pipeline:

- skill для формування функціональних сценаріїв;
- skill для вибору тест-дизайн технік;
- skills для застосування окремих технік;
- skill для розподілу сценаріїв по рівнях тестової піраміди;
- Markdown-документи, які передають результати між етапами.

Повністю автоматичний процес виду `Jira ticket → готові автотести` ще не реалізований. Guided orchestrator skill `.claude/skills/test-pipeline/SKILL.md` уже існує та координує hand-offs у межах Claude session, але автоматичного ticket intake, executable workflow engine, генератора коду автотестів або інтеграції з test-management системою немає.

За замовчуванням результати ticket/run зберігаються ізольовано в `artifacts/test-pipeline/<ticket-or-feature-slug>/<run-id>/`. Спільні `docs/test-scenarios.md` і `docs/test-strategy.md` залишаються project-level contracts/templates або явно запитаними legacy projections.

> Цей документ містить історичні hand-off notes. У місцях, де нижче згадуються майбутні компоненти, враховуйте актуальний статус цього розділу та `README.md`.

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

У direct/shared режимі результат може записуватися у:

```text
docs/test-scenarios.md
```

Під час `test-pipeline` run canonical output записується у run-local `artifacts/test-pipeline/<ticket-or-feature-slug>/<run-id>/test-scenarios.md`; shared file не змінюється без explicit legacy projection.

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

- читати run-local `test-scenarios.md` під час orchestrated ticket run або `docs/test-scenarios.md` у direct/shared режимі;
- перевіряти scenario IDs, категорії, references та exact oracles;
- знаходити реальні application sources та test harnesses у repository;
- призначати найнижчий adequate test-pyramid layer;
- пояснювати contested decisions;
- виявляти pyramid anti-patterns;
- описувати defense-in-depth для критичних правил.

У direct/shared режимі результат може записуватися у:

```text
docs/test-strategy.md
```

Під час `test-pipeline` run canonical output записується у run-local `artifacts/test-pipeline/<ticket-or-feature-slug>/<run-id>/test-strategy.md`; shared file не змінюється без explicit legacy projection.

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

Для повного guided run використовується `.claude/skills/test-pipeline/SKILL.md`; окремі skills також можна запускати напряму.

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

Запускається `create-scenarios`. У повному orchestrated run він отримує explicit run-local destination; його documented shared default застосовується лише у direct/shared режимі.

Skill:

1. визначає scope;
2. інвентаризує sources;
3. створює `SRC-*`, `FLOW-*`, `RULE-*` IDs;
4. застосовує шість lenses;
5. формує `TC-*` scenarios;
6. додає priorities, preconditions, steps та exact oracles;
7. фіксує gaps, assumptions та residual risks;
8. у direct/shared режимі записує результат у `docs/test-scenarios.md`; під час orchestrated run записує його у run-local `artifacts/test-pipeline/<ticket-or-feature-slug>/<run-id>/test-scenarios.md`.

Усі ticket-specific outputs мають залишатися в межах run directory; shared document є legacy projection лише за explicit request.

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

Результати технік об’єднуються у run-local `artifacts/test-pipeline/<ticket-or-feature-slug>/<run-id>/test-scenarios.md` під час orchestrated run. У direct/shared режимі допустимий `docs/test-scenarios.md`; це legacy projection, а не canonical ticket history.

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

1. під час orchestrated run читає run-local `artifacts/test-pipeline/<ticket-or-feature-slug>/<run-id>/test-scenarios.md`; у direct/shared режимі читає `docs/test-scenarios.md`;
2. перевіряє структуру scenario records;
3. сканує фактичні repository sources;
4. визначає required boundaries та oracle;
5. призначає Unit/API/Integration/Component/E2E;
6. перевіряє можливість push-down;
7. фіксує contested decisions;
8. аналізує defense-in-depth;
9. формує distribution table;
10. під час orchestrated run записує результат у run-local `artifacts/test-pipeline/<ticket-or-feature-slug>/<run-id>/test-strategy.md`; у direct/shared режимі записує `docs/test-strategy.md`.

Усі ticket-specific outputs мають залишатися в межах run directory; shared document є legacy projection лише за explicit request.

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

У поточному вигляді pipeline можна використовувати як guided напівавтоматичний процес:

```text
1. Передати Claude текст ticket або requirements.
2. Запустити `.claude/skills/test-pipeline/SKILL.md` або окремі child skills.
3. Для orchestrated run зберігати hand-offs у `artifacts/test-pipeline/<ticket-or-feature-slug>/<run-id>/`.
4. У direct/shared режимі явно використовувати `docs/test-scenarios.md` та `docs/test-strategy.md`.
5. На основі документів сформувати final test cases.
```

`test-pipeline` координує послідовність, але не є executable workflow engine; `test-strategy` має intentional `disable-model-invocation: true`, тому orchestrator застосовує його contract у run-local контексті.

Для автоматично керованого runtime pipeline ще потрібні окремі інтеграції та execution tooling.

Запуск окремого `test-strategy` у direct/shared режимі:

```text
/test-strategy <feature або scope>
```

Результат direct/shared запуску зберігається у `docs/test-strategy.md` лише після explicit request; orchestrated run використовує run-local artifact.

Версії skills позначені custom metadata `version: 0.1.0` і мають записуватися у run metadata лише як зафіксоване значення frontmatter, а не як автоматична runtime capability.

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

### 1. Executable orchestration engine

Guided coordinator `.claude/skills/test-pipeline/SKILL.md` уже реалізований. Він описує та координує hand-offs у межах Claude session, але не є окремим runtime engine, який програмно викликає child skills або запускає application tests.

Залишаються нереалізованими автоматичний workflow execution, Jira intake без доступного connector-а, automation-code generation і test-management export.

### 2. Автоматичний ticket intake

Немає реалізованого адаптера, який автоматично:

- приймає Jira issue key;
- отримує issue details;
- читає comments, links та attachments;
- збирає пов’язану документацію;
- знаходить code/test evidence;
- передає нормалізований контекст у наступні skills.

Ticket можна передати текстом або отримати через доступний Jira connector/MCP, але автоматичний запуск повного ланцюга ще не налаштований.

### 3. Executable automatic skill invocation

Guided `test-pipeline` описує послідовність і hand-offs, але немає окремої executable orchestration logic, яка програмно викликає:

```text
create-scenarios
→ choose-technique
→ detailed technique skill(s)
→ test-strategy
```

Це означає, що child skills не запускаються як runtime jobs без участі Claude session.

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

## Бажані майбутні інтеграції та execution tooling

Guided orchestrator уже має логіку, описану в `.claude/skills/test-pipeline/SKILL.md`. Для повністю автоматичного pipeline ще потрібні окремі capability-и:

```text
1. Прийняти Jira ticket key через доступний connector.
2. Отримати ticket та доступні linked evidence.
3. Нормалізувати requirements, actors, rules, risks та oracle.
4. Викликати child skills через executable orchestration layer.
5. Передати hand-offs у відповідні technique skills.
6. Зберегти canonical outputs у run-local artifacts/test-pipeline/<slug>/<run-id>/.
7. Запустити application test executor або automation-code generator.
8. Опційно експортувати cases у Jira/Xray/TestRail або automation framework.
```

Ці capability-и не є частиною поточного guided skill і потребують окремої реалізації та explicit integrations.

Legacy приклади нижче ілюструють можливу структуру результату; вони не є execution evidence і не змінюють canonical artifact policy.

Один ticket може мати такий результат:

```text
Ticket
  ├── FLOW-001
  │     ├── TC-001 Happy Path → E2E
  │     ├── TC-100 Business Rules → API/Integration
  │     ├── TC-300 Negative/Error → API/Integration
  │     └── TC-500 UI State → Component
  ├── FLOW-002
  │     ├── TC-101 Business Rules boundary → Unit
  │     └── TC-401 Edge Cases limit → API/Integration
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

Guided orchestrator уже координує цей design flow. Для режиму `ticket → повний test package` необхідні додаткові ticket integration, executable test tooling, machine-readable schemas та, за потреби, automation/test-management exporters.
