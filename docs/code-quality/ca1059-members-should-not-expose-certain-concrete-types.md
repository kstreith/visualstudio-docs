---
title: "CA1059: Members should not expose certain concrete types"
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
  - "CA1059"
  - "MembersShouldNotExposeCertainConcreteTypes"
helpviewer_keywords:
  - "MembersShouldNotExposeCertainConcreteTypes"
  - "CA1059"
ms.assetid: 59f61f52-8d6c-49cb-aefb-191910523a3c
author: gewarren
ms.author: gewarren
manager: jillfra
ms.workload:
  - "multiple"
---
# CA1059: Members should not expose certain concrete types

|||
|-|-|
|TypeName|MembersShouldNotExposeCertainConcreteTypes|
|CheckId|CA1059|
|Category|Microsoft.Design|
|Breaking change|Breaking|

## Cause
An externally visible member is a certain concrete type or exposes certain concrete types through one of its parameters or return value. Currently, this rule reports exposure of the following concrete types:

- A type derived from <xref:System.Xml.XmlNode?displayProperty=fullName>.

## Rule description
A concrete type is a type that has a complete implementation and therefore can be instantiated. To allow widespread use of the member, replace the concrete type with the suggested interface. This allows the member to accept any type that implements the interface or be used where a type that implements the interface is expected.

The following table lists the targeted concrete types and their suggested replacements.

|Concrete type|Replacement|
|-------------------|-----------------|
|<xref:System.Xml.XPath.XPathDocument>|<xref:System.Xml.XPath.IXPathNavigable?displayProperty=fullName>.<br /><br /> Using the interface decouples the member from a specific implementation of an XML data source.|

## How to fix violations
To fix a violation of this rule, change the concrete type to the suggested interface.

## When to suppress warnings
It is safe to suppress a message from this rule if the specific functionality provided by the concrete type is required.

## Related rules
[CA1011: Consider passing base types as parameters](../code-quality/ca1011-consider-passing-base-types-as-parameters.md)