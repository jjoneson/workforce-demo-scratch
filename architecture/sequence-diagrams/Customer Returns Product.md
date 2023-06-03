
@startuml Customer Returns Product
title Customer Returns Product
actor Customer
participant Website
participant OrderService as "Order Service" 
participant EmailService as "Email Service"
participant PaymentService as "Payment Service"

Customer -> Website: Request Return 
Website -> OrderService: POST /orders/{orderId}/returns
note right
{
    "return_reason": "Defective Product"
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

Customer -> OrderService: Ship Product Back
OrderService -> PaymentService: Issue Refund POST /payments/refunds
note right
{
    "order_id": 1,
    "amount": 100.00
}
end note
PaymentService -> OrderService: Refund Processed 200 OK
OrderService -> EmailService: Send Refund Confirmation Email POST /emails
note right
{
    "body": "Your refund for order 1 has been processed.",
    "customer_id": 1,
    "order_id": 1
}
end note 
EmailService -> OrderService: Email Sent 200 OK

@enduml
