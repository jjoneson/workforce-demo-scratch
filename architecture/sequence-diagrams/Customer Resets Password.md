
@startuml Customer Resets Password
title Customer Resets Password
actor Customer
participant Website
participant IdentityService as "Identity Service"
participant EmailService as "Email Service"

Customer -> Website: Navigate to Password Reset Page
Website -> Customer: Prompt for Email
Customer -> Website: Enter Email
Website -> IdentityService: POST /passwordResets
note right
{
    "email": "customer@example.com"
}
end note
IdentityService -> EmailService: Send Password Reset Email
note right
{
    "to": "customer@example.com",
    "subject": "Reset your password",
    "body": "Click here to reset your password: https://website.com/reset?token=abc123" 
}
end note
EmailService -> IdentityService: Email Sent 200 OK
IdentityService -> Website: Password Reset Requested 202 Accepted

Customer -> Website: Click Password Reset Link
Website -> IdentityService: GET /passwordResets?token=abc123
IdentityService -> Website: Password Reset Form
Website -> Customer: Prompt for New Password

Customer -> Website: Enter New Password
Website -> IdentityService: PATCH /passwordResets?token=abc123
note right 
{
    "password": "newpassword"
}
end note
IdentityService -> Website: Password Reset 200 OK

Website -> Customer: Password Reset Successful!

@enduml
