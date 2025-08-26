|cpp| Intro to C++
==================

.. |cpp| image:: ../logos/cpp-icon.png
    :width: 40px

The world runs on C++. Operating systems, web browsers, video games, heck even
Python is written in C++. Unfortunately, C++ isn't exactly known for being easy
to learn, but if you already know another language like Java or Python, it will
be easier to pick up. This little tutorial can't teach you everything there is
to know, but we'll cover some of the basics.

If you want to follow along, you can set up a C/C++ compiler in VS Code with
this tutorial: https://code.visualstudio.com/docs/languages/cpp

Plain Old C
-----------

Before C++ there was C. C introduced the syntax that many modern programming
languages are based on. There's still quite a lot of C code out there, so it's
likely that you'll encounter some. Luckily, C++ is a "superset" of C, meaning
that \*almost\* all C code is also valid C++ code.

Unlike some other languages, C/C++ don't care what file extensions you use, so
file extensions are more convention than standard. C source code is typically
denoted with ``.c`` and C++ source code is typically denoted with ``.cpp``.

For these examples, assume we have a file ``main.c``.

Functions
---------

All C programs need a place to start and that place is the ``main()`` function.

Function (also called a Method or a Procedure)
    A named segment of code that a program can jump to and execute before
    returning to where it left off.

.. code:: c++

    int main() {
        return 0;
    }

This program isn't very interesting because it doesn't do anything at all.
``int main()`` means "a function named ``main`` that returns an ``int``".
Sure enough, ``main()`` returns ``0`` which is an integer. Since we are
returning from the ``main()`` function, the program simply ends.
By convention, 0 means "success" and anything more than 0 indicates "error".

For your first program, it's a long-held tradition to write the classic
"Hello World!" program. Printing to the console is actually a lot of work.
It requires interacting with the kernel and its device drivers. Luckily for us,
C comes with a standard library that does all the heavy lifting for us.

.. code:: c++

    #include <stdio.h>

    int main() {
        printf("Hello, World!");
        return 0;
    }

>>> Hello, World!

Here, we use ``#include`` to paste all the code from the file ``stdio.h`` at
the top of our program. That code contains a function called ``printf()`` which
accepts a string of text as a parameter.

Parameter (also called an Argument)
    A value passed into a function to change what it does.

Now, let's get a little funky by defining some functions of our own:

.. code:: c++

    #include <stdio.h>

    int add(int a, int b) {
        return a + b;
    }

    void printNumber(int n) {
        printf("%d", n);
    }

    int main() {
        printNumber(add(10, -2));
        return 0;
    }

>>> 8

We've defined two functions ``add()``, which accepts two integers and returns
an integer, and ``printNumber()``, which accepts an integer and returns...
nothing. ``printNumber()``'s return type is ``void``. Note that you can also
chain functions together. The result of ``add()`` was used as the input for
``printNumber()``.

Variables
---------

That nested function call we used in our previous example was pretty cool, but
it's a little hard to read. Let's tweak it to use a variables.

Variable
    A name given to a value to save it for later.

.. code:: c++

    #include <stdio.h>

    int add(int a, int b) {
        // Add integers a and b and store the result
        int sum = a + b;
        // Return the sum
        return sum;
    }

    void printNumber(int n) {
        printf("%d", n);
    }

    int main() {
        // Declare integer variables x and y
        int x = 10, y = -2;
        // Add x and y together and store the result
        int sum = add(x, y);
        // Print the result
        printNumber(sum);
        return 0;
    }

>>> 8

Now that our program has gotten more complex, I've added some comments. This is
way more pedantic than you need to be, but comments can be extremely helpful
for others looking at your code in the future.

Comment
    A note left by the programmer that is ignored by the compiler.

Scopes
^^^^^^

All variables must have a unique name. However, you might have noticed that
``sum`` is declared twice: once in ``main()`` and again in ``add()``! This is
ok because the two variables are in different scopes.

Scope
    The region of a program where a variable is accessible.

In C, a new scope is created between every pair of curly braces ``{}``.

