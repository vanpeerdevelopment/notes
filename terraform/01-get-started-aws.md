# [01 - Get Started - AWS](https://learn.hashicorp.com/terraform?track=getting-started#getting-started)

## [01 - Infrastructure as Code with Terraform](https://learn.hashicorp.com/terraform/getting-started/intro)
- HashiCorp product
- HashiCorp [configuration language](https://www.terraform.io/docs/configuration/index.html) (hcl)

### Infrastructure as code
- Manage infrastructure in a file
- Provider: AWS, Azure, Google, ...
- Resource: piece of infrastructure in an environment
- Declaratively describe wanted resources in hcl

### Workflows
1. Scope: decide which resources needed
2. Author: create hcl configuration file for these resources
3. Initialize: `terraform init` to download provider plugins
5. Apply: `terraform apply` 
    - Show execution plan
    - Create resources
    - Create state file: `terraform.tfstate`
    - Inspect state: `terraform show`
6. Destroy: `terraform destroy` to destroy all resources

### Advantages of Terraform
- Platform agnostic: manage heterogenous environment
- State management: state file to which config changes are measured, decide on new resources or resource modifications
- Operator confidence: planning phase to ensure changes will not disrupt environment


## [02 - Install Terraform](https://learn.hashicorp.com/terraform/getting-started/install)
- Install: `brew install terraform`
- Verify: `terraform --version`
- Tab completion: `terraform -install-autocomplete`


## [03 - Build Infrastructure](https://learn.hashicorp.com/terraform/getting-started/build)
### Provider
- Plugin used by Terraform to translate the API interactions with the service
- Multiple provider blocks can exist if a Terraform configuration manages resources from different providers

### Resources
- [Configuration](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) for pieces of infrastructure
- `resource "<resource type>" "<resource name>" { ... }`
- Prefix of the resource type maps to the provider 

### Format and validate
- `terraform validate`: checks syntax and validity
- `terraform fmt`: format configuration


## [04 - Change Infrastructure](https://learn.hashicorp.com/terraform/getting-started/change)
- Terraform builds an execution plan that only modifies what is necessary to reach your desired state
- `-/+`: destroy and recreate resource
- `~`: update in place


## [05 - Destroy Infrastructure](https://learn.hashicorp.com/terraform/getting-started/destroy)
- `terraform destroy`
- Terraform determines the order in which things must be destroyed


## [06 - Create Resource Dependencies](https://learn.hashicorp.com/terraform/getting-started/dependencies)
- Reference attributes of other resources
- Interpolation expression: `<resource-type>.<resource-name>.<attribute>`
- Terraform determines the order in which things must be created based on
    - Implicit dependencies: using interpolations expressions (preferred)
    - Explicit dependencies: using `depends_on: [<resource-type>.<resource-name>, ...]` argument
- Non dependent resources are created in parallel


## [07 - Provision Infrastructure](https://learn.hashicorp.com/terraform/getting-started/provision)
### Defining and running
- Use provisioners to initialize instances: upload files, run shell scripts, install software, ...
- Provisioners are only run when resource is created
- Provisioners are a last resort, use declarative model if possible
- Add a provisioner block to a resource block: `provisioner "<provisioner type>" { ... }`
    - `local-exec`: run command on machine running Terraform
    ```
    resource "aws_instance" "example" {
      ami           = "ami-b374d5a5"
      instance_type = "t2.micro"
    
      provisioner "local-exec" {
        command = "echo ${aws_instance.example.public_ip} > ip_address.txt"
      }
    }
    ```
    - `remote-exec`: run command on remote resource after it's created, over `ssh` or `winrm` connection
    ```
    resource "aws_key_pair" "example" {
      key_name   = "examplekey"
      public_key = file("~/.ssh/terraform.pub")
    }
    
    resource "aws_instance" "example" {
      key_name      = aws_key_pair.example.key_name
      ami           = "ami-04590e7389a6e577c"
      instance_type = "t2.micro"
    
      connection {
        type        = "ssh"
        user        = "ec2-user"
        private_key = file("~/.ssh/terraform")
        host        = self.public_ip
      }
    
      provisioner "remote-exec" {
        inline = [
          "sudo amazon-linux-extras enable nginx1.12",
          "sudo yum -y install nginx",
          "sudo systemctl start nginx"
        ]
      }
    }
    ```

### Tainted resources
- Resource successfully created but provisioner failed: tainted resource 
- On next `apply` tainted resource will be removed and recreated
- Manually taint resource: `terraform taint <resource type>.<resource name>`

### Destroy provisioner
- Run during resource destroy


## [08 - Define Input Variables](https://learn.hashicorp.com/terraform/getting-started/variables)
- Parameterize configuration variables
- Define variables in `variables.tf`: `variable "region" { default = "us-east-1" }`
- Use variables in configuration: `var.<variable name>`
- Assign variables (in priority order):
    - Command-line flag: `terraform apply -var="region=us-east-1"`
    - File: `terraform.tfvars`, `*.auto.tfvars`, `terraform apply -var-file=<file name>`
    - Environment variables: `TF_VAR_region="us-east-1"` (only string-type variables)
    - Interactive UI input
    - Default in `variables declaration`
- Rich data types:
    - List: `variable "cidrs" { type = list }`
    - Maps: `variable "amis" { type = map }`; usage: `var.amis[var.region]` 


## [09 - Query Data with Output Variables](https://learn.hashicorp.com/terraform/getting-started/outputs)
- Indicate which attributes are important
- Outputted on `terraform apply`
- Queried with `terraform output`
- Defined as:`output "ip" { value = <resource type>.<resource name>.<attribute> }`


## [10 - Store Remote State](https://learn.hashicorp.com/terraform/getting-started/remote)
- Store state on a [remote backend](https://www.terraform.io/docs/backends/index.html) (e.g. Terraform Cloud) 
    - Working in a team
    - Keep sensitive information off developer pc
    - Long running commands not on developer pc
- Configure remote backend
```
terraform {
  backend "remote" {
    organization = "<ORG_NAME>"

    workspaces {
      name = "Example-Workspace"
    }
  }
}
```
- Terraform Cloud user token in `~/.terraformrc`: `credentials "app.terraform.io" { token = "REPLACE_ME" }`