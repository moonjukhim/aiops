
```text
Create a complete AWS CloudFormation YAML template for an AIOps demo environment with the following requirements:

[Overall Goal]

* The template should support an AIOps use case integrated with Amazon Bedrock Agents.
* It must include monitoring, analysis, and automated remediation capabilities.

[Lambda Functions]

1. Create a Lambda function named "CheckCloudWatchAlarmsFunction"

   * Runtime: python3.13
   * It should:

     * Check CloudWatch alarms (specifically CPU alarm for EC2)
     * If no alarms are in ALARM state, return a "no issue" message
     * If alarms exist, return EC2 instance ID and issue description
   * Must support Bedrock agent invocation (structured response format)

2. Create a Lambda function named "RemediateEC2OperationalIssueFunction"

   * Runtime: python3.13
   * It should:

     * Reboot EC2 instances
     * Create snapshot of EC2 volumes
   * Must parse instance ID from request input

[Permissions]

* Each Lambda must have an IAM Role with:

  * CloudWatch read permissions
  * EC2 describe permissions
  * EC2 reboot and snapshot permissions (for remediation Lambda)
* Add Lambda invoke permissions for:

  * Principal: bedrock.amazonaws.com

[Networking]

* Create a VPC (CIDR: 10.0.0.0/16)
* Create 2 public subnets in different AZs
* Attach Internet Gateway
* Create Route Table and associate subnets

[Security]

* Create a Security Group:

  * Allow SSH (port 22) inbound from configurable CIDR
  * Allow all outbound traffic

[EC2]

* Launch one EC2 instance:

  * Instance type: t2.micro
  * Public IP enabled
  * KeyPair created in template
  * Amazon Linux 2023 AMI (region-based mapping)

[Monitoring]

* Create a CloudWatch Alarm:

  * Metric: CPUUtilization
  * Threshold: 70%
  * Period: 60 seconds
  * EvaluationPeriods: 1
  * No actions enabled

[Outputs]

* Output:

  * Lambda ARNs
  * KeyPair name

[Other Requirements]

* Use Parameters for:

  * Prefix
  * SecurityGroupIngressCidrIp
* Use Mappings for AMI per region
* Follow AWS best practices for naming and structure
* Ensure YAML is valid and deployable

Return only the final CloudFormation YAML.
```