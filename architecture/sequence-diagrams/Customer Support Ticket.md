
```plantuml
@startuml Customer Support Ticket
title Customer Support Ticket
actor Customer
participant Website
participant SupportAPI as "Support API"
participant EmailAPI as "Email API"
participant CustomerService as "Customer Service"

Customer -> Website: Contact Support (Phone, Email, Chat)
Website -> SupportAPI: Create Support Ticket POST /support_tickets
note right
{
    "customer_id": 1,
    "issue_description": "My order is delayed." 
}
end note
SupportAPI -> CustomerService: Get Customer Details GET /customers/1
CustomerService -> SupportAPI: Customer Details 200 OK
SupportAPI -> EmailAPI: Send Support Ticket Confirmation Email POST /emails
note right
{
    "body": "Thank you for contacting support. Your ticket has been created.",
    "customer_id": 1,
}
end note
EmailAPI -> SupportAPI: Email Sent 200 OK
SupportAPI -> Website: Support Ticket Created 201 Created

Website -> SupportAPI: Get Support Ticket Details GET /support_tickets/1
SupportAPI -> Website: Support Ticket Details 200 OK

Website -> SupportAPI: Update Support Ticket PUT /support_tickets/1
note right 
{
    "status": "resolved",
    "resolution": "Your order has shipped and will arrive within 2-3 business days."
}
end note
SupportAPI -> EmailAPI: Send Resolution Email POST /emails
note right
{
    "body": "Your support ticket has been resolved. ",
    "resolution": "Your order has shipped and will arrive within 2-3 business days.",
    "customer_id": 1,
}
end note
EmailAPI -> SupportAPI: Email Sent 200 OK
SupportAPI -> Website: Support Ticket Updated 200 OK

@enduml
```
