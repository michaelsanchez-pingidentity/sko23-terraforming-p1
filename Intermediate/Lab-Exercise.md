# Lab Exercise

Just planning notes. Move this to the Slides presentation.

1. Flog anyone not prepared with pre-reqs.
1. Clone lab project.
1. Explain `terraform init` and initialize the project directory.
1. Demo and run the format command. *Readability, consistency*.
1. Explain `terraform validate` and run it.
1. Terraform a new PingOne environment in your tenant.
    - *P1Risk and Authorize services are commented out.*
    - Run `terraform plan -out=skotfplan`. Copy and paste values when prompted. A plan file is saved.
    -- Talk about no stipulation of file names. Builds dependency graph.
    - Run `terraform output` as example. (*TF state file required for this*.)
    - Run `terraform apply skotfplan`. If you didn't pass in the plan file, Terraform would ask for the var values again to see if state is changing. (`terraform apply` runs an execution plan again to avoid unnecessary runs or pick up changes by comparing state to .tf files).
    - Creates a new environment.
    - Reload your P1 admin console.
1. Change the Terraform files to modify your new PingOne environment.
    - Comment out the SAML app. (Triggers a destroy).
    - Uncomment the P1Risk and Authorize services. (Triggers an add).
    - Run `terraform plan -var="varName=varValue" [-var="varName=varValue" ...] -out=skotfplan` to update the plan file.
    - Run `terraform apply skotfplan`. (Alternatively, you could have run `terraform apply -var="varName=varValue" [-var="varName=varValue" ...]`).
    - View update message in console.
    - Updates your new environment.
    - Reload your P1 admin console.
1. Destroy all the things.
    - `terraform plan [vars -out=destroyPlan] -destroy`
    - `terraform destroy skotfplan`
1. Using tfvars to abstract your sensitive data.
    - Talk about setting defaults in vars.tf but concern about sensitive data
    - Create terraform.tfvars
    - Add name value pairs used in previous exercises. (Names much mactch what is in vars.tf).
    - run stuff.
    - Updates your new environment again.
    - Reload your P1 admin console.
1. Tear it all down. 
(*Do we do this earlier and then recreate so they have an environment for the advanced sessions? Or move the tear down to the advanced session?*)
1. Q&A.
1. Point out reference links and where slides will be available.