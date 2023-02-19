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