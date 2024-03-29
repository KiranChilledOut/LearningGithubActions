# Basic concepts of IAC

## Past

![alt text](image-2.png)

- Internet is just a graph of nodes connected together
- Cloud is not very different from On-Premises because Cloud is just renting someone elses hardware
  
## Cloud

![alt text](image-4.png)

- Let us see from Outside to Inside
- Devices connect to ISP which connects to the backbone of internet
- From Internet you reach the Cloud/Pvt Cloud/ VPC - Virtual Private cloud /Vnet - These just hosts different services and an abstraction.
- Inside this cloud you will start with Gateway - Gateway is an entry point for traffic/requests etc. The gate helps in traffic control
  - VPC peerting/Vnt Peering
  - Nat
- Then you have NACL - Network access control layer - FW, NSG etc
- NLB,ALB - Network Load balancer, Application Load balancer etc
- Subnets - Private Subnets, Public Subnet etc
- VMs, Dbs etc.
- See how a Public Subnet which needs access to a data in Private Database is given access
- Cloud Providers also provide us other services which does not sit in your Vnet like Blob Storage, Event Bridge or Lamda, Entra ID, IAM etc they are integrated deeply in your Vnet but not inside it .
- You need to follow the correct design Principles else it easy to leave a hole which attackers can use to hack.

## IAC

-Cloud Provider provisions

- Console UI - All Cloud providers will give u a dashboard / Portal where you can setup things manually.
- CLI/ API - you can use this for automation and scripts and also if u like terminal.
- SDK/CDK - Software/ Cloud development kit - Instead of using bath script you can leverage other softwares like Python, Powershell etc to manage the Cloud. This is already a step up.
- Template Tools - Using structures like Json or Yaml we can define a complex infrastructures and this is powerful if we can define and infra in such a code and get it deployed. This is basic of IAC
  
## Terraform

![alt text](image-5.png)

- Practitioner - You and me
- Infrastructure as Code - The configuration / Template of the infrastructure
- Plan - It takes the template , Compares with live state and if there is a drift it will show what will change
- Apply - It will apply the template and make the change on the infrastructure and makes sure it matches the template.
- It is cloud provider Agnotic , meaning its syntax will work on multiple cloud provider . Ofcourse you need to know the cloud you are dealing with.
- Terraform can be also used with things like Github
- It is a statefull Resource management tool
- It uses HCL which is a domain specific language which give us some control and semi declarative language
- Terraform Locally
  - main.tf
  - variables.tfvars to pass variable
  - vars.tf for variables
  - .terraform.lock.hcl - This locks the providers that we are using , Sometimes in upgrade we see there is breaking change . To avoid this file locks the providers used for the deployment and it uses them all the time.
  - [Repository](https://github.com/KiranChilledOut/iac-testing)
  - [Terraform AWS](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)

## Steps to follow

- from the CD documentation you would have created a AWS account, and a user and also have an access key for this user.
- Create a new Repo or follow the Repo mentioned above in this file
- Create `main.tf` file 

```terraform
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

# Configure the AWS Provider
provider "aws" {
  region = "eu-west-1"
}

```
- 
