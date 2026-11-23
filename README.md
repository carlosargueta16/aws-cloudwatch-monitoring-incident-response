# AWS CloudWatch Monitoring & Incident Response

## Project Overview

Built an AWS monitoring environment to collect EC2 performance metrics, centralize Apache web server logs, configure automated alerts, and investigate a simulated CPU performance incident.

**Project:** AWS-003  
**Region:** us-east-2 (Ohio)

## Technologies Used

- Amazon EC2 (Amazon Linux 2023)
- Amazon CloudWatch Metrics, Logs, Dashboards, and Alarms
- Amazon SNS
- AWS Systems Manager Session Manager
- Terraform
- Apache HTTP Server
- Linux and stress-ng

## Project Implementation

1. Deployed AWS infrastructure and an Apache web server using Terraform.
2. Installed and configured the CloudWatch Agent.
3. Collected CPU utilization, memory usage, and disk usage metrics.
4. Centralized Apache access and error logs in CloudWatch Logs.
5. Investigated and resolved a Linux permission issue preventing log collection.
6. Created a monitoring dashboard containing CPU, memory, and disk metrics.
7. Configured a high-CPU alarm with an 80% threshold and two consecutive five-minute evaluation periods.
8. Configured Amazon SNS email notifications and successfully tested alarm delivery.
9. Simulated elevated CPU utilization using stress-ng and investigated the results in CloudWatch.

## Incident Investigation

During a controlled CPU stress test, CPU utilization increased from approximately 1% to 71.75%, then returned to normal after the test ended.

The configured 80% alarm threshold was not breached for the required evaluation periods. Alarm notifications were verified separately by temporarily setting the alarm state through the AWS CLI.

## Key Skills Demonstrated

- Cloud infrastructure monitoring
- Centralized log collection
- Linux permission troubleshooting
- CloudWatch alarm configuration
- AWS SNS notification integration
- Performance investigation and recovery verification
- Infrastructure as Code with Terraform

## Project Documentation

Additional incident investigation details and monitoring evidence are provided in the project repository.
