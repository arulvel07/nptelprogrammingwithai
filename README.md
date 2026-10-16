# Programming with Generative AI - Complete Revision Notes

**NPTEL lecture coverage:** L01-L14
**Course:** Programming with Generative AI
**Use:** Fast concept review, worked examples, edge cases, and exam-style practice.

These notes follow the lecture order and retain the examples used in the slides. For any question, first decide what the specification says (especially for edge cases), then trace the code exactly.

## Quick map

1. Programming, algorithms, data-driven solutions, and GenAI (L01)
2. REPL, Python arithmetic, bits, numeric types, strings (L02)
3. Decomposition, variables, assignment, names (L03)
4. Functions, input/output, built-ins, f-strings (L04)
5. Function design and conditions (L05)
6. Scope, tracing, refutation, leap years, short-circuiting (L06)
7. Helpers and recursion (L07)
8. Clarifying ambiguity and string operations (L08)
9. Lists, tuples, exceptions, date parsing (L09)
10. Iteration patterns and list operations (L10)
11. While loops, search, sets, dictionaries, testing (L11)
12. Translating Python ideas into C (L12)
13. C arrays, strings, pointers, and memory (L13)
14. Algorithm/data improvements and Python lists (L14)

---

## L01 - What programmers do; why GenAI can code

### Algorithms and data-centric solutions

- A **programmer writes code to solve a problem**. An **algorithm** is a finite sequence of basic instructions. The words algorithm/algebra connect historically to al-Khwarizmi, as the slide's prompt notes.
- An algorithm-centric solution gives explicit steps. For primality, one simple algorithm is:

      Input n
      If n < 2: answer No
      For each x from 2 through n - 1:
          If n is divisible by x: answer No
      Otherwise: answer Yes

  - **18470723 = 4111 * 4493**, so that number is not prime.
  - A better primality check only needs divisors up to the square root of n, but the lecture's first algorithm is useful for understanding what an algorithm is.
- A data-centric solution learns a decision from examples/data. The lecture contrasts explicit prime checking with deciding whether an image contains a cat.
- A follow-on algorithm can reuse a helper: start from n + 1, check integers until one is prime, and return the first such number.

### Critiquing generated code

For the product problem, add 1 to exactly one digit and maximize the product. On [4, 3, 2]:

| Digit increased | Product |
|---|---:|
| 4 becomes 5 | 5 * 3 * 2 = 30 |
| 3 becomes 4 | 4 * 4 * 2 = 32 |
| 2 becomes 3 | 4 * 3 * 3 = 36 |

The slide's sample [4, 3, 2, 3, 4] has maximum product 432; the unique-digit/glue method also happens to produce 432 there. That is coincidence, not proof. On [4, 3, 2], the actual maximum is **36**, while glueing unique digits gives 432, so this input refutes the method.

### GenAI use

GenAI can produce code because models have learned patterns from large amounts of open-source, structured, documented code. Risks covered in the lecture: hallucinated code, hidden assumptions, shallow learning through over-reliance, and licensing questions around training code. A programmer must review and test its output.

For a temperature converter, ask: Is the input numeric? What should happen below absolute zero (-273.15 C)? Should invalid text raise an error? When a task says “find the smallest number in a list,” clarify empty-list behavior and allowed values. The lecture also uses true/false checks to stress that C remains worth learning, IDEs are useful tools rather than a substitute for understanding, and over-reliance on GenAI can create shallow learning. It mentions numeric-representation failures: the Patriot missile incident (28 deaths) and Ariane 5 (about USD 7 billion loss).

**Exam targets:** define algorithm; distinguish algorithm-centric/data-centric approaches; find a counterexample; identify unstated assumptions; explain why generated code needs testing.

---

## L02 - Python REPL, arithmetic, bits, and types

### REPL and operators

The REPL is a playground that repeatedly **Reads, Evaluates, Prints, and Loops**. Use it for small experiments and output-prediction questions.

| Operator | Meaning | Example/point |
|---|---|---|
| * | multiplication | 3 * 4 = 12 |
| / | true division | 3 / 2 = 1.5 (float) |
| // | floor division | 3 // 2 = 1; -5 // 3 = -2 |
| % | remainder | -5 % 3 = 1 |
| ** | exponentiation | 2 ** 3 = 8 |

For nonzero y, quotient and remainder obey:

      x == (x // y) * y + (x % y)

Thus -5 = (-2) * 3 + 1. /0 and %0 raise ZeroDivisionError. In Python, 0 ** 0 evaluates to 1. Also, 3 * 1 // 2 evaluates to int 1, while 3 * 1 / 2 evaluates to float 1.5.

Precedence (high to low): parentheses, **, unary +x/-x, * / // %, + -. Exponentiation groups right-to-left:

      2 ** 3 ** 4 == 2 ** (3 ** 4) == 2 ** 81
      -3 ** 2 == -(3 ** 2) == -9
      (-3) ** 2 == 9

### Binary representation and numeric types

- Computers represent information with bits (0/1), while people commonly count in decimal. Binary counting begins 0, 1, 10, 11, 100, ...; k bits encode 2**k patterns. 2**10 = 1024 is about 1K; 2**20 about 1M; 2**30 about 1G.
- 24 bits per colour allow 2**24 = 16,777,216 colours.
- Python integers have arbitrary precision, limited by memory. A 32-bit signed integer has a finite range; a very large Python integer may not fit it.
- Floats have finite precision. 0.1 is not exactly representable in binary; Python's float approximation is 3602879701896397 / 36028797018963968. Approximate double range is about -1.8e308 to 1.8e308, with smallest positive normal about 2.2e-308. Tests may also use NaN and positive/negative infinity; NaN is not equal to itself, so ordinary equality checks need care.
- Every Python value is an object of some type. int, float, and str are abstract representations of real-world data. A representation can approximate reality (for example, float 0.1 approximates one tenth). In strings, backslash is an escape character: newline is written \n and a literal backslash is written \\.

**Exam targets:** precedence; negative floor division/modulo; bit counts; why a Python int can exceed a fixed-width integer; why float comparisons can surprise you.

---

## L03 - Decomposing tasks, variables, assignment

A complex calculation can be turned into simple steps. For example, calculate (19 + 31) / (11 + 19) with intermediate names:

      numerator = 19 + 31       # 50
      denominator = 11 + 19     # 30
      answer = numerator / denominator  # 1.666666...

The point is to name intermediate results and make the calculation readable.

An assignment variable = expression does two things:

1. Evaluate the expression using the objects currently named by its variables.
2. Bind the left-hand variable name to the resulting object.

      x = 2
      y = x
      x = x + 1
      # x is 3; y is still 2

Variables are labels, not boxes that own values. Python is dynamically typed: x = 12 then x = 12.5 makes x label an int first and a float later. For mutable objects, assignment copies a reference/label relationship, not the list contents.

Swap values with a temporary variable or Python tuple assignment:

      temp = x
      x = y
      y = temp

      # Python also supports this single statement:
      x, y = y, x

Use names like twice_n, total, return_value, temperature_celsius; avoid illegal names like n*2 or opaque names like t/sum when they hide meaning. = assigns; == tests equality.

**Exam targets:** trace variable labels after reassignment; distinguish immutable reassignment from mutation; identify legal/meaningful identifiers; compute a multi-step expression.

---
## L04 - Functions, built-ins, input, and f-strings

A mathematical function f: A -> B accepts inputs from domain A and returns an output in B determined by the input. Real programs also interact with users and can have side effects, so input() is not a mathematical function.

Examples from the slides:

      len('')          # 0
      len('1947')      # 4
      max('1947')      # '9' (largest character lexicographically)
      max('Zero')      # 'r' (case-sensitive character ordering)
      ord('Z')         # 90
      ord('r')         # 114
      ord('z')         # 122

len(1947) is invalid because an integer has no length. max('') raises ValueError because the iterable is empty.

input() always returns a string:

      temp_text = input('Temperature in C: ')  # e.g. '25'
      temp_c = float(temp_text)
      temp_f = 1.8 * temp_c + 32
      print(f'{temp_c} Celsius is equal to {temp_f} Fahrenheit.')

If the program converts the input using float(...), typing 25 produces the float 25.0, so the f-string displays 25.0. If celsius is actually the int 25, formatting {celsius} normally displays 25, while 1.8 * celsius + 32 is a float and displays 77.0. Multiplying the original input string by 1.8 causes a type error. Calls can be nested; evaluate innermost calls first.

**Exam targets:** return type of built-ins; lexicographic max; ord; input conversion; trace nested function calls; output of an f-string.

---

## L05 - Well-designed functions and conditions

### Function design

A well-designed function has a name that expresses its purpose, meaningful parameter names, a docstring that says **what it does** (not merely how), type hints, and doctests. Type hints document intent but Python does not enforce them.

For Celsius-to-Fahrenheit conversion:

      def celsius_to_fahrenheit(celsius: float) -> float:
          """Convert Celsius to Fahrenheit.

          >>> celsius_to_fahrenheit(0)
          32.0
          >>> celsius_to_fahrenheit(100)
          212.0
          """
          return 1.8 * celsius + 32

Tests should include ordinary and boundary cases. AI-suggested types/behavior may help, but verify them against the specification.

### Conditionals and operators

      if condition:
          ...
      elif other_condition:
          ...
      else:
          ...

Conditions commonly use <, >, <=, >=, ==, !=, in, not in, is, is not, and, or, not. is tests identity; == tests value equality. Strings compare lexicographically and case-sensitively. Python allows chained comparisons such as a <= b <= c. Conditions are usually Boolean expressions, but Python also tests truthiness: 0, 0.0, and '' are false; nonempty strings and nonzero numbers are true.

The lecture's median example has a bug:

      def median(a, b, c):
          if a <= b <= c:
              return b
          elif b <= a <= c:
              return a
          else:
              return c

It passes median(1, 2, 3) == 2 and median(9, 3, 6) == 6, but median(7, 8, 0) falls to return c and returns 0; the correct median is 7. A counterexample reveals that the cases are incomplete. A correct approach is to sort the three values and take the middle, or carefully cover every ordering.

**Exam targets:** what a docstring/type hint/doctest contributes; identify the bug in condition ordering; use a counterexample; distinguish is and ==; trace if/elif/else; determine truthiness.

---

## L06 - Scope, control/data flow, refutation, leap years

### Function calls and local scope

A def statement binds a function; it does not execute the function body. When called, each parameter becomes a local name bound to its corresponding argument. Local names are not directly accessible outside that call. A returned object can be passed back to the caller.

When tracing code, track both:

- **Control flow:** which statement executes next (branches, loops, calls).
- **Data flow:** where objects/values are bound, passed, changed, and returned.

### is_multiple and counterexamples

A typical implementation is:

      def is_multiple(n: int, m: int) -> bool:
          if m == 0:
              return False
          return n % m == 0

A bad modification might return n == 0 when m == 0. It makes is_multiple(0, 0) return True, which can conflict with the expected definition. This example reminds us that the zero case must be specified. Refutation workflow: legal arguments -> expected answer -> actual trace/return -> counterexample.

Simplifications worth recognizing:

      if expression:
          return True
      else:
          return False

is equivalent to return expression. After an if branch returns, a following elif may become if, and an unnecessary else can often be removed.

### Month lengths and leap years

      def is_leap(year: int) -> bool:
          return year % 400 == 0 or (year % 4 == 0 and year % 100 != 0)

Thus 1900 is not a leap year; 2000 is. A month-length function first handles invalid months according to the contract, then February, then 30-day months, then the rest:

      def num_days(month: int, is_leap: bool) -> int:
          if not 1 <= month <= 12:
              return 0  # only if this is the chosen contract
          if month == 2:
              return 29 if is_leap else 28
          if month in (4, 6, 9, 11):
              return 30
          return 31

The lecture emphasizes checking special cases first. not (1 <= month <= 12) is equivalent to month < 1 or month > 12. De Morgan equivalences: not (A or B) == (not A) and (not B); not (A and B) == (not A) or (not B).

### Short-circuit evaluation

Python evaluates and/or left-to-right and stops when the result is known. Boolean algebra says A and B is commutative, but evaluation order matters when one side can fail:

      m > 0 and n % m == 0   # safe: modulo runs only if m > 0
      n % m == 0 and m > 0   # may divide by zero first

**Exam targets:** local vs global; parameter binding; m == 0 counterexample; century-year rule; simplify a condition; identify which side of and executes.

---
## L07 - Helper functions and recursion

A helper makes a smaller subproblem reusable. For example, a month-days helper can handle month lengths while the public function first computes whether the year is leap:

      def num_days(month: int, year: int) -> int:
          leap = is_leap(year)
          return num_days_helper(month, leap)

The helper's parameters are arguments supplied by the caller; its return value becomes part of the caller's data flow.

Recursion is a function using itself as its helper. Example: sum the integers 1 through n:

      def sum_to(n: int) -> int:
          if n == 0:                 # base case
              return 0
          return sum_to(n - 1) + n  # recursive case makes progress

Trace: sum_to(4) = sum_to(3) + 4 = sum_to(2) + 3 + 4 = sum_to(1) + 2 + 3 + 4 = 10. The leap-year review asks which statement is false: every leap year is divisible by 4 (true); every year divisible by 4 is a leap year (false, e.g. 1900); 1900 is leap (false); 2000 is leap (true).

A recursive function needs a base case that solves the task directly and a recursive step that moves toward it. If it calls itself without reaching a base case, it recurses indefinitely. Clarify the domain for negative inputs: a function specified only for natural numbers need not have a meaningful negative result. The closed form n * (n + 1) // 2 agrees for nonnegative n, but does not define the same repeated-sum meaning for all negative n.

**Exam targets:** label base and recursive cases; evaluate sum_to(4); find infinite recursion; decide whether a negative input is legal; explain helper call arguments/return.

---

## L08 - Clarifying questions and strings

### Ambiguity before implementation

The num_digits(100) slide shows inconsistent possible outputs (3 or 2). Ask what “digit count” means and whether zeros/leading zeros are included. For a normal integer representation, 100 has 3 digits; a string such as '0100' has 4 characters. The contract decides.

One recursive integer implementation is:

      def num_digits(n: int) -> int:
          if -9 <= n <= 9:
              return 1
          return 1 + num_digits(n // 10)

Trace num_digits(234): 1 + num_digits(23) -> 1 + (1 + num_digits(2)) -> 3. This counts digits in an integer, not formatting characters such as a sign.

### String indexing and methods

For 'Jasminder' (length 9): indices run 0..8, negative indices -9..-1; name[0] == 'J', name[-1] == 'r'. Valid index condition for length n: -n <= i < n. name[1:4] copies characters at indices 1, 2, 3; stop is exclusive.

Strings are immutable:

      s = 'cat'
      s[0] = 'b'  # TypeError
      s = 'bat'   # rebind s to a new string

'at' in 'cat' checks substring membership; 'Nana'.lower() returns 'nana' without changing the original string.

### Counting vowels recursively

The slide's task is explicitly to count **lowercase** vowels:

      def num_vowels(s: str) -> int:
          if s == '':
              return 0
          first = 1 if s[0] in 'aeiou' else 0
          return first + num_vowels(s[1:])

num_vowels('cat') == 1. With this lowercase-only contract, uppercase I in 'India' is not counted; lowercase i and a are, giving 2. A case-insensitive variant should normalize first or test s[0].lower().

**Exam targets:** valid index range; negative indexing; slice stop; immutability error; recursion trace; identify hidden case-sensitivity requirements.

---

## L09 - Lists, tuples, exceptions, date parsing

### From string to list

date.split('/') turns '27/02/2025' into ['27', '02', '2025']. The original string is immutable; the resulting list is mutable. This avoids brittle character indexing and handles both '27/2/2025' and '27/02/2025'.

The slide's date task also asks questions before coding:

- Should the next date preserve zero padding ('28/02/2025') or omit it ('28/2/2025')?
- Should invalid dates such as '01/13/2025', other separators such as '1-3-25', or nonnumeric input such as 'a/b/c' raise an error?
- What should happen on the last day of a month/year?

A parser should convert components with int, validate ranges/calendar rules, and translate expected parse errors to a clear ValueError. Wrong part counts may raise IndexError; invalid numeric components raise ValueError. Catch errors that the contract expects and do not silently turn invalid dates into plausible dates.

Next-date logic: if day < max_day, increment the day; otherwise set day to 1 and increment month; if month was December, set month to 1 and increment year. The slide's example is next('22/10/2008') == '23/10/2008'. Also test month/year rollovers and decide whether output keeps zero padding.

### Lists and tuples

- Lists are mutable sequences, may be heterogeneous, and support indexing, slicing, negative indexing, membership, append, extend, pop, count, and sort.
- Tuples are immutable sequences and can be more efficient than lists for fixed data. Slide example: iisc = (13.0171, 77.5659), len(iisc) == 2, iisc[0] == 13.0171, iisc[-1] == 77.5659; item assignment raises TypeError.
- Sequence unpacking: day, month, year = date.split('/'); swapping: x, y = y, x.
- append(x) adds one item; extend(xs) adds items from an iterable. sort() mutates the list and returns None; sorted(xs) creates a new sorted list.

### Exceptions

      try:
          day, month, year = date.split('/')
          day, month, year = int(day), int(month), int(year)
      except (ValueError, TypeError) as err:
          raise ValueError(f'Illegal date format: {date}') from err

A wrong number of split fields may also require handling unpacking's ValueError (or validating length first). Exception handling should make the public behavior clear.

**Exam targets:** split output type; tuple immutability; unpacking; exception for malformed input; why date[0]/fixed character positions fail with variable-width day/month.

---
## L10 - Iteration patterns and list operations

### Search until success (Pattern 1)

Find the first vowel, returning it or an empty string if none exists:

      def first_vowel(s: str) -> str:
          for c in s:
              if c.lower() in 'aeiou':
                  return c
          return ''

Slide examples: first_vowel('hello') == 'e'; first_vowel('sky') == ''; first_vowel('Act') == 'A'. The slide asks about 'über': the simple membership test uses English letters aeiou, so ü is not treated as a vowel. Specification must define whether accented letters count.

Correct search shape:

      for item in sequence:
          if matches(item):
              return success
      return failure

A common bug is putting return failure inside the loop after each nonmatch. Then the first nonmatching item causes early failure before later items are checked. A for-else clause is different: it runs after normal loop completion only when no break occurred.

### Accumulate (Pattern 2)

Initialize an accumulator before the loop, update it for each item, return it after the loop:

      def prime_product(data: list[int]) -> int:
          product = 1
          for i in data:
              if is_prime(i):
                  product *= i
          return product

The identity for a product is 1; for a sum it is 0. Use result.append(item) to accumulate output items rather than repeatedly doing result = result + [item].

### Primality example and nested loops

      def is_prime(n: int) -> bool:
          if n < 2:
              return False
          for i in range(2, int(n ** 0.5) + 1):
              if n % i == 0:
                  return False
          return True

The +1 includes the integer square root in range's exclusive-stop interval. For example, 4 must test divisor 2; 2 is prime because its range is empty. The lecture also presents a nested-loop for-else form: loop else runs only if no divisor caused break. Searching until success is linear in the worst case; the recursive mystery(data) example slices the list at each call, which can add repeated copying and make it quadratic. The iterative loop visits items without recursively rebuilding tails. The slides compare the recursive mystery(data) function, which checks data[0] % 2 and recurses on data[1:], with an iterative search. For a heterogeneous list such as [0.0, -1, '0'], define “odd integer” carefully and skip non-int entries; the iterative version is more robust and avoids repeated list slicing. A safe version is:

      def has_odd_int(data):
          for item in data:
              if type(item) is int and item % 2 != 0:
                  return True
          return False

List comprehension equivalent for filtering primes:

      primes = [i for i in data if is_prime(i)]

**Exam targets:** identify Pattern 1 vs Pattern 2; find early-return bug; compute product/count; understand loop else; evaluate range boundaries.

---

## L11 - While loops, binary search, sets, dictionaries, tests

### while and translating loops

A while loop repeats as long as its condition is true. State must change so the condition eventually becomes false. When converting while to for, preserve initialization, update, boundary, and return behavior; missing an increment can create an infinite loop. enumerate(data) supplies (index, item) and often removes manual index updates.

The lecture's mystery(n=3) example repeatedly calls input() while n > 0, returns the first nonempty result, decrements n otherwise, and returns '' if all attempts are empty. Trace the default argument and each loop iteration.

### Searching sorted lists

For an ascending sorted list:

1. Linear improvement: stop once target is found or the current item is already too large.
2. Binary search: inspect data[len(data) // 2]. If too small, keep the right half; if too large, keep the left half; otherwise it is found. Requires sorted input. Complexity O(log n) versus O(n) for linear scan.

### Sets, dictionaries, and choosing a structure

The lecture asks whether list[int] can be assumed before using a set. If items are hashable:

      len(set([1, 2, 1]))   # 2 unique values

But set([[1], [1], '3']) raises TypeError: unhashable type: 'list'. Sorting is not a universal alternative: mixed unorderable types such as str and list cannot be compared, and sort() changes the input list.

A dict directly models key-value data. From the slide's school survey example:

      maths = {'III': 306, 'V': 284, 'VIII': 255, 'X': 220}
      maths['VIII']      # 255
      'V' in maths       # True (tests keys)
      284 in maths       # False; in checks keys, not values

Dictionaries are mutable and preserve insertion order in current Python; updating a value does not move its key. For Boolean membership use a set; for key -> value use a dict; for frequency counts use collections.Counter (import Counter from collections). The slides also show frequency(data, target) accumulating matches and indices(data, target) accumulating their positions. In those examples item is target tests identity; item == target tests value equality.

### All-pairs pattern

For each unordered pair once:

      for i in range(len(seq)):
          for j in range(i + 1, len(seq)):
              use(seq[i], seq[j])

This is the “slow finger, fast finger” pattern and does n(n-1)/2 pairs. itertools.combinations(data, 2) produces the same kind of pairs.

### Test-driven development

High-quality tests must be:

1. **Valid:** expected output is correct for each input.
2. **Thorough:** the set of tests is likely to catch a bug.

For is_prime, include values below 2, 2, a small prime, a composite square (4 or 9), and a composite with a factor near the end of the search (e.g. 25). For date/list functions, include empty/invalid inputs, boundaries, and rollover cases.

**Exam targets:** while-loop termination; enumerate; binary-search trace; set/hashability; dict membership; ordering; all-pairs count; classify test suites as valid/thorough.

---

## L12 - Learning C and translating from Python

### Language comparison

Python and C are imperative: statements control execution order, and assignments/operations can have side effects.

| Python | C |
|---|---|
| Dynamically typed; generally interpreted | Statically typed; compiled before execution |
| int arbitrary precision (memory-limited), immutable | int fixed width for a platform (often 32-bit), mutable; overflow can cause bugs |
| Functions can be defined/called at top level | Entry point is int main(); return 0 conventionally indicates success |
| // is floor division | No floor-division operator; // begins a comment |
| Arguments are passed by assignment | Function arguments are passed by value |
| Runtime can discover some type errors | Compiler can detect incompatible types before running |

A compiler warning is not necessarily a compilation failure, but it can point to unintended behavior. Common workflow: compile with gcc, then run executable a.out or a.exe. printf needs stdio.h and correct format strings. C functions declare fixed return/parameter types (the return type may be void). C statements normally end in ;, function bodies need braces. void main is poor style; use int main and return 0. Examples using bool need stdbool.h; INFINITY is provided by math.h.

### Important translation traps

**Chained comparison:** Python 'A' <= c <= 'Z' is valid. C interprets ('A' <= c) <= 'Z'; the first comparison becomes 0 or 1, then is compared with 'Z', so it is almost always true. Correct C:

      return 'A' <= c && c <= 'Z';

**Assignment vs comparison:** if (n = 0) is a valid assignment expression in C, not a comparison. It assigns zero and evaluates false. Use if (n == 0).

**Integer division and function arguments:**

      double inverse(int n) {
          if (n == 0) return INFINITY;
          return 1.0 / n;
      }

Use 1.0 / n; 1 / n performs integer division first and is 0 for n > 1. Python's 1 / n returns a float. The slide's inverse(1001) is approximately 0.000999000999000999 (shown to 18 decimal places).

C integer division truncates toward zero, unlike Python floor division for negative values. C signed overflow is unsafe; never assume a too-large value behaves like Python's arbitrary-precision integer. The slides contrast Python rebinding an immutable integer with C's mutable int stored at a stable address (&x); multiplying a C int in place can overflow. Arguments still pass by value, so the callee's parameter is a copy.

**Exam targets:** static/dynamic; compiled/interpreted; compile-time vs run-time error; = vs ==; fix chained comparison; predict integer division; explain pass-by-value.

---
## L13 - C strings, arrays, pointers, memory

### C array vs Python list

| Python list | C array |
|---|---|
| Can be heterogeneous | Homogeneous element type |
| Resizes automatically | Fixed size unless programmer allocates/replaces storage |
| Built-in len | No stored length operation for a plain array |
| Indices -n through n-1 | Indices 0 through n-1 |
| Higher-level object | Contiguous memory starting at an address |

A declaration such as int values[5] allocates a contiguous array. When an array is passed to a function, its base address/pointer is passed; the whole array is not copied. The function needs a length separately.

### Null-terminated C strings

A C string is a char array ending with \0. The five visible characters in "color" need six array slots:

      char s1[6] = "color";  // c,o,l,o,r,\0

char s1[5] = "color" does not leave room for the required terminator. strlen counts visible characters until \0; standard helpers such as strcmp and strcpy are in string.h. strcmp(a, b) == 0 means strings are equal.

A pointer to a string literal may point at read-only storage; assigning a second pointer (word_lower = word) aliases the same characters, it does not make a copy. Writing through that pointer is unsafe.

### Copying/converting a string

To make a lowercase copy, allocate strlen(word) + 1 characters, process each character, and write final \0. A local array on the stack cannot safely be returned by address; its lifetime ends when the function returns. Heap allocation with malloc can outlive the function, but:

- include stdlib.h;
- check whether malloc returned NULL;
- reserve space for the terminator;
- the caller must eventually call free.

For English ASCII uppercase letters, conversion word[i] + ('a' - 'A') works when 'A' <= word[i] && word[i] <= 'Z'. Do not use C's invalid chained comparison.

**Exam targets:** count null byte; array bounds; string-literal mutability; pointer alias vs copy; returning address of local array; stack vs heap; malloc/free; strcmp result.

---

## L14 - Algorithm/data improvement; Python lists

### Improve the algorithm or data representation

The lecture's spelling example maps US to Indian English forms: color -> colour, favorite -> favourite, analyze -> analyse.

- A long if/elif chain does a linear comparison against each spelling.
- Algorithm-centric improvement: keep a sorted US-word array and a parallel Indian-word array; binary-search the US list for index i, then return the corresponding entry in the Indian list.
- A better representation is a dictionary: to_ind['color'] == 'colour'. A dictionary models key-value pairs directly and avoids parallel-index bookkeeping.
- A data-centric improvement stores word pairs in a CSV file and loads them at startup. The same fixed program can gain new behavior by updating its data file. ML similarly depends on training data as well as its algorithm.

Performance workflow: implement a correct version in Python -> profile it -> locate hot code -> improve the algorithm/data structure -> translate only the hot part to C if still needed -> call it from Python. Do not translate everything before measuring.

### Demystifying Python lists

CPython, a popular Python implementation, is written in C. A simplified list[int] implementation needs to represent an empty list, track length, support indexing, and append values. A Python list behaves as a resizable sequence, while a raw C array has fixed capacity. Conceptually, append needs room for a new reference and storage management when capacity is exhausted; exact CPython internals are implementation details.

**Exam targets:** choose dict vs parallel arrays; distinguish algorithm improvement from data improvement; explain CSV updates; profile-first optimization; explain why list append requires resizable storage.

---

# Exam practice: examples and answer checks

These are representative formats suggested by the lecture slides: objective concepts, output prediction, code tracing, code completion, bug finding, and test-case selection. They are not claims about the exact paper.

## A. Predict the result

1. -5 // 3 and -5 % 3 -> -2 and 1.
2. 2 ** 3 ** 2 -> 512 because it is 2 ** (3 ** 2).
3. -3 ** 2 -> -9; (-3) ** 2 -> 9.
4. len('1947'), max('1947'), max('Zero') -> 4, '9', 'r'.
5. x=2; y=x; x=x+1 -> x is 3, y is 2.
6. For 'Jasminder', s[-1] -> 'r'; valid indices are -9..8.
7. range(2, 6, 2) yields 2, 4; stop is excluded.
8. iisc=(13.0171,77.5659): iisc[-1] -> 77.5659; item assignment raises TypeError.
9. maths={'III':306,'V':284}: 'V' in maths -> True; 284 in maths -> False because in checks keys.
10. a=[3,1]; result=a.sort() -> a becomes [1,3]; result is None.

## B. Trace or debug

1. Why does float(input(...)) * 1.8 work where input(...) * 1.8 fails? The first converts text to numeric float; raw input is str.
2. What is wrong with the median function for (7, 8, 0)? It falls through to return c and returns 0; expected median is 7.
3. Why can n % m fail in is_multiple? If m == 0, remainder is undefined; guard/contract must handle that before evaluating %.
4. Which is safe: m > 0 and n % m == 0, or n % m == 0 and m > 0? The first, because evaluation is left-to-right and short-circuited.
5. sum_to(4) with the recursive rule -> 10; base case n == 0 and recursive call sum_to(n-1).
6. num_digits(234) -> 1 + num_digits(23) -> 1 + 1 + num_digits(2) -> 3.
7. Why is “if item matches: success; else: failure” wrong inside a search loop? It reports failure on the first nonmatch without checking later items.
8. Why does set([[1], [1]]) fail? Lists are mutable and unhashable; set elements must be hashable.
9. Why can sorting mixed values such as ['b', ['a']] fail? The elements do not support a common ordering.
10. What causes an infinite loop in a manual while-to-for conversion? Failing to update the index/state or updating it on the wrong path.

## C. C translation questions

1. Fix return 'A' <= c <= 'Z' in C -> return 'A' <= c && c <= 'Z'.
2. Fix if (n = 0) when equality is intended -> if (n == 0).
3. Why use 1.0 / n in a double function? It forces floating-point division; 1 / n with ints truncates to 0 for n > 1.
4. How many bytes for C string "color"? Six: five letters plus \0.
5. Why not return word_lower when word_lower is a local array? Its storage lifetime ends when the function returns.
6. Why is char *word_lower = word not a copy? Both pointers refer to the same character storage.
7. What should accompany malloc? Check for NULL, allocate length + 1, add terminator, and establish who frees it.

## D. Test-design checklist

For every function, think through:

- minimum and maximum legal values;
- empty and one-element inputs;
- zero and negative numbers;
- duplicate values and all-distinct values;
- exact boundaries (day/month/year rollover, first/last list index);
- uppercase/lowercase and non-ASCII text;
- invalid types/formats and the exception expected;
- whether the function mutates input or returns a new object;
- duplicate handling and output ordering.

Tests must be **valid** and collectively **thorough**. A passing sample is not proof of correctness; deliberately search for a counterexample.

---

# Last-minute memory sheet

- Algorithm = finite instructions; AI can generate code but cannot certify it.
- input() returns str; convert before arithmetic.
- Python // floors; C integer / truncates toward zero.
- Python int is arbitrary precision; C int has finite width.
- Python = binds; == compares. In C, if (n = 0) is still assignment.
- is checks identity; == checks value.
- and/or short-circuit; put guards first.
- String valid indices: -n <= i < n; slice stop is exclusive; strings are immutable.
- Lists mutate; tuples do not. sort() returns None; sorted() returns a new list.
- Pattern 1: search and stop on success; failure after loop. Pattern 2: initialize/update accumulator. All-pairs uses nested loops.
- Binary search requires sorted input. range excludes stop.
- Sets need hashable values; dict in checks keys; Counter counts frequencies.
- Recursion needs a base case plus progress. Loops also need progress.
- Leap year: divisible by 4, except centuries unless divisible by 400.
- C string: char sequence plus final \0; a local array cannot be returned by address.
- Profile Python first; improve algorithm/data structure before translating hot code to C.
- If a requirement is ambiguous, ask/state the rule and test that edge case.
