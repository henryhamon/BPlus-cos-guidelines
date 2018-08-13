# Code Smells

Code Smells are indicators of where your code might be hard to read, maintain or evolve, rather than things that are specifically _wrong_.

## Multiple If/Else

_Multiple if/else_ makes the code difficult to test and maintain

### Example

## Long Parameter List

A _Long Parameter List_ occurs when a method has a lot of parameters.

### Solution
A common solution to this problem would be the introduction of parameter objects.

### Example

## Unused Parameter

_Unused Parameter_ refers to methods with parameters that are unused in scope of the method.

Having unused parameters in a method is code smell because leaving dead code in
a method can never improve the method and it makes the code confusing to read.

### Example

## Data Clump

In general, a _Data Clump_ occurs when the same two or three items frequently
appear together in classes and parameter lists, or when a group of instance
variable names start or end with similar substrings.

The recurrence of the items often means there is duplicate code spread around to handle them. There may be an abstraction missing from the code, making the system harder to understand.

### Example

##
Code should be modular and methods should perform a specific task.  Do not try and create methods that do everything (UI layout, validation, database updates, etc...) in a single method.

### Example

## God Class

Classes that control many other classes and have many dependencies and lots of responsibilities.
