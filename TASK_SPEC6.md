ти Senior QA, який пояснює як вірно обрати техніку тест дизайну для оптимальної підготовки тесткейсів, коли яку застосовувати і за якими ознаками

в  папці /Users/romanmakarenko/Documents/code/QAHelperPipeline/docs/TestDesignAndSoftwareTestingTechniques/BlackBox описані техніки:
Boundary Value Analysis (BVA)
Decision Table Testing
Equivalence Partitioning
Error Guessing Test Design
Pairwise Testing
State-Transition Testing

ти повинен описати чіткі критерії вибору техніки або технік до задачі, описати причини, характерні ознаки які вказують на необхідність певної техніки та приклади

ось варіант, структурований опрацьований варіант тексту запиши в /Users/romanmakarenko/Documents/code/QAHelperPipeline/docs/TestDesignAndSoftwareTestingTechniques/chooseTechnique.md
ще важливо - описану доку буде використовувати однойменний скіл який буде в залежності від задачі обирати один з існуючих найвідповідніших до задачі скілів застосування техніки /Users/romanmakarenko/Documents/code/QAHelperPipeline/.claude/skills

Так. Як Senior QA Automation я б радив обирати техніку не за принципом “яку знаю — ту й використовую”, а відштовхуватися від типу вимоги та характеру даних.

Головний алгоритм вибору
Що є у вимозі	Основна техніка
Діапазон числових/кількісних значень	Boundary Value Analysis (BVA)
Різні категорії/класи значень	Equivalence Partitioning (EP)
Багато комбінацій умов → різні результати	Decision Table Testing
Сценарій залежить від попереднього стану	State Transition Testing
Потрібно перевірити взаємодію параметрів	Pairwise Testing
Складна бізнес-логіка з правилами	Decision Table + EP/BVA
Користувацький сценарій від початку до кінця	Use Case / Scenario Testing
Немає чітких вимог, потрібно досліджувати	Exploratory Testing / Error Guessing
1. Equivalence Partitioning — коли значень багато

Ідея: розділити всі можливі значення на групи, всередині яких система повинна поводитися однаково.

Наприклад:

Вік користувача: від 18 до 60 років.

Можемо отримати:

< 18 — невалідний клас
18–60 — валідний клас
> 60 — невалідний клас

Замість 100+ тестів беремо представника кожного класу:

17 → invalid
30 → valid
61 → invalid
Коли використовувати

Коли вимога говорить про:

діапазони;
типи даних;
категорії;
формати;
допустимі/недопустимі значення.

EP відповідає на питання:

"Які групи значень мені потрібно перевірити?"

2. Boundary Value Analysis — коли важливі межі

Якщо є діапазон:

Password length: 8–20 characters

То найбільш цікаві значення знаходяться біля меж:

7   → invalid
8   → valid
9   → valid

19  → valid
20  → valid
21  → invalid

BVA особливо корисний тому, що помилки дуже часто виникають саме на межах.

Класичний набір

Для межі 8:

7
8
9

Для межі 20:

19
20
21
Коли використовувати

Коли бачиш:

min/max;
from/to;
не більше;
не менше;
більше/менше;
довжину;
кількість;
розмір;
ліміти.

BVA відповідає на питання:

"Які значення біля межі найбільш ризикові?"

3. EP + BVA — дуже часта комбінація

У реальному тестуванні їх часто використовують разом.

Наприклад:

Сума платежу від $10 до $1000.

EP:

< 10       invalid
10–1000    valid
> 1000     invalid

BVA:

9
10
11

999
1000
1001

Тобто:

EP визначає класи, BVA — конкретні критичні значення.

4. Decision Table — коли є багато умов

Ось тут дуже часто помиляються новачки.

Наприклад:

Користувач може отримати знижку, якщо:

він premium;
сума покупки > $100;
промокод валідний.

Маємо 3 умови:

Premium?
Amount > 100?
Promo valid?

І результат:

Discount?

Тут добре працює Decision Table.

Наприклад:

Premium	> $100	Promo valid	Discount
No	No	No	No
No	Yes	Yes	No
Yes	No	Yes	Yes
Yes	Yes	Yes	Yes
...	...	...	...
Коли використовувати

Коли бачиш слова:

AND;
OR;
IF;
ELSE;
якщо;
і;
або;
тільки якщо;
залежить від декількох умов.

Decision Table відповідає на питання:

"Які комбінації умов можуть впливати на результат?"

5. State Transition — коли система має стани

