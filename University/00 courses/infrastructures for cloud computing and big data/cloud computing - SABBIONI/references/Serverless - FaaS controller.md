![[FaaS_controller_mom.png]]

The controller is what actually manages the resource allocation for multiple components inside the infrastructure to execute the whole workflow and, therefore, run certain functions in order for them to manage all the events incoming in that time according with the QoS specified in the [[Service level agreement (SLA)]].
This includes ==managing resources for triggers== to manage all the incoming events, resources for the ==functions== execution, ==load balancing== events between different instances of the same function.

>[!note]
>This is the first controller that we see that is not only managing resource allocation (See K8s controller and [[Openstack]] controller) but, in a sense, also implementing business logic, as it implements the concept of workflow, associating events to functions.

Most of the time, as shown in the above picture, controllers are paired with a **[[Middleware - Message oriented Middleware (MOM)]]** in order to decouple in space and time the trigger and the function, by ==buffering== the incoming requests and stream those events later, for when there's actually someone that can manage them, **making the trigger actually stateless**.
