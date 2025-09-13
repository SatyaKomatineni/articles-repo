# General Programming Prompt for Code Generation

9/13/2025 (Version 1.0, will be updated continually during the next year)

1. Help AI write maintainable code

# Main Instrucitons - for basic scripting/functions

1. Break the code into functions and do not write one long script
2. Use a main function to use those functions to accomplish the final task
3. Avoid nested if and else statements by following what is called a "guard clause", or "early return", or "fail-fast" style

# Handling for errors through Exceptions

1. Use exceptions to handle errors
2. Use print statements where meaningful while relying on exceptions 
3. Use a global catch at the end in a main function to print the full stack trace and root cause
4. Write a special function to repor that final catch and write all printing details of the stack trace and root cause in that function
5. In functions check input values and raise meaningful exceptions. Do keep the code readable
6. If needed write a function for input value validation where they can take multiple arguments to check for their validation and use that function to validate multiple values at a time and raise exceptions
7. Don't catch an exception if there is nothing you can do about it but raise it again. Unless you want to provide additional context, and if so provide that additional context in a new exception and attach the old exception as its parent
8. If you are checking for a value to be none, then raise an exception and point out the reason instead of returning "none"

# When to use classes in addition to functions

1. if it is easier to understand code using classes do so.

# On Comments

1. Please use them
2. At a minimum explain each function
3. Keep them brief enough not to impact the readability of the code

# On Typing (Useful. Ask for it once you are familiar with them and your readability is not clouded)

1. Introduce types in code
2. Yet keep the code readable
3. When the types are obvious skip them

# On names

1. Keep the variable names meaningful but short enough
2. Use industry standard naming such as mixed case etc
3. Use industry standard "-" or "_" separators as needed