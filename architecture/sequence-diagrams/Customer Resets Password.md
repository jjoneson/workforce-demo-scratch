
@startuml Customer Resets Password
title Customer Resets Password
actor Customer
participant Website
participant IdentityService as "Identity Service"
participant EmailService as "Email Service"

Customer -> Website: Click "Forgot Password"
Website -> Customer: Prompt for Email
Customer -> Website: Enter Email
Website -> IdentityService: GET /customers?email={email}
IdentityService -> Website: Customer Found 200 OK
Website -> EmailService: Send Password Reset Email 
note right
{
    "body": "Please click the link below to reset your password.",
    "link": "https://website.com/reset-password?token={token}"
    "customer_id": 1,
}
end note
EmailService -> Website: Email Sent 200 OK
Website -> Customer: Email Sent

Customer -> Website: Click Password Reset Link
Website -> IdentityService: GET /customers/reset-password?token={token}
IdentityService -> Website: Token Valid 200 OK
Website -> Customer: Prompt for New Password
Customer -> Website: Enter New Password
Website -> IdentityService: PATCH /customers/{id}
note right 
{
    "password": "newpassword"
}
end note
IdentityService -> Website: Password Updated 200 OK
Website -> Customer: Password Reset Successful

@enduml
