---
title: Understanding Cancellation Tokens in .NET
description: Understanding how **cancellation token** work (it is a mechanism provided by .NET to signal and handle the cancellation of asynchronous or long-running operations).
author: timo
date: 2024-12-01 11:00:00 +0000
categories: [.NET, cancellation tokens]
tags: [cancellation tokens]
showLastMod: false
---

In modern .NET applications, managing asynchronous operations efficiently is crucial. One of the most powerful tools for handling long-running or potentially unresponsive tasks is the **cancellation token**. This post will explore what cancellation tokens are, how they work, and how to use them effectively in your .NET applications.

## What Are Cancellation Tokens?

A **cancellation token** is a mechanism provided by .NET to signal and handle the cancellation of asynchronous or long-running operations. It is part of the `System.Threading` namespace and works in conjunction with the `CancellationToken` and `CancellationTokenSource` classes.

Cancellation tokens allow developers to gracefully terminate tasks, freeing up resources and improving application responsiveness.

## How Do Cancellation Tokens Work?

Cancellation tokens rely on two key components:

1. **CancellationTokenSource**: This is the producer of the cancellation signal. It provides a `CancellationToken` that can be passed to tasks or methods.
2. **CancellationToken**: This is the consumer of the cancellation signal. It is used by tasks or methods to check if a cancellation request has been made.

When a cancellation is requested via the `CancellationTokenSource`, all tasks or methods observing the associated `CancellationToken` can respond accordingly.

## Using Cancellation Tokens in Practice

Here’s a simple example of how to use cancellation tokens in a .NET application:

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        using var cts = new CancellationTokenSource();

        // Simulate user cancellation after 3 seconds
        Task.Run(() =>
        {
            Thread.Sleep(3000);
            cts.Cancel();
        });

        try
        {
            await PerformLongRunningOperationAsync(cts.Token);
        }
        catch (OperationCanceledException)
        {
            Console.WriteLine("Operation was canceled.");
        }
    }

    static async Task PerformLongRunningOperationAsync(CancellationToken token)
    {
        for (int i = 0; i < 10; i++)
        {
            token.ThrowIfCancellationRequested();
            Console.WriteLine($"Processing step {i + 1}");
            await Task.Delay(1000); // Simulate work
        }

        Console.WriteLine("Operation completed successfully.");
    }
}
```

Explanation of the Code:

1. ***CancellationTokenSource:*** We create a CancellationTokenSource to manage the cancellation signal.
2. ***Simulating Cancellation:*** A separate task simulates a user-triggered cancellation after 3 seconds. 
3. **T*hrowIfCancellationRequested:*** The ThrowIfCancellationRequested method checks if a cancellation has been requested and throws an OperationCanceledException if so.
4. ***Graceful Handling:*** The try-catch block ensures that the cancellation is handled gracefully.

## Best Practices for Using Cancellation Tokens
1. ***Always Check for Cancellation:*** Use ThrowIfCancellationRequested or check the IsCancellationRequested property regularly in long-running tasks.
2. ***Dispose of CancellationTokenSource:*** Always dispose of the CancellationTokenSource to release resources.
3. ***Avoid Ignoring Cancellation:*** Ensure that your code respects cancellation requests to avoid resource leaks or unresponsive applications.

## Conclusion
Cancellation tokens are an essential tool for building responsive and resource-efficient .NET applications. By incorporating them into your asynchronous workflows, you can ensure that your application remains robust and user-friendly, even under heavy workloads or unexpected interruptions.

Start using cancellation tokens today to take full control of your asynchronous operations! 
