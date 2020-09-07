# [02 - Organize Configuration](https://learn.hashicorp.com/collections/terraform/modules)

## [01 - Modules Overview](https://learn.hashicorp.com/tutorials/terraform/module)
### What are modules for
- Keeping related parts of configuration together
- Encapsulate configuration into distinct logical components
- Re-use configuration
- Provide consistency and ensure best practices

### What is a module
- Set of Terraform configuration files in a single directory (root module)
- Calling modules: use module blocks to call modules in other directories (child modules)
- Local vs remote modules

### Best practices
1. Write configuration with modules in mind
2. Use local modules and encapsulate your code
3. Use modules from public [Terraform Registry](https://registry.terraform.io/)
4. Publish and share modules with your team


## [02 - Use Modules from the Registry](https://learn.hashicorp.com/tutorials/terraform/module-use)
### Calling module
```
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "2.21.0"
  
  name = "my-vpc"
}
```
- `source`: where module can be found
- `version`

### Module variables
- Arguments besides `source` and `version` are [input variables](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/2.21.0?tab=inputs) for the called module
- Use [output variables](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/2.21.0?tab=outputs) from module: `module.<module name>.<output name>`
- Input variables which vary over environments can be made configurable by defining in `variables.tf`

### Provision
- `terraform init`: download modules and provider plugins
- `terraform get`: download modules


## [03 - Build a Module](https://learn.hashicorp.com/tutorials/terraform/module-create)
### Module structure
- Typical structure:
    - `LICENSE`
    - `README.md`
    - `main.tf`: main configuration
    - `variables.tf`: module input variables
    - `outputs.tf`: module output variables
- Do not distribute
    - `terraform.tfstate`
    - `terraform.tfstate.backup`
    - `.terraform`
    - `*.tfvars`

### Create a module
- No `provider` block, will be inherited from root module
- Input variables without default value are required variables


## [04 - Share Modules in the Private Module Registry](https://learn.hashicorp.com/tutorials/terraform/module-private-registry)
- Create and share infrastructure modules within an organization using Terraform Cloud

### GitHub
- Repository containing the Terraform module
- Naming convention: `terraform-<provider>-<name>`
- Semantic versioning using GitHub release tags

### Terraform Cloud
- Prerequisite: [GitHub OAuth Access](https://www.terraform.io/docs/cloud/vcs/github.html)
- Add module: `organization -> modules -> add module -> GitHub -> repository`

### Use private module
- Source: `app.terraform.io/<ORGANIZATION-NAME>/terraform/<NAME>/<PROVIDER>`
- Version
- Optional: configure Terraform Cloud [workspace](https://learn.hashicorp.com/tutorials/terraform/module-private-registry#create-a-workspace-for-the-configuration) to automate deployment


## [05 - Separate Development and Production Environments](https://learn.hashicorp.com/tutorials/terraform/organize-configuration)
0. Starting point: monolith configuration for both dev and prod environment
1. Split `main.tf` in `dev.tf` and `prod.tf`
2. Separate dev and prod state
    - Directories
        - State file stored in directory of environment
        - Duplicated code
        - Environments with potential configuration differences
    - Workspaces
        - State file per environment in `terraform.tfstate.d/<env>/terraform.tfstate`
        - Same Terraform code: remove references to individual environments, create variables file per environment
        - Environments that do not greatly deviate from one another
        - All Terraform configurations start out in the `default` workspace
        - Create Terraform workspace per environment: `terraform workspace new <env>`
        - Change workspace using `terraform workspace select <env>`
        - Execute commands in correct workspace using correct variable file: `terraform apply -var-file=<env>.tfvars`
3. To avoid configuration drift, bundle resources as modules
 