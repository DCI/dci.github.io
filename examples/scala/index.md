---
layout: default
title: Scala Examples
category: examples
---

## DCI in Scala

By annotating a plain Scala class with @context we allow it to be a DCI Context where we can define Roles and role methods. The names of object identifiers act as Role names and Roles are defined with a 'role' keyword:
```Scala
@context
class MoneyTransfer(Source: Account, Destination: Account, amount: Int) {

  Source.withdraw // Trigger method setting off interactions...

  role Source {
    def withdraw() {
      Source.decreaseBalance(amount)  
      Destination.deposit // First Interaction with another role...
    }
  }

  role Destination {
    def deposit() {
      Destination.increaseBalance(amount)
    }
  }
}
```
The annotation is a macro that will transform the above source code at compile time to the equivalent of the following code as though we had written this form the beginning:
```Scala
class MoneyTransfer(Source: Account, Destination: Account, amount: Int) {
  
  Source_withdraw()
  
  private def Source_withdraw() {
    Source.decreaseBalance(amount) // Calling Data instance method
    Destination_deposit()          // Calling Role method
  }
  
  private def Destination_deposit() {
    Destination.increaseBalance(amount)
  }
}
```
As you see, the role methods have been lifted into the Context namespace by being prefixed with the role name, like in RoleName_roleMethod(). 

So the Data objects passed into the Context are never modified or wrapped in any way but simply calls the newly created role methods now living in the Context scope.

### Resources

- [Downloads and more info](https://github.com/DCI/scaladci)
- [MoneyTransfer examples](https://github.com/DCI/scaladci/blob/master/examples/src/test/scala/scaladci/examples/MoneyTransfer1.scala)
- [Dijkstra example](https://github.com/DCI/scaladci/blob/master/examples/src/test/scala/scaladci/examples/Dijkstra.scala) demonstrating recursion and having different Roles being "bound" to the same object at the same time.

### Credits
- Marc Grue, author of the Scala macro
- Risto Välimäki for [inspiration](https://groups.google.com/d/msg/object-composition/ulYGsCaJ0Mg/rF9wt1TV_MIJ)
- Rune Runch for inspiration with the [Marvin](http://fulloo.info/Examples/Marvin/Introduction/) language
