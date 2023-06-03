
@startuml Customer Resets Password
title Customer Resets Password
actor Customer
participant Website
participant IdentityService as "Identity Service"
participant EmailService as "Email Service"

Customer -> Website: Click "Forgot Password"
Website -> Customer: Prompt for email
Customer -> Website: Enter email
Website -> IdentityService: POST /passwordResets
note right
{
    "email": "customer@example.com"
}
end note
IdentityService -> EmailService: Send password reset email
note right
{
    "to": "customer@example.com",
    "subject": "Reset your password",
    "body": "Click here to reset your password: https://example.com/reset?token=abc123" 
}
end note
EmailService -> IdentityService: Email sent 200 OK
IdentityService -> Website: Password reset request created 201 Created

Customer -> Website: Click password reset link
Website -> IdentityService: GET /passwordResets?token=abc123
IdentityService -> Website: Password reset page
Website -> Customer: Prompt for new password
Customer -> Website: Enter new password
Website -> IdentityService: PATCH /passwordResets?token=abc123
note right 
{
    "password": "newpassword"
}
end note
IdentityService -> Website: Password updated 200 OK
Website -> Customer: Password successfully updated

@enduml
