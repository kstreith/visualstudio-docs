---
title: "CA1013: Overload operator equals on overloading add and subtract"
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
  - "OverrideOperatorEqualsOnOverridingAddAndSubtract"
  - "OverrideOperatorEqualsOnOverloadingAddAndSubtract"
  - "CA1013"
  - "OverloadOperatorEqualsOnOverloadingAddAndSubtract"
helpviewer_keywords:
  - "OverrideOperatorEqualsOnOverloadingAddAndSubtract"
  - "OverrideOperatorEqualsOnOverridingAddAndSubtract"
  - "CA1013"
  - "OverloadOperatorEqualsOnOverloadingAddAndSubtract"
ms.assetid: 5bd28d68-c179-49ff-af47-5250b8b18a10
author: gewarren
ms.author: gewarren
manager: jillfra
dev_langs:
 - CSharp
 - VB
ms.workload:
  - "multiple"
---
# CA1013: Overload operator equals on overloading add and subtract

|||
|-|-|
|TypeName|OverloadOperatorEqualsOnOverloadingAddAndSubtract|
|CheckId|CA1013|
|Category|Microsoft.Design|
|Breaking change|Non-breaking|

## Cause
A public or protected type implements the addition or subtraction operators without implementing the equality operator.

## Rule description
When instances of a type can be combined by using operations such as addition and subtraction, you should almost always define equality to return `true` for any two instances that have the same constituent values.

You cannot use the default equality operator in an overloaded implementation of the equality operator. Doing so will cause a stack overflow. To implement the equality operator, use the Object.Equals method in your implementation. See the following example.

```vb
If (Object.ReferenceEquals(left, Nothing)) Then
    Return Object.ReferenceEquals(right, Nothing)
Else
    Return left.Equals(right)
End If
```

```csharp
if (Object.ReferenceEquals(left, null))
    return Object.ReferenceEquals(right, null);
return left.Equals(right);
```

## How to fix violations
To fix a violation of this rule, implement the equality operator so that it is mathematically consistent with the addition and subtraction operators.

## When to suppress warnings
It is safe to suppress a warning from this rule when the default implementation of the equality operator provides the correct behavior for the type.

## Example
The following example defines a type (`BadAddableType`) that violates this rule. This type should implement the equality operator to make any two instances that have the same field values test `true` for equality. The type `GoodAddableType` shows the corrected implementation. Note that this type also implements the inequality operator and overrides <xref:System.Object.Equals%2A> to satisfy other rules. A complete implementation would also implement <xref:System.Object.GetHashCode%2A>.

[!code-csharp[FxCop.Design.AddAndSubtract#1](../code-quality/codesnippet/CSharp/ca1013-overload-operator-equals-on-overloading-add-and-subtract_1.cs)]

## Example
The following example tests for equality by using instances of the types that were previously defined in this topic to illustrate the default and correct behavior for the equality operator.

[!code-csharp[FxCop.Design.TestAddAndSubtract#1](../code-quality/codesnippet/CSharp/ca1013-overload-operator-equals-on-overloading-add-and-subtract_2.cs)]

This example produces the following output:

```txt
Bad type:  {2,2} {2,2} are equal? No
Good type: {3,3} {3,3} are equal? Yes
Good type: {3,3} {3,3} are == ?   Yes
Bad type:  {2,2} {9,9} are equal? No
Good type: {3,3} {9,9} are == ?   No
```

## See also

- [Equality Operators](/dotnet/standard/design-guidelines/equality-operators)