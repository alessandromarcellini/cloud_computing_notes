
Zero scaling an application means deleting all of its instances due to no traffic. This means that at the next request at least one instance of the application needs to be cold started.

Cold starting an application means getting an instance of an application up and running from scratch (weather its in a container, Vm or in bare metal). This can be really slow and can cause latency so its really important to optimize it (one approach in containerized services can be making the image "leaner" using slim or alpine versions of the image).