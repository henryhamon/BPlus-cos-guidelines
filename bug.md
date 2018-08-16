# Bug

Is an error, flaw, failure or fault in a computer program or system that causes it to produce an incorrect or unexpected result, or to behave in unintended ways.

## Class not found

 :red_circle: Status: Fatal.

 The class must exists.

 ### Example
 
 The class _User.Test_ is not compiled and another method call it.
 ```cos
    Set tSC = ##class(User.Test).NotCompiled() //Return an error on execution
    
 ```

## Incompatible argument type in a method
 
 :red_circle: Status: Fatal.
 
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

## Local variable name is too long

:red_circle: Status: Fatal.

ObjectScript only account for the 31 first characters in a local variable name; further characters will be ignored.

This check ensures that all declared local variables have a length less than, or equal to, 31 characters.

See [here](https://docs.intersystems.com/cache201513/csp/docbook/DocBook.UI.Page.cls?KEY=GORIENT_appx_identifiers#GORIENT_appx_identifiers_cos_localvars) for more information.

## Method not found

:red_circle: Status: Fatal.

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