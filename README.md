# INTRODUCTION
This project focuses on enhancing the security posture of an AWS environment by monitoring and managing Amazon EC2 security group configurations. The core objective is to ensure that only approved inbound ports are open, in alignment with organizational security policies.
By leveraging AWS Config and AWS Lambda, the solution automatically detects and remediates unauthorized changes to security group inbound rules. When a non-compliant modification is identified—such as the opening of an unapproved port—AWS Config triggers a Lambda function to revert the security group to its approved state, maintaining continuous compliance without manual intervention.
This hands-on implementation demonstrates a proactive approach to cloud security using AWS-native services.
####  CURRENT ARCHITECTURE 
<img width="1569" alt="start-arch" src="https://github.com/user-attachments/assets/65e4a703-42a0-4c64-bee8-c5e38a4a251a" />
####  ARCHITECTURE AT THE END OF THE PROJECT
<img width="1572" alt="end-arch" src="https://github.com/user-attachments/assets/71157806-c92c-4026-8250-e6bdd5eb27b1" /> 
After the solution, a security incident will be remediated through the following steps:

- The AWS Config rule will monitor for any changes to security groups that are tracked in the AWS Config resources inventory.
- When the rule notices that changes were made to a security group, the rule will invoke the Lambda function.
- The function will remediate the situation by updating the desired inbound rule configuration for the security group.

### Creating IAM ROLES FOR LAMBDA FUCTION 
First create a custom role. This role defines the permissions that the Lambda function will have when it runs. The policy will allow the Lambda function to add or remove inbound rules on Amazon EC2 security groups. The policy will also allow the Lambda function to create and write events to CloudWatch logs.
### Next Create  AWS_ConfigRole with a S3Access and AWS_ConfigRole Permissions 
 These permissions will allow AWS Config to write CloudWatch log files to Amazon S3 and monitor one of the Regions in the AWS account.
![Screenshot 2025-07-10 005104](https://github.com/user-attachments/assets/4ba522fe-2566-4365-800c-5f6f4f8bf15f) 

# Setting up AWS Config to monitor resources
1. Specific resource types.
2. AWS EC2 SecurityGroup
3. For Frequency choose Continuous.
4. IAM role for AWS Config Choose AwsConfigRole.
5. In the Delivery channel section, notice that AWS Config will store findings in an S3 bucket by default. Keep the default setting.
6. AWS Managed Rules
7. Confirm

#  Modifying a security group that AWS Config monitors
configure new inbound rule settings in one of the security groups that is listed in the AWS Config resource inventory. The purpose is to effectively emulate a security incident. Some of the inbound rule settings that you will define during this task won't match the desired settings, which you will define in a later task.  

![Screenshot 2025-07-10 013329](https://github.com/user-attachments/assets/2b8760dac775-4f72-82d8-274707c414a9)
# Create a Lambda Function
![Screenshot 2025-07-10 014520](https://github.com/user-attachments/assets/d8a83814-5190-42e0-ad39-ef1ee09b6129)

# Create an AWS Config rule that calls a Lambda function
1. Navigate to the AWS Config console.
2. Choose Add rule.
3. Select rule type, choose Create custom Lambda rule.
4. On the Configure rule page, configure the following:
    - AWS Lambda function ARN
    - Name
    - Description
    - Trigger type
    - Scope of changes
    - Resource type
    - add a parameter (key:, Value)
      
![Screenshot 2025-07-10 015722](https://github.com/user-attachments/assets/6f6ba228-11af-4901-a1c9-1331efc8030b)
# Revisiting the security group configuration
On the Inbound rules tab, notice that only HTTP and HTTPS traffic is permitted.
The inbound rules should now look like the rules in the following screenshot (although your security group rule IDs are different).  Recall that you defined inbound rules for SMTPS and IMAPS, as well as HTTP and HTTPS, on this security group. However, the rules for SMTPS and IMAPS no longer exist. 
![Screenshot 2025-07-10 020323](https://github.com/user-attachments/assets/7b2dbd4a-3b3a-4ed5-992a-701c11e2d2ef)

# CloudWatch logs for verification
Each event provides details about the action that the Lambda function took. In one of the events, you should find details showing that the inbound rules that you manually added for SMTPS (TCP port 465) and IMAPS (TCP port 993) were removed.

The other filtered events logged the changes to the other two security groups that exist in your account. These security groups are also in the resources inventory that your AWS Config rule is monitoring.
![Screenshot 2025-07-10 021230](https://github.com/user-attachments/assets/e5d948df-8668-49f6-9944-84331efb727b)

![Screenshot 2025-07-10 021914](https://github.com/user-attachments/assets/fbdf6df0-7bbf-47bf-a5c4-57eafcb70c61)
