# ECS and ALB with Terraform

This repository provides Terraform configurations to set up an ECS cluster with an Application Load Balancer (ALB) integrated. The ECS cluster will run containerized applications, and the ALB will route traffic to the ECS service.

## Components
- **ECS Cluster**: Manages ECS services and tasks, including Fargate capacity providers and service discovery integration.
- **ALB (Application Load Balancer)**: Routes HTTP traffic to the ECS service based on target groups.
- **VPC**: Creates a custom Virtual Private Cloud (VPC) with public and private subnets, and NAT gateway configuration.
- **Service Discovery**: Configures AWS CloudMap to enable service discovery for ECS tasks.
- **Security Groups**: Configures necessary security group rules for ECS tasks and ALB.

## Setup Instructions

### 1. **Configure Terraform Providers:**
   - **AWS provider**: For creating and managing resources in AWS.
   - **ECS provider**: To deploy and manage ECS services and tasks.
   - **ALB provider**: To create and manage the Application Load Balancer.
   - **Service Discovery provider**: For managing CloudMap namespaces for service discovery.

### 2. **ECS Cluster Configuration:**
   - The ECS cluster is created using the `terraform-aws-modules/ecs/aws` module.
   - Fargate and Fargate Spot are configured as capacity providers for the ECS cluster.
   - The ECS service will be linked to the ALB using a target group.

### 3. **ALB Setup:**
   - The Application Load Balancer is created using the `terraform-aws-modules/alb/aws` module.
   - The ALB listens on port 80 and forwards traffic to the ECS service's target group.
   - Security group rules are configured to allow traffic on port 80.

### 4. **Service Discovery:**
   - The ECS service uses AWS CloudMap for service discovery, enabling other services to discover it using DNS.

### 5. **VPC Configuration:**
   - The `terraform-aws-modules/vpc/aws` module creates the necessary VPC, subnets, and NAT gateway for the ECS cluster and ALB.

## Usage

### 1. **Variables:**
   - `region`: AWS region where resources will be deployed (default: `us-east-1`).
   - `container_name`: Name of the ECS container (default: `ecs-sample`).
   - `container_image`: Docker image for the ECS container (default: `public.ecr.aws/aws-containers/ecsdemo-frontend:776fd50`).
   - `container_port`: Port the container listens on (default: `80`).

### 2. **Terraform Configuration:**
   - Run `terraform init` to initialize the Terraform workspace.
   - Run `terraform apply` to create the resources in AWS.

## Outputs:
- **alb_dns_name**: The DNS name of the Application Load Balancer.
- **ecs_cluster_name**: The name of the ECS cluster.
- **ecs_service_name**: The name of the ECS service.
- **vpc_id**: The ID of the VPC created.
- **region**: The AWS region where the resources are deployed.

## Files in the Repository

- **`alb.tf`**: Configures the Application Load Balancer, security groups, and target groups.
- **`ecs.tf`**: Configures the ECS cluster, services, containers, and Fargate capacity providers.
- **`locals.tf`**: Local variables used throughout the configuration.
- **`main.tf`**: Defines resources like Service Discovery namespace and availability zones.
- **`version.tf`**: Specifies the required Terraform version and AWS provider.
- **`vpc.tf`**: Creates the VPC with subnets, NAT gateway, and security groups.

## Requirements
- Terraform >= 1.0
- AWS CLI installed and configured

## Notes
- Ensure your AWS credentials are set up before running `terraform apply`.
- Modify the `container_name`, `container_image`, and `container_port` variables if necessary.
