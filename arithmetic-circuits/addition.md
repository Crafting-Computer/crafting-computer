# Addition

Logic is fun but as the name computer suggests, we need to conduct arithmetic computations in order to do more interesting stuff. We will learn how to add and subtract two binary numbers with an added bonus of multiplication.

## Half Adder

Let's start from the bare minimum - adding two 1-bit numbers together. Binary addition works the same way as decimal addition. The only difference is that it's much simpler! A bit can only be either `0` or `1`. Addition of two bits have only four cases:

| a | b |
| :---: | :---: |
| 0 | 0 |
| 0 | 1 |
| 1 | 0 |
| 1 | 1 |

For the first three cases, the sum is very straight forward:

| a | b | sum |
| :---: | :---: | :---: |
| 0 | 0 | 0 + 0 = 0 |
| 0 | 1 | 0 + 1 = 1 |
| 1 | 0 | 1 + 0 = 1 |

The fourth case is a little tricky. In decimals, `1 + 1` equals `2`. However, the highest digit we got in binary is `1`. What do we do now? Let's think about what we do in this situation when adding two decimals. If we add `1` and `9`, we get the sum equal to `0` with a carry of `1`. Because the carry is at a more significant digit, we right the result as carry \(`1`\) then sum \(`0`\) or `10`.

Similarly in binary, we also get a sum of `0` and a carry of `1` or `10`. However, `10` in binary means `(1 + 2 ^ 1) + (0 * 2 ^ 0) = 2 + 0 = 2`. This makes sense because we expect `1 + 1` to equal `2` no matter what base we are in!

Ok. Let's add the fourth case to our truth table while introducing a new carry output:

| a | b | carry | sum |
| :---: | :---: | :---: | :---: |
| 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 0 |

It's time for you to implement the half adder in Sim:

```text
half_adder a b -> { carry, sum }
```

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints"
{% endtab %}

{% tab title="See Hints" %}
Look at carry and sum separately. Do their truth tables look familiar?
{% endtab %}
{% endtabs %}

## Full Adder

Half adder works well for adding two 1-bit numbers. However, when we need to add two multi-bit numbers, we need to take account of the carry bit from the previous less significant digit. That's when the full adder comes in:

| a | b | c | carry | sum |
| :---: | :---: | :---: | :---: | :---: |
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 | 1 |
| 0 | 1 | 0 | 0 | 1 |
| 0 | 1 | 1 | 1 | 0 |
| 1 | 0 | 0 | 0 | 1 |
| 1 | 0 | 1 | 1 | 0 |
| 1 | 1 | 0 | 1 | 0 |
| 1 | 1 | 1 | 1 | 1 |

```text
full_adder a b c -> { carry, sum }
```

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints"
{% endtab %}

{% tab title="See Hints" %}
Fill in the blanks below:

```text
full_adder a b c -> { carry, sum } =
    let
        { carry = ____, sum = ____ } =
            ___________________
        { carry = ____, sum = ____ } =
            ___________________
        ____ =
            ___________________
    in
    { carry = ____, sum = ____ }
```
{% endtab %}
{% endtabs %}

## 2-bit Full Adder

There are only very few things you can do with 1-bit numbers. It's time to spice up our full adder to accept multi-bit numbers.

The full truth table has `(2 ^ 2) ^ 3 = 64` cases so we will not show it here. You can do some quick checks by adding the numbers yourself to see if your 2-bit full adder works.

```text
full_adder2 a[2] b[2] c -> { carry, sum[2] }
```

The idea for the 2-bit full adder is:

* Calculate the sum of the first bits, i.e. `c0, s0 = a[0] + b[0]`
* Calculate the sum of the second bits

  and the carry of the previous sum, i.e. `c1, s1 = a[1] + b[1] + c0`

There's a little challenge though. How do we combine the two sums `s0` and `s1` together as the final 2-bit sum? For this, we need the **bus literal**. Bus literals are a list of bits stringed together, hence the name bus. Here's an example:

```text
[ 0, 1, 1, 0, 1 ]
```

Note that a bus is much stricter than a list or array in other languages. For instance, buses can only contain 1-bit numbers. The followings **is not legal**:

```text
-- 2 or 0b10 occupies 2 bit (bigger than 1 bit).
[ 0, 1, 2 ]

-- Elements can only be 1-bit numbers.
-- records like { a = 2 } and any other type are not allowed.
[ 0, { a = 2} ]
```

One important property of bus literal is that it preserves leading zeros:

```text
-- Leading zeros are preserved
[ 0, 0, 1, 0 ]
```

This is not that important for a single bus literal. However, sometimes we need to concatenate or combine two buses together using `++`:

```text
-- Prefixing a `1` to our bus literal
-- results in [ 1, 0, 0, 1, 0 ]
[ 1 ] ++ [ 0, 0, 1, 0]
```

Concat the bus literals results in `[ 1, 0, 0, 1, 0 ]` \(preserving leading zeros\) instead of `[ 1, 0, 1, 0 ]` \(discarding leading zeros\). Compare this to int literals with the same values:

```text
-- Prefixing a `1` to our int literal
-- results in 0b110
0b1 ++ 0b0010
```

Concatenating the int literals results in `0b110` \(discarding leading zeros\) instead of `0b10010` \(preserving leading zeros\).

If you now understand bus literals and have ideas on how to implement the 2-bit full adder, give it a go.

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints".
{% endtab %}

{% tab title="See Hints" %}
Fill in the blanks below:

```text
full_adder2 a[2] b[2] c -> { carry, sum[2] } =
    let
        { carry = ____, sum = ____ } =
            ___________________
        { carry = ____, sum = ____ } =
            ___________________
    in
    { carry = ____, sum = [ ____, ____ ] }
```
{% endtab %}
{% endtabs %}

## 4-bit Full Adder

```text
full_adder4 a[4] b[4] c -> { carry, sum[4] }
```

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints".
{% endtab %}

{% tab title="See Hints" %}
Fill in the blanks below:

```text
full_adder4 a[4] b[4] c -> { carry, sum[4] } =
    let
        { carry = ____, sum = ____ } =
            ___________________
        { carry = ____, sum = ____ } =
            ___________________
    in
    { carry = ____, sum = [ ____, ____ ] }
```
{% endtab %}
{% endtabs %}

## 8-bit Full Adder

By far you should get the patterns of a multi-bit full adder. We can repeat this pattern any many time we want to generate arbitrary-sized full adders. However, we will only go until 16-bit which is the size of our computer registers \(more on that later\).

```text
full_adder8 a[8] b[8] c -> { carry, sum[8] }
```

## 16-bit Adder

Our last adder in the series will be a half adder because we don't need to compose the 16-bit adder even more to create bigger adders.

```text
adder16 a[16] b[16] -> { carry, sum[16] }
```

