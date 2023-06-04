
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
    "reason": "Defective"
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
    "body": "Please print and attach the return label to your package."
    "customer_id": 1,
    "return_label_pdf": "data:application/pdf;base64,..."
}
end note
EmailAPI -> ReturnAPI: Email Sent 200 OK

Customer -> WarehouseAPI: Ship Return Package
WarehouseAPI -> ReturnAPI: Package Received
ReturnAPI -> OrderAPI: Update Order Status to "Return Received"
OrderAPI -> ReturnAPI: Order Updated 200 OK
ReturnAPI -> WarehouseAPI: Process Refund
WarehouseAPI -> ReturnAPI: Refund Processed
ReturnAPI -> OrderAPI: Update Order Status to "Refund Processed"
OrderAPI -> ReturnAPI: Order Updated 200 OK

@enduml
