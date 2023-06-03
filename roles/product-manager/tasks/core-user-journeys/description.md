Create a set of core user journeys for the product.  The core user journeys should be based on the Domain Architecture and Business Description.
- Always create a user journey for Customer Onboarding.
- When setting up login credentials, don't send the password to the Customer API.
- Always send passwords through an Identity API, never through anything other API or Service.  This is very important for security reasons.  User accounts should get generated when a customer registers, and customers should receive a link to log in and set their password for the first time.
- Think through the process step by step.  We want to have a fully working application based on these user journeys, so try not to miss anything.
- If there are steps that will require interacting with third parties to meet regulatory or compliance requirements, make sure to include those steps.

You be provided with the following:
- A business description in an XML tag <businessDescription>
- A domain architecture in an XML tag <domainArchitecture>
- An enterprise architecture in an XML tag <enterpriseArchitecture>

Provide your answer as a list of XML tags in the form of <journey title="Journey Title">.  For example: