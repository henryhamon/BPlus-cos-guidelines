
![Image of Judge Dredd](https://forums.2000ad.com/avatars_static/avatar_235.jpg)

> I am the law and you'd better believe it! <br>
> -- Judge Dredd


# Code Smells

Code Smells are indicators of where your code might be hard to read, maintain or evolve, rather than things that are specifically _wrong_.

## :warning: Warnings

### Shorty

Many ObjectScript functions have a short name in addition to their regular name.
However, using these short names can be confusing, especially for new developers, and harm code readability.

#### Example

```cos
  // Bad
  S today = $H

  // Good
  Set today = $Horolog
```

### Multiple If/Else

_Multiple if/else_ makes the code difficult to test and maintain

#### Example


### Long Parameter List

A _Long Parameter List_ occurs when a method has a lot of parameters.

_Long Parameter List_ reports any method or block with more than 5 parameters.

#### Solution
A common solution to this problem would be the introduction of parameter objects.

#### Example

```cos
 ClassMethod LongList(foo as %String, bar as %String, baz as %String, fling as %String, flung as %String, blung as %String)
 {
 }
```

### Unused Parameter

_Unused Parameter_ refers to methods with parameters that are unused in scope of the method.

Having unused parameters in a method is code smell because leaving dead code in
a method can never improve the method and it makes the code confusing to read.

#### Example

```cos
 ClassMethod LongList(foo as %Numeric, bar as %Numeric, baz as %Numeric) as Numeric
 {
	Quit foo + bar // but not baz
 }
```

---

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
(`if`, `else`, `case`, `when`, `for`, `while`) but it doesn't count the control structure itself.

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

## Hardcoded

Use parameters insted

## List Over Pieces

Use $Piece is slightly less intuitive than $ListFromString/$ListNext.

* A While or Do...While loop performs slightly better than an equivalent argumentless For loop.

### Example

```cos
  // Bad
  Set string = "1,2,3,4"
  For i=1:1:$Length(string,",") {
    Set piece = $Piece(string,",",i)
    Write !, piece
  }

  // Good
  Set string = "1,2,3,4"
  Set list = $ListFromString(string,",")
  Set pointer = 0
  While $ListNext(list, pointer, piece) {
    Write !, piece
  }
```

## ListBuild over ListFromString

Given a choice of data structures, traversal of $ListBuild lists seems to have a slight performance edge over an equivalent local array for small inputs, while the local array offers significantly better performance for larger inputs. I'm not sure why there's such a huge difference in performance. (Relatedly, it would be worth comparing the cost of random inserts and removals in a $ListBuild list vs. a local array with integer subscripts; I suspect that set $list would be much faster than shifting the values in such an array.)

```cos
  Set list = $ListFromString("1,2,3,4",",")
  // Good
  Set list = $ListBuild(1,2,3,4)
```

## No Default case:w

The $CASE function can be written without a default case, as in:

```cos
  // Bad
  Set x = $case(someVar, "foo": val1, "bar": val2)
```

However, this means that if, in the above example, the value of x is neither "foo" nor "bar", the resulting value will be undefined.

While it can happen that the tested variable only has a certain set of possible values, it is preferred to always have a default case; for instance:

```cos
  // Good
  Set x = $case(someVar, "foo": val1, "bar": val2, : val3)
```

This issue may lead to false positives if the set of possible values is indeed limited; such a scenario is not detected.

## Method uses too many variables

This check detects the number of local variables declared/used in a method and reports an issue if this number is deemed to be too high (this number is configurable).
Moreover, it is often the case that a method with too many variables is also too complex. It is therefore strongly recommended to refactor the code in order to avoid this issue.

## ByRef/Output method argument not assigned

This rule detects when a method argument is passed by reference but is never assigned in the body of the method.
This either means that this argument does not need to be passed by reference, or that the code forgets to assign a value to it.

## Boolean expression not surrounded by parentheses

ObjectScript's flow control keywords accepting boolean expressions (such as if, while and others) allow to write boolean expressions without surrounding parentheses; for instance:

```cos
  // Bad
  If x > 4 {
    Write x, " is greater than 4", !
  }

  // Good
  If (x > 4) {
    Write x, " is greater than 4", !
  }
```

## Class has too many methods

This rule checks that a given class has a number of methods which is less than, or equal to, a configurable threshold. A class with too many methods is often an indication that this class needs to be refactored into several, smaller classes.

*Note that class methods are not accounted for.*

## Class has too many parameters

A long parameter list can indicate that a new structure should be created to wrap the numerous parameters or that the class is doing too many things.

## Class has too many properties

A long property list can indicate that a new structure should be created to wrap the numerous properties or that the class is doing too many things.

## Confusing use of numeric test operators

Given two variables n1 and n2, and assuming both variables have a numeric value, there are two ways to check whether one is greater than, or equal, to the other:

```cos
  // Bad
  // Old way; literally, "n1 not strictly less than n2"
    n1 '< n2

  // Good
  // New way
    n1 >= n2
```

For readability reasons, the second form is preferred. Similarly, you will want to use <= instead of '> to test whether a number is less than, or equal, to another

## Legacy flow control with more than one statement

It is possible for legacy flow control statements (if, else and for) to have more than one statement; for instance:

```cos
  // Bad
    // if condition c is true, set variable x then print variable y
    if c set x = 1 w y,!
```

However, this is hardly readable.

If you have more than one statement, it is preferred to use the brace forms of these statements instead; the code above becomes:

```cos
  // Good
    If c {
        Set x = 1
        Write y, !
    }
```

More generally, in new code, you should prefer using the brace forms of flow control statements instead of the legacy, braceless forms.

## Method argument with no type

This rule checks that all parameters of a given method or class method have a type declared.

```cos
  // Bad
  Method m(foo, bar)
  {
      // body of the method here
  }

  // Good
  Method m(foo as %String, bar as %Integer)
  {
      // body of the method here
  }
```

## Local variable name is too long

ObjectScript only account for the 31 first characters in a local variable name; further characters will be ignored.
[reference](http://docs.intersystems.com/cache20151/csp/docbook/DocBook.UI.Page.cls?KEY=GORIENT_appx_identifiers#GORIENT_appx_identifiers_cos_localvars)


## Boolean expression not surrounded by parentheses

ObjectScript's flow control keywords accepting boolean expressions (such as if, while and others) allow to write boolean expressions without surrounding parentheses; for instance:

```cos
  // Bad
  If x > 4 {
    Write x, " is greater than 4", !
  }

  // Good
  If (x > 4) {
    Write x, " is greater than 4", !
  }
```

## Empty catch block

Empty *CATCH* blocks

## Method has too many lines

This check retrieves the number of lines in a given method and checks that it is less than, or equal to, a given threshold. Note that the opening and closing braces are taken into account, but the method declaration is not.
The default value is 50.

## Method uses too many variables

This check detects the number of local variables declared/used in a method and reports an issue if this number is deemed to be too high (this number is configurable).
Moreover, it is often the case that a method with too many variables is also too complex. It is therefore strongly recommended to refactor the code in order to avoid this issue.

## Property of type %String without a MAXLEN

If a property of type %String (or %Library.String) is declared without a MAXLEN parameter, then the default length (of 50) will be used when an instance of the class is persisted.
Depending on the situation, this will lead to either wasted storage (if 50 is too high) or writes and retrievals which do not perform as well as they could (if 50 is too low).

## Property name declared with quotes

Don't use quoted names in classes to avoid confusion.

```cos
  // Bad
  Property "Hello_World";

  // Good
  Property Hello_World;
```

## Several statements on a single line

ObjectScript does allow more than one statement on the same line, and in fact if you use legacy flow control statements (which you shouldn't, really) you even have to put more than one statement on a same line.

```cos
  // Bad
  // If c is true, write variable "x" then quit
  If c W x q

  // Bad
  // Create a variable named x, then invoke a method on it
  Set x = ##class(some.Class).%New() Do x.someMethod()
```

For readability reasons, it is therefore strongly recommended to always put statements on different lines.

## Try block is too long

In all the statements inside the try block, it frequently happens that only one or two actually throw an exception.
It is therefore advised, both for readability and understandability, to keep try blocks as small and self-contained as possible.

## Unnecessary ELSE

```cos
  // Bad
    If (condition) {
        Return value1
    } Else {
        Do something
        Do somethingElse
        etc
        etc
    }

  // Good
    If (condition1) {
        Return value1
    }

    Do something
    Do somethingElse
    etc
    etc
```

## Use of $ETRAP

```cos
  // Bad
    new $etrap
    set $etrap = "do somethingElse()"

    // Execute "do somethingElse()" if "do something()" throws an exception
    do something()

  // Good
    try {
        do something()
    } catch (e) {
        // Perform something with e if needed
        do somethingElse()
    }
```

[More Information](http://docs.intersystems.com/cache20151/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_ctry)

## Using =' instead of '= for null oref checks

Consider this code:

```cos
    Class Fubar
    {
      ClassMethod m(someArg as Some.Class)
      {
          if (someArg =' "") {
              w "my argument was a null oref", !
          }
      }
    }
```

There is a trap in this code.

ObjectScript allows spaces between the unary negation operator (that is, ') and its argument: 'a and ' a are the same.

## Variable declared but not used

This inspection reports all variables declared in the context of a method or classmethod, which is not used afterwards anywhere in this method.
Such variables take unnecessary memory and processing power; they should be removed for efficiency reasons.

## Track uses of "TODO" tags

TODO tags are commonly used to mark places where some more code is required, but which the developer wants to implement later.
Sometimes the developer will not have the time or will simply forget to get back to that tag.
This rule is meant to track those tags and to ensure that they do not go unnoticed.

## Track uses of CacheTemp

CacheTemp are commonly used, but it´s good to use different variables types like List or Array.

## Use of $ZUTIL may be replaced

Starting with Caché 2010.1, InterSystems does not document the $ZUTIL function anymore. It is advised that uses of this function be replaced with alternatives.
[Please see this URL for a list of replacements.](http://docs.intersystems.com/cache20151/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_replacements)
