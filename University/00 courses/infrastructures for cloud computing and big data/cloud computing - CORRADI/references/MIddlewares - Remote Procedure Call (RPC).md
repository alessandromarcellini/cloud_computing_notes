There are some middlewares that enable **calling remote procedure as if they were local**.
So, these middlewares, make the remote communication transparent, making it possibile to handle communications always as if they were local, with close to no difference in normal procedure calls.

In order to achieve this the both the client and the server require a particular piece of software called the **stub**, that is the one that actually handles the underlying communication and exposing procedures.

Upon that, each service needs to implement an interface defining its possible usages.

>[!note]
>This interface needs to be written in a specific language called Interface Definition Language (**IDL**), that isn't tied to any specific programming language, in order for entities written in different languages to still use this middleware and interact with each other.

>[!note]
>RMI is an implementation of this.

>[!note]
>These systems are usually making it so that clients and servers are really bound with each other.