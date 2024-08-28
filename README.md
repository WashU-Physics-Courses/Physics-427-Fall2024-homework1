# Physics 427 Homework 1

__Due 11:59pm Wednesday 9/4/2023__

## 1. Machine Precision (10 points)

In C++, you can access the machine precision for `float`, `double`, and `long double` types using the standard library function `std::numeric_limits<T>::epsilon()`, where `T` is one of the floating point types. In order to use this function, you need to include the `<limits>` header file.

Write a simple C++ program to print this value for all of the three basic floating point types. Your output should look like this (replacing anything in square brackets with your numerical choices and results):

``` markdown
float epsilon: [SOME NUMBER]
double epsilon: [SOME NUMBER]
long double epsilon: [SOME NUMBER]
```

Verify that these are indeed the correct machine precision by printing whether `(1.0 + delta == 1.0)` is true. This expression will evaluate to `1` if true, and `0` otherwise. Remember `1.0` is automatically interpreted as a `double` in C++. Print the following for the machine epsilon of `double` type:

``` markdown
1.0 + epsilon == 1.0: [0 or 1]
1.0 + epsilon / 2.0 == 1.0: [0 or 1]
```

Name your source file `problem1.cpp` in the homework repository. Include its output in a separate text file `problem1.txt`. You can create such a file using the "redirect" command `>`, which takes the standard output from the command on the left and pass it as input to the file on the right. For example, the following command can generate such a text file:

``` sh
./a.out > problem1.txt
```

Commit and push both `problem1.cpp` and `problem1.txt` to the homework repository. Do not commit the compiled binary file.

## 2. Quadratic Solver (10 points)

Write a C++ header that defines two functions. The two functions should have the following signatures:

``` c++
std::tuple<double, double> solve_quadratic_1(double a, double b, double c);
std::tuple<double, double> solve_quadratic_2(double a, double b, double c);
```

Function 1 implements the usual quadratic formula that solves the algebraic equation $ax^{2} + bx + c = 0$:

$$
    x_{1,2} = \frac{-b \pm\sqrt{b^{2} - 4ac}}{2a}.
$$

Function 2 implements the more stable quadratic formula:

$$
    \displaystyle x_{1} = \frac{q}{a},\quad x_{2} = \frac{c}{q}
$$

where

$$
    q = -\frac{1}{2}\left[b + \text{sgn}(b)\sqrt{b^{2} - 4ac}\right]
$$

This way of solving the quadratic equation avoids the case when $b^2\gg 4ac$, $b$ and $\sqrt{b^2-4ac}$ are indistinguishable within numerical precision. Since $q$ does not involve the difference between two very close numbers, it behaves better for some combinations of coefficients.

The `std::tuple<double, double>` type is a two-element tuple that contains two `double` numbers. This is the main way to return multiple results from a single function, since by default C++ only allows one return value. You'll need to include the header file `<tuple>`. You can create a tuple from two numbers `x1` and `x2` using the function `std::make_tuple(x1, x2)`.

You can print the content of a tuple $x$ using the following line of code:

``` c++
std::cout << "x1 = " << std::get<0>(x) << ", x2 = " << std::get<1>(x) << std::endl;
```

Name your header file `problem2.h` in the homework repository. Write a program `problem2.cpp` to test out these functions for three different combinations of $(a, b, c)$ parameters. Include:

1. One example of $(a, b, c)$ that produces no real solutions.
2. One example of $(a, b, c)$ that produces only one real solutions.
3. One example of $(a, b, c)$ that produces different results for these two implementations. 

Organize your output as follows (replacing anything in square brackets with your numerical choices and results):

``` markdown
Case 1:
Method 1: ([a], [b], [c]): x1 = [SOME NUMBER], x2 = [SOME NUMBER]
Method 2: ([a], [b], [c]): x1 = [SOME NUMBER], x2 = [SOME NUMBER]
Case 2:
...
```

