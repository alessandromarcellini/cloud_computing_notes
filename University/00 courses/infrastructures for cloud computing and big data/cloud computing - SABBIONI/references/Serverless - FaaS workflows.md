Workflows, in their basic form, are what enables the user to **bind** a function to a specific event **creating the app business logic**.
More complex possibilities include:
- chaining functions together, creating a pipeline;
- executing multiple functions in parallel;
- executing functions with a db request (for those who actually sell also a BaaS);
- associate multiple events to a single function (e.g. exposing the function to both http and mqtt requests).
%%Note: these capabilities are provided by the provider so not all of them support each and everyone of the listed before%%


As we just mentioned, providers often offer the possibility of creating a pipeline of function associated with an event (or more), creating the possibility of implement a functionality not within the scope of a single function but with a chain of functions. This has several pros, a few of which being:
1) Reducing the scope of a function makes the code more "==modular==", simple to manage and reusable (we usually can reuse the smaller function in multiple workflows to compose them into new features);
2) **Increasing throughput** for the system by pipelining the workload. E.g. an event arrives and function1 of the pipeline is activated, function1 finishes executing and function2 is activated, leaving function1 able to process the next event arriving;
3) Each function can be developed in a different programming language.
These are all [[microservices]] properties that with FaaS are ==lead to the extreme==.

## Different Pipelining Workflow implementations
When thinking of workflows 2 main different implementation approaches come to mind:
- an Orchestrated one;
- a Choreographed one.

#### Orchestrated workflow
In order to implement the pipelining of functions in a workflow the first approach is to have a **central controller that manages all the single functions**, in other words, an orchestrator.

>[!note]
>- Orchestrator activates function1;
> - Orchestrator retrieves the result of function1 and activates function2 inputting the result of function1;
>- so on...

%% Aws uses this%%
#### Choreographed workflow
In the choreography approach there's no central component that orchestrates, rather, the functions themselves trigger the next functions by emitting events containing their result as payload.

Strict binding, not really reusable.

## Workflows Building blocks
- event sourcing: how to actually obtain the events that trigger a workflow:
- functions: no need to explain this one;
- state management: **every workflow is a state** (that specific event makes me reach that specific part of the pipeline). Also some cloud providers support ==introducing if statements== inside workflow's logic (e.g. if the result of function1 is true trigger function2 else trigger function3);
- orchestrator: entity that is able to execute the workflow.