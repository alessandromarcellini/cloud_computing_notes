#### internal components
- **nova-API**: RESTful API web service used to send commands to interact with OpenStack. It is also possible to use CLI clients to make OpenStack API calls;
- **nova-compute**: hosts and manages VM instances by communicating with the underlying hypervisor (configurable, not standard);
- **nova-scheduler**: coordinates all services and determines placement of new requested resources;
- **nova database**: stores build-time and run-time states of Cloud infrastructure, uses ==MySql== (why???).
- **queue**: handles interactions between other Nova services By default, it is implemented by RabbitMQ, but also Qpid can be used;
- **nova-console**, **nova-novncproxy** e **nova-consoleauth**: provides, through a proxy, user access to the consoles of virtual instances;
- %%  nova-network: accepts requests coming from the queue and executes tasks to configure networks (i.e., changing IPtables rules, creating bridging interfaces, …) ==These functionalities are now moved to Neutron service==;
- nova-volume: handles persistent volumes creation and their de/attachment from/to virtual instances. ==These functionalities are now moved to Cinder services==. %%

![[openstack_nova_interaction.png]]
