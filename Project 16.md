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






Then run the command



terraform init










![Screenshot from 2024-01-14 02-51-12](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/a594a5b5-75dd-4730-bbce-b0ad65258e55)






![Screenshot from 2024-01-14 03-03-17](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/e883445d-4e0b-487b-8ddc-5a3f8eeb6679)







A new file has been created: .terraform. This is where Terraform keeps plugins.




Create the resource we just defined - aws_vpc by running the following commands




$ terraform validate - To validate the code





$ terraform fmt - To format the code for readability








![Screenshot from 2024-01-14 03-08-11](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/77c0be5b-d848-48be-bb6b-a16b1a0c698d)





we should check to see what terraform intends to create before we tell it to go ahead and create it by running the command



$ terraform plan








![Screenshot from 2024-01-14 03-10-49](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/6b7dd205-56f1-456b-aab3-d5afd4668bd4)







If we are good with the chages planned, run the command





$ terraform apply








![Screenshot from 2024-01-14 03-14-03](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/b2ecf4da-9f54-469a-9901-de9f11c50c80)






This creates the VPC - narbyd-VPC and its attributes.






![Screenshot from 2024-01-14 03-22-09](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/dcb66f85-0f77-4b30-88ce-fc911cf19411)






A new file is created terraform.tfstate This is how Terraform keeps itself up to date with the exact state of the infrastructure. It reads this file to know what already exists, what should be added, or destroyed based on the entire terraform code that is being developed.




If you also observed closely, you would realise that another file - terraform.tfstate.lock.info gets created during planning and apply but this file gets deleted immediately. This is what Terraform uses to track, who is running its code against the infrastructure at any point in time. This is very important for teams working on the same Terraform repository at the same time. The logs prevents a user from executing Terraform configuration against the same infrastructure when another user is doing the same – it allows to avoid duplicates and conflicts.





#### Subnets resource section






According to our architectural design, we require 6 subnets:


    2 public
    
    2 private for webservers
    
    2 private for data layer
    

Let us create the first 2 public subnets.


Add below configuration to the main.tf file:







~~~

# Create public subnets1
resource "aws_subnet" "public1" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "172.16.1.0/24"
  map_public_ip_on_launch = true
  availability_zone       = "us-east-1a"
  tags = {
    Name = "netrill-pub-1"
  }

}

# Create public subnet2
resource "aws_subnet" "public2" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "172.16.2.0/24"
  map_public_ip_on_launch = true
  availability_zone       = "us-east-1b"
  tags = {
    Name = "netrill-pub-2"
  }
}
~~~








![Screenshot from 2024-01-14 03-31-42](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/ce7d1b71-7257-467f-b0fa-0156ae55c559)







We are creating 2 subnets, therefore declaring 2 resource blocks – one for each of the subnets. We are using the vpc_id argument to interpolate the value of the VPC id by setting it to aws_vpc.main.id. This way, Terraform knows inside which VPC to create the subnet.


Run the commands


$ terraform validate


$ terraform fmt


$ terraform plan


Then


$ terraform apply to create the public subnets.








![Screenshot from 2024-01-14 09-42-46](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/1b92adef-5523-4aba-bbe9-b7c207ecfeae)





#### Observations:








Hard coded values: Remember our best practice hint from the beginning? Both the availability_zone and cidr_block arguments are hard coded. We should always endeavour to make our work dynamic.


Multiple Resource Blocks: Notice that we have declared multiple resource blocks for each subnet in the code. This is bad coding practice. We need to create a single resource block that can dynamically create resources without specifying multiple blocks. Imagine if we wanted to create 10 subnets, our code would look very clumsy. So, we need to optimize this by introducing a count argument.





Now let us improve our code by refactoring it.






First, destroy the current infrastructure. Since we are still in development, this is totally fine. Otherwise, DO NOT DESTROY an infrastructure that has been deployed to production.



To destroy whatever has been created run terraform destroy command, and type yes after evaluating the plan.







![Screenshot from 2024-01-14 09-49-34](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/0c1aa4de-3cb8-45b0-a167-68a87dede636)









![Screenshot from 2024-01-14 09-49-54](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/67de82c3-9f65-48c1-851c-d6908afa5d09)