Include the output in a separate text file `problem2.txt`. Commit and push all 3 files to the homework repository. Remember to make sure that `problem2.txt` is the exact output of the program compiled from `problem2.cpp`.

## 3. Stability of a Recurrence Relation (10 points)

The "Golden mean":

$$
    \phi \equiv \frac{\sqrt{5} - 1}{2} \approx 0.61803398
$$

satisfies the following recursion relation:

$$
    \phi^{n+1} = \phi^{n-1} - \phi^{n}
$$

Use this recursion relation to write a simple function to calculate $\phi^{n}$ for a given positive integer $n$. Your function should use _recursion_, which means calling the function itself with arguments $n-1$ and $n-2$. Print out $\phi^{n}$ for $n\in [1,\dots,30]$ and compare the result from the recursion relation with simply calculating the power using `std::pow(phi, n)`. Use double precision for all numbers. Organize your output like the following:

``` markdown
phi^1: [RESULT FROM RECURSION] vs [RESULT FROM POW]
phi^2: [RESULT FROM RECURSION] vs [RESULT FROM POW]
phi^3: [RESULT FROM RECURSION] vs [RESULT FROM POW]
...
phi^30: [RESULT FROM RECURSION] vs [RESULT FROM POW]
```

You should see that the first few results should be identical, but at some point the two numbers start to differ significantly. This is because the recursion relation is _unstable_. It has and alternative solution which is $-(\sqrt{5} + 1)/2$. Any rounding error will mix a small part of this alternative solution into the results, and this mixture will grow exponentially. Choosing a good algorithm is really important!

Your source code should be written in `problem3.cpp`, and its output should be included in file `problem3.txt`. Commit and push both files to the homework repository.

## 4. Function Evaluation (20 points)

Write a C++ function that evaluates the following somewhat complicated function:

$$
    f(x) = \zeta x^2 \frac{(1+x^2)^{-\zeta-1}\sqrt{2\zeta - 1}}{\sqrt{(1+x^2)^{2\zeta} - 1 - 2\zeta x^2}}
$$
where $\zeta$ is a constant. For our purpose, lets set $\zeta = 1.44$. This function comes up when studying the global stability of a helical magnetic field in a plasma.

Write your function implementation in a file `problem4.cpp`. In the main function of the same file, print out the function values for $x = 10^{-2}$, $10^{-3}$, $10^{-4}$, $10^{-5}$, $10^{-6}$, and $10^{-7}$ in the following format:

``` markdown
Regular evaluation:
f(0.01) = [SOME NUMBER]
f(0.001) = [SOME NUMBER]
f(0.0001) = [SOME NUMBER]
...
f(1e-07) = [SOME NUMBER]
```
Note that `1e-7` means $10^{-7}$ in the code, and `std::cout` may automatically format the number this way. Notice the trend for $f(x)$. What do you think is the limit of the function when $x\to 0$?

In fact, the function given above is absolutely smooth near $x=0$ and has a well-defined limit of $f(0) = \sqrt{\zeta}$. However, floating point precision significantly messes up the calculation when $x$ is near zero. You can try this function on Desmos, and if you zoom in near $x=0$ you will notice that it too fails to properly handle the limit.

What to do in this kind of situation? One solution is to switch to some approximate representation of the function when $x$ is small enough. Find the Taylor series of $f(x)$ near $x = 0$ up to second order in $x$. Write a new function in `problem4.cpp` that evaluates $f(x)$ normally when $|x| > 10^{-3}$, and switch to the series representation when $|x| < 10^{-3}$. Again evaluate it for the same collection of $x$ as above, and print the results in the following format, immediately following the earlier results:

``` markdown
With Series:
f(0.01) = [SOME NUMBER]
f(0.001) = [SOME NUMBER]
f(0.0001) = [SOME NUMBER]
...
f(1e-07) = [SOME NUMBER]
```

Put all the output (including regular evaluation and series version) in `problem4.txt` and commit to the homework repository with `problem4.cpp`.
