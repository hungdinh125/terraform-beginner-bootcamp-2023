# Terraform Beginner Bootcamp 2023

## Semantic Versioning :smile:

This project is going to utilize semantic versioning for its tagging:

[semver.org](https://semver.org/)

The general format:

 **MAJOR.MINOR.PATCH**, eg. `1.0.1`
- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

## Install the terraform CLI

[Terraform installation](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

[Shebang wiki](https://en.wikipedia.org/wiki/Shebang_(Unix))

[Gitpod tasks](https://www.gitpod.io/docs/configure/workspaces/tasks)

### Working Env Vars

#### env command

We can list out all Environment variables usng the `env` command.

We can filter specific env vars using grep, eg. `env | grep AWS_`

#### SEtting and Unsetting Env Vars

In the terminal we can set using `expore HELLO = 'world'`

To unset, using `unset HELLO`

We can set an env var termporarily when just running a command 

```sh
HELLO = 'world' ./bin/print_message
```

Within a bash script we cna set env without writing exporrt eg.

```sh
#!/usr/bin/env bash

HELLO = 'world'

echo $HELLO
```

#### Print Vars

We can print the env var by echo cli, eg. `echo $HELLO`

#### Scoping of Env Vars

When you poen up new bash temrinals in VScode it iwll not be aware of env vars that you have set in another window.

If you want to Env Vars to persist across all future bash terminals that are open you need to set env vars in your bash profile.

#### Persisitng Env Vars n Gitpod

We can persist env vars into gitpod by soting them in Gitpod Secrets Storage.

```
gp env HELLO='world'
```

All future workspaces launched will set the env vars for all bash temrinals opened in those workspaces.

You can also set en vars in the `.gitpod.yml` but this can only contain non-sentitive env vars.

Fixed the merge conflicts.

### AWS CLI installation

AWS CLI is installed for project via the bash script [`./bin/install-aws-cli`](./bin/install-aws-cli)

[Getting started installed](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

# Terraform Basics

### Terraform Registry

Terraform sources their providers and modules from the Terraform registry which is located at [registry.terraform.io](https://registry.terraform.io/)

- **Providers** are an interface to APIs that allow you o create resources in Terraform.
- **Modules** are a way to make large amount of terraform code modular, portable and sharable.

### Terraform Console

We can see a list of all the terraform cli by simply typing `terraform`

#### Terraform Init

At the start of a new terraform project, we will run `terraform init` to donwload the binaries for the terraform providers that we'll use in the project.

#### Terraform Plan

`terraform plan`

This will generate out a changeset, about the state of infrastructure and what will be changed.

We can output this changeset ie. "plan" to be passed to an apply, but often you can just ignore outputting.

#### Terrform Apply

`terraform apply`

This cli runs the plan and pass the changeset to be executed by terraform. Appply should prompt yes or no.

If we want to automatically approve an apply, we can propvide the auto-approve flag, eg. `terraform apply --auto-approve`

#### Terraform Destroy

`terraform destroy`

This will destroy resources.

You can also use the auto approve flag to skip the approve prompt, eg. `terraform destroy --auto-approve`


### Terraform Locked Files

`.terraform.lock.hcl` contains the locked versioning for the providers or modules that should be used in your project.

The Terraform Lock File **should be committed** to your Version Control System (VCS), eg. Github

### Terraform State Files

`.terraform.tfstate` contains information about the current state of infrastructure .

The file **should not be committed** to your VCS.

This file contains sensitive date, so if you lose it, you lose the known state of your infrastructure.

`terraform.tfstate.backup` is the previous state of file state.

### Terraform Directory

`.terraform` directory contains binaries of terraform providers.

## Issue with Terraform Login when using Terraform Cloud

When performing `terraform login`, it will launch bash with URL to generate token https://app.terraform.io/app/settings/tokens?source=terraform-login, but it does not work as expect in Gitpod VCS.

The workaround is to manually create the file

```bash
open /home/gitpod/.terraform.d/credentials.tfrc.json
```

Provide the following code, and replace your token

```json
{
    "credentials": {
      "app.terraform.io": {
        "token": "YOUR TOKEN IS NEEDED HERE"
      }
    }
  }
  ```

We have autmoated this workaround with the following bash script [bin/generate-tfrc-credentials]

```bash
#!usr/bin/env bash

# Destination directory
terraform_dir="/home/gitpod/.terraform.d/"

# Check if the directory exists, create if not
if [ ! -d "$terraform_dir" ]; then
    mkdir -p "$terraform_dir"
fi

# Check if TERRAFORM_CLOUD_TOKEN is set
if [ -z "$TERRAFORM_CLOUD_TOKEN" ]; then
    echo "Error: TERRAFORM_CLOUD_TOKEN environment variable is not set."
    exit 1
fi

# JSON structure for credentials.tfrc.json
json_data='{
  "credentials": {
    "app.terraform.io": {
      "token": "'"$TERRAFORM_CLOUD_TOKEN"'"
    }
  }
}'

# Write JSON data to credentials.tfrc.json
echo "$json_data" > "$terraform_dir/credentials.tfrc.json"

echo "credentials.tfrc.json generated successfully in $terraform_dir."

```
