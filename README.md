# CloudFormation Examples

These are some very basic raw CloudFormation template examples.  This examples are part of blog tutorials.

1. [A Simple Introduction to AWS CloudFormation Part 1: EC2 Instance](https://medium.com/boltops/a-simple-introduction-to-aws-cloudformation-part-1-1694a41ae59d)
2. [A Simple Introduction ot AWS CloudFormation Part 2: EC2 Instance and Route53](https://medium.com/boltops/a-simple-introduction-to-aws-cloudformation-part-2-d6d95ed30328)
3. [A Simple Introduction to AWS CloudFormation Part 3: Updating a Stack](https://medium.com/boltops/a-simple-introduction-to-cloudformation-part-3-updating-a-stack-6fe2bb3931a9)
4. [A Simple Introduction to AWS CloudFormation Part 4: Change Sets = Dry Run Mode](https://medium.com/boltops/a-simple-introduction-to-cloudformation-part-4-change-sets-dry-run-mode-c14e41dfeab7)


## Configuration

### Configuring the aws cli

First configure your command line interface to amazon with

    aws configure
	
To ensure you have access keys and that a region is specified.

### Create a key pair

Create a key pair named 'tutorial' using the [EC2 console](https://eu-central-1.console.aws.amazon.com/ec2/v2/home?region=eu-central-1#KeyPairs:sort=keyName).


## Commands from Tutorials

### Tutorial 1

```bash
aws cloudformation create-stack --template-body file://templates/single-instance.yml --stack-name single-instance --parameters ParameterKey=KeyName,ParameterValue=tutorial ParameterKey=InstanceType,ParameterValue=t2.micro
```

### Tutorial 2

```bash
aws cloudformation create-stack --template-body file://templates/single-instance.yml --stack-name --stack-name route53-$(date +%s) --parameters ParameterKey=KeyName,ParameterValue=tutorial ParameterKey=InstanceType,ParameterValue=t2.micro ParameterKey=HostedZoneName,ParameterValue=sub.tongueroo.com. ParameterKey=Subdomain,ParameterValue=testsubdomain

aws cloudformation create-stack --template-body file://templates/instance-and-route53.yml --stack-name route53-$(date +%s) --parameters file://parameters/instance-and-route53.json
```

### Tutorial 3

```bash
aws cloudformation create-stack --stack-name example --template-body file://templates/single-instance.yml --parameters file://parameters/single-instance.json

aws cloudformation update-stack --stack-name example --template-body file://templates/instance-and-route53.yml --parameters file://parameters/instance-and-route53.json

aws cloudformation update-stack --stack-name example --template-body file://templates/single-instance.yml --parameters file://parameters/single-instance.json
```

### Tutorial 4

```bash
aws cloudformation create-stack --stack-name example --template-body file://templates/single-instance.yml --parameters file://parameters/single-instance.json

aws cloudformation create-change-set --stack-name example --template-body file://templates/instance-and-route53.yml --parameters file://parameters/instance-and-route53.json --change-set-name changeset-1

aws cloudformation describe-change-set --stack-name example --change-set-name changeset-1 | jq '.Changes[]'

aws cloudformation execute-change-set --stack-name example --change-set-name changeset-1
```

## Cleanup

### Destroying a stack

    aws cloudformation delete-stack --stack-name single-instance

## Troubleshooting

### No subnets found

`No subnets found for the default VPC 'vpc-901391fb'. Please specify a subnet. (Service: AmazonEC2; Status Code: 400; Error Code: MissingInput; Request ID: c0765f7a-39c4-4ce7-9038-9ecd481732bd)`


