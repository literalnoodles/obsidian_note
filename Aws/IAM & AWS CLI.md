#aws #aws-iam #aws-service
# IAM
## Users and groups
- IAM = Identity & Access Management, ==Global Service==
- **Root account** is created by default, shouldn't be used or shared
- **Users** are people within organization, can be ==grouped==
- **Groups** only contain users, **NOT** other group
- Users don't have to belong to a group, and can belong to multiple groups

![[Pasted image 20240226233131.png]]
## IAM Permissions:
- Users or Groups can be assigned JSON documents called policies
- These defined the permissions of the users
- Apply the ==**least privilege principle**==: no more than user needs
## IAM Policies:
- Can be inherit from multiple groups or attach directly
- IAM Policies consists of:
	- Version: "2012-10-17"
	- Id: identifier for the policy (optional)
	- Statement:
		- Sid: identifier for statement ("1")
		- Effect: (Allow, Deny)
		- Principal: account/user/role to apply
		- Action: list of actions this policy allows or deny
		- Resource: list of resources
		- Condition: conditions for when to apply or not (optional).
## IAM MFA:
- Password:
	- Set a minimum password length
	- Require specific character types:
		- including uppercase
		- lowercase
		- number
		- non-alphanumeric characters
	- Allow all IAM users to change their passwords
	- Require users to change their password after some time
	- Prevent password re-use
- MFA:
	- You want to protect your Root Account and IAM users.
	- MFA = password + security device
	- MFA devices options:
		- Virtual MFA device
			- Google Authenticator (phone)
			- Authy (multi-device)
		- U2F Security device (physical device)
			- Support multiple root and IAM users using 1 device
		- Hardware Key Fob MFA device
		- Hardware Key Fob MFA device for Aws GovCloud
## IAM Roles for Services
- To give permissions to AWS Services
- Example: EC2 Instance Roles, Lambda function roles
## IAM Security Tools
- IAM Credentials Report (account-level): list all accounts's users and status credential
- IAM Access Advisor (user-level): show permission granted to a user and last accessed
# AWS CLI

- 3 way to access AWS:
	- AWS Management Console (protected by password + MFA)
	- AWS CLI: protected by access key
	- AWS SDK: for code, protected by access key
- Access key:
	- Generated in AWS Console
	- Like password
