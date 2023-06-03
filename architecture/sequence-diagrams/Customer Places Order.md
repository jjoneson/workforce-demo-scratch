
@startuml Customer Places Order
title Customer Places Order
actor Customer
participant Website
participant OrderAPI as "Order API"
participant OrderLineItemAPI as "OrderLineItem API"
participant EmailAPI as "Email API"

Customer -> Website: Login
Website -> Customer: Products Page
Customer -> Website: Add to Cart
Website -> Customer: Checkout Page
Customer -> Website: Confirm Shipping/Billing 
Customer -> Website: Confirm Payment
Website -> OrderAPI: POST /orders
note right
{
    "customer_id": 1,
    "order_total": 100.00
}
end note
OrderAPI -> OrderLineItemAPI: POST /orderlineitems
note right
{
    "order_id": 1,
    "product_ids": [1,2],
    "quantities": [1,2]
} 
end note
OrderLineItemAPI -> OrderAPI: OrderLineItems Created 201 Created
OrderAPI -> EmailAPI: Send Confirmation Email POST /emails
note right
{
    "body": "Your order has been placed."
    "customer_id": 1,
}
end note
EmailAPI -> OrderAPI: Email Sent 200 OK
OrderAPI -> Website: Order Created 201 Created

@enduml
