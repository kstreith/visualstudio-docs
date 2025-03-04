---
title: "CA1407: Avoid static members in COM visible types"
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
  - "CA1407"
  - "AvoidStaticMembersInComVisibleTypes"
helpviewer_keywords:
  - "CA1407"
  - "AvoidStaticMembersInComVisibleTypes"
ms.assetid: bebd0776-ad04-453c-bca8-8c124c2d7840
author: gewarren
ms.author: gewarren
manager: jillfra
ms.workload:
  - "multiple"
---
# CA1407: Avoid static members in COM visible types

|||
|-|-|
|TypeName|AvoidStaticMembersInComVisibleTypes|
|CheckId|CA1407|
|Category|Microsoft.Interoperability|
|Breaking change|Non-breaking|

## Cause
A type that is specifically marked as visible to Component Object Model (COM) contains a `public``static` method.

## Rule description
COM does not support `static` methods.

This rule ignores property and event accessors, operator overloading methods, or methods that are marked by using either the <xref:System.Runtime.InteropServices.ComRegisterFunctionAttribute?displayProperty=fullName> attribute or the <xref:System.Runtime.InteropServices.ComUnregisterFunctionAttribute?displayProperty=fullName> attribute.

By default, the following are visible to COM: assemblies, public types, public instance members in public types, and all members of public value types.

For this rule to occur, an assembly-level <xref:System.Runtime.InteropServices.ComVisibleAttribute> must be set to `false` and the class- <xref:System.Runtime.InteropServices.ComVisibleAttribute> must be set to `true`, as the following code shows.

```csharp
using System;
using System.Runtime.InteropServices;

[assembly: ComVisible(false)]
namespace Samples
{
    [ComVisible(true)]
    public class MyClass
    {
        public static void DoSomething()
        {
        }
    }
}
```

## How to fix violations
To fix a violation of this rule, change the design to use an instance method that provides the same functionality as the `static` method.

## When to suppress warnings
It is safe to suppress a warning from this rule if a COM client does not require access to the functionality that is provided by the `static` method.

## Example Violation

### Description
The following example shows a `static` method that violates this rule.

### Code
[!code-csharp[FxCop.Interoperability.ComVisibleStaticMembersViolation#1](../code-quality/codesnippet/CSharp/ca1407-avoid-static-members-in-com-visible-types_1.cs)]

### Comments
In this example, the **Book.FromPages** method cannot be called from COM.

## Example Fix

### Description
To fix the violation in the previous example, you could change the method to an instance method, but that does not make sense in this instance. A better solution is to explicitly apply `ComVisible(false)` to the method to make it clear to other developers that the method cannot be seen from COM.

The following example applies <xref:System.Runtime.InteropServices.ComRegisterFunctionAttribute> to the method.

### Code
[!code-csharp[FxCop.Interoperability.ComVisibleStaticMembersFixed#1](../code-quality/codesnippet/CSharp/ca1407-avoid-static-members-in-com-visible-types_2.cs)]

## Related rules
[CA1017: Mark assemblies with ComVisibleAttribute](../code-quality/ca1017-mark-assemblies-with-comvisibleattribute.md)

[CA1406: Avoid Int64 arguments for Visual Basic 6 clients](../code-quality/ca1406-avoid-int64-arguments-for-visual-basic-6-clients.md)

[CA1413: Avoid non-public fields in COM visible value types](../code-quality/ca1413-avoid-non-public-fields-in-com-visible-value-types.md)

## See also
[Interoperating with Unmanaged Code](/dotnet/framework/interop/index)