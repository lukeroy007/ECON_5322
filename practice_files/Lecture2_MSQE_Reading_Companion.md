# Lecture 2 · Python foundations for quantitative economics
## From recognizing code to reasoning independently

This is a companion to **Lecture2_InClass.ipynb**, inspected directly from the attachment to *Python for Economists*. The source is primarily an in-class scaffold with blank code cells. This version preserves its stated concepts and exercises; worked demonstrations below are newly supplied, not reconstructions of missing instructor code.

The intellectual upgrade is to connect Python semantics to economic measurement: **What object exists? What type is it? What does its value measure? Which transformation is economically meaningful?** A program can execute successfully and still answer the wrong economic question.

By the end you should be able to trace names and values, distinguish display from return values, inspect unfamiliar objects, translate equations into expressions, clean and format text, and express economic eligibility rules precisely. You will apply those skills to a small wage-analysis report.

### How to work through this notebook

Work in three sittings of roughly 40–60 minutes: sections 1–3; sections 4–6; section 7 and the challenge. Take longer where your predictions differ from Python's behavior.

1. **Predict:** before executing an example, write the result, its type, and your reason in a note.
2. **Run and explain:** reconcile discrepancies; do not just memorize the output.
3. **Close the example:** attempt the related blank cell for 5–10 minutes. Documentation is allowed. Copying a finished solution is not the exercise.
4. **Escalate gradually:** concept hint → pseudocode → syntax hint. Open one hint at a time, then try again. If still stuck, share your attempt and request a partial scaffold. Full solutions are deliberately omitted.
5. **Retrieve:** later, rebuild one exercise without viewing your earlier code and change an input to test whether you understand it.

Keep a short record: exercise ID, first prediction, error, repair, strongest hint used, and whether you reproduced it later. Independence means explaining and transferring a solution, not avoiding all reference material.

**Scope:** ordinary Python 3, using the standard `math` module. No loops, custom functions, pandas, or regressions to fit yet. A short optional NumPy bridge is shown as text. Square brackets introduce only the small lists needed for `all()` and `any()`. All economic numbers are synthetic teaching inputs, not current statistics or personal financial advice.

### Connection to the original lecture

| Source topic | Here | Economic purpose |
|---|---|---|
| Assignment and comments | 1 | Track scenarios and document units |
| Built-in functions, help, input | 2 | Read inputs and inspect outputs |
| Types and methods | 3 | Separate numerical values from labels |
| Modules, packages, aliases | 4 | Access mathematical tools explicitly |
| Numerical operations | 5 | Growth, compounding, units, rounding |
| Strings and formatting | 6 | Clean labels and communicate results |
| Comparisons, Boolean operators, all/any | 7 | Express valid economic restrictions |
| Greeting and gross pay | E2.1, E2.2 | Preserve the original practice tasks |

Intentional broken examples appear in Markdown fences so **Run All** will not stop at a planted error. The two `input()` exercises will prompt only after you write and run your answers. Exercise cells are genuinely blank.


## 1 · Assignment is a change in program state

In `price = 12.50`, Python evaluates the right-hand side, then binds the name `price` to the resulting object. Assignment is not an algebraic equation that remains true forever. A name does not carry a live spreadsheet formula.

For the immutable numerical objects used here, rebinding one name does not update values previously computed from it. Later, mutable collections will introduce another issue: multiple names can refer to an object whose contents can change. Keep that distinction in reserve.

**Read R1 before running:** What are the final values of `demo_price`, `demo_quantity`, and `demo_revenue`? Which line would you need to execute again to update revenue?


```python
demo_price = 10.0
demo_quantity = 50
demo_revenue = demo_price * demo_quantity
demo_price = 12.0
print(demo_price, demo_quantity, demo_revenue)
```


Jupyter remembers names in the current kernel, even after you delete the cell that created them. Executing cells out of order can therefore conceal a missing input. Restarting the kernel clears that state. A reproducible notebook creates everything it needs in a sensible top-to-bottom order.

A `NameError` means a name cannot be resolved in the current context. It does not necessarily mean the intended value was economically wrong. Capitalization matters: `GDP` and `gdp` are different names.

Comments begin with `#` outside a string and continue to the end of the line. Useful comments document **units, assumptions, or reasoning**, rather than narrating obvious syntax: `# Annual rate as a decimal; 0.05 means 5%.`


### E1 · Revenue and a counterfactual

Preserve the source exercise: assign `price = 12.50` and `quantity = 80`, then calculate `revenue`.

Extend it: save baseline revenue, raise the price by 8%, and assume quantity falls by 5%. Calculate counterfactual revenue and its percentage change from baseline. Keep the baseline available for comparison. Add comments with units.

**Checks:** baseline revenue is $1,000; counterfactual revenue exceeds baseline but rises by less than 8%. Explain why adding 8% and −5% is not the exact revenue growth rate.

