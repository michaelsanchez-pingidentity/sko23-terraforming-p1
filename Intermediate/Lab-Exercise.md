# Lab Exercise

Just planning notes. Move this to the Slides presentation.

1. Clone lab project.
1. Flog anyone not prepared with pre-reqs.
1. Terraform a new PingOne environment in your tenant.
    - *P1Risk and Authorize services are commented out.*
    - Run `terraform plan -out=skotfplan`. Copy and paste values when prompted. A plan file is saved.
    - Run `terraform apply skotfplan`. If you didn't pass in the plan file, Terraform would ask for the var values again to make sure nothing has changed. (`terraform apply` runs an execution plan again).
    - Creates a new environment.
    - Reload your P1 admin console.
1. Change the Terraform files to modify your new PingOne environment.
    - Comment out the SAML app.
    - Uncomment the P1Risk and Authorize services.
    - Run `terraform plan -var="varName=varValue" [-var="varName=varValue" ...] -out=skotfplan` to update the plan file.
    - Run `terraform apply skotfplan`. (Alternatively, you could have run `terraform apply -var="varName=varValue" [-var="varName=varValue" ...]`).
    - Updates your new environment.
    - Reload your P1 admin console.
1. Doing something with the vars.tf file
    - something.
    - something else.
    - run stuff.
    - Updates your new environment again.
    - Reload your P1 admin console.
1. Tear it all down. 
(*Do we do this earlier and then recreate so they have an environment for the advanced sessions? Or move the tear down to the advanced session?*)
1. Q&A.
1. Point out reference links and where slides will be available.