Наприклад, банківська картка:

Active
↓ wrong PIN
Blocked

Або замовлення:

Created
↓
Paid
↓
Processing
↓
Shipped
↓
Delivered

Тут важливо перевіряти не просто значення, а переходи між станами.

Наприклад:

Created → Paid       valid
Paid → Processing    valid
Processing → Shipped valid
Delivered → Paid     invalid
Особливо добре для
login;
account status;
order status;
payment;
workflow;
блокування;
retry mechanisms;
FSM-подібної логіки.

State Transition відповідає на питання:

"Що станеться, якщо система зараз перебуває в конкретному стані і відбудеться певна дія?"

6. Pairwise Testing — коли багато параметрів

Уявімо, що вебзастосунок підтримує:

Browser:
Chrome / Firefox / Safari

OS:
Windows / macOS / Linux

Language:
UA / EN / PL

Payment:
Visa / Mastercard / PayPal

Повний перебір:

3 × 3 × 3 × 3 = 81 комбінація.

Це дорого.

Pairwise дозволяє підібрати значно менший набір тестів так, щоб кожна пара значень параметрів зустрілася хоча б один раз.

Коли використовувати

Коли є багато незалежних параметрів:

browser;
OS;
device;
language;
payment method;
user role;
configuration;
feature flags.

Pairwise відповідає на питання:

"Як зменшити кількість комбінацій, але зберегти хороше покриття взаємодій?"

7. Use Case / Scenario Testing

Тут ми тестуємо бізнес-сценарій цілком.

Наприклад, інтернет-магазин:

Login
↓
Search product
↓
Add to cart
↓
Checkout
↓
Payment
↓
Order confirmation

Це вже не просто перевірка одного поля.

Ми перевіряємо:

"Чи може користувач успішно виконати бізнес-ціль від початку до кінця?"

Коли використовувати
критичні user journeys;
checkout;
registration;
booking;
payment;
onboarding;
складні бізнес-процеси.
8. Error Guessing

Це вже більше про досвід тестувальника.

Ти думаєш:

"Де розробник найімовірніше міг помилитися?"

Наприклад:

email:
test@test.com
test@
@test.com
test
test@test

Або:

password:
empty
spaces only
very long
special characters
Unicode

Тут немає чіткої математичної моделі — ти використовуєш досвід, історію дефектів і типові помилки.

Коли використовувати

Практично завжди як доповнення до формальних технік.

Особливо коли:

вимоги неповні;
мало часу;
система legacy;
є відома історія багів.
9. Exploratory Testing

Це не зовсім класична техніка тест-дизайну на кшталт BVA/EP, а підхід до тестування.

Ти одночасно:

learn → design → execute → investigate

Например, знайшов дивну поведінку:

після зміни мови під час checkout зникає payment method.

Ти починаєш досліджувати пов'язані сценарії.

Коли особливо корисно
нова функціональність;
недостатні вимоги;
нестабільна система;
investigation;
regression discovery;
пошук unexpected behavior.
Як це вибирати на співбесіді

Я б використовував ось таку ментальну шпаргалку:

Є діапазон?
↓
BVA
+
EP

Є класи значень?
↓
EP

Є багато умов?
↓
Decision Table

Є стани?
↓
State Transition

Є багато параметрів?
↓
Pairwise

Є end-to-end бізнес-сценарій?
↓
Use Case Testing

Немає чітких вимог / шукаємо типові баги?
↓
Error Guessing
+
Exploratory
Найважливіше для Senior QA

Не потрібно вибирати одну техніку на всю фічу.

Наприклад, є форма реєстрації:

Password: 8–20 символів

→ EP + BVA

Email: валідний/невалідний формат

→ EP

Registration залежить від:

Age
Country
Email verified
Promo code

→ Decision Table

Registration підтримує:

Chrome
Firefox
Safari

Windows
macOS

→ Pairwise

Після 5 неправильних паролів акаунт блокується

→ State Transition + BVA

Користувач повинен пройти:

Registration
→ Email verification
→ Login
→ Profile

→ Use Case

А потім:

→ Error Guessing / Exploratory для edge cases, які не покрили формальні техніки.

Тобто Senior-підхід виглядає так:

Requirements → identify risk/logic → select technique → minimize redundant cases → maximize meaningful coverage.

І головне: техніка тест-дизайну обирається від структури вимоги, а не від типу тесту (manual/automation). Автоматизувати можна тест-кейси, отримані будь-якою з цих технік.