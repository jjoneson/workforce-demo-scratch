
@startuml Customer Returns Product
title Customer Returns Product
actor Customer
participant Website
participant OrderAPI as "Order API"
participant ReturnAPI as "Return API"
participant EmailAPI as "Email API"
participant WarehouseAPI as "Warehouse API"

Customer -> Website: Request Return 
Website -> ReturnAPI: POST /returns
note right
{
    "order_id": 1,
    "order_line_item_id": 5,
    "reason": "Defective Product"
}
end note
ReturnAPI -> OrderAPI: GET /orders/1
OrderAPI -> ReturnAPI: Order Details 200 OK
ReturnAPI -> EmailAPI: Generate Return Label
EmailAPI -> ReturnAPI: Return Label PDF
ReturnAPI -> Website: Return Request Created 201 Created
ReturnAPI -> EmailAPI: Send Return Label Email
note right
{
    "to": "customer@example.com",
    "subject": "Return Shipping Label",
    "body": "Please see attached return shipping label.",
    "attachments": "return_label.pdf"
}
end note
EmailAPI -> ReturnAPI: Email Sent 200 OK

Customer -> WarehouseAPI: Ship Return Product 
WarehouseAPI -> ReturnAPI: Product Received
ReturnAPI -> OrderAPI: Refund Order
OrderAPI -> ReturnAPI: Refund Processed 200 OK
ReturnAPI -> Website: Refund Complete
Website -> Customer: Refund Complete

@enduml
