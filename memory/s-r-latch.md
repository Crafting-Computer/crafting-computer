# S R Latch

Last time, we were able to set `1` to be the stored value of our cascading not gates but can't reset it back to `0`. To be able to reset, we can use a new circuit called a Set Reset Latch or S R Latch for short:

![S R Latch \(Initial State\)](../.gitbook/assets/image%20%2818%29.png)

Notice that we used two same gates ans weaved the outputs and inputs together into a loop that looks like the number `8`. The gates we used are called `nor` gates which is short for `not or`. It's basically the output of `or` gate inverted.

![Nor Gate](../.gitbook/assets/image%20%2811%29.png)

Let's see the truth table of `nor` gate:

| a | b | a or b | not \(a or b\) / a nor b |
| :--- | :--- | :--- | :--- |
| 0 | 0 | 0 | 1 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 1 | 0 |

So the `nor` gate outputs `1` if and only if both inputs are `0`. This explains the red wires of our initial state. Since the two `nor` gates are symmetrical, we will just look at the top one:

![Top Nor Gate \(Initial State\)](../.gitbook/assets/image%20%282%29.png)

The switch supplies a `0` but since we don't know the output of the other `nor` gate, the output is undetermined. The same goes with the bottom `nor` gate.

## Set State

Let's see what happens when we turn on the bottom switch:

![S R Latch \(Set State\)](../.gitbook/assets/image%20%286%29.png)

We see that the top light bulb is on while the bottom is off. Let's see what happened step by step. 

### Step 1

First, we turned on the bottom switch:

![Set State Step 1](../.gitbook/assets/image%20%2810%29.png)

Looking at the truth table of `nor` gate, we see that if any one of the inputs is `1`, our output has to be `0`.

| a | b | a nor b |
| :--- | :--- | :--- |
| 0 | 0 | 1 |
| **0** | **1** | **0** |
| **1** | **0** | **0** |
| **1** | **1** | **0** |

### Step 2

![Set State Step 2](../.gitbook/assets/image%20%289%29.png)

The output `0` of the bottom `nor` gate goes to the input of the top `nor` gate.

### Step 3

![Set State Step 3](../.gitbook/assets/image%20%2817%29.png)

Since the top switch is off and the output of the bottom `nor` gate is `0`, the output of the top `nor` gate has to be `1` because of our truth table:

| a | b | a nor b |
| :--- | :--- | :--- |
| **0** | **0** | **1** |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

### Step 4

![Set State Step 4](../.gitbook/assets/image%20%284%29.png)

The output `1` of the top `nor` gate goes to the input of the bottom `nor` gate. Since both inputs of the bottom `nor` gate is `1`, its output is `0` referencing our truth table:

| a | b | a nor b |
| :--- | :--- | :--- |
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| **1** | **1** | **0** |

If we put the state of step 1 and step 4 side by side, we can see that they are the same. So if you continue the tracing, you will simply loop through these 4 steps again and get the same outputs.

![](../.gitbook/assets/step_1_and_step_4.png)

## Reset State

Now if we instead turn on the top switch, we will get:

![Reset State](../.gitbook/assets/image%20%2812%29.png)

The top light bulb is off while the bottom light bulb is on. The steps are the same as above so we will let you trace through them yourself to get the feel. By now, you may notice that the top and bottom light bulb is always the complement of each other. When one is on, the other must be off. This why we call the top light bulb `q` and the bottom light bulb `q complement`. You should also get that the bottom switch sets `q` to `1` while the top switch resets `q` to `0`. That's why they are called `s` for set and `r` for reset respectively.

![Input and Output Names](../.gitbook/assets/image%20%2816%29.png)



