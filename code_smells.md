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

## Spaghetti

Code should be modular and methods should perform a specific task.  Do not try and create methods that do everything (UI layout, validation, database updates, etc...) in a single method.

### Example

## God Class

Classes that control many other classes and have many dependencies and lots of responsibilities.

## Too Many Statements

A method with _Too Many Statements_ is any method that has a large number of lines.

_Too Many Statements_ warns about any method that has more than 5 statements.
Count +1 for each statement in a method and +1 for every statement within a control structure
(`if`, `else`, `case`, `when`, `for`, `while`, `until`, `begin`, `rescue`) but
it doesn't count the control structure itself.

### Example

## Dinosaur

Code uses the old Mumps style of coding with line tags, goto, break, then I do not recommend adding Cache ObjectScript (COS) procedure blocks within the legacy code, because if added within code that contains things like structured DOs, then it could have impact on process flow, variable scoping, etc...

## Dot

Dotted syntax is not generally allowed

### Example

```cos
  // Bad
  If date=$Horolog
  . Set count = $Increment(count)

  // Good
  If date=$Horolog {
    Set count = $Increment(count)
  }
```

## Shorty

Many ObjectScript functions have a short name in addition to their regular name.
However, using these short names can be confusing, especially for new developers, and harm code readability.

### Example

```cos
  // Bad
  S today = $H

  // Good
  Set today = $Horolog
```

## Hardcoded

Use parameters insted

## Xecuter

The use of XECUTE command has security and performance implications:

* Security: the command to be executed may be a user input; if validation is not performed thoroughly, malicious code may be executed.
* Performance: the string input needs to be constructed (if not a single string literal but a concatenation of strings, for instnace) and evaluated.

For these reasons, you should avoid using XECUTE and use a proper set of commands instead.
