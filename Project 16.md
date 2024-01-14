## Automate Infrastructure With IaC using Terraform Part 1

In this project, I will be automating the AWS infrastructure for 2 websites that I manually created in Project-15 using Terraform.


### BENEFITS:

The benefits of using Terraform in this Project include:


Improved collaboration: It allows teams to define and manage infrastructure using version control, which makes it easier for multiple people to collaborate and work on the same codebase. This can help improve collaboration and reduce the risk of errors - This is implemented in Project-18 and Project-19.



Version history: It automatically maintains a version history of your infrastructure, which makes it easy to roll back to previous versions if necessary. This can help protect against mistakes and ensure that you can recover from failures quickly..



Consistency: It allows you to define your infrastructure using a high-level configuration language, which means that you can specify the desired state of your infrastructure in a consistent and predictable way.



Reusability: It allows you to define infrastructure as modular components, which can be easily reused across multiple deployments. This can help reduce duplication and make it easier to manage your infrastructure at scale - This is implemented in Project-18.



Portable across cloud providers: It is portable across different cloud providers, which means that you can use the same tools and processes to manage your infrastructure regardless of where it is deployed.



Overall, Terraform offers a number of benefits for managing infrastructure as code. It has become a mandatory tool to set up reproducible multi-cloud infrastructure.



### AWS MULTIPLE WEBSITE ARCHITECTURE




![Screenshot from 2024-01-12 19-34-31](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/64036006-2f01-4d36-adca-72ab98ed525c)
![Screenshot from 2024-01-12 19-35-57](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/f3977675-100d-48be-a1f2-aa574dbd078b)



### Prerequisites:




    AWS account;
    
    
    AWS Identify and Access Management (IAM) credentials and programmatic access.


    Create IAM user and name it 'terraform' and grant this user AdministratorAccess permissions.
    

    Download and save the Access Key ID and Secret Access Key

    
    Set up AWS credentials locally with aws configure in the AWS Command Line Interface (CLI). Click https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

    To install AWS CLI, use the commands below on your local machine terminal; if you don't have curl installed already on your local machine use the command 'sudo snap install curl'



    Also ensure you have zip installed on your machine


    ~~~
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    unzip awscliv2.zip
    sudo ./aws/install
    ~~~




Then, Configure programmatic access from your workstation to connect to AWS using the access keys copied above and a Python SDK (boto3). You must have Python 3.6 or higher on your workstation.




![Screenshot from 2024-01-13 22-48-51](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/fac04aeb-b521-471c-80d7-f55953eacbc5)



To install Python boto3



~~~
sudo apt install python3-pip
pip install boto3
~~~






To write quality Terraform codes, we need to:


    Understand the goal of the desired infrastructure.

    
    Ability to effectively use up to date Terraform documentation. Click here

    

The resources to be created include:

    S3 buket
    
    A VPC
    
    2 Public subnets



#### CREATE S3 BUCKET


Create an S3 bucket to store Terraform state file. You can name it something like <yourname>-dev-terraform-bucket (Note: S3 bucket names must be unique unique within a region partition.



![Screenshot from 2024-01-14 00-39-38](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/57762e55-a572-4674-aff8-b60458b8d2ad)





From my terminal, refresh the CLI credentials again as shown below with the Access Key ID and Secret Acces key



![Screenshot from 2024-01-14 01-20-41](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/f40cefb6-0563-44e0-940a-06d6077d6268)




I should be able to see the S3 bucket i just created if my aws CLI is configured. Run the command

~~~
aws s3 ls
~~~



If you can't see it do the following;


Sign in to the AWS Management Console.

Navigate to the "IAM" service.

In the IAM dashboard, select the IAM user or role that is encountering the 403 error.

Go to the "Permissions" tab to review the attached policies and permissions.

Ensure that the user or role has the s3:ListAllMyBuckets permission to list S3 buckets. Click on 'Add Permissions > Create inline policy, then select s3 from the dropdown and select the access level

If the necessary policy is not attached, attach the AmazonS3ReadOnlyAccess managed policy to the user or role. This policy includes the required permissions for listing S3 buckets.

Alternatively, you can create a custom inline policy ( with json) with the s3:ListAllMyBuckets permission:



~~~
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*"
    }
  ]
}
~~~




![Screenshot from 2024-01-14 01-24-59](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/8bf65f50-5eae-4f0d-add6-569756a1410e)




![Screenshot from 2024-01-14 01-26-00](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/8e2718b1-4f23-48da-956d-3f85c18fd136)




#### CREATING VPC | SUBNETS | SECURITY GROUPS



Create a directory structure.

In VS Code, Create a folder called PBL and create a file in the folder, name it main.tf

Install the following Terraform extensions on Vs Code





![Screenshot from 2024-01-14 01-39-59](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/8534dc3e-5cbb-4441-bbec-c0aa6e711979)








![Screenshot from 2024-01-14 01-46-50](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/740c92bb-7665-444c-8000-1f2ae7cb7a92)







#### Provider and VPC resource section



Add AWS as a provider, and a resource to create a VPC in the main.tf file. 


The provider block informs Terraform that we intend to build infrastructure within AWS.


The resource block will create a VPC.


Go to Terraform documentation ( https://registry.terraform.io/) , select the provider and go to documentation to access the various resources.







![Screenshot from 2024-01-14 02-00-05](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/d3e60d33-841d-497b-823b-9284b7d8acaa)







on the main.tf file, add the following codes




~~~
provider "aws" {
  region = "us-east-1"
}

# Create VPC
resource "aws_vpc" "main" {
  cidr_block                     = "172.16.0.0/16"
  enable_dns_support             = "true"
  enable_dns_hostnames           = "true"
  enable_classiclink             = "false"
  enable_classiclink_dns_support = "false"
  tags = {
    Name = "netrill-VPC"
  }
}
~~~








![Screenshot from 2024-01-14 02-22-28](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/e8b9208b-ccf8-44b7-b4f0-c95e72fb642a)





Then download the necessary plugins for Terraform to work. These plugins are used by providers and provisioners. At this stage, we only have provider in our main.tf file. So, Terraform will just download plugin for AWS provider. 


Install terraform on your local machine using the command 




~~~
wget https://releases.hashicorp.com/terraform/0.15.5/terraform_0.15.5_linux_amd64.zip
sudo apt-get install unzip
unzip terraform_0.15.5_linux_amd64.zip
sudo mv terraform /usr/local/bin/
terraform --version
~~~






![Screenshot from 2024-01-14 02-51-12](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/a594a5b5-75dd-4730-bbce-b0ad65258e55)









Run the command


terraform init





