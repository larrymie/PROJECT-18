# PROJECT-18

**AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM. PART 3 – REFACTORING IMAGE 01
![01](https://user-images.githubusercontent.com/91284177/167148267-e7eb13dd-5234-47d4-84ca-4a2365d121df.png)

*Introducing Backend on S3

I configured a backend where the state file can be accessed remotely other DevOps team members. There are plenty of different standard backends supported by Terraform that you can choose from. Since I have been using AWS from project 16 I decided to S3 bucket as a backend. 

Let us configure it!

Here is my plan to Re-initialize Terraform to use S3 backend:

Added S3 and DynamoDB resource blocks before deleting the local state file
Updated terraform block to introduce backend and locking
Re-initialize terraform
Deleted the local tfstate file and check the one in S3 bucket
Added outputs
terraform apply

Next, we will create a DynamoDB table to handle locks and perform consistency checks. In previous projects, locks were handled with a local file as shown in terraform.tfstate.lock.info. Since we now have a team mindset, causing us to configure S3 as our backend to store state file, we will do the same to handle locking. Therefore, with a cloud storage database like DynamoDB, anyone running Terraform against the same infrastructure can use a central location to control a situation where Terraform is running at the same time from multiple different people

Created a file and named it backend.tf. Added the below code to the repository. IMAGE 02 & 03

![02](https://user-images.githubusercontent.com/91284177/167149429-eeb5ef77-157e-434b-8059-7abe681a384a.png)
![03](https://user-images.githubusercontent.com/91284177/167150530-e66c7332-e94e-4b10-aa8a-b800deb28012.png)


I re-initialize the backend. Ran terraform init and confirmed the changes the backend by typing yes. Verified the changes in my AWS console. IMAGE 04 and 05

![04](https://user-images.githubusercontent.com/91284177/167151799-cab9b3e6-a3a1-412d-92fd-13fa478eaa5f.png)

![05](https://user-images.githubusercontent.com/91284177/167151815-99406cae-5be2-4a6f-b7f0-6e4123c36873.png)

Added Terraform Output
Before runing terraform apply I added an output so that the S3 bucket Amazon Resource Names ARN and DynamoDB table name can be displayed.

**Terraform Modules and best practices to structure your .tf codes

As a DevOps engineer, you must produce reusable and comprehensive IaC code structure, and one of the tool that Terraform provides out of the box is Modules.

**REFACTOR YOUR PROJECT USING MODULES

I reviewed the repository from project 17, I had a list of long file for creating all of my resources, but that is not the best way to go about it because it makes my code base vey hard to read and understand therefore making future changes can be quite stressful.

After regfactoring my codes.The resulting configuration structure in my working directory may look like this: IMAGE 06

![06](https://user-images.githubusercontent.com/91284177/167158045-c1e12844-2f9e-4276-9451-e02f4eb12a41.png)




