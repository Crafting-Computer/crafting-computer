# Arithmetic Logic Unit

After implementing all the logic and arithmetic circuits, we can now combine them into a powerful machine called the Arithmetic Logic Unit or ALU. For our ALU, it will be able to do addition, subtraction, negation, `and`, `or`, and `not`. More precisely, given two 16-bit inputs `x` and `y`, the ALU can do:

```text
x+y, x-y, y-x, 0, 1, -1, x, y, -x, -y, !x, !y,
x+1, y+1, x-1, y-1, x&y, x|y
```

according to `6` control bits:

```text
zx,nx,zy,ny,f,no
```

The ALU logic manipulates the x and y inputs and operates on the resulting values, as follows:

```text
if (zx == 1) set x = 0            16-bit constant
if (nx == 1) set x = !x           bitwise not
if (zy == 1) set y = 0            16-bit constant
if (ny == 1) set y = !y           bitwise not
if (f == 1)  set out = x + y      integer 2's complement addition
if (f == 0)  set out = x & y      bitwise and
if (no == 1) set out = !out       bitwise not
```

According to the final `out` value, we will compute two 1-bit outputs:

```text
if the ALU output == 0, zr is set to 1; otherwise zr is set to 0;
if the ALU output < 0, ng is set to 1; otherwise ng is set to 0.
```

Here's the header of the ALU:

```text
alu
    x[16] y[16] -- 16-bit inputs
    zx -- zero the x input?
    nx -- negate the x input?
    zy -- zero the y input?
    ny -- negate the y input?
    f  -- compute out = x + y if f == 1 or x & y if f == 0
    no -- negate the output?
    ->
    { out[16] -- 16-bit output
    , zr -- 1 if out == 0, 0 otherwise
    , ng -- 1 if out < 0, 0 otherwise
    }
```

The ALU seems very complicated at first sight. However, it only uses the functions we defined earlier and apply the operations that depend on the control bits sequentially.

{% tabs %}
{% tab title="Hints" %}
If you get stuck, click on "See Hints".
{% endtab %}

{% tab title="See Hints" %}
Fill in the blanks below:

```text
alu
    x[16] y[16] -- 16-bit inputs
    zx -- zero the x input?
    nx -- negate the x input?
    zy -- zero the y input?
    ny -- negate the y input?
    f  -- compute out = x + y if f == 1 or x & y if f == 0
    no -- negate the output?
    ->
    { out[16] -- 16-bit output
    , zr -- 1 if out == 0, 0 otherwise
    , ng -- 1 if out < 0, 0 otherwise
    } =
    let
        x1 =
            ____ ____ ____ zx
        x2 =
            ____ ____ ____ nx
        y1 =
            ____ ____ ____ zy
        y2 =
            ____ ____ ____ ny
        out1 =
            ____ ____ ____ f
        out2 =
            ____ ____ ____ no
        zr =
            ____ ___________________
        ng =
            ___________________
    in
    { out = out2, zr = zr, ng = ng }
```
{% endtab %}
{% endtabs %}

