<enterpriseArchitecture>
# Enterprise Architecture
We use a microservices architecture and web front-ends.

Microservices fall into the following categories:

- **Domain Entity Services** These services are responsible for managing the core business entities.  They are the source of truth for the business entities.  They are the only services that can directly access the database.  They manage the data and enforce the business rules.  Examples: Customer Service, Order Service, Product Service, etc.  There should always be a customer service.

- **Integration Services** These services are responsible for integrating with external systems.  They are responsible for translating between the external system and the internal system.  They are responsible for ensuring that the data is in the correct format for the external system.  Examples: Email Service, Core Banking Service, etc.

- **Domain Process Services** These services are responsible for implementing business processes.  They are responsible for orchestrating the Domain Entity Services and Integration Services to implement the business processes.  Examples: Order Fulfillment Service, Payment Service, etc.
</enterpriseArchitecture>