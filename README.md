# Quick Revision: Programming with Generative AI

**Coverage:** Lectures 1-14 | **Course:** Programming with Generative AI (NPTEL)

A compact pass over the concepts, common traps, and question styles suggested by the lecture slides. The question list is a study guide, not a prediction of exact exam questions.

## 1. Programming and Generative AI

- A **program** is a sequence of instructions that solves a problem. Break a complex task into smaller steps and helper functions.
- An **algorithm-centric** solution uses explicit rules and steps (for example, checking primality). A **data-centric** solution relies on examples/data and a model (for example, recognizing a cat). Better training data can improve a fixed ML algorithm.
- Generative AI can produce useful code because it has learned from large amounts of structured code. It can still hallucinate, misunderstand requirements, or silently assume behavior. Ask about input types, valid ranges, empty inputs, duplicates, case sensitivity, and output formatting.
- Treat generated code as a draft: read it, trace it, test it, and try to **refute** it with a counterexample. A counterexample shows that the expected result and the program's actual result differ.

## 2. Python essentials

### REPL, expressions, and types

The Python REPL repeatedly **reads, evaluates, prints, and loops**. It is useful for checking small expressions.

| Operator | Meaning |
|---|---|
| `*`, `/`, `//`, `%`, `**` | multiply, true divide, floor divide, remainder, exponent |
| `+`, `-` | addition, subtraction |

Precedence (high to low): parentheses, exponentiation, unary `+`/`-`, `* / // %`, then `+ -`. Exponentiation groups from the right: `2 ** 3 ** 2 == 2 ** (3 ** 2) == 512`. Unary minus is applied after exponentiation: `-3 ** 2 == -9`; use `(-3) ** 2` for 9.

- `/` produces a `float`; `//` rounds **down** (toward negative infinity); `%` satisfies `x == (x // y) * y + (x % y)`. For example, `-5 // 3 == -2` and `-5 % 3 == 1`. Division or remainder by zero raises `ZeroDivisionError`.
- Python `int` has arbitrary precision subject to available memory. `float` uses finite binary precision, so many decimal fractions (including `0.1`) are approximate.
- `str` represents text. A backslash introduces an escape sequence. Strings, integers, and floats are immutable: operations create/refer to new objects rather than changing an existing one.
- `input()` returns a `str`. Convert explicitly with `int(...)` or `float(...)` before numeric work. Use an f-string such as `f'{celsius} C'` to insert values into text.

### Names, assignment, functions

- A variable is a **label** for an object. `x = expression` evaluates the right side, then binds the name on the left. `=` assigns; `==` compares values. Python is dynamically typed.
- Reassignment changes a label; it does not mutate an immutable object. For mutable lists, two names can refer to the same list, so mutation through one name is visible through the other.
- Use clear names and comments. A function should have a meaningful signature, a docstring stating **what/why** it does, type hints, and doctests for normal and corner cases. Python does not enforce type hints at runtime.
- Function parameters are local to each call. Arguments are bound to those parameters; a `return` value is how a function sends a result back.
- A mathematical function maps every valid input in its domain to a result determined by that input. Built-ins include `len`, `max`, `min`, `abs`, `ord`, `int`, and `float`. Nested calls evaluate from the inside out. `input()` is not a pure mathematical function because it interacts with the user.

### Conditions and logic

- Comparisons: `<`, `<=`, `>`, `>=`, `==`, `!=`. Strings compare lexicographically and are case-sensitive.
- `in`/`not in` check membership; `is`/`is not` check object identity. Usually use `==` for value equality.
- Boolean operators: `and`, `or`, `not`. `and` and `or` short-circuit left to right. Put safety checks first, e.g. `m != 0 and n % m == 0`.
- De Morgan's laws: `not (A and B) == (not A) or (not B)`; `not (A or B) == (not A) and (not B)`.
- Chained comparisons work: `a <= b <= c`. Use `if`/`elif`/`else` for mutually exclusive cases. A conditional expression is `value_if_true if condition else value_if_false`.
- Define edge behavior before coding. For example, whether zero counts as a multiple of zero is a specification question; avoid dividing by zero and clarify what the client expects.

