# D Flip Flop

S R Flip Flop works well but we need to supply two inputs besides the clock signal to set or reset the data and we may also accidentally get into the Not Used State which messes up our circuit with undertermined behavior when we try to read the data afterwards.

| s | r | clk | Total Possibile Input Combinations |
| :--- | :--- | :--- | :--- |
| 2 \(1/0\) | 2 \(1/0\) | 2 \(1/0\) | 2 \* 2 \* 2 = 8 |

As you can see, we have `8` possible input combinations for our S R Flip Flop. However, all we need are `3` combinations to store and retrieve 1 bit:

| 1 | 2 | 3 |
| :--- | :--- | :--- |
| Set | Reset | Read |

If we can condense `s` and `r` into one input called `d` \(stands for data\), we reduced the possibilities down to `4`. We still have one extra possible input combination but that's unavoidable since we need at least 2 binary inputs to have 3 states.

| d | clk | Total Possibile Input Combinations |
| :--- | :--- | :--- |
| 2 \(1/0\) | 2 \(1/0\) | 2 \* 2 = 4 |

Let's take a look at how the new D Flip Flop is constructed:

![D Flip Flop](../.gitbook/assets/image%20%2826%29.png)

Notice that we just pass `d` directly to `s` and invert `d` and pass it to `r`.

## Implementation

The extension from S R Flip Flop to D Flip Flop is trivial.

Here's the header:

```text
d_flip_flop clk d[n] -> [n]
```