**Write from a blank cell.** First state your inputs, their types and units, and your intended output.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept — open only after an attempt</summary>

A percentage change acts on a baseline through a multiplier. Store the baseline before changing scenario inputs.

</details>


<details>
<summary>Hint 2 — Pseudocode — open only after an attempt</summary>

Calculate baseline revenue → calculate new price and quantity → multiply them → divide the change in revenue by baseline revenue.

</details>


<details>
<summary>Hint 3 — Syntax — open only after an attempt</summary>

For a decimal change `g`, the multiplier is `(1 + g)`. Percentage change is `100 * (new_value / old_value - 1)`. Use distinct names for the two scenarios.

</details>


### D1 · A stale calculation

This report is intended to show interest after the rate changes. It runs, but is it correct?

```python
principal = 1000
rate = 0.05
interest = principal * rate
rate = 0.06
print(f"Interest at the new rate: {interest}")
```

Before repairing: identify the first failing line, or the first line with the wrong meaning. Predict the error or incorrect result. Then make the smallest justified repair and explain why it works. Copy the snippet into the blank cell to investigate.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept</summary>

Assignment stores a computed value; it does not install a dependency.

</details>


<details>
<summary>Hint 2 — Pseudocode</summary>

Trace rate and interest after every line. Locate the point where interest must be recomputed.

</details>


<details>
<summary>Hint 3 — Syntax</summary>

Re-evaluate the expression that defines `interest` after changing `rate`.

</details>


## 2 · Functions: inputs, return values, and side effects

A call such as `len("GDP")` evaluates an operation with arguments. A **return value** can be assigned or used in another expression. A **side effect** changes something outside that returned value: `print()` displays text, for example, but returns `None`.

`None` represents the absence of a substantive value; it is not zero and it is not the string `"None"`. Jupyter may display a cell's final expression automatically. That display is distinct from explicitly calling `print()`.

**Read R2:** Predict both what appears on screen and what each name refers to. Why is `demo_message` unsuitable for subsequent arithmetic?


```python
demo_label = "GDP"
demo_count = len(demo_label)
demo_message = print(demo_label)
print(demo_count, type(demo_count))
print(demo_message, type(demo_message))
```


### Learn how to ask the object a question

Use `type(x)` to identify the kind of value you hold; use `help(len)` or `help(print)` to inspect an operation. Notice that `help(len)` passes the function itself, while `len("GDP")` calls it. Jupyter also supports `print?` for help; that is notebook-specific syntax rather than ordinary Python.

Try `help(len)` in the blank cell. Explain why `len("2026")` is valid but `len(2026)` is not. The number of characters in a label is different from the magnitude of a number.


*Blank notebook cell — write your own code here.*


`input()` always returns text, even if the user types digits. `float("3.5")` parses numerical text; `float("three")` raises `ValueError`. Converting a type and validating an economic assumption are different operations: a negative hourly wage can be a valid float but an invalid input for your application.


### E2.1 · Greeting and sample label

Recreate the original greeting exercise using `input()`. Ask for a name and display `Hello Husky` when the entered name is `Husky`.

Then ask for a country label and display its character count. Predict what leading and trailing spaces do to that count; you will learn to remove those spaces in section 3.

**Write from a blank cell.** First state your inputs, their types and units, and your intended output.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept — open only after an attempt</summary>

User input is already a string; no numerical conversion is required.

</details>


<details>
<summary>Hint 2 — Pseudocode — open only after an attempt</summary>

Read the name → greet the user → read the country label → measure its length.

</details>


<details>
<summary>Hint 3 — Syntax — open only after an attempt</summary>

Use `input("Prompt: ")`, `print(...)`, and `len(...)`. String concatenation uses `+`; include the space you want displayed.

</details>


### E2.2 · Gross pay

Ask for hours worked and an hourly rate; convert them to numbers and calculate gross pay. Assume all hours receive the same rate, with no overtime, deductions, or taxes.

**Checks:** 20 hours at $3.50 yields $70; 37.5 hours at $24 yields $900; zero hours yields zero pay. Do not truncate fractional hours. Write down what happens if someone enters `twenty`.

**Write from a blank cell.** First state your inputs, their types and units, and your intended output.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept — open only after an attempt</summary>

Separate input acquisition, conversion, calculation, and presentation.

</details>


<details>
<summary>Hint 2 — Pseudocode — open only after an attempt</summary>

Read hours and rate as text → parse each as a float → multiply → display the numeric result.

</details>


<details>
<summary>Hint 3 — Syntax — open only after an attempt</summary>

Use `float(raw_text)`. Assign the multiplication result itself to the pay variable; use `print()` separately.

</details>


### D2 · A report that loses its result

Find two different problems. Repairing the first should expose the second.

