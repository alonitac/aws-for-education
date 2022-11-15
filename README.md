# AWS Education Accounts

## Education account setting

- All students IAM users must reside in the existed `students` IAM group.
- Users outside this group are not restricted.
- Students can create IAM users and groups but aren't able to associate any policies on them.
- Students can create IAM role **and associate policies on them!**
- Students can operate on US and EU regions only.
- **ALL** EC2 instances are stopped twice a day (currently at 4pm and midnight).
- Unused EC2 instances are **terminated** after 30 days on inactivity.
- RDS databases are being deleted after 30 days.
- LoadBalancers are being deleted after 30 days.

## Policies enforced

- Read-only operations on AWS Marketplace.
- Deny account audit trail deletion and core configuration changes (CloudTrail, Config, Guard).
- Deny organization leave.
- Deny DynamoDB reserved capacity purchases.
- Deny privilege escalation (students cannot leave students group or change any policy of it).
- Deny VPC peering.
- Deny enablement of non US and EU regions.
- Limit EC2 instance type to `*.nano`, `*.micro`, `*.small`.
- Deny reserved instances purchases.
- Limit EBS volume type to `gp2` and `magnetic` and max size of **30 GB**.
- Protect account maintenance resources (the CloudFormation stack, Lambda function etc..) from being deleted by students. 

## Deploy the CloudFormation stack 

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/).
2. If this is a new CloudFormation account, choose **Create New Stack**\. Otherwise, choose **Create Stack** (standard).
3. In the **Template** section, select **Upload a template file**, then click **Choose file** and upload the [cloudformation.yaml](cloudformation.yaml) file. Then choose **Next**.
4. In the **Specify Details** section, enter a stack name in the **Name** field. For this example, use **accountMaintenanceStack**. The stack name can't contain spaces.
5. On the **Parameters** page, you'll recognize the parameters from the Parameters that protect and clean the account from student's work. Please provide values for all parameters.
6. Choose **Next** with all the other default configurations.
7. On the last step, review the information for the stack\. When you're satisfied with the settings, choose **Create**.

## Update the CloudFormation stack 

1. Given an existed stack, click on it.
2. On the stack details page, click the **Update** button.
3. Choose **Replace current template**.
4. Proceed as described in the _Deploy_ section above.

# Give exemption to resources

You can tag EC2 instance, Load Balancers and RDS Databases with a `lifespan` tag key and value of the number of days would you like your resource to be protected from being deleted.


