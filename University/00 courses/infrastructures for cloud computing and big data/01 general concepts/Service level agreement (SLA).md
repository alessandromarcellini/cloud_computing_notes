A **Service Level Agreement (SLA)** is a formal (**non-ambigous**) ==contract between a service provider and a customer== that defines the expected level of service, including measurable performance targets and the responsibilities of both parties.

The indicators that are included to define QoS are divided into:
- **functional**: easily quantifiable (e.g. throughput, jitter, latency...);
- **non-functional**: difficult to quantify (e.g. service availability, security level, quality of the experience...).

Typical Key Performance Indicators (**KPIs**) are:
- **Availability/Uptime** (e.g., 99.9% service availability);
- **Mean-time between failure**;
- **Response time** (how quickly requests are handled)
- **Resolution time** (how fast issues must be fixed)
- **Performance guarantees** (latency, throughput, etc.)
- **Penalties or compensations** if the agreed service levels are not met

For example, a cloud provider might offer an SLA guaranteeing **99.99% uptime**, meaning the service is expected to be available for all but a very small amount of downtime each month.

>[!important]
>If these expectations are not met the cloud provider doesn't charge the service to the customer, so it's really important for providers that these contracts go as well as possibile during the whole lifecycle of the service.

In short, an **SLA defines what level of service a customer can expect and how that service quality will be measured and enforced**.