```python
hours = "20"
hourly_rate = "3.5"
gross_pay = print(hours * hourly_rate)
next_week_pay = gross_pay + 10
```

Before repairing: identify the first failing line, or the first line with the wrong meaning. Predict the error or incorrect result. Then make the smallest justified repair and explain why it works. Copy the snippet into the blank cell to investigate.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept</summary>

First inspect the operands of multiplication; then inspect what `print` returns.

</details>


<details>
<summary>Hint 2 — Pseudocode</summary>

Convert input text → store numerical pay → display pay → use the stored number in later arithmetic.

</details>


<details>
<summary>Hint 3 — Syntax</summary>

Use `float(...)` for both inputs, and keep assignment of a numerical expression separate from its display.

</details>


## 3 · Types, methods, and economic meaning

Objects have types; names can be rebound to objects of different types. Python is dynamically typed, but it does not make arbitrary incompatible operations meaningful.

| Value | Python type | Possible meaning |
|---|---|---|
| `80` | `int` | Count of units sold |
| `12.50` | `float` | Price in dollars per unit |
| `"080"` | `str` | An identifier with significant leading zeros |
| `True` | `bool` | A restriction is satisfied |
| `None` | `NoneType` | No substantive return value |

Python types do not encode economic units: dollars and thousands of dollars can both be floats. Preserve units through names, comments, and explicit conversion.

A **method** is an operation accessed through an object, such as `label.strip()`. Attribute lookup via `.` also accesses non-callable attributes, so not everything after a dot is a method. A method call includes parentheses.

Strings are immutable. `strip()` and `upper()` return strings rather than altering the original string. Some methods on mutable objects behave differently; inspect documentation instead of assuming a universal rule.

**Read R3:** What remains in `demo_raw_country`? What does each successive operation do? Why might `"00123"` need to remain a string?


```python
demo_raw_country = "  usa  "
demo_country = demo_raw_country.strip().upper()
print(repr(demo_raw_country), repr(demo_country))
print(type(demo_country), len(demo_country))
```


`repr(x)` produces a representation useful for inspection; for strings it makes surrounding quotes and escape sequences visible. `str(x)` produces a readable text representation. `int(3.9)` truncates toward zero; it does not round to the nearest integer. `int("3.9")` fails because that string is not an integer literal.

In Jupyter, type an object's name followed by `.` and request completion with Tab; behavior can vary by notebook interface. `help(str.strip)` is a direct alternative.


### E3 · Clean a tiny data record

Start with `raw_country = "  uSa "`, `raw_year = "2026"`, `raw_gdp_billions = "27500.5"`, and `raw_region_code = "007"`.

Create a clean country label, an integer year, and GDP in dollars as a float. Preserve the original region code as text. Inspect the resulting types and explain each choice. Do not overwrite the raw values.

**Checks:** cleaned country has three characters; GDP is larger by a factor of one billion; the region code still has three characters.

**Write from a blank cell.** First state your inputs, their types and units, and your intended output.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept — open only after an attempt</summary>

Meaning determines the desired type. An identifier is not a quantity just because it contains digits.

</details>


<details>
<summary>Hint 2 — Pseudocode — open only after an attempt</summary>

Strip and standardize the country → parse year and GDP → convert GDP units → preserve the identifier.

</details>


<details>
<summary>Hint 3 — Syntax — open only after an attempt</summary>

Use `.strip().upper()`, `int(...)`, `float(...)`, and `10**9`.

</details>


### D3 · A cleaning operation that appears to do nothing

The intended clean label is USA. Explain the result and repair it.

```python
country = " usa "
country.strip().upper()
print(country == "USA")
```

Before repairing: identify the first failing line, or the first line with the wrong meaning. Predict the error or incorrect result. Then make the smallest justified repair and explain why it works. Copy the snippet into the blank cell to investigate.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept</summary>

A string method returns a value. Where does that returned value go here?

</details>


<details>
<summary>Hint 2 — Pseudocode</summary>

Save the result of cleaning, then compare the saved result.

</details>


<details>
<summary>Hint 3 — Syntax</summary>

Assign the method chain to a name, using either a new name or an intentional reassignment.

</details>


## 4 · Imports create access to a namespace

A module organizes Python definitions; a package organizes modules and related contents. Importing makes a module or selected names available in your program. Installing a third-party package and importing it are separate actions.

With `import math`, the module is available as `math`, and its sine function is accessed as `math.sin`. The import does not independently bind a bare name `sin`. The original notebook's `sin(3)` therefore fails in a fresh kernel unless that name was previously defined or imported. Its argument is in radians.

**Read R4:** Which names are available after the import? Is `math` the function, or the object through which you access it? What does `math.log(1.05)` measure if 1.05 is a gross growth factor?


