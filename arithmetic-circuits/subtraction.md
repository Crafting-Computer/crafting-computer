# Subtraction

Our new 16-bit adder looks all fancy and well. However, now you might be curious about how subtraction works in binary. But before we talk about subtraction, we need to talk about negative numbers. There are two reasons:

* A bigger number - a smaller number = a negative number
* a - b = a + \(-b\)

So how do we represent negative numbers in binary?

## Option 1: Sign + Magnitude

Let's refer to the decimal system with "+" and "-" and encode "+" as `0` and "-" as `1`.

| n in decimal | n in binary | -n in decimal | -n in binary |
| :---: | :---: | :---: | :---: |
| +1 | 0001 | -1 | 1001 |
| +2 | 0010 | -2 | 1010 |
| +3 | 0011 | -3 | 1011 |
| +4 | 0100 | -4 | 1100 |
| +5 | 0101 | -5 | 1101 |
| +6 | 0110 | -6 | 1110 |
| +7 | 0111 | -7 | 1111 |

What about `0`? We unfortunately get two representations of `0`: `+0` and `-0`.

| +0 | -0 |
| :---: | :---: |
| 0000 | 1000 |

Two zeros are an extreme pain to deal with both in the hardware and software level. For example, a simple check whether a number is zero need to handle two cases - whether it's a positive or a negative zero.

This looks fine until we try to do a subtraction, say `7 - 3`. Because subtraction is just the addition of the negated, `7 - 3` is just `(+7) + (-3)`. If we look up the above table for `+7` and `-3` and add them together:

```text
       s|mag
   +7  0|111
+  -3  1|011
――――――――――――
   +4 10|010
```

Notice that we get `010` which is `2` in decimals but we expect `7 - 3` to be `4`. We don't even want to think about the messy carry of the sign bit. This system of tagging an extra sign bit in front of the magnitude is looks familiar and fine at first sight, but is very tricky to deal with.

## Option 2: 1's Complement

In Mathematics, the method of complements is a technique to encode a symmetric range of positive and negative integers. For a given number of places half of the possible representations of numbers encode the positive numbers, the other half represents their respective negative numbers.

We obtain the 1's complement of a binary number by subtracting it from `1111`. Let's say we have a binary number `0110`. We get the 1's complement of `0110` like so:

```text
   1111
-  0110
―――――――
   1001
```

Another example:

```text
   1111
-  1010
―――――――
   0101
```

Do you notice any patterns?

Our Answer Yes, we simply flip each bit to get the 1's complement.

| n in decimal | n in binary | -n in decimal | -n in binary |
| :---: | :---: | :---: | :---: |
| +1 | 0001 | -1 | 1110 |
| +2 | 0010 | -2 | 1101 |
| +3 | 0011 | -3 | 1100 |
| +4 | 0100 | -4 | 1011 |
| +5 | 0101 | -5 | 1010 |
| +6 | 0110 | -6 | 1001 |
| +7 | 0111 | -7 | 1000 |

What about `0`? We unfortunately still get two representations of `0`: `+0` and `-0`.

| +0 | -0 |
| :---: | :---: |
| 0000 | 1111 |

Let's try the same `7 - 3` by looking up the table for the binary representations of `+7` and `-3` and add them together:

```text
   +7  0111
+  -3  1100
――――――――――――
   +4 10011
```

We get back `0011` or `3` which is not the right answer. However, if we add the carry bit `1` to `0011`:

```text
  0011
+    1
――――――
  0100
```

Voila! We get back `0100` which is the correct answer `+4` in binary!

This seems like a neat trick and works every time. However, as computer scientists, we want to understand exactly how things work instead of memorizing tricks. In this case, the explanation is simple with some clever tweaks:

```text
  a - b
= a + (-b)
= a + (-b) + 1111 - 1111
= a + (1111 - b)  - 1111
```

Notice that `1111 - b` is our 1's complement of `b`. Continue from the previous step:

```text
= a + (1111 - b)  - 1111
= result - 1111
```

Notice the `result` means the result of adding `a` and the 1's complement of `b`. This is analogous to the result `10011` we got previously when adding `0111` \(`+7`\) and `1100` \(`-3`\). Continue from the previous step:

```text
= result - 1111
= result - (10000 - 1)
= result - 10000 + 1
```

We rewrote `1111` with `10000 - 1` and extract the `-1` out of the parentheses to become a `+1`. Continue:

```text
= result - 10000 + 1
= result + 1
```

Subtracting any 4-bit number by `10000` does nothing to the 4-bits. Since we are dealing with 4-bit numbers only, we don't care what happens to the `1` on the fifth bit.

Now you should understand why we add the `1` from the carry bit to our result.

## Option 3: 2's Complement

While 1's complement is much better than the sign + magnitude approach, we still need to deal with two zeros and had to add the carry bit when doing addition. Enter 2's complement which elegantly solves both issues. All you need to do is add `1` to a number's 1's complement to get its 2's complement.

| n in decimal | n in binary | -n in decimal | -n in binary |
| :---: | :---: | :---: | :---: |
| +1 | 0001 | -1 | 1111 |
| +2 | 0010 | -2 | 1110 |
| +3 | 0011 | -3 | 1101 |
| +4 | 0100 | -4 | 1100 |
| +5 | 0101 | -5 | 1011 |
| +6 | 0110 | -6 | 1010 |
| +7 | 0111 | -7 | 1001 |
|  |  | -8 | 1000 |

Note that the numbers we can represent with 4 bits range from `-8` to `+7`. The reason for the extra `-8` is because we freed one slot by removing the double zeros!

Just in case you are not convinced, let's try to get the negative zero by taking the 2's complement of zero:

```text
  0000
  1111  flip all bits to get 1's complement
+    1  add one to get 2's complement
――――――
 10000  2's complement of 0
```

Since we only care about 4 bits, the result `10000` is effectively still `0`.

Let's try the same `7 - 3` by looking up the table for the binary representations of `+7` and `-3` and add them together:

```text
   +7  0111
+  -3  1101
――――――――――――
   +4 10100
```

Voila! We got `7 - 3 = 4` as expected.

Let's see why 2's complement works:

```text
  a - b
= a + (-b)
= a + (-b) + 1111 - 1111
= a + (1111 - b)  - 1111         same steps as the 1's complement
= a + (1111 - b + 1) - 1111 - 1  add then subtract 1
```

Notice that `(1111 - b + 1)` is the 2's complement of `b`. It also matches our algorithm of getting the 1's complement of a number then add `1` to get its 2's complement. Continue:

```text
= a + (1111 - b + 1) - 1111 - 1  add then subtract 1
= result - 1111 - 1
= result - 10000
= result
```

After going through three viable options for representing negative numbers in binary, we and most computer scientists choose 2's complement for its simplicity. Enough theory, let's try to implement a subtractor! So what circuits do we need? Because `a - b = a + (-b)`, we just need to create a negator and add the negated `b` to `a` as if we are subtracting `b` from `a`.

## 16-bit Negator

How do we negate a binary number? We just take its 2's complement:

* Flip all bits
* Add 1

Try implement the negator on your own.

Here's the header:

```text
neg16 input[16] -> [16]
```

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints".
{% endtab %}

{% tab title="See Hints" %}
Does flipping bits sound similar to a logic gate we created before? Which circuit we designed earlier allows you add two numbers?
{% endtab %}
{% endtabs %}

## Additional Resource

Check out Computerphile's awesome explaination of how signed binary works:

{% embed url="https://www.youtube.com/watch?v=lKTsv6iVxV4" caption="Computerphile Signed Binary Video" %}



