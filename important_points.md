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
