---
title: "Terratest: Streamline Your Infrastructure Testing"
datePublished: Wed Jul 12 2023 00:43:44 GMT+0000 (Coordinated Universal Time)
cuid: cljyzzyi7000109l53xjn3ecu
slug: terratest-streamline-your-infrastructure-testing
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689122571845/9aaf58c5-7a16-4969-b361-baac6dd5b94b.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1689122608500/0f85e013-230f-4073-a685-bcd78e06324b.png
tags: aws, kubernetes, devops, terraform, infrastructure-as-code

---

## Introduction

Terratest is an open-source testing framework developed by Gruntwork, designed to facilitate infrastructure testing and validation. Whether you are managing infrastructure on cloud platforms like AWS, Azure, or Google Cloud, or using configuration management tools like Terraform or Ansible, Terratest can streamline and automate your testing process, giving you confidence in the reliability of your infrastructure.

## Getting Started with Terratest

To get started with Terratest, you need to have Go installed on your machine. Once you have Go set up, you can initialize a new Go module and import the Terratest library. From there, you can begin writing your infrastructure tests using Terratest's expressive syntax.

Terratest has extensive documentation, including examples and best practices, which can guide you through the process of writing effective tests. The Gruntwork GitHub repository hosts various example projects and modules that demonstrate how to use Terratest in real-world scenarios.

## Testing AWS EC2 resource

* Directory structure
    

```bash
├── aws-ec2-test
│   ├── aws_ec2_test.go
│   ├── go.mod
│   └── go.sum
├── aws_ec2
│   ├── main.tf
│   ├── terraform.tfstate
│   └── terraform.tfstate.backup
```

## Steps

1. Create 2 directories
    
    1. For terraform code
        
    2. For testing golang code
        
2. Add terraform scripts for aws ec2 instance
    
    ```json
    provider "aws" {
        region = "eu-north-1"
      
    }
    data "aws_ami" "app_ami" {
        most_recent = true
        owners = ["amazon"]
        filter{
            name = "name"
            values = ["amzn2-ami-hvm*"]
        }
    }
    resource "aws_instance" "instance-dev" {
      ami           = data.aws_ami.app_ami.id
      instance_type = "t3.micro"
    }
    output "instance_type" {
        # value = aws_instance.instance-dev.public_ip
        value = aws_instance.instance-dev.instance_type
    }
    ```
    

## 3\. Add Golang testing file in 2nd testing directory.

* Create a golang file in the test directory.
    
    * NOTE: test file should be always in this syntax: `yourfilename_test.go`(`_test.go` should be in the last)
        
* First define modules to import
    

```go
package test
import(
	"testing"
	"github.com/gruntwork-io/terratest/modules/terraform"
	"github.com/stretchr/testify/assert"
)
```

* Initialize go module and install required packages
    
    ```bash
    go mod init testmodule
    go mod tidy
    ```
    
* Now define the function for the testing
    
* Here's the complete test file
    

```go
package test
import(
	"testing"
	"github.com/gruntwork-io/terratest/modules/terraform"
	"github.com/stretchr/testify/assert"
)
func TestAWSEC2Instance(t *testing.T){
	terraformOptions := terraform.WithDefaultRetryableErrors(t, &terraform.Options{
		TerraformDir: "../aws_ec2",
	})
	defer terraform.Destroy(t, terraformOptions)
	terraform.InitAndApply(t, terraformOptions)
	instanceType := terraform.Output(t, terraformOptions, "instance_type")
	assert.Equal(t, "t3.micro",instanceType)
}
```

* What testing will do?
    
    * First it will do terraform init in the specified terraform directory
        
    * Then it will do terraform apply
        
    * Then it will check our defined terraform output is same as expected or not(using assert.Equal)
        
        * If not it will fail the test.
            
        * If true, it will continue.
            
    * If all tests passed, in the last it will destroy the infrastructure(define with defer)