+++
title = "Early Return Pattern"
date = 2024-12-31
description = "Example post showing callouts"
[taxonomies]
tags = ["coding", "learning"]
[extra]
toc = true
+++

{% note(title="Note") %}
Happy New Year 2025, everyone!
I wrote this on December 31st, 2024, but hit publish on January 1st — so yeah, a bit of a time-warped message. Still, wishing you all the best for the year ahead!
{% end %}

# Background

When writing code, especially as projects grow bigger, it can become easy to fall into a trap of complex, nested logic. As you try to handle various conditions — such as errors or edge cases — you might find yourself writing deeply nested if statements that make the code harder to read and maintain. This is where a technique called early return can help.

In programming, early return allows us to handle edge cases or errors right away, making the "happy path" (the ideal path where everything works as expected) clearer and the code easier to follow. By exiting a function as soon as something goes wrong, we avoid deep nesting and focus the main logic of the function where it belongs.

In this article, I’ll talk about early return with a bit of a story-like approach, comparing how functions behave when written with and without it, and explaining why early return can be a good practice to improve readability and maintainability.

# The Problem

Imagine you're writing a function to validate someone’s age before proceeding with further logic, like processing a form submission. In the first example, we’ll write it without early return, using nested if statements to handle each edge case.

Here’s what that might look like:

```go
func processAge(age int) error {
    if age >= 0 {
        if age >= 18 {
            if age <= 100 {
                // Valid age range, process further
                // More processing here...
                fmt.Println("Age is valid and processed.")
                return nil
            } else {
                // Age is greater than 100
                return fmt.Errorf("age cannot be greater than 100")
            }
        } else {
            // Age is less than 18
            return fmt.Errorf("age must be at least 18")
        }
    } else {
        // Age is negative
        return fmt.Errorf("age cannot be negative")
    }
}
```

Now, let’s break this down:

1. First, we check if the age is a valid number (`age >= 0`).
2. If the age is valid, we check if it’s greater than or equal to 18.
3. Then, we check if the age is less than or equal to 100.

As you can see, we’re nesting one if statement inside another. This makes the function harder to read, especially as it gets longer. The actual logic — where the age is valid and processed — is buried under multiple checks, and the happy path (when everything is fine) is not immediately obvious.

# The Solution

Now let’s try the same function, but this time we’ll use early return. The idea is simple: check for edge cases or errors right at the start and return immediately if something is wrong. By doing this, we avoid nesting and make the happy path much clearer.

Here’s the refactored version using early returns:

```go
func processAge(age int) error {
    if age < 0 {
        return fmt.Errorf("age cannot be negative")
    }
    if age < 18 {
        return fmt.Errorf("age must be at least 18")
    }
    if age > 100 {
        return fmt.Errorf("age cannot be greater than 100")
    }

    // Valid age range, process further
    // More processing here...
    fmt.Println("Age is valid and processed.")
    return nil
}
```

In this version:

1. We immediately check if the age is invalid and return an error right away if it is.
2. If none of those conditions are met, we proceed with the happy path — where the age is valid and everything is processed.
3. The main logic, which is simple and clean, is now easy to follow because it comes right after the error checks.

By returning early, we’ve reduced the nesting and made the happy path much clearer. It’s immediately obvious that if the age is valid, we process it; otherwise, we exit early with an error.

# Rationale

Now, let’s talk about some deeper concepts that early return touches on: the happy path and error handling strategies.

The happy path refers to the scenario where everything works as expected — the perfect scenario. In our example, the happy path is when the age is between 18 and 100. That’s when the function proceeds with its main logic.

In the nested version of the function, the happy path is hidden under multiple layers of conditions. We have to go through all the checks before even seeing the part where the age is valid and processed. Using early return brings the happy path to the forefront, making it easier to understand what the function is really doing.

Good error handling is about catching problems as soon as they happen. Early return is a great way to do this: we handle errors right away, which makes it clear that something went wrong and the function can’t continue. This is a good practice because it prevents us from working with invalid data or running unnecessary logic when an error has already occurred.

In our nested version, error handling is mixed in with the main logic, which makes it harder to see and follow. With early return, we’ve made error handling upfront and separated it from the core function. This makes our code clearer and safer, as we catch issues right at the start.

# Closing

Early return is a powerful technique in programming languages that can make your code cleaner, easier to read, and easier to maintain. By handling errors and edge cases upfront, you avoid deep nesting and highlight the "happy path" — the part of the code where everything works as expected.

This technique aligns with good error handling practices and improves the overall flow of your functions. By making error handling upfront and focusing on the core logic afterward, you’ll end up with code that’s not only easier to understand but also safer to run.

So next time you write a function, consider whether you could use early return to make it clearer and more direct. It’s a small change that can make a big difference in how your code reads and behaves.