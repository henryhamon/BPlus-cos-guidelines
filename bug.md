# Bug

Is an error, flaw, failure or fault in a computer program or system that causes it to produce an incorrect or unexpected result, or to behave in unintended ways.

## :red_circle: Fatal.
## :warning: Warnings

## :red_circle: Class not found

 :red_circle: Status: Fatal.

 The class must exists.

 ### Example
 
 The class _User.Test_ is not compiled and another method call it.
 ```cos
    Set tSC = ##class(User.Test).NotCompiled() //Return an error on execution
    
 ```

## :red_circle: Incompatible argument type in a method
  
 The argument passed must be of the type expected by the method. When no type is defined for a parameter, a %String argument is expected.

## Example
```cos
Class Sample.Pacient
{
    ClassMethod DoWork1(pObj As Sample.Person)
    {
        Write !,pObj.name_" "_pObj.surname
    }

    ClassMethod DoWork2(pObj As Sample.Person, pNmbObj As %Numeric)
    {
        For i=1:1:pNmbObj
        {
            Write !,pObj.name_" "_pObj.surname
        }
    }

    ClassMethod DoWork3(pDate As %DateTime)
    {
        Write !,pDate
    }

    ClassMethod foo()
    {
        #Dim objPerson As Sample.Person = ##class(Sample.Person).%New()
        Set objPerson.name = "Joseph"
        Set objPerson.surname = "Matias"
        Set objPerson.age = 42

        Do ..DoWork1(objPerson)
        Do ..DoWork2(objPerson, 3)  // PARAMETER error
        Do ..DoWork3(objPerson)     // PARAMETER error
    }
}
```

## :red_circle: Local variable name is too long

ObjectScript only account for the 31 first characters in a local variable name; further characters will be ignored.

This check ensures that all declared local variables have a length less than, or equal to, 31 characters.

See [here](https://docs.intersystems.com/cache201513/csp/docbook/DocBook.UI.Page.cls?KEY=GORIENT_appx_identifiers#GORIENT_appx_identifiers_cos_localvars) for more information.

## :red_circle: Method not found

Declare the method you need to invoke for the class, or remove the reference to the undefined method.

```cos
Class Sample.NightWatch Extends %RegisteredObject
{
    Property name As %String;
    Property surname As %String;
    Property age As %Integer;

    ClassMethod FullName() {
      Write !,..name_" "_..surname
    }
}

  ...

Set obj = ##class(Sample.Person).%New()
Set obj.name = "Joseph"
Set obj.surname = "Matias"

Do obj.() // METHOD DOES NOT EXIST
```

## :red_circle: Method with unexpected arguments

Use the right number of parameters to call the function.

## Example
```cos
Class Sample.Test
{
    ClassMethod Test(pstrArg1 As %String, pstrArgs2 As %String)
    {
        Write !,pstrArg1
        Write !,pstrArg2
    }

    ClassMethod bar()
    {
        Do ..Test("a","b","c") // Wrong number of parameters
    }
}
```

## :red_circle: Missing argument(s) in method call

Use the right number of parameters to the function call.

## Example
```cos
Class Sample.Foobar
{
    ClassMethod m(x As %String, y As %String)
    {
        Write !,x
        Write !,y  // UNDEFINED
    }

    ClassMethod Test()
    {
        do ..m("Hello")
    }
}
```
### Exceptions

Note that this check will account for arguments with default values, which means that if in the code above foo is declared as:
```cos
ClassMethod m(x As %String, y As %String = "World")
```

## :red_circle: No quit (or return) found

Method is defined with ReturnType value but has no QUIT or RETURN command.

## Example
```cos
ClassMethod Test() As %String
{
}

ClassMethod Call()
{
    #Dim str As %String = ..Test()  // Function must return a value
}
```

## :red_circle: Property not found

Declare the property you need to use in the class, or remove the reference to the undefined property.

## Example
```cos
Class Sample.Training Extends %RegisteredObject
{
    Property name As %String;
    Property surname As %String;

    Method CreateJon()
    {
        Set obj = ##class(Sample.Person).%New()
        Set obj.name = "Joseph"
        Set obj.surname = "Matias"
        Set obj.age = 55 // age is UNDEFINED

        Return obj
    }
}
```

## :red_circle: Quit whitout arguments

Method is defined with ReturnType value but has `QUIT` or `RETURN` without arguments.

## Example
```cos
ClassMethod foo() As %String
{
    Quit /// QUIT WITHOUT RETURN
}

ClassMethod bar()
{
    #Dim str As %String = ..foo()
}
```

## :red_circle: Variable not found

A variable is used in a expression, but was neither passed as argument nor set before.

Declare or define as argument the variable you need to use, or remove the reference to the undefined variable.

**NOTE**: You can enable a Best Practice option of this rule to apply following additional validations:

* Kill command argument must be declared.
* Exception variable (catch) must be declared.
* When using "into:" in a $sql statement, the output variable must be previously         declared.
* Variable passed by reference must be declared on the calling origin.

## Example
```cos
Method Add()
{
    Set a=1
    Set c=a+b // b is UNDEFINED
    Write !,c
}
```
### Exceptions
* Methods with `ProcedureBlock=0` are not checked because those methods can use variables from global scope.
* It is not necessary to define "i" when calling $I(i) as it intiates the variable automatically.
* A variable passed by reference does not need to be initialized as it is expected to be set inside the method called.
* A variable used in a macro does not need to be initialized neither declared as it is expected to be set inside the macro.