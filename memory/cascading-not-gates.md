# Cascading Not Gates

So far we succeeded in creating the ALU, which does all the heavy arithmetic and logic computations of our computer. However, our computer is kind of passive. You or a human operator has to tell the computer exactly the inputs and operations it need to perform. Wouldn't it be cool if we can store our instructions in the computer and let it do the work for us? Can we store the result of our computations and use them later? All of these tasks require our computer to remember things or have a memory.

Memory enables humans like us to learn new knowledge and development relationships. Not only that, memory forms the foundation for our identity, culture, religion, and ideology. The ability to remember stuff is what makes our society tick. In our opinion, memory is also the key that makes computer truly trancending and magical. Just like before, we will look at the simplest case first - storing 1 bit of information.

The simplest way to store 1 bit is a loop containing two cascading not gates:

![Cascading Not Gates \(Initial State\)](../.gitbook/assets/image%20%2829%29.png)

The `or` gate is just used to combine the inputs from the switch and the second `not` gate. It does serves no other function in the circuit. The above is the initial state when you finished connecting the circuit but haven't turned on the switch. Notice the red wires which mean undetermined state. These wires are undetermined because the switch outputs a `0` but we don't know what the second `not`gate outputs. So the output of the `or` gate and all the wires connected to its output are undertermined. However, once we turn on the switch:

![Cascading Not Gates \(On State\)](../.gitbook/assets/image%20%2815%29.png)

The or gate is no longer confused because one of its input is `1` so the output has to be `1`. The `1` outputed by the `or` gate is negated once by the first `not` gate to become `0` but is negated again by the second `not` gate to return back to `1`. Now if we turn off the switch:

![Cascading Not Gates \(Off State\)](../.gitbook/assets/image%20%285%29.png)

Quite unexpected right? The light bulb remains on because one of the inputs of the `or` gate is `1` so whether the switch is on or off doesn't matter. This behavior is both good and bad. The good part is we successfully created our first memory using digital circuits! We are able to remember the on state forever. The bad part is once we store the value, we can never change it. It's like you can only deposit money into a bank account once and it's immediately frozen forever so you can't deposite more or retrieve your money back. This really sucks because the whole point of having a memory is to be able to be able to add new stuff to it or update the stuff it contains. To create a more useful memory, we will need to create a latch. Before we move on, you can actually implement this in Sim:

```text
cascading_not_gates input[1] -> [1] =
  let
    output = or output (not (not input))
  in
  output
```

This circuit is unlike any other ones we built previously. Notice that `output` depends on both `input` and `output` itself. Because any value is undetermined initially, `output` will be undertermined in the first run. However, if we set `input` to `1`, `output` will be set to `1` after the first run and stays at `1` forever because of the `or` gate.

