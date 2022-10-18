# AWS Education Accounts Deployment

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

# Working locally with Python and MFA 

Here is an example of using Python to communicate with AWS API when MFA is enforced (We assume the machine has credentials configured locally).

```python
import boto3
from botocore.session import Session

mfa_otp = input("Enter the MFA code: ")

mfa_creds = boto3.client('sts').get_session_token(
    DurationSeconds=36000,
    SerialNumber=Session().get_scoped_config().get('mfa_serial'),
    TokenCode=mfa_otp
)

session = boto3.session.Session(
    aws_access_key_id=mfa_creds['Credentials']['AccessKeyId'],
    aws_secret_access_key=mfa_creds['Credentials']['SecretAccessKey'],
    aws_session_token=mfa_creds['Credentials']['SessionToken']
)

ec2 = session.client('ec2', region_name='us-east-1')
descriptions = ec2.describe_instances()
```



