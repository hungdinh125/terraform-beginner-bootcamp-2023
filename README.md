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