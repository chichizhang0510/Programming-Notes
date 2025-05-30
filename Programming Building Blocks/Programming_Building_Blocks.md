# Table of Contents

- [Statement Sequence](#statement-sequence)
- [Branching](#branching)
  - [Branching Statements](#branching-statements)
    - [Basic Concepts](#basic-concepts)
    - [Examples](#examples)
  - [Expression Evaluation](#expression-evaluation)
    - [Boolean Expressions as Conditions](#boolean-expressions-as-conditions)
    - [Side Effect of Expressions](#side-effect-of-expressions)
    - [Expression Evaluation](#expression-evaluation)
      - [Operator Precedence](#operator-precedence)
      - [Operator Associativity](#operator-associativity)
      - [Operand/Subexpression Evaluation Order](#operandsubexpression-evaluation-order)
      - [Further Reading(Optional)](#further-readingoptional)
    - [Short-Circuit Evaluation](#short-circuit-evaluation)
    - [Applying Mathematical Logic](#applying-mathematical-logic)
      - [De Morgan’s Laws](#de-morgans-laws)
      - [Distributive Laws](#distributive-laws)
      - [Absorption Laws](#absorption-laws)
      - [Exclusive OR (XOR)](#exclusive-or-xor)
  - [Tricks and Caveats](#tricks-and-caveats)
    - [Invert Complicated Conditions](#invert-complicated-conditions)
    - [Treat Overlapping Conditions Carefully](#treat-overlapping-conditions-carefully)
    - [Put the Most Likely First](#put-the-most-likely-first)
      - [Basic Ideas](#basic-ideas)
      - [Moving Likely Branches Closer to the Top](#moving-likely-branches-closer-to-the-top)
      - [Moving Likely Boolean Expressions Closer to the Left](#moving-likely-boolean-expressions-closer-to-the-left)
      - [Putting Simplest or Cheapest Checks First](#putting-simplest-or-cheapest-checks-first)
    - [Use Guard Clauses to Flatten Nested Branches](#use-guard-clauses-to-flatten-nested-branches)
  - [Recommended Leetcode Problems](#recommended-leetcode-problems)
- [Loop/Iteration](#loopiteration)
  - [Idea of Repetition](#idea-of-repetition)
  - [Loop Statements](#loop-statements)
  - [Methodical Loop Analysis](#methodical-loop-analysis)
    - [Elements of a Loop](#elements-of-a-loop)
    - [Loop Goal](#loop-goal)
    - [Loop Premise](#loop-premise)
    - [Loop Initialization](#loop-initialization)
    - [Loop Conditions](#loop-conditions)
    - [Loop Termination](#loop-termination)
    - [Iteration Goal](#iteration-goal)
    - [Loop Body](#loop-body)
    - [Edge Cases](#edge-cases)
    - [Loop Variant](#loop-variant)
    - [Loop Invariant](#loop-invariant)
      - [Definition](#definition)
      - [Importance](#importance)
      - [Loop Variable Semantics](#loop-variable-semantics)
      - [Good Questions to Ask](#good-questions-to-ask)
      - [More Examples](#more-examples)
  - [Methodical Loop Implementation](#methodical-loop-implementation)
    - [From Analysis to Implementation](#from-analysis-to-implementation)
    - [Bubble Sort](#bubble-sort)
      - [High-Level Design](#high-level-design)
      - [Mid-Level Design](#mid-level-design)
      - [Outer Loop Implementation](#outer-loop-implementation)
      - [Inner Loop Implementation](#inner-loop-implementation)
      - [Complete Implementation](#complete-implementation)
      - [Property-Based Testing](#property-based-testing)
      - [Define Variables Wisely](#define-variables-wisely)
      - [Optimization](#optimization)
    - [Insertion Sort](#insertion-sort)
      - [High-Level Design](#high-level-design)
      - [Mid-Level Design](#mid-level-design)
      - [Outer Loop Implementation](#outer-loop-implementation)
      - [Inner Loop Implementation](#inner-loop-implementation)
      - [Complete Implementation](#complete-implementation)
      - [Alternative Implementation and Optimization](#alternative-implementation-and-optimization)
        - [Alternative Search while Shifting](#alternative-search-while-shifting)
        - [Adjacent Swapping](#adjacent-swapping)
        - [Linear Search then Shifting](#linear-search-then-shifting)
        - [Binary Search then Shifting](#binary-search-then-shifting)
    - [Three-Way Partitioning](#three-way-partitioning)
  - [Recommended Leetcode Problems](#recommended-leetcode-problems)
- [Recursion](#recursion)
- [Documentation](#documentation)
  - [Block/Loop Documentation](#blockloop-documentation)
  - [Function/Method Documentation](#functionmethod-documentation)
  - [Reference Materials](#reference-materials)
- [Master Your Arsenal](#master-your-arsenal)
  - [Built-In Library & Functions](#built-in-library-functions)
  - [Classic Algorithms & Data Structures](#classic-algorithms-data-structures)
  - [Build Your Own Templates](#build-your-own-templates)

# Statement Sequence

A statement is a complete instruction that performs some action. In imperative programming languages, statements are the basic unit of execution. Common types of statements include declarations, assignments, and function/method calls.

A **statement sequence** (also called sequential execution) means the computer executes statements one after another, in the exact order they appear, from top to bottom. This is the default behavior of a program: without any control flow, it simply runs each line in sequence.

In practice, it’s difficult to implement anything complex or interesting using only statement sequences without control flow. Such purely sequential code usually only works for very simple, short tasks. For example:

```java
void statementSequenceExample() {
    // Suppose we have two variables and we want to exchange their values
    int x = 5;
    int y = 7;
    System.out.printf("x: %d, y: %d\n", x, y);

    // One of the simplest way is to introduce a temporary variable
    int tmp = x;
    x = y;
    y = tmp;
    System.out.printf("x: %d, y: %d\n", x, y);
}

statementSequenceExample();
```

Program control mechanisms such as branching, loops and recursion interrupt or modify this basic sequential flow, as we will discuss below.
1. **Sequential Execution**: Execute statements in order (default behavior).
2. **Branching**: Choose which block of statement sequence to execute based on some condition.
3. **Looping**: Repeat a block of statements multiple times.
4. **Recursion**: Have a function or routine call itself, re-entering the same sequence of steps with new inputs.

Other control flow constructs (such as `goto`) exist, but they are generally discouraged and rarely needed in competitive programming, so we won’t cover them here.

# Branching

## Branching Statements

### Basic Concepts

Branching is about making decisions in a program: perform different actions under different conditions, meaning the program can execute different blocks of code depending on the conditions. Branching lets a program react dynamically to various inputs and values. This concept is also known as **Conditional Statements** or **Selection**.

In programming, branching is typically implemented with statements like `if`, `else if`, and `else`. Some programming languages also support `switch` statements, which are more efficient when dealing with single variable against multiple values, but `switch` is less common in competitive programming and is omitted here. Branching statements can be combined and nested to form complex decision logic. For example:

```java
if (condition_1) {
    // Statements_1 to be executed if condition_1 is true
} else if (condition_2) {
    // Statements_2 to be executed if condition_2 is true and all previous conditions are false
} else if (condition_3) {
    // Statements_3 to be executed if condition_3 is true and all previous conditions are false
} else if (condition_n) {
    // Statements_N to be executed if condition_n is true and all previous conditions are false
} else {
    // Default statements to be executed in case all above conditional expressions are false.
}
```

For complicated branching statements, you could use a **decision tree** to visualize it. Below is an example decision tree, and each decision node (often drawn as a diamond in flowcharts) could be further broken into more branches if conditions involve multiple parts or variables. For instance, if `condition_1` is composed of $N$ boolean expressions and involves $M$ variables, that single decision can be expanded into a sub-tree exploring all combinations of those sub-expressions.

<img src="img\branching_statement_as_decision_tree.png" alt="branching_statement_as_decision_tree" style="width: 70%; height: 70%; display: block; margin-left: auto; margin-right: auto;">

From a mathematical perspective, **set theory** can help determine which branch executes for a given set of variable assignments, and **Venn diagrams** can illustrate how those sets overlap. For example, consider a Venn diagram for the branching logic above:

<img src="img\venn_diagram_for_branching_statement.png" alt="venn_diagram_for_branching_statement" style="width: 50%; height: 50%; display: block; margin-left: auto; margin-right: auto;">

In this diagram, there are nine distinct regions, formally called *atomic equivalence classes*, representing different combinations of variable assignments for the branching code. Notice that the order of the branches matters when conditions overlap:

- Variable assignments corresponding to A1, A5, A4, A7 will cause Statement_1 to execute.
- Variable assignments corresponding to A2, A6 will cause Statements_2 to execute.
- Variable assignments corresponding to A3 will cause Statements_3 to execute.
- Variable assignments corresponding to A8 will cause Statements_N to execute.
- Variable assignments corresponding to A9 will cause the default statements to execute.

Different programmers might implement the same branching logic in different but logically equivalent ways, which is one of the reasons why another programmer's implementation of the same algorithm may look drastically different than yours. Understanding how branching works not only helps you write better code but also makes it easier to read and interpret other people’s code.

### Examples

**Example 1**

```java
// Code_Block_1
if (condition_1) {
    if (condition_2) {
    // Statements to be executed
    }
}
```

```java
// Code_Block_2
if (condition_1 && condition_2) {
    // Statements to be executed
}
```

Do you think the above two branching code blocks are equivalent? If so, which one do you think is better?
<details>
<summary>Click to show the answer</summary>
Yes, they are always functionally equivalent, even if the conditions have side effects or involve method calls.<br/>
In this case, <code>Code_Block_2</code> is preferable for clarity and conciseness.<br/>
But in practice, you might want to execute some code whenever <code>condition_1</code> is true and regardless of <code>condition_2</code>. In this case you could insert code inside the outer <code>if</code> but outside the inner <code>if</code>, so <code>Code_Block_1</code> gives you more flexibility.<br/>
Also notice that due to short-circuit evaluation, in <code>Code_Block_2</code>, if <code>condition_1</code> evaluates to false, then <code>condition_2</code> won't be evaluated.
</details>

**Example 2**
```java
// Code_Block_3
if (condition_1) {
    if (condition_2) {
    // Statements_A to be executed
    }
    if (condition_3) {
    // Statements_B to be executed
    }
}
```

```java
// Code_Block_4
if (condition_1 && condition_2) {
    // Statements_A to be executed
}
if (condition_1 && condition_3) {
    // Statements_B to be executed
}
```

What about the above pair of code blocks?
<details>
<summary>Click to show the answer</summary>
They are equivalent <b>only if</b> evaluating <code>condition_1</code> has no side effects. If <code>condition_1</code> does have side effects, then they are <b>not</b> equivalent.<br/>
Even when they are logically equivalent, <code>condition_1</code> will always be evaluated twice in code>Code_Block_4</code>, so if the evaluation of <code>condition_1</code> is expensive, then <code>Code_Block_3</code> is preferred.<br/>
In <code>Code_Block_4</code>, even though <code>condition_1</code> is evaluated in the fist <code>if</code>, the program does not store that result and will evaluate it again in the second <code>if</code>.
</details>

**Example 3**

```java
// Code_Block_5
if (condition_1) {
    if (condition_2) {
    // Statements_A to be executed
    } else if (condition_3) {
    // Statements_B to be executed
    }
}
```

```java
// Code_Block_6
if (condition_1 && condition_2) {
    // Statements_A to be executed
}
if (condition_1 && condition_3) {
    // Statements_B to be executed
}
```

Do you think the above pair of code blocks are equivalent?
<details>
<summary>Click to show the answer</summary>
No, they are not equivalent.<br/>
In <code>Code_Block_5</code>, at most one branch can execute, whereas in <code>Code_Block_6</code>, both branches might execute. For example, if both <code>condition_2</code> and <code>condition_3</code> are true, <code>Code_Block_5</code> will execute only Statements_A (since the <code>else if</code> prevents the second check when the first succeeds), but <code>Code_Block_6</code> will execute both Statements_A and Statements_B.<br/>
Additionally, in <code>Code_Block_6</code>, <code>condition_1</code> will always be evaluated twice.
</details>

**Example 4**

```java
// Code_Block_7
if (condition_1) {
    // Statements_A to be executed
} else if (condition_2) {
    // Statements_A to be executed
}
```

```java
// Code_Block_8
if (condition_2) {
    // Statements_A to be executed
} else if (condition_1) {
    // Statements_A to be executed
}
```

```java
// Code_Block_9
if (condition_1 || condition_2) {
    // Statements_A to be executed
}
```

```java
// Code_Block_10
if (condition_2 || condition_1) {
    // Statements_A to be executed
}
```

```java
// Code_Block_11
if (condition_1 | condition_2) {
    // Statements_A to be executed
}
```

```java
// Code_Block_12
if (condition_2 | condition_1) {
    // Statements_A to be executed
}
```

Which pairs of the above code blocks are equivalent? Why?
<details>
<summary>Click to show the answer</summary>
<code>Code_Block_7</code> and <code>Code_Block_9</code> are always equivalent to each other. They are equivalent to the other versions only when both conditions have no side effects.<br/>
<code>Code_Block_8</code> and <code>Code_Block_10</code> are always equivalent to each other. They are equivalent to the other versions only when both conditions have no side effects.<br/>
For <code>Code_Block_11</code> and <code>Code_Block_12</code>, both conditions will be evaluated (because <code>|</code> is a bitwise OR without short-circuiting). Whether they are equivalent depends on whether they have side effects and their nature. If they have no side effects, then they are equivalent. Otherwise, the execution order between them matters and can change the outcome. (For example, adding 5 then multiplying by 5 yields a different result than multiplying then adding 5.)<br/><br/>
To conclude, if both conditions have no side effects, all the code blocks produce the same logical result, but <code>Code_Block_11</code> and <code>Code_Block_12</code> will still evaluate both conditions. If side effects are present, you have to analyze each case separately. This complexity is one reason refactoring code can be tricky: if a boolean-returning function has side effects and you change those effects, you must review every place that calls it.
</details>

**Practical Examples**
One simple use of branching is determining whether an integer is odd or even:

```java
boolean isOdd(int num) {
    if (num % 2 == 0)
        return false;
    else
        return true;
}

boolean isEven(int num) {
    return !isOdd(num);
}

void oddityExample() {
    System.out.printf("Is 5 odd: %b\n", isOdd(5));
    System.out.printf("Is 10 even: %b\n", isEven(10));
}

oddityExample();
```

Not all branching programs are as straightforward as the above example. The complexity of a block of conditional statements depends on several factors: the complexity of the each condition (predicate), the number of branches, the nesting depth of branches and etc. For instance:

```java
// What is even happening?
if (((!a || b) && c && !(d && e)) || ((f && (!g || h)) && !(i && j && !k)) || l) {
    doSomething();
}
```


The potential for an **exponential explosion of paths** in complex conditions can make branching code difficult to write and reason about. For example, consider tax preparation software (like Sprintax or TurboTax) that calculates your taxes or refund, likely contain extremely intricate branching code.

In competitive programming practice, you will definitely encounter branching logic that is not so obvious or simple. As an exercise, consider implementing a function to check whether a given date (day, month, year) is valid. Note that this problem could be solved using only branching without loop or recursion.

```java
boolean isValidDate(int day, int month, int year) {
    // TODO add your code here
    return true;
}

// Test cases
System.out.println(isValidDate(29, 2, 2020));  // true (leap year)
System.out.println(isValidDate(29, 2, 2021));  // false
System.out.println(isValidDate(31, 4, 2022));  // false (April has 30)
System.out.println(isValidDate(31, 12, 2023)); // true
System.out.println(isValidDate(0, 1, 2024));   // false
System.out.println(isValidDate(15, 13, 2024)); // false
```

## Expression Evaluation

### Boolean Expressions as Conditions

The behavior of a branching statement depends on its conditions, which are boolean expressions that evaluate to either `true` or `false`. These boolean expressions can take several forms:
- Boolean literals `true` and `false` used directly in boolean expressions. For example, some loops starts with `while(true)`.
- Boolean variables that hold boolean value. For example, `boolean v = true`.
- Comparison operators such as `==`, `!=`, `<`, `>`, `<=`, `>=` that take two operands and evaluates to a boolean value.
- Logical Operators such as `&&`, `||`, `!`, `^`, `&`, `|` to combine one or more boolean expressions.
- Method calls that return boolean value.

You are supposed to be familiar of the meanings and usage of the above operators.

It's worth noting that boolean expressions are used not just as branching conditions, but in many places in code for making decisions:
- `if`, `else if`, `while`, `for`, `do-while`
- ternary operator (`? :`), which acts like a compact `if-else`
- `assert`, `switch` (in languages where `switch` can take a boolean)

They also determines when loops continue or stop .

The ternary operator (`? :`) is essentially an inline `if-else` block: `condition ? expr1 : expr2`.
- If condition is true, only `expr1` is evaluated, and its value is used.
- If condition is false, only `expr2` is evaluated, and its value is used.

The ternary operator is often used for concise conditional assignments. For example:
```java
max = (n1 > n2) ? n1 : n2;
```

### Side Effect of Expressions

**Side Effects** are an important concept that goes beyond boolean expressions, and extend to all expression evaluation. In the context of **expression evaluation**, a side effect is **any observable change in state** that occurs in addition to producing a value. This includes modifying variables, changing an object’s state, performing I/O, etc.
- Expressions with no side effects are pure: they just compute and return a value without altering any program state.
- Expressions with side effects modify state (memory, I/O, etc.) or trigger something beyond just computing a value.

Side Effects stem from modification operations that write or mutate variables. "Read-only" operations are expressions that access state but do not change it, and thus are free of side effects.

```java
int x = 5;
boolean result = (x > 0);   // just evaluates a boolean, no state change
```

```java
int[] nums = {1, 2, 3};
boolean result = (++nums[0] > 0); // modifies nums[0] as a side effect
```

```java
int method1(int x) {
    return x + 1;
}
int x = 0;
boolean result = (method1(x) > 0);   // method1 has no side effects
```

```java
int method2(int[] nums) {
    nums[0] = 1;
    return nums[0];
}
int[] nums = {0, 1};
boolean result = (method2(nums) > 0);   // method2 has side effects
```

```java
int[] nums = {0, 1};
int x = 0;
// Because of the side effects of method2 and the short-circuit evaluation,
// the following two blocks are not equivalent.
if (method1(x) > 0 || method2(nums) > 0) {
    doSomething();
}
if (method2(nums) > 0 || method1(x) > 0) {
    doSomething();
}
```

Side Effects matter in expression evaluation because they could introduce bugs if not handled carefully:
- **Reordering**: If the compiler or programmer changes the evaluation order, side effects might occur in a different order.
- **Short-Circuiting**: as shown above, side effects may or may not occur because of short-circuiting, so don’t rely on side effects in short-circuited branches.
To keep program behavior predictable and code clear, it’s often best to avoid embedding important side effects in complex boolean expressions. If a method call or operation has a significant side effect, consider doing it in a separate statement. 

### Expression Evaluation

**Expression Evaluation** is determined by several factors, mainly **operator precedence**, **operator associativity** and **operand/subexpression evaluation order**. They could be trick and confuse a lot of people. To conclude:
- Operator Precedence and Operator Associativity in Java (and most programming languages) only determine how expressions are grouped, that is how the parser builds the abstract syntax tree (AST). They do not dictate when function arguments are evaluated, when side effects (like `i++`) happen and the exact runtime order of evaluation.
- These are determined by the Java Language Specification, which explicitly defines operand evaluation order (e.g., left-to-right for most binary operators).

#### Operator Precedence

Operator Precedence determines which operator binds first when two different operators appear together without parentheses. For example, `*` and `/` take precedence over `-` and `+`. You can find a complete table of precedence here: https://introcs.cs.princeton.edu/java/11precedence/

If you are unsure of operator precedence, you can always use parentheses to make the order explicit. For boolean expressions, using parentheses to remove ambiguity is a good practice for readability. For instance:


```java
// When there is only one type of logical operator, there is no ambiguity.
if (E_1 && E_2 && E_3 && E_4) {
    // Statements
}
```

```java
// If you don't remember the precedence between && and || then you can't be sure how this boolean expression will evaluate.
if (E_1 || E_2 && E_3 || E_4) {
    // Statements
}
```

```java
// Use parenthesis to eliminate ambiguity and make your code more readable.
if ((E_1 || E_2) && (E_3 || E_4)) {
    // Statements
}
// OR
if (((E_1 || E_2) && E_3) || E_4) {
    // Statements
}
// OR other ways
```

#### Operator Associativity

**Operator Associativity** is about how operators of the same precedence are grouped in the absence of parentheses. When an expression has multiple operators with the same precedence, associativity (either left-to-right or right-to-left) tells us how to group them.

For example, `100 / 2 / 5` is interpreted as `(100 / 2) / 5` (left-to-right associativity for division) rather than `100 / (2 / 5)`. Similarly, `100 * 2 / 5` is treated as `(100 * 2) / 5` not `100 * (2 / 5)`, since `*` and `/` share the same precedence and are left-associative.

Most Java operators are left-to-right associative, but the assignment operators(`=`, `+=`, `-=` etc) are right-to-left associative. So the expression `x = y = z = 5` is treated as `x = (y = (z = 5))`.

#### Operand/Subexpression Evaluation Order

Operator Precedence and Associativity determine in which order Java groups operands and operators, but it does not determine in which order the operands are evaluated. By determining grouping, they in turn constrains the order of execution, but only to the extent required by the grouping. So the grouping does constrain evaluation order, but only partially, and only through structure, not through actual timing or sequencing rules.

For example, `a[A()] = B() = C()` will be treated as `a[A()] = (B() = C())`, but `A()` will be evaluated before `B()` and `C()`.

For another example, `int result = 2 + 3 + 4 * 5`, due to operator Associativity and Precedence:
- `*` has higher precedence -> grouped as `2 + 3 + (4 * 5)`
- `+` is left-associative -> grouped as `((2 + 3) + (4 * 5))`

You could replace the literals with method call or complicated expressions, but even in this case, grouping constrains evaluation, but doesn’t explicitly dictate the low-level order of evaluation of operands, which is determined by the Java Language Specification (left-to-right for most cases), but in some other languages (like C/C++), the order is unspecified.

**Operand/Subexpression Evaluation Order** determines the order in which the operand/subexpression are evaluated.

In recent versions of the Java Language Specification, evaluation order for most kinds of expressions is well-defined as left-to-right. However, due to short-circuit evaluation rules, not all subexpressions are guaranteed to be evaluated:

```java
// In very old Java versions, the order of B(), C(), D() might not have been guaranteed.
// In modern Java, this will call B(), then C(), then D() in order.
A a = new A(B(), C(), D());

// Here, B(), C(), D() are called left-to-right, but short-circuiting applies.
// If B() returns true, C() and D() won't be called at all due to short-circuit OR.
if (B() || C() || D()) {
    doSomething();
}
```

There are many other rules of Expression Evaluation Order and you can refer to Java Language Specification, but a general rule of thumb is not to rely on the programming language to resolve ambiguity. If your code’s correctness depends on certain things being evaluated in a specific order, it’s usually better to refactor the code to make that order explicit (for example, by splitting the expression and using temporary variables).

For example, if in the above code, you really need `B()` to execute before `C()` and `C()` before `D()`, you should use the following code(replace `var` with the actual type):
```java
var b = B();
var c = C();
var d = D();
A a = new A(b, c, d);
```

Notice that for other languages, this may not be the case. For example, C and C++ are different to Java in this topic: their language definitions leave evaluation order undefined deliberately to enable more optimizations.

#### Further Reading(Optional)

The nuances of operator precedence, associativity, and evaluation order can get very complex. For a deeper understanding, you can refer to discussions like this Stack Overflow post: https://stackoverflow.com/questions/6800590/what-are-the-rules-for-evaluation-order-in-java

The question in that post involves a tricky Java expression:
```java
int[] a = {4,4};
int b = 1;
a[b] = b = 0;
```

The result of the above line is that `a[1]` becomes `0`. It takes some careful thought to understand why, but the takeaway is not to write such code that could raise ambiguity. Avoid convoluted expressions that are hard to reason about, and don’t write code where it’s unclear in what order things happen.

This means you should avoid code like the following:
```java
a[j++] = a[j - 1];
```

### Short-Circuit Evaluation

In many programming languages, **logical operators** `&&` (AND) and `||` (OR) perform **Short-Circuit Evaluation**: They skip evaluating the right-hand side if the left-hand side is enough to determine the result. Short-circuiting improves performance and helps avoid side effects or errors (e.g., null pointer exceptions).

Specifically:
- For `&&` (Logical AND), if left side evaluates to false, the result must be false regardless of the right side, so right side is not evaluated.
- For `||` (Logical OR), if left side evaluates to true, the result must be true regardless of the right side, so right side is not evaluated.

In contrast, **bitwise operators** `&` (Bitwise AND) and `|` (Bitwise OR) are not short-circuit operators:
- For `&` (Bitwise AND), both sides are always evaluated from left to right, even if the first is false.
- For `|` (Bitwise OR), both sides are always evaluated from left to right, even if the first is true.

You can use `&` or `|` when you need both operands to be evaluated (typically this means they have side effects that you want to always occur), or when you are explicitly doing bit-level operations on integers.

Java’s strict left-to-right evaluation + short-circuiting is safe and predictable.
    - It’s a key reason why idioms like `if (x != null && x.doSomething())` are so common. 
    - It’s safe because if `x` is `null`, the second part (`x.doSomething()`) is never evaluated, thus avoiding a `NullPointerException`.
    - You’ll often use such idioms when dealing with linked lists, trees, or other structures where you need to check an object for null before accessing its members.

```java
boolean isTrue() {
    System.out.println("isTrue called");
    return true;
}

boolean isFalse() {
    System.out.println("isFalse called");
    return false;
}

if (isTrue() || isFalse()) {
    System.out.println("Result: true"); // only isTrue() will be called
}

if (isTrue() | isFalse()) {
    System.out.println("Result: true"); // both will be called
}
```

Short-circuit evaluation is exactly why some earlier code block pairs were equivalent and others were not.

```java
// Code_Block_1
if (condition_1) {
    if (condition_2) {
    // Statements to be executed
    }
}
```

```java
// Code_Block_2
if (condition_1 && condition_2) {
    // Statements to be executed
}
```

For example, `Code_Block_1` and `Code_Block_2` are equivalent, that is the statements inside will only be executed if both conditions evaluate to `true`. And due to the reason we put `condition_1` before `condition_2` and connect them with `&&` in `Code_Block_2`, if `condition_1` evaluates to `false`, then `condition_2` won't be evaluated. This matters if `condition_2` has side effects, such as modifying the values of some variables: those side effects will never occur unless `condition_1` was true.

### Applying Mathematical Logic

A bit of knowledge about mathematical logic can help you simplify boolean expressions and make the code more readable.

#### De Morgan’s Laws

According to De Morgan's Laws:

$$
\neg (A \lor B) \equiv (\neg A) \land (\neg B)
$$

$$
\neg (A \land B) \equiv (\neg A) \lor (\neg B)
$$

Thus in programming languages, we have the following:
- `!(A && B && C)` is equivalent to `!A || !B || !C`, and vice versa
- `!(A || B || C)` is equivalent to `!A && !B && !C`, and vice versa

#### Distributive Laws

According to De Morgan's Laws:

$$
A \land (B \lor C) \equiv (A \land B) \lor (A \land C)
$$

$$
A \lor (B \land C) \equiv (A \lor B) \land (A \lor C)
$$


Thus in programming languages, we have the following:
- `A && (B || C)` is equivalent to `(A && B) || (A && C)`, and vice versa
- `A || (B && C)` is equivalent to `(A || B) && (A || C)`, and vice versa

#### Absorption Laws

According to Absorption Laws:

$$
A \lor (A \land B) \equiv A
$$

$$
A \land (A \lor B) \equiv A
$$


Thus in programming languages, we have the following:
- `A || (A && B)` is equivalent to `A`, and vice versa
- `A && (A || B)` is equivalent to `A`, and vice versa

#### Exclusive OR (XOR)

According to the definition of Exclusive OR (XOR):

$$
(A \land \neg B) \lor (\neg A \land B) \equiv A \oplus B
$$


Thus in programming languages, we have the following:
- `(A && !B) || (!A && B)` is equivalent to `A ^ B`, and vice versa

## Tricks and Caveats

When implementing branching, there are some good practices and common pitfalls to keep in mind. These optional tips can improve performance, clarity, and maintainability. We’ll revisit some of these tricks later as well.

### Invert Complicated Conditions

You must have noticed that for the `else` branch, you don't have to explicitly specify the branching condition: it automatically handles "everything else". This follows the complement idea: all the conditions covered by `if` and `else if` clauses form one set of possibilities (of variable assignments), and the `else` covers the complement of that set.

This is particularly useful when we only have a pair of `if-else`, meaning we’re covering exactly two possibilities (a binary decision). That’s essentially a binary partition: **the predicate and its complement**. If the predicate is very complex or cumbersome to write, sometimes it’s easier to write the negation of that predicate for one of the branches, rather than the predicate itself. In binary search, you can see a lot of applications of this technique.

For example, suppose we have six colors: `RED`, `BLUE`, `YELLOW`, `GREEN`, `WHITE`, `BLACK`. And we just want to know if an object is black or not. We could write it two ways:

```java
boolean isBlack_(COLOR color) {
    if (color is RED || color is BLUE || color is YELLOW || color is GREEN || color is WHITE)
        return false;
    else
        return true;
}
```

```java
boolean isBlack(COLOR color) {
    if (color is BLACK)
        return true;
    else
        return false;
}
```

In general, if the branching structure has only two branches, then there are two predicates/boolean expressions. In such cases, we can use the simpler predicate in the `if` branching condition, leaving the `else` implicitly handle the other (complementary) case. This can lead to clearer and less error-prone code.

### Treat Overlapping Conditions Carefully

Ideally the branching conditions should be **mutually exclusive** and cover all possibilities, because then the order of the branches doesn't matter and the logic is less error-prone. But sometimes it's difficult or expensive to achieve this goal. You need to weigh the clarity vs. complexity trade-off.

Consider the previous Venn diagram example, `condition_1`, `condition_2` and `condition_3` overlap, and if we want to make all branches mutually exclusive, we need to break them into many sub-conditions, like 7 mutually exclusive conditions corresponding to A1~A7. That could become unwieldy.

In addition, sometimes we can utilize the overlap between the conditions to improve performance.

<img src="img\grading_number_line.png" alt="venn_diagram_for_branching_statement" style="width: 50%; height: 50%; display: block; margin-left: auto; margin-right: auto;">

Consider the above example, we want to write a program that determines the grade (`A`, `B`, `C`, `D`, `F`) of a student using the score as input. If we insist on making the branching conditions mutually exclusive, we would have something similar to the following:

```java
String getGrade(int score) {
    if (score >= 90) {
        return "A";
    } else if (score < 90 && score >= 80) {
        return "B";
    } else if (score < 80 && score >= 70) {
        return "C";
    } else if (score < 70 && score >= 60) {
        return "D";
    } else {
        return "F";
    }
}
```

You may think the code is clear and more readable, but the trade-off is redundancy, which is why it's not always encouraged. For example, whenever the program evaluates `score < 90 && score >= 80`, `score < 90` must be true, because the branching condition `score >= 90` has already been evaluated to false. Utilizing the intersection of the conditions we could refactor it into something like the following, which is considered more efficient:

```java
String getGrade(int score) {
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    if (score >= 70) return "C";
    if (score >= 60) return "D";
    return "F";
}
```

Or equivalently:
```java
String getGrade(int score) {
    if (score < 60) return "F";
    if (score < 70) return "D";
    if (score < 80) return "C";
    if (score < 90) return "B";
    return "A";
}
```

If the branching conditions are not mutually exclusive, the order of the branches makes a difference, and remember to make the more specific condition come **before** the more general one.

Consider the following example:
```java
if (x is divisible by 2) {
    // Statement_A to be executed
} else if (x is divisible by 4) {
    // Statement_B to be executed
}
```
The condition `x is divisible by 4` is more specific than `x is divisible by 2`. From the perspective of set theory, the first condition is a super set of the second condition, so the second branch will never get a chance to execute.

You could refactor it to the following:
```java
if (x is divisible by 4) {
    // Statement_B to be executed
} else if (x is divisible by 2) {
    // Statement_A to be executed
}
```

Sometimes this will make more sense, but remember when reading branching code, read all the conditions from top to bottom and understand that by the time you get to a certain branch, earlier conditions have failed.

In this case you should realize that although the condition says `x is divisible by 2`, but only when `x is divisible by 2 not not divisible by 4` will go into this branch.

### Put the Most Likely First

#### Basic Ideas

If you know that certain conditions are more likely than others, consider ordering your branches (and even the parts of complex boolean expressions) to check the likely cases first. This can provide a performance benefit by short-circuiting checks that are unlikely to succeed.

**For branches (if/else if)**: Since the evaluation order of the branching conditions is from top to bottom, we can move the most likely branches (the ones with the highest probability) closer to the top to improve performance. Notice that when doing so, we need to consider the overlap between branching conditions to avoid bugs.

**For boolean expressions**: Since the evaluation order of a boolean expression is from left to right, we can move the most likely expressions (the ones with the highest probability to evaluate to false, if you use &&, or the highest probability to evaluate to true, if you use ||) closer to the left start to improve performance. Notice that when doing so, we need to consider the side effects of the expressions to avoid bugs.

If you have no idea about the likelihood or they’re roughly equal, you can instead order by simplicity or cost—check simpler or cheaper conditions first. For example, if two conditions have similar probability, but one involves a quick variable check and the other calls an expensive function, do the quick check first.

#### Moving Likely Branches Closer to the Top

```java
// Before (Less efficient)
if (user.isAdmin()) {             // rarely true
    adminDashboard();
} else if (user.isGuest()) {      // somewhat frequent
    guestDashboard();
} else if (user.isRegularUser()) { // most frequent
    userDashboard();
}
```

```java
// After (More efficient)
if (user.isRegularUser()) {        // most frequent, moved first
    userDashboard();
} else if (user.isGuest()) {       // second most frequent
    guestDashboard();
} else if (user.isAdmin()) {       // rarely true, moved last
    adminDashboard();
}
```

In the before version, every regular user login will go through two additional branch condition branches that rarely evaluates to true. If the evaluation of those two expression is expensive, then the after version improve the performance drastically.

#### Moving Likely Boolean Expressions Closer to the Left

The following example is for `&&`, but for `||` the idea is similar.

```java
// Before (Less efficient)
if (user.hasPremiumAccount() && user.isLoggedIn()) {
    accessPremiumContent();
}
```

```java
// After (More efficient)
if (user.isLoggedIn() && user.hasPremiumAccount()) {
    accessPremiumContent();
}
```

In this case, most users aren't logged in, so user.isLoggedIn() is more likely false than premium account status. In the after version, if the user isn't logged in, the premium account check isn't performed unnecessarily.

#### Putting Simplest or Cheapest Checks First

```java
if (database.hasRecord(user.id()) && user.id() != null && isValid(user.id())) {
    processRecord(user.id());
}
```

```java
if (user.id() != null && isValid(user.id()) && database.hasRecord(user.id())) {
    processRecord(user.id());
}
```
Quickly check for null/invalidity before the expensive database operation.

### Use Guard Clauses to Flatten Nested Branches

A **guard clause** is a check at the top of a function or loop that exits early if a certain condition is not met. Using guard clauses can reduce nesting by handling "bad" or edge conditions immediately, and letting the rest of the code assume those conditions are true. 

Deeply nested branching (especially many levels of `if` inside `if`) can make code hard to read. Guard clauses allow you to handle error cases or preconditions upfront and then proceed with the main logic in a flatter structure, thus avoid deeply nested branches.


Use guard clauses when:
- There are multiple validations that must all pass before executing the main logic.
- Handling error cases or edge cases that should immediately stop processing.
- The main action should clearly stand out at the bottom.

Consider the below example. An order will only processed if a series of logical checks are passed, and if any of them fails, we log the reason.
This program is hard to read and mainly due to deeply nested branches and in reality it could go even worse.

```java
void processOrder(Order order, User user) {
    if (order != null) {
        if (order.isValid()) {
            if (user != null) {
                if (user.isLoggedIn()) {
                    if (!user.isBanned()) {
                        order.process(); // Actual processing happens here
                    } else {
                        System.out.println("User is banned.");
                    }
                } else {
                    System.out.println("User is not logged in.");
                }
            } else {
                System.out.println("User object is null.");
            }
        } else {
            System.out.println("Order is invalid.");
        }
    } else {
        System.out.println("Order is null.");
    }
}
```

Fortunately, we can use guard clauses to flatten the branches.
```java
void processOrder(Order order, User user) {
    if (order == null) {
        System.out.println("Order is null.");
        return;
    }

    if (!order.isValid()) {
        System.out.println("Order is invalid.");
        return;
    }

    if (user == null) {
        System.out.println("User object is null.");
        return;
    }

    if (!user.isLoggedIn()) {
        System.out.println("User is not logged in.");
        return;
    }

    if (user.isBanned()) {
        System.out.println("User is banned.");
        return;
    }

    order.process();
}
```

## Recommended Leetcode Problems

Now it's time to test your understanding and mastery of writing branching code. The following leetcode problems are the ones that involve relatively complicated branching logic.

The following problems don't involve using data structures or algorithms:
- [13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/)
- [844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/)
- [8. String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)
- [71. Simplify Path](https://leetcode.com/problems/simplify-path/)
- [334. Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)
- [665. Non-decreasing Array](https://leetcode.com/problems/non-decreasing-array/)
- [1071. Greatest Common Divisor of Strings](https://leetcode.com/problems/greatest-common-divisor-of-strings/)
- [65. Valid Number](https://leetcode.com/problems/valid-number/)

The following problems somehow involves data structures such as stack, binary tree:
- [1763. Longest Nice Substring](https://leetcode.com/problems/longest-nice-substring/)
- [99. Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree/)
- [117. Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)
- [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)
- [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
- [549. Binary Tree Longest Consecutive Sequence II](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence-ii/)
- [678. Valid Parenthesis String](https://leetcode.com/problems/valid-parenthesis-string/)
- [735. Asteroid Collision](https://leetcode.com/problems/asteroid-collision/)
- [930. Binary Subarrays With Sum](https://leetcode.com/problems/binary-subarrays-with-sum/)
- [951. Flip Equivalent Binary Trees](https://leetcode.com/problems/flip-equivalent-binary-trees/)
- [971. Flip Binary Tree To Match Preorder Traversal](https://leetcode.com/problems/flip-binary-tree-to-match-preorder-traversal/)
- [1676. Lowest Common Ancestor of a Binary Tree IV](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-iv/)
- [269. Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)

# Loop/Iteration

## Idea of Repetition

Repetition has great power, as it allows a finite set of instructions (our code) to handle infinite number of problems. When dealing with collections of arbitrary size or problems of arbitrary scale, repetition is necessary. In programming, there are two types of repetition in programming: loops (iteration) and recursion. In fact, loops and recursion are interchangeable in the sense that any loop can be rewritten recursively and vice versa.

If we need to do something for more than once or an unknown number of times to achieve some goal, we should consider loop or recursion. When designing an algorithm, we need to identify steps that might need to repeat many times in the process/algorithm, and those steps will likely be expressed as a loop or recursion.

In this section, we’ll focus on loops (iteration) and discuss systematic ways to implement and reason about them.

## Loop Statements

Loops allow a program to repeat a block of statements(a set of actions) multiple times until a condition is met. Different programming languages have different types of supported looping mechanisms. 

You should know about the following types of loops and their structures:
- `while` loop is a type of **condition-controlled** loop and is often used when you don’t know in advance how many iterations you’ll need.
- `for` loop is a type of **counting** loop and is often used when you know how many iterations are required, such as iterating over arrays, lists and strings of known length.
- `for-each` loop is a type of simplified and enhanced `for` loop that is used for clean iteration over collections.
- `do-while` loop is a variant of `while` loop that executes the loop body at least once, which is rarely used but good to know.

You should also be familiar with the following loop control statements:
- `break` immediately terminates the loop, and it's used for early exit when a desired condition is met.
- `continue` skips the rest of the current iteration and proceeds to the next iteration (in a `for` loop, it still executes the `update` part before the next condition check).
- `return` inside loop exits not just the loop but the entire function/method, which is common in search problems where a result can be returned immediately.

The most basic loop structure is the `while` loop structure, and all other loop mechanisms could be converted to a `while` loop. For example, `for` loop can be easily converted to `while` loop, but it's often easier to write and read than the `while` loop counterpart. In fact, in competitive programming, you’ll likely use `for` loops a lot (for counting or iterating collections) and `while` loops for more open-ended repetition.

```java
// while loop structure
while (condition) {
    // Body: do something while condition holds
}
```

```java
// for loop structure
for (initialization; condition; update) {
    // Body: do something with current index/state
}
```

```java
// for-each loop structure
for (ElementType element : collection) {
    // Body: process current element
}
```

```java
// do-while loop structure
do {
    // Body: do something at least once
} while (condition);
```

```java
// infinite while loop with manual exit
while (true) {
    doSomething();
    if (shouldExit()) break;
    doSomethingElse();
}
```

## Methodical Loop Analysis

### Elements of a Loop

When analyzing or designing a loop, especially in algorithmic problems, it’s helpful to decompose it into fundamental elements, which are closely related to each other. The following are the core elements of a loop:
- **Loop Goal**: the target/objective of this loop, what it aims to accomplish.
- **Loop Premise**: for this loop to operate correctly, what premise must be true.
- **Loop Initialization**: setup code that initializes loop control variables and other related variables before entering the loop.
- **Loop Condition**: the conditions that determines whether the loop should continue or terminate(exit condition).
- **Loop Termination**: what we know after the loop terminates and whether the variables have correct and useful values.
- **Iteration Goal**: the goal or the side-effect of each iteration.
- **Loop Body**: the block of code executed in each iteration of the loop.
- **Edge Cases**: initialization, termination and corner cases.
- **Loop Variant**: abstract quantity that decreases in each iteration and drives the loop towards termination.
- **Loop Invariant**: useful properties of variables used inside the loop that are true before(and after) each iteration and can help reason about the correctness of the loop.

In addition, we can also mention the definition of variables(suc as loop control variables) used in the loop, which is optional but provides more information in the documentation.

To make a loop correct, all of these elements are important, and the following sections will elaborate each of them. We confine the discussion to single-thread algorithmic programs.

I personally think **Loop Invariant** is the most valuable and important thing about a loop that helps programmer implement and reason about the loop. In a coding interview, if you cannot explain the loop invariant of a particular loop, the interviewer may think you don't understand your code and cannot prove its correctness.

Asking yourself the following questions and perhaps documenting them in the code comment would be helpful:
- what's the loop invariant?
- what's the meaning of each variable/loop
- what's the goal/objective of each iteration
- what happens after each iteration?
- what do we know after the loop terminates?

Relationships Between Elements:
1. **Loop Premise** holds -> **Loop Initialization** sets variables -> **Loop Invariant** established before first iteration.
2. **Loop Condition checked** -> If true: **Loop Body** transforms state from iteration $i-1$ to $i$, achieving **Iteration Goal**.
3. **Loop Variant** progresses via updates -> **Loop Invariant** maintained.
4. **Loop Termination** when condition fails or early exit occurs.
5. **Loop Goal** achieved after termination, due to **Loop Invariant** being true.
6. **Edge cases** ensure correctness at boundaries (first, last, zero iterations).

### Loop Goal

**Definition**: Loop goal is the high-level objective that the loop seeks to accomplish.

**Relation**: It’s equivalent to the postconditions of a function/method, as we want the goal to be true once the loop terminates. 

**Importance**: It establishes correctness criteria for the loop.

**Notes**:
- A lot of things could happen within a loop, so there could be other side effects besides the loop goal after termination, especially if you are not careful with you code. Remember to check your code to see if there are unwanted side effects aside from the loop goal.

**Concept Examples**:
- Find the sum of all elements.
- Check if the array contains duplicates.
- Find the target value in a sorted array.

**Code Example**:
```java
// Loop goal: Find the sum of all elements in array nums[].
int sum = 0;
for (int num : nums) {
    sum += num;
}
// After the loop terminates, sum is the total of elements.
```

### Loop Premise

**Definition**: The required assumptions for the loop to operate correctly.

**Relation**: It's equivalent to the preconditions of a function/method.

**Importance**: Prevents runtime errors and logical faults.

**Notes**:
- To make loop premises true, we often need to do some work before the loop. For example, if we want to use binary search to search for a number in an array, we need to first sort it. But this kind of actions are not part of the loop. 
- Loop premises are established outside the loop. Do not confuse it with loop initialization.
- Loop premises are often checked explicitly using guard clauses.
- Sometimes loop premise are too trivial and obvious so it won't be mentioned. For example, iterating through a collection that is guaranteed to be non-null and not empty.

**Concept Examples**:
- The array is non-null and not empty. This premise is common all loop that read or write an array.
- The array is sorted in ascending order. This premise is common for many algorithms, for example, using binary search on an unsorted array doesn't make sense.

**Code Example**:
```java
// Loop premise: nums is non-null and not empty.
if (nums == null || nums.length == 0) return -1; // safeguard loop premise
int max = Integer.MIN_VALUE; // This is loop initialization, not used to ensure loop premise
for (int i = 0; i < nums.length; i++)
    if (nums[i] > max)
        max = nums[i];
```

### Loop Initialization

**Definition**: Setup code that initializes loop control variables other related variables before entering the main loop. 

**Relation**: Takes place before the main loop body. How the variables should be initialized should be determined by the loop variants. It also ensures loop invariants hold before first iteration. 

**Importance**: Guarantees loop starts correctly, establishing loop invariant initially.

**Notes**:
- Logically, loop initialization is part of the loop, but it's physically outside the main loop structure. In fact, a loop will run indefinitely if it only access variables declared inside the loop.
- Loop Initialization is not used to satisfy the loop premises. It could be simple or complicated. For example, in many sliding window problems, we need to initialize the window variable before going into the main loop, and that initialization itself involves a loop.
- Initialization is important because not everything we need to do should be included inside the loop. Only those that are exactly same in nature requires repetition. 
- Sometimes the first case(s) work differently from the rest, and it's a good practice to handle them in the initialization phase.

**Concept Examples**:
- Loop counters (`int i = 0;`)
- Two pointers (`int left = 0, right = nums.length - 1;`)
- Collection initialization (`List<Integer> list = new ArrayList<>();`)

**Code Examples**:
```java
// Initialization for two-pointer approach to reverse array.
int left = 0, right = nums.length - 1;
while (left < right) {
    int temp = nums[left];
    nums[left++] = nums[right];
    nums[right--] = temp;
}
```

```java
// Initialization for a collection that stores all positive numbers in an array.
List<Integer> list = new ArrayList<>();
for (int num : nums) {
    if (num > 0) {
        list.add(num);
    }
}
```

```java
int max = Integer.MIN_VALUE; // This is part of loop initialization
// Although written inside the for loop header, int i = 0 is part of the loop initialization and not part of the main loop
for (int i = 0; i < nums.length; i++) 
    if (nums[i] > max)
        max = nums[i];
```

### Loop Conditions

**Definition**: The boolean expressions determining whether to continue or terminate the loop.

**Relation**: Provides inferred knowledge after the loop terminates as we know loop condition doesn't hold or one of the exit condition is true. It's also closely connected to the loop variants.

**Importance**: Controls execution flow, ensures termination, prevents infinite loops and contributes to loop goal.

**Notes**:
- No loop is supposed to run forever, so we need to use loop conditions that can guarantee the termination of the loop. Notice that there are practical long running programs that are supposed to have no exit condition and run forever until the program is shut down from outside. So our discussion is confine to algorithmic programs.
- Loop conditions include when the loop should continue as well as when the loop should terminate. Some people distinguish between the two:
    - **Loop Condition** is continuation logic, for example `while (condition) { statements }`.
    - **Exit Condition** is early stopping logic, for example `if (condition) break;`. 
    - Here we adopt the broad definition of loop condition and when we will use the term exit conditions when we discuss specifically the case of breaking out of the loop inside it.
    - Thus the location of loop condition is flexible, as it could be at the beginning of each iteration, or inside the loop body.
- Always ensure loop conditions are clear and capable of eventually becoming false. Due to the if-else structure, sometimes the exit condition is not explicitly defined. Make the conditions explicit or implicit according your needs.
- Loop conditions should be determined by loop variants. For example when we use a loop index, then the loop range could be connected with the loop conditions. If we want the index $i$ to vary from $0$ to $n - 1$, then the loop condition would be $i < n$. Besides loop index, there are other forms.

**Concept Examples**:
- Index bound checks (`i < n`). For example, in `for` loops, the loop condition could be that the value of the loop index must be within a certain range.
- Pointer crossing checks (`left <= right`). For example, in binary search, which is often implemented as a `while` loop, the loop condition is that the subarray delimited by $[left...right]$ is not empty.
- Remaining elements in the collection (`!collection.isEmpty()`)

**Code Example**:

```java
int left = 0, right = nums.length - 1;
while (left <= right) { // Loop condition for binary search. It continues if the search space isn't empty
    int mid = left + (right - left) / 2;
    if (nums[mid] < target) left = mid + 1;
    else if (nums[mid] > target)right = mid - 1;
    else return mid; // Exit condition, when nums[mid] is equal to target, which means we have found the answer
}
// After the loop terminates we know that left > right (target not found). The other exit condition nums[mid]==target won't reach this line.
```

### Loop Termination

**Definition**: When and why the loop stops; what is known after termination.

**Relation**: Closely connected with loop initialization, loop variants and loop conditions. We need all relevant information to determine the properties of the variables after termination.

**Importance**: Critical for reasoning about final outcomes. 

**Notes**:
- The termination describes precisely when and how the loop stops, and what conditions hold true after termination.
- If there are $n$ conditions for the loop to terminate, and there are $2^n$ possibilities of their combinations after the termination. In addition, we need to combine the knowledge of how the loop is initialized and what are the loop variants to reach useful conclusion. It's common that after the loop terminates, we use branching to handle different combinations of outcomes.
- For a correct loop implementation, loop invariant and loop goal should be true after termination. We need to be very clear about what properties about what variables we have after the loop ends.

**Concept Examples**:
- If the only loop condition is (`i < n`) and no other exit conditions, $i$ is initially $0$, $n$ is constant and initially positive, $i$ is incremented for each iteration. we know that after the loop terminates, $i = n$.
- If the only loop condition is (`!collection.isEmpty()`), we know that after the loop terminates the collection is empty.


**Code Example**:
```java
// Search for a value
boolean found = false; // Initialization
for (int num : nums) {
    if (num == target) { // Exit condition
        found = true;
        break; // early exit
    }
}
// After termination: if found is true then target exists in the nums, otherwise it's not (because found is initially false)
```

### Iteration Goal

**Definition**: The per-iteration objective (intermediate step toward loop goal).

**Relation**: Iteration goal is defined using loop invariants, and achieved through the loop body.

**Importance**: Provides logical clarity per iteration, ensures correctness and progress.

**Notes**:
- Iteration goal describes what each individual iteration aims to accomplish. Typically with loop invariant we can know the properties of the `#i-1` iteration, and using these properties what we want to accomplish for the `#i` iteration.
- Iteration goal typically involves side-effects, because there is no way to terminate a loop if we only read but not modify variables. Notice that this is only true for single-thread programs. In a multi-threading setting, the variables we access in a loop could be modified by other threads.
- Loop could often be an inductive process, where each iteration is a step to the next.

**Concept Example**:
- Find maximum among elements in $[0..i]$ at `#i` iteration.
- Make the elements in $nums[0..i]$ sorted in non-decreasing order.

**Code Example**:
```java
// Loop goal: find max element in nums[0..n-1], where n is nums.length
// Iteration goal: find max element in nums[0..i].
int currentMax = nums[0];
for (int i = 1; i < nums.length; i++) {
    if (nums[i] > currentMax) {
        currentMax = nums[i];
    }
    // iteration goal achieve: currentMax is not max(nums[0..i])
}
```

### Loop Body

**Definition**: The block of code executed each iteration.

**Relation**: It achieves the iteration goal, maintains or utilizes loop invariants, and perform the actions of loop variants.

**Importance**: Performs the real work, transitions from one iteration to the next, core of the repetition.

**Notes**:
- **Iteration Dependency**: Depending on the concrete code, iterations could be dependent or independent.
  - Independent iteration have no dependence on previous iterations. e.g., printing elements.
  - Dependent iteration builds upon previous iterations. e.g., DP, prefix sums.
- For dependent iteration, the loop body shows how we use the properties of the `#i-1` iteration to accomplish the properties of `#i` iteration.

**Code Examples** (dependent iteration, Fibonacci):
```java
// Loop goal: compute Fibonacci sequence up to n
// Dependent iteration, the fibonacci number of i depends on previous two fibonacci numbers
int a = 0, b = 1;
for (int i = 0; i < n; i++) {
    int next = a + b; // dependent on previous two numbers
    a = b;
    b = next;
}
```

```java
// Independent iteration, each iteration doesn't depend on previous iterations
for (int num : nums) {
    System.out.println(num);
}
```

### Edge Cases

**Definition**: Special cases handled explicitly, often at initialization or termination.

**Importance**: Prevents subtle bugs, especially off-by-one errors.

**Notes**:
- Both initialization and termination(before the first iteration and after the last iteration) are edges cases. Corner cases are also edge cases, but it's highly case-by-case.
- Different algorithms have different corner cases, but typically if the collection we need to deal with have 0 or 1 element, then it's a corner case.
- Typically the first and the last iteration of the loop would introduce some problems. The first iteration is about initialization and the last iteration is about termination.
- Some people also consider the **best case** and **worst case** for an algorithm as its edge cases.
- Edge cases should also be stated with comments. we can find edge cases by making the assumption of our procedure FALSE or by observing out-of-bound cases of the codes

**Concept Examples**:
- Empty array (`nums.length == 0`)
- One-element array (`nums.length == 1`)
- Loop condition false from start (zero iterations)
- Loop with One Iteration Only

**Code Example**:

```java
if (nums == null || nums.length == 0) return -1; // handle empty array explicitly
```

### Loop Variant

**Definition**: A loop variant is a concrete measure (often numeric) that shows how close a loop is to termination. It's a quantity that:
- Changes in each iteration, often in a sense of decrement.
- Moves consistently toward a condition that stops the loop.
- Guarantees that the loop won't run forever.

**Relation**: Directly related to the loop control variables, how they should be initialized and updated in loop body, and thus closely connected with loop initialization, loop conditions and loop termination.

**Importance**: Guarantees loop termination by ensuring measurable progress.

**Notes**:
- In single-thread algorithmic programs, a loop is not supposed to run forever, and any loop should eventually terminate. The loop variant is such a measure of how close the loop is to termination. For a loop destined to terminate in a deterministic way, it must have a finite number of iterations, and the loop variant acts like a counter of the number of remaining iterations, going from a non-negative integer to 0.
- Another way to describe loop variant: You can think of the loop variant as a "countdown" toward loop termination.
    - Each iteration "ticks" the countdown closer to zero or some termination boundary.
    - When the variant reaches a termination point, the loop stops.
- To put loop variant under the context of programming elements, an explanation is to treat the loop body as a function, all the variables accessed(read or write) in the loop body as parameters of that function, and the output value should decrease for every iteration. 
    - Indeed, if we take a closer look at the variables accessed in the loop body, they can be divided into several categories and we will discuss that later. The ones that are directly relevant to loop variant are called loop control variables.
- **Loop Control Variables**: variables used in loop conditions(including exit conditions), whose values could be changed inside the loop body. They could have many forms. 
    - For example in `for (int i = 0; i < n; i++)`, the integer $i$ is a loop index variable, which is a common form of loop control variables.
    - Loop control variables directly control whether the loop continues or terminates, and thus they collectively map to an abstract measure of how close the loop is to termination, which is the loop variant.
    - A concept that is closely related to loop control variables is **loop (variable) range**, that is the valid range of the variables that are allowed to go into the loop body. Typically, when the variables go outside of its valid range, the loop condition will evaluate to false and the loop terminates.
    - As you will see, loop control variables also typically appear in the loop invariant. Due to their duel roles in the loop code, the choice of their definitions can impact the complexity of reading and writing the loop.
- Loop variants are represented by loop control variables, but they are not identical. 
    - A loop variant is typically derived from loop control variables. It is an abstract, logical measure that maps clearly to the control variables.
    - Loop control variables are concrete variables modified each iteration and appear directly in loop conditions.
    - For example in binary search, we have the $left$ and $right$ pointers of an array, which are loop control variables. But the loop variant is the size of the subarray delimited by $[left...right]$. A correct implementation of a loop ensures that loop control variables are updated after each iteration, moving the loop toward termination. Here the size of the subarray $[left...right]$ must decrease for each iteration, and when it reaches 0, the loop terminates.
- **Loop Update**: Concrete code that modifies loop control variables.
    - Loop Update is a concrete progress mechanism that changes the loop control variables, while loop variant is an abstract measure of progress.
    - For example `i++`, `left++`, `right--` are loop update (the actual code modifying loop variables); while the size of search space, number of unprocessed nodes, etc are loop variants.
    - The correctness and successful termination of a loop depends on loop update. For each iteration, at least one of the loop control variables should be assigned a new value. Otherwise the loop will run indefinitely.
- We care about how loop control variables are initialized outside loop, how they are updated inside the loop body, and whether they can lead to successful termination. 
    - For example, if we use a loop index variable as the loop control variable, we should pay attention to the index range: the start index, the end index and how the index is updated. 
    - The start and end of the loop index indicates the range of the array/list that the loop will examine. The reason why choosing a specific start and end index often have some mathematical meanings in the context, and it's a good practice to state that in comment, unless it's very obvious.
    - `for (i = a; i < b; i++)` is equivalent to `for(i = a; i <= b - 1; i++)`, both iterate `b - a` times. The loop range is `[a...b-1]`.
    - `for (i = a; i > b; i--)` is equivalent to `for(i = a; i >= b + 1; i--)`, both iterate `a - b` times. The loop range is `[b+1...a]`.
- There is a lot to talk about how the loop control variables could be updated.
    - **Idempotent Loops**: Some loops can run multiple times with no state change (idempotent behavior).
    - **Order-sensitive Loops**: Some loops depend on iteration order (stateful accumulations), e.g., DP, prefix sums.
    - **Order-agnostic Loops**: Some loops don’t depend on iteration order (map-reduce style parallel loops), e.g., summing independent values.
    - **Iteration direction**: Typically when traversing a list/array, iterating from left to right and right to left are equivalent(may requires some conversion on how we initialize and update the loop control variables). Sometimes one direction would be easier than the other and for some algorithms only one of the directions work.
- If you take a look at the wikipedia page of loop variant, you will see some some rather formal academic definitions, so here I tried to somehow simplify it but still keep the essence.
- Although we typically assume a loop should always terminate due to some loop condition, there are exceptions especially when we consider multi-threading and event loop programs. For example, in a producer-consumer pattern multi-threaded program, the consumer could be long-running until the system stops it. 
- Each loop could have multiple loop variants and the net effect is to take the minimum of them. 
    - For this reason, loop variants are closely tied to the time complexity of the loop.
    - For example, a loop could have loop variant 1, which makes the loop terminates in 20 iterations, but the loop could also have another loop variant 2, which makes the loop terminates in 10 iterations. The net effect of the two loop variants, by taking the minimum, is that the loop will terminate in 10 iterations.
- The time complexity of running a loop is directly tied to loop variant: 
    - The loop variant's rate of change impacts time complexity: how the loop control variables are initialized, how they are updated and when the loop terminates. So loop variant helps analyze loop performance from design.
    - Variant decreases by $1$ -> $O(n)$
    - Variant halves each time -> $O(\log(n))$
- Both loop variant and loop invariant are connected with meaning and definition of variables accessed in the loop. There are variables accessed in the loop that are not considered as loop control variables and thus have little to do with loop variant, so it's normal for you to feel confused. We will discuss their distinction and more about loop variant, loop control variables in the section of loop invariant.

**Identify Loop Control Variables and Loop Variant**:
1. Identify the loop condition clearly. For example, `while (left <= right) { ... }`.
2. Extract variables involved that change inside the loop: here, $left$ and $right$ (both potentially modified). These are your loop control variables.
3. Define loop variant clearly:
    - Usually a single numeric measure directly derived from these variables.
    - Must consistently approach a termination boundary.
    - Example for binary search: Variant = $right - left + 1$ (size of search space), which decreases every iteration (for a correct implementation).

However, this only tells you how to identify them, not how to choose them for your loop.

**Concept Examples**:
- Loop index $i$ could increment (`i++`) or decrement (`i--`), which means the remaining number of unexamined elements in the array decreases($nums[i...n-1]$ or $nums[0...i]$ depending on the iteration direction)
- Size reduction of search space ($right - left$ decreases for each iteration)

**Code Example**:
- See the examples in the section of loop invariant, where we will discuss the loop variants and invariants of typical algorithms together.

### Loop Invariant

#### Definition

A **loop invariant**:
- Is a non-trivial logical condition/statement/proposition about variables accessed in the loop
- Is true before the loop starts
- Remains true before and after every iteration
- Helps us reason about correctness of the loop, as it's true after loop terminates
- Ultimately supports the achievement of the loop goal

Loop invariant is connected with most loop elements and it's the key of why complicated loops work. It is like a promise that you make before the loop starts, and you must keep this promise through every iteration until the loop ends.

---

```java
int max = Integer.MIN_VALUE;
int n = nums.length;
for (int i = 0; i < n; i++) {
    if (nums[i] > max) {
        max = nums[i];
    }
}
```

In the above code example, the loop invariant could be stated as bellow:
> At the start of `#i` iteration, $max$ is the maximum element in $nums[0..i-1]$.

- At the start of the `#0` iteration, that is before the main loop, $max$ is assigned `Integer.MIN_VALUE`, a reasonable initial value, and the code also suggests if the $nums[]$ is empty, $max$ will end up with this value. So initially, $nums[0...-1]$ is an empty subarray, and $max$ being `Integer.MIN_VALUE` conforms the loop invariant.
- During the loop, the `if` branching code maintains the loop invariant: if $max$ is the maximum element in $nums[0..i-1]$ at the start of `#i` iteration, then it would be the maximum element in $nums[0..i]$ at the end of the `#i` iteration, which after the increment of $i$, makes the loop invariant also true at the start of `#i+1` iteration.
- After the loop terminates, we actually have $i=n$, which is the start of the final iteration that never enters the loop body. If we successfully maintain the loop invariant throughout every iteration, which we do, then according to the loop invariant, $max$ is the maximum element in $nums[0..n-1]$, by substituting $i$ with $n$. Since $nums[0..n-1]$ is the entire array, now we can be certain that $max$ store the maximum element of the array.
---

<img src="img\loop_invariant_on_a_line.png" alt="loop_invariant_on_a_line" style="display: block; margin-left: auto; margin-right: auto;">

**Notes**:
- We typically state the loop invariant as something that is true at the start of every iteration.
- When we talk about the "start of an iteration",
    - In both `while` and `for` loops, when we say the start of an iteration, we mean the moment just before the evaluation of the loop condition, and after the update of the previous iteration. As for the end of an iteration, in `for` loops, we consider it the moment after the update part in loop header executes, in the above example it's `i++`; in `while` loops, we consider it the moment after the last statement inside the loop body.
    - So either way, the end of an iteration is identical to the start of the next iteration, and you can see this from the above image, which corresponds to the red part.
    - For the first iteration: the moment at which we check the loop invariant just prior to the first iteration is immediately after the whole loop initialization, which includes the initial assignment to the loop-counter variable, and just before the first loop condition evaluation in the loop header.
    - For the last iteration: the loop invariant is true before the loop condition evaluation, but since the loop terminates at this iteration, just after the loop terminates, the loop invariant is still true. But those variables defined in the loop won't be accessible anymore.
- Loop invariants are highly case-by-case, but they should be non-trivial, dynamic, and structurally relevant properties that 
    - Depend on loop-executed changes (e.g., updated variables)
    - Are necessary to prove the loop's correctness
    - Would not obviously be true unless carefully maintained during each iteration.
- In the above example, the statement "$n$ is the length of $nums[]$" is always true (before, during, and after loop), never changes, does not depend on the loop, and is not useful for proving correctness of the loop's behavior. So it is not what we consider a loop invariant in the algorithmic sense.
- In general, a loop invariant must hold at the start of each iteration (before the loop body executes), and it must be re-established by the end of that iteration (just before the next condition check or loop exit).
- In complex loops, you may have multiple invariants tracking different aspects of state. They must all hold simultaneously at the start of each iteration.
- While the invariant is guaranteed at iteration boundaries, it may not hold during execution of the loop body, which corresponds to the blue part in the image, especially in complex or multi-step loops. This is OK as long as it’s restored by the end of the iteration.

**Variant vs Invariant**:
- Loop variant = "The progress meter": A numeric measure explicitly moving toward termination every iteration.
- Loop invariant = "The truth that stays true": A logical condition or meaning that holds true every iteration.
- Some people are confused about the difference between loop variant and invariants, but they are not opposite, instead they complement each other, serving distinct but interlocking roles in ensuring loop correctness.
- Two Pillars of Loop Correctness
    - Variant ensures the loop stops. A correct loop that never terminates is useless.
    - Invariant ensures the loop does the right work. A terminating loop that gives wrong results is also useless.
- Applied at Different Phases
    - The loop variant tracks progress between iterations. At the end of each iteration, the variant must have decreased.
    - The loop invariant ensures correctness within each iteration. At the start of each iteration, the invariant holds.
- Loop control variables often serve a dual purpose:
    - As part of the loop variant, they represent how much work is left and they drive termination through loop conditions.
    - As part of the loop invariant, they help describe how much work has been done, acting as parameters in loop invariants.
    - This dual role makes loop control variables the bridge between loop variant and loop invariant.
- Some people say the loop variant represents the future of the loop and the loop invariant represents the past of the loop.

Variant vs. Invariant: Complementary Roles
| Concept                 | Loop Variant                              | Loop Invariant                           |
| ----------------------- | ----------------------------------------- | -----------------------------------------|
| **Purpose**             | Guarantees termination                    | Guarantees correctness of computation    |
| **Tracks**              | How close the loop is to finishing        | What property is consistently maintained |
| **Changes?**            | Must strictly decrease (well-founded)     | Must not change (logically preserved)    |
| **Failure to Maintain** | Risk of infinite loop                     | Risk of logical error / incorrect output |
| **Analogous to**        | A countdown clock                         | A safety contract                        |

**Exception**:
- There are special cases that could make loop invariant false after loop termination. This is because the invariant could be temporarily violated inside the loop body, and if we use `break` or `return` to early exit the loop before restoring the loop invariants, they could be false after the loop terminates.
    - Natural loop termination, where the invariant can be assumed to hold afterward.
    - Abrupt loop termination (via break/return), where you must verify manually whether the invariant holds at the exit point.
        - `break/return` after re-establishing invariant, like `Example_2`, then the invariant holds after the loop, 
        - `break/return` while invariant is broken, like `Example_1`, then you have to reason about what happen to the variables used in invariants
- You could either redefine the invariant or restructure the loop to avoid code like `Example_1`. But even if this is unavoidable, the loop invariant is still true at the start of the last iteration, and using that information along with how the variables related to loop invariants are modified before the loop exits, you could still properly reason about the correctness of the loop.
- This means even in the special cases, loop invariants still play an important role in the reasoning about correctness, but it's just that you can't directly use it after the loop terminates and need to do some extra work.

```java
// Example_1
while (true) {
    doSomethingThatViolatesLoopInvariants();
    if (shouldExit()) break;
    doSomethingThatRestoresLoopInvariants();
}
```

```java
// Example_2
while (true) {
    doSomethingThatViolatesLoopInvariants();
    doSomethingThatRestoresLoopInvariants();
    if (shouldExit()) break;
    doSomethingIrrelevantToLoopInvariants();
}
```

**Other Conceptual Examples**:
- At the start of `#i` iteration, $nums[0..i-1]$ consists of the original elements in $nums[0...i-1]$ but now sorted in non-decreasing order.
- At the start of `#i` iteration, all elements in $nums[0...i-1]$ have been printed to console.
- At the start of `#i` iteration, sub-array $nums[0...i-1]$ doesn't contain $target$.

#### Importance

**Correctness**: loop invariants are used in proofs, especially inductive reasoning to ensure every iteration maintains certain properties essential for correctness.
- Loop invariants are the core of correctness proofs and basis for inductive reasoning about the loop. On the other hand, deviation from loop invariant, or equivalently, failure to maintain loop invariant, implies incorrect loop implementation.
- The reasoning of a loop is basically mathematical induction to prove when the loop invariant for $i-1$ iteration is true, how the loop body makes it also true for the current $i$ iteration. So that at the beginning of the iteration we have the properties of the previous loop and at the end of the iteration we need to arrive at something useful for the next iteration, which is exactly defined by the loop invariant.
- This is essentially a proof by induction and loop invariant is essentially the inductive hypothesis.:
    - Base case $P(0)$: The invariant holds before the first iteration of the loop (after initialization).
    - Inductive step $P(i)$: If it holds before an `#i` iteration, the loop body may mutate state, but must ensure the invariant holds again before `#i+1` iteration.
    - Termination $P(n)$: When the loop ends, the invariant combined with loop conditions implies the loop goal.

```plain
                                        [Initialization]
                                                ↓
                                        Base Case: P(0) holds
                                                ↓
                                            [Loop Entry]
                                                ↓
                                Inductive Step: If P(i), show P(i+1)
                                                ↓
                                        [Condition Fails]
                                                ↓
                                Termination: P(n) + ¬Condition ⇒ Goal
```

- Think about the following questions when using or identifying loop invariants:
    - Before the loop begins: Is the invariant true?
    - During each iteration: Does the loop body preserve the invariant?
    - After the loop ends: Does the invariant, combined with the loop condition being false, help prove the loop goal?
- Loop invariants are less important for some loops, those who have little to no relation between iterations. For example in a lot of `for-each` loop, the iterations are independent, in which case loop invariant doesn't tell us much useful information. But typically loops like this doesn't require complicated analysis.
- If you want to see an detailed example of proving the correctness of an algorithm using loop invariant, you can refer to the insertion sort chapter of *Introduction to Algorithms*, which gives a perfect demonstration. 

**Design and Implementation**: clear loop invariants could guide loop design and implementation.
- Writing or stating an invariant before coding can:
    - Clarify what the loop must maintain
    - Help programmer decide what variables are needed
    - Prevent incorrect updates or misplaced statements
- While coding, we need keep the invariants in mind:
    - When implementing the loop body, which is code for each iteration, we can assume the loop invariant is true at the start of the loop, and use this piece of information as the basis to come up with the loop body.
    - We need to make sure at the end of the iteration, the invariants are preserved, which could shape the loop body.
- Since loop invariants should be true before the first iteration, so they give us clear clue about how to implement loop initialization, how to initialize the variables, and what are the proper initial values for them.
- Loop invariants are about how current iteration connects with all work that has been done in all previous iterations. Implementing and understanding a loop requires us to be clear what we want to go from `#i` iteration to `#i+1` iteration, and break it down into multiple smaller task. During the process, we need to generalize the loop goal as $P(n)$ and find out what $P(i)$ means, which is typically closely connected with loop invariants.
- We will see some examples on how to utilize loop invariants in solving programming problems.

**Documentation**: loop invariants helps the reader understand the logical structure and guarantees of the loop.
- Source code is not just for execution, but also for reading, and not just for other people, but also for the programmer who wrote the code. Because no one could remember everything about the code they write. Proper documentation makes maintaining and understanding code much easier. And writing documentation or comment for loop, loop invariant is the most important element to include.
- Stating the invariant explicitly as a comment or docstring:
    - Helps readers understand the loop's structure
    - Makes code easier to maintain and review
    - Especially helpful for complex logic or nested loops
- When reading code of a loop, focus on the loop invariants:
    - If the loop invariants are clear documented, remember to inspect how it's maintained across iterations
    - Hypothesize the invariant if it's not written, and use it to explain each line in the loop
- Over time, a seasoned programmer will naturally internalize this habit, because it's like writing pre/post conditions, but inside the loop.

**Debugging**: loop invariants help check internal assumptions during development.
- When a loop doesn't produce the expected result:
    - Think about what should be true at the start of each iteration
    - Insert assertions or logging to validate the invariant during runtime
    - Quickly isolate the point where state diverges from expectation
- You can add runtime assert statements or print checks to catch invariant violations.
- We will see examples of property-based testing(PBT) utilizing loop invariants later.

```java
// Code could be simplified if you throw error inside the loop invariant check
while (checkLoopInvariant() && condition) {
    loopBody();
}
```
OR
```java
if (!checkLoopInvariant()) throw error;
while (true) {
    if (!checkLoopInvariant()) throw error;
    if (condition) break;
    loopBody();
}
if (!checkLoopInvariant()) throw error;
```

#### Loop Variable Semantics

**Know the Variables**: A key idea that cannot be over-emphasized - you should always be clear about the meaning/definitions of the variables that you declare and use in the program, as each of them serves a purpose and could be subject to some constraints. A clear and accurate understanding of every variables used in the program is the premise of ensuring we perform correct read and write operations on them.

We discussed about variables accessed in the loop briefly in the section of loop variant, and now is a good time to scrutinize them. We don't need to concern ourselves with the variables not accessed in the loop, because they have no impact on how the loop operates.

For the variables that are accessed in the loop, there are two categories:
1. **Contextual Variables**: Variables that are read-only inside the loop, but they are critical to its behavior.
    - They don't change their value in the loop, so they could be considered as constants, but still semantically essential.
    - They define boundaries and contexts for iteration and they’re referenced constantly. For the loop, they often act as parameters or inputs.
    - They may not participate in control flow progression, and even though they’re not "control" or "state" variables, they form the logical framework in which loop computation takes place.
    - They may appear in the loop condition (e.g., $i < n$), but since they do not change over time, and thus do not drive the termination, while the actual progress is made by Loop Control Variables.
    - They may appear in the loop invariants (e.g., $nums[]$), but they don't carry stateful information, but provides the context of State-Carrying Variables.
2. **Stateful Variables**: Variables that may be modified during loop execution. They play different roles in the logic and semantics of the loop and can be classified into three distinct categories:
    1. **Loop Control Variables**: stateful variables that are read in the loop conditions, including the exit conditions. 
        - They control whether the loop continues or terminates.
        - They collectively map to an abstract measure of how close the loop is to termination, which is the loop variant.
        - They must change for each iteration, meaning at least one of the loop control variables should be assigned a new value. Otherwise the loop will run indefinitely.
        - They are typically also part of loop invariants and they help describe how much work has been done. In fact, you can hardly analyze or describe a loop without reference to the loop control variables.
    2. **State-Carrying Variables**: stateful variables that accumulate or evolve state of the loop, and often embody intermediate product of the loop.
        - They represent state being built, accumulated, or transformed across iterations.
        - They are directly tied to loop invariants, which describe their meaning per iteration.
        - They have values that hold semantic meaning tied to the loop’s purpose, even though they don’t necessarily affect loop flow.
        - They may or may not appear in loop variants.
    3. **Incidental Variables**: all other stateful variables. Although they also have many kinds, we don't divide them further.
        - They are modified inside the loop but neither drive the loop's control logic nor carry meaningful loop invariants.
        - They mainly support secondary purposes such as side effects or auxiliary operations that are unrelated to loop control or algorithmic correctness. 
        - They include any variables defined inside the loop, and these variables are short-lived and temporary as they don't carry information across iterations.
        - They include any variables that serve external purposes, such as logging, I/O, metrics collection etc. They are part of the loop's side effects but not tied to its main purpose.
        - They include any variables that are part of the legacy or misplaced code. They could be never used and don't affect anything, serving no functional purposes. 
- The above classification of variables are based on their access pattern and role in control or logic.
- It's possible for an variable to be both Loop Control Variable and State-Carrying Variable, and this often means it's part of both the variants and invariants.
- For Stateful Variables, we say a variable could be modified when there is at least one statement in the loop that modifies its value. In the actual execution, its value may stay unchanged due to branching, but as long as there is possibility it could be modified, we don't consider it a constant.

When we analyze a loop, the meaning of the Contextual Variables are often straightforward. But for the Stateful Variables, many people struggle to properly define their meanings. Let’s revisit a classic example:

```java
int n = nums.length;       // [Constant]
int sum = 0;               // [State-Carrying]
for (int i = 0; i < n; i++) {  // [Loop Control]
    sum += nums[i];        // state evolves
}
```

For **Contextual Variables** in the example, $n$ is part of the control logic defining the loop range and is used in loop condition. and $nums[]$ provides context of operations on stateful variable, which is $sum$.

For a **Loop Control Variable**, we can combine the initialization, termination and update of the control variable to describe it, and stress its connection to the loop variant. In the above example,
- **Meaning of $i$**: $i$ is the loop control variable. To be more specific, it's the index variable we use to access the element in the array one-by-one. Its initial value is $0$, which corresponds to the first element, and we increment it by $n$ for after iteration. The loop continues until $i$ reaches $n$, ensuring that the value of $i$ covers the range of $[0...n-1]$ and each element is processed exactly once.
- **Connection to Loop Variants**: the loop variant is the number of remaining elements not yet examined in the array, which is the sub array $nums[i...n-1]$, whose size is closely tied to $i$. When this subarray becomes empty, the loop terminates.
- **Connection to Loop Invariants**: $i$ helps define what work has already been done. As mentioned below, we cannot state the loop invariant without mentioning $i$.
- **Oversimplification**: some people may say: "for each iteration $i$ points to an element in the array, which will be added to $sum$". This is also ok, but a little oversimplified, because it only states its meaning for a particular iteration, without showing the bigger picture of how it's related to the whole loop.

For a **State-Carrying Variable**, we can state how it's initialized, whether it's connected with loop invariant, how it's modified to maintain the invariant and what it tells us after termination. In the above example,
- **Meaning of $sum$ and Loop Invariant**: as a variable that accumulates the summation of elements in $nums[]$, at the start of iteration $i$, $sum$ is the total sum of element in $nums[0...i-1]$. Notice that the loop control variable $i$ also plays a part in the loop invariant, because it also indicates how much progress we have made.
- **Initialization, Maintenance and Termination**: $sum$ is initially $0$, which is a proper summation value of an empty subarray $nums[0...-1]$ and respects the invariant. For each iteration, the number pointed by $i$, which is $nums[i]$ will be added to $sum$, maintaining the invariant. According to the loop variant, there is only one way the loop terminates, that is when $i=n$, and after termination $sum$ is the total summation of the $nums[]$ array.
- **Oversimplification**: when new programmers are asked about the meaning of $sum$, some say "it's the sum of all elements in the integer array $nums[]$". 
	- They are not entirely wrong about the purpose, but they are wrong about the timing: they forget that $sum$ is a stateful variable, not a constant. Its value changes during the loop, and it only becomes the total sum after termination. So if we access it within the loop, we cannot assume it is the sum of all elements.
	- The biggest problem about this definition is that they don't see that $sum$ is modified by the loop for every iteration. Thus we should define the meaning of $sum$ with regard to the iterations of the loop, that is to contextualize its meaning per iteration, which often leads to an invariant.
- Normally, you will see that loop invariants are often the properties or definitions of some State-Carrying Variables, but Loop Control Variables are often used as parameters in loop invariants.

There are not **Incidental Variables** in this example, but we will see some in later ones.

**How to Define Stateful Variables**:
- Clearly states initialization (e.g. $0$).
- Clearly states how it's modified in the loop body. 
	- The update step (e.g. increment by $1$) for Loop Control Variables.
	- The maintenance step for State-Carrying Variables.
- Clearly states termination
	- The termination condition for Loop Control Variables (e.g. $i$ reaches `nums.length`).
	- The useful properties we can get from State-Carrying Variables after termination. (e.g. $sum$ is the total sum of the array).
- Other useful information
	- For Loop Control Variables, we can explicitly states coverage of the range and its guarantee to termination (e.g. it ensures that each element is processed exactly once).

**Summary**:
| Category                     | Role                                  | Access    | Commonly Tied to | Affects Control Flow? | Example                     |
| ---------------------------- | ------------------------------------- | --------- | ---------------- | --------------------- | --------------------------- |
| **Contextual Variables**     | Define fixed boundaries or parameters | Read-only | Loop Condition   | No                    | `nums.length`           |
| **Loop Control Variables**   | Drive loop condition and update       | Modified  | Loop Variant     | Yes                   | `i`, `left`, `right`        |
| **State-Carrying Variables** | Carry intermediate or final results   | Modified  | Loop Invariant   | No                    | `sum`, `prefix`, `result[]` |
| **Incidental Variables** | Side effects or Auxiliary operations   | Modified  | Other Purposes   | No                    | `LOGGER`, `tmp` |

We will see more examples in later sections.

#### Good Questions to Ask

What Makes a Good Invariant:
- It connects meaningfully to the goal of the loop.
- A non-trivial property that involves at least one variable modified in the loop.
- It’s always true during loop execution, and is preserved across all iterations.
- It’s strong enough to help prove the loop works correctly, and must be true for the loop to behave correctly.

Summary of Key Questions a Programmer Should Learn to Ask:
- What is the loop trying to accomplish?
- What must be true before and after every iteration?
- What variables or data structures capture this truth?
- How does this help me prove the loop is correct?
- What happens to the invariant when the loop ends?

How to Identify Loop Invariant?
- Is this property something that needs to be preserved by the loop body? Or is it just always true and unaffected?
- If the loop must act to preserve the property, then it's likely a loop invariant.
- If the property is just always true regardless of the loop, it’s not a meaningful loop invariant.

How to Classify Stateful Variables?
- Does this variable determine whether the loop stops? -> Loop Control
- Does this variable accumulate a result or represent evolving state? -> State-Carrying
- Is this variable modified but unrelated to correctness or flow? -> Incidental

#### More Examples

In this section, we will go through some simple but typical algorithms and list their loop variants and invariants. To make full use of these examples, you can look at the code first, think about what are the loop variants and invariants, and then go on toe see the answers.

In the Methodical Loop Implementation section, things will go wild and we will use variants and invariants to solve more complicated problems.

---

**Printing to Console**

```java
int i = 10;
while (i > 0) {
    System.out.println(i);
    i--;
}
```

- Contextual Variables: None.
- State-Carrying Variables: None.
- Loop Control Variable: $i$ (appears in condition and is modified)
- Loop Variant: $i$ itself (initially $10$, reduces each iteration, loop stops at $0$).
- Loop Invariant: at the start of each iteration, integers in the range $[i+1...10]$ have been printed to the console.
- Loop Termination: $i$ is $-1$.

---

**Finding Max**

```java
int max = nums[0];
for (int i = 1; i < nums.length; i++) {
    if (nums[i] > max) {
        max = nums[i];
    }
}
```
- Contextual Variables: $nums[]$.
- Loop Control Variable: index variable $i$, whose index range is $[1...nums.length-1]$
- State-Carrying Variables: $max$.
- Loop Variant: number of elements remaining ($nums.length - i$), which is the size of $nums[i...nums.length-1]$
- Loop Invariant: at the start of `#i` iteration, $max$ is the maximum element of subarray $nums[0...i-1]$
- Loop Termination: $max$ is the maximum element in the array.
- Notice that in the above code, we assume the array $nums[]$ is not empty (a loop premise). A more general version is as below:

```java
int max = Integer.MIN_VALUE;
for (int i = 0; i < nums.length; i++) {
    if (nums[i] > max) {
        max = nums[i];
    }
}
```

---

**Summing an Array**

```java
int n = nums.length;
int sum = 0;
for (int i = 0; i < n; i++) {
    sum += nums[i];
}
```

- Contextual Variables: $nums[]$.
- Loop Control Variable: $i$.
- State-Carrying Variables: $sum$.
- Loop Variant: number of elements remaining ($n - i$), which is the size of $nums[i...n-1]$.
    - Initially: $n - 0 = n$
    - After each iteration: this value decreases by one.
    - When $n - i == 0$, that is when $i$ is $n$, loop terminates.
- Loop Invariant: at the start of `#i` iteration, $sum$ is the summation of subarray $nums[0...i-1]$.
- Loop Termination: $sum$ is the summation of the entire array.
- Next we take a look at several variants of this program.
---

**Summing an Array Variant 1**

```java
int sum = 0; // the variable bound is declared before
for (int i = 0; i < nums.length && sum < bound; i++) {
    sum += nums[i];
}
```

- Contextual Variables: $bound$ and $nums[]$.
- Loop Control Variables: $i$ and $sum$.
    - Both variables appear directly in the loop termination condition.
    - Both variables are modified within each iteration of the loop.
- State-Carrying Variables: $sum$.
- Loop Variants: 
    - The loop’s progress can be measured by the minimum of two variants:
        - Number of elements remaining in the array: $nums.length - i$
        - Remaining allowed sum before exceeding bound: $bound - sum$
    - In each iteration, one or both decrease:
        - $nums.length - i$ decreases by $1$.
        - $bound - sum$ decreases by $nums[i]$.
    - This ensures that the loop cannot run forever.
- Loop Invariants: 
    - At the start of `#i` iteration, $sum$ is the summation of subarray $nums[0...i-1]$. Notice that this invariant is always preserved after the loop terminates.
    - $sum < bound$ is not a loop invariant, although it appears in the loop condition, but it can be false immediately after the loop ends, which disqualifies it as an invariant.
- Loop Termination:
    - The loop ends when either $i = nums.length$ or $sum \ge bound$, or both.
    - Without additional logic, we can’t determine which condition failed.
    - Loop invariant is maintained after termination.
- Notes:
    - When a variable (like $sum$) directly affects loop termination, it indeed becomes part of the loop control mechanism and loop variant. And it could also be part of loop invariant. This is the case where the same variable is both a Loop Control Variable and State-Carrying Variable.
- Notice that the above code is equivalent to the following:

```java
int sum = 0;
for (int i = 0; i < n; i++) {
    if (sum >= bound) break; // early exit
    sum += nums[i]; // modifies state
}
```
- For this implementation, even when the early exit takes place, the loop invariant is still maintained after termination.

---

**Summing an Array Variant 2**

```java
int sum = 0;
for (int i = 0; i < nums.length; i++) {
    sum += nums[i]; // modifies state
    if (sum >= bound) break; // early exit
}
```
- Contextual Variables: $bound$ and $nums[]$.
- Loop Control Variables: $i$ and $sum`$ (similar to Summing an Array Variant 1).
- State-Carrying Variables: $sum$.
- Loop Variant: MIN$(nums.length - i, bound - sum)$ (similar to Summing an Array Variant 1).
- Loop Invariant: at the start of `#i` iteration, $sum$ is the summation of subarray $nums[0...i-1]$
    - This is violated inside the iteration after $sum += nums[i]$.
    - If the loop continues, `i++` will reestablish the invariant at the next iteration.
    - But if `break` is triggered right after the update, the invariant is violated after the loop terminates.
- Loop Termination:
    - The loop ends when either $i = nums.length$ or $sum \ge bound$, but not both. Need additional logic to determine which is true.
    - If $sum >= bound$ is true, then the loop invariant doesn't hold anymore, and in this case $sum$ is the summation of subarray $nums[0...i]$, not $nums[0...i-1]$. Although $i$ is not accessible after termination, in general, we could have defined $i$ outside of the `for` loop.
    - This is why we need to be careful about early exit, as they could violate invariants.

Notice that the above code is equivalent to the following:
```java
int sum = 0;
int i = 0;
while (i < nums.length) {
    sum += nums[i]; // modifies state
    if (sum >= bound) break; // early exit
    i++;
}
```
---

**Binary Search**

```java
int binarySearch(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] > target) right = mid - 1;
        else if (nums[mid] < target) left = mid + 1;
        else return mid;
    }
    return -1; // not found
}
```

- Contextual Variables: $target$ and $nums[]$.
- Loop Control Variables: $left$ and $right$.
- State-Carrying Variables: None.
- Incidental Variables: $mid$.
- Loop Variant: size of elements in subarray $nums[left..right]$ (the search space), which is $right - left + 1$
    - It strictly decreases each iteration, ensuring termination.
    - In fact the midpoint calculation and the update on $left$ and $right$ is non-trivial, and incorrect implementation could cause infinite loop.
- Loop Invariant: if the $target$ is in the array, it's in the interval $nums[left...right]$.
- Loop Termination:
    - The loop ends when either $left > right$ or $target$ is located in the array.
    - We can use the return value to determine which case is true: $-1$ if it's the former case and a non-negative integer number for the latter case.

---

**Searching for the First Element Satisfying a Condition**

```java
for (int i = 0; i < nums.length; i++) {
    if (nums[i] % 2 == 0) {
        return nums[i];
    }
}
return -1;
```

- Contextual Variables: $nums[]$.
- Loop Control Variable: $i$, whose range is $[0...nums.length-1]$.
- State-Carrying Variables: None.
- Loop Variant: number of elements remaining ($nums.length - i$), which is the size of $nums[i...nums.length-1]$.
- Loop Invariant: at the start of iteration $i$, no even number exists in $nums[0...i-1]$.
- Loop Termination:
  - Normally when $i = n$, and then the invariant ensures no even number exists at all.
  - Or via `return`, in which case the invariant is bypassed (but not contradicted) because we exit immediately upon finding a violating case.
- Notes:
  - The invariant still helps explain the correctness — but does **not** necessarily hold after the loop if exited early. And that’s OK because the postcondition is separately ensured by the `return`.

---

## Methodical Loop Implementation

### From Analysis to Implementation

- Loop analysis helps you read and understand looping code, and it's the prerequisite of good loop implementation: you can only learn how to write good looping code after you can read them well and reason about them in a methodical way. 
- You will find loop implementation strongly connected to the techniques introduced in loop analysis, and the main work we need to do to implement a loop is to figure out the loop elements correctly.
- In this section, we will try to find a way to methodically implement looping code, with clear code structure and meaningful documentation.
- Typically when you solve a coding problem, you need to design the algorithm and then implement it. But since this section focuses on implementation, the high-level design will be provided for each algorithm, and you are encouraged to implement the algorithm according to that design first. After you finish the code, you can go on to read the rest.
- Depending on the coding problems, some are harder to design and others are harder to implement. This section is mainly about how to implement the algorithms methodically, and the premise is that we already have the high-level idea about the algorithm, which itself could be harder in solving practical problems.
- There will be several case studies. I will go into verbose details in the first case study, implementing the bubble sort, and be concise for later ones.

### Bubble Sort

#### High-Level Design

Sorting algorithms are the most classic algorithms. Bubble Sort is one of the easiest sorting algorithms, but it serves good learning purposes. For the following discussions, suppose we want to sort an array in non-decreasing order.

**This section is not a full treatment** about sorting algorithms or even Insertion Sort specifically, we only focus on how to methodically implement it.

**High-Level Design Perspective**:
- Bubble Sort is a simple comparison-based sorting algorithm that repeatedly iterates through the array and compares adjacent pairs of elements, swapping them if they are in the wrong order (i.e., the left is greater than the right), until the list is sorted. 
- **High-Level Procedures** that break this idea into concrete steps:
    - Start from the end of the array.
    - Compare each pair of adjacent elements.
    - If the left element is greater than the right one, swap them.
    - Repeat this process until you reach the end of the array, leaving the smallest unsorted element at the far left.
    - On the next pass, repeat the process again, but ignore all elements to the left of the last smallest element inclusively.
    - Continue until no more passes are needed (i.e., the array is sorted).
- This version of Bubble Sort works by selecting the minimum element. Conversely we can also select the maximum element, and the whole process is just a mirror version of the above steps.

<img src="img\bubblesort_visualization.png" alt="bubblesort_visualization.png" style="width: 30%; height: 30%; display: block; margin-left: auto; margin-right: auto;">

The above is a graphical representation of the procedure. 
- The grey portion doesn't participate in the later process.
- The graphical representation is a tool to help us understand the flow of the algorithm when writing the code. 
- For simpler problems, seasoned programmers plot the graph in their mind, quickly identify all the key information and finish the code swiftly. I believe with sufficient practice, you can do this too.

Before you go on reading, please **implement Bubble Sort by yourself** according to the steps of the algorithm. In addition, also annotate your loop control structure and explain what invariants you’re trying to maintain. We will later go into mid-level design and then implement the algorithm based on these steps as well.

Selection Sort is another sorting algorithm that is very similar to Bubble Sort. Please implement your version of Selection Sort and analyze its elements after you finish reading about Bubble Sort.

#### Mid-Level Design

Compared to high-level design, which describes the conceptual flow of the algorithms, mid-level design translates the conceptual flow into program-level procedures without discussing the actual code, which is what low-level implementation is about.

**Applying Repetition**:
- Comparing the adjacent elements in the array repeatedly strongly suggests using loop/iteration. 
- According to the description of the steps, the comparison of adjacent elements will be repeated. But even if we finish comparing all adjacent pairs of elements, we only find the smallest element and place it on the leftmost position, which is still far from sorting the whole array. 
- But if we repeat the above process(repetition of repetition), the sorted part of the array grows and the unsorted part of the array shrinks, and eventually the sorted part will be the whole array and the unsorted part will be an empty array. 
- In the process of repetition, we make sure each time we modify the sorted subarray, we only append one element to its end without disrupting the already sorted prefix, an element that is no less than the maximum of the sorted subarray, and that element is also the minimum in the unsorted array. This makes sure all elements in the unsorted subarray are no less than those in the sorted subarray.
- This analysis suggests we need **two layers of nested loop**: the first repetition is the inner loop and the second is the outer loop.
- *Side Note:* when we say an array or subarray is unsorted, it means that we are uncertain about its sortedness. Although it could already sorted, but without iterating through the element we cannot assert its sortedness.

**Mid-Level Design** of Bubble Sort:
- Bubble Sort partitions the entire array into two sections, a sorted subarray followed by an unsorted subarray, and it iteratively increase the size of the former while decreasing that of latter. It has two layers of loops.
- In the outer loop, it handles the unsorted subarray and the size of that subarray decrement by 1 for each iteration. Accordingly, the size of the sorted subarray increment by 1. 
- In the inner loop, it iterates through all adjacent pairs of elements in the unsorted subarray and exchange them if they are in the wrong order. After the loop terminates, the smallest unsorted element will be in its correct final position, which is like "bubbled" to the end like a rising bubble in water.

```java
// Second Approach
LoopForEachUnsortedSubarray() {
    LoopForPairExchange();
}
```

This is the conceptual design of the algorithm. Although it captures the essence of the algorithm, we still need to translate it into actual code by declaring variables, using statement sequence, branching and looping to realize the conceptual idea. Let's go through this example to see how we can methodically implement two layers of loops.

#### Outer Loop Implementation

**Analysis at a Glance**:
- For the outer loop, it's obvious that the Loop Goal is identical to the intent of the whole method, that is to sort the input array $a[]$ in non-decreasing order. 
- The Loop Premise is also the precondition of the method: the array $a[]$ cannot be null.
- Indeed, you could identify that an array of 0 or 1 element doesn't need any operations to make it sorted. They are obviously edge cases for our algorithm, and you may be tempted to write code like the following to take the shortcut to early return:

```java
void bubbleSort(int[] a) {
    if (a == null || a.length <= 1) return;
    // rest of the original code
}
```

- In an actual interview, you should explicitly ask the interviewer whether the input array could be null, but here for the sake of brevity, we assume it's not the method's responsibility to handle null or invalid input, instead the precondition of the method states that the input array can not be null, and it's the caller's responsibility to honor that contract. So we don't consider it as an edge case for now.
- Here the code to handle the edge case `a.length <= 1` is simple, but in general it may takes you several lines of code to handle the edge cases. My recommendation : it's ok to recognize the edge cases before or after you finish the code for the general logic, but **do not rush to implement code to specifically handle edge cases**. The reason is simple: with minor or no tweaks, the code for the general logic can often handle edge cases gracefully.

In the following discussion, we will use $n$ to replace $a.length$ for brevity.

<img src="img\bubblesort_outer_loop.png" alt="bubblesort_outer_loop.png" style="width: 30%; height: 30%; display: block; margin-left: auto; margin-right: auto;">

**Implementation Design**
- The above image is a graphical visualization of what the outer loop does.
- We can see how how the State-Carry Variable, the input array $a[]$ is modified from iteration to iteration. Suppose $n$ is its length.
- The number of iterations is fixed w.r.t the length of the input array: the unsorted part shrinks from the whole array to empty while the sorted part goes through an opposite process. 
- This leads to a natural conclusion that we can use a `for` loop as the outer loop, where the number of iteration required is known and clear. We can use $i$ as the variable name of the Loop Control Variable in this case.

**Definition of the Loop Control Variable**:
- Next we need to determine the meaning/definition of $i$ and its range. 
- This is where things get interesting because we have multiple choices here and although all of them can lead to correct implementation, some are more difficult than the others. 
- **Counting-Based Definition**:
    - You may think that we can count the number of iteration.
    - Suppose the number of iterations is $X$, and in the `for` loop header we can let $i$ go from $1$ to $X$, or equivalently from $0$ to $X-1$, from $X$ to $1$, and from $X-1$ to $0$. You can choose one based on your coding habit and preference. But what is the value of $X$ exactly? 
    - Again, let's go back to the graph. Take a look at the size of the unsorted sub-array(or the sorted subarray because they complement each other). 
    - Initially its size is $n$, for each iteration it decrement by $1$, and eventually it's $0$, which is the case when the loop terminate, so the last valid value should be $1$. From $n$ to $1$, that is $n$ iterations, so $X=n$. But if you take a closer look, we don't have to find the minimum element or exchange any pair of elements when the sub-array has only 1 element, so it's ok to leave the last valid value to be $2$.
    - It' totally normal that you modify your code, for example the `for` loop header, back and forth when writing the loop to change the initial value of the loop index $i$ or its final value. Because before you fully implement the loop body, you only have an abstract overview of what you algorithm does and how it does the job. After you finish it, you can use the techniques of loop analysis to see if there are mistakes and correct them accordingly. That being said, if you are uncertain about the loop range, like the start and end valid value for $i$, leave it there and go on to finish the loop body first and then come back to decide the proper values.
    - However, most of the time, counting the number of iterations and use it as the loop range is not the best choice. It may not be obvious in this case, but in a lot of algorithms, it would make your code harder to write, mainly because it doesn't connect closely enough to the loop variant, what we need to do in the loop body and how the state of the loop evolves.
- **Variant-Based Definition**:
    - Before we go on, take a look at the image again and answer the question: what is the loop variant?
    - It's the size of the unsorted subarray, or equivalently the size of the sorted subarray, but since they complement each other, either of them would work. And this prompts us to consider defining $i$ in terms of the loop variant.
    - You could define $i$ to be the size of the unsorted subarray, so initially it's $n$, and in the end(after the loop terminates) it's $0$. So the loop range could be from $n$ to $1$. But in fact as we have analyzed, in the last iteration we only need to handle an unsorted subarray of 2 elements, so we can also let $i$ go from $n$ to $2$.
    - In this way, the loop control variable is equal to the loop variant: they are identical. As you will see, in the inner loop, we need to examine all pairs of adjacent elements in the unsorted subarray, which is delimited by its start and end indices. Its end index is fixed as $n-1$, and what is the start index for a particular $i$? 
    - You need to use equation to determine the value of the start index. Suppose it's $s$, then $n-1-s+1$, which is the length/size of the unsorted subarray, should be equal to $i$, then we have $s=n-i$. So the start index is $n-i$, which is acceptable and not too complicated.
    - Using this definition would work, but in hindsight we have a better choice. Because loop control variable is not only used in loop variant to determine the number of iterations, it's also used in the loop body to delimit the subarray range that we need to perform work on in the inner loop. We need to strike a balance between these two different requirements, and notice that loop variant is more for correctness analysis, so we can put implementation convenience first when actually writing the code.
- **Index-Based Definition**:
    - My personal pick is the start index of the unsorted subarray.
    - If you take a look at the image, it's most obvious changing element across iteration. The end index of the sorted array is also ok, but there is an offset of 1.
    - Although not being identical to the loop variant, they are directly connected: as $i$ moves toward the end of the array, the unsorted subarray shrinks and its size decreases.
    - In addition, it makes the inner loop easier to write, because inside the outer loop, the range of the unsorted subarray is fixed as $a[i...n-1]$, which is simple and clear and no more composite expression like $n-i$ is required.
    - Although using the number of iterations or the size of the unsorted subarray also works (you can always equations to solve for the start index), they are not as straightforward as directly using the start index. And not being straightforward means it's going to take longer time for the reader to understand and feel convinced, which also means more documentation to explain your code. You may worry about questions like "is it $n-i$ or $n-i-1$?".
- By now we have seen three different definitions of $i$:
    - **Index-Based**: `i = start index`
    - **Variant-Based**: `i = size of unsorted`
    - **Counting-Based**: `i = iteration count`
- The choices of definitions of the loop control variables vary from algorithm to algorithm, and in other algorithms we may have a different set of choices. For example, in binary search, the number of iterations depends on the input array, which is unknown to the programmer when writing the code, so Counting-Based definition is not workable at all. 
- Index-Based definition is a subset of Range-Based definition, and it's often the most intuitive and convenient choice when implementing a loop. But after all, it's a matter of personal preference.

Coding is often a **non-linear mind process**, unlike in many other tasks where you can go top-down all the way without looking back. 
- Picking the start index of the unsorted subarray requires you to know how the loop body will use it, and in this particular case, how the inner loop works. 
- You have to think about the inner loop when implementing the outer loop because they are interlocked and correlated.
- So in general, an overview of the entire loop before you starting writing any code could really help. And if you can't see things clear from the start, it's absolutely ok to think as you code, and go back and forth to modify you code to make the whole loop coherent.

Now we have the framework of the outer loop:
```java
for (int i = 0; i < a.length; i++) {
    // inner loop
}
```

Before we go on, let's summarize what elements of loop we have figured out so far for the outer loop:
- **Loop Goal**: sort $a[0...n-1]$ in non-decreasing order.
- **Loop Premise**: $a[]$ is not `null`.
- **Loop Initialization**: initialize $i$ to $0$ in the `for` loop header.
- **Loop Condition**: the loop continues as long as the unsorted subarray is not empty. 
    - Combined with loop termination and edge case analysis, we see that $i<n$ is a proper expression for loop condition.
- **Loop Termination**: we terminate the loop when unsorted subarray is empty. 
    - So after the loop terminates $i=n$, and the whole array is sorted.
- **Edge Cases**: $n=0$
    - Using the loop condition we picked, no iteration is required and correctness is guaranteed. No additional code is required.
- **Loop Variant**: the size of the unsorted subarray, which is $n-i$. 
    - We decrease it by 1 for each iteration, so `i++` is how we update the loop control variable and make sure the loop will terminate.
- **Loop Control Variable**: $i$ is the loop control variable acting as the partition boundary.
    - It's the start index of the unsorted subarray that we handle in `#i` iteration, while the end index is always $n-1$. 
    - So at the star of an iteration, the sorted subarray is $a[0...i-1]$, while the unsorted subarray is $a[i...n-1]$.
    - Its initial value is 0, which corresponds to the entire array, and we increment it by $1$ for each iteration. 
    - The loop continues until $i$ reaches $n$, ensuring that the value of $i$ covers the range of $[0...n-1]$, making sure each iteration we move only one element from unsorted subarray to sorted subarray.

*Side Note:* using `i < a.length - 1` as the loop condition is also ok. 
- Doing so reduces one unnecessary iteration when the remaining subarray has only 1 element, but makes the elements a little different and complicated:
- **Loop Condition**: the loop continues as long as the unsorted subarray has more than one element, combined with loop termination and edge case analysis, we see that $i<n-1$ is a proper expression for loop condition.
- **Loop Termination**: we terminate the loop when unsorted subarray has less than one element. 
    - When $n \ge 1$, after the loop terminates $i=n-1$, the whole array should be sorted.
    - When $n=0$, after the loop terminates $i=0$, the array is trivially sorted.
- **Edge Cases**: $n=0$ or $n=1$, using the loop condition we picked, no iteration is required and correctness is guaranteed. No additional code is required.

Now let's figure out the rest. 
- Since **Loop Body** is just the code itself, and in our case it's the inner loop because the outer loop is mainly used to delimit the range of the unsorted subarray, so no more analysis is required. 
- **Iteration Goal** is determined by **Loop Invariant**, so the implementation of the loop body eventually falls upon our understanding and definition of the loop invariant. The loop invariant of BubbleSort is not trivial, and you can pause and take some time to think about it. A hint is to take a look at the image.
- Loop invariants are about the unchanging truth of the changing component. For each iteration, the sorted subarray grows by 1, but still stays sorted, which is a truth across all iterations and helpful in sorting the entire array, because eventually the sorted subarray covers the entire array. So it's naturally to think that it's part of the loop invariant.
- But there is more to the loop invariant than simply stating that the sorted subarray grows by one and still sorted. According to the steps of the algorithm, after the inner loop terminates, the first element(the leftmost one) of the unsorted subarray will be the minimum of the unsorted array, which also becomes the last element(the rightmost) of the sorted subarray. For this transformation to work, the following statement must be true: the minimum of the unsorted subarray is no less than the maximum of the sorted subarray.

Now we have both the loop invariant and iteration goal:
- **Loop Invariant**: at the start of `#i` iteration, $a[0...i-1]$ is sorted in non-decreasing order, and MAX($a[0...i-1]$)<=MIN($a[i...n-1]$)`.
- **Iteration Goal**: push MIN($a[i...n-1]$) to $a[i]$ by exchanging adjacent elements, making $a[0...i]$ sorted and $a[0...i-1]$ unchanged.

By now we have all of the elements of the outer loop ready, and we can go on to implement the loop body, which happens to be equivalent to the inner loop. But before we do that, let's take a look **how loop invariant ensures correctness**. This step is actually not necessary in the implementation of the algorithm, but serves as another example of loop analysis

**Loop Invariant Analysis**:
- **Initialization**: initially, $i$ is $0$, so according to the invariant, $a[0...-1]$ is sorted. Notice that this is an empty subarray, so the invariant is trivially true. The first iteration will move the minimum of the whole array to $a[0]$, making MAX($a[0...0]$)<=MIN($a[1...n-1]$) true. 
- **Maintenance**: for all later iterations, since we only move the minimum of the unsorted subarray to the end of the sorted subarray, and that minimum is still no less than the maximum of the sorted subarray, the invariant is maintained.
- **Termination**: after the loop terminates, $i=n$, so the sorted subarray is $a[0...n-1]$, which is the entire array, accomplishing the loop goal.

#### Inner Loop Implementation

<img src="img\bubblesort_inner_loop.png" alt="bubblesort_inner_loop.png" style="width: 40%; height: 40%; display: block; margin-left: auto; margin-right: auto;">

**Implementation Design**
- The above image is a graphical visualization of what the inner loop does. Exchange of adjacent elements are simple.
- The inner loop is relatively straightforward, because from the analysis of the outer loop we already know the range of elements we need to examine and the operation we need to perform.
    - The loop goal of the inner loop is a subset of the iteration goal of the outer loop, without the context of the sorted subarray because the inner loop only handles the unsorted subarray. No particular loop premise, because the subarray won't be empty.
    - The start and end indices of the unsorted subarray is clear: $a[i...n-1]$, so it's natural for us to a `for` loop and choose a loop control variable, say $j$ to cover all possible pairs of elements and perform the swap/exchange if necessary.
    - The loop variant is obvious: the remaining number of pairs of adjacent.
- **Iteration Direction**: 
    - In this case, the **iteration direction** is more important than the other elements and it determines the choice of meaning of $j$. 
    - Since we can only exchange adjacent elements, iterating from the end of the subarray to its start can work but not the opposite direction. 
    - This is because we need to move the minimum element in the subarray to the leftmost position, which could only be done if we iterate from right to left. 
    - This has to do with the loop invariant: if you iterate from left to right, at the `#j` iteration, only $a[j]$ could be exchanged, and all elements in $a[i...j-1]$ won't be touched, so if $a[i]$ is not already the minimum of $a[i...n-1]$, which is totally possible, you can't achieve your loop goal after the loop terminates.
    - On the other hand, iterating from left to right moves the maximum element in the subarray to the rightmost position. So you are implementing the BubbleSort by selecting the maximum for each iteration, you could use this direction.
- **Definition and Range of $j$**:
    - As for the meaning of $j$, we again have many choices.
    - The number of iterations work. You can definitely count the number of iterations, then let $j$ represent the `#0` iteration, `#1` iteration and so on. You could also use the reverse order of counter, from $n-i$ to $1$.
    - You could also directly use the loop variant. The number of elements in the subarray is $n-i$, so there are $n-i-1$ pairs in total.
    - Like the analysis in the outer loop, I prefer what is most obvious in the image: the indices of the pairs of elements to inspect. The two indices in a pairs makes not much difference, but I prefer to choose $j$ as the index of the element with larger index.
    - By doing so, for each iteration the pair we examine is $(a[j-1],a[j])$.
- Once the iteration direction and the meaning of $j$ are settled, most of the other elements are also clear. For the edge cases, the inner loop don't need to worry about empty subarray, but need to handle subarray of only one element.

Now we have figure out the following elements:
- **Loop Goal**: push MIN($a[i...n-1]$) to $a[i]$ by exchanging adjacent elements.
- **Loop Premise**: none.
- **Loop Initialization**: initialize $j$ to $n-1$ in the `for` loop header.
- **Loop Condition**: the loop continues as long as there are more pairs to examine. 
    - Combined with loop termination and edge case analysis, we see that $j>i$ is a proper expression for loop condition.
- **Loop Termination**: we terminate the loop when there are no more pairs to examine. 
    - The last pair to examine is $(a[i],a[i+1])$, and this is when $j=i+1$. 
    - So after the loop terminates $j=i$, and $a[i]$ is the minimum in the unsorted subarray.
- **Edge Cases**: 
    - When the size of the unsorted subarray is 1, no work is required; 
    - When $j=i$, that is when $j$ points to the first element of the unsorted subarray, we don't need to compare $(a[i-1],a[i])$, because $a[i-1]$ is not in the range that inner loop cares about.
    - For both cases, no additional code is required.
- **Loop Variant**: the number of unexamined adjacent pairs in the unsorted subarray, which is $j-i$. 
    - We decrease it by 1 for each iteration, so `j--` is how we update the loop control variable and make sure the loop will terminate.
- **Loop Control Variable**: $j$ is the loop control variable. 
    - We use it to scan all adjacent pairs of elements in the unsorted subarray from right to left, and for the `#j` iteration it points to the pair $(a[j-1],a[j])$. 
    - Its initial value is $n-1$, which corresponds to the last pair $(a[n-2],a[n-1])$, and we decrement it by $1$ for each iteration. 
    - The loop continues until $j$ reaches $i$, ensuring that the value of $j$ covers the range of $[i+1...n-1]$, making sure we examine all pairs and move the minimum to $a[i]$.

Now we have the framework of the inner loop:
```java
for (int i = 0; i < a.length; i++) {
    for (int j = a.length - 1; j > i; j--) {
        // loop body of inner loop
    }
}
```

Now for the rest of the elements:
- The **Loop Body** of BubbleSort's inner loop is in fact very simple: exchange the two adjacent elements if they are in the wrong order.
- The wrong order in this case is the order the violates non-decreasing order. The correct order is $a[j-1] \le a[j]$, so its negation $a[j-1]>a[j]$ is the branching condition for exchange.
- We can use the three-variable exchange method or other methods, e.g. the XOR exchange.
- Then what is the loop invariant? How does it relate to our loop goal? Why does exchanging adjacent elements move the minimum to the leftmost position? These are good questions that you should pause to think about and you can get inspiration from the image.
- The key to understand this is that as we move $j$ from right to left and exchanging pairs in the wrong order, we always keep the minimum of the examined subarray at the leftmost position. 

Now we have both the loop invariant and iteration goal:
- **Iteration Goal**: make sure $a[j-1] \le a[j]$, exchange them if not, and ensure $a[j]$ is the minimum of $a[j...n-1]$.
- **Loop Invariant**: at the start of the `#j` iteration, $a[j]$=MIN($a[j...n-1]$).

**Loop Invariant Analysis**:
- **Initialization**: initially, $j$ is $n-1$, so according to the invariant, $a[j]$=MIN($a[n-1...n-1]$), which is trivially true.
- **Maintenance**: For all later iterations, since we only exchange $a[j-1]$ and $a[j]$ when they are in the wrong order, that is when $a[j-1]>a[j]$, so we have two cases
    - If $a[j-1]>a[j]$, then they will be exchanged, so at the end of the iteration, $a[j-1]$ is the minimum in the unsorted subarray, because now it hold the originally value of $a[j]$. 
    - If $a[j-1] \le a[j]$, no exchange, and since $a[j]$ is already the minimum in the subarray $a[j...n-1]$ and $a[j-1]$ is no greater than it, we can conclude that $a[j-1]$ is the minimum in the subarray $a[j-1...n-1]$. 
    - In both cases, at the start of the next iteration, `j--` has been executed, so the invariant is maintained.
- **Termination**: after the loop terminates, $j=i$, so $a[i]$=MIN($a[i...n-1]$), accomplishing the loop goal.

#### Complete Implementation

Based on our design, below is the complete implementation of Bubble Sort.

```java
void bubbleSort(int[] a) {
    for (int i = 0; i < a.length; i++) {
        for (int j = a.length - 1; j > i; j--) {
            if ( a[j - 1] > a[j]) {
                swap(a, j, j - 1);
            }
        }
    }
}

void swap(int[] a, int i, int j) {
    int tmp = a[i];
    a[i] = a[j];
    a[j] = tmp;
}
```	

If you look closer at the code, you could see how the use of index could be messed up:
- Programmer could use `j = a.length` instead of `j = a.length - 1`, leading to ArrayIndexOutOfBoundException.
- Programmer could use `j>=i` instead of `j>i`, which is fine most of the time, but leads to ArrayIndexOutOfBoundException when $i$ is 0.
- This is why methodical analysis and implementation could help me minimize the chance of bugs.

Table for Loop Element Summary
| Element                 | Outer Loop (i)                                              | Inner Loop (j)                    |
| ----------------------- | ----------------------------------------------------------- | --------------------------------- |
| **Goal**                | Sort the array                                              | Push min element left             |
| **Invariant**           | $a[0..i-1]$ is sorted, MAX($a[0...i-1]$)<=MIN($a[i...n-1]$) | $a[j]$ = MIN($a[j..n-1]$)         |
| **Variant?**            | $n - i$ (size of unsorted)                                  | $j - i$ (remaining comparisons)   |
| **Control Variable**    | $i$ (start of unsorted subarray)                            | $j$ (index of pair right element) |

It's a good idea to document your code with the essential loop elements. How much to include in the documentation is typically a personal preference, but in general loop invariant is a must.

```java
void bubbleSort(int[] a) {
    // Loop Goal: sort a[0...n-1] in non-decreasing order.
    // Loop Control Variable i: the start index of the current unsorted subarray a[i...n-1].
    // Loop Ranges: start=0; end=n-1.
    // Loop Invariant: at the start of #i iteration, a[0...i-1] is sorted and MAX(a[0...i-1])<=MIN(a[i...n-1]),
    // Iteration Goal: push MIN(a[i...n-1]) to a[i], making a[0...i] sorted and a[0...i-1] unchanged.
    // Edge Cases: n=0 or n=1, no iteration will execute.
    for (int i = 0; i < a.length; i++) {
        // Loop Goal: push MIN(a[i...n-1]) to a[i] by exchanging adjacent elements.
        // Loop Control Variable j: current pair to examine is (j-1,j), and we examine all adjacent pairs in a[i...n-1].
        // Loop Ranges: start=n-1; end=i+1, last pair to compare is (i,i+1).
        // Loop Invariant: at the start of #j iteration, a[j]=MIN(a[j...n-1]).
        // Iteration Goal: make sure a[j-1]<=a[j], swap them if not.
        // Edge Cases: when j=n-1, no execution; when j=i+1, the last iteration.
        for (int j = a.length - 1; j > i; j--) {
            if ( a[j - 1] > a[j]) {
                swap(a, j, j -1);
            }
        }
    }
}
```

As complexity analysis is not the focus of this note, we briefly mention it here.
- Bubble Sort is in-place, with constant extra space, so the space complexity is O(1).
- Due to the two layers of loop, the time complexity is O(n^2). The following table summarizes the complexity of comparisons and swaps. The best case corresponds to the case where the input array is already sorted in non-decreasing order, and the worst case is where the input array is sorted but in non-increasing order.

| Metric      | Best Case    | Average Case | Worst Case |
| ----------- | ------------ | ------------ | ---------- |
| Comparisons | $O(n^2)$     | $O(n^2)$     | $O(n^2)$   |
| Swaps       | $O(1)$       | $O(n^2)$     | $O(n^2)$   |
| Overall     | $O(n^2)$     | $O(n^2)$     | $O(n^2)$   |

#### Property-Based Testing

In one of the previous section, I briefly mentioned Property-Based Testing(PBT) and its relation to loop invariant. Now it's a good time to see it in action. On top of the complete implementation of BubbleSort, we can insert method calls that check the properties of the loop, in this case the loop invariant. You can modify the code to see the output to console and you can also try other test cases.

Indeed, we can also insert method calls that check whether the loop variant decrease for each iteration. In BubbleSort it's too straightforward, but in binary search it would be interesting.

```java
void bubbleSort(int[] a) {
    // Loop Invariant: at the start of #i iteration, a[0...i-1] is sorted and MAX(a[0...i-1])<=MIN(a[i...n-1]),
    for (int i = 0; checkOuterInvariant(a, i) && i < a.length - 1; i++) {
        // Loop Invariant: at the start of the #j iteration, a[j]=MIN(a[j...n-1]).
        for (int j = a.length - 1; checkInnerInvariant(a, i, j) && j > i; j--) {
            if (a[j] < a[j - 1]) {
                swap(a, j, j -1);
            }
        }
    }
}

// Since the loop conditions have no side effects, it's also ok to put the check inside and after each loop
void bubbleSort_(int[] a) {
    for (int i = 0; i < a.length - 1; i++) {
        checkOuterInvariant(a, i);
        for (int j = a.length - 1; j > i; j--) {
            checkInnerInvariant(a, i, j);
            if (a[j] < a[j - 1]) {
                swap(a, j, j -1);
            }
        }
        checkInnerInvariant(a, i, i);
    }
    checkOuterInvariant(a, a.length);
}

void swap(int[] a, int i, int j) {
    int tmp = a[i];
    a[i] = a[j];
    a[j] = tmp;
}

boolean checkOuterInvariant(int[] a, int i) {
    if (!isSorted(a, 0, i - 1)) {
        System.out.printf("Outer Loop Invariant Broken, i=%d\n", i);
        System.out.printf("Current array: %s\n", Arrays.toString(a));
        return false;
    }
    return true;
}

boolean checkInnerInvariant(int[] a, int i, int j) {
    if (!isMinimum(a, a[j], j, a.length - 1)) {
        System.out.printf("Inner Loop Invariant Broken, i=%d, j=%d\n", i, j);
        System.out.printf("Current array: %s\n", Arrays.toString(a));
        return false;
    }
    return true;
}

boolean isSorted(int[] a, int low, int high) {
    for (int i = low + 1; i <= high; i++) {
        if (a[i] < a[i - 1]) {
            return false;
        }
    }
    return true;
}

boolean isMinimum(int[] a, int val, int low, int high) {
    for (int i = low; i <= high; i++) {
        if (a[i] < val) {
            return false;
        }
    }
    return true;
}

void bubbleSortTest() {
    int[] a = new int[]{5,2,0,3,9,1,6,3,1,-2,4};
    System.out.printf("Input array: %s\n", Arrays.toString(a));
    bubbleSort(a);
}

bubbleSortTest();
```

#### Define Variables Wisely

- I mentioned that using **Counting-Based** definition for the loop control variable is not a good idea. Here I will show one such example.
- Suppose we want to implement BubbleSort by exchanging and selecting the maximum instead of the minimum as we did, and we choose to use the number of iterations as the loop control variable $i$. We already know in the outer loop there will be $n-1$ iteration, so let's make $i$ go from $0$ to $n-1$.
- So in this case, the relative position of the unsorted subarray and sorted subarray is reversed: the unsorted subarray is on the left and the sorted subarray on the right.
- Now in the inner loop, we need to push the maximum of the unsorted subarray to its rightmost position. What is the range of the unsorted subarray? Its start index is fixed as $0$, but what about the end index?
- We have to setup equations to solve it. When $i=0$, the length of the unsorted subarray is $n$. As $i$ grows, the length of the unsorted subarray decreases, and their relation is $l=n-i$ (because the number of iterations is exactly have many elements in the unsorted subarray have been removed to the sorted subarray). Now suppose the end index of the unsorted subarray is $e$, we have $e-0+1=l=n-i$, then we have $e=n-i-1$.
- This is why I said using the number of iterations is not the best practice. This calculation is not straightforward and error-prone. You definitely want to avoid such complication in a coding interview. The corresponding code is as below. The part `j < a.length - 1 - i` will likely stun the interviewer and make he or she confused.

```java
void bubbleSortMax_(int[] a) {
    for (int i = 0; i < a.length; i++) {
        for (int j = 0; j < a.length - 1 - i; j++) {
            if (a[j + 1] < a[j]) {
                swap(a, j + 1, j);
            }
        }
    }
}
```

- In fact, if we want to select the maximum instead of the minimum in BubbleSort, a better choice of the definition of $i$ is the end index of the unsorted subarray, which is the **Index-Based** definition. In this case we can iterate from $n-1$ to $0$, leading to a much cleaner and easier code.
- We better define the loop index as something directly connected to what changes from iteration to iteration

```java
void bubbleSortMax(int[] a) {
    for (int i = a.length - 1; i >= 0; i--) {
        for (int j = 0; j < i; j++) {
            if (a[j + 1] < a[j]) {
                swap(a, j + 1, j);
            }
        }
    }
}
```

You are encouraged to implement Bubble Sort using other Loop Control Variable definition, for example, the Variant-Based definition and you will gain more insight into the importance of choosing a good definition.

#### Optimization

How to optimize Bubble Sort? 
- Recall that we partition the entire array into a sorted subarray and an unsorted subarray. As part of the outer loop invariant, we are certain that the sorted subarray is indeed sorted. 
- But in fact, during the loop, at some point, the unsorted subarray could also be sorted, because we have done work to fix the pairs in the wrong order, or perhaps it's originally sorted. In our graphical example, the whole array is already sorted when $i=3$, but terminates when $i=5$.
- Recall the second part of the outer loop invariant: MAX($a[0...i-1]$)<=MIN($a[i...n-1]$). This implies that if the unsorted subarray is also sorted, then the entire array is sorted, and we can terminate the algorithm without executing the rest of the loop, which will swap no elements in the unsorted subarray anymore.
- So the key of optimization falls upon whether we can easily identify that the unsorted subarray is already sorted. If you take a look at the method `isSorted()` in the Property-Based Testing section, you will likely notice that it's strikingly similar to the inner loop of Bubble Sort: if there are no pairs are in wrong order, then the entire subarray is sorted.
- Based on the above observation, we can keep track of an additional boolean variable `swapped` which acts as a flag whether any swapping takes place in the inner loop, and if it's false after the inner loop terminates, according to our analysis, we can safely terminate the algorithm.
- Although on average and worst the time complexity is the same as the original version, in the best case where input array is already sorted, the complexity is O(n). In reality it would run faster when the input array is partially sorted.

```java
void bubbleSortOptimized(int[] a) {
    for (int i = 0; i < a.length; i++) {
        boolean swapped = false;
        for (int j = a.length - 1; j > i; j--) {
            if ( a[j - 1] > a[j]) {
                swap(a, j, j -1);
                swapped = true;
            }
        }
        if (!swapped) return;
    }
}
```

### Insertion Sort

#### High-Level Design

Insertion Sort is another classic sorting algorithm, and it shares some similarity with Bubble Sort. We will compare it with Bubble Sort from time to time. For the following discussions, suppose we want to sort an array in non-decreasing order. 

**This section is not a full treatment** about sorting algorithms or even Insertion Sort specifically, we only focus on how to methodically implement it.

**High-Level Design Perspective**:
- Insertion Sort is a simple comparison-based sorting algorithm that builds the sorted array one element at a time. The algorithm works by inserting each new element into an already sorted subarray, whose initial size is 1, until the whole array is sorted.
- **Definition of Correct Insert Position**:
    - How to define the correct insert position for an element?
    - If there are duplicates in the array, there are actually multiple valid insert positions for an element, because the order between equal elements doesn't affect sortedness.
    - The choice we pick is the position that is not only correct, but also requires minimum number of data movement. This makes the desired insert position for an element in an already sorted subarray unique if it could only grow to one direction (e.g. left or right).

<img src="img\best_insert_position.png" alt="best_insert_position.png" style="width: 50%; height: 50%; display: block; margin-left: auto; margin-right: auto;">

- **High-Level Procedure**:
    - Start from the second element and treat the first element as a trivially sorted portion.
    - For each remaining element, insert it into the sorted portion:
        - Compare it with the elements to its left.
        - Shift all elements greater than it one position to the right to make room.
        - Insert the current element into its correct position in the sorted portion.
    - Repeat until all elements have been inserted into their correct positions.
    - According to our analysis of the correct insert position, in our case, it should points to the first element larger than the new element.
- **Analogy**: sorting playing cards
    - A good analogy of Insertion Sort is how you might sort playing cards in your hand. 
    - You take the cards one by one from a pile and insert each card into the correct position among those already sorted in your hand.
    - Before and after you insert the new card in your hand, the cards are sorted.

<img src="img\insertionsort_playing_cards.png" alt="insertionsort_playing_cards.png" style="width: 30%; height: 30%; display: block; margin-left: auto; margin-right: auto;">

- **Variants of Insertion Sort**:
    - There many versions of Insertion Sort, and each version could be implemented in many ways. So don't be surprised when you see an unfamiliar implementation.
    - This version of Insertion Sort works from left to right. Conversely, we can also work from right to left, and the whole process is a mirror version of the above steps. Notice that in this reversed direction, the best insert position points to the first element smaller than the new element.
    - Shifting all elements greater than or equal to the element is also correct but introduce redundant data movement. In addition, doing so breaks the **stability** of the sorting algorithm, so people generally don't consider this option. 
- The below is a graphical representation of the procedure. The grey portion doesn't participate in the later process.

<img src="img\insertionsort_visualization.png" alt="insertionsort_visualization.png" style="width: 30%; height: 30%; display: block; margin-left: auto; margin-right: auto;">

Before you go on reading, please implement Insertion Sort by yourself according to the steps of the algorithm. In addition, also annotate your loop control structure and explain what invariants you’re trying to maintain. From this case study, we will skip verbose details of the thought process of implementing the algorithms.

#### Mid-Level Design

**Applying Repetition**:
- The high-level description of Insertion Sort heavily suggests looping/iteration, just like Bubble Sort.
- We need to perform the insertion operations for each element in the array. Repetition of the same process for each element suggests using loop. 
- By repeating this process, the sorted part of the array grows and the unsorted part of the array shrinks, and eventually the sorted part will be the whole array and the unsorted part will be an empty array. 
- The insertion operation itself also requires us to compare a new element with potentially multiple existing elements in the subarray, and shifting those elements to the right, which is also repetitive and requires loop.
- This analysis concludes that we need **two layers of nested loop**: for each element in the array, and for each insertion into a subarray.

Insertion Sort conceptually partitions the entire array into a sorted subarray followed by an unsorted subarray. The main difference between Insertion Sort and Bubble Sort is that the operation of insertion mainly involves the sorted subarray, as we only access one element in the unsorted subarray at a time, leaving the rest untouched.

By now we are pretty close to the mid-level design of the algorithm: 
- Two nested loops, and the outer loop simply moves the boundary between the sorted and unsorted subarrays. 
- The only lingering question about the design is how exactly do we insert the new element into a sorted subarray in the inner loop? 

**Element Insertion**:
- Unlike "exchange two adjacent elements if they are in the wrong order", the insertion operation could be implemented in more than one way. Although all of them achieve the same goal, there are distinction in terms of efficiency:
- **Adjacent Swapping**: We can reuse the `swap()` function defined in the Bubble Sort to exchange the new element with elements in the sorted subarray, starting from the last/rightmost one, if they are in the wrong order, i.e. left element > right element. The exchange continues until the new element is exchanged to the correct position, and at this point the sorted subarray maintains sorted with one more element.
- **Search while Shifting**: Instead of swapping the new element all the way to the correct position, we can adopt an shifting approach. We save the new element, start from the end of the sorted subarray, and for each element larger than the new element, shift them one position to the right(overriding that position with the value of the element). In the end, we override the value at the insert position with the new element.
- **Search then Shifting**: We can decouple the operations of array element shifting and comparison by first comparing the new element with the elements in the sorted subarray, starting from the last/rightmost one (you can also do this in the opposite direction), until we find the insert position. And then we shift all elements in the range starting from the insert position to the end of the sorted subarray one position to the right. As a final step, we override the value at the insert position with the new element.
- Other approaches of insertion.

**Search while Shifting** and **Search then Shifting** are very similar in nature. For the rest of the section, we will go with **Search while Shifting**, but here we also mention how to implement **Search then Shifting**. 

**Mid-Level Design** of Insertion Sort:
- Insertion Sort partitions the entire array into two sections, a sorted subarray followed by an unsorted subarray, and it iteratively increase the size of the former while decreasing that of the latter. It has two layers of loops.
- In the outer loop, it iterates through all elements in the array, picking the next element to insert.
    - This makes the size of the unsorted subarray decrement by 1, and the size of the sorted subarray increment by 1 for each iteration.
    - Depending on whether we choose to decouple element comparison and shifting or, the loop body could be different.
- If we don't decouple comparison and shifting, then we have one inner loop.
    - The inner loop iterates through elements in the sorted subarray from right to left, compare them with the new element and shift the old element one position to the right if it's greater than the new one. The process continues until an element no less than the new element to be inserted is found or all elements are greater than it. Finally we insert the new element to its correct insert position.
- If we decouple comparison and shifting, then we have two inner loops.
    - In the inner loop of element comparison, it iterates through elements in the sorted subarray from right to left, until an element no less than the new element to be inserted is found or all elements are greater than it. 
    - In the inner loop of element shifting, we shift all elements in the subarray that are greater than the new element one position to their right, and we perform the shifting from right to left to avoid data loss. Finally we insert the new element to its correct insert position.

```java
// Second Approach
LoopForEachElement() {
    LoopForComparisonAndMovement();
}
```
OR
```java
// Third Approach
LoopForEachElement() {
    LoopFroComparison();
    LoopForMovement();
}
```

This design leverages the incremental nature of the algorithm: each new element is placed into an already sorted context, preserving sorted order as we go. Next we translate the mid-level design into low-level code.

#### Outer Loop Implementation

<img src="img\insertionsort_outer_loop.png" alt="insertionsort_outer_loop.png" style="width: 30%; height: 30%; display: block; margin-left: auto; margin-right: auto;">

**Implementation Design**
- The above image is a graphical visualization of what the outer loop does conceptually.
- It's strikingly similar to Bubble Sort in terms of how sorted and unsorted subarrays change from iteration to iteration. 
    - The difference is that in Bubble Sort, modification is done on the unsorted subarray, but in Insertion Sort, it's done on the sorted subarray. 
    - In both cases, one element in the unsorted subarray is incorporated into the sorted subarray.
- We don't go into lengthy discussion like in Bubble Sort anymore. For example, Loop Goal, Premise and Edge Cases are almost identical.
- The `#0` iteration is optional, you can skip it by initializing $i$ as 1.
- You should be able to see why using a `for` loop as the outer loop, and choosing the start index of the unsorted subarray as the definition of the Loop Control Variable $i$, that is using Index-Based definition, are good choices.

In the following discussion, we will use $n$ to replace $a.length$ for brevity.

**Loop Elements** of the outer loop:
- **Loop Goal**: sort $a[0...n-1]$ in non-decreasing order.
- **Loop Premise**: $a[]$ is not `null`.
- **Loop Initialization**: initialize $i$ to $1$ in the `for` loop header.
  - In fact both $0$ and $1$ work. We choose $1$ because a subarray of only one element is trivially sorted, so we don't have to insert it into an empty sorted subarray. Thus the first element we need to insert is $a[1]$.
  - It's ok if you are uncertain about this, and you can just put either value temporarily here and come back to inspect it after you finish the inner loop.
- **Loop Condition**: the loop continues as long as the unsorted subarray is not empty. 
  - Combined with loop termination and edge case analysis, we can use $i<n$ as loop condition.
- **Loop Termination**: we terminate the loop when unsorted subarray is empty. 
  - So after the loop terminates $i=n$, and the whole array is sorted.
- **Iteration Goal**: insert $a[i]$ into $a[0...i-1]$, making $a[0...i]$ sorted.
- **Edge Cases**: $n=0$ or $n=1$
  - Using the loop condition we picked and the initialization choice, no iteration is required and correctness is guaranteed. This also means we don't need additional code to specifically handle them.
- **Loop Variant**: the size of the unsorted subarray, which is $n-i$. 
  - We decrease it by 1 for each iteration, so `i++` is how we update the loop control variable and make sure the loop will terminate.
- **Loop Invariant**: at the start of `#i` iteration, $a[0...i-1]$ is sorted in non-decreasing order.
  - Notice that unlike Bubble Sort, we have no information between the elements in the sorted subarray and the elements in the unsorted array.
  - **Initialization**: the invariant is initially true for the `#1` iteration: $a[0...0]$ is trivially true.
  - **Maintenance**: if the iteration goal is accomplished, the sorted order is maintained, while sorted subarray grows by 1, then we maintain the invariant.
  - **Termination**: after the loop terminates, $i$ would be $n$, which means $a[0...n-1]$, the whole array is sorted and the loop goal is achieved.
- **Loop Control Variable**: $i$ is the loop control variable acting as the partition boundary.
  - It points to the element we need to insert for the current iteration, and it's the start index of the unsorted subarray that we handle in `#i` iteration, while the end index is always $n-1$.
  - So at the star of an iteration, the sorted subarray is $a[0...i-1]$, while the unsorted subarray is $a[i...n-1]$.
  - Its initial value is 1, which corresponds to a sorted subarray of only $1$ element, and we increment it by $1$ for each iteration. 
  - The loop continues until $i$ reaches $n$, ensuring that the value of $i$ covers the range of $[1...n-1]$, making sure each iteration we insert only one element from unsorted subarray to sorted subarray.

According to the above analysis, we have the framework of the outer loop:
```java
void insertionSort(int[] a) {
    for (int i = 1; i < a.length; i++) {
        // inner loop
    }
}
```

#### Inner Loop Implementation

<img src="img\insertionsort_inner_loop.png" alt="insertionsort_inner_loop.png" style="width: 50%; height: 50%; display: block; margin-left: auto; margin-right: auto;">

**Implementation Design**
- The above image is a graphical visualization of what the inner loop does. After the mid-level design, the inner loop is conceptually clear, and we need to do two things at the same time:
    - Locating the correct insert position for the new element, which is an array index.
    - Shifting all larger elements one position to the right, making room for the insertion.
    - It's like the insert position is a gap between the original sorted subarray, and the new element fills that gap. 
- Now we just need to define appropriate variables and translate the procedure into code.
    - Most of the loop elements are straightforward, so we only discuss a few important points.
    - The element we need to insert is $a[i]$, and the start and end indices of the sorted subarray is clear: $a[0...i-1]$. 
    - It makes sense use a loop control variable, say $j$ to scan the subarray, which means we use it to point to element in the subarray to compare it with $a[i]$.
- **Insert Position Cases**: according to our previous discussion of the insert position, since we partition the whole array as the sorted subarray followed by the unsorted subarray, so the sorted subarray could only grow to the left. We can analyze the possibilities of insert position:
    - **Best Case**: $a[i]$ is already at the correct position, in which case $a[i]$ >= MAX($a[0...i-1]$), no data movement is required.
    - **Worst Case**: $a[i]$ needs to be inserted at the start of the subarray, in which case $a[i]$ < MIN($a[0...i-1]$), maximum data movement as all elements in the subarrays needs to be shifted. Notice that if there are elements equal to $a[i]$, it's impossible to be the worst case, because we can append $a[i]$ to the end of the elements equal to it.
    - **Common Case**: $a[i]$ needs to be inserted into somewhere middle of the subarray with some data movement.
- **Edge Cases**: Both bast case and worst case are both .
    - Because the insert positions are at the two ends of the subarray, where things could easily go awry. 
    - As with before, it's a good practice to recognize the edge cases, but don't rush to write code for them, instead let's focus on the common cases first.
- Below is an image summarizing the three cases.

<img src="img\insertionsort_insert_position.png" alt="insertionsort_insert_position.png" style="width: 50%; height: 50%; display: block; margin-left: auto; margin-right: auto;">

- **Temporary Variable**: You should be able to notice that except for the best case, $a[i]$ will always be overridden in the loop.
    - But we need to access the value of $a[i]$ during the loop for comparison. In addition, after the loop terminates, we need to insert it into the correct position, so we can't afford to lose the data store at $a[i]$. 
    - A good way to handle this is to use a temporary variable, say $entry$ to store $a[i]$ during the loop initialization.
- **Iteration Direction**: What is the proper iteration direction? in fact both iteration direction work. But right-to-left is preferred for our approach.
    - The left-to-right direction starting from the first element of the subarray is only suitable for the **Search then Shifting** approach. 
    - For example, in the common case of the above image, if we scan from left to right, both $a[0]$ and $a[1]$ don't involve in shifting, so this kind of drifts from our choice of **Search while Shifting**. 
    - For **Search while Shifting**, we should scan from left to right, starting from the last element in the sorted subarray. In this case, every element that is larger than th new element will be compared and shifted.
    - There is some uncertainties about the insert position for a particular new element, so we cannot say right-to-left is definitely better than left-to-right when we use **Search then Shifting**, at least in terms of the number of comparison.
- **Definition and Range of $j$**: it's obvious that the good definition of $j$ is to the array index of elements to be compared.
    - Like in Bubble Sort, there are other choices, but you should be able to see why use $j$ directly point to the elements to be compared with the new element is the go-to choice. 
    - In addition, doing so makes $j$ directly related to the insert position we want to locate.
    - This means the range of $j$ is $[0...i-1]$, although we don't necessarily need compare all $a[0...i-1]$ with $entry$: as soon as we find an element no greater than $entry$ the loop could stop, which suggests another part of the loop condition besides checking the range of $j$.
    - Let's take a look the above three cases:
        - For the **Best Case**, $j$ will end as $i-1$, so the insert position is $j+1$.
        - For the **Worst Case**, since $a[0]$ is the last element to be compared with $entry$, after that comparison $j$ will be $-1$. This is ok, because the insert position is $0$, which is also $j+1$.
        - For the **Common Case**, $j$ will points to an element no greater than $entry$, and in this case, the loop should terminate, and $entry$ should be inserted at $j+1$.
    - We can see in all three possible cases, after the loop terminates $j+1$ is the correct insert position. So using $j$ in this way is sound.
- **Loop Structure Choice**: Do we use `for` loop or `while` loop? 
    - Since `for` loop and `while` loop could be converted to each other, the choice of the loop structure is not so important. 
    - What is important is to notice that, we need to access the value of $j$ to insert $entry$ after the loop terminates, which means if we use `for` loop, we cannot declare $j$ inside the `for` loop header.
    - In addition, we also need to initialize $entry$ outside of the loop and access it after the loop. These reasons suggests using `while` loop will be more clear and easy to read.

After the above analysis, we have figured out the loop elements of the inner loop.
- **Loop Goal**: insert the element $a[i]$ into the sorted subarray $a[0 .. i-1]$ so that after insertion, $a[0 .. i]$ is sorted.
- **Loop Premise**: the $a[0 .. i-1]$ needs to be sorted.
    - This is guaranteed by the outer loop invariant So we can use it as a prior knowledge throughout the analysis.
- **Loop Initialization**: initialize $j$ as $i-1$ and $entry$ as $a[i]$ before the main loop.
- **Loop Condition**: the loop continues as long as there are more elements to compare, and we haven't found an element no greater than $entry$. 
    - Combined with loop termination and edge case analysis, we see that $j \ge 0$ `AND` $a[j]>entry$ is a proper expression for loop condition.
    - It checks two things: that we haven’t run off the left end of the array ($j \ge 0$ ensures we still have elements to check on the left side) and that the current element $a[j]$ is greater than the key (meaning the key should be inserted to the left of $a[j]$).
    - As long as both conditions hold, the loop continues. This condition embodies the idea "while we haven't reached the beginning and the key is still smaller than the element at $a[j]$, keep shifting." 
- **Loop Termination**: we terminate the loop when there are no more elements to compare or we find an element no greater than $entry$.
    - For the first case, it is the worst case, and $j=-1$.
    - For the second case, we know that $a[j] \le entry$.
    - Either case, after the loop terminates, all elements originally in $a[j+1...i-1]$ before the shifting are larger than $entry$, and they have been already shifted to the correct position. 
    - And according to the above analysis, $j+1$ is the correct insert position. Thus inserting $entry$ to $a[j+1]$ will make $a[0 .. i]$ is sorted. The inner loop’s job is done, and control returns to the outer loop to process the next element.
- **Iteration Goal**: shift $a[j]$ one position to the right if it's greater than $entry$.
    - Inside the loop body, we already know that $a[j]>entry$. Then we know that $j+1$, the temporary gap, is not the insert position for $entry$. 
    - So by moving a larger element out of the way to $a[j+1]$ and decrementing $j$, we continue to consider the next position as a potential candidate for insertion.
- **Edge Cases**: both best case and worst case are edge cases, and we have analyzed them in the previous discussion.
    - Even in edge cases, $j+1$ is still the correct insert position and all involved elements will be correctly shifted, so no additional code is required to handle edge cases specifically.
- **Loop Variant**: the number of unexamined adjacent elements in the sorted subarray, which is $j+1$. 
    - We decrease it by 1 for each iteration, so `j--` is how we update the loop control variable and make sure the loop will terminate.
    - With each iteration, j moves closer to the beginning of the array (toward -1), guaranteeing that the loop will eventually end. Even in the worst case, the loop finishes after $i$ comparisons/shifts.
- **Loop Invariant**: at the start of the `#j` iteration, $entry$ < MIN($a[j+2...i]$); both $a[0...j]$ and $a[j+2...i]$ are sorted, and their concatenation is the original sorted subarray $a[0...i-1]$.
    - Since we are shifting the elements in the loop, we need to note that elements in $a[j+2...i]$ during loop, are actually the elements originally in $a[j+1...i-1]$ before the loop, but are shifted one position to the right.
    - Another way to understand this invariant is that except for the yet-to-be-placed $entry$ and the one-position gap we create by shifting, the order of the other elements is still sorted, exactly as before the loop runs.
    - **Initialization**: the invariant is initially true for the first iteration, which is when $j=i-1$ : $a[j+2...i]$ is actually $a[i+1...i]$ is an empty subarray, and $a[0...i-1]$ is sorted.
    - **Maintenance**: during the loop execution, we know that $a[j]>entry$ according to the loop condition. The loop body will shift $a[j]$ one position to the right, which is $a[j+1]$, making $a[j+1...i]$ sorted, which are the original element of $a[j...i-1]$ in the same order before the loop. This preserves the order property for indices beyond $j$ because the element that was at $a[j]$ was greater than $entry$ and also greater than all elements to its left. At the end of the iteration, $j--$ executes and we we maintain the invariant.
    - **Termination**: After the loop terminates, $j$ could be $-1$ (in the worst case), or $i-1$ (in the best case), or between them. No matter which case, inserting $entry$ to $a[j+1]$ would make $a[0 .. i]$ sorted. 
- **Loop Control Variable**: $j$ is the loop control variable. 
    - We use it to scan elements in the sorted subarray from right to left, and for the `#j` iteration it points to the element $a[j]$. 
    - Its initial value is $i-1$, which corresponds to the element immediately to the left of $a[i]$ in the sorted subarray.
    - The loop moves $j$ leftwards decrementing it by $1$ for each iteration, as long as the element at $a[j]$ is greater than $entry$.
    - The loop continues until $j$ reaches $-1$ or exit when $a[j] \le entry$. This ensures that the value of $j$ covers the range of $[0...i-1]$ if no such element is found, making sure we examine all elements and find the correct insert position.

Now we have the framework of the inner loop as well:

```java
void insertionSort__(int[] a) {
    for (int i = 1; i < a.length; i++) {
        int j = i - 1; // Initialization of j, the first element to the left of a[i]
        int entry = a[i]; // Initialization, saving a[i] to a temporary variable
        while (j >= 0 && a[j] > entry) {
            // shift a[j] one position to the right
            j--; // Loop Control Variable Update, moving to next element
        }
        // insert entry into the correct position pointed by j+1
    }
}
```

#### Complete Implementation

Based on our design, below is the complete implementation of Insertion Sort.

```java
void insertionSort(int[] a) {
    for (int i = 1; i < a.length; i++) {
        int j = i - 1;
        int entry = a[i];
        while (j >= 0 && a[j] > entry) {
            a[j + 1] = a[j];
            j--;
        }
        a[j + 1] = entry;
    }
}
```

- If there are many equal elements, the inner loop will still shift elements that are equal to the key? 
  - Actually, note the condition uses $a[j] > entry$ (strictly greater than). That means if an element is equal to the key, the loop stops. - Insertion Sort as typically implemented above is **stable**, that is it preserves the order of equal elements, because it doesn’t move equal elements past each other. 
  - As an edge consideration, this ensures that the relative order of equal keys remains the same after sorting.
- Finally, it’s worth noting the logical relationship between the inner and outer loops.
    - The outer loop sets up the scenario by identifying which element to insert next and maintaining that all prior elements are sorted
    - The inner loop performs the insertion by rearranging the sorted portion to accommodate the new element. 
    - Each outer iteration uses the inner loop to ensure that the outer loop invariant continues to hold true for an expanded sorted section.
    - Each inner iteration relies on the outer loop invariant as premise to insert the new element.
    - In combination, these loops systematically build up a sorted array.

Table for Loop Element Summary
| Element                 | Outer Loop (i)                   | Inner Loop (j)                                                      |
| ----------------------- | -------------------------------- | ------------------------------------------------------------------- |
| **Goal**                | Sort the array                   | Insert $a[i]$ into the sorted subarray                              |
| **Invariant**           | $a[0...i-1]$ is sorted           | $entry$ < MIN($a[j+2...i]$); $a[0...j]$ and $a[j+2...i]$ are sorted |
| **Variant?**            | $n - i$ (size of unsorted)       | $j+1$ (number of remaining elements)                                |
| **Control Variable**    | $i$ (start of unsorted subarray) | $j$ (index of remaining element)                                    |

Below is the annotated implementation of Insertion Sort that includes documentation comments that describe the purpose and properties of each loop (loop goal, invariant, variant, etc.), aligning with the analysis above. The documentation comments in the code help to bridge the gap between the algorithm’s formal reasoning and its implementation.

```java
void insertionSort(int[] a) {
    // Loop Goal: sort a[0...n-1] in non-decreasing order
    // Loop Control Variable i: the start index of the current unsorted subarray a[i...n-1]
    // Loop Ranges: start=1, a subarray of only one element is trivially sorted; end=n-1
    // Loop Invariant: at the start of #i iteration, a[0...i-1] is sorted
    // Iteration Goal: insert a[i] into a[0...i-1], making a[0...i] sorted
    // Edge Cases: n=0 or n=1, no iteration will execute
    for (int i = 1; i < a.length; i++) {
        // Loop Goal: insert a[i] into a[0...i-1]
        // Loop Control Variable j: points current element a[j] to be compared with entry
        // Loop Ranges: start=i-1; end=0. We scan all elements pairs in a[0...i-1] until the insert position is found
        // Loop Invariant: at the start of #j iteration, entry<MIN(a[j+2...i]); a[0...j] and a[j+2...i] are sorted
        // Iteration Goal: shift a[j] one position to the right if it's greater than entry
        // Edge Cases: if entry<MIN(a[0...i-1]), j would be -1; if entry>=MAX(a[0...i-1]), j would be i-1.
        int j = i - 1; // Initialization of j, the first element to the left of a[i]
        int entry = a[i]; // Initialization of entry, saving a[i] to a temporary variable
        while (j >= 0 && a[j] > entry) {
            a[j + 1] = a[j]; // Shift a[j] one position to the right
            j--; // Loop Control Variable Update, moving to next element
        }
        // Loop Termination: all elements greater than entry has been shifted one position; j+1 points to insert position
        a[j + 1] = entry;  // Insert entry into the correct position
    }
}
```

Brief complexity analysis:
- Insertion Sort is in-place, with constant extra space, so the space complexity is O(1).
- Due to the two layers of loop, the time complexity is O(n^2). The following table summarizes the complexity of comparisons and shifts.
- **Best Case**: If the input array is already sorted, the outer loop will still run $n-1$ times, but the inner loop will do minimal work (each time it finds that the key is larger than or equal to the last sorted element, so no shifts occur). 
- **Worst Case**: If the input array is reverse sorted, the outer loop still runs $n-1$ times, but each iteration will trigger the inner loop to run all the way to the beginning of the sorted portion, making the algorithm do the maximum number of shifts, which is $O(n)$.

| Metric      | Best Case    | Average Case | Worst Case |
| ----------- | ------------ | ------------ | ---------- |
| Comparisons | $O(n)$       | $O(n^2)$     | $O(n^2)$   |
| Shifts      | $O(1)$       | $O(n^2)$     | $O(n^2)$   |
| Overall     | $O(n)$       | $O(n^2)$     | $O(n^2)$   |

#### Alternative Implementation and Optimization

What we have discussed and implemented so far is the **Search while Shifting** approach of insertion. In this section we take a brief look at the other approaches, but we don't delve into the analysis details of them. 

##### Alternative Search while Shifting

You may be tempted to replace the following two lines in our Search while Shifting approach:
```java
a[j + 1] = a[j];
j--;
```
WITH
```java
a[j-- + 1] = a[j];
```
OR
```java
a[j + 1] = a[j--];
```
The above tow lines are not identical:
- `a[j + 1] = a[j-- ]` is equivalent to the original code.
- `a[j-- + 1] = a[j]` is not equivalent to the original code.

If you are confused, you can refer to the previous section of Expression Evaluation Order. But even if one of them is equivalent to the original code, it's not recommended to use it. Yes we have one line of code less, but the readability is significantly reduced and the chances of bug increase. So please, avoid doing optimizations like this.

##### Adjacent Swapping

```java
void insertionSort_AdjacentSwapping(int[] a) {
    for (int i = 1; i < a.length; i++) {
        for (int j = i; j > 0 && a[j] < a[j - 1]; j--) {
            swap(a, j, j - 1);
        }
    }
}
```

- Adjacent Swapping Insertion Sort looks somehow similar to Bubble Sort.
- While easier to think about, swapping can result in three assignments per step (per swap) and tends to be slower in practice than the shifting approaches, which uses one assignment per step.
- The overall complexity are on the same level, but this approach is typically avoided in professional code.

##### Linear Search then Shifting

```java
void insertionSort_SearchThenShifting(int[] a) {
    for (int i = 1; i < a.length; i++) {
        int j = i - 1;
        int entry = a[i];
        while (j >= 0 && entry < a[j])
            j--;
        for (int k = i - 1; k > j; k--)
            a[k + 1] = a[k];
        a[j + 1] = entry;
    }
}
```

- Search then Shifting is similar to Search while Shifting but decouples the search/comparison and shifting processes, so you can actually scan from left to right.
- Using two loops that scan the same range of values $[j+1...i-1]$ seems wasteful, but it can be optimized, which is typically recommended by many modern IDEs:

```java
void insertionSort_SearchThenShifting_(int[] a) {
    for (int i = 1; i < a.length; i++) {
        int j = i - 1;
        int entry = a[i];
        while (j >= 0 && entry < a[j])
            j--;
        if (j != i - 1) {
            // System.arraycopy(src, srcPos, dest, destPos, length)
            System.arraycopy(a, j + 1, a, j + 2, i - j - 1);
            a[j + 1] = entry;
        }
    }
}
```

- The shifting here can be implemented as bulk array copy operation, which is more efficient when the size of the elements shifted is large.
- You may feel the line of `arraycopy()` has poor readability. This is because the portion we need to shift is $a[j+1...i-1]$, and its length is $i-1-(j+1)-1$, which is $i-j-1$.

##### Binary Search then Shifting

If you are acute about searching for insert position in a sorted subarray or if you have done this particular leetcode problem: https://leetcode.com/problems/search-insert-position/description/ .

Then you might have notice that if we decouple searching and shifting, we can actually use binary search to find the insert position, which is $O(\log n)$, much better than the $O(n)$ average case or worst case.

```java
void insertionSort_BinarySearch(int[] a) {
    for (int i = 1; i < a.length; i++) {
        int entry = a[i];
        int k = binarySearchInsertPosition(a, 0, i - 1, entry);
        if (k != i) { // the range of k is [0,i], k=i indicates a[i-1]<=a[i]
            System.arraycopy(a, k, a, k + 1, i - k);
            a[k] = entry;
        }
    }
}

int binarySearchInsertPosition(int[] array, int low, int high, int key) {
    if (key >= array[high]) return high + 1;
    // At least one entry is strictly larger than key
    while (low < high) {
        int mid = low + (high - low) / 2;
        if (key < array[mid]) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }
    return low;
}
```

- We will have a dedicated notebook for binary search, so we don't argue why it's correct here.
- This is a classic optimization of insertion sort for the search phase, reducing from $O(\log n)$ to $O(n)$. 
- But the algorithm still has $O(n^2)$ overall time complexity due to shifting.
- Notice that this optimization doesn't always bring performance gain, because the original algorithm only requires $O(1)$ in the best case. - In fact this optimization is rarely worth it in practice unless:
    - comparisons are much more expensive than moves
    - the array is large and the data is not primitive (e.g., sorting large objects)

### Three-Way Partitioning

TODO

## Recommended Leetcode Problems

Now it's time to test your understanding and mastery of writing looping code. The following leetcode problems are the ones that involve relatively complicated loop statements. Try them and see if you can solve them in a methodical way.

Easy
- [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
- [27. Remove Element](https://leetcode.com/problems/remove-element/)
- [67. Add Binary](https://leetcode.com/problems/add-binary/)
- [228. Summary Ranges](https://leetcode.com/problems/summary-ranges/)
- [925. Long Pressed Name](https://leetcode.com/problems/long-pressed-name/)
- [2200. Find All K-Distant Indices in an Array](https://leetcode.com/problems/find-all-k-distant-indices-in-an-array/)

Medium
- [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
- [31. Next Permutation](https://leetcode.com/problems/next-permutation/)
- [38. Count and Say](https://leetcode.com/problems/count-and-say/)
- [43. Multiply Strings](https://leetcode.com/problems/multiply-strings/)
- [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)
- [80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)
- [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)
- [159. Longest Substring with At Most Two Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)
- [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
- [340. Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)
- [395. Longest Substring with At Least K Repeating Characters](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/)
- [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)
- [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
- [443. String Compression](https://leetcode.com/problems/string-compression/)
- [487. Max Consecutive Ones II](https://leetcode.com/problems/max-consecutive-ones-ii/)
- [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)
- [713. Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)
- [926. Flip String to Monotone Increasing](https://leetcode.com/problems/flip-string-to-monotone-increasing/)
- [1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)
- [1493. Longest Subarray of 1's After Deleting One Element](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/)
- [1838. Frequency of the Most Frequent Element](https://leetcode.com/problems/frequency-of-the-most-frequent-element/)
- [2348. Number of Zero-Filled Subarrays](https://leetcode.com/problems/number-of-zero-filled-subarrays/)

Hard
- [41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/)
- [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

# Recursion

Recursion is when a function calls itself to solve a smaller piece of a larger problem.

Recursion is another form of repetition, but in all of the previous notes, we never discuss when to use loop vs. recursion.

Almost every loop element has a recursive corresponding counter part. Since recursion itself is closely related to algorithms (in fact you rarely use recursion if you are not implementing an algorithm), it deserves its own treatment, a whole notebook dedicated to it.

# Documentation

## Block/Loop Documentation

Checklist for Block Documentation:

| **Element**                 | **Importance**                                             |
| --------------------------- | ---------------------------------------------------------- |
| **Purpose / Intent**        | What is this loop doing logically? (High-level goal)       |
| **Loop Invariant**          | What condition remains true before/after each iteration?   |
| **Key Variables & Roles**   | Clarify index meanings, accumulators, pointers             |
| **Exit Condition**          | When and why does this loop stop?                          |
| **Special Cases**           | Explain corner cases (e.g., early termination, edge cases) |
| **Time Complexity**         | What’s the cost of this loop?                              |
| **Side Effects (Optional)** | If this loop mutates external structures                   |


```java
// ------------------------------------------------------------------
// Loop Purpose:
//   - [What problem is this loop solving?]
//   - [What is the goal at a high level?]
//
// Loop Invariant:
//   - [What condition remains true before and after each iteration?]
//   - [How does this ensure correctness of the algorithm?]
//
// Loop Variant (Progress Measure):
//   - [What value decreases/increases to ensure termination?]
//   - [Why will the loop eventually stop?]
//
// Key Variables & Roles:
//   - [What do 'i', 'left', 'right', 'current', etc. represent?]
//   - [Clarify pointers, accumulators, state trackers, etc.]
//
// Exit Condition:
//   - [Under what condition does the loop terminate?]
//   - [Is there an early return?]
//
// Special Cases / Early Exit:
//   - [Any optimization or shortcuts inside the loop?]
//   - [Edge cases handled during iteration?]
//
// Side Effects:
//   - [Does this loop mutate any external structures?]
//   - [Does it affect global state, I/O, etc.?]
//
// Time Complexity:
//   - [What's the cost of this loop in Big O terms?]
//
// ------------------------------------------------------------------
```

## Function/Method Documentation

Checklist for Method Documentation:
|**Element**        | **Importance**                     |
|------------------ | ---------------------------------- |
|**Description**    | Understand purpose                 |
|**Parameters**     | Know what to pass                  |
|**Return Value**   | Know what to expect back           |
|**Preconditions**  | Ensure correct usage               |
|**Postconditions** | Know what is guaranteed            |
|**Side Effects**   | Avoid unintended external changes  |
|**Exceptions**     | Handle errors properly             |
|**Complexity**     | Algorithm efficiency awareness     |

```java
/**
 * [Brief one-line summary of what the method does.]
 *
 * [Optional: Detailed description explaining the purpose of the method, how it works, 
 * and any important context about why it exists.]
 *
 * <p><b>Preconditions:</b>
 * <ul>
 *   <li>[List all assumptions or required states before calling this method.]
 *   <li>[E.g., "The input array must be sorted."]
 * </ul>
 *
 * <p><b>Postconditions:</b>
 * <ul>
 *   <li>[What guarantees are true after the method finishes.]
 *   <li>[E.g., "Returns the index of target if found, array remains unchanged."]
 * </ul>
 *
 * <p><b>Side Effects:</b>
 * <ul>
 *   <li>[Describe any external changes caused by the method.]
 *   <li>[E.g., "Modifies the internal state of this object."]
 * </ul>
 *
 * <p><b>Parameters:</b>
 * @param param1 [Description of param1, expected values, constraints.]
 * @param param2 [Description of param2, expected values, constraints.]
 *
 * <p><b>Return Value:</b>
 * @return [Describe what is returned, meaning of special cases.]
 *
 * <p><b>Throws:</b>
 * @throws SomeException [Under what condition is this thrown.]
 * @throws AnotherException [Under what condition is this thrown.]
 *
 * <p><b>Time Complexity:</b>
 * O(N log N) [Describe in terms of input size.]
 *
 * <p><b>Space Complexity:</b>
 * O(N) [Optional but useful.]
 */
```

## Reference Materials


|**Resource**|**Link**|
|---|---|
|Javadoc Syntax Overview|https://www.oracle.com/technical-resources/articles/java/javadoc-tool.html|
|Javadoc Comment Specification|https://docs.oracle.com/en/java/javase/11/docs/specs/doc-comment-spec.html|
|Java SE API Docs Example|https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Set.html|
|Javadoc Inline Tags| https://docs.oracle.com/javase/8/docs/technotes/tools/unix/javadoc.html#CHDIAJDJ|
|Javadoc Supported HTML Tags|https://docs.oracle.com/en/java/javase/11/docs/specs/doc-comment-spec.html#supported-html-elements|

# Master Your Arsenal

## Built-In Library & Functions

Below is a list of built-in library and functions that useful in competitive programming. I didn't all of the commonly used methods, because some methods like `add()` in `List` or `Set` are so commonly used, so listing them here seem redundant.

- Objects
    - equals(Object a, Object b)
    - hashCode(Object o)
    - hash(Object... values)

- Arrays
    - sort(int[] a)
    - sort(int[] a, int fromIndex, int toIndex)
    - sort(Object[] a)
    - sort(Object[] a, int fromIndex, int toIndex)
    - sort(T[] a, Comparator`<? super T>` c)
    - sort(T[] a, int fromIndex, int toIndex, Comparator`<? super T>` c)
    - equals(int[] a, int[] a2)
    - equals(Object[] a, Object[] a2)
    - equals(T[] a, T[] a2, Comparator`<? super T>` cmp)
    - fill(int[] a, int val)
    - fill(int[] a, int fromIndex, int toIndex, int val)
    - swap(Object[] x, int a, int b) 
    - copyOf(int[] original, int newLength)
    - copyOf(T[] original, int newLength)
    - copyOfRange(int[] original, int from, int to)
    - asList(T... a)
    - hashCode(int a[])
    - hashCode(Object a[])
    - binarySearch(int[] a, int fromIndex, int toIndex, int key)
	    - If `key` is not in the input array, the return value is `(-(insertion point) - 1)`, we can use `~idx` to get the position.

- Collections
    - sort(List\<T> list)
    - sort(List\<T> list, Comparator`<? super T>` c)
    - reverse(List`<?>` list)
    - rotate(List`<?>` list, int distance)
        - distance<0 is left rotation, distance>0 is right rotation
    - emptySet()
    - emptyList()
    - emptyMap()
    - min(Collection`<? extends T>` coll)
    - max(Collection`<? extends T>` coll)

- List
    - sort(Comparator`<? super E>` c)
    - subList(int fromIndex, int toIndex)
    - indexOf(Object o)
    - lastIndexOf(Object o)
    - of()
	    - 0 to 10 parameters

- Set, HashSet
    - entry(K k, V v)

- SortedSet, NavigableSet, TreeSet
    - subSet(E fromElement, E toElement)
    - headSet(E toElement)
        - toElement is exclusive
    - tailSet(E fromElement)
        - fromElement is inclusive
    - first()
    - last()
    - lower(E e)
    - floor(E e)
    - ceiling(E e)
    - higher(E e)
    - pollFirst()
    - pollLast()
    - iterator()
    - descendingIterator()
    - descendingSet()
    - subSet(E fromElement, boolean fromInclusive, E toElement,   boolean toInclusive)
    - headSet(E toElement, boolean inclusive)
    - tailSet(E fromElement, boolean inclusive)

- Map, HashMap
    - keySet()
    - entrySet()
    - getOrDefault(Object key, V defaultValue)
    - putIfAbsent(K key, V value)
    - computeIfAbsent(K key, Function`<? super K, ? extends V>` mappingFunction)
    - toArray(T[] a)
    - entry(K k, V v)

- SortedMap, NavigableMap, TreeMap
    - subMap(K fromKey, K toKey)
    - headMap(K toKey)
        - toKey is exclusive
    - tailMap(K fromKey)
        - fromKey is inclusive
    - firstKey()
    - lastKey()
    - lowerEntry(K key)
    - lowerKey(K key)
    - floorEntry(K key)
    - floorKey(K key)
    - ceilingEntry(K key)
    - ceilingKey(K key)
    - higherEntry(K key)
    - higherKey(K key)
    - firstEntry()
    - lastEntry()
    - pollFirstEntry()
    - pollLastEntry()
    - descendingMap()
    - descendingKeySet()
    - subMap(K fromKey, boolean fromInclusive, K toKey,   boolean toInclusive)
    - headMap(K toKey, boolean inclusive)
    - tailMap(K fromKey, boolean inclusive)

- Deque
    - addFirst(E e)
    - offerFirst(E e)
    - addLast(E e)
    - offerLast(E e)
    - removeFirst()
        - throws exception when empty
    - removeLast()
        - throws exception when empty
    - pollFirst()
        - returns null when empty
    - pollLast()
        - returns null when empty
    - getFirst()
        - throws exception when empty
    - getLast()
        - throws exception when empty
    - peekFirst()
        - returns null when empty
    - peekLast()
        - returns null when empty

- PriorityQueue
    - add(E e)
    - poll()
    - peek()
    - remove(Object o)
        - Actually not recommended because it's O(n)
    - contains(Object o)

- Integer, Long, Double
    - valueOf(String s)
    - parseInt(String s)
        - ignores lead 0s
    - toBinaryString(int i)

- BigInteger
    - BigInteger(String val)
    - BigInteger(long val)
    - add(BigInteger val)
    - subtract(BigInteger val)
    - multiply(BigInteger val)
    - divide(BigInteger val)
    - abs()
    - negate()
    - mod(BigInteger m)
    - and(BigInteger val)
    - or(BigInteger val)
    - xor(BigInteger val)
    - not()
    - toString(int radix)

- Math
    - sqrt(double a)
    - ceil(double a)
    - floor(double a)
    - pow(double a, double b)

- Iterator
    - hasNext()
    - next()
    - remove()

- String
    - String(char value[])
    - String(char value[], int offset, int count)
    - String(byte[] bytes)
    - String(byte[] bytes, int offset, int length)
    - charAt(int index)
    - valueOf(int i)
    - getBytes()
    - toCharArray()
    - startsWith(String prefix)
    - endsWith(String suffix)
    - indexOf(int ch)
    - lastIndexOf(int ch)
    - indexOf(String str)
    - lastIndexOf(String str)
    - substring(int beginIndex)
    - substring(int beginIndex, int endIndex)
    - split(String regex)
        - beware of meta characters, such as *, ., +, ?, we need to write "\\\\."
        - remember \ is the escape character
    - split(String regex int limit)
        - the parameter llimit is quite tricky, please refer to the original documentation
    - toLowerCase()
    - toUpperCase()
    - trim()

- StringBuilder
    - append(String str)
    - append(char c)
    - append(int i)
    - deleteCharAt(int index)
    - delete(int start, int end)
    - insert(int offset, String str)
    - reverse()

- System
    - arraycopy(Object src,  int  srcPos, Object dest, int destPos, int length)

- Random
    - nextInt()
    - nextInt(int bound)
    - Random(long seed)

## Classic Algorithms & Data Structures

Every Computer Science graduate student is expected to be familiar with the following concepts of algorithms and data structures. I have also included the most relevant leetcode problems for each topic.

- Elementary Data Structure
	- Stack
		- [225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)
		- [155. Min Stack](https://leetcode.com/problems/min-stack/)
		- [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
	- Queue
		- [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)
		- [622. Design Circular Queue](https://leetcode.com/problems/design-circular-queue/)
		- [1670. Design Front Middle Back Queue](https://leetcode.com/problems/design-front-middle-back-queue/)
	- Deque
		- [641. Design Circular Deque](https://leetcode.com/problems/design-circular-deque/)
	- Linked List
		- Singly Linked List
			- [707. Design Linked List](https://leetcode.com/problems/design-linked-list/)
			- [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
			- [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
		- Doubly Linked List
			- [146. LRU Cache](https://leetcode.com/problems/lru-cache/)
			- [460. LFU Cache](https://leetcode.com/problems/lfu-cache/)
- Searching and Selection
	- Sequential Search
		- [1773. Count Items Matching a Rule](https://leetcode.com/problems/count-items-matching-a-rule/)
	- Binary Search
		- [704. Binary Search](https://leetcode.com/problems/binary-search/)
		- [278. First Bad Version](https://leetcode.com/problems/first-bad-version/)
		- [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
	- Quick Selection
		- [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
		- [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)
- Sorting
	- Counting Sort
		- [75. Sort Colors](https://leetcode.com/problems/sort-colors/)
	- Radix Sort
		- [164. Maximum Gap](https://leetcode.com/problems/maximum-gap/)
	- Bucket Sort
		- [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)
		- [1636. Sort Array by Increasing Frequency](https://leetcode.com/problems/sort-array-by-increasing-frequency/)
	- Selection Sort
		- [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
	- Bubble Sort
		- No specific LeetCode problem focuses on bubble sort (rarely tested directly).
	- Insertion Sort
		- Sequential Search or Binary Search
		- [147. Insertion Sort List](https://leetcode.com/problems/insertion-sort-list/)
	- Shell Sort
		- No direct LeetCode problem for Shell sort (not commonly tested).
	- Merge Sort
		- Array and Linked List
		- Iterative and Recursive
		- Out-of-Place or In-Place
		- [148. Sort List](https://leetcode.com/problems/sort-list/)
		- [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
		- [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
	- Quick Sort
		- Lomuto Partition Scheme
		- Hoare Partition Scheme
		- Bentley-McIlroy 3-way Partitioning
		- [75. Sort Colors](https://leetcode.com/problems/sort-colors/)
		- [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
	- Heap Sort
		- [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
		- [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
- Tree
	- Binary Heap
		- [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)
		- [1046. Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)
	- Binary Tree
		- Preorder Traversal
			- [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)
		- Inorder Traversal
			- [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)
		- Postorder Traversal
			- [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)
	- Binary Search Tree
		- Search
			- [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/)
		- Insertion
			- [701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)
		- Deletion
			- [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)
		- Successor and Predecessor
			- [285. Inorder Successor in BST](https://leetcode.com/problems/inorder-successor-in-bst/)
	- Self-Balanced Binary Search Tree(Optional)
- Hash Table
	- Separate Chaining
	- Linear Probing
	- [706. Design HashMap](https://leetcode.com/problems/design-hashmap/)
	- [705. Design HashSet](https://leetcode.com/problems/design-hashset/)
- Elementary Graph Algorithms
	- Depth-First Search
		- [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)
		- [733. Flood Fill](https://leetcode.com/problems/flood-fill/)
	- Breadth-First Search
		- [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
		- [127. Word Ladder](https://leetcode.com/problems/word-ladder/)
	- Cycle Detection
		- [207. Course Schedule](https://leetcode.com/problems/course-schedule/)
		- [261. Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/)
	- Topological Sort
		- [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
		- [269. Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)
- Minimum Spanning Tree Algorithms
	- Prim's Algorithm
		- [1584. Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)
	- Kruskal's Algorithm
		- [1135. Connecting Cities With Minimum Cost](https://leetcode.com/problems/connecting-cities-with-minimum-cost/)
		- [1168. Optimize Water Distribution in a Village](https://leetcode.com/problems/optimize-water-distribution-in-a-village/)
- Shortest Path Algorithms
	- Dijkstra's Algorithm
		- [743. Network Delay Time](https://leetcode.com/problems/network-delay-time/)
	- Naive Bellman-Ford Algorithm
		- [787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)
	- Queue-based Bellman-Ford Algorithm(SPFA)
	- DAG Bellman-Ford Algorithm
	- Floyd-Warshall Algorithm
		- [1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/)
- Trie
	- R-way Trie
		- [208. Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)
		- [211. Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)
	- Ternary Search Trie
- Substring Search
	- KMP Algorithm
		- [28. Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)
	- Boyer-Moore Algorithm
- Algorithm Design Paradigm
	- Brute-Force
		- [1. Two Sum](https://leetcode.com/problems/two-sum/)
	- Backtracking
		- [46. Permutations](https://leetcode.com/problems/permutations/)
		- [39. Combination Sum](https://leetcode.com/problems/combination-sum/)
		- [51. N-Queens](https://leetcode.com/problems/n-queens/)
	- Divide and Conquer
		- [50. Pow(x, n)](https://leetcode.com/problems/powx-n/)
		- [241. Different Ways to Add Parentheses](https://leetcode.com/problems/different-ways-to-add-parentheses/)
	- Dynamic Programming
		- Top-Down Recursion + Memoization
			- [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
		- Botton-Up Iteration + Cache
			- [198. House Robber](https://leetcode.com/problems/house-robber/)
	- Greedy Algorithm
		- [55. Jump Game](https://leetcode.com/problems/jump-game/)
		- [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)
		- [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

I highly recommend that you understand and implement every one of the above algorithms or data structures. They incorporate the most classic algorithm design principles and are good programming practice. There are tons of material talking about these classic topic, and I personally used [Introduction to Algorithms](https://www.amazon.com/Introduction-Algorithms-3rd-MIT-Press/dp/0262033844 "https://www.amazon.com/introduction-algorithms-3rd-mit-press/dp/0262033844") and [Algorithms 4ed](https://www.amazon.com/Algorithms-4th-Robert-Sedgewick/dp/032157351X "https://www.amazon.com/algorithms-4th-robert-sedgewick/dp/032157351x"). 

The following are some advanced topics that are less frequently seen in solving LeetCode problems, but some are still very useful, e.g. Binary Indexed Tree.

- Advanced Topics
	- 2-3 Tree
	- Red-Black Tree
	- AVL Tree
	- Connected Component
		- [323. Number of Connected Components in an Undirected Graph](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)
		- [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)
		- [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/)
	- B Tree
	- B+ Tree
	- Rolling Hash
		- [187. Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences/)
		- [1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/)
		- [2156. Find Substring With Given Hash Value](https://leetcode.com/problems/find-substring-with-given-hash-value/)
	- Interval Tree
		- [715. Range Module](https://leetcode.com/problems/range-module/)
		- [2276. Count Integers in Intervals](https://leetcode.com/problems/count-integers-in-intervals/)
	- Segment Tree
		- [307. Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)
		- [308. Range Sum Query 2D - Mutable](https://leetcode.com/problems/range-sum-query-2d-mutable/)
		- [732. My Calendar III](https://leetcode.com/problems/my-calendar-iii/)
	- Suffix Tree
		- [1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/)
		- [1062. Longest Repeating Substring](https://leetcode.com/problems/longest-repeating-substring/)
	- Suffix Array
		- [1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/)
		- [1062. Longest Repeating Substring](https://leetcode.com/problems/longest-repeating-substring/)
	- Range Minimum Query
	- Hierholzer's Algorithm
		- [332. Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)
		- [753. Cracking the Safe](https://leetcode.com/problems/cracking-the-safe/)
	- Union Find/Disjoint Set
		- [684. Redundant Connection](https://leetcode.com/problems/redundant-connection/)
		- [721. Accounts Merge](https://leetcode.com/problems/accounts-merge/)
		- [990. Satisfiability of Equality Equations](https://leetcode.com/problems/satisfiability-of-equality-equations/)
	- Binary Indexed Tree/Fenwick Tree
		- [315. Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)
		- [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)
		- [1649. Create Sorted Array through Instructions](https://leetcode.com/problems/create-sorted-array-through-instructions/)
	- A* Algorithm
		- [773. Sliding Puzzle](https://leetcode.com/problems/sliding-puzzle/)
	- Skip List
		- [1206. Design Skiplist](https://leetcode.com/problems/design-skiplist/)
	- Treap
	- Spray Tree
	- LSD String Sort
	- MSD String Sort
	- Regular Expression
		- [10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)
		- [44. Wildcard Matching](https://leetcode.com/problems/wildcard-matching/)
	- Network Flow
		- [1349. Maximum Students Taking Exam](https://leetcode.com/problems/maximum-students-taking-exam/)

## Build Your Own Templates

TODO

