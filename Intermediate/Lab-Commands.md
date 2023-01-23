# Lab Exercises
## TL;DR Version

A working directory must be initialized before Terraform can perform any operations in it. Executing the Initialize command in your Terraform directory will create the required files, including a download of providers and modules. All of these resources are downloaded from the Terraform Registry. This is no different than Google Container Registry (GCR) for Docker images, or DockerHub, or AWS ECR. Or even the NPM Registry for Node packages. In our case, we're getting PingOne providers and modules.  After the directory is initialized there will be some files that should not be directly managed/updated such as lock files.

    terraform init

The next step will reformat your configuration in the standard style, this includes variable files, configuration files and the direct structure.
"In matters of utmost importance, style, not sincerity, is the vital thing."
~ Oscar Wilde

    terraform fmt

To check whether your configuration is valid, a sanity check can be run to verify the status.  This is particularly important when implementing newer versions of Terraform projects, as the may have updated requirements such as new variables or renamed objects used by Terraform.  In cases where validate shows newer versions of modules, it might be necessary to run “terraform init -upgrade” to resolve new interdependencies.

    terraform validate

Terraform can show a dry-run of the changes required by the current configuration and save it to a plan file. It evaluates a Terraform configuration to determine the desired state of all the resources it declares, then compares that desired state to the real infrastructure objects being managed with the current working directory and workspace.

Prompting for var values.

    terraform plan -out=tfplan

To commit, create or update infrastructure Terraform has the apply command. It performs a plan just like terraform plan does, but then actually carries out the planned changes to each resource using the relevant infrastructure Terraform provider’s API.

    terraform apply tfplan

**Optional** Show output values from your root module. You can see this configuration in the outputs.tf file.

    terraform output


 This time comment out the SAML app configs and UNcomment the P1AZ configs so we can change the state of our infrastructure. Then run the plan command this time passing in the var values as parameters to the command.

 Passing vars into terraform plan command.

    terraform plan -var="admin_env_id=XXXX" -var="admin_user_id=XXXX" -var="env_name=SKO 23 Lab Test" -var="org_id=XXXX" -var="region=XXXX" -var="worker_id=XXXX" -var="worker_secret=XXXX" -out=skotfplan

Now run the apply plan passing in the new plan file we created. When it's done, reload your P1 console and view the added and removed services.

    terraform apply -out=skotfplan

Destroy previously created infrastructure. While you typically don't want to destroy long-lived objects in a production environment, Terraform is sometimes used to manage ephemeral infrastructure for development or testing purposes, in which case you can use terraform destroy to conveniently clean up all of those temporary objects once you are finished with your work.

Plan the destroy

    terraform plan -var="admin_env_id=XXXX" -var="admin_user_id=XXXX" -var="env_name=SKO 23 Lab Test" -var="org_id=XXXX" -var="region=XXXX" -var="worker_id=XXXX" -var="worker_secret=XXXX" -destroy -out=destroyPlan

Destroy all the thingz

    terraform apply destroyPlan

Any company, especialy a security company like Ping should protect sensitive data. This should not be included in your HCL and .tf files. They way to do that is put sensitive data in .tfvars files and not include them in your repository. Terraform checks for their existance and uses them. So you can add them at runtime with a pipeline or other mechanism.

Using tfvars file

    touch terraform.tfvars

Move your workerapp client ID and secret, or all your variables into this file.

Create a new plan using tfvars

    terraform plan -var="admin_env_id=XXXXa" -var="admin_user_id=XXXX" -var="env_name=SKO 23 Lab Test" -var="org_id=XXXX" -var="region=XXXX" -var="worker_id=XXXX" -var="worker_secret=XXXX" -out=skotfplan -out=skotfvarsplan

Apply it

    terraform apply skotfvarsplan


