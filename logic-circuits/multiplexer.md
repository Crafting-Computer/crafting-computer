# Multiplexer

## 2-way Mux

Since you should be familiar with Sim by now, we will use the multiplexer's truth table to directly derive its implementation.

| a | b | sel | result |
| :---: | :---: | :---: | :---: |
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |
| 1 | 1 | 1 | 1 |

Simply put, the multiplexer outputs the value of `a` when `sel = 0` and outputs the value of `b` when `sel = 1`. So we can condense our truth table to be:

| a | b | sel | result |
| :---: | :---: | :---: | :---: |
| a | b | 0 | a |
| a | b | 1 | b |

Now we have our truth table, we can derive the implementation like so:

```text
mux a[1] b[1] sel[1] -> [1] =
    let
        sel_a =
            and (not sel) a
        sel_b =
            and sel b
    in
    or sel_a sel_b
```

Let's check for optimizations:

```text
a*-sel + b*sel
-(-(a*-sel + b*sel))
-(-(a*-sel) * -(b*sel))
(a NAND -sel) NAND (b NAND sel)
```

Indeed, we can save `2` gates by switching from `or` to `nand` and still `2` more gates by switching from `and` to `nand`. Here's the optimized version:

```text
mux a[1] b[1] sel[1] -> [1] =
    let
        sel_a =
            nand (not sel) a
        sel_b =
            nand sel b
    in
    nand sel_a sel_b
```

Since we use 1-bit input pins quite often, we can omit the `[1]` and Sim will assume it's 1 bit:

```text
mux a b sel -> [1] =
    let
        sel_a =
            nand (not sel) a
        sel_b =
            nand sel b
    in
    nand sel_a sel_b
```

Here's a problem though, what if we want to select between two values that are 2 bits, 8 bits, or 32 bits? Do we have to write a separate `mux2`, `mux8`, and `mux16` even though the logic for any bit is the same? What if we can declare a general `mux` function that works for any sized input pins like so where `n` can be any positive number?

```text
mux a[n] b[n] sel -> [1]
```

But this requires our `nand` and `not` gate to handle arbitrary sized inputs as well... Do we need to reimplement all the previous circuits? The answer is you just need to change `[1]` to `[n]` and you are done!

So here's our updated `not` gate:

```text
not a[n] -> [n] =
    nand a a
```

What about `nand` gate? Sim has already done it for us if you check out `nand`'s header:

```text
nand a[n] b[n] -> [n]
```

Perfect! Now let's generalize our `mux` to n bits:

```text
mux a[n] b[n] sel -> [n] =
    let
        sel_a = nand (not sel) a
        sel_b = nand sel b
    in
    nand sel_a sel_b
```

The above does not work surprisingly as Sim complains:

```text
I'm expecting to find the type [n] here:
1| mux a[n] b[n] sel -> [n] =
                 ^^^          
but got the type [1].
```

Why does Sim tell us this message? Let's check the place we used `sel`:

```text
sel_a = nand (not sel) a
```

and

```text
sel_b = nand sel b
```

Notice that `sel` is passed in as an argument to the `nand` function which expects both its input to have a size of `n`. However, `sel` has a fixed size of `1`. It will be neat if we can expand a 1-bit value into multiple bits by copying its value multiple times. That is exactly what the built-in function `fill` does:

```text
fill a[1] -> [n]
```

Finally, we can finish our generalized implementation of `mux` by expanding `sel` to size `n`:

```text
mux a[n] b[n] sel -> [n] =
    let
        sel_a = nand (fill (not sel)) a
        sel_b = nand (fill sel) b
    in
    nand sel_a sel_b
```

## 4-way Mux

Now that you have learned to how to construct basic logic gates such as `and`, `or`, and `mux`, we will let you carry on the work to create the rest of the computer. Don't panic. We will specify exactly what you need to construct and cover new concepts in details as usual.

Here's the truth table for the 4-way mux:

| a | b | c | d | sel | result |
| :---: | :---: | :---: | :---: | :---: | :---: |
| a | x | x | x | 00 | a |
| x | b | x | x | 01 | b |
| x | x | c | x | 10 | c |
| x | x | x | d | 11 | d |

Here's the header for the 4-way mux:

```text
{-
    4-way multiplexer:
    result = a if sel == 00
             b if sel == 01
             c if sel == 10
             d if sel == 11
-}
mux_4_way a[n] b[n] c[n] d[n] sel[2] -> [n]
```

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints".
{% endtab %}

{% tab title="See Hints" %}
Fill in the blanks below:

```text
mux_4_way a[n] b[n] c[n] d[n] sel[2] -> [n] =
    let
        sel_a_b =
            ___________________
        sel_c_d =
            ___________________
    in
    ____ sel_a_b sel_c_d ____
```
{% endtab %}
{% endtabs %}

Now implement the body by yourself and check the truth table of your function with the expected truth table above.

> Note: By default the Sim editor outputs all values in the truth table in decimals. However, we always supply the expected truth table with all values in binary. We will add an option to output in binary in the future.

## 8-way Mux

| a | b | c | d | e | f | g | h | sel | result |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| a | x | x | x | x | x | x | x | 000 | a |
| x | b | x | x | x | x | x | x | 001 | b |
| x | x | c | x | x | x | x | x | 010 | c |
| x | x | x | d | x | x | x | x | 011 | d |
| x | x | x | x | e | x | x | x | 100 | e |
| x | x | x | x | x | f | x | x | 101 | f |
| x | x | x | x | x | x | g | x | 110 | g |
| x | x | x | x | x | x | x | h | 111 | h |

```text
{-
    8-way multiplexer:
    result = a if sel == 000
             b if sel == 001
             c if sel == 010
             d if sel == 011
             e if sel == 100
             f if sel == 101
             g if sel == 110
             h if sel == 111
-}
mux_8_way a[n] b[n] c[n] d[n] e[n] f[n] g[n] h[n] sel[3] -> [n]
```

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints".
{% endtab %}

{% tab title="See Hints" %}
Fill in the blanks below:

```text
mux_8_way a[n] b[n] c[n] d[n] e[n] f[n] g[n] h[n] sel[3] -> [n] =
    let
        sel_a_b_c_d =
            ___________________
        sel_e_f_g_h =
            ___________________
    in
    ____ sel_a_b_c_d sel_e_f_g_h ____
```
{% endtab %}
{% endtabs %}

