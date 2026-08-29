Dynamic - Experience based

Error Guessing;

Предположение об ошибках (EG - error guessing): Метод проектирования тестов, когда опыт тестировщика используется для предугадывания того, какие дефекты могут быть в тестируемом компоненте или системе в результате сделанных ошибок, а также для разработки тестов специально для их выявления. (ISTQB)

Предугадывание ошибки (Error Guessing - EG). Это когда тест аналитик использует свои знания системы и способность к интерпретации спецификации на предмет того, чтобы "предугадать" при каких входных условиях система может выдать ошибку. Например, спецификация говорит: "пользователь должен ввести код". Тест аналитик, будет думать: "Что, если я не введу код?", "Что, если я введу неправильный код? ", и так далее. Это и есть предугадывание ошибки.

Некоторые факторы использующиеся при Error Guessing:

    Уроки, извлеченные из прошлых релизов;

    Исторические знания;

    Интуиция;

    Тикеты с прода;

    Review checklist;

    Пользовательский интерфейс приложения;

    Отчеты о рисках программного обеспечения;

    Тип данных, используемых для тестирования;

    Общие правила тестирования;

    Результаты предыдущих тестов;

    Знание об AUT (тестируемое приложение);

Исследовательское тестирование (Exploratory testing);

См. в видах тестирования

Ad-hoc testing;

См. в видах тестирования

Attack Testing;

Атака (attack): Направленная и нацеленная попытка оценить качество, главным образом надежность, объекта тестирования за счет попыток вызвать определенные отказы. См. также негативное тестирование. (ISTQB)

_Тестирование на основе атак (attack-based testing): Методика тестирования на основе опыта, использующая программные атаки с целью провоцирования отказов, в частности - отказов, связанных с защищенностью. (ISTQB) _


ГОЛОВНАБЛОГТехнічні статтіПередбачення помилки як техніка тест-дизайну
Передбачення помилки як техніка тест-дизайну

    26.05.2023
    Опубліковано: Admin

Передбачення помилки як техніка тест-дизайну

Кожна людина по-своєму унікальна: різний характер, поведінка і, звичайно ж, спосіб мислення. Тому, в силу цього чинника, в процесі розробки ПЗ неминуче будуть виникати різного роду дефекти і недоробки.

По суті, передбачення помилки в широкому сенсі – це все, що робить тестувальник при складанні тестових сценаріїв. Це словосполучення можна використовувати для опису всіх технік тест-дизайну. Адже основна мета цього процесу і полягає в тому, щоб визначити, в якому місці і за яких обставин з найбільшою ймовірністю може виникнути помилка, а також перевірити це в процесі тестування. Для тестувальника цей навик може стати відмінною підмогою в роботі. Щоб використовувати передбачення помилки в тестуванні, зовсім необов'язково мати досвід розробки ПЗ. Важливо розуміти базову логіку написання коду і розбиратися у вимогах, які будуть пред'являтися до кінцевого продукту. Далеко не зайвим буде і досвід в тестуванні схожих проєктів. Якщо ж немає такого досвіду - як варіант, можна скористатися допомогою колег, які успішно застосовують цю техніку у повсякденній роботі. Але найкраще, в такому випадку, сконцентруватися на інших, більш відчутних техніках складання тест-кейсів, які дозволять отримати надійніший результат.
Передбачення помилок як техніка тест-дизайну

Як правило, в процесі створення тест-кейсів застосовується далеко не одна техніка тест-дизайну. Це пов'язано з тим, що у кожної з них свої способи знаходження дефектів. Різні методи призначені для певного ряду завдань і дозволяють «виловити» нові баги.

На додаток до решти технік часто використовується передбачення помилки - ситуація, при якій тестувальник думає над тим, які помилки могли бути допущені в процесі розробки, а також визначає шляхи їх появи, використовуючи інтуїцію, знання і досвід. Саме тому цей спосіб рекомендується до використання тестувальникам, у яких є загальний досвід роботи, а також знання особливостей конкретного проєкту або навіть розробника. Наприклад, якщо відомо, що функціонал з масивами буде реалізовувати розробник, який часто помиляється з сортуванням, потрібно обов'язково написати кейси для перевірки порядку розташування елементів.

