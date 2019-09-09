# gitlab-runner-module

This module assumes following resources are already available:
1. VPC
2. Public Subnet
3. Private Subnet
4. Route table, Route, Route Table Association for public subnet
5. Internet gateway for public subnet
6. Gitlab Instance
7. Gitlab Runner Token

and it performs following actions:
1. Creates a number of Gitlab Runners
2. Configures the runners to authenticate to a private docker registry
3. Register Runners with Gitlab using Gitlab URL and Token

Usage:
```
module gitlab_runner {
      source                               = "../module/"
      vpc_id                               = var.vpc_id
      security_group_id                    = var.security_group_id
      namespace                            = "eg"
      name                                 = "app"
      stage                                = "test"
      attributes                           = ["xyz"]
      ssh_key_name                         = var.ssh_key_name
      gitlab_runner_count                  = var.gitlab_runner_count
      gitlab_runner_url                    = var.gitlab_runner_url
      gitlab_runner_token                  = var.gitlab_runner_token
      gitlab_runner_tags                   = var.gitlab_runner_tags
      gitlab_runner_docker_registry_url    = var.gitlab_runner_docker_registry_url
      gitlab_runner_docker_registry_auth   = var.gitlab_runner_docker_registry_auth
}
```

## INPUT VALUES

| Input                                | Description                                                                                                                                           | Type    | Default                  | Required |
| -------------------------------------| ------------------------------------------------------------------------------------------------------------------------------------------------------| --------|--------------------------|----------|
| namespace                            | Namespace, which could be your organization name or abbreviation"                                                                                     | `string`| ""                       | yes      |
| stage                                | Stage, e.g. 'prod', 'staging', 'dev'                                                                                                                  | `string`| ""                       | yes      |
| name                                 | Solution name, e.g. 'app' or 'jenkins'                                                                                                                | `string`| ""                       | yes      |
| attributes                           | Additional attributes                                                                                                                                 | `list`  | `<list>`                 | no       |           
| delimiter                            | Delimiter to be used between namespace, environment, stage, name and attributes                                                                       | `string`| "-"                      | no       |
| security_group_id                    | ID of Security group for gitlab runner instances                                                                                                      | `string`| ""                       | yes      |
| vpc_id                               | VPC ID                                                                                                                                                | `string`| ""                       | yes      |
| ssh_key_name                         | Key Pair to be used for gitlab-runner ec2 instance                                                                                                    | `string`| ""                       | yes      |
| root_ebs_size                        | Size of EBS volume attached to gitab runner                                                                                                           | `number`| `32`                     | no       |
| gitlab_runner_count                  | How many runners you want?                                                                                                                            | `number`| `2`                      | no       |
| gitlab_runner_url                    | URL for Runner setup                                                                                                                                  | `string`| ""                       | yes      |
| gitlab_runner_token                  | Gitlab Runner registration token                                                                                                                      | `string`| ""                       | yes      |
| gitlab_runner_tags                   | Gitlab Runner tag list (comma separated)                                                                                                              | `string`| ""                       | yes      |
| gitlab_runner_docker_image           | Gitlab Runner default docker image.                                                                                                                   | `string`| `alpine:3.9`             | no       |
| gitlab_runner_docker_registry_url    | URL of a private docker registry.                                                                                                                     | `string`| ""                       | no       |
| gitlab_runner_docker_registry_auth   | Base64 encoded basic auth credentials for the private registry (`echo -n "my_username:my_password" | base64`).                                        | `string`| ""                       | no       |
| gitlab_concurrent_job                | Number of Gitlab CI concurrent job to run                                                                                                             | `string`| `"4"`                    | no       |
| gitlab_check_interval                | Gitlab CI check interval value                                                                                                                        | `string`| `"0"`                    | no       |
| amazon_linux_ami                     | AMI for AWS EC2 instance resources.                                                                                                                   | `string`| `"ami-0ff8a91507f77f867"`| no       |

## OUTPUT VALUE NAMES

| Name                              | Description                                   | 
| ----------------------------------| ----------------------------------------------| 
| instance_public_ip                | List of Gitlab Runner Instance   Public IP    | 