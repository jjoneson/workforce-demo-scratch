
@startuml Customer Resets Password
title Customer Resets Password
actor Customer
participant Website
participant IdentityService as "Identity Service"
participant EmailService as "Email Service"

Customer -> Website: Click "Forgot Password"
Website -> Customer: Prompt for email
Customer -> Website: Enter email
Website -> IdentityService: GET /customers?email={email}
IdentityService -> Website: Customer Found 200 OK
Website -> EmailService: Send Password Reset Email 
note right
{
    "body": "Please click the link below to reset your password.",
    "customer_id": 1,
    "reset_token": "abc123"
}
end note
EmailService -> Website: Email Sent 200 OK
Website -> Customer: Email sent

Customer -> Website: Click password reset link
Website -> IdentityService: POST /customers/reset_password
note right
{
    "customer_id": 1,
    "reset_token": "abc123",
    "new_password": "password1234"
} 
end note
IdentityService -> Website: Password Reset 200 OK
Website -> Customer: Password reset. You can now login.

@enduml
