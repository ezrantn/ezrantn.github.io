---
title: "Interval Constraint Propagation: Teaching Computers to Reason with Ranges"
date: "2026-07-23"
description: "Basis on constraint solver"
draft: false
math: true
---

When we write programs, we usually think about variables as having a single value.

For example:

```text
x = 10
```

The variable `x` contains exactly one value.

However, many problems in computer science and engineering are not about finding one exact value immediately. Sometimes, we only know that a variable exists within a certain range.

For example:

```text
x ∈ [0, 10]
```

This means that the value of `x` can be anywhere between 0 and 10.

This simple idea becomes powerful when combined with constraints. Instead of checking every possible value, we can reason about the possible ranges of variables and eliminate impossible solutions.

This technique is called **Interval Constraint Propagation**.

---

## What is an Interval?

An interval represents a range of possible values.

A normal variable:

```text
x = 5
```

contains one exact value.

An interval variable:

```text
x ∈ [1, 5]
```

means:

```text
1 ≤ x ≤ 5
```

The exact value of `x` is unknown, but we know its boundaries.

For example:

```text
Temperature ∈ [20°C, 30°C]
```

means the temperature can be any value between 20 and 30 degrees.

---

## Interval Arithmetic

To reason about intervals, we need operations that work on ranges.

Consider two intervals:

```text
A = [1, 5]

B = [10, 20]
```

If we add them together:

```text
A + B
```

The smallest possible result is:

```text
1 + 10 = 11
```

The largest possible result is:

```text
5 + 20 = 25
```

Therefore:

```text
[1,5] + [10,20] = [11,25]
```

The result is another interval representing all possible outcomes.

---

## Why Do We Need Constraint Propagation?

Imagine we have this constraint:

```text
x + y = 10
```

Initially, we only know:

```text
x ∈ [0,10]

y ∈ [0,10]
```

At this point, many possibilities exist.

`x` could be 0, 1, 2, ..., 10.

However, constraints can provide additional information.

Suppose we discover:

```text
y ∈ [7,10]
```

Since:

```text
x + y = 10
```

we can rewrite the constraint:

```text
x = 10 - y
```

Because:

```text
y ∈ [7,10]
```

then:

```text
x ∈ [0,3]
```

The possible values of `x` have been reduced.

Before propagation:

```text
x ∈ [0,10]
```

After propagation:

```text
x ∈ [0,3]
```

The solver has learned more information without trying every possible value.

This process is called **constraint propagation**.

---

## Detecting Impossible Constraints

Interval propagation can also detect contradictions.

Consider:

```text
x ∈ [0,5]

y ∈ [0,5]

x + y = 20
```

The maximum possible value of:

```text
x + y
```

is:

```text
5 + 5 = 10
```

However, the constraint requires:

```text
x + y = 20
```

The required value is outside the possible range.

Therefore, this branch is impossible.

A solver can immediately reject this constraint without exploring every possible combination.

---

## Branch and Prune

Propagation alone is sometimes not enough.

Consider:

```text
x ∈ [0,100]
```

There are still many possible values.

A common strategy is **branch and prune**.

The solver splits the interval:

```text
x ∈ [0,50]

x ∈ [50,100]
```

Then each branch is analyzed separately.

For each branch:

1. Apply constraint propagation.
2. Remove impossible ranges.
3. Continue searching if necessary.

This creates a search process where the solver continuously narrows the solution space.

---

## Implementing Interval Propagation in Rust

A minimal interval representation can be defined as:

```rust
#[derive(Debug, Clone)]
pub struct Interval {
    pub lower: f64,
    pub upper: f64,
}
```

An interval addition operation:

```rust
impl Interval {
    pub fn add(&self, other: &Interval) -> Interval {
        Interval {
            lower: self.lower + other.lower,
            upper: self.upper + other.upper,
        }
    }
}
```

For example:

```rust
let a = Interval {
    lower: 1.0,
    upper: 5.0,
};

let b = Interval {
    lower: 10.0,
    upper: 20.0,
};

let result = a.add(&b);
```

The result:

```text
[11,25]
```

represents every possible value produced by adding the two intervals.

---

## Relationship with SMT Solvers

Interval Constraint Propagation is closely related to how modern constraint solvers reduce search spaces.

Instead of blindly exploring every possible assignment:

```text
x = 0
x = 1
x = 2
...
```

the solver uses mathematical reasoning to eliminate impossible regions.

This idea appears in many areas:

* SMT solving
* formal verification
* symbolic execution
* optimization
* numerical analysis

A solver becomes more efficient when it can prove that an entire region of the search space is impossible.

---

## Conclusion

Interval Constraint Propagation is a simple idea with powerful consequences.

Instead of asking:

> "What is the exact value of this variable?"

we ask:

> "What values are still possible?"

By representing uncertainty as ranges and continuously refining those ranges through constraints, a solver can reason about complex problems more efficiently.

This idea became one of the foundations of the constraint solver I have been building in Rust, where interval reasoning is combined with branch-and-prune strategies to explore continuous constraint spaces.

Understanding intervals is not only about mathematics. It is about teaching computers how to reason.
