# AWS-serverless

Deployment strategy using AWS-Services that allows routing traffic from one server to another without any downtime.

# Architecture 
## Infra
Complete Infrastructure deployment automated through IAC .
Here I am using an AWS Serverless Application Model (SAM) template. SAM is a CloudFormation extension that is optimized for serverless, and provides a standard way to create a complete serverless application.

## Prerequisites for IAC
1. Setup IAM role for Infra deployment
2. Setup IAM role for CICD pipeline 

## AWS
Deploying two Lambda functions , routed through an API gateway with optional weighted routing 80-20 (with optinal sticky sessions) to enable routing with zero downtime.
Also as shown below for DR & HA capabilities lambdas can exist in different zones.We can utilise Route 53 health check to trigger routing in case of failover.
![AWS lambdas](https://github.com/ImmLearning/AWS-nginx/assets/51955196/8615b937-f90f-483f-b9ee-8f8c5061ba7b)

##  CICD
CICD pipeline created in code deploy with manual approvals for the different stages.

## Code repo 
Code hosted in this Github repo and optionally deployment can be triggered using a web hook integration following GitOps best practices.

