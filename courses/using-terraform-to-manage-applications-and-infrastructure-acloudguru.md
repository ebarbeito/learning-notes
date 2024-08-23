# Using Terraform to Manage Applications and Infrastructure

by Jesse Hoch – A Cloud Guru

------

> Use Terraform to easily deploy applications and infrastructure to a variety of providers, like AWS, Azure, and Kubernetes clusters.
>
> We are about to take an exciting journey into the wonderful world of Terraform. We look at how you, as an admin, can use Terraform to easily deploy infrastructure to a variety of providers. Whether it's a single, simple configuration or a more complex configuration with multiple providers, we'll demonstrate how easy it is to manage infrastructure from one place.

**Available resources**

-  [Course materials](https://localhost)

🏷️ Tags: `course`, `2023`, `acloudguru`, `terraform`, `iac`, `infrastructure`, `automation`, `cloud`, `aws`

------

## Introduction

* Build, change, and version infrastructure safely; can be done locally or in the cloud
* Manage existing service providers, as well as custom in-house solutions
* Key features
  - **Infrastructure as Code** – IaC
  - **Execution plan**. It generates an execution with its “planning” steps. Like a dry-run, avoiding surprises when Terraform manipulates infrastructure
  - **Resource graph**. It builds infrastructure as efficiently as possible, by building a graph of all your resources
  - **Change automation**. Complex changes can be applied to your infrastructure with minimal interaction. Combining the execution plan and resource graph, you will know exactly what Terraform will change and in what order. Avoids many possible human errors

**Typical terraform project structure**

![Terraform Architecture](./.assets/using-terraform-to-manage-applications-and-infrastructure-acloudguru.md/terraform_architecture.png)

- Configuration file
- Variables file
- Provisioning changes into a Cloud provider
  - With your configuration file, vars file, and other config files if created. You would then plan and use those configuration files with Terraform to apply those configurations to your cloud provider,
  - Which would then in turn build your cloud environment and deploy your infrastructure with your resources
  - That information would then be sent back to Terraform where it would then write it to a Terraform state file
- Terraform state file
  - Is like a backup or snapshot of your successfully applied Terraform configuration.

## Command Line Interface

* `terraform <subcommand>`
* BASH tab autocompletion: `terraform -install-autocomplete`
* Change the working directory: `terraform -chdir=<path_to/tf> <subcommand>`
* Initialize the current directory: `terraform init` (when you create a new terraform configuration, or when you clone one)
* Create execution plan: `terraform plan`
  - Output a deployment plan: `terraform plan -out <plan_name>
  - Output a destroy plan: `terraform plan -destroy`
* Apply changes: `terraform apply`
  - Apply a specific plan: `terraform apply <plan_name>`
  - Apply only changes to a targeted resource: `terraform apply -target=<resource_name>`
  - Pass a variable via the command line: `terraform apply -var my_variable=<value>`
* Destroy the managed infrastructure: `terraform destroy`
* Get provider info used in your configuration: `terraform providers`
* Use an interactive console for evaluating [expressions](https://developer.hashicorp.com/terraform/language/expressions): terraform cons
