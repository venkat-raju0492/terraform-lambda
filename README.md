# terraform-Lambda
Common terraform module for AWS Lambda

# Overview

This module will do the following:
- Setup AWS lambda function
- Optional create lambda function in vpc
- Optional set env variables
- Optional enable x-ray tracing
- Optional set lambda layers
- Other properties that can be set includes: timeout, memory, reserved_concurrent_executions

## Usage

```terraform
terraform {
  backend "s3" {
    encrypt = true
  }
}

provider "aws" {
  region = var.region
}

locals {
  # Common tags to be assigned to all resources
  common_tags = {
    Project = var.project
    Environment = var.env
    CreatedBy = "Terraform"
  }
}

module "lambda" {
  source               = "git@github.levi-site.com:LSCO/terraform-Lambda.git?ref=RELEASE_VERSION"
  s3_bucket            = var.s3_bucket //S3 bucket which stores zip for lambda function
  s3_key               = var.s3_key //S3 location where lambda zip is present in S3
  function_name        = "<FUNCTION-NAME>" //Lambda function name
  role                 = "<LAMBDA-ROLE>" //Lambda function role
  handler              = "<LAMBDA-HANDLER>" //Lambda handler name
  runtime              = "<LAMBDA-RUNTIME>" //Lambda runtime environment
  timeout              = "<LAMBDA-TIMEOUT>" // Optional, set lambda timeout
  memory_size          = "<LAMBDA-MEMORY>" // Optional, set lambda memory
  vpc_enabled          = true // Optional, create lambda inside vpc
  subnet_ids           = var.private_subnet_ids // Optional, subnet id if vpc_enabled is true
  security_group_ids   = "<LAMBDA-SG>" // Optinal, set security group if vpc_enabled is true
  env_vars             = var.env_vars // Optional, set env variables as map type variable
  lambda_layers        = ["<LAMBDA-LAYER>"] // Optional, set lambda layer defined as list 
  common_tags          = local.common_tags // Common tags
}

```