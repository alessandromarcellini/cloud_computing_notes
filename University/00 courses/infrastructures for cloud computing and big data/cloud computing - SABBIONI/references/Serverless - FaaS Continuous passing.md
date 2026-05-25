![[faas_continous_passing.png]]

In this pattern there's no central coordinating entity like in [[Serverless - FaaS function merging]] or in [[Serverless - FaaS Reflective invocation]]. Here the functions directly know who's the next function to be triggered, and to do so they themselves emit the event that triggers the next function.

## Pros:
- **Direct calling** without passing through another entity;
- **no need to keep state** in the coordinating function to track the responses of the functions that where called before;
- **One less activation**: there's no coordinating function so, as we're spending only for function activations, we're spending less.

## Cons:
- **Tight coupling** of functionA with functionB: If I want to reuse functionA to trigger function C I need to change its code or create a new function. 