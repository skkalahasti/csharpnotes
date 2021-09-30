* Derived classes cannot have greater accessibility than their base types. In other words, you cannot have a public class B that derives from an internal class A. 
  If this were allowed, it would have the effect of making A public, because all protected or internal members of A are accessible from the derived class.

* Namespace vs Assembly

    * Namespace is a logical grouping of classes belongs to same functionality.

    * Assembly is chunk of (precompiled) code that can be executed by the .NET runtime environment. It contains one or more than one Namespaces. A .NET program consists of one or 
      more assemblies. 
      
          System.Web.dll and System.Data.dll are assemblies.

*  Variou access modifiers

| Caller's location                      | `public` | `protected internal` | `protected` | `internal` | `private protected` | `private` |
| -------------------------------------- | :------: | :------------------: | :---------: | :--------: | :-----------------: | :-------: |
| Within the class                       |   ✔️️   |         ✔️         |     ✔️     |     ✔️     |         ✔️          |    ✔️     |
| Derived class (same assembly)          |   ✔️   |         ✔️         |     ✔️     |     ✔️     |         ✔️          |    ❌     |
| Non-derived class (same assembly)      |   ✔️   |         ✔️         |     ❌     |     ✔️     |         ❌          |    ❌     |
| Derived class (different assembly)     |   ✔️   |         ✔️         |     ✔️     |     ❌     |         ❌          |    ❌     |
| Non-derived class (different assembly) |   ✔️   |         ❌         |     ❌     |     ❌     |         ❌          |    ❌     |

* The default access for everything in C# is "the most restricted access you could declare for that member".
   ```
      top level class: internal
      method: private
      members (unless an interface or enum): private (including nested classes)
      members (of interface or enum): public
      constructor: private (note that if no constructor is explicitly defined, a public default constructor will be automatically defined)
      delegate: internal
      interface: internal
      explicitly implemented interface member: public!
   ```
    * As stated in the above code, if we dont specify an accessor for constructor it will be private by default

* Private constructors
  A private constructor is a special instance constructor. It is generally used in classes that contain static members only. If a class has one or more private constructors and   no public constructors, other classes (except nested classes) cannot create instances of this class.
* Static Constructors
  A static constructor is used to initialize any static data, or to perform a particular action that needs to be performed only once. It is called automatically before the first   instance is created or any static members are referenced. 
    * A static constructor doesn't take access modifiers or have parameters.
    * A class or struct can only have one static constructor.
    * Static constructors cannot be inherited or overloaded.
    * A field declared as static readonly may only be assigned as part of its declaration or in a static constructor. When an explicit static constructor isn't required,
      initialize static fields at declaration rather than through a static constructor for better runtime optimization.

* Implicitly-typed variables must be initialized at the time of declaration; otherwise C# compiler would give an error: Implicitly-typed variables must be initialized.
* var i = 100, j = 200, k = 300; // Error: cannot declare var variables in a single statement
* ```
   void Display(var param) //Compile-time error
  {
      Console.Write(param);
  }
  ```
* Values types vs Reference types
  ![image](https://user-images.githubusercontent.com/30605841/135105218-960ae45d-6814-4cba-bad8-050d21317d01.png)

* Prefixing the string with an @ indicates that it should be treated as a literal and should not escape any character.
  ```
  string text = @"This is a "string." in C#."; // error
  string text = @"This is a \"string\" in C#."; // error
  string text = "This is a \"string\" in C#."; // valid
  ```
* If you declare a variable of struct type without using new keyword, it does not call any constructor, so all the members remain unassigned. Therefore, you must assign values     to each member before accessing them, otherwise, it will give a compile-time error.
  ```
  struct Coordinate
  {
      public int x;
      public int y;
  }

  Coordinate point;
  Console.Write(point.x); // Compile time error  

  point.x = 10;
  point.y = 20;
  Console.Write(point.x); //output: 10  
  Console.Write(point.y); //output: 20  
  ```
* A struct cannot contain a parameterless constructor. It can only contain parameterized constructors or a static constructor.
* ```
  struct can include constructors, constants, fields, methods, properties, indexers, operators, events & nested types.
  struct cannot include a parameterless constructor or a destructor.
  struct can implement interfaces, same as class.
  struct cannot inherit another structure or class, and it cannot be the base of a class.
  struct members cannot be specified as abstract, sealed, virtual, or protected.
  ```
* Classes vs Structs
  * Structs are value types
    * Structs are assigned memory as below: So this would mean they cannot have cyclic dependencies.
      ![image](https://user-images.githubusercontent.com/30605841/135491199-dfb8a71d-9177-4e1d-ac68-ca081d39100d.png)
      
      Below code would result in compile time error.
        ```
        struct Node
        {
            int data;
            Node next; // error, Node directly depends on itself
        }
        ```
    * Assignment to a variable of a struct type creates a copy of the value being assigned
    * Inheritence is not allowed in structs:
      ```
      Consider the following class declarations with inheritance:

      class C1 {
        int x;
      }
      class C2 : C1 {
        int y;
      }
      Unsurprisingly, this works:

      C1 baseTypeVariable = new C2();
      Because C1 is a reference type, the compiler can statically deduce the size of the storage location baseTypeVariable (it simply has the size of a reference). The reference 
      is then set to point to an instance of C2, which is definitely larger than C1 because it contains an additional field. On the other hand, the following is illegal in C#:

      struct S1 {
        int x;
      }
      struct S2 : S1 {
        int y;
      }
      If it were legal C#, then users might expect to be able to write something like:

      S1 baseTypeVariable = new S2();
      The mere declaration S1 baseTypeVariable asks the compiler to allocate enough space for a value of type S1 – and this size is just the size of an int by definition of S1.
      Assigning a value of type S2 to that variable poses a problem, because S2 values use up more memory.
      ```
    * Boxing of value types
      ```
      int x = 42;
      object a = x; // value of x is copied into box of a along with meta information of x type, so equals calls appropriate int equals
      x++;
      a.Equals(x); // this evaluates to false, since x is 43, but x contains 42.
      ```
      ```
      using System;

      interface ICounter
      {
          void Increment();
      }

      struct Counter: ICounter
      {
          int value;

          public override string ToString() {
              return value.ToString();
          }

          void ICounter.Increment() {
              value++;
          }
      }

      class Program
      {
          static void Test<T>() where T: ICounter, new() {
              T x = new T();
              Console.WriteLine(x);
              x.Increment();                    // Modify x
              Console.WriteLine(x);
              ((ICounter)x).Increment();        // Modify boxed copy of x, so this would not affect the struct x and struct x still retains x value as 1
              Console.WriteLine(x);
          }

          static void Main() {
              Test<Counter>();
          }
      }
      
      // Output is
      0
      1
      1
      ```
    * 
      

  
  