#### Fixing The Problems By Code Refactoring




Fixing Hard Coded Values: We will introduce variables, and remove hard coding.


    Starting with the provider block, declare a variable named region, give it a default value, and update the provider section by referring to the declared variable.




~~~
    variable "region" {
        default = "us-east-1"
    }

    provider "aws" {
        region = var.region
    }
~~~






Do the same to cidr value in the vpc block, and all the other arguments.




~~~
    variable "region" {
        default = "us-east-1"
    }

    variable "vpc_cidr" {
        default = "172.16.0.0/16"
    }

    variable "enable_dns_support" {
        default = "true"
    }

    variable "enable_dns_hostnames" {
        default ="true" 
    }

    variable "enable_classiclink" {
        default = "false"
    }

    variable "enable_classiclink_dns_support" {
        default = "false"
    }

    provider "aws" {
    region = var.region
    }

    # Create VPC
    resource "aws_vpc" "main" {
    cidr_block                     = var.vpc_cidr
    enable_dns_support             = var.enable_dns_support 
    enable_dns_hostnames           = var.enable_dns_support
    enable_classiclink             = var.enable_classiclink
    enable_classiclink_dns_support = var.enable_classiclink

    }
~~~







Fixing multiple resource blocks: This is where things become a little tricky. It's not complex, we are just going to introduce some interesting concepts. Loops & Data sources





Terraform has a functionality that allows us to pull data which exposes information to us. For example, every region has Availability Zones (AZ). Different regions have from 2 to 4 Availability Zones. With over 20 geographic regions and over 70 AZs served by AWS, it is impossible to keep up with the latest information by hard coding the names of AZs. Hence, we will explore the use of Terraform's Data Sources to fetch information outside of Terraform. In this case, from AWS


Let us fetch Availability zones from AWS, and replace the hard coded value in the subnet's availability_zone section.




~~~
        # Get list of availability zones
        data "aws_availability_zones" "available" {
        state = "available"
        }
~~~





To make use of this new data resource, we will need to introduce a count argument in the subnet block: Something like this.






~~~
    # Create public subnet1
    resource "aws_subnet" "public" { 
        count                   = 2
        vpc_id                  = aws_vpc.main.id
        cidr_block              = "172.16.1.0/24"
        map_public_ip_on_launch = true
        availability_zone       = data.aws_availability_zones.available.names[count.index]

    }

~~~






Let us quickly understand what is going on here.


    The count tells us that we need 2 subnets. Therefore, Terraform will invoke a loop to create 2 subnets.

    
    The data resource will return a list object that contains a list of AZs. Internally, Terraform will receive the data like this





~~~
  ["us-east-1a", "us-east-1b"]
~~~





Each of them is an index, the first one is index 0, while the other is index 1. If the data returned had more than 2 records, then the index numbers would continue to increment.


Therefore, each time Terraform goes into a loop to create a subnet, it must be created in the retrieved AZ from the list. Each loop will need the index number to determine what AZ the subnet will be created. That is why we have data.aws_availability_zones.available.names[count.index] as the value for availability_zone. When the first loop runs, the first index will be 0, therefore the AZ will be eu-central-1a. The pattern will repeat for the second loop.


But we still have a problem. If we run Terraform with this configuration, it may succeed for the first time, but by the time it goes into the second loop, it will fail because we still have cidr_block hard coded. The same cidr_block cannot be created twice within the same VPC. So, we have a little more work to do.




Let's make cidr_block dynamic.



We will introduce a function cidrsubnet() to make this happen. It accepts 3 parameters. Let us use it first by updating the configuration, then we will explore its internals.





~~~
    # Create public subnet1
    resource "aws_subnet" "public" { 
        count                   = 2
        vpc_id                  = aws_vpc.main.id
        cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
        map_public_ip_on_launch = true
        availability_zone       = data.aws_availability_zones.available.names[count.index]

    }

~~~







A closer look at cidrsubnet - this function works like an algorithm to dynamically create a subnet CIDR per AZ. Regardless of the number of subnets created, it takes care of the cidr value per subnet.

