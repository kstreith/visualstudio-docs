---
title: "CA1406: Avoid Int64 arguments for Visual Basic 6 clients"
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
  - "AvoidInt64ArgumentsForVB6Clients"
  - "CA1406"
helpviewer_keywords:
  - "AvoidInt64ArgumentsForVB6Clients"
  - "CA1406"
ms.assetid: d5d0d3fc-f105-43da-be5b-923ab023309c
author: gewarren
ms.author: gewarren
manager: jillfra
dev_langs:
 - CSharp
 - VB
ms.workload:
  - "multiple"
---
# CA1406: Avoid Int64 arguments for Visual Basic 6 clients

|||
|-|-|
|TypeName|AvoidInt64ArgumentsForVB6Clients|
|CheckId|CA1406|
|Category|Microsoft.Interoperability|
|Breaking change|Breaking|

## Cause
A type that is specifically marked as visible to Component Object Model (COM) declares a member that takes a <xref:System.Int64?displayProperty=fullName> argument.

## Rule description
Visual Basic 6 COM clients cannot access 64-bit integers.

By default, the following are visible to COM: assemblies, public types, public instance members in public types, and all members of public value types. However, to reduce false positives, this rule requires the COM visibility of the type to be explicitly stated; the containing assembly must be marked with the <xref:System.Runtime.InteropServices.ComVisibleAttribute?displayProperty=fullName> set to `false` and the type must be marked with the <xref:System.Runtime.InteropServices.ComVisibleAttribute> set to `true`.

## How to fix violations
To fix a violation of this rule for a parameter whose value can always be expressed as a 32-bit integral, change the parameter type to <xref:System.Int32?displayProperty=fullName>. If the value of the parameter might be larger than can be expressed as a 32-bit integral, change the parameter type to <xref:System.Decimal?displayProperty=fullName>. Note that both <xref:System.Single?displayProperty=fullName> and <xref:System.Double?displayProperty=fullName> lose precision at the upper ranges of the <xref:System.Int64> data type. If the member is not meant to be visible to COM, mark it with the <xref:System.Runtime.InteropServices.ComVisibleAttribute> set to `false`.

## When to suppress warnings
It is safe to suppress a warning from this rule if it is certain that Visual Basic 6 COM clients will not access the type.

## Example
The following example shows a type that violates the rule.

[!code-csharp[FxCop.Interoperability.LongArgument#1](../code-quality/codesnippet/CSharp/ca1406-avoid-int64-arguments-for-visual-basic-6-clients_1.cs)]
[!code-vb[FxCop.Interoperability.LongArgument#1](../code-quality/codesnippet/VisualBasic/ca1406-avoid-int64-arguments-for-visual-basic-6-clients_1.vb)]

## Related rules
[CA1413: Avoid non-public fields in COM visible value types](../code-quality/ca1413-avoid-non-public-fields-in-com-visible-value-types.md)

[CA1407: Avoid static members in COM visible types](../code-quality/ca1407-avoid-static-members-in-com-visible-types.md)

[CA1017: Mark assemblies with ComVisibleAttribute](../code-quality/ca1017-mark-assemblies-with-comvisibleattribute.md)

## See also

- [Interoperating with Unmanaged Code](/dotnet/framework/interop/index)
- [Long Data Type](/dotnet/visual-basic/language-reference/data-types/long-data-type)