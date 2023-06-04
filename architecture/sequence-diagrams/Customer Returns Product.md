
@startuml Customer Returns Product
title Customer Returns Product
actor Customer
participant Website
participant OrderAPI as "Order API"
participant ProductAPI as "Product API"
participant EmailAPI as "Email API"
participant FulfillmentAPI as "Fulfillment API"

Customer -> Website: Request Return 
Website -> OrderAPI: GET /orders/{order_id}
OrderAPI -> Website: Order Details 200 OK
Website -> OrderAPI: POST /returns
note right
{
    "order_id": 1,
    "product_id": 5,
    "reason": "Defective" 
}
end note
OrderAPI -> FulfillmentAPI: POST /returns/review
FulfillmentAPI -> OrderAPI: Return Approved 200 OK
OrderAPI -> FulfillmentAPI: Generate Return Label
FulfillmentAPI -> OrderAPI: Return Label PDF
OrderAPI -> EmailAPI: Send Return Label Email
note right
{
    "body": "Your return has been approved.",
    "customer_id": 1,
    "return_label_pdf": {pdf_data} 
}
end note  
EmailAPI -> OrderAPI: Email Sent 200 OK

' ... Time Passes ... 

FulfillmentAPI -> OrderAPI: Product Received, Refund Issued
OrderAPI -> ProductAPI: UPDATE /products/{product_id}
note right 
{
    "quantity": {quantity} + 1 
}
end note
ProductAPI -> OrderAPI: Product Updated 200 OK
OrderAPI -> Website: Return Complete, Refund Issued

@enduml
