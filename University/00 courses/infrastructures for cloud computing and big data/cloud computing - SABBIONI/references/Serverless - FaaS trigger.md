Triggers are a foundamental part of FaaS architectures as they are the **frontier** of our infrastructure. A trigger, infact, ==intercepts requests== from the external environment ==translating== them as events of the internal FaaS infrastructure, spreading them into the system itself (this is called **event-streaming**, it's a temporal-unbounded streaming of informations, we don't know when the events will end, we'll see it in the last lecture).

> [!quote]  
> An Event is a unit of information with a specific timestamp and metadata (contextual informations). This is the only state that is managed by functions, they're stateless.

>[!example]
>External = http request
>The trigger intercepts it and translates it in an event that is spread in the internal infrastructure.