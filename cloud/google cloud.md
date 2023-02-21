# Introduction
### Development
- renting physical space -> virtualized data centers -> container-based architecture
### Type of cloud offerings
- IaaS, PaaS and SaaS
- Compute
- Storage
- Bid data
- Machine learning
- Application services
### Network
- multiple regions and zones
### Security
- Hardware infrastructure layer
- Service deployment layer
- User identity layer
- Storage services layer
- Internet communication layer
- Operational security layer
### Open source ecosystems
### Pricing and biling
- Deliver per-second billing
- When you run an instance for more than 25% of a month, Compute Engine automatically gives you a discount for every incremental minute you use for that instance.
- Budgets, Alerts, Reports and Quotas(rate & allocation)
- Each Google Cloud project has a quota allowing it no more than five Virtual Private Cloud networks.
- You can change some of quotas by requesting an increase from supoort
# Resources and Access in the Cloud
### Google Cloud resource hierarchy
- Organization node -> Folder -> Project -> Resources
- Policies can be inherited from parent nodes.
- to use folder, you must have an organization node
- if you are a Google Workspace customer, you already have a Organization node, if not, you need to create one.
- IAM policies that are implemented by lower-level policies can override the policies defined at a higher level.
### Identity and Access Management (IAM)
- policy recipient can be **a Google account, a Google group, a service account or a Cloud Identity domain.**
###### Basic role
- quite broad in scope
- owner, editor, viewer and billing administrator
###### Predefined role
- specific, existed role
###### Custom role
- more specific, need manage by yourself, can be only applied to either the project level or organization level.
### Cloud Identity
- using Google Admin Console to disable or remove someone from groups
- a free edition or a premium edition that provides capabilitied to manage mobile devices.
- if you're a Google Workspace customer, this functionality is alread available
### Interacting with Google Cloud
- four ways: Google Cloud console, Cloud SDK and Cloud Shell, APIs and Cloud Mobile App
- libraries of API support Java, Python, PHP, C#, Go, Node.js, Ruby
# Virtual Machines and Networks in the Cloud
### VPC
- VPC networks connect Google Cloud resources to each other and to the internet
- Including segmenting networks, using firewall rules to restrict access to instances, and createing static routes to forward traffic to specific destinations.
- VPC can also have subnets, which is a segmented piece of the larger network.
- two instances in a VPC, they can be in different zones. (applicable for resilient to disruptions yet retain a simple network layout)
### Compute Engine
- can be created via the Google Cloud console, Google Cloud CLI, or the Compute Engine API
- Some Cloud Marketplace images charge usage fees.
- Sustained-use discount (more than 25% of a month, apply a discount for every additional minute)
- Committed-use discount (up to a 57% discount, committing to a usage term of one year or three years)
- Preemptible and Spot VMs (up to 90%, can be terminate, the former can only run for up to 24 hours at a time)
### Scaling virtual machines
- VPC supports kinds of load balancing.
- Most of customers start off with scaling out, not up
### Important VPC compatibilities
- VPCs have routing tables, can forward traffic from one to another within the same network, across subnetworks, or even zones, without requiring an external IP addredd.
- VPCs provide a global distributed firewall. It can based on tags which are tagged your server.
- VPC Peering can estabish a relationship between two VPCs
- Configure a Shared VPC to use the full power of IAM to control who and what in one project can interact with a VPC in another.
### Cloud Load Balancing
- No pre-warming
###### cross region
- Global HTTP(S), Global SSL Proxy, Global TCP Proxy, Regional(UDP)
###### internal
- Regional internal(UDP)
### Cloud DNS and Cloud CDN
- CDN can interconnect parter program
### Connecting networks to Google VPC
- IPsec VPN protocol
- Direct Peering
- Carrier Peering
- Dedicated Interconnect (99.99% SLA)
- Partner Interconnect (99.99% SLA)
# English note
### vocabulary
- round off: To **round off** this section of the course
# Abbreviation
- GFE: Google Front End
- GKE: Google Kubernetes Engine
- IAM: Identity and Access Management
- GUI: Graphical User Interface
- API: Application Programming interface
- LAMP: Linux Apache, MySQL, PHP
- VPC: Virtual Private Cloud
- NAT: Network Address Translation