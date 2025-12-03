[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=21934925)

# README – Using `try`, `except`, `else`, `finally` and `raise` in Python

This document explains the basic concepts of exception handling in Python and then walks step by step through the logic of solving the four exercises you have, **without** showing the full solutions.

---

## 1. What are exceptions?

An *exception* is an error that occurs during the execution of a program.  
Examples:

- Trying to divide by zero
- Converting a non-numeric string to an integer
- Opening a file that does not exist

If an exception is not handled, the program stops with a traceback.  
Python gives you tools (`try`, `except`, `else`, `finally`, `raise`) to handle these situations gracefully.

---

## 2. `try` and `except`

### `try`
You put code that **might fail** inside a `try` block.  
If everything runs without errors, Python skips the `except` blocks.

### `except`
You put code that **handles errors** inside one or more `except` blocks.  
Each `except` can handle a specific type of exception (e.g. `ValueError`, `ZeroDivisionError`).

**Typical pattern:**
- `try`: do something risky (division, file open, type conversion).
- `except`: catch and handle specific errors.

---

## 3. `else` with `try`

The `else` block is executed **only if no exception was raised** in the `try` block.

This is useful when:
- You want to separate “error handling” (`except`) from “normal success behavior” (`else`).
- Example pattern:
  - `try`: attempt something (like converting input).
  - `except`: handle invalid input.
  - `else`: executed only if input is valid, e.g. print a success message or continue processing.

---

## 4. `finally`

The `finally` block is executed **no matter what happens**:

- runs if there was no error
- runs if there was an error that was caught
- even runs if you `return` inside `try`/`except` (the `finally` happens before the actual return)

Typical uses:

- closing a file or database connection
- printing a “cleanup” or “operation finished” message
- freeing resources

---

## 5. `raise`

`raise` is used to **manually trigger an exception**.

You use it when:

- You detect an invalid situation yourself (e.g. negative age, invalid grade).
- You want to stop execution and signal that something is wrong.
- You want to create **custom error messages**.

Example form (conceptual, not the full solution):

```python
raise ValueError("Age must be a positive integer!")
````

You can then catch this error in an `except` block elsewhere.

---

## 6. Further reading

For more details and examples on `try`/`except` in Python, you can check:

[W3Schools – Python Try Except](https://www.w3schools.com/python/python_try_except.asp)

---

## 7. Step-by-step: How to solve the exercises (without showing solutions)

Below are **guides** for solving each exercise using the concepts above. They describe the algorithm and structure, but **do not include the final code**.

---

### Exercise 1 – Safe division with `try / except / else / finally`

**Goal:**
Ask the user for two numbers, divide them, handle invalid input and division by zero, and always print a final message.

**Steps to solve:**

1. **Read inputs:**

   * Ask the user for a numerator.
   * Ask the user for a denominator.
   * Place these lines inside a `try` block.

2. **Convert to numbers:**

   * Convert both inputs to `float` inside the `try` block.
   * This is where a `ValueError` might occur if the user types text.

3. **Perform the division:**

   * Still inside the `try` block, divide numerator by denominator.
   * This is where a `ZeroDivisionError` can occur if the denominator is `0`.

4. **Handle input errors:**

   * Add an `except ValueError` block:

     * Print a message telling the user they must enter numeric values.

5. **Handle division by zero:**

   * Add an `except ZeroDivisionError` block:

     * Print a message that division by zero is not allowed.

6. **Use `else` for successful division:**

   * In the `else` block:

     * Print that the division was successful.
     * Print the result calculated earlier.

7. **Use `finally`:**

   * In the `finally` block:

     * Print `"Operation finished."` regardless of success or error.

---

### Exercise 2 – Using `else` after a successful `try`

**Goal:**
Ask the user for an integer, confirm success in `else`, and always print a finishing message in `finally`.

**Steps to solve:**

1. **Read input in `try`:**

   * Ask the user to “Enter an integer”.
   * Convert the input to `int` inside the `try` block.

2. **Handle invalid integers:**

   * Add an `except ValueError`:

     * Print a message that the input is not a valid integer.

3. **Use `else` for success:**

   * In the `else` block:

     * Print `"Conversion successful!"` if no exception occurred.

4. **Use `finally`:**

   * In the `finally` block:

     * Print `"Attempt finished."` so it always appears at the end.

---

### Exercise 3 – Using `raise` to create a custom error (`check_age`)

**Goal:**
Create a function `check_age(age)` that ensures `age` is a positive integer, raises a `ValueError` otherwise, and then use it inside a `try` block when reading user input.

**Steps to solve:**

#### Part A – The `check_age` function

1. **Define the function:**

   * Create a function that takes one parameter: `age`.

2. **Check that `age` is an integer:**

   * Inside the function, verify that `age` is of type `int`.
   * You can use an `isinstance` check or assume the caller has already converted the input to `int`.

3. **Check positivity:**

   * If `age` is less than or equal to zero:

     * Use `raise ValueError("Age must be a positive integer!")`.

4. **Return on success:**

   * If all checks pass:

     * Return `True` (or any indication that the age is valid).

#### Part B – Using `check_age` with user input

1. **Use a `try` block:**

   * Ask the user to “Enter your age”.
   * Convert the input to `int` inside the `try` block (this can raise `ValueError` itself).

2. **Call `check_age`:**

   * Still in the `try` block:

     * Call `check_age` with the integer you obtained.
     * If it returns successfully, print `"Age accepted."`.

3. **Handle errors:**

   * Use `except ValueError` to catch:

     * conversion errors (from invalid input), and
     * the custom error from `check_age`.
   * Print the error message (e.g. using the exception object).

---

### Exercise 4 – Original example: `read_input(prompt, low, high)`

**Goal:**
Write a function that repeatedly asks the user for a number until they enter an integer within a given range. It uses `try/except` for conversion errors and prints a message in `finally` at each attempt.

**Steps to solve:**

1. **Define the function:**

   * Create a function with three parameters: `prompt`, `low`, and `high`.

2. **Use an infinite loop:**

   * Inside the function, use a `while True:` loop to keep asking until the user inputs a valid value.

3. **Place input and conversion in `try`:**

   * Inside the loop:

     * In the `try` block:

       * Ask the user for input using `prompt`.
       * Convert the received string to an `int`.

4. **Check the range:**

   * Still inside the `try` block:

     * If the number is **less than `low` or greater than `high`**:

       * Print a message like
         `"You must type in an integer between low and high"`.
       * Use `continue` to go to the next iteration of the loop.

5. **Handle invalid integers:**

   * Add an `except ValueError` block:

     * Print the same error message about the required integer range.

6. **Use `else` or direct `return`:**

   * In the `else` block (or after `try/except` if you prefer):

     * If the number is valid (and within the range), **return** it.
     * Returning exits both the loop and the function.

7. **Use `finally`:**

   * Add a `finally` block:

     * Print `"Attempt processed."` each time the user enters something, regardless of success or error.

8. **Outside the function:**

   * Call the function with appropriate arguments (e.g. prompt and bounds).
   * Store the return value in a variable.
   * Print the final accepted number.


