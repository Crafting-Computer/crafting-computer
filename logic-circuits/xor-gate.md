# Xor Gate

Following the same procedure for implementing `or` gate, we start by specifying the interface of the exclusive or gate.

Looking at the truth table of `xor`, we notice that it expects two inputs `a` and `b` and produces a single output.

| a | b | a xor b |
| :--- | :--- | :--- |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

So we can declare the header of the `xor` gate as:

```text
xor a[1] b[1] -> [1]
```

Not bad!

Now since `a xor b` is equivalent to `((not a) and b) or (a and (not b))`, we can implement the body of the `xor` gate as:

```text
or (and (not a) b) (and a (not b))
```

We can finish the implementation right here and combine the header and the body as usual. However, the logic is getting cluttered in a single line. We can improve the readability by using the `let` expression:

```text
let
    first = and (not a) b
    second = and a (not b)
in
or first second
```

Combining the header and the body, we got:

```text
xor a[1] b[1] -> [1] =
    let
        first = and (not a) b
        second = and a (not b)
    in
    or first second
```

## Optimizing Xor Gate

If we just care about creating a `xor` gate that works, we can happily wrap up here. However, if we want to produce the circuit as a part of a real computer, we need to worry about both cost and efficiency. The two most basic ways to assess cost and efficiency is by calculating the total number of logic gates used and the amount of gate delays.

#### Decrease Gate Counts

First, let's count the number of gates used.

> The reason we care is because `xor` gate is a fundamental logic gate that's used in adders and many other circuits. We will likely end up using tens if not hundreds of `xor` gate in our CPU so even reducing the subgate count by one or two can save us loads of extraneous gates, silicon space, and production cost.

Counting gates in Sim is easy, we just need to track the number of function calls like so:

```text
xor a[1] b[1] -> [1] =
    let
        first = and (not a) b -- 2 calls: 1 and, 1 not
        second = and a (not b) -- 2 calls: 1 and, 1 not
    in
    or first second -- 1 call: 1 or
```

Adding up the number of calls in the body of `xor` got us `2 + 2 + 1 = 5`. So it seems that we used `5` logic gates to construct the `xor` gate. However, don't forget that each `not` and `and` gate is composed of one or several `nand` gates. Indeed, if we look up our definition for `not`:

```text
not a[1] -> [1] =
    nand a a
```

We used one `nand` gate so the number of `nand` gates remains the same. If we look up our definition for `and`:

```text
and a[1] b[1] -> [1] =
    not (nand a b)
```

We used one `not` gate and one `nand` gate, so `1 + 1 = 2` nand gates in total for our `and` gate.

Looking up our definition for `or` gate:

```text
or a[1] b[1] -> [1] =
    nand (not a) (not b)
```

We used `2` not gates and `1` nand gate in the implementation, so `2 + 1 = 3` nand gates in total for our `or` gate.

Now let's redo our calculation for `xor`. Based on our previous analysis, we used `2` and gates, `2` not gates, and `1` or gate in the implementation. So it's `2 (and) * 2 (nand) + 2 (not) * 1 (nand) + 1 (or) * 3 (nand) = 4 + 2 + 3 = 9` nand gates.

Alright, let's attempt to optimize the `xor` gate. One thing we can try is to expand the definition to using only nand gates and see some share parts that can be reused. We will use a more concise syntax from now on to represent common logic gates like `not` and `and`. Here's a table for your reference:

| gate | symbol | precedence |
| :---: | :---: | :---: |
| not | -a | 0 |
| and | a \* b | 1 |
| or | a + b | 2 |

> Note: The symbols are borrowed from decimal number algebra to boolean algebra. They are similar algebraic properties and operator precedence. Also, a smaller number means higher precedence.

In the beginning we came up with a definition for `xor` like so:

```text
((not a) and b) or (a and (not b))
```

Now we can make it cleaner by using algebraic symbols:

```text
-a*b + a*-b
```

The following expansion steps require knowledge of boolean algebra. We show them just to illustrate how optimization may look like but don't expect you to conduct optimization like this yourself.

```text
-a*b + a*-b
-(-(-a*b + a*-b))
-(-(-a*b) * -(a*-b))
-(-a*b) NAND -(a*-b)
-(-a*b + -b*b) NAND -(a*-b + a*-a)
-(b*(-a + -b)) NAND -(a*(-b + -a))
(b NAND (-a + -b)) NAND (a NAND (-a + -b))
(b NAND -(a * b)) NAND (a NAND -(a * b))
(b NAND (a NAND b)) NAND (a NAND (a NAND b))
```

Now you may notice we have a shared subexpression `-a + -b`. We can express this optimized version in Sim:

```text
xor a[1] b[1] -> [1] =
    let
        nand_a_b = nand a b
        first = nand b nand_a_b
        second = nand a nand_a_b
    in
    nand first second
```

We can easily see that this version uses only `4` nand gates which saves `5` gates compared to our original version!

#### Decrease Gate Delays

Gate delays is a rough way to calculate the execution speed of a given circuit.

> The reason we care about gate delays is because even though we can imagine that electric signal float instantly from input pins to output pins, it takes a small amount of time for the electrons to actually travel through the gates and wires. While saving a few nanoseconds in one gate does not sound very exciting, saving millions of nanoseconds in millions of gates making up a computer is very crucial to performance.

Let's start by calculating the gate delays of the `not` gate.

**Not Gate Delays**

```text
not a[1] -> [1] =
    nand a a
```

![not\_gate\_delays](../.gitbook/assets/not_gate_delays.png)

Notice that any electric signal passes through `1` nand gate before reaching the output light bulb. This implies `1` gate delay.

**And Gate Delays**

```text
and a[1] b[1] -> [1] =
    not (nand a b)
```

![and\_gate\_delays](../.gitbook/assets/and_gate_delays.png)

Notice that any electric signal passes through `2` nand gates before reaching the output light bulb. This implies `2` gate delays.

**Or Gate Delays**

```text
or a[1] b[1] -> [1] =
    nand (not a) (not b)
```

![or\_gate\_delays](../.gitbook/assets/or_gate_delays.png)

Notice that any electric signal passes through `2` nand gates before reaching the output light bulb. This implies `2` gate delays.

Finally, we can calculate the gate delays of our original `xor` gate:

```text
xor a[1] b[1] -> [1] =
    let
        first = and (not a) b
        second = and a (not b)
    in
    or first second
```

![xor\_gate\_delays](../.gitbook/assets/xor_gate_delays.png)

Notice that any electric signal passes through

* `1` not gate \(1 gate delay\)
* `1` and gate \(2 gate delays\)
* `1` or gate \(2 gate delays\)

before reaching the output light bulb. This implies `1 + 2 + 2 = 5` gate delays.

Now let's calculate the gate delays of our optimized `xor` gate from the previous section:

```text
xor a[1] b[1] -> [1] =
    let
        nand_a_b = nand a b
        first = nand b nand_a_b
        second = nand a nand_a_b
    in
    nand first second
```

![xor\_optimized\_gate\_delays](../.gitbook/assets/xor_optimized_gate_delays.png)

Notice that any electric signal passes through `3` nand gates before reaching the output light bulb. This implies `3` gate delays. Compared to our original implementation, the optimized version saves us `2` gate delays!

**In conclusion, our optimization saves us 5 gates and 2 gate delays. This means half the cost and almost double the performance!**

