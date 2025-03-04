---
title: "CA2003: Do not treat fibers as threads"
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
  - "CA2003"
  - "DoNotTreatFibersAsThreads"
helpviewer_keywords:
  - "CA2003"
  - "DoNotTreatFibersAsThreads"
ms.assetid: 15398fb1-f384-4bcc-ad93-00e1c0fa9ddf
author: gewarren
ms.author: gewarren
manager: jillfra
ms.workload:
  - "multiple"
---
# CA2003: Do not treat fibers as threads

|||
|-|-|
|TypeName|DoNotTreatFibersAsThreads|
|CheckId|CA2003|
|Category|Microsoft.Reliability|
|Breaking change|Non-breaking|

## Cause

A managed thread is being treated as a Win32 thread.

## Rule description

Do not assume a managed thread is a Win32 thread; it's a fiber. The common language runtime (CLR) runs managed threads as fibers in the context of real threads that are owned by SQL. These threads can be shared across AppDomains and even databases in the SQL Server process. Using managed thread local storage works, but you may not use unmanaged thread local storage or assume that your code will run on the current OS thread again. Do not change settings such as the locale of the thread. Do not call CreateCriticalSection or CreateMutex via P/Invoke because they require that the thread that enters a lock must also exit the lock. Because the thread that enters a lock doesn't exit a lock when you use fibers, Win32 critical sections and mutexes are useless in SQL. You may safely use most of the state on a managed <xref:System.Threading.Thread> object, including managed thread local storage and the current user interface (UI) culture of the thread. However, for programming model reasons, you won't be able to change the current culture of a thread when you use SQL. This limitation will be enforced through a new permission.

## How to fix violations

Examine your usage of threads and change your code accordingly.

## When to suppress warnings

Do not suppress this rule.