## 3. Debugging, decomposition, and recursion

- Trace code statement by statement, tracking current values, function calls, local variables, and the path taken through conditions. PythonTutor can help visualize control flow and data flow.
- To refute a buggy function, first choose a legal input, determine the correct expected result from the specification, trace the actual return value, and compare them. Useful edge cases include zero, negative values, empty inputs, endpoints, duplicates, and invalid types.
- A helper function handles a smaller subproblem; the main function combines its result with the rest of the solution.
- A recursive function needs at least one **base case** solved directly and a recursive case that makes progress toward it. Without a reachable base case, recursion does not terminate.
  - Example: `sum_to(n) = 0` when `n == 0`; otherwise `sum_to(n - 1) + n`.
  - Digit counting can reduce the input with `n // 10`; count a one-digit value (including zero) in the base case. Vowel counting can inspect the first character and recurse on `s[1:]`, with the empty string as its base case.
- Recursion is a useful problem-solving pattern, but iteration is often more efficient for sequence processing.

## 4. Strings, lists, tuples, and exceptions

### Strings

- A string is an immutable sequence. For length `n`, valid indices are `-n` through `n - 1`; `-1` is the last character. Slices use an exclusive stop: `s[start:stop:step]`.
- `substring in s` checks for a substring. Common methods include `.lower()`. Decide whether comparisons/counting should be case-sensitive and whether non-ASCII letters count as vowels.

### Lists and tuples

| Type | Key properties |
|---|---|
| `list` | Mutable sequence; can hold mixed types; supports indexing, slicing, membership, and methods such as `.append`, `.extend`, `.pop`, `.count`, `.sort` |
| `tuple` | Immutable sequence; supports indexing, slicing, membership, and unpacking; can be more efficient for fixed data |

- `.append(x)` adds one item; `.extend(items)` adds each item. `.sort()` changes the list in place and returns `None`; `sorted(data)` returns a new sorted list.
- Sequence unpacking assigns corresponding values: `day, month, year = parts`; Python also supports swaps such as `x, y = y, x`.
- Lists are mutable, so aliases share changes. Tuples and strings cannot be changed by item assignment.
- Use `try`/`except` to handle expected errors such as `ValueError` from integer parsing or `IndexError` from missing positions. Raise a clear exception when an input violates the stated format. Do not hide unrelated bugs with a broad, silent catch.

## 5. Iteration, search, and data structures

### Three reusable iteration patterns

1. **Search until success:** inspect each item; return/break on the first match; report failure only after the loop. A `for ... else` block runs only when the loop did not `break`. Putting failure in the loop's `else` branch can incorrectly stop after the first non-match.
2. **Accumulate:** initialize a result, update it for each item, then return it. Examples: a sum, product, count, or result list. Appending to a list is preferable to repeatedly concatenating lists.
3. **All pairs:** nested loops with `i` over the sequence and `j` from `i + 1` onward examine each unordered pair once; this is typically quadratic, `O(n^2)`.

- `range(n)` gives `0` through `n - 1`; `range(m, n)` stops before `n`; `range(m, n, k)` steps by `k`.
- Use `for` when iterating over a known sequence/range. Use `while` when repetition depends on a condition, and ensure each iteration changes state toward termination.
- Linear search is `O(n)`. On an ascending sorted list, binary search checks the middle and discards half the remaining range each step, giving `O(log n)`. It requires sorted data.
- A **set** stores distinct hashable items; `len(set(data))` counts unique values. Mutable lists are unhashable and cannot be set elements.
- A **dict** stores key-value pairs; membership checks keys, `.items()` yields pairs, and assigning an existing key updates its value. Dictionaries preserve insertion order in modern Python. Use a set for membership/uniqueness, a dict for key-to-value mapping, and `collections.Counter` for frequencies.
- A list comprehension builds a filtered/transformed list, e.g. `[x for x in data if condition]`. `itertools.combinations(data, 2)` is useful for pairs.

### Testing

