# ECS Project Test

This repository contains steps for Project Tech Test

# Setup
The Webserver is desployed in two regions i.e Virginia and Ireland, and each region has multiple AZ, each AZ runs webserver to provide reslience, if any of the server is crashed, new instance should automatically gets created. The new server gets added to ALB running in that specific region.

Geo Location has been tested and works as expected, adding ALB is manual since it doesn't change and is one time, Hosted Zone is already been created for domain. Just for the demostration I'm using deployment in only 2 AZ's within the region and this can be extended, its not a limitation and its done to save cost at this minute.

# Prerequisite
S3 Bucket and Dynamodb table should be created before executing terraform.  S3 bucket for each region is kept locally to avoid storing content in another region, so is the case with Dynamodb Table.

# Software Used

Terraform verion - 0.12.29

AWS Cli Version - 2.0.36


# Steps to build infrastructure

```
1. git clone <this repository>
2. cd terraform-aws-tech-test.ireland
3. execute the command - 
		terraform apply -var-file=virginia.tfvars
		This should build instructure in Ireland Region
4. execute command 
		terraform apply -var-file=ireland.tfvars
		This will build infrastructure in Virginia Region
5. Create Geolocation record for each ALB. DNS names of ALB is displayed after execution of above terraform scripts in step 3,5.
```

