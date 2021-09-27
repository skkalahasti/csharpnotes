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