High-quality tests must each be **valid** (the expected answer is correct) and the set must be **thorough** enough to expose likely bugs. Cover ordinary cases plus boundaries and corner cases: empty/one-item sequences, zero, negative values, duplicates, first/last positions, case differences, malformed input, and special calendar rules. Leap years are divisible by 4, except century years must also be divisible by 400: 1900 is not a leap year; 2000 is.

## 6. Python and C: high-yield differences

| Python | C |
|---|---|
| Dynamically typed; commonly run by an interpreter | Statically typed; compiled (for example, with `gcc`) |
| `int` is arbitrary precision (memory-limited); immutable | `int` has fixed implementation-defined width (often 32 bits); mutable and can overflow |
| `//` is floor division | No `//` operator; `int / int` truncates toward zero |
| Top-level statements can run directly | Execution normally starts at `main`; return `0` conventionally signals success |
| Arguments are passed by assignment/object reference semantics | Arguments are passed by value; a parameter receives a copy of the value |
| Strings/lists are high-level objects | Arrays are fixed-size, contiguous, homogeneous storage; an array argument is handled via its base address/pointer |

- C requires declarations and matching types. `if (n = 0)` assigns; `if (n == 0)` compares. Assignment in a condition may compile while doing the wrong thing.
- C uses `{}` for function bodies; `#include <stdio.h>` provides I/O declarations. `printf` needs correct format specifiers and does not add a newline automatically. Compiler warnings can reveal unintended behavior.
- A C string is a `char` array terminated by `\0`; a visible string of length `n` needs at least `n + 1` bytes. C arrays do not automatically resize and use indices `0` to `n - 1`. `strlen` finds a string's length; `strcmp` returns 0 when strings match.
- Do not return the address of a local C array: its storage expires when the function returns. Heap memory from `malloc` must be checked for failure and later released with `free`. A pointer assignment can alias the original string instead of copying it.
- Practical performance approach: first write a correct Python version, profile it, find hot code, and translate only that part to C if needed. For string processing, Python is usually simpler and safer.

## 7. Likely NPTEL question styles from these lectures

Prepare for objective questions, code tracing, and short code-completion/debugging tasks along these lines:

1. **Predict an expression/output:** precedence, right-associative `**`, negative `//`/`%`, string indices/slices, reassignment, f-strings, or `range` endpoints.
2. **Trace a function:** identify local values and return values through `if`/`elif`, nested calls, loops, or recursion; find a base case or a non-terminating loop.
3. **Find the bug or choose a counterexample:** zero divisors, empty strings/lists, off-by-one bounds, bad indentation, using `is` instead of `==`, or confusing loop `else` with `if` `else`.
4. **Complete or select a function:** prime test, digit/vowel count, leap-year/month-days, searching, counting, or list transformation. State assumptions and handle boundary cases.
5. **Choose a data structure or iteration pattern:** list vs tuple, set vs dict vs `Counter`, search-until-success vs accumulate vs all-pairs, or binary vs linear search.
6. **Evaluate test quality or clarify a requirement:** decide if a test is valid, add a missing corner case, or ask what output is expected for ambiguous/invalid inputs.
7. **Compare Python and C:** compile-time vs run-time errors, dynamic vs static typing, `=` vs `==`, integer division/overflow, pass-by-value, arrays, pointers, and null-terminated strings.
8. **Explain a design choice:** algorithm-centric vs data-centric improvement, why a dictionary beats a long chain of comparisons, or why code should be profiled before translating it to C.

### Last-minute trap checklist

- `input()` is text until converted.
- `-5 // 3` is `-2`; `-5 % 3` is `1`.
- `==` compares values; `is` compares identity.
- `and` can protect a later expression only when the guard comes first.
- A function's body runs when called, not when `def` is executed; parameters are local.
- Recursion needs a base case and progress; a loop needs progress too.
- Search failure belongs after the loop, not after each unsuccessful item.
- `.sort()` returns `None`; use `sorted(...)` for a new list.
- `set` elements must be hashable; mixed values may not be sortable.
- A C string needs its final `\0`; array capacity must include that byte.
- In C, `int / int` truncates toward zero; integer overflow is possible.
- Never assume an ambiguous edge case: clarify it or state the chosen rule.
