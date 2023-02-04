# Identity and access management for Amazon RDS<a name="UsingWithRDS.IAM"></a>





AWS Identity and Access Management \(IAM\) is an AWS service that helps an administrator securely control access to AWS resources\. IAM administrators control who can be *authenticated* \(signed in\) and *authorized* \(have permissions\) to use Amazon RDS resources\. IAM is an AWS service that you can use with no additional charge\.

**Topics**
+ [Audience](#security_iam_audience)
+ [Authenticating with identities](#security_iam_authentication)
+ [Managing access using policies](#security_iam_access-manage)
+ [How Amazon RDS works with IAM](security_iam_service-with-iam.md)
+ [Identity\-based policy examples for Amazon RDS](security_iam_id-based-policy-examples.md)
+ [AWS managed policies for Amazon RDS](rds-security-iam-awsmanpol.md)
+ [Amazon RDS updates to AWS managed policies](rds-manpol-updates.md)
+ [Preventing cross\-service confused deputy problems](cross-service-confused-deputy-prevention.md)
+ [IAM database authentication for MariaDB, MySQL, and PostgreSQL](UsingWithRDS.IAMDBAuth.md)
+ [Troubleshooting Amazon RDS identity and access](security_iam_troubleshoot.md)

## Audience<a name="security_iam_audience"></a>

How you use AWS Identity and Access Management \(IAM\) differs, depending on the work you do in Amazon RDS\.

**Service user** – If you use the Amazon RDS service to do your job, then your administrator provides you with the credentials and permissions that you need\. As you use more Amazon RDS features to do your work, you might need additional permissions\. Understanding how access is managed can help you request the right permissions from your administrator\. If you cannot access a feature in Amazon RDS, see [Troubleshooting Amazon RDS identity and access](security_iam_troubleshoot.md)\.

**Service administrator** – If you're in charge of Amazon RDS resources at your company, you probably have full access to Amazon RDS\. It's your job to determine which Amazon RDS features and resources your employees should access\. You must then submit requests to your administrator to change the permissions of your service users\. Review the information on this page to understand the basic concepts of IAM\. To learn more about how your company can use IAM with Amazon RDS, see [How Amazon RDS works with IAM](security_iam_service-with-iam.md)\.

**Administrator** – If you're an administrator, you might want to learn details about how you can write policies to manage access to Amazon RDS\. To view example Amazon RDS identity\-based policies that you can use in IAM, see [Identity\-based policy examples for Amazon RDS](security_iam_id-based-policy-examples.md)\.

## Authenticating with identities<a name="security_iam_authentication"></a>

Authentication is how you sign in to AWS using your identity credentials\. You must be *authenticated* \(signed in to AWS\) as the AWS account root user, as an IAM user, or by assuming an IAM role\.

You can sign in to AWS as a federated identity by using credentials provided through an identity source\. AWS IAM Identity Center \(successor to AWS Single Sign\-On\) \(IAM Identity Center\) users, your company's single sign\-on authentication, and your Google or Facebook credentials are examples of federated identities\. When you sign in as a federated identity, your administrator previously set up identity federation using IAM roles\. When you access AWS by using federation, you are indirectly assuming a role\.

Depending on the type of user you are, you can sign in to the AWS Management Console or the AWS access portal\. For more information about signing in to AWS, see [How to sign in to your AWS account](https://docs.aws.amazon.com/signin/latest/userguide/how-to-sign-in.html) in the *AWS Sign\-In User Guide*\.

If you access AWS programmatically, AWS provides a software development kit \(SDK\) and a command line interface \(CLI\) to cryptographically sign your requests using your credentials\. If you don't use AWS tools, you must sign requests yourself\. For more information about using the recommended method to sign requests yourself, see [Signature Version 4 signing process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) in the *AWS General Reference*\.

Regardless of the authentication method that you use, you might be required to provide additional security information\. For example, AWS recommends that you use multi\-factor authentication \(MFA\) to increase the security of your account\. To learn more, see [Multi\-factor authentication](https://docs.aws.amazon.com/singlesignon/latest/userguide/enable-mfa.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide* and [Using multi\-factor authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.

### AWS account root user<a name="security_iam_authentication-rootuser"></a>

  When you create an AWS account, you begin with one sign\-in identity that has complete access to all AWS services and resources in the account\. This identity is called the AWS account *root user* and is accessed by signing in with the email address and password that you used to create the account\. We strongly recommend that you don't use the root user for your everyday tasks\. Safeguard your root user credentials and use them to perform the tasks that only the root user can perform\. For the complete list of tasks that require you to sign in as the root user, see [Tasks that require root user credentials](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html) in the *AWS Account Management Reference Guide*\. 

### Federated identity<a name="security_iam_authentication-federatedidentity"></a>

As a best practice, require human users, including users that require administrator access, to use federation with an identity provider to access AWS services by using temporary credentials\.

A *federated identity* is a user from your enterprise user directory, a web identity provider, the AWS Directory Service, the Identity Center directory, or any user that accesses AWS services by using credentials provided through an identity source\. When federated identities access AWS accounts, they assume roles, and the roles provide temporary credentials\.

For centralized access management, we recommend that you use AWS IAM Identity Center \(successor to AWS Single Sign\-On\)\. You can create users and groups in IAM Identity Center, or you can connect and synchronize to a set of users and groups in your own identity source for use across all your AWS accounts and applications\. For information about IAM Identity Center, see [What is IAM Identity Center?](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\.

### IAM users and groups<a name="security_iam_authentication-iamuser"></a>

An *[IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html)* is an identity within your AWS account that has specific permissions for a single person or application\. Where possible, we recommend relying on temporary credentials instead of creating IAM users who have long\-term credentials such as passwords and access keys\. However, if you have specific use cases that require long\-term credentials with IAM users, we recommend that you rotate access keys\. For more information, see [Rotate access keys regularly for use cases that require long\-term credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#rotate-credentials) in the *IAM User Guide*\.

An [https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html) is an identity that specifies a collection of IAM users\. You can't sign in as a group\. You can use groups to specify permissions for multiple users at a time\. Groups make permissions easier to manage for large sets of users\. For example, you could have a group named *IAMAdmins* and give that group permissions to administer IAM resources\.

Users are different from roles\. A user is uniquely associated with one person or application, but a role is intended to be assumable by anyone who needs it\. Users have permanent long\-term credentials, but roles provide temporary credentials\. To learn more, see [When to create an IAM user \(instead of a role\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html#id_which-to-choose) in the *IAM User Guide*\.

You can authenticate to your DB instance using IAM database authentication\.

IAM database authentication works with the following DB engines:
+ RDS for MariaDB
+ RDS for MySQL
+ RDS for PostgreSQL

For more information about authenticating to your DB instance using IAM, see [IAM database authentication for MariaDB, MySQL, and PostgreSQL](UsingWithRDS.IAMDBAuth.md)\.

### IAM roles<a name="security_iam_authentication-iamrole"></a>

An *[IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)* is an identity within your AWS account that has specific permissions\. It is similar to a user, but is not associated with a specific person\. You can temporarily assume an IAM role in the AWS Management Console by [switching roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-console.html)\. You can assume a role by calling an AWS CLI or AWS API operation or by using a custom URL\. For more information about methods for using roles, see [Using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html) in the *IAM User Guide*\.

IAM roles with temporary credentials are useful in the following situations:
+ **Temporary user permissions** – A user can assume an IAM role to temporarily take on different permissions for a specific task\. 
+ **Federated user access** –  To assign permissions to a federated identity, you create a role and define permissions for the role\. When a federated identity authenticates, the identity is associated with the role and is granted the permissions that are defined by the role\. For information about roles for federation, see [ Creating a role for a third\-party Identity Provider](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp.html) in the *IAM User Guide*\. If you use IAM Identity Center, you configure a permission set\. To control what your identities can access after they authenticate, IAM Identity Center correlates the permission set to a role in IAM\. For information about permissions sets, see [ Permission sets](https://docs.aws.amazon.com/singlesignon/latest/userguide/permissionsetsconcept.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\. 
+ **Cross\-account access** – You can use an IAM role to allow someone \(a trusted principal\) in a different account to access resources in your account\. Roles are the primary way to grant cross\-account access\. However, with some AWS services, you can attach a policy directly to a resource \(instead of using a role as a proxy\)\. To learn the difference between roles and resource\-based policies for cross\-account access, see [How IAM roles differ from resource\-based policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html) in the *IAM User Guide*\.
+ **Cross\-service access** –  Some AWS services use features in other AWS services\. For example, when you make a call in a service, it's common for that service to run applications in Amazon EC2 or store objects in Amazon S3\. A service might do this using the calling principal's permissions, using a service role, or using a service\-linked role\. 
  + **Principal permissions** –  When you use an IAM user or role to perform actions in AWS, you are considered a principal\. Policies grant permissions to a principal\. When you use some services, you might perform an action that then triggers another action in a different service\. In this case, you must have permissions to perform both actions\. To see whether an action requires additional dependent actions in a policy, see [Actions, Resources, and Condition Keys for Amazon RDS](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonrds.html.html) in the *Service Authorization Reference*\. 
  + **Service role** –  A service role is an [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) that a service assumes to perform actions on your behalf\. An IAM administrator can create, modify, and delete a service role from within IAM\. For more information, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\. 
  + **Service\-linked role** –  A service\-linked role is a type of service role that is linked to an AWS service\. The service can assume the role to perform an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view, but not edit the permissions for service\-linked roles\. 
+ **Applications running on Amazon EC2** –  You can use an IAM role to manage temporary credentials for applications that are running on an EC2 instance and making AWS CLI or AWS API requests\. This is preferable to storing access keys within the EC2 instance\. To assign an AWS role to an EC2 instance and make it available to all of its applications, you create an instance profile that is attached to the instance\. An instance profile contains the role and enables programs that are running on the EC2 instance to get temporary credentials\. For more information, see [Using an IAM role to grant permissions to applications running on Amazon EC2 instances](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) in the *IAM User Guide*\. 

To learn whether to use IAM roles, see [When to create an IAM role \(instead of a user\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html#id_which-to-choose_role) in the *IAM User Guide*\.

## Managing access using policies<a name="security_iam_access-manage"></a>

You control access in AWS by creating policies and attaching them to IAM identities or AWS resources\. A policy is an object in AWS that, when associated with an identity or resource, defines their permissions\. AWS evaluates these policies when an entity \(root user, user, or IAM role\) makes a request\. Permissions in the policies determine whether the request is allowed or denied\. Most policies are stored in AWS as JSON documents\. For more information about the structure and contents of JSON policy documents, see [Overview of JSON policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#access_policies-json) in the *IAM User Guide*\.

An administrator can use policies to specify who has access to AWS resources, and what actions they can perform on those resources\. Every IAM entity \(permission set or role\) starts with no permissions\. In other words, by default, users can do nothing, not even change their own password\. To give a user permission to do something, an administrator must attach a permissions policy to a user\. Or the administrator can add the user to a group that has the intended permissions\. When an administrator gives permissions to a group, all users in that group are granted those permissions\.

IAM policies define permissions for an action regardless of the method that you use to perform the operation\. For example, suppose that you have a policy that allows the `iam:GetRole` action\. A user with that policy can get role information from the AWS Management Console, the AWS CLI, or the AWS API\.

### Identity\-based policies<a name="security_iam_access-manage-id-based-policies"></a>

Identity\-based policies are JSON permissions policy documents that you can attach to an identity, such as a permission set or role\. These policies control what actions that identity can perform, on which resources, and under what conditions\. To learn how to create an identity\-based policy, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\.

Identity\-based policies can be further categorized as *inline policies* or *managed policies*\. Inline policies are embedded directly into a single permission set or role\. Managed policies are standalone policies that you can attach to multiple permission sets and roles in your AWS account\. Managed policies include AWS managed policies and customer managed policies\. To learn how to choose between a managed policy or an inline policy, see [Choosing between managed policies and inline policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#choosing-managed-or-inline) in the *IAM User Guide*\.

For information about AWS managed policies that are specific to Amazon RDS, see [AWS managed policies for Amazon RDS](rds-security-iam-awsmanpol.md)\.

### Other policy types<a name="security_iam_access-manage-other-policies"></a>

AWS supports additional, less\-common policy types\. These policy types can set the maximum permissions granted to you by the more common policy types\. 
+ **Permissions boundaries** – A permissions boundary is an advanced feature in which you set the maximum permissions that an identity\-based policy can grant to an IAM entity \(permission set or role\)\. You can set a permissions boundary for an entity\. The resulting permissions are the intersection of entity's identity\-based policies and its permissions boundaries\. Resource\-based policies that specify the permission set or role in the `Principal` field are not limited by the permissions boundary\. An explicit deny in any of these policies overrides the allow\. For more information about permissions boundaries, see [Permissions boundaries for IAM entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html) in the *IAM User Guide*\.
+ **Service control policies \(SCPs\)** – SCPs are JSON policies that specify the maximum permissions for an organization or organizational unit \(OU\) in AWS Organizations\. AWS Organizations is a service for grouping and centrally managing multiple AWS accounts that your business owns\. If you enable all features in an organization, then you can apply service control policies \(SCPs\) to any or all of your accounts\. The SCP limits permissions for entities in member accounts, including each AWS account root user\. For more information about Organizations and SCPs, see [How SCPs work](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_about-scps.html) in the *AWS Organizations User Guide*\.
+ **Session policies** – Session policies are advanced policies that you pass as a parameter when you programmatically create a temporary session for a role or federated user\. The resulting session's permissions are the intersection of the permission sets or role's identity\-based policies and the session policies\. Permissions can also come from a resource\-based policy\. An explicit deny in any of these policies overrides the allow\. For more information, see [Session policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_session) in the *IAM User Guide*\. 

### Multiple policy types<a name="security_iam_access-manage-multiple-policies"></a>

When multiple types of policies apply to a request, the resulting permissions are more complicated to understand\. To learn how AWS determines whether to allow a request when multiple policy types are involved, see [Policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) in the *IAM User Guide*\.