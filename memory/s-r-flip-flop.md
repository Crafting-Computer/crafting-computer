# S R Flip Flop

Our S R Latch works brilliantly in storing and retrieving 1 bit of data. However, for a memory unit to be useful in a CPU, we need to be able to control the time to store or retrieve data. Currently, the inputs to the S R Latch always have values so we have to be in one of Set, Reset, Read, or Not Used State. Wouldn't it be great if we can add an Idle State so no matter what are the inputs `s` and `r` we are guaranteed to not modify the data? We can do it by adding a clock signal \(abbreviated as `clk`\)

![S R Flip Flop with Q Complement](../.gitbook/assets/image%20%2838%29.png)

What the `clk` does is simple:

* If `clk` = `0`, we will always be in the Read State no matter what `r` and `s` are
* If `clk` = `1`, we get our normal S R Latch back

You can think of `clk` as a way to block and unblock our two inputs `r` and `s`. If `r` and `s` are like cars on the road, then `clk` is like the the traffic light that block the cars when it turns red and allows the cars to pass when it turns green. Also similar to the traffic light, our clock signal has a uniform rythm that controls the heartbeat of all our memory units and the entire CPU. You may have heard of a computer's clock speed, overclocking, underclocking, etc. We will talk about clocking more when constructing the CPU.

## Implementation

Since you already see the circuit diagram, implementing the S R Flip Flop wouldn't be so hard. It will be even easier if you extend our previous S R Latch function without reimplementing the whole latch again.

Here's the header:

```text
s_r_flip_flop clk s[n] r[n] -> [n]
```

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints".
{% endtab %}

{% tab title="See Hints" %}
Fill in the blanks below:

```text
s_r_flip_flop clk s[n] r[n] -> [n] =
    let
        ____ =
            ___________________
        ____ =
            ___________________
    in
    s_r_latch ____ ____
```
{% endtab %}
{% endtabs %}