Таке тестування є найбільш корисним в умовах відсутності або недостатньої кількості специфікацій і суворих дедлайнів. Але, як і інші техніки тест-дизайну, передбачення помилки має свої позитивні і негативні сторони.
Переваги методу передбачення помилок:

    можливість використання на різних етапах розробки;
    відсутність необхідності в формалізації, тобто зовсім необов'язково оформляти тест-кейси – досить просто виконувати потрібні перевірки, маючи лише доступ до готового продукту;
    можливість застосування при нестачі часу. 

Недоліки:

Основний же недолік техніки передбачення помилки – покриття тестами. Якщо в ході застосування цього методу буде знайдено певну кількість помилок – неможливо гарантувати те, що весь функціонал був протестований. Наприклад, якщо був знайдений дефект верстки при зменшенні розміру вікна браузера – це не означає, що всі можливі баги верстки були виявлені. Тому, саме через відсутність повного охоплення об'єкту тестами, передбачення помилки зазвичай застосовується в поєднанні з іншими техніками тест-дизайну.
Застосування методу на реальних проєктах

Залежно від характеру і специфіки продукту, передбачення помилки може виглядати по-різному. Найпростіший приклад при тестуванні будь-якого веб-сайту: якщо вимкнути виконання Javascript у браузері, то напевно що-небудь зламається. Нижче буде наведена лише незначна частина поширених помилок розробників, які можна виявити в процесі тестування.
Сортування.

Тестуючи цією функцією, необхідно враховувати поширену помилку серед розробників, коли числа «10», «11», «12» і так далі до числа «19» виявляються між числами «1» і «2». Таким чином, масив виводиться в такому вигляді:
1
10
11
12
...
19
2
20
Тому при тестуванні подібних масивів не варто обмежувати діапазон даних, що вводяться одним десятком.

Продовжуючи тему масивів, можна згадати про сортування розмірів при тестуванні інтернет-магазинів. Тут досить часто зустрічається помилка, при якій сортування універсальних розмірів відбувається за алфавітом:

L, M, S, XL, …
Тоді як правильно вибудувати в порядку збільшення розміру:
XXS, XS, S, M, L, XL, …
«У мене в браузері все працює».

Як правило, в зв'язку з нестачею часу, розробники використовують для тестування своїх продуктів тільки один браузер. Далеко не завжди його версія збігається з тією, яка є пріоритетною на проєкті. Також розробка часто відбувається на MacOS, де стандартом вважається браузер Safari, а поновлення Chrome виходять з невеликим запізненням. Через це також можуть виникати помилки, так як різні браузери можуть інакше обробляти один і той же код. Рішення просте - уточнити пріоритетність кожного браузера і виконати тест-кейси для кожного з них. Корисно тестувати сайт на декількох браузерах одночасно - так буде простіше помітити відмінності в поведінці.
Ігнорування обробки некоректних вхідних даних.

Нерідкі випадки, коли розробники навмисно не витрачають час на продумування і впровадження станів, які, на їхню думку, неможливі. Але потрібно враховувати, що продуктом будуть користуватися люди з самим різним мисленням, і вони можуть вводити в систему найрізноманітніші дані, які будуть некоректно оброблятися. У цьому випадку завдання тестувальника – передбачити всі можливі варіанти поведінки користувача і перевірити їх.
Наприклад: в Україні номера телефонів мобільних операторів складаються з 12 символів, тому розробник обмежує довжину поля для введення телефону до 12. Але якщо поле не містить попередньої розмітки (рисок і дужок), то користувач може вводити номер телефону самими різними способами:

    380yyxxxxxxx: цей варіант буде прийнятий системою, так як був передбачений розробником;
    +380yyxxxxxxx: через символ «+» номер не пройде валідацію, так як довжина поля перевищує очікувану;
    380 yy xxxxxxx: якщо поле вважає прогалини як окремі символи – тут вже 14 символів, а не 12;
    380-yy-xxxxxxx/380(99)xxxxxxx: ситуація аналогічна до описаної вище.

У зв'язку з величезним вибором продуктів і підходів до розробки ПЗ, помилки можуть бути різними. Їх всі необхідно знайти і усунути. Головною перевагою техніки передбачення помилки слід зазначити відсутність необхідності складання тестових сценаріїв. Саме тому застосовувати цей метод можна вже на самих ранніх етапах розробки, або коли час строго обмежений, і навіть коли немає специфікації. Але успішність застосування цієї техніки безпосередньо залежить від рівня професійної підготовки тестувальника на різних проєктах.
Error Guessing in Software Testing
Last Updated : 9 Jun, 2026

