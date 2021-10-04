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
  *  Boxing of value types
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
      
* Enums:
  Enum can be of any numeric data type such as byte, sbyte, short, ushort, int, uint, long, or ulong. However, an enum cannot be a string type.
  enum is an abstract class

  Example:
  ```
   enum Categories: byte
    {
        Electronics = 1,  
        Food = 5, 
        Automotive = 6, 
        Arts = 10, 
        BeautyCare = 11, 
        Fashion = 15
    }
  ```
      
* In C# the string type is immutable whereas StringBuilder dynamically expands its memory.
* Anonymous type is a type (class) without any name that can contain public read-only properties only. It cannot contain other members, such as fields, methods, events, etc.
   * The properties of anonymous types are read-only
   * An anonymous type will always be local to the method where it is defined. It cannot be returned from the method. However, an anonymous type can be passed to the method          as object type parameter, but it is not recommended. If you need to pass it to another method, then use struct or class instead of an anonymous type.
 
* Dynamic are types that avoids compile-time type checking. A dynamic type escapes type checking at compile-time; instead, it resolves type at run time.
  * Dynamic types change types at run-time based on the assigned value. The following example shows how a dynamic variable changes type based on assigned value.
  * ```
    dynamic MyDynamicVar = 100;
    Console.WriteLine("Value: {0}, Type: {1}", MyDynamicVar, MyDynamicVar.GetType());

    MyDynamicVar = "Hello World!!";
    Console.WriteLine("Value: {0}, Type: {1}", MyDynamicVar, MyDynamicVar.GetType());

    The above code would output:
    Value: 100, Type: System.Int32
    Value: Hello World!!, Type: System.String
    ```
* Nullable Types:
  `Nullable<int> i = null;`
  Short hand syntax is: `int? i = null;`

* Interfaces
   * You cannot apply access modifiers to interface members. All the members are public by default. If you use an access modifier in an interface, then the C# compiler will give 
     a compile-time error "The modifier 'public/private/protected' is not valid for this item.".
   * Interface can still contain `const` fields and also default implementations;
   * Implemented classes know nothing about the default methods
     ```
     public interface ITest {
	
        public const string name = "interface";

        public void DoSomething(string tex)
        {
          Console.WriteLine($"Done interface: {tex} : {name}");
        }
      }

      public class Test : ITest {

        public string name = "class";
      }
      
       Test t = new Test();
       t.DoSomething("yoo"); // Error Class1 does not know about default implementation of ITes

       ITest t1 = new Test();
       t1.DoSomething("yoo"); // 
     ```
    * If there is an implementation of default method, interface reference still executes the implemented method rather than default method
       ```
       public interface ITest {
	
          public const string name = "interface";

          public void DoSomething(string tex)
          {
            Console.WriteLine($"Done interface: {tex} : {name}");
          }
        }

        public class Test : ITest {

          public string name = "class";

          public void DoSomething(string tex)
          {
            Console.WriteLine($"Done class: {tex} : {name}");
          }
        }
        
            Test t = new Test();
            t.DoSomething("yoo");

            ITest t1 = new Test();
            t1.DoSomething("yoo");
            
            // Output is:
            Done class: yoo : class
            Done class: yoo : class
       ```
     * Constant fields can be accessed outside the interface only with its reference: `Interface.constant`
* Run time polymorphism:
	```
	public class Class1 {
	
		public string name = "class1";

		public virtual void DoSomething()
		{
			Console.WriteLine($"In class1 & name: {name}");
		}
	}

	public class Class2 : Class1 {

		public string name = "class2";


		public override void DoSomething()
		{
			Console.WriteLine($"In class2 & name: {name}");
		}
	}
	
	Class1 class1 = new Class1();
	class1.DoSomething();
	Console.WriteLine(class1.name);

	Class2 class2 = new Class2();
	class2.DoSomething();	
	Console.WriteLine(class2.name);

	Class1 hybrid = new Class2();
	hybrid.DoSomething();
	Console.WriteLine(hybrid.name);
	
	Output is: 
	In class1 & name: class1
	class1
	In class2 & name: class2
	class2
	In class2 & name: class2
	class1
	```
* Partial Classes
   * Multiple Developer Using Partial Classes multiple developer can work on the same class easily.
   * Code Generator Partial classes are mainly used by code generator to keep different concerns separate
   * Partial Methods Using Partial Classes you can also define Partial methods as well where a developer can simply define the method and the other developer can implement that.
   * Partial Method Declaration only Even the code get compiled with method declaration only and if the implementation of the method isn't present compiler can safely remove 
     that piece of code and no compile time error will occur.
