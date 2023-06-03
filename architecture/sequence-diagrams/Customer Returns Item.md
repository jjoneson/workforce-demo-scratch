
@startuml Customer Returns Item
title Customer Returns Item
actor Customer
participant Website
participant OrderService as "Order Service"
participant EmailService as "Email Service"

Customer -> Website: Request Return
Website -> OrderService: PATCH /orders/{order_id}
note right
{
    "order_status": "Return Requested"
}
end note
OrderService -> EmailService: Send Return Label Email POST /emails
note right
{
    "body": "Your return shipping label is attached.",
    "customer_id": 1,
    "order_id": 1
}
end note
EmailService -> OrderService: Email Sent 200 OK

Customer -> OrderService: Ship Returned Item
OrderService -> EmailService: Send Refund Confirmation Email POST /emails
note right
{
    "body": "Your refund has been processed.",
    "customer_id": 1,
    "order_id": 1
}
end note
EmailService -> OrderService: Email Sent 200 OK
OrderService -> Website: PATCH /orders/{order_id}
note right
{
    "order_status": "Return Complete"
}
end note

@enduml