Software applications are an essential part of daily life, so ensuring their quality is very important. Software testing helps identify defects and improve product reliability. Along with formal test cases, testers also use experience-based techniques like error guessing.

    Software testing ensures the application is error-free and meets user expectations.
    Testers go beyond written test cases and use experience to find hidden bugs.
    Error guessing is an informal technique where testers predict defects based on intuition and past experience.

Process of Error Guessing

The process of Error Guessing involves identifying potential defect-prone areas in an application and designing test cases based on experience and intuition rather than formal rules.

    Understanding the application requirements and functionality.
    Using tester experience to identify areas where errors are likely to occur.
    Thinking about possible mistakes made by developers or users (e.g., invalid inputs, missing values, boundary conditions).
    Creating test cases based on these guessed error scenarios.
    Executing the test cases on the application.
    Observing results and identifying unexpected behavior or defects.
    Reporting the found issues for fixing.

Error Guessing Techniques

Error Guessing is based on experience and intuition, but testers use some common techniques to improve its effectiveness:
w14124124
Error Guessing Techniques

    Experience-based testing: Using past project knowledge and previously found defects to predict new ones.
    Defect history analysis: Studying old bug reports to identify frequently occurring issues.
    Boundary-related guessing: Testing extreme values, limits, and edge cases where errors are common.
    Invalid input testing: Providing wrong, unexpected, or random inputs to check system behavior.
    Common mistake assumption: Thinking like a developer or user and guessing typical human errors.
    Complex area focus: Targeting highly complex modules or logic-heavy parts of the application.
    Error-prone feature targeting: Focusing on frequently used or recently modified features.
    Negative testing approach: Intentionally testing scenarios where the system should fail or handle errors gracefully.

Factors Considered in Error Guessing

Error Guessing depends on several important factors that help testers predict possible defects in software. These factors include:

    Tester’s experience and domain knowledge
    Past defects and historical bug data
    Lessons learned from previous projects or releases
    Common mistakes found in similar applications
    Complexity of the application or module
    User behavior and frequently used features
    Production issues and customer-reported problems
    Test execution results and failure patterns

Application of Error Guessing in Software Testing

Error Guessing is commonly used along with black-box testing techniques such as Boundary Value Analysis and Equivalence Partitioning, especially when formal methods do not cover all possible error-prone scenarios.
It is best applied in the following situations

    Time and Resource Constraints: Helps quickly identify critical defects when time for detailed test design is limited.
    Agile Environments: Supports fast and iterative testing during continuous development cycles.
    Complex or Unfamiliar Systems: Useful when documentation is limited and testers rely on experience.
    Unclear or Incomplete Specifications: Helps identify missing or incorrect requirements.
    High-Risk Modules: Focuses on areas where failures can cause major functional or business impact. 

Error Guessing complements structured testing by covering real-world, experience-based scenarios that formal techniques may miss.
Advantages of Error Guessing

    Helps find hidden and unexpected defects
    Very simple and easy to apply
    Requires no formal test design technique
    Based on real experience and practical knowledge
    Effective for finding critical and high-risk bugs
    Can be used along with other testing techniques
    Useful when time is limited for testing
    Helps in exploratory and ad-hoc testing

Limitations of Error Guessing

    Depends heavily on tester experience and intuition
    No formal structure or documented procedure
    Cannot guarantee complete test coverage
    May miss defects if tester lacks domain knowledge
    Highly unpredictable and subjective approach
    Not suitable for large or complex systems alone
    Difficult to repeat or measure results consistently
    Effectiveness varies from tester to tester

Best Practices for Effective Error Guessing

    Gain strong knowledge of application requirements and functionality
    Use past experience from previous projects and defect history
    Focus on error-prone and complex areas of the application
    Think like a developer and end user to predict mistakes
    Pay attention to boundary values and invalid inputs
    Perform negative testing to check system behavior under wrong inputs
    Prioritize recently changed or frequently used features
    Keep a checklist of common defects and mistakes
    Combine Error Guessing with formal testing techniques for better coverage
    Continuously improve skills through practice and learning from bugs