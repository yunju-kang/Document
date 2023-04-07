### Identity Based IAM Policies

##### Policy Overview

1. Identity Based Policy

   : Attached to users, role, groups

   - Managed Policy

     :  Policy created and managed by customer.


   - Inline Policy

     : IAM Policy embedded within the user, group or role. When user, group or role is deleted, inline policy is also deleted.


2. Resource Based Policy 

   : Attached to resources like bucket or ec2

##### Implementing IAM Policies for All Users

Configure permission template at the Policy section.

```yaml
# This code was translated to yaml from json. Some lines would be incorrect gramaticaclly.
Version: "2012-10-17"
Statement:
	   - Effect: Deny # Allow or Deny
         Action: * # every action will be denied
         Resource : * # object that statement covers
         Condition : # (optional)
            NotIpAddress : # "Deny" the accesses from "the other" IP
            	"aws:SourceIP" : # (Deny + NotIp) => allow only these IP
            		- 192.0.2.0/24
            		  203.0.113.0/24
```

##### Implementing IAM Policies for Specific Users/Groups

Attach policies with the users/groups at the User groups section.

##### Enable Users to Configure Their Own Credentials and MFA

```yaml
Version : "2012-10-17"
Statement :
	- Sid : AllowIndividualUserToManageTheirOwnMFA
	  Effect : Allow
	  Action :
	  	- iam:CreateVirtualMFADevice
	  	  iam:DeleteVirtualMFADevice
	  	  iam:EnableMFADevice
	  	  iam:ResyncMFADevice
	  Resource : 
	  	- arn:aws:iam:*:mfa/${aws:username} # Individual can manage his own MFA
	  	  arn:aws:iam:*:user/${aws:username} # Individual can manage his own info
```

```yaml
Statement :
	- Sid : AllowIndividualUserToDeactivateTheirOwnMFAOnlyWhenUsingMFA
	  Effect : Allow
	  Action :
	  	- iam:DeactivateMFADevice
	  Resource : 
	  	- arn:aws:iam:*:mfa/${aws:username} 
	  	  arn:aws:iam:*:user/${aws:username} 
	  Condition : # set 'Allow'able user condition
	  	Bool : 
	  		'aws:MultiFactorAuthPresent' : true # Allow to deactivate "when using MFA"
```

```yaml
Statement :
	- Sid : BlockMostAccessUnlessSignedInWithMFA
	  Effect : Deny # block most access
	  NotAction : # (Deny + NotAction) => Deny except for these actions
	  	- iam:CreateViritualMFADevice
	  	  iam:DeleteVirtualMFADevice 
	  	  (...)
	  	  sts:GetSessionToken
	  Resource : 
	  	- arn:aws:iam:*:mfa/${aws:username} 
	  	  arn:aws:iam:*:user/${aws:username} 
	  Condition : 
	  	BoolIfExists : 
	  		'aws:MultiFactorAuthPresent' : false # 'Deny' statment works only when 'MFA_Present' is false
```

?What is the difference between Bool condition and boolifexist condition?

##### Using Managed Access Policies to Crete a Limited Administrator

```yaml
Statement : 
	(...)
	Condition :
		ArnEquals : 
			iam:PolicyArn : # den access from 
				- arn:aws:iam::299170662764:policy/MyProjectS3Access
				  arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess
```

1. Create Policy
   `aws iam create-policy --policy-name MyProjectLimitedAdminAccess --description "grants limited IAM administrator access" --policy-document file://LimitedAccessAdmin.json`

2. Create User

   `aws iam create-useer --user-name limitadmin`

   `aws iam create-access-key --user-name limitadmin`

   `aws iam create-login-profile --user-name limitadmin --passworld consult1`

3. Attach user to policy
   (managed policy)
   `aws iam attach-user-policy --ser-name limitedadmin --policy-arn arn:aws:iam::299170662766:policy/LimitedAdminAccess`

   (inline policy)
   `aws iam attach-user-policy --ser-name limitedadmin --policy-arn arn:aws:iam::aws:policy/IAMReadOnlyAccess`



### Using Policies to Access Resources

##### Attaching Policies to Groups for S3 Bucket Access

```
# can see bucket
Statement : 
	- Effect : Allow
	  Action : "s3:GetBucketLocation"
	  Resource : "*"
# access bucket in the "Q/* dirctory
	- Effect : Allow
	  Action :
		- s3:ListBucket
	  Resource : 
		- arn:aws:s3:::iamsecurecorp
	  Condition :
		Stringlike :
			s3:prefix :
				- "QA/*"
```



### Understanding and Applying IAM Roles

##### Strategies for IAM Roles