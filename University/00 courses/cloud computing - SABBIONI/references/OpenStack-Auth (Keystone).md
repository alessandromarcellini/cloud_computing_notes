first auth is done normally and sync, then keystone sends a token to the tenant and will authenticate it asynchronously, faster and without checking the internal state using the token for the next auth requests.

Keystone keeps track of the "users and groups" of the openstack system.
The correct term to use is **tenants** (the projects). Inside a project a set of users is created each with different roles.
In OpenStack, tenants are **logical organizational units** (also called “projects”) that group cloud resources such as VM instances, volumes, networks, and images, isolating them from one another to support multi-tenancy.

Provides 4 primary services:
-  **Identity**: user information authentication;
- **Token**: after logged-in, replaces password authentication;
- **Catalog**: maintains an endpoint registry used to discovery OpenStack services endpoints (?);
- **Policy**: provides a rule-based authorization engine,

![[openstack_keystone_flow.png]]
### Auth Main Functions

They are used to separate work environments: each tenant has its own ==resource namespace==, usage ==quotas==, and access control managed through Keystone (the identity service).

An administrator creates tenants and assigns ==roles== to users (e.g., “member” to use resources, “admin” to manage them within the tenant).

#### Example

A university may create one tenant for the Engineering department and another for the Medical department, preventing interference between their respective environments.