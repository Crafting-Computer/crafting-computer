# Not Gate

Let's start by looking at how to implement a logical not gate in Sim:

```text
not a[1] -> [1] =
    nand a a
```

Notice that we first specify the interface of the circuit by declaring:

```text
not a[1] -> [1]
```

This means that the `not` circuit takes one input pin called `a` that has a size of `1` and produces an output of size `1`. We can use `not` as follows:

```text
not_0 = not 0
-- not_0 is now 1
not_1 = not 1
-- not_1 is now 0
```

> Note: line comments in Sim starts with `--`. Multi-line comments starts with `{-` and ends with `-}`.

At first, the way we called `not` may seem odd because you may be used to call syntax like this:

```c
not_0 = not(0)
```

In Sim, to make function calls cleaner, we do not use parentheses around arguments. In addition, we use space instead of comma to separate arguments:

```text
nand a a
```

instead of

```c
nand(a, a)
```

Now you may also wonder where is `nand` defined. The answer is that unlike the `not` gate which is defined by the user, `nand` gate is a built-in circuit provided by Sim. It's the most fundamental logic gate and we can build all other gates and circuits from the `nand` gate.

