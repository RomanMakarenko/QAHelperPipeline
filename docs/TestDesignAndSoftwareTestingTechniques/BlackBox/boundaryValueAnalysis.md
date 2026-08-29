
Equivalence Partitioning and Boundary values

    04.01.2022
    Posted by: Admin

Equivalence Partitioning and Boundary values

Test design is one of the earliest stages of software testing, where the test cases are planned and designed according to the project’s quality criteria, objectives, and requirements.  The main objective of test design is the coverage of all functionality by using a minimum number of tests. For this purpose, different design techniques are used, such as general rules and recommendations, regarding creating tests during the testing process.

In this article, we will look at the equivalence classes partition and boundary value analysis techniques.
Test design techniques

These techniques are among the most important in tests design because:

    they are appropriate for a separate unit and a whole system; 
    they are intuitively used every day; 
    they help to reduce the number of tests;
    misusing these techniques can lead to omitting critical defects.

Equivalence class – set of data processing of which leads to the same result.

Two tests we can call equivalent when:

    they verify the same part of the system (function, unit);
    if the bug is identified in the first test, it is also present in the other one, and on the contrary, if there is no bug detected during the first test, there is the same result for another test;       
    they use similar sets of input data;  
    the same actions are required to run these two tests;
    as the result of the tests, we receive the same output data with the system being in the same state:

                     – the same defect block is activated;

                     – the defect processing block fails to function.

Equivalence classes partitioning is a software testing technique used for partitioning the input data to equivalent classes for its action on the system.

Let’s look at the following examples of equivalent partitioning.

There is a field with a «1-1000» input data range allowed. Entering the whole range is a long process. Moreover, there are invalid values (symbols, negative numbers, letters), which inputting should be verified.

As mentioned before, all the data of the same system affects the system equally: the valid data is accepted, the negative one is not. The invalid value should be forbidden in the field.

Therefore, we can identify two equivalence classes:

    Valid values: Numbers from 1 – 1000.
    Invalid values: numbers from -∞ to 0, from 1001 to + ∞, letters, and symbols.

The invalid value class can be divided into groups:

    from -∞ to 0;
    from 1001 to + ∞;
    special characters (# @ + - / _:; ");
    letters.

As a result, a minimum of five tests can be used for testing the input field because of equivalence classes. For example, to fill the field with the following data: 46, -37, 1773, Name, $ _ = #.

The equivalence partitioning technique is used to reduce the number of tests keeping the appropriate test coverage. The given approach also fits the text data or other data types.

There are the following steps of using the equivalence classes partitioning technique:

    Determine the equivalence classes. It is an essential step because the effectiveness of future testing depends on how right it is done. 
    Choose a value from every class.
    Run the tests.

Let us look at one more example.

There is a 50% discount on the basic tariff with advance payment for the excess baggage carriage earlier than 24 hours before the scheduled flight departure. However, if we pay for the extra baggage carriage during the boarding, the rate is 20% higher than the basic one. Check-in starts 3 hours before the departure.
Now let’s go over the steps:
1. Determining the equivalence classes.
   Class 	Values 	Extra baggage cost
   Class 1 	time> 24 	50% discount
   Class 2 	24 >= time> 3 	Basic tariff
   Class 3 	3 >= time> 0 	+20%
2. Choose the value from every class.

   departure time = before 30 hours (Class 2);
   departure time = before 10 hours (Class 2);
   departure time = before 2 hours (Class 3).

3. Run the tests

   Make the prepayment 30 hours before the scheduled flight departure. Check the 50% discount included in the rate.
   Make the prepayment 10 hours before the scheduled flight departure. Check that the rate is basic.
   Make the prepayment 2 hours before the scheduled flight departure. Check that the rate is increased to 20%.

Analyse boundary values is a test design technique aimed at testing the system behavior with boundary values of the input data.

It is essential to check the boundary values as the defects are usually found precisely on the ends of the equivalence classes.

We should check every boundary for 3 values: the boundary value and before and after the limit.

For example, for the value range within 1 – 1000, all the values between 1 and 1000 lead to the same result. So these are boundary values of the class, low and high accordingly.

The first boundary value is 1; the second value is «1000». Then, add the neighboring values to them.

    0, 1, 2;
    999, 1000, 1001.

Equivalence Partitioning and Boundary values
The algorithm of using the boundary values technique is the following:

    Identify equivalence classes.
    Identify the boundary values for every class. (It is essential to understand which class the value belongs to).
    Run tests to check the value just before the boundary, the boundary value, and just above the boundary. 

Then let’s choose the boundary values for the example described above.

    First, identify the equivalence classes to the airline company example.

    time > 24 (24 hours before the scheduled departure of the flight the discount is 50%);
    24> = time > 3 (check-in starts 3 hours before departure);
    3> = time > 0.

    Then, identify boundary values.

    24 hours (belong to the second class)
    3 hours (belong to the third class)

    Test the values at the boundary, before and after it

To this end, let’s make a prepayment.

    Before 24 hours + 1 minute. Check the 50% discount.
    Just before 24 hours. Check the basic rate.
    Before 23 hours 59 minutes. Check the basic rate.
    Just before 3 hours. Check if the rate is increased to 20%.
    Before 2 hours 59 minutes. Check if the rate is increased to 20%.

Consequently, it is enough to perform 6 tests to check the boundary values.

Considering some boundary values belong to a particular class, we can use them as class representatives to reduce the number of tests.

Using the equivalence partitioning and analyzing boundary values techniques helps decrease the risk of missing defects, improve the quality of test results and reduce the number of tests. Therefore, we save a lot of time. However we shouldn’t forget the importance of using one or the other technique properly.
