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

```cos
Class Sample.NightWatch
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
        Set objPerson.name = "Jon"
        Set objPerson.surname = "Snow"
        Set objPerson.age = 42

        Do ..DoWork1(objPerson)
        Do ..DoWork2(objPerson, 3)  // PARAMETER error
        Do ..DoWork3(objPerson)     // PARAMETER error
    }
}
```