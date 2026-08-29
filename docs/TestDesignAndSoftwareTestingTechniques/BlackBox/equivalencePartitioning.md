Equivalence Partitioning (EP) is a black-box testing technique that divides input data into valid and invalid partitions. It helps reduce the number of test cases while ensuring effective test coverage.

    Divides input data into partitions, where each partition represents similar behavior
    Selects one representative value from each partition for testing
    Reduces test cases while maintaining good coverage

input_set
Input-set
Guidelines for EP

The way equivalence classes are defined depends on the type of input. Each input type has corresponding valid and invalid partitions.
Input Type	Valid Class	Invalid Class(s)
Range Input	Values within the range	Values below or above the range
Specific Value	Exact valid value	Values less than or greater than the valid value
Set of Values	Values in the set	Values not in the set
Boolean Input	Expected value (true/false)	Unexpected value
Steps of Equivalence Partitioning

Equivalence Partitioning is applied by dividing inputs into logical groups and selecting representative values. Following a structured approach helps ensure effective and efficient testing.

    Identify input fields: Determine all input variables that need to be tested.
    Define input ranges or conditions: Understand valid and invalid conditions for each input.
    Divide into equivalence classes: Group inputs into valid and invalid partitions.
    Select representative values: Choose one value from each partition for testing.
    Design test cases: Create test cases using the selected values.
    Execute and validate results: Run the test cases and compare actual results with expected results.

Limitations of EP

Equivalence Partitioning is useful for reducing test cases, but it has certain limitations. It may miss defects if partitions are not properly defined.

    May miss boundary defects if used alone without BVA.
    It depends on correct partitioning; incorrect grouping can lead to missed defects.
    Does not cover all combinations of inputs.
    Less effective for complex logic or interdependent inputs. 

Example: Consider a college admission form where the percentage field accepts values between 50% and 90% only.

Using Equivalence Partitioning, the input can be divided into three classes:

    Invalid Class 1: Percentage < 50%
    Valid Class: Percentage between 50% and 90%
    Invalid Class 2: Percentage > 90%

If a student enters a percentage outside the valid range, the application displays an error message. If the entered percentage is within 50%–90%, the input is accepted.
percentage
College Admission Percentage
Benefits of EP

Equivalence Partitioning helps simplify the testing process by reducing redundant test cases. It improves efficiency while still ensuring proper validation of system behavior.

    Reduces the number of test cases while maintaining good coverage
    Identifies valid and invalid inputs effectively
    Saves time and effort in testing large input domains
    Can be combined with BVA for more robust and effective testing

Best Practices of EP Method

Equivalence Partitioning is most effective when input classes are defined accurately. Following best practices helps improve test coverage and defect detection.

    Define clear partitions based on requirements and input conditions
    Include both valid and invalid classes for complete testing
    Select proper representative values from each partition
    Avoid overlapping partitions to prevent confusion 

QA: Equivalence Partitioning as specification-based test design technique
Let start from definition, per ISTQB glossary:

    Equivalence partitioning: a black-box test design technique in which test cases are designed to execute representative from equivalence partitions. In principle, test cases are designed to cover each partition at least once. 

What does it mean?

Inputs, outputs, internal values and time-related values should be split into equivalent groups, where values on boundaries and inside of group will lead to the same result.
Each equivalence partition should be tested with valid and invalid values at least once. Multiple values for a single partition doesn't increase the coverage percentage.

Benefits: number of tests significantly descrise

Data set was split into 2 subset with selection of test case of each subset

Equivalence partitioning

Data set was split into 2 major subsets and 2 subsets of one of major subsets

Equivalence partitioning

Difficulties which can be faced: participation was identified incorrectly. That could mean that invalid values accepted or valid values rejected by the system.

Avoid errors:
a. Subsets shouldn't have a common elements. Each sub set should have unique data elements
b. Subsets shouldn't be empty. Element should exists in data set
c. Subsets union should be equivalent to original set

Types of defect: functional defects


Техніка «Equivalence Partitioning» — це спосіб створення тестових випадків, який дозволяє ефективно та економно перевірити програму/ систему. Замість того, щоб випадково обирати тести, ми розділяємо можливі дані вхідних значень на групи, які поводяться однаково. Вони [групи] можуть бути позитивними та негативними.

Ось приклад: уявіть, що ви тестуєте програму для обчислення віку людини за її датою народження. Ви знаєте, що програма приймає дату народження як вхідні дані і вираховує вік. Замість того, щоб створювати тестові випадки для кожної окремої дати народження, ви застосовуєте техніку «Equivalence Partitioning».

По-перше, треба розділити можливі дати народження на групи (візьмемо позитивні):

    Дитячі дати народження (наприклад, менше ніж 12 років).
    Підліткові дати народження (наприклад, від 12 до 18 років включно).
    Дати народження для дорослих (наприклад, від 19 до 60 років включно).
    Дати народження для літніх людей (наприклад, старше 60 років).

Після цього потрібно обрати одне або декілька значень з кожної групи і протестувати програму, використовуючи ці значення. Це дозволить перевірити, чи правильно працює програма для кожного класу дат народження. Якщо тест пройшов для одного представника з групи, він буде, скоріш за все, працювати і для інших значень з тієї ж групи.

Не забуваємо про негативні сценарії — це ситуації, коли програма повинна обробити або повідомити про помилку через некоректні або недійсні дані вхідних значень. Ось декілька прикладів:

    Введення некоректної дати: вводимо дату народження у неправильному форматі або з не дійсними значеннями. Наприклад, введення дати у форматі «місяць/день/рік» з недійсним числом дня або місяця (13 місяць, 30 лютого і т.д.).
    Дати у майбутньому.
    Дати з неправильним порядком: наприклад, ми вводимо місяць, потім рік, а потім день, замість очікуваного порядку рік-місяць-день.
    Обробка некоректних символів: перевірка, як програма реагує на некоректні символи або літери, які не пов’язані з датою народження.

Використання «Equivalence Partitioning» допомагає зменшити кількість тестів, які потрібно виконати, при цьому ефективно перевіряючи різні сценарії використання програми.