Its parameters are cidrsubnet(prefix, newbits, netnum)

    The prefix parameter must be given in CIDR notation, same as for VPC.

    
    The newbits parameter is the number of additional bits with which to extend the prefix. For example, if given a prefix ending with /16 and a newbits value of 4, the resulting subnet address will have length /20

    
    The netnum parameter is a whole number that can be represented as a binary integer with no more than newbits binary digits, which will be used to populate the additional bits added to the prefix
    

You can experiment how this works by entering the terraform console and keep changing the figures to see the output.


    On the terminal, run terraform console

    
    type cidrsubnet("172.16.0.0/16", 4, 0)

    
    Hit enter
    
    
    See the output

    
    Keep change the numbers and see what happens.

    
    To get out of the console, type exit




#### The final problem to solve is removing hard coded count value.




If we cannot hard code a value we want, then we will need a way to dynamically provide the value based on some input. Since the data resource returns all the AZs within a region, it makes sense to count the number of AZs returned and pass that number to the count argument.


To do this, we can introuduce length() function, which basically determines the length of a given list, map, or string.


Since data.aws_availability_zones.available.names returns a list like ["us-east-1a", "us-east-1b", "us-east-1c"] we can pass it into a lenght function and get number of the AZs.



length(["us-east-1a", "us-east-1b", "us-east-1c"])



Open up terraform console and try it 




ow we can simply update the public subnet block like this





~~~
# Create public subnet1
    resource "aws_subnet" "public" { 
        count                   = length(data.aws_availability_zones.available.names)
        vpc_id                  = aws_vpc.main.id
        cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
        map_public_ip_on_launch = true
        availability_zone       = data.aws_availability_zones.available.names[count.index]

    }
~~~





#### Observations:





What we have now, is sufficient to create the subnet resource required. But if you observe, it is not satisfying our business requirement of just 2 subnets. The length function will return number 3 to the count argument, but what we actually need is 2.


Now, let us fix this.



    Declare a variable to store the desired number of public subnets, and set the default value





~~~
  variable "preferred_number_of_public_subnets" {
      default = 2
}
~~~





Next, update the count argument with a condition. Terraform needs to check first if there is a desired number of subnets. Otherwise, use the data returned by the lenght function. See how that is presented below.






~~~
# Create public subnets
resource "aws_subnet" "public" {
  count  = var.preferred_number_of_public_subnets == null ? length(data.aws_availability_zones.available.names) : var.preferred_number_of_public_subnets   
  vpc_id = aws_vpc.main.id
  cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
  map_public_ip_on_launch = true
  availability_zone       = data.aws_availability_zones.available.names[count.index]

}
~~~






Now lets break it down:


    The first part var.preferred_number_of_public_subnets == null checks if the value of the variable is set to null or has some value defined.

    
    The second part ? and length(data.aws_availability_zones.available.names) means, if the first part is true, then use this. In other words, if preferred number of public subnets is null (Or not known) then set the value to the data returned by lenght function.

    
    The third part : and var.preferred_number_of_public_subnets means, if the first condition is false, i.e preferred number of public subnets is not null then set the value to whatever is definied in var.preferred_number_of_public_subnets


Now the entire configuration should now look like this






~~~
# Get list of availability zones
data "aws_availability_zones" "available" {
state = "available"
}

variable "region" {
      default = "us-east-1"
}

variable "vpc_cidr" {
    default = "172.16.0.0/16"
}

variable "enable_dns_support" {
    default = "true"
}

variable "enable_dns_hostnames" {
    default ="true" 
}

variable "enable_classiclink" {
    default = "false"
}

variable "enable_classiclink_dns_support" {
    default = "false"
}

  variable "preferred_number_of_public_subnets" {
      default = 2
}

provider "aws" {
  region = var.region
}

# Create VPC
resource "aws_vpc" "main" {
  cidr_block                     = var.vpc_cidr
  enable_dns_support             = var.enable_dns_support 
  enable_dns_hostnames           = var.enable_dns_support
  enable_classiclink             = var.enable_classiclink
  enable_classiclink_dns_support = var.enable_classiclink

}


# Create public subnets
resource "aws_subnet" "public" {
  count  = var.preferred_number_of_public_subnets == null ? length(data.aws_availability_zones.available.names) : var.preferred_number_of_public_subnets   
  vpc_id = aws_vpc.main.id
  cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
  map_public_ip_on_launch = true
  availability_zone       = data.aws_availability_zones.available.names[count.index]

}
~~~






