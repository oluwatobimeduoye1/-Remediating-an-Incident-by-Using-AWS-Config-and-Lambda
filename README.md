# INTRODUCTION
This project focuses on enhancing the security posture of an AWS environment by monitoring and managing Amazon EC2 security group configurations. The core objective is to ensure that only approved inbound ports are open, in alignment with organizational security policies.
By leveraging AWS Config and AWS Lambda, the solution automatically detects and remediates unauthorized changes to security group inbound rules. When a non-compliant modification is identified—such as the opening of an unapproved port—AWS Config triggers a Lambda function to revert the security group to its approved state, maintaining continuous compliance without manual intervention.
This hands-on implementation demonstrates a proactive approach to cloud security using AWS-native services.
