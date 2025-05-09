# DevOps Technical Challenge

## Requirements
- Automated Deployment
- Fault Tolerant / Highly Available
- Secure
- Autoscaling

## Assumptions
- We are to use only one infrastructure platform
- No budget constraints
- We want regional high availability, if global it would span regions in a cloud infra provider. Could be even more available in a multi-cloud blue/green deployment 

## Challenge Tasks

1. Choose the infrastructure platform to use
I will use Amazon Web Services (AWS), given it is a proven Platform as a Service and market leader in its industry. It also has proven Infrastructure as a Service offerings that are highly scalable and available, utilised by the largest companies across the globe. 

AWS allows us to leverage services like Secrets Manager to handle our secrets, ECS for containers in a managed service, EKS for a managed K8S instance, in-built monitoring through CloudWatch and also Managed Grafana/Prometheus instances. Terraform also supports AWS, and the services are all able to be leveraged declaratively with state management. AWS also allows us to securely communicate between components by using their backend network infrastructure. We can leverage AWS Security Groups to define rules to also control traffic flow and permit/reject traffic between microservices and AWS infrastructure.

Ideally - we would remove access from everyone and only allow CI/CD deployments to the cluster and AWS account entirely. If required, I would also configure IAM roles to only allow specific users to have access to our environment.

2. Choose the orchestration technology and their components
3. Describe the solution to automate the infrastructure deployment and prepare the most important snippets of code/configuration
4. Describe the solution to automate the microservices deployment and prepare the most important snippets of code/configuration
5. Describe the release lifecycle for the different components
6. Describe the testing approach for the infrastructure
7. Describe the monitoring approach for the solution