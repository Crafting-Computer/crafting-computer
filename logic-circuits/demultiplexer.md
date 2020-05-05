# Demultiplexer

## 2-way Dmux

Dmux is short for demultiplexer and does the inverse of what mux or multiplexer does. Given an input, the dmux select its output between several paths based on an address.

| input | sel | a | b |
| :---: | :---: | :---: | :---: |
| input | 0 | input | 0 |
| input | 1 | 0 | input |

```text
{-
    {a, b} = {input, 0} if sel == 0
             {0, input} if sel == 1
-}
dmux input[n] sel[1] -> { a[n], b[n] }
```

What's that `{ a[n], b[n] }` output? It's our first time seeing a function that returns more than one output. In Mathematics, we usually think of a function as a machine that accepts one or multiple inputs and produces a single output. Single output also makes working with return values a lot easier. However, for our `dmux` function, we clearly need to return two values. What do we do? To contain multiple outputs, we use a **record**. In Sim, a record is a bunch of key-value pairs:

```text
{ a = 0
, b = 1
, c = 0
}
```

where `a`, `b`, and `c` are keys and `0`, `1`, and `0` are the keys' values.

The great thing about the record is that we always know the names of our many output values, just like we do for our function inputs. Plus, a record is a single value, just like `0` and `input`!

If you have ideas on how to use records and how to implement `dmux`, go ahead and implement it.

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints".
{% endtab %}

{% tab title="See Hints" %}
Fill in the blanks below:

```text
dmux input[n] sel[1] -> { a[n], b[n] } =
    let
        a =
            ___________________
        b =
            ___________________
    in
    { a = a, b = b }
```
{% endtab %}
{% endtabs %}

## 4-way Dmux

| input | sel | a | b | c | d |
| :---: | :---: | :---: | :---: | :---: | :---: |
| input | 00 | input | 0 | 0 | 0 |
| input | 01 | 0 | input | 0 | 0 |
| input | 10 | 0 | 0 | input | 0 |
| input | 11 | 0 | 0 | 0 | input |

```text
{-
    {a, b, c, d} = {input, 0, 0, 0} if sel == 00
                   {0, input, 0, 0} if sel == 01
                   {0, 0, input, 0} if sel == 10
                   {0, 0, 0, input} if sel == 11
-}
dmux_4_way input[n] sel[2] -> { a[n], b[n], c[n], d[n] }
```

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints".
{% endtab %}

{% tab title="See Hints" %}
Fill in the blanks below:

```text
dmux_4_way input[n] sel[2] -> { a[n], b[n], c[n], d[n] } =
    let
        { a = a, b = b } =
            ___________________
        { a = c, b = d } =
            ___________________
    in
    { a = ___________________
    , b = ___________________
    , c = ___________________
    , d = ___________________
    }
```
{% endtab %}
{% endtabs %}

## 8-way Dmux

| input | sel | a | b | c | d | e | f | g | h |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| input | 000 | input | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| input | 001 | 0 | input | 0 | 0 | 0 | 0 | 0 | 0 |
| input | 010 | 0 | 0 | input | 0 | 0 | 0 | 0 | 0 |
| input | 011 | 0 | 0 | 0 | input | 0 | 0 | 0 | 0 |
| input | 100 | 0 | 0 | 0 | 0 | input | 0 | 0 | 0 |
| input | 101 | 0 | 0 | 0 | 0 | 0 | input | 0 | 0 |
| input | 110 | 0 | 0 | 0 | 0 | 0 | 0 | input | 0 |
| input | 111 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | input |

```text
{-
    {a, b, c, d, e, f, g, h} =
        {input, 0, 0, 0, 0, 0, 0, 0} if sel == 000
        {0, input, 0, 0, 0, 0, 0, 0} if sel == 001
        ...
        {0, 0, 0, 0, 0, 0, 0, input} if sel == 111
-}
dmux_8_way input[n] sel[3] -> { a[n], b[n], c[n], d[n], e[n], f[n], g[n], h[n] }
```

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints".
{% endtab %}

{% tab title="See Hints" %}
Fill in the blanks below:

```text
dmux_8_way input[n] sel[3] ->
    { a[n], b[n], c[n], d[n], e[n], f[n], g[n], h[n] } =
    let
        { a = a, b = b, c = c, d = d } =
            ___________________
        { a = e, b = f, c = g, d = h } =
            ___________________
        sel_when_0 output[n] -> [n] =
            ___________________
        sel_when_1 output[n] -> [n] =
            ___________________
    in
    { a = ___________________
    , b = ___________________
    , c = ___________________
    , d = ___________________
    , e = ___________________
    , f = ___________________
    , g = ___________________
    , h = ___________________
    }
```
{% endtab %}
{% endtabs %}

🎉 We just completed all the common logic circuits for our computer!

