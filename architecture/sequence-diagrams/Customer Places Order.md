
@startuml Customer Places Order
title Customer Places Order
actor Customer
participant Website
participant OrderAPI as "Order API"
participant CustomerAPI as "Customer API"
participant ProductAPI as "Product API"
participant OrderLineItemAPI as "Order Line Item API"
participant EmailAPI as "Email API"

Customer -> Website: Place Order
Website -> OrderAPI: POST /orders
note right
{
    "customer_id": 1,
    "order_total": 100.00
}
end note
OrderAPI -> CustomerAPI: GET /customers/1
CustomerAPI -> OrderAPI: Customer Data 200 OK
OrderAPI -> ProductAPI: GET /products
ProductAPI -> OrderAPI: Product Data 200 OK
OrderAPI -> OrderLineItemAPI: POST /orderlineitems
note right
{
    "order_id": 1,
    "product_id": 1,
    "quantity": 1,
    "price": 50.00
}
end note
OrderLineItemAPI -> OrderAPI: Order Line Item Created 201 Created
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
