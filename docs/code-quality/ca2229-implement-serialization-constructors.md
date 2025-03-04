---
title: "CA2229: Implement serialization constructors"
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
  - "CA2229"
  - "ImplementSerializationConstructors"
helpviewer_keywords:
  - "CA2229"
  - "ImplementSerializationConstructors"
ms.assetid: 8e04d5fe-dfad-445a-972e-0648324fac45
author: gewarren
ms.author: gewarren
manager: jillfra
ms.workload:
  - "multiple"
---
# CA2229: Implement serialization constructors

|||
|-|-|
|TypeName|ImplementSerializationConstructors|
|CheckId|CA2229|
|Category|Microsoft.Usage|
|Breaking change|Non-breaking|

## Cause
The type implements the <xref:System.Runtime.Serialization.ISerializable?displayProperty=fullName> interface, is not a delegate or interface, and one of the following conditions is true:

- The type does not have a constructor that takes a <xref:System.Runtime.Serialization.SerializationInfo> object and a <xref:System.Runtime.Serialization.StreamingContext> object (the signature of the serialization constructor).

- The type is unsealed and the access modifier for its serialization constructor is not protected (family).

- The type is sealed and the access modifier for its serialization constructor is not private.

## Rule description

This rule is relevant for types that support custom serialization. A type supports custom serialization if it implements the <xref:System.Runtime.Serialization.ISerializable> interface. The serialization constructor is required to deserialize, or recreate, objects that have been serialized using the <xref:System.Runtime.Serialization.ISerializable.GetObjectData%2A?displayProperty=nameWithType> method.

## How to fix violations

To fix a violation of this rule, implement the serialization constructor. For a sealed class, make the constructor private; otherwise, make it protected.

## When to suppress warnings

Do not suppress a violation of the rule. The type will not be deserializable, and will not function in many scenarios.

## Example

The following example shows a type that satisfies the rule.

[!code-csharp[FxCop.Usage.ISerializableCtor#1](../code-quality/codesnippet/CSharp/ca2229-implement-serialization-constructors_1.cs)]

## Related rules

[CA2237: Mark ISerializable types with SerializableAttribute](../code-quality/ca2237-mark-iserializable-types-with-serializableattribute.md)

## See also

- <xref:System.Runtime.Serialization.ISerializable?displayProperty=fullName>
- <xref:System.Runtime.Serialization.SerializationInfo?displayProperty=fullName>
- <xref:System.Runtime.Serialization.StreamingContext?displayProperty=fullName>
