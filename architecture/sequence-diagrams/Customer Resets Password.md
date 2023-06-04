
@startuml Customer Resets Password
title Customer Resets Password
actor Customer
participant Website
participant CustomerService as "Customer Service"
participant EmailService as "Email Service"

Customer -> Website: Navigate to Password Reset Page
Website -> Customer: Enter Email Address
Customer -> Website: Enter Email
Website -> EmailService: Send Password Reset Email POST /emails
note right
{
    "body": "Please click the link below to reset your password.",
    "customer_id": 1,
    "reset_link": "https://website.com/reset-password?token=abc123" 
}
end note
EmailService -> Website: Email Sent 200 OK
Website -> Customer: Password Reset Link Sent

Customer -> Website: Click Password Reset Link
Website -> Customer: Enter New Password
Customer -> Website: Enter New Password
Website -> CustomerService: Update Customer Password PATCH /customers/1
note right 
{
    "password": "newpassword"
}
end note
CustomerService -> Website: Password Updated 200 OK
Website -> Customer: Password Successfully Reset

@enduml
