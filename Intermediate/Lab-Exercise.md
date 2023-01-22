# Lab Exercise

Just planning notes. Move this to the Slides presentation.

1. This project repo includes everything needed to self-run this lab at a later date if needed.
1. Clone lab project. `git clone https://github.com/michaelsanchez-pingidentity/sko23-terraforming-p1.git --branch SKO23-labs --single-branch`
1. Explain `terraform init` and initialize the project directory. *The .terraform directory is a hidden directory which is used by Terraform to cache provider plugins referenced by the Terraform project code.*
1. Show the main.tf file and run the `terraform fmt` command. *Readability, consistency*.
1. Explain `terraform validate` and run it. *Find issues in advance.* 
    - In vars.tf, rename `locals` to `localsx` and validate again.
1. Terraform a new PingOne environment in your tenant.
    - *P1Risk and Authorize services are commented out.*
    - Run `terraform plan -out=skotfplan`. Copy and paste values when prompted. A plan file is saved.
    -- Talk about no stipulation of file names. Builds dependency graph.
    - Run `terraform apply skotfplan`. If you didn't pass in the plan file, Terraform would ask for the var values again to see if state is changing. (`terraform apply` runs an execution plan again to avoid unnecessary runs or pick up changes by comparing state to .tf files).
    - Run `terraform output` as example. (*TF state file required for this*.)
    - Creates a new environment.
    - Reload your P1 admin console.
1. Change the Terraform files to modify your new PingOne environment.
    - Comment out the SAML app. (Triggers a destroy).
    - Uncomment the P1Risk and Authorize services. (Triggers an add).
    - Run `terraform plan -var="varName=varValue" [-var="varName=varValue" ...] -out=skotfplan` to update the plan file.
    - Run `terraform apply skotfplan`. 
    (Alternatively, you could have run `terraform apply -var="varName=varValue" [-var="varName=varValue" ...]`).
    - View update message in console.
    - Updates your new environment.
    - Reload your P1 admin console.
1. Destroy all the things.
    - `terraform plan -var="admin_env_id=98fcf1dd-b4f9-4bd8-acdf-7292efc3112a" -var="admin_user_id=71bb94f5-89a8-4cad-a506-df5e7ad811ea" -var="env_name=SKO 23 Lab Test0" -var="org_id=0f2ff549-eaba-4515-b278-45097d8bb913" -var="region=NorthAmerica" -var="worker_id=b6e3bbcb-fa9c-4808-a420-243f1641557f" -var="worker_secret=PtcUC2~skzkvarbD2dP-o40U0mViTL7LER9O3UdvTzT5-yWQGRxISmOj_tixWio3" -destroy -out=destroyPlan`
    - `terraform apply destroyPlan`
1. Using tfvars to abstract your sensitive data.
    - Talk about setting defaults in vars.tf but concern about sensitive data
    - Create .tfvars file. `touch development.tfvars`
    - Add name value pairs for worker app and secret used in previous exercises. *Names much match what is in vars.tf. Can't declare new vars, only provide values*.
    - Run `terraform plan -var="varName=varValue" [-var="varName=varValue" ...] -out=skotfvarsplan` to update the plan file.
    - Run `terraform apply skotfvarsplan`.
    - Updates your new environment again.
    - Reload your P1 admin console.
    - This won't scale for multiple environments, like dev, test, stage, prod. But you can create environment specific files, named as needed; dev.tfvars, prod.tfvars and you can pass the file name in at runtime, making it easy to manage in a pipeline.
1. Don't edit or delete your project files if you are attending the advanced session next. We continue from here.
1. Q&A.
1. Point out reference links and where slides will be available.