```python
import math
demo_gross_growth = 1.05
demo_log_growth = math.log(demo_gross_growth)
print(math.sin(3))
print(demo_log_growth)
```


`math.log` computes the natural logarithm by default. A log change is related to a proportional change but is not exactly the same quantity. An alias changes how you refer to an imported module, not what the module does:

```python
import math as m
m.log(1.05)
```

**Optional bridge, not required to execute:** the source introduces `import numpy as np`. If NumPy is installed, `np` is the chosen local name, and `np.log(...)` accesses an operation through that name. NumPy supports array calculations; pandas organizes tabular data; matplotlib creates plots. These tools extend the same ideas about names, types, and operations. They are not prerequisites for this lecture.


### E4 · Growth through two lenses

GDP rises from 100 to 105 in the same units. Import `math` using the alias `m`. Compute the ordinary proportional change and the natural-log change. Display both multiplied by 100, labeling one as percent growth and the other as 100 times the log change.

Explain why the values are close but unequal. Repeat for a rise from 100 to 150. The log calculation requires strictly positive levels; explain why zero is problematic.

**Write from a blank cell.** First state your inputs, their types and units, and your intended output.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept — open only after an attempt</summary>

Use the ratio of new to old levels. Log growth transforms the ratio rather than subtracting one.

</details>


<details>
<summary>Hint 2 — Pseudocode — open only after an attempt</summary>

Form the gross growth ratio → calculate ratio minus one → take its natural log → scale for display.

</details>


<details>
<summary>Hint 3 — Syntax — open only after an attempt</summary>

Use `m.log(ratio)` and `100 * (...)`. Log changes and ordinary percentage changes need distinct labels.

</details>


## 5 · Numerical expressions are economic claims

`+`, `-`, `*`, `/`, and `**` implement addition, subtraction, multiplication, division, and exponentiation. Parentheses make the intended structure explicit. `^` is not exponentiation in Python; for integers it is bitwise exclusive OR.

For principal $P$, a constant per-period decimal rate $r$, and $T$ periods, end-of-period wealth is $P(1+r)^T$, assuming interest compounds and there are no cash flows. Translate the mathematical structure before entering numbers. Match the rate's period to the number of periods. A rate of `0.05` means 5%; a value of `5` is one hundred times larger.

**Read R5:** Explain the economic assumptions in this expression. Predict whether replacing `** 2` with `* 2` would represent the same model.


```python
demo_principal = 1000
demo_rate = 0.05  # Decimal rate per period.
demo_wealth = demo_principal * (1 + demo_rate) ** 2
print(demo_wealth)
```


### Division and negative values

`/` performs true division; integer inputs such as `7 / 2` still yield a float. `//` takes the floor of the quotient, rounding toward negative infinity. `%` gives the corresponding remainder. For integers and a nonzero divisor, `a == (a // b) * b + a % b`.

**Read R6:** Predict these four outputs. Why is `-7 // 3` not the same as truncating `-7 / 3` toward zero? In allocating observations to equal-sized groups, what could the quotient and remainder tell you?


```python
print(7 / 3)
print(7 // 3, 7 % 3)
print(-7 // 3, -7 % 3)
print(7 == (7 // 3) * 3 + 7 % 3)
```


### Floating-point approximation

Many decimal fractions cannot be represented exactly in binary floating point. Small discrepancies are a representation issue, not necessarily an economic or algebraic mistake. Avoid rounding intermediate calculations simply to make the display attractive. Formatting changes presentation; `round()` produces a rounded numerical value, still generally stored as a float.

Use a tolerance when numerical closeness is the claim. `math.isclose` allows relative and absolute tolerances; choose them to fit the scale and meaning of the calculation. Near zero, an absolute tolerance is especially relevant. Tolerances do not repair an incorrect formula.

**Read R7:** Predict the Boolean results. Explain why exact equality and closeness ask different questions.


```python
demo_total = 0.1 + 0.2
print(demo_total)
print(demo_total == 0.3)
print(math.isclose(demo_total, 0.3, rel_tol=1e-12, abs_tol=1e-12))
```


### E5.1 · Compounding and deflating

First preserve the original one-period exercise: principal $1,000 and a 5% rate. Calculate ending nominal wealth; it should be $1,050.

Extend to three years at the same annual rate, with annual inflation of 2% and no deposits or withdrawals. Calculate nominal wealth, real wealth in initial-year dollars, and the exact cumulative real return. Assume both rates remain constant.

**Checks:** real wealth is below nominal wealth and above initial wealth. At zero inflation the two wealth measures coincide. Explain why a nominal dollar amount and an initial-year-dollar amount need different labels.

**Write from a blank cell.** First state your inputs, their types and units, and your intended output.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept — open only after an attempt</summary>

Deflate a nominal end value by the cumulative price-level growth factor. A real return compares real end wealth with initial wealth.

