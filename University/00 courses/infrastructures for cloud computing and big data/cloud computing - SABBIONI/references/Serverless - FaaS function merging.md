![[faas_function_merging.png]]

If the provider doesn't provide the user with lots of freedom to create workflows this can be a nice pattern to overrcome the provider's limits.

It consists of putting each of our functions inside one single function (the merge one, that wraps them).
It will be the main of the Merge function that will coordinate and activate the functions in it, providing with a full programming language expressivity; you can use conditions (ifs), loops (while, for...) and so on.

## Pros:
- **Efficient** execution: only one workflow, only one trigger, the coordination of the underlying functions is done """directly on hardware""", through normal procedure calls.
## Cons:
- **No indipendent scalability**:  the function that scales is the merge one;
- **Tight coupling** of functions: we use modularity of the functions (TODO: not really sure about this: since the main is "implementing the workflow logic" we can actually reuse the functions inside different parts of the main and under different conditions. Richiedi).