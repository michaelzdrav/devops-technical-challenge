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



We could achieve fault tolerance and high availability through a few different managed services which we could consider:

- Load balanced EC2 instances with a database in RDS. However, this requires an orchestration layer and bit of consideration around EC2 placements in differing availability zones, autoscaling is not as simple
- ECS cluster to manage all containers for us with rules to control traffic between them, with a RDS instance. It has an orchestration layer, can autoscale easily, but isn't cloud agnostic and done in a way that we can support on-prem and cloud.
- EKS allows us to satisfy all of the above. We get an orchestration layer in K8s, can deploy in a region across Availability Zones using Subnets in differing AZ in the same VPC, and use helm charts that deploy in clusters in differing cloud providers/on-prem - the infrastructure declarations would be the only thing that really change

To handle orchestration, I will go with a managed Kubernetes instance. In AWS, this would be an Amazon EKS cluster that would be provisioned. I believe this would be the best way going forward as it would provide a cloud agnostic way to host applications, we can configure helm charts which can then be moved between cloud provider's managed K8S services or on-premise K8S clusters as well in the future. However, we can keep EKS as the managed service for now in AWS. Additionally, we can use helm charts and be declarative with our configuration, and be declarative with our Infrastructure as Code as well. 

EKS also allows us to use Karpenter for horizontal node autoscaling, which will allow our nodes to automatically scale upon an increase in traffic and load. We can use Horizontal Pod Autoscaler to scale the number of pods based on metrics like CPU and RAM usage. This should be sufficient for autoscaling of our EKS instance and application pods.

For security, we can leverage network policies to control traffic flow between microservices. Using a Load Balancer infront of our cluster also allows a single point of entry where we can restrict access from. Our microservices can also communicate securely with mTLS. We can also inject our own certificates to the cluster, which simplifies our certificate management. Our secrets can also be externally managed with AWS Secrets Manager. These are my general thoughts around why using Kubernetes in EKS is the best way going forward, there are a lot of integrations with AWS that help secure our application and handling scaling, as well as fault tolerance.


3. Describe the solution to automate the infrastructure deployment and prepare the most important snippets of code/configuration
4. Describe the solution to automate the microservices deployment and prepare the most important snippets of code/configuration
5. Describe the release lifecycle for the different components
6. Describe the testing approach for the infrastructure
7. Describe the monitoring approach for the solution