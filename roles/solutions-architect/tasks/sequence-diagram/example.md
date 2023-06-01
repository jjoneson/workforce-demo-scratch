<sequenceDiagram title="Customer Places Order">
@startuml Customer Places Order
title Customer Places Order
actor Customer
participant Website
participant OrderAPI as "Order API"
participant WarehouseAPI as "Warehouse API"
participant EmailAPI as "Email API"

Customer -> Website: Place Order
Website -> OrderAPI: POST /orders
note right
{
    "customer_id": 1,
    "order_total": 100.00
}
end note
OrderAPI -> WarehouseAPI: POST /fulfillments
note right
{
    "order_id": 1
}
end note
WarehouseAPI -> OrderAPI: Order Fulfilled 200 OK
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
</sequenceDiagram>