</details>


<details>
<summary>Hint 2 — Pseudocode — open only after an attempt</summary>

Compound nominal wealth → compound inflation → divide nominal wealth by that price factor → calculate cumulative real growth.

</details>


<details>
<summary>Hint 3 — Syntax — open only after an attempt</summary>

Use `(1 + rate)**years`; real wealth is `nominal_wealth / (1 + inflation)**years`.

</details>


### E5.2 · Data batches and remainder

You have 103 observations and want batches of 12. Calculate the number of full batches and the leftover observations. Verify numerically that full batches plus the remainder reconstruct 103. Then change the count to 108.

Explain why ordinary division alone does not provide the desired two outputs.

**Write from a blank cell.** First state your inputs, their types and units, and your intended output.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept — open only after an attempt</summary>

The count of complete groups and the unused count are different quantities.

</details>


<details>
<summary>Hint 2 — Pseudocode — open only after an attempt</summary>

Floor-divide the count by batch size → calculate remainder → reconstruct the original total.

</details>


<details>
<summary>Hint 3 — Syntax — open only after an attempt</summary>

Use `//`, `%`, and `==`.

</details>


### D5 · A plausible-looking growth calculation

This code executes. It is supposed to compute percentage GDP growth from 200 to 210.

```python
old_gdp = 200
new_gdp = 210
growth_percent = new_gdp - old_gdp / old_gdp * 100
print(growth_percent)
```

Before repairing: identify the first failing line, or the first line with the wrong meaning. Predict the error or incorrect result. Then make the smallest justified repair and explain why it works. Copy the snippet into the blank cell to investigate.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept</summary>

The numerator must be the entire change in GDP, not just the old level.

</details>


<details>
<summary>Hint 2 — Pseudocode</summary>

Write the fraction on paper → group the numerator → divide by the baseline → multiply by 100.

</details>


<details>
<summary>Hint 3 — Syntax</summary>

Parenthesize `(new_gdp - old_gdp)` before division. A no-change case should yield zero.

</details>


## 6 · Strings: labels, cleaning, and reporting

