# Problem statement 
  Implement a deployment strategy using AWS-Services that allows routing traffic from one server to another without any downtime.The routing should be controllable i.e weighted routing possible.

# AWS-serverless Architecture
Creating a Deployment strategy using AWS-Services that allows routing traffic from one server to another without any downtime.
For the serverless model approach we are using AWS Lambda functions as servers for handling clinet reponse, which sends the Lambda region information to differentiate between the two Lambdas ( Two Lambdas deployed in this case but its easily scalabe )

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

##  CICD
CICD pipeline created in code deploy with manual approvals for the different stages.

## Code repo 
Code hosted in this Github repo and optionally deployment can be triggered using a web hook integration following GitOps best practices.

### Explaingint this Design 
1. Usual Architecture for this scenario - 
   The general solution for a multi server , weighted routing is by hosting Nginx servers on EC2 machines as part of an autoscaling group with an ALB in front to handle the weighted routing.

2. Why Serverless Architecture is better for this Scenario
   In this particular, we only expect a short response for any client server calls made to the Lmabda / API gateway , as such the Compute cost are minimal - which makes serverless approach most suitable.
   The serverless approach is highly scalbe and by deploying in multiple region ( or optionally also in AZs ) its High Availability, High Scalabily .
    As added in the Optional setup deploying a Route 53 based health check also provides with with DR and High Tolerance.
   
4. Disadvantages 
   Serverless has few disadvantages too. In this setup once we can better and easily protect against DDoS attack but at the same time once we reach a scale > 1 million API calls the Lambda calls as well as the compute for Lambda trigger ( though however small for single client request ) would start to add up and this would no longer be a cost optimised solution. We cannot use reserved instances and other discounted billing options etc.
   Optionally we can connect the health check with automated trigger to deploy more Lambda functions which though faster than bringer up servers , might else take some time. Though a pre-emptive load optimation would be able to mitigate this issue. 
