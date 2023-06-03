
@startuml Customer Returns Items
title Customer Returns Items
actor Customer
participant Website
participant OrderService as "Order Service" 
participant EmailService as "Email Service"
participant PaymentService as "Payment Service"

Customer -> Website: Request Return
Website -> OrderService: PATCH /orders/{order_id}
note right
{
    "order_status": "Return Requested"
}
end note
OrderService -> EmailService: Generate Return Label POST /labels
note right
{
    "order_id": 1,
    "customer_email": "customer@example.com"
}
end note
EmailService -> OrderService: Return Label Generated 200 OK
OrderService -> Website: Order Updated 200 OK

Customer -> OrderService: Ship Returned Items
OrderService -> PaymentService: Process Refund POST /refunds
note right
{
    "order_id": 1,
    "amount": 100.00 
}
end note
PaymentService -> OrderService: Refund Processed 200 OK
OrderService -> Website: Order Status Updated 200 OK
note right 
{
    "order_status": "Return Complete"
}
end note

@enduml