A string is text enclosed in matching quotes. A quote inside a string needs either a different enclosing quote or an escape. The source's `'What's wrong with this string'` example, if written with an unescaped interior apostrophe, closes its literal too early and raises `SyntaxError`.

Valid alternatives include `"What's wrong with this string"` and `'What\'s wrong with this string'`. A syntax error prevents a cell from being parsed and executed; it differs from a runtime error on a later executed operation.

String `+` concatenates; multiplying a string by an integer repeats it. Multiplying two strings has no defined meaning. Python does not automatically add spaces between concatenated strings.

**Read R8:** Predict each line's output, including spaces. Explain why the first two results differ, and whether repeating a numeric-looking string constitutes multiplication of its numerical value.


```python
demo_a = "Hello"
demo_b = "UConn"
print(demo_a + demo_b)
print(demo_a + " " + demo_b)
print("2.5" * 3)
print(float("2.5") * 3)
```


### Reporting is a separate layer from calculation

An f-string evaluates expressions within braces. A format specification controls their display: `:,.2f` uses grouping commas and two decimal places; `:.1%` multiplies a decimal proportion by 100 and appends `%`.

These two representations of inflation require different formatting:

```python
decimal_inflation = 0.025
percent_inflation = 2.5
f"{decimal_inflation:.1%}"
f"{percent_inflation:.1f}%"
```

Using percent formatting on `2.5` would display 250.0%. The language cannot infer which unit convention you intended.

The older expression `"GDP: {:.1f}".format(123.45)` serves a similar purpose; recognize it when reading code, while using f-strings for your new work.

**Read R9:** What type is `demo_report`? Does formatting alter `demo_gdp_dollars`? Why should the numeric value remain available separately?


```python
demo_gdp_billions = 123.456
demo_gdp_dollars = demo_gdp_billions * 10**9
demo_report = f"GDP: ${demo_gdp_dollars:,.2f}"
print(demo_report)
print(type(demo_report), type(demo_gdp_dollars))
```


### E6 · Reproduce, then improve a macroeconomic report

Preserve the source exercise: set `country = "USA"`, `year = 2026`, and `inflation = 2.5`, then use an f-string to print exactly:

> Inflation in USA was 2.5% in 2026.

Next represent that same inflation rate as a decimal in a separate variable and reproduce the sentence using percent formatting. Explain the unit convention for both versions. Finally add a separate line reporting synthetic GDP of 27,500.5 billion dollars in dollars, with commas and two decimal places.

**Write from a blank cell.** First state your inputs, their types and units, and your intended output.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept — open only after an attempt</summary>

Formatting conventions must match the stored units. Converting billions to dollars changes scale, while display formatting does not.

</details>


<details>
<summary>Hint 2 — Pseudocode — open only after an attempt</summary>

Build the original sentence → convert percentage units to a decimal fraction → use percent formatting → convert and display GDP.

</details>


<details>
<summary>Hint 3 — Syntax — open only after an attempt</summary>

Use `f"... {value} ..."`, `:.1%` for a decimal rate, and `:,.2f` for the dollar amount.

</details>


### D6 · Four mistakes in one short report

Work sequentially. Some mistakes raise errors; one silently gives the wrong quantity.

```python
country = 'Cote d'Ivoire'
rate = "2.5"
decimal_rate = float(rate)
report = "Inflation in " + country + " was " + decimal_rate + "%"
print(f"Rate: {decimal_rate:.1%}")
print(report)
```

Before repairing: identify the first failing line, or the first line with the wrong meaning. Predict the error or incorrect result. Then make the smallest justified repair and explain why it works. Copy the snippet into the blank cell to investigate.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept</summary>

Inspect quote delimiters, text-versus-number concatenation, unit conversion, and whether the final label matches the stored rate.

</details>


<details>
<summary>Hint 2 — Pseudocode</summary>

Repair quoting → parse the rate and convert percent units to decimal → format a string without adding unlike types → ensure both displayed rates use the intended units.

</details>


<details>
<summary>Hint 3 — Syntax</summary>

Use double quotes around the country, divide the percentage-unit value by 100, and insert the decimal rate with `:.1%` in an f-string. Do not append an additional percent sign to percent formatting.

</details>


## 7 · Booleans make economic restrictions executable

Comparisons such as `>`, `>=`, `<`, `<=`, `==`, and `!=` produce Boolean values for the scalars used here. `=` assigns; `==` compares. Choosing strict versus inclusive inequalities is part of the substantive rule, not a cosmetic choice.

Use `and` when both conditions must hold; `or` when at least one is sufficient; `not` to reverse a truth value. Parentheses make compound rules readable. These operators act on truth values; more generally, `and` and `or` can return operands rather than Boolean objects. In this lecture, combine explicit comparisons so your results are Booleans.

**Read R10:** Preserve the source's prediction question. Predict each Boolean and identify which economic restriction it represents.


```python
demo_price = 12
demo_quantity = 80
print(demo_price > 10)
print(demo_quantity >= 100)
print((demo_price > 10) and (demo_quantity < 100))
```


### Short-circuit evaluation and guarding a division

Python evaluates `and` left to right and stops when the left operand is false; `or` stops when the left operand is true. In a Boolean rule, you can use this to guard an operation that would otherwise fail.

**Read R11:** Why does this not divide by zero? What happens if you reverse the two conditions? Here `False` means the acceptance rule was not met; it does not establish that an undefined ratio is numerically small.


```python
demo_standard_error = 0.0
demo_estimate = 0.4
demo_passes_screen = (demo_standard_error > 0) and (abs(demo_estimate / demo_standard_error) > 1.96)
print(demo_passes_screen)
```


### A first small collection: `all()` and `any()`

Square brackets create a list. For a list of Boolean checks, `all(checks)` asks whether every check passed; `any(checks)` asks whether at least one passed. The functions accept truthy/falsy values more broadly, but explicit Boolean checks keep the economic meaning clear.

**Read R12:** Predict the two outputs below. How does asking whether any check failed differ from asking whether every check passed? As an optional logic question, predict `all([])` and `any([])` and then run them: no counterexample exists in an empty collection, but no successful example exists either.


```python
demo_checks = [80 > 0, 12.50 >= 0, 0.05 < 1]
print(all(demo_checks))
print(any([not (80 > 0), not (12.50 >= 0), not (0.05 < 1)]))
```


List elements are evaluated **before** calling `all()` or `any()`. Thus `all([denom != 0, numerator / denom > 1])` does not safely guard against division by zero. Use a scalar `and` expression with the guard first for this task.

Later, pandas and NumPy introduce collections of Booleans and different rules for combining elementwise comparisons. Do not transfer scalar `and`/`or` mechanically to arrays.


### E7.1 · A precise screening rule

A synthetic training program admits applicants aged **18 through 64 inclusive**, with annual income **strictly below $40,000**, who are either unemployed or working **fewer than 20 hours per week**.

Start with age 22, income 18,000, `unemployed = False`, and 15 weekly hours. Create a Boolean for each restriction and one final eligibility Boolean. Display individual checks as well as the final result.

Test manually, changing one input at a time: age 17, age 18, age 64, age 65, income exactly 40,000, and exactly 20 hours while employed. Also test an unemployed applicant with 20 hours reported: apply the stated rule and separately flag whether that data combination deserves investigation. Do not silently change the rule.

**Write from a blank cell.** First state your inputs, their types and units, and your intended output.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept — open only after an attempt</summary>

Translate the logical structure before coding. The age and income requirements must hold regardless of which employment alternative passes.

</details>


<details>
<summary>Hint 2 — Pseudocode — open only after an attempt</summary>

Evaluate age range → evaluate income cutoff → evaluate the unemployment-or-hours condition → combine all three with AND.

</details>


<details>
<summary>Hint 3 — Syntax — open only after an attempt</summary>

Use `18 <= age <= 64` and parenthesize the employment alternatives. Comparisons against the cutoffs encode inclusivity.

</details>


### E7.2 · An econometric significance screen

Suppose a coefficient estimate is 0.08 and its standard error is 0.03. Under an assumed standard-normal approximation, form a Boolean indicating whether the absolute estimate-to-standard-error ratio exceeds 1.96. Require a strictly positive standard error and put that check first.

Test a negative estimate of the same magnitude, an estimate of zero, and standard errors of zero and −0.03. Explain why sign should not affect this two-sided screen. Explain why passing this screen alone does **not** establish causality, economic importance, or the validity of the standard error. The cutoff is an approximate 5% two-sided normal threshold, not a universal finite-sample critical value.

**Write from a blank cell.** First state your inputs, their types and units, and your intended output.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept — open only after an attempt</summary>

The standardized distance from zero uses magnitude. Invalid standard errors must prevent division.

</details>


<details>
<summary>Hint 2 — Pseudocode — open only after an attempt</summary>

Check positive standard error → only then compute the absolute ratio → compare with the cutoff.

</details>


<details>
<summary>Hint 3 — Syntax — open only after an attempt</summary>

Use `(standard_error > 0) and (abs(estimate / standard_error) > 1.96)`. Explain every part before adapting it.

</details>


### D7 · A rule that always passes

The intended rule accepts USA or CAN only. Trace what happens for MEX.

```python
country = "MEX"
allowed = country == "USA" or "CAN"
print(allowed)
```

Before repairing: identify the first failing line, or the first line with the wrong meaning. Predict the error or incorrect result. Then make the smallest justified repair and explain why it works. Copy the snippet into the blank cell to investigate.


*Blank notebook cell — write your own code here.*


<details>
<summary>Hint 1 — Concept</summary>

A nonempty string is truthy; it is not a comparison with country.

</details>


<details>
<summary>Hint 2 — Pseudocode</summary>

Compare country to the first label → compare it to the second label → combine those Boolean results.

</details>


<details>
<summary>Hint 3 — Syntax</summary>

Repeat `country == ...` on both sides of `or`. Inspect `type(allowed)` before and after your repair.

</details>


## 8 · Applied challenge: audit a synthetic wage offer

You are preparing a small, reproducible report for a labor-economics seminar. Use only scalar variables, the tools above, and one small list of validation checks. No loops, custom functions, or data-analysis libraries are required. Build each stage from a blank cell.

### Data dictionary

| Input | Raw value | Meaning |
|---|---|---|
| Country | `" usa "` | Reporting label |
| Year | `"2026"` | Reporting year |
| Hours per week | `"37.5"` | Assume every paid week has these hours |
| Paid weeks per year | `"48"` | Same schedule in both years |
| Prior-year hourly wage | `"24.00"` | Dollars per hour |
| Current hourly offer | `"25.20"` | Dollars per hour |
| Annual inflation | `"3.0"` | Percentage units, not decimal units |
| Education | `"16"` | Years |
| Experience | `"4"` | Years |

Assume all hours are paid at the stated rate, with no overtime, taxes, transfers, benefits, or other cash flows. Prior and current nominal earnings use their respective wages. The same schedule isolates the wage-rate change. All observations and coefficients are synthetic.

### C1 · Parse and audit

Write the raw inputs yourself. Keep them, then create typed and cleaned variables. Build Boolean checks requiring positive wages, weekly hours between 0 and 168 inclusive, paid weeks between 0 and 52 inclusive, nonnegative education and experience, and an inflation decimal greater than −1. Use `all()` to summarize the checks.

State which assumptions these checks cannot verify. For the next stages, use the supplied valid inputs; do not treat a printed `False` as something that automatically stops later cells.


*Blank notebook cell — write your own code here.*


### C2 · Nominal and real earnings

Calculate prior-year and current nominal annual earnings. Deflate current earnings into prior-year dollars. Calculate nominal earnings growth and exact real earnings growth as decimal proportions.

Explain why subtracting inflation from nominal growth is only an approximation. Identify the difference between a growth rate and a change in dollar levels.

**Sanity checks:** prior earnings are $43,200 and current nominal earnings are $45,360. Real earnings growth should be positive but less than nominal growth. Zero inflation should make nominal and real growth equal; if wage growth equals inflation with the schedule fixed, real growth should be zero up to floating-point tolerance.


*Blank notebook cell — write your own code here.*


### C3 · Read an econometric equation as a computation

For this exercise, a supplied descriptive model predicts the natural log of the numerical hourly wage measured in dollars:

$$\widehat{\log(w)} = 2.1 + 0.06\,education + 0.03\,experience.$$

Compute the predicted log wage, then exponentiate with `math.exp` to obtain a wage-scale prediction. Compare the offer with that prediction and report the proportional gap, using the prediction as the denominator. Do not estimate coefficients or import a modeling library.

Explain why comparing the dollar offer directly to the predicted log wage is meaningless. Also explain why exponentiating a fitted log outcome does **not generally yield the conditional arithmetic mean wage**: a retransformation adjustment depends on the error distribution. We use this only as a descriptive model benchmark; the equation supplies no causal identification.

**Sanity checks:** the predicted log wage is between 3 and 3.3, and the exponentiated prediction is positive. A one-year increase in education, holding experience fixed, adds 0.06 to predicted log wage; its exact proportional effect on the exponentiated prediction is `exp(0.06) - 1`.


*Blank notebook cell — write your own code here.*


### C4 · Communicate and screen

Produce a readable report containing country and year, both nominal annual earnings, current real annual earnings, both growth rates, the wage-scale model benchmark, and the offer's proportional gap from that benchmark. Show dollar amounts with commas and two decimal places and rates with one decimal percentage place. Keep unrounded numerical values for calculations.

Create a final Boolean that is true only if all input checks pass, exact real earnings growth is **strictly positive**, and the offered wage is **at least** the model benchmark. This is an exercise-defined screening rule, not a recommendation to accept a job.

Write three sentences: what the arithmetic establishes, what relies on assumptions, and what the model cannot establish.


*Blank notebook cell — write your own code here.*


### C5 · Transfer and reproduce

1. Restart the kernel and run your challenge cells in order. Define the required import within your challenge so it does not depend on section 4 having run.
2. Set inflation to zero; verify the nominal/real relationship.
3. Make the offered wage equal to the prior wage while retaining positive inflation; explain the sign of real growth before executing.
4. Restore baseline inputs. Raise education by one year, holding other values fixed; explain why only model-based quantities and potentially the final screen should change.
5. Try an invalid negative wage in the audit stage alone. Explain why merely creating a validity Boolean does not block later arithmetic. Enforcing such control flow belongs to a later lesson.
6. Restore baseline inputs and write a six-line pseudocode outline without looking at your implementation.

**Completion standard:** your notebook runs from a fresh kernel, your labels identify units, the audit catches the planted invalid input, your boundary predictions agree with your output, and you can explain each transformation without reading it aloud as syntax.


<details>
<summary>Challenge hint — Concept</summary>

Keep four layers separate: raw data, typed data, calculated quantities, and formatted output. A real value is a nominal value divided by a price-level factor. A log prediction must be transformed before comparison with a dollar wage.

</details>


<details>
<summary>Challenge hint — Pseudocode</summary>

Parse and clean → validate → compute annual earnings in each year → deflate current earnings → compare each current measure with prior earnings → compute log-wage prediction and exponentiate → compare offer with benchmark → evaluate the screen → format the report.

</details>


<details>
<summary>Challenge hint — Syntax</summary>

Useful pieces: `float(text)`, `int(text)`, `raw.strip().upper()`, `all([check_a, check_b])`, `math.exp(log_prediction)`, and `f"{decimal_rate:.1%}"`. Exact real growth is `(1 + nominal_growth) / (1 + inflation_decimal) - 1`. Keep the original data dictionary visible; do not substitute a guessed input unit.

</details>


## 9 · Exit ticket: prove that the foundations are becoming yours

Close the worked examples and answer in your own words:

1. Why does changing `price` not update an existing `revenue` variable?
2. How can a function display something useful but return `None`?
3. Why might converting `"007"` to an integer destroy information?
4. What distinguishes `NameError`, `TypeError`, `ValueError`, and `SyntaxError` in the examples you investigated?
5. Why can a correct Python expression still be an incorrect economic calculation?
6. What is the difference between a stored rate of `0.025` and `2.5`, and how would each be displayed as 2.5%?
7. Why does `all([guard, risky_calculation])` not protect the risky calculation in the same way as `guard and risky_calculation`?
8. What does restarting the kernel test that rerunning only the last cell does not?

For each major exercise, record **independent**, **concept hint**, **pseudocode**, or **syntax help**. Repeat two exercises tomorrow with different inputs. A correct output with an unexplained line is a prompt for more practice, not proof of mastery.

When asking for coaching, share: **exercise ID; expected behavior; your code; actual output/error; your current hypothesis**. Ask for the next hint level, not replacement code. We can then work from your reasoning and introduce a partial scaffold only where it is needed.
