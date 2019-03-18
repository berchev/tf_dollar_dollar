# tf_dollar_dollar
This repository is a guideline on HOW to use data source `template_file`, provisioner `file` and escaping interpolation syntax `$${}`, in Terraform.

## What this Terraform code do?
- The code provided in this repository will create AWS EC2 instance. 
- Once the instance is created we will send the content of `template.tpl` file to `/tmp/script.sh` file.
- We will run the `/tmp/script.sh` bash script (This script should create file called `testfile1` into user's home directory - `$HOME`)

## Basic information
- `template_file` - The **template_file** data source renders a template from a template string, which is usually loaded from an external file. In our case it will be **template.tpl** file.
- provisioner `file` - The **file** provisioner is used to copy files or directories from the machine executing Terraform to the newly created resource. **Source** and **Destination** need to be specified. In our case we **don't want** to copy file, we want to get the content from `template_file` data source and then to create file remotely, with that content! For this purpose we will specify **Content** and **Destination**.
- `$${}` (escaping interpolation syntax) - Let's take a look at our `template.tpl` content:
    ```
    #!/usr/bin/env bash

    touch $${HOME}/testfile1

    ```

    We mentioned above that we would like to crete `testfile1` into user's into user's home directory. If we use `touch ${HOME}/testfile1`, we will hit an error, because Terraform will try to do **variable interpolation** with `$HOME` variable and will try to replace `$HOME` with it's value.
    We do not want this! We want to pass `$HOME` variable to newly created ubuntu machine, where we will use it!

    We can accomplish this using `$${HOME}`. 

    `$${}` have an special meaning and after Terraform process the template, will replace `$${}` with `${}`
    
    This is how is going to look our script after processing:
    ![]()

## Files
|Files | Description |
| --- | --- |
|`main.tf` | our example code |
|`template.tpl` | file pattern used from template engine |
|`image` | screenshot folder |

## Requirements
- Terraform installed. Click [here](https://learn.hashicorp.com/terraform/getting-started/install.html) to install
- AWS account
  - If you do not have AWS account click [here](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/) in order to create one.
  - Set [MFA](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#multi-factor-authentication)
  - Set [Access Key ID and Secret Access Key ](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys)
  - Set [Key Pair](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#key-pairs)

## How to run this project?
- `git clone https://github.com/berchev/tf_dollar_dollar.git`
- `cd tf_dollar_dollar`
- Export following ENV Variables:
    ```
    export AWS_ACCESS_KEY_ID="XXXXXXXXXXXXXXXXXXXX"
    export AWS_SECRET_ACCESS_KEY="XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    export AWS_DEFAULT_REGION="us-west-2"
    ```
- `terraform init`
- `terraform apply`

## TODO
