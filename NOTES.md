# ECS Project Test

This repository contains steps for Project Tech Test

# Setup
The Webserver is desployed in two regions i.e Virginia and Ireland, and each region has multiple AZ, each AZ runs webserver to provide reslience, if any of the server is crashed, new instance should automatically gets created. The new server gets added to ALB running in that specific region.

Geo Location has been tested and works as expected, adding ALB is manual since it doesn't change and is one time, Hosted Zone is already been created for domain. Just for the demostration I'm using deployment in only 2 AZ's within the region and this can be extended, its not a limitation and its done to save cost at this minute.

# Prerequisite
Following infrastucture should be created before executing terraform. 
1. **S3 Bucket** -  It stores terraform state files remotely. Name of bucket used in this project is `ecs-tf-state-nnaruka-ireland` and `ecs-tf-state-nnaruka-virginia`. They are different buckets since region information is stored locally in the region.
2. **Dynamodb table** - Stores locking state to avoid accidental multiple users executing terraform script. Command used to create dynamodb table
```
aws dynamodb create-table \
         --region eu-west-1 \
         --table-name tf-state-ecs-nnaruka \
         --attribute-definitions AttributeName=LockID,AttributeType=S \
         --key-schema AttributeName=LockID,KeyType=HASH \
         --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5 \
         --tags Key=Owner,Value=NareshNaruka Key=Project,Value="Tech Test"   


aws dynamodb create-table \
         --region us-east-1 \
         --table-name tf-state-ecs-nnaruka \
         --attribute-definitions AttributeName=LockID,AttributeType=S \
         --key-schema AttributeName=LockID,KeyType=HASH \
         --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5 \
         --tags Key=Owner,Value=NareshNaruka Key=Project,Value="Tech Test"   

```
3. Domain is registed in goDaddy, and its hosted zone with the name nnaruka.co.uk is created in Route53. Geolocation records to be added after execution of terraform scripts.

# Software Used and its version

1. Terraform Version - `0.12.29`
2. AWS Cli Version - `2.0.36`


# Steps to build infrastructure

```
1. git clone <this repository>
2. execute following command and this should build instructure in Ireland Region
	cd terraform-aws-tech-test/ireland
	terraform apply -var-file=ireland.tfvars	
3. execute following command and this should build instructure in virginia Region
	cd ../terraform-aws-tech-test/virginia
	terraform apply -var-file=virginia.tfvars
5. Create Geolocation record for each ALB. DNS names of ALB is displayed after execution of above terraform scripts in step 2,3.
```

