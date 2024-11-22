# IAM
AWS Identity and Access Management (IAM) is a web service that enables secure access control for AWS resources. 
It allows you to manage permissions and authenticate users, groups, roles, and services.

basically IAM is a service provided by aws to ensure secure access to any of the resources and services provided by aws in an account.

# Core Components of IAM
### a) Users: individual who you provide permission
Represent individual entities (e.g., developers, administrators).
Can have long-term credentials (access keys and passwords).
Best for human access.
### b) Groups: the group who you want to provide permission (Easy to give permission for a specific task to a specific group)
Logical collection of users with shared permissions.
Simplifies management by associating permissions at the group level.
### c) Roles: give permission to aws resources/services (eg ec2, lambda...)
Assume temporary permissions for AWS resources or third-party services.
Commonly used for applications or AWS services needing access to other AWS resources.
### d) Policies: 
JSON documents defining permissions.
Types:
#### Managed Policies: AWS or user-created reusable policies.
#### Inline Policies: Embedded directly within a user, group, or role.
### Identity Providers:
Allows integration with third-party identity providers (e.g., Microsoft Active Directory, Google) or SAML 2.0 for Single Sign-On (SSO).
### Access Keys:
Pair of keys (access key ID and secret access key) used for programmatic access.
### MFA (Multi-Factor Authentication):
Adds an extra layer of security by requiring an OTP from a registered device.


# use case
## IAM in Action
#### Scenario: You have an EC2 instance that needs access to an S3 bucket.

Solution:
Create an IAM role with an S3 access policy.

Attach the role to the EC2 instance.

The instance now uses temporary credentials to interact securely with S3.
