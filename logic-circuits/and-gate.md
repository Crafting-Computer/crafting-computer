# And Gate

The `not` gate we implemented just now is very simple, let's now check out a more complex circuit - the `and` gate.

We start by specifying the interface of the `and` gate.

Looking at the truth table of `and`, we notice that it expects two inputs `a` and `b` and produces a single output.

| a | b | a and b |
| :--- | :--- | :--- |
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

So we can declare the header of the `and` gate as:

```text
and a[1] b[1] -> [1]
```

Great!

Now since `a and b` is equivalent to `not (a nand b)`, we can implement the body of the `and` gate as:

```text
not (nand a b)
```

Combining the header and the body, we got:

```text
and a[1] b[1] -> [1] =
    not (nand a b)
```

