---
title: "CA2139: Transparent methods may not use the HandleProcessCorruptingExceptions attribute"
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
  - "CA2139"
ms.assetid: 45a0328a-add7-40f9-8934-dff59beb02b3
author: gewarren
ms.author: gewarren
manager: jillfra
ms.workload:
  - "multiple"
---
# CA2139: Transparent methods may not use the HandleProcessCorruptingExceptions attribute

|||
|-|-|
|TypeName|TransparentMethodsMustNotHandleProcessCorruptingExceptions|
|CheckId|CA2139|
|Category|Microsoft.Security|
|Breaking change|Breaking|

## Cause
A transparent method is marked with the <xref:System.Runtime.ExceptionServices.HandleProcessCorruptedStateExceptionsAttribute> attribute.

## Rule description
This rule fires any method which is transparent and attempts to handle a process corrupting exception by using the <xref:System.Runtime.ExceptionServices.HandleProcessCorruptedStateExceptionsAttribute> attribute. A process corrupting exception is a CLR version 4.0 exception classification of exceptions such <xref:System.AccessViolationException>. The HandleProcessCorruptedStateExceptionsAttribute attribute may only be used by security critical methods, and will be ignored if it is applied to a transparent method. To handle process corrupting exceptions, this method must become security critical or security safe-critical.

## How to fix violations
To fix a violation of this rule, remove the <xref:System.Runtime.ExceptionServices.HandleProcessCorruptedStateExceptionsAttribute> attribute, or mark the method with the <xref:System.Security.SecurityCriticalAttribute> or the <xref:System.Security.SecuritySafeCriticalAttribute> attribute.

## When to suppress warnings
Do not suppress a warning from this rule.

## Example
In this example, a transparent method is marked with the <xref:System.Runtime.ExceptionServices.HandleProcessCorruptedStateExceptionsAttribute> attribute and will fail the rule. The method should also be marked with the <xref:System.Security.SecurityCriticalAttribute> or the <xref:System.Security.SecuritySafeCriticalAttribute> attribute.

[!code-csharp[FxCop.Security.CA2139.TransparentMethodsMustNotHandleProcessCorruptingExceptions#1](../code-quality/codesnippet/CSharp/ca2139-transparent-methods-may-not-use-the-handleprocesscorruptingexceptions-attribute_1.cs)]