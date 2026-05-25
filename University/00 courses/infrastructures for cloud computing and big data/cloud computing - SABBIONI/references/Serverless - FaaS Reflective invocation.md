![[faas_function_composition.png]]


The principle here is really similar to [[Serverless - FaaS function merging]], we still have one function that coordinates and is triggered from the external world but with a substantial difference:
If in the function merging pattern we where coordinating the underlying functions from a merging function, here the **coordinating function actively emits events** in order to trigger the other functions.

## Pros:
- **maintain modularity** of functions;
- **maintain functions independent**: they can scale independently (e.g. if functionA is really fast and functionB really slow a lot more replicas of functionB will be instantiated, while functionA replicas remain lower in number).
## Cons:
- **more computational resources** used: emitting events, that go through 2 different workflows (one for functionA and one for functionB), 2 different triggers, 2 different function activations. With this setup we're spending 3 times more than in the merging one;
- **more latency**: everything goes through the network, the different functions can be in different nodes really far away between them.

## Comparison with function merging:
Compared with [[Serverless - FaaS function merging]] this can be slower and more expensive in terms of resources.
This is because all the coordination is done through the network, in function merging it was all inside the same function, coordinating  """directly on hardware""" by leveraging simple procedure calls.