Note: You should try changing the value of preferred_number_of_public_subnets variable to null and notice how many subnets get created.




#### Introducing variables.tf & terraform.tfvars



Instead of havng a long lisf of variables in main.tf file, we can actually make our code a lot more readable and better structured by moving out some parts of the configuration content to other files.

    We will put all variable declarations in a separate file

    
    And provide non default values to each of them




    Create a new file and name it variables.tf

    
    Copy all the variable declarations into the new file.

    
    Create another file, name it terraform.tfvars

    
    Set values for each of the variables.




#### Maint.tf





~~~
# Get list of availability zones
data "aws_availability_zones" "available" {
state = "available"
}

provider "aws" {
  region = var.region
}

# Create VPC
resource "aws_vpc" "main" {
  cidr_block                     = var.vpc_cidr
  enable_dns_support             = var.enable_dns_support 
  enable_dns_hostnames           = var.enable_dns_support
  enable_classiclink             = var.enable_classiclink
  enable_classiclink_dns_support = var.enable_classiclink

}

# Create public subnets
resource "aws_subnet" "public" {
  count  = var.preferred_number_of_public_subnets == null ? length(data.aws_availability_zones.available.names) : var.preferred_number_of_public_subnets   
  vpc_id = aws_vpc.main.id
  cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
  map_public_ip_on_launch = true
  availability_zone       = data.aws_availability_zones.available.names[count.index]
}
~~~







![Screenshot from 2024-01-14 20-29-39](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/cf215b95-ddb6-4cd2-a266-125d2d2498f2)







#### variables.tf




~~~
variable "region" {
      default = "us-east-1"
}

variable "vpc_cidr" {
    default = "172.16.0.0/16"
}

variable "enable_dns_support" {
    default = "true"
}

variable "enable_dns_hostnames" {
    default ="true" 
}

variable "enable_classiclink" {
    default = "false"
}

variable "enable_classiclink_dns_support" {
    default = "false"
}

  variable "preferred_number_of_public_subnets" {
      default = null
}
~~~









![Screenshot from 2024-01-14 20-29-02](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/fc55024f-eff7-43e5-a04a-a6dd6294b686)






#### terraform.tfvars





~~~
region = "us-east-1"

vpc_cidr = "172.16.0.0/16" 

enable_dns_support = "true" 

enable_dns_hostnames = "true"  

enable_classiclink = "false" 

enable_classiclink_dns_support = "false" 

preferred_number_of_public_subnets = 2
~~~






![Screenshot from 2024-01-14 20-28-15](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/18635f79-eacc-467c-b1e2-df3e260ccad4)





You should also have this file structure in the PBL folder.






~~~
└── PBL
    ├── main.tf
    ├── terraform.tfstate
    ├── terraform.tfstate.backup
    ├── terraform.tfvars
    └── variables.tf
~~~







Run 




terraform init


terraform validate


terraform fmt


terraform plan


terraform apply







![Screenshot from 2024-01-14 20-35-56](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/e9976fe6-1cde-4b7a-b171-a987d44ef59b)








![Screenshot from 2024-01-14 20-37-05](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/e35e2007-6bbc-468a-b634-924022640140)







![Screenshot from 2024-01-14 20-48-05](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/fc0786c4-4f00-4cd5-95f8-f36c98e151ae)









![Screenshot from 2024-01-14 20-48-44](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/41c10c72-2073-4b87-945e-2a092b2e0a35)










We have successfully created our VPC and subnets in the region and availability zones respectively.



Run



$ terraform destroy to delete the resources.






![Screenshot from 2024-01-14 20-50-51](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/782f6d40-2bb9-4c36-bb99-7f4a4bf95aac)









![Screenshot from 2024-01-14 20-51-16](https://github.com/ekomoku/Project-16-Automate-Infrastructure-With-IaC-using-Terraform-Part-1/assets/66005935/6639c187-cfe0-46e0-a45f-d74651a97b9a)









#### Congratulations!


You have learned how to create and delete AWS Network Infrastructure programmatically with Terraform!









