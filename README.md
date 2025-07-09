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
