
@startuml Customer Resets Password
title Customer Resets Password
actor Customer
participant Website
participant CustomerService as "Customer Service"
participant EmailService as "Email Service"

Customer -> Website: Navigate to Password Reset Page
Website -> Customer: Enter Email Address
Customer -> Website: Enter Email
Website -> EmailService: Generate Password Reset Link POST /emails
note right
{
    "body": "Please click the following link to reset your password: https://website.com/reset?token=abc123",
    "customer_id": 1,
}
end note
EmailService -> Website: Link Generated 200 OK
Website -> Customer: Link Sent to Email
Customer -> Website: Click Password Reset Link
Website -> CustomerService: Validate Password Reset Token GET /customers/reset?token=abc123
CustomerService -> Website: Token Valid 200 OK
Website -> Customer: Enter New Password
Customer -> Website: Enter New Password
Website -> CustomerService: Update Customer Password PATCH /customers
note right 
{
    "password": "newpassword"
    "customer_id": 1
}
end note
CustomerService -> Website: Password Updated 200 OK
Website -> Customer: Password Reset Successful!

@enduml
