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
    
    Set up AWS credentials locally with aws configure in the AWS Command Line Interface (CLI). Click here
    

To write quality Terraform codes, we need to:


    Understand the goal of the desired infrastructure.

    
    Ability to effectively use up to date Terraform documentation. Click here

    

The resources to be created include:

    S3 buket
    
    A VPC
    
    2 Public subnets


