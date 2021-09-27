* Derived classes cannot have greater accessibility than their base types. In other words, you cannot have a public class B that derives from an internal class A. 
  If this were allowed, it would have the effect of making A public, because all protected or internal members of A are accessible from the derived class.

* Namespace vs Assembly

    * Namespace is a logical grouping of classes belongs to same functionality.

    * Assembly is chunk of (precompiled) code that can be executed by the .NET runtime environment. It contains one or more than one Namespaces. A .NET program consists of one or 
      more assemblies. 
      
          System.Web.dll and System.Data.dll are assemblies.

* 
