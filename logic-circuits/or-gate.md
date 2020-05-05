# Or Gate

## 2-way Or Gate

Following the same procedure for implementing `and` gate, we start by specifying the interface of the `or` gate.

Looking at the truth table of `or`, we notice that it expects two inputs `a` and `b` and produces a single output.

| a | b | a or b |
| :--- | :--- | :--- |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

So we can declare the header of the `or` gate as:

```text
or a[1] b[1] -> [1]
```

Not bad!

Using De Morgan's laws, we can rewrite `a or b` as `not ((not a) and (not b))`. Since `not (_ and _)` is equivalent to `_ nand _`, we can further rewrite `a or b` as `(not a) nand (not b)`. We can implement the body of the `or` gate as:

```text
nand (not a) (not b)
```

Combining the header and the body, we got:

```text
or a[1] b[1] -> [1] =
    nand (not a) (not b)
```

## 4-way Or Gate

Sometimes, we need to `or` more than two numbers together. One use case we will see later is checking whether a number equal to zero by `or`ing all its digits. If the result is `1` then we have at least one `1` in the number which makes it not equal to zero. Otherwise, the number only contains `0` as its digits so it must be equal to zero.

Because the truth table is rather big, we will show the gate diagram instead:

![or\_4\_way horizontal](../.gitbook/assets/or_4_way_horizontal.png)

So far, we have seen most logic gates laid out horizontally like the above: inputs on the left and outputs on the right. This layout also suggests that we have four inputs and one output like so:

```text
or_4_way a[1] b[1] c[1] d[1] -> [1]
```

The problem with the above header is verbosity. While it's manageable to lay out all inputs individually for our 4-way or gate, consider laying out all inputs for a 8-way or 16-way or gate:

8-way Or Gate:

```text
or_8_way a[1] b[1] c[1] d[1] e[1] f[1] g[1] h[1] -> [1]
```

16-way Or Gate:

```text
or_16_way a[1] b[1] c[1] d[1] e[1] f[1] g[1] h[1] i[1] j[1] k[1] l[1] m[1] n[1] o[1] p[1] -> [1]
```

It's getting very hard to keep track of the letters and count how many inputs we have specified. In summary, laying out inputs individually for a large number of inputs is a messy strategy which we will avoid most of the time unless there are no other ways. Is there a better way here? For that we need a **bus**, which is a bunch of 1-bit values stringed together. So instead of listing out our four inputs like above:

```text
or_4_way a[1] b[1] c[1] d[1] -> [1]
```

we will condense them into a 4-bit bus:

```text
or_4_way input[4] -> [1]
```

See how much cleaner our code becomes? To get the individual bits of our bus `input`, all we need to do is to specify the index we are looking for:

```text
-- get the 0th index
input[0]

-- get the 1th index
input[1]

-- get the 2nd index
input[2]

-- get the 3rd index
input[3]
```

Here's how a 4-bit number `0101` or `6` in decimals will be laid out:

![bus indices demo](../.gitbook/assets/bus_indices.png)

Notice that **the first or the 0th index starts from the right and grow to the left**. In Mathematics, we call the 0th digit the least significant bit and the 3rd digit the most significant bit of a 4-bit binary number. The reason why we start the indices at zero is because:

![bus indices as exponents](../.gitbook/assets/bus_indices_as_exponents.png)

The binary number `0101` has a decimal value of:

```text
0 * 2^3 + 1 * 2^2 + 0 * 2^1 + 1 * 2^0 = 4 + 2 = 6
```

> Note: From now on, we may refer a binary number as `0b0101` instead of spelling out something like the binary number `0101`. The `0b` is a prefix that signifies that the digits following it is in binary. Similarly, `0x` is a prefix that signifies that the digits following it is in hexadecimal \(16-bits\). By default, Sim uses base 10 or decimals but you can specify binary numbers by prefixing `0b` and hexadecimal numbers by prefixing `0x`.

Now that we understand what a bus is, we will show a 4-way or gate in bus layout:

![or\_r\_way bus layout](../.gitbook/assets/or_4_way_vertical.png)

We can implement `or_4_way` in Sim like so:

```text
or_4_way input[4] -> [1] =
    or (or input[0] input[1]) (or input[2] input[3])
```

This implementation works but the logic is getting cluttered in a single line. We can improve the readability by using the `let` expression:

```text
or_4_way input[4] -> [1] =
    let
        first = or input[0] input[1]
        second = or input[2] input[3]
    in
    or first second
```

`let` expression allows us to give descriptive names to subexpressions. It is especially useful when the expression is becoming to cluttered or we wish to reuse certain expressions multiple times. As you will see later, `let` expression also reduce repeated code, boost performance, and reduce the complexity and cost of circuits.

## 8-way Or Gate

We can follow the same logic as a 4-way or gate to implement an 8-way or gate.

We have been showing you how to implement circuits up till now. It's time for you to experiment a little. Before we set you loose, we have a new concept to cover. It turns out that `input[0]` and the like are not the only way to access the bits of a bus. You can also slice a section of a bus like so:

```text
-- Create a 3-bit slice from the bus `input`
input[0..2]
```

`input[0..2]` literally means: Get me the bits from index `0` to `2` of the `input` bus. So you will get the bits `input[0]`, `input[1]`, and `input[2]` by using `input[0..2]`.

Here's an example:

```text
get_first_3_bits input[4] -> [3] =
    input[0..2]
```

Now you have all the needed knowledge to create `or_8_way` yourself!

Here's the header:

```text
or_8_way input[8] -> [1]
```

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints".
{% endtab %}

{% tab title="See Hints" %}
Fill in the blanks below:

```text
or_8_way input[8] -> [1] =
    or (____ input[_.._]) (____ input[_.._])
```
{% endtab %}
{% endtabs %}

## 16-way Or Gate

Try implement a 16-way or gate using 8-way or gates.

Here's the header:

```text
or_16_way input[16] -> [1]
```

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints".
{% endtab %}

{% tab title="See Hints" %}
Fill in the blanks below:

```text
or_16_way input[16] -> [1] =
    or (____ input[_.._]) (____ input[_.._])
```
{% endtab %}
{% endtabs %}

