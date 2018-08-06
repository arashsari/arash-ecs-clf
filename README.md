
### Application URL (ALB DNS)

[Simple Go Web App](http://ecs-simple-go-webapp-1568821535.ap-southeast-2.elb.amazonaws.com/)


### Configuration
master.yaml is a master CloudFormation Stack that contains 6 other nested stacks. Most Parameters are set in master.yaml file and passed through nested stacks. 

Application source code is in the [services/simple-go-web-app-master](services/simple-go-web-app-master) folder and it's nested stack is in [services/simple-go-web-app-master/service.yaml](services/simple-go-web-app-master/service.yaml) that can be run seperatly to update the stack. 


### Launch this CloudFormation stack in your account:
Master.yaml will run all the stacks while you could run one of the stack to just update them. 

| AWS Region | Short name | | 
| -- | -- | -- |
| Asia Pacific (Sydney) | ap-southeast-2 | [![cloudformation-launch-button](img/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?stackName=Production&templateURL=https://s3.amazonaws.com/arash-ecs-clf/master.yaml) |



## Architecture

![iArchitecture](img/aws.png)


## Stacks Relationship (Entity Relationship)

![ER](img/template-ER.png)


The repository consists of a set of nested templates that deploy the following:

 - VPC: A tiered VPC with public and private subnets, spanning an AWS region.
 - ECS: A highly available ECS cluster deployed across two Availability Zones in an Auto Scaling group and that are AWS SSM enabled.
 - NAT: A pair of NAT gateways (one in each zone) to handle outbound traffic.
 - ALB: An Application Load Balancer ALB to the public subnets to handle inbound traffic. ALB path-based routes for each ECS service to route the inbound traffic to the correct service.
 - CloudWatch: Centralized container logging with Amazon CloudWatch Logs.
 - Health Check: A Lambda Function and Auto Scaling Lifecycle Hook to drain Tasks from your Container Instances when an Instance is selected for Termination in your Auto Scaling Group.
 - Service: A simple hello world microservice deployed as a ECS service. 



### Deploy multiple environments

Just add the stack name with Env name(dev, test, production) and deploy another CloudFormation stack. Stacks will add prefixed to all taggable resources so you can distinguish the different environment resources in the AWS Management Console. 



### TO DO
- finalise CI/CD and Test
- add another listener for HTTP to HTTPS redirection in Load Balancer. It requires another Load Balancer Listener to redired http to https then https forward to target group. The redirection need to have a SslPolicy, Certificates, and DefaultActions should be redirect
```
	HttpToHttpsListener:
		Type: AWS::ElasticLoadBalancingV2::Listener
		Properties: 
		  Certificates:
		    - Certificate
		  DefaultActions:
		    - Action
		  LoadBalancerArn: String
		  Port: Integer
		  Protocol: String
		  SslPolicy: String
```