.. code:: c++

    // Global scope
    int sum;

    int add(int a, int b) { // Beginning of add()'s scope
        // This sum is different from the global sum variable
        int sum = a + b;

        { // Anonymous, unnamed scope
            // This sum variable is different from the other two
            int sum = 21;
        } // End anonymous scope

        // This sum is still a + b because the anonymous sum variable is in a
        // lower scope and thus inaccessible, and the global sum variable is
        // "masked" by the sum variable in the current scope
        return sum;
    } // End of add()'s scope

    // Back in global scope

Note that the global ``sum`` variable isn't assigned anything with the ``=``
operator. So what is it's value? It's *uninitialized*, meaning it contains
whatever garbage memory was left over from the previous program that was
running in the same memory area. It could be 420, or it could be -100000000.
Statistically, it would probably be 0. If we accessed the global ``sum``, we
would get "undefined behavior".

Undefined Behavior
    When something illegal happens so the output of your program is
    unpredictable.

Types
-----

In programming, all numbers are represented in binary --  0's and 1's.
A single 0 or 1 is called a "bit". Memory is divided into groups of 8 bits
called a "byte". Every byte in memory has a unique address. Larger numbers
require more than one byte to be represented.

Here are the basic types in C:

=========== ======== ========================================== ========================
Type        Bytes    Description                                Example
=========== ======== ========================================== ========================
bool [#f1]_ 1        true or false                              ``bool b = true;``
char        1        character (-128 to 127)                    ``char c = 'a';``
short       2        integer -2\ :sup:`15` to 2\ :sup:`15`-1    ``short s = 21;``
int         4 [#f2]_ integer                                    ``int i = 21;``
long        4        integer -2\ :sup:`31` to 2\ :sup:`31`-1    ``long l = 21L;``
long long   8        integer -2\ :sup:`63` to 2\ :sup:`63`-1    ``long long ll = 21LL;``
float       4        single-precision floating point number     ``float f = 2.1f;``
double      8        double-precision floating point number     ``double d = 2.1;``
=========== ======== ========================================== ========================

.. [#f1] Only in C++
.. [#f2] Varies by system. Usually 4 bytes, but can be 2 bytes

For ``char``, ``short``, ``int``, ``long``, and ``long long``, you can force
them to be positive with the ``unsigned`` keyword. Example::

    unsigned int ui = 21U;
    unsigned long ul = 21UL;

All variables can also be declared as ``const`` which prevents them from being
accidentally changed.

.. code:: c++

    int main() {
        float f1 = 3.64f;
        const float f2 = 6.78;
        f1 = 4.64f; // OK
        f2 = f1; // ERROR: assigning to const
        return 0;
    }

C is a statically typed language, meaning that the type of every function and
variable must be known at compile time. In Python you could do something like
this:

.. code:: python

    # Declare variable x
    x = 21
    # Declare it again as something else!
    x = "cat"
    # Crazy array with mixed types!
    x = [21, "cat"]

This is a big no-no in C! But why? Why would you want this extra restriction?
Well Python has to do a lot behind the scenes to get variables to work like
that. In C, variables are allocated next to each other one by one in a region
of memory called the "stack" which you can imagine like a stack of books.

.. code:: c++

    int main() {
        int a = 1;
        // Stack: [a:1]

        { // Enter new scope
            int b = 2;
            // Stack: [a:1][b:2]
            int c = 3;
            // Stack: [a:1][b:2][c:3]
            b = a; // Reassign b to another integer
            // Stack: [a:1][b:1][c:3]
        } // Variables b and c are "popped" off the stack

        // Stack: [a:1]
    }

Sure, we reassigned ``b``, but we could do that because ``a`` already contained
an ``int``. It's like sliding a new book into the middle of the stack. It needs
to be the exact same size as the old book, or else you'd have to move all the
books above the old book out of the way!

Python gets around this by storing all its variables in a big "heap" of books,
which behaves like a bunch of shelves with books scattered around all over.
Sure, you can remove and add books of different sizes wherever you please, but
the downside is that it takes a longer to find the book you're looking for
because you have to find it by its Dewey Decimal number. This is one of the
(many) things that makes C way faster than Python, at the cost of some
flexibility.

For more information on the technical details, check out `this <https://www.geeksforgeeks.org/function-call-stack-in-c/>`_
article about the function call stack.

Exact-Width Integer Types
^^^^^^^^^^^^^^^^^^^^^^^^^

Because C is so old, it was around for 8-bit machines, 16-bit machines, 32-bit
machines, and nowadays 64-bit machines. Consequently, what was considered a
"normal" size for a regular integer has changed over the years. Depending on
the machine, an ``int`` might be 2 bytes or 4 bytes. A ``long`` is guaranteed
to be at least 4 bytes, but some systems make it 8 bytes under the hood. This
problem is especially prevalent in the microcontroller wold because
architectures can vary wildly between chips. To make code more portable, C
provides *exact-width integer types*.

.. code:: c++

    // Contains definitions for exact-width integer types
    #include <stdint.h>

    int main() {
        int32_t integer = 21; // Always 4 bytes
        uint8_t unsignedChar = 'E'; // Always 1 byte
        return 0;
    }

These will appear quite a lot once we get into Arduino code.

You can get the size in bytes of any type with the ``sizeof`` operator:

.. code:: c++

    #include <stdint.h>

    int main() {
        float f = 4.56f;
        // Evaluate the size of the variable f
        printf("%ld", sizeof(f));
        // Evaluate the size of the type float
        printf("%ld", sizeof(float));
        return 0;
    }

>>> 4
>>> 4

Type Casting
^^^^^^^^^^^^

Sometimes you need to assign, say, a decimal number to an integer. You can't
change the type of a variable in C, so some extra work has to happen to convert
a decimal value into an integer. This is called "casting":

.. code:: c++

    #include <stdio.h>

    double divideByTwo(double d) {
        return d / 2;
    }

    int printInteger(int n) {
        printf("%d", n);
    }

    int main() {
        int x = 21;
        double result = divideByTwo(x);
        // printInteger(result); // ERROR
        printInteger((int)result); // We need to cast result to an int
        return 0;
    }

>>> 10

Wait... something doesn't add up here. Why did we have to cast our
``double result`` into an ``int`` before giving it to ``printInteger()``, but
we *didn't* have to cast our ``int x`` into a ``double`` before giving it to
``divideByTwo()``?

C performed an "implicit" cast. In C, types exist in a kind of hierarchy of
lower precision to higher precision. It looks something like this:

+--------------------+
| double             |
+--------------------+
| float              |
+--------------------+
| unsigned long long |
+--------------------+
| long long          |
+--------------------+
| unsigned long      |
+--------------------+
| long               |
+--------------------+
| unsigned int       |
+--------------------+
| int                |
+--------------------+
| short              |
+--------------------+
| char               |
+--------------------+
| bool               |
+--------------------+

A variable with any of these types can be assigned with a value of any type
below it on the table without explicitly invoking a cast. However, to assign
a higher type to a lower type would cause a loss of precision, so C makes you
specify the cast. In our example, an ``int`` casted automatically into a
``double`` because an ``int`` is lower than a ``double``. Hoever, we had to
explicity invoke a cast to assign a ``double`` to an ``int``. Yeah, it's a bit
confusing. When in doubt, it never hurts to specify the cast anyways.

Defining Your Own Types
^^^^^^^^^^^^^^^^^^^^^^^

You can define your own types, too! Let's create a simple "type alias" with
the ``typedef`` keyword:

.. code:: c++

    #include <stdio.h>

    // Define MyInteger as an alias of int
    typedef int MyInteger;

    int main() {
        // Declare a MyInteger
        MyInteger superCoolInteger = 21;
        // Use it like a normal int
        printf("%d", superCoolInteger);

        return 0;
    }

>>> 21

Enums (C)
^^^^^^^^^

Enum (short for "Enumeration")
    A type that can be one of a predetermined set of values.

Enums essentially let you give names to integers.

.. code:: c++

    #include <stdio.h>

    enum CarType {
        SEDAN,
        TRUCK,
        SUV
    };

    int main() {
        enum CarType myCarType = TRUCK;
        printf("%d", myCarType);

        return 0;
    }

>>> 1

By default, the first value in the ``CarType`` list is ``0`` and each
subsequent enumeration is one value higher.

Structs (C)
^^^^^^^^^^^

Struct (short for "Structure")
    A type that allows you to group multiple types into a single unit.

Structs are super useful when you need multiple groups of similar variables.
Structs can contain multiple different types -- even other structs! Each type
inside a struct is called a "field".

.. code:: c++

    #include <stdio.h>

    enum CarType {SEDAN, TRUCK, SUV};

    struct Car {
        enum CarType type;
        float maxSpeed;
        int numberOfWheels;
    };

    // Function that accepts a Car as an argument
    void printMaxSpeed(struct Car car) {
        // Access field of myCar1 with the "." operator
        printf("%f", car.maxSpeed);
        // This has no effect because car is a copy of myCar1
        car.maxSpeed = 0;
    }

    int main() {
        // To create a Car, arguments must be in the correct order
        struct Car myCar1 = {TRUCK, 107.5f, 4};
        printMaxSpeed(myCar1);

        // {0} is special and means "enter 0 for all fields"
        struct Car myCar2 = {0};

        // Declare myCar3 with uninitialized values
        struct Car myCar3;
        // Fill in values
        myCar3.type = SEDAN;
        myCar3.maxSpeed = 99.9f;
        myCar3.numberOfWheels = 3;

        return 0;
    }

>>> 107.500000

There's also another type in C called a "union", but their use is discouraged
in C++, so I won't cover them here, but you can learn about them `here <https://www.geeksforgeeks.org/c/c-unions/>`_
if you're curious.

The Peculiar Typedef Pattern
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It's a bit annoying to have to say ``struct Car`` every time we want to make a
car. C keeps separate lists of names for structs, enums, and unions.
Programmers are lazy though, so they got in the habit of using the ``typedef``
keyword to just make ``Car`` always mean  ``struct Car``.

.. code:: c++

    enum CarType {SEDAN, TRUCK, SUV};
    typedef enum CarType CarType;

    struct Car {
        CarType type;
        float maxSpeed;
        int numberOfWheels;
    };
    typedef struct Car Car;

    Car myCar; // No need for the extra struct keyword!

You can even do the ``typedef`` in one line, though it looks a little peculiar:

.. code:: c++

    typedef enum CarType { ... } CarType;
    typedef struct Car { ... } Car;
    Car myCar;

In C++, this ``typedef`` thing is done **automatically**, so you can just:

.. code:: c++

    enum CarType { ... };
    struct Car { ... };

    Car myCar1; // Automatic typedef thing
    struct Car myCar2; // You can still use the old way too

Operators
---------

Operator
    A special symbol that invokes a specific operation.

We've already seen an operator: ``+`` (addition). Most of them work like they
do in math class and they obey the typical order of operations.

Math Operators
^^^^^^^^^^^^^^

Here are a few C math operators:

====== ===================
Symbol Operation
====== ===================
\*     Multiplication
\/     Division
\%     Modulus (remainder)
\+     Addition
\-     Subtraction
====== ===================

You can use parentheses ``()`` to override order of operations like in normal
math::

    5 + 2 * 10 = 25
    (5 + 2) * 10 = 70

Here's an example of adding 1 to a number:

.. code:: c++

    int x = 5;
    x = x + 1;
    // x is 6

That ``x = x + 1;`` is a bit redundant, so C has some more operators to make it
a bit more concise:

.. code:: c++

    int x = 5;
    x += 1;
    // x is 6

There are also the operators ``+=``, ``-=``, ``*=``, ``/=``, and ``%=`` that do
exactly what you'd expect.

For the case of adding 1 and subtracting 1, there are two *unary* operators:

.. code:: c++

    int x = 5;
    x++; // x is now 6
    x--; // x is now 5 again

Unary Operator
    Operates on a single operand. (``++`` for example)

Binary Operator
    Operates on two operands. (``+`` for example)

``++`` and ``--`` are a bit special because they can be *prefix* or *postfix*,
meaning they can go before or after the variable they operate on, respectively.
The usage does change slightly:

.. code:: c++

    int x = 5;
    int y = ++x * 4;
    // x was incremented before it was used, so y = 6 * 4 which is 24
    // and x is now 6

    int a = 5;
    int b = a++ * 4;
    // a was incremented after it was used, so b = 5 * 4 which is 20
    // and a is now 6

**Where Are My Python Types?**

Those of you coming from Python might be wondering where floor division ``//``
and exponentiation ``**`` went.

For floor division, you have to cast to ``int`` after the division:

.. code:: c++

    double floor = (int) 4.4 / 3.3;

For exponentiation, you need to use the C ``math.h`` library:

.. code:: c++

    #include <math.h>
    double exp = pow(4.0, 3.0);

``math.h`` also has your trig functions, logarithms, absolute value, et cetera.

Logical Operators
^^^^^^^^^^^^^^^^^

Logical operators operate on ``true`` and ``false`` values. In plain C, there
are no keywords for ``true`` and ``false``, so ``0`` is used for ``false`` and
any non-zero value evaluates to ``true``, but usually ``1`` is ``true``.
``true`` and ``false`` are known as "Boolean" values, named after English
mathematician George Boole.

Here are the logical operators:

====== ========================
Symbol Operation
====== ========================
\!     Not
\=\=   Equal
\!\=   Not Equal
\>     Greater Than
\<     Less Than
\>\=   Greater Than Or Equal To
\<\=   Less Than Or Equal To
\&\&   And (both must be true)
\|\|   Or (either or both)
====== ========================

``!`` is evaluated first, followed by the comparison operators, followed by
``&&`` and lastly ``||``.

For example, the following evaluates to ``true``::

    !(5 == 6) && 5 != 6 || 5 > 6

Bitwise Operators
^^^^^^^^^^^^^^^^^

Instead of determining if an entire expression is ``true`` or ``false``, the
bitwise operators treat each individual bit in an integer as a boolean value.

Here are the bitwise operators:

====== ========================
Symbol Operation
====== ========================
\~     Logical NOT
\<\<   Shift Left
\>\>   Shift Right
\&     Logical AND
\^     Logical XOR
\|     Logical OR
====== ========================

In normal programming, the bitwise operators are seldom used, but in low-level
embedded programming, they are used quite often. For instance, microcontrollers
often check for which bits are set in specific memory registers to determine
how to set up hardware like output pins and timers.

Bitwise operators deserve some extra attention, so we'll return to them once
we get into the nitty-gritty of embedded programming.

Operator Precedence
^^^^^^^^^^^^^^^^^^^

At this point, we've covered most of the operators, so it's about time we look
at the order they are evaluated in. In this table, operators towards the top
are evaluated first, and operators on the same level are evaluated left to
right.

+---------------------------------------------------+
| ``()`` ``[]`` [#f3]_ ``.``                        |
+---------------------------------------------------+
| ``!`` ``~`` ``++`` ``--`` ``(cast)`` ``sizeof()`` |
+---------------------------------------------------+
| ``*`` ``/`` ``%``                                 |
+---------------------------------------------------+
| ``+`` ``-``                                       |
+---------------------------------------------------+
| ``<<`` ``>>``                                     |
+---------------------------------------------------+
| ``<`` ``<=`` ``>`` ``>=``                         |
+---------------------------------------------------+
| ``==`` ``!=``                                     |
+---------------------------------------------------+
| ``&```                                            |
+---------------------------------------------------+
| ``^``                                             |
+---------------------------------------------------+
| ``|``                                             |
+---------------------------------------------------+
| ``&&``                                            |
+---------------------------------------------------+
| ``||``                                            |
+---------------------------------------------------+
| ``=`` ``+=`` ``-=`` ``*=`` ``/=`` ``%=`` ``&=``   |
| ``^=`` ``|=`` ``<<=`` ``>>=``                     |
+---------------------------------------------------+

.. [#f3] Array index access. We'll cover this one soon.

There are a few more operators, but we'll get to them in
:ref:`pointers-and-friends`.

Flow Control
------------

So far, our programs have been pretty linear, but in the real world, your
programs will have to change their behavior based on unknown conditions.

Branching
^^^^^^^^^

An ``if`` statement only executes the code within its scope if an expression
evalutates to ``true`` (or any non-zero value).

.. code:: c++

    if (condition) {
        // Execute this code if condition is true
    }

Optionally, you can define an ``else`` clause:

.. code:: c++

    if (condition) {
        // Execute this code if condition is true
    } else {
        // Otherwise, this code will execute
    }

If you want to check for yet another condition, but only if the previous check
failed, use ``else if``:

.. code:: c++

    if (condition) {
        // Execute this code if condition is true
    } else if (other condition) {
        // Otherwise, if the second condition is true, this code will execute
    } else {
        // Otherwise, this code will execute if neither were true
    }

The ``goto`` statement is a vestige of an older time. It's a carry over from
when we programmed in assembly. It allows you to jump code execution to some
arbitrary place in your program.

.. code:: c++

    if (condition) {
        // Execute this code if condition is true
        goto mylabel;
        // This code is never executed because of the goto
    }
    mylabel:
    // Execute this code

Edsger Dijkstra, one of the most famous computer scientists of all time,
famously criticised the ``goto`` statment as "harmful", advocating for other
constructs like ``if``. If you can, you should completely avoid the use of
``goto``.

Here's one of the dangers that ``goto`` introduces:

.. code:: c++

    int x = 9;
    goto mylabel;
    int y = 10; // ERROR: crossed variable definition
    mylabel: printf("%d", x + y);

The only reason I am informing you of this evil keyword is so that we can talk
about our next keyword.

Suppose you have the following situation:

.. code:: c++

    enum CarType {SEDAN, TRUCK, SUV};

    enum CarType carType = SUV;

    if (carType == SEDAN) {
        printf("SEDAN specific code\n");
    } else if (carType == TRUCK) {
        printf("TRUCK specific code\n");
    } else if (carType == SUV) {
        printf("SUV specific code\n");
    } else {
        printf("Unknown car type\n");
    }

>>> SUV specific code

You can see how as the number of car types increases, those ``else if`` would
start to pile up. Additionally, ``carType`` had to be compared to every other
type before finally matching ``SUV``. As more cases are added, the program
would need to perform more and more checks, which is inefficient.

We can fix this with a ``switch`` statement.

.. code:: c++

    enum CarType {SEDAN, TRUCK, SUV};

    enum CarType carType = SUV;

    switch (carType) {
    case SEDAN:
        printf("SEDAN specific code\n");
        break;
    case TRUCK:
        printf("TRUCK specific code\n");
        break;
    case SUV:
        printf("SUV specific code\n");
        break;
    default:
        printf("Unknown car type\n");
    }

>>> SUV specific code

Under the hood, the ``switch`` statement generates a "jump table" that allows
the program to jump to the corresponding ``case`` label without necessarily
checking each individual case. This works just like ``goto``, which makes
``switch`` a little weird to work with. The first thing you might have noticed
is the ``break`` statements after each ``case`` block. Because ``switch`` just
tells the program to perform a jump to a label, the program will normally just
keep executing after it jumps. Here's what would happen if we didn't have
``break``:

.. code:: c++

    enum CarType {SEDAN, TRUCK, SUV};

    enum CarType carType = TRUCK;

    switch (carType) {
    case SEDAN:
        printf("SEDAN specific code\n");
    case TRUCK:
        printf("TRUCK specific code\n");
    case SUV:
        printf("SUV specific code\n");
    default:
        printf("Unknown car type\n");
    }

>>> TRUCK specific code
>>> SUV specific code
>>> Uknown car type

Additionally, if you declare a new variable inside one of the ``case`` blocks,
you run into the "crossed variable declaration" problem that ``goto`` had. We
can fix this by adding scopes to our ``case`` blocks:


.. code:: c++

    enum CarType {SEDAN, TRUCK, SUV};

    enum CarType carType = TRUCK;

    switch (carType) {
        case SEDAN: {
            float sedanSpeed = 95.0f;
            // SEDAN specific code
            break;
        }
        case TRUCK: {
            float truckSpeed = 70.0f;
            // TRUCK specific code
            break;
        }
        case SUV: {
            float suvSpeed = 84.5f;
            // SUV specific code
            break;
        }
        default: {
            printf("Unknown car type\n");
        }
    }


Loops
^^^^^

The ``while`` loop repeatedly executes code while its condition is true.
This is an infinite loop:

.. code:: c++

    while (1) {
        // Execute this code repeatedly forever, because 1 is always true
    }

We often don't want our code to run forever, so let's add a counter variable
to limit our ``while`` loop to 4 cycles:

.. code:: c++

    int count = 0;
    while (count < 4) {
        // Execute this code 10 times
        printf("%d\n", count); // \n denotes a newline character
        count++;
    }

    // Count is now 4, so the while loop terminates

>>> 0
>>> 1
>>> 2
>>> 3

The ``do while`` loop executes the code in the loop's scope before evaluating
whether it should run again.

.. code:: c++

    int count = 5;
    do {
        // Execute this code 10 times
        printf("%d", count);
        count++;
    } while (count < 4);

    // Count is now 6, so the while loop terminates

>>> 5

We can also exit from a ``while`` loop prematurely with the ``break``
statement. And we can skip a cycle with the ``continue`` statment. Here's a
program that prints the even numbers up to 10:

.. code:: c++

    int count = 0;
    while (1) {
        count++;
        // Exit the loop if count is greater than 10
        if (count > 10) break;
        // Skip the rest of the loop if count / 2 has a remainder of 1 (odd)
        if (count % 2 == 1) continue;
        printf("%d\n", count);
    }

>>> 2
>>> 4
>>> 6
>>> 8
>>> 10

Keeping track of that ``count`` variable is a bit cumbersome. You might forget
to increment it and end up with an infinite loop! And it's sort of annoying
that it's accessible in the scope above the ``while`` loop where it isn't used.
C provides a helpful construct that allows us to keep our ``count`` variable
self-contained:

.. code:: c++

    for (int i = 0; i < 4; i++) {
        printf("%d\n", i);
    }

    // i is no longer accessible after the for loop's scope

>>> 0
>>> 1
>>> 2
>>> 3

The ``for`` loop has three clauses. The first is executed before the first
loop. Here, we use it to declare a new variable ``i``. The second expression is
evaluated after each loop. Here, we check that ``i`` is less than 4. The loops
will end when ``i`` finally reaches 4. The last clause is executed after the
second expression. Here, we increment ``i`` by 1.

You can leave any of these clauses empty, too. We can abuse this fact to make
a really funny looking infinite loop:

.. code:: c++

    for (;;) {
        // Repeat forever because second clause is always true
    }

.. _`pointers-and-friends`:

Pointers and Friends
--------------------

Pointers are a little difficult to wrap your head around at first, but they're
absolutely essential to understanding important concepts like strings and
arrays.

Pointers
^^^^^^^^

Pointer
    A variable that contains a memory address.

Every variable has an associated memory address. You can grab this address with
the ``&`` "addressof" operator. This is not the same operator as logical AND.
They just ran out of special symbols on the keyboard, I guess.

.. code:: c++

    #include <stdio.h>

    int main() {
        // Declare integer x
        int x = 42;
        // Assign the address of x to int pointer ptr
        int *ptr = &x;
        printf("%p\n", ptr)
        // Dereference ptr to access the value at the address it contains
        printf("%d", *ptr);
        return 0;
    }

>>> 0x7ffc8cf98ed8
>>> 42

The asterisk ``*`` on the ``ptr`` variable denotes that ``ptr`` contains the
address of some ``int`` variable. If we print out the contents of ``ptr``, we
get some very big hexidecimal number. This number will be different every time
we rerun the program because our program gets allocated a different range of
memory addresses every run. If we want to access our original variable, we can
"dereference" the pointer by putting a ``*`` in front of it.

But why is this useful? Well, now we can pass "handles" to our variables and
let them be changed elsewhere. Consider this function:

.. code:: c++

    void swap(int *xptr, int *yptr) {
        // Save the value at address xptr into temp
        int temp = *xptr;
        // Assign the value at address xptr with the value at address yptr
        *xptr = *yptr;
        // Assign the value at address yptr with temp which contains x
        *yptr = temp;
    }

    int main() {
        int x = 1, y = 2;
        printf("%d, %d\n", x, y);
        swap(&x, &y);
        printf("%d, %d\n", x, y);
        return 0;
    }

>>> 1, 2
>>> 2, 1

That's right, we modified the contents of x and y outside of the scope they
belonged to! It's almost like cheating.

Another situation in which you might want to pass a pointer is if you have a
larger object like a ``struct`` that you want to pass into a function. Take our
``struct Car`` and our ``printMaxSpeed()`` from earlier:

.. code:: c++

    #include <stdio.h>

    enum CarType {SEDAN, TRUCK, SUV};

    struct Car {
        enum CarType type; // 4 bytes
        float maxSpeed; // 4 bytes
        int numberOfWheels; // 4 bytes
    };

    void printMaxSpeed(struct Car car) {
        printf("%f", car.maxSpeed);
        // The car variable is a copy, so this has no effect
        car.maxSpeed = 0;
    }

    int main() {
        struct Car myCar = {TRUCK, 107.5f, 4};
        printf("A Car is %ld bytes.\n", sizeof(myCar));
        printMaxSpeed(myCar);
        return 0;
    }

>>> A Car is 12 bytes.
>>> 107.500000

With the ``sizeof`` operator, we find that our ``struct Car`` is 12 bytes big!
In C, everything is "passed by value", meaning that the ``car`` in the
``printMaxSpeed`` function is different than ``myCar``. That means that every
time we call ``printMaxSpeed``, we are copying 12 bytes of memory.

We can reduce this burden by passing a pointer to a Car instead of a whole Car.

.. code:: c++

    #include <stdio.h>

    enum CarType {...};
    struct Car {...};

    void printMaxSpeed(struct Car *car) {
        // Dereference car and access its maxSpeed
        printf("%f", (*car).maxSpeed);
        // The car variable is a pointer, so this changes the original! Oh no!
        (*car).maxSpeed = 0;
    }

    int main() {
        struct Car myCar = {TRUCK, 107.5f, 4};
        printf("A Car pointer is %ld bytes.\n", sizeof(&myCar));
        printMaxSpeed(&myCar);
        printMaxSpeed(&myCar);
        return 0;
    }

>>> A Car pointer is 8 bytes.
>>> 107.500000
>>> 0.000000

Now, we only have to copy the size of a single memory address instead of a
whole 12-byte object. This doesn't save us too much for our little example, but
you can imagine structs with many more fields would be much more expensive to
copy. On 64-bit machines, a pointer is 8 bytes (64 bits). On 32-bit machines
(like our microcontrollers), a pointer is only 4 bytes (32 bits).

We've run into another problem, though: we've accidentally modified
``maxSpeed``! To prevent this, we can use ``const`` so that our compiler won't
let us touch ``car``:

.. code:: c++

    void printMaxSpeed(const struct Car *car) {
        // Dereference car and access its maxSpeed
        printf("%f", (*car).maxSpeed);
        // ERROR: assignment to const
        (*car).maxSpeed = 0;
    }

The ``(*car).`` thing is kind of cumbersome, so C has a special shortcut that
makes it easier to access members of pointers to structs called the
"Arrow Operator":

.. code:: c++

    void printMaxSpeed(const struct Car *car) {
        // Dereference car and access its maxSpeed with the "->" operator
        printf("%f", car->maxSpeed);
    }

The act of passing a pointer to an object instead of the object itself is
called "passing by reference". Programmers did this so much, that C++ added a
super cool feature called "reference types", which we'll take a closer look at
in a later section: :ref:`reference-types`.

**The Asterisk Placement Debate**

The asterisk denoting that a variable is a pointer can also be put on the type
of the variable. Actually, all three of these do the same thing:

.. code:: c++

    int *ptr1;
    int* ptr2;
    int * ptr3;

Which one is the best is a hotly debated topic. The second option seems more
intuitive because the type is "int pointer", so ``int*`` reads nicely. However,
this line of thinking breaks down in the following situation:

.. code:: c++

    int* ptr1, ptr2;

In this example, ``ptr1`` is a pointer, but ``ptr2`` is in fact not a pointer.
This is clear in option 1:

.. code:: c++

    int *ptr1, *ptr2;

Now, both ``ptr1`` and ``ptr2`` are pointers.

Pointer Arithmetic
^^^^^^^^^^^^^^^^^^

Wait... Why does a pointer have a type like ``int`` if it's just a memory
address? Aren't all memory addresses the same? Well, yes. But C can use that
extra type information to know how to treat your variable.

Arrays
^^^^^^

Strings
^^^^^^^

Header Files
------------

Preprocessor Macros
^^^^^^^^^^^^^^^^^^^

C++: C With Classes
-------------------

Namespaces
^^^^^^^^^^

Structs and Enums Again (C++)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Classes
^^^^^^^

Constructors
^^^^^^^^^^^^

Member Functions
^^^^^^^^^^^^^^^^

Visibility
^^^^^^^^^^

Inheritance
^^^^^^^^^^^

.. _`reference-types`:

Reference Types
---------------


