Assignment 2.13

Make a comparison between Serverless Framework and Terraform as tools for IaC by answering:
●	What type of infrastructure and application deployments are each tool best suited for?

Answer: 

Serverless Framework:

Optimal For: 	Implementing serverless functions and applications. It excels in 
environments driven by events.

Infrastructure: 	Primarily focus on application-level cloud resources necessary for a serverless app.

This includes:
i) Compute: 		AWS Lambda, Azure Functions, Google Cloud Functions.
ii) Events/Triggers: 	API Gateway endpoints, S3 buckets, SQS queues, SNS 
topics, CloudWatch Events.
iii) App Layers: 		DynamoDB tables, S3 buckets serving as frontends.
iv) App Deployment: 	It goes beyond infrastructure to actually zip and deploy your application code to the function runtime. It manages function versions, aliases, and can be integrated into CI/CD pipelines for the application layer.
Terraform: 

Optimal For: 	Setting up and supervising a diverse array of infrastructure, both cloud-based and on-
premises. It functions as a versatile tool for infrastructure deployment.

Infrastructure: 	Capable of handling almost any resource available via an API, spanning from high-
level components to fundamental ones.

i) Foundational: 		VPCs, subnets, security groups, IAM roles, and policies.
ii) Compute: 		EC2 instances, ECS clusters, Kubernetes engine clusters, 
and Lambda functions.
iii) Data Stores: 		RDS databases, S3 buckets, ElastiCache clusters.
iv) Networking: 		Load Balancers, Route53 DNS records.
v) App Deployment: 	The primary function of Terraform usually concludes with 
setting up the infrastructure where applications will be executed (for instance, generating an ECS task definition or a Kubernetes pod). It generally does not manage the process of deploying application code directly (though it may initiate other tools).

●	How do their primary objectives differ?

Answer: 

Serverless Framework:	
The primary objective is to develop, deploy, and manage serverless applications with a fantastic
developer experience. It aims to abstract away the complexity of underlying infrastructure so
developers can focus on application logic.

Terraform: 
The main goal is to provide declarative, consistent, and reproducible infrastructure provisioning
across providers. The objective is to describe and maintain entire infrastructure lifecycles.
It delivers a cohesive method to manage infrastructure across various service providers (AWS,
Azure, GCP, etc.). It's geared towards "infrastructure."

●	How do they differ in terms of learning curve and ease of use for developers or DevOps teams?

Answer: 

Serverless Framework:

Learning Curve: 		Relatively low for developers, especially for those constructing serverless 
applications. The YAML/JSON setup is easy to use for specifying functions and their events.
Ease of Use: 	Outstanding for its designated purpose. Commands like serverless deploy and serverless invoke are user-friendly and straightforward. It features practical defaults that enhance development speed.
Drawback: 	It may appear "magical" or restrictive when trying to set up resources beyond its main focus. Although CloudFormation syntax can be utilized, it can complicate the configuration.

Terraform:

Learning Curve: 	Generally more challenging. You need to grasp the essential concepts of Terraform: HashiCorp Configuration Language (HCL), state management, providers, resources, data sources, and the plan/apply process.
Ease of Use: 	Very powerful but requires more upfront knowledge. There are no hidden defaults; you explicitly define every resource and its properties. This explicit nature leads to a more robust and understandable infrastructure definition in the long run.
Benefit: 	The learning investment pays off as it provides a consistent method to manage any infrastructure component.

●	What are the differences in how each tool handles state tracking and deployment changes?

Answer: 

There is a clearly a significant and critical distinction between both when handling state tracking and deployment changes. Here’s the main difference:

Serverless Framework:

State Tracking: 
Implicit and regulated by the API of the provider in use. When deploying to AWS, the Serverless Framework operates using AWS CloudFormation in the background. The details related to the deployment state (which resources are available) are preserved and overseen by CloudFormation itself.

Deployment Changes:
The framework creates a CloudFormation template from your serverless.yml file and interacts with the CloudFormation API. CloudFormation then assesses the necessary adaptations (a "change set") and carries them out. You receive a brief overview of this sequence.

Terraform:

State Tracking: 
Explicit and managed by a specific state file. This document (.tfstate) acts as a bridge between the resources defined in the configuration and the actual resources in the cloud environment. It serves as Terraform's "source of truth." 



Deployment Changes: 
The terraform plan command is essential. It evaluates your configuration against the current state file to create an execution strategy. This strategy outlines precisely what will be established, modified, or removed prior to executing terraform apply. This ensures a high level of assurance and safety when implementing changes. The state file should be securely stored and managed, often in a remote backend such as S3 with locking features (for instance, a DynamoDB table).

●	In what scenarios would you recommend using Serverless Framework over Terraform, and vice versa?

Using Serverless Framework will be recommended when:

i) 	The primary workload is serverless functions (e.g., a REST API backend with Lambda and API Gateway).
ii)	The team consists of application developers who want to deploy code quickly without deep infrastructure expertise.
iii)	Require fast iteration and development speed for event-driven microservices.
iv) 	The infrastructure required is almost entirely within the framework's supported events and 
resources.

Using Terraform will be recommended when:

i) 	Having technical resources to manage a broad, multi-service infrastructure (e.g., VPC, Kubernetes, databases, virtual machines, and serverless functions).
ii)	Building a multi-cloud or hybrid-cloud environment and want a single tool and workflow.
iii)	Precise control and visibility over your entire infrastructure is a requirement.
iv)	Compliance and auditability are critical, as the explicit plan/apply workflow and state file provide a clear audit trail.

●	Are there scenarios where using both together might be beneficial?


Answer: 

Yes. Employing both tools in conjunction is not only feasible but frequently regarded as a best practice for intricate serverless applications. It is often the best practice for complex serverless applications. The strategy is to use the right tool for the right layer. Example is when a company is building a new product with a serverless backend (Lambda, DynamoDB) but also needs a robust data lake (S3, Glue), a VPC for security, and a Kubernetes cluster for some long-running batch jobs.

How both can be integrated together:

Terraform's Role (The Foundation): 

Use Terraform to provision the foundational, shared, and stateful infrastructure that is outside the scope of a single serverless application.
· VPC, Subnets, Security Groups.
· IAM Roles and Policies that are shared across services.
· The S3 data lake buckets and related Glue jobs.
· The EKS (Kubernetes) cluster.
· Maybe even the base DynamoDB table, if it's shared.

Serverless Framework's Role (The Application): 

Use the Serverless Framework to develop and deploy the individual serverless application (eg. the API service).
· The Lambda functions and their code.
· The API Gateway routes specific to this service.
· The DynamoDB table if it's exclusive to this service.
· The S3 bucket for the frontend.
The Integration: 
The Serverless Framework service can reference resources created by Terraform. For example, in the serverless.yml file, you can use Terraform outputs (e.g., the ARN of a shared SNS topic or the name of a VPC) via AWS SSM Parameter Store or directly by name/ARN. This creates a clean separation of concerns: Terraform manages the platform, and Serverless Framework manages the application.


