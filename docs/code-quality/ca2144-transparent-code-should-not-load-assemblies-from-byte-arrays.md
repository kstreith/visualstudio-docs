---
title: "CA2144: Transparent code should not load assemblies from byte arrays"
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
  - "CA2144"
ms.assetid: 777b1310-6e16-4413-b4ee-5f3136304f82
author: gewarren
ms.author: gewarren
manager: jillfra
ms.workload:
  - "multiple"
---
# CA2144: Transparent code should not load assemblies from byte arrays

|||
|-|-|
|TypeName|TransparentMethodsShouldNotLoadAssembliesFromByteArrays|
|CheckId|CA2144|
|Category|Microsoft.Security|
|Breaking change|Breaking|

## Cause
A transparent method loads an assembly from a byte array using one of the following methods:

- <xref:System.Reflection.Assembly.Load%2A>

- <xref:System.Reflection.Assembly.Load%2A>

- <xref:System.Reflection.Assembly.Load%2A>

## Rule description
The security review for transparent code is not as thorough as the security review for critical code, because transparent code cannot perform security sensitive actions. Assemblies loaded from a byte array might not be noticed in transparent code, and that byte array might contain critical, or more importantly safe-critical code, that does need to be audited. Therefore, transparent code should not load assemblies from a byte array.

## How to fix violations
To fix a violation of this rule, mark the method that is loading the assembly with the <xref:System.Security.SecurityCriticalAttribute> or the <xref:System.Security.SecuritySafeCriticalAttribute> attribute.

## When to suppress warnings
Do not suppress a warning from this rule.

## Example
The rule fires on the following code because a transparent method loads an assembly from a byte array.

[!code-csharp[FxCop.Security.CA2144.TransparentMethodsShouldNotLoadAssembliesFromByteArrays#1](../code-quality/codesnippet/CSharp/ca2144-transparent-code-should-not-load-assemblies-from-byte-arrays_1.cs)]