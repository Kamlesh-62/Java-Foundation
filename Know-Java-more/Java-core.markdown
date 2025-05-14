## Signature

### 🧾 What Is a Method Signature in Java?
- The method signature is the unique identifier of a method within a class.

### ❌ Not Part of the Method Signature

| **Element**       | **Included in Signature?** | **Why Not?**                                                       |
|-------------------|----------------------------|---------------------------------------------------------------------|
| Method name        | ✅ Yes                     | Core identifier for distinguishing methods                          |
| Parameter types    | ✅ Yes                     | Determines overload uniqueness                                      |
| Return type        | ❌ No                      | Can't overload on return type alone                                |
| Parameter names    | ❌ No                      | Used for readability, not method resolution                         |
| `throws` clause    | ❌ No                      | Not considered part of the method signature for overloading rules   |

