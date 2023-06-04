
@startuml Customer Registration
title Customer Registration
actor Customer
participant Website
participant CustomerService as "Customer Service"
participant EmailService as "Email Service"

Customer -> Website: Navigate to Registration Page
Customer -> Website: Submit Registration Form
Website -> CustomerService: POST /customers
note right
{
    "first_name": "John",
    "last_name": "Doe",
    "email": "john@example.com",
    "date_of_birth": "01/01/1990",
    "address_line1": "123 Main St",
    "city": "San Francisco",
    "state": "CA",
    "zip_code": "94122",
    "country": "US"
} 
end note
CustomerService -> Website: Customer Created 201 Created
Website -> EmailService: Send Welcome Email POST /emails
note right
{
    "to": "john@example.com",
    "subject": "Welcome!",
    "body": "Thank you for registering.  Your login credentials are: 
    username: john@example.com
    password: password123" 
}
end note
EmailService -> Website: Email Sent 200 OK

@enduml
