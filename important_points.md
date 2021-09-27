- Derived classes cannot have greater accessibility than their base types. In other words, you cannot have a public class B that derives from an internal class A. 
  If this were allowed, it would have the effect of making A public, because all protected or internal members of A are accessible from the derived class.
