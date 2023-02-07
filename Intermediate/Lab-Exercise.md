# Lab Exercises
## TL;DR Version

A working directory must be initialized before Terraform can perform any operations in it. Executing the Initialize command in your Terraform directory will create the required files, including a download of providers and modules. All of these resources are downloaded from the Terraform Registry. This is no different than Google Container Registry (GCR) for Docker images, or DockerHub, or AWS ECR. Or even the NPM Registry for Node packages. In our case, we're getting PingOne providers and modules.  After the directory is initialized there will be some files that should not be directly managed/updated such as lock files or anything inside the `.terraform` directory

    terraform init

The next step will reformat your configuration in the standard style, this includes variable files, configuration files and the direct structure.

**BEFORE** running the command, look at the `main.tf` file and the `provider pingone` declaration and notice the layout around the equal signs. Run the command and look at how it changed.

    terraform fmt

>"In matters of utmost importance, style, not sincerity, is the vital thing."
~ Oscar Wilde

Terraform can show a dry-run of the changes required by the current configuration and save it to a plan file. It evaluates a Terraform configuration to determine the desired state of all the resources it declares, then compares that desired state to the real infrastructure objects being managed with the current working directory and workspace. A plan file is not required but can be usedful in a CI/CD pipeline implemenation where a plan file can be pre-generated and referenced in the `apply` command we'll see later.

Since we didn't provide variable values, Terraform will prompt you for any missing variables values.

    terraform plan -out=tfplan

To check whether your configuration is valid, a sanity check can be run to verify the status.  This is particularly important when implementing newer versions of Terraform projects, as the may have updated requirements such as new variables or renamed objects used by Terraform.  In cases where validate shows newer versions of modules, it might be necessary to run “terraform init -upgrade” to resolve new interdependencies.

Open the `vars.tf` file and remove one of the curly braces, save the change, and run the following command. Notice the output from the command.

    terraform validate

To commit, create or update infrastructure Terraform has the apply command. It performs a plan just like terraform plan does, but then actually carries out the planned changes to each resource using the relevant infrastructure Terraform provider’s API. This meands depending on your implementation scenario, you can simply do an `init` and then `apply` without running a `plan`.

After you run this command, you can browse to the URL output to terminal window by Terraform.

    terraform apply tfplan

**Optional** Show output values from your root module. You can see this configuration in the outputs.tf file. An output file is used when you want to display dynamic feedback to the user as a result of the `terraform apply` run. you should have seen this output after running the previous `apply` command.

    terraform output


 This time comment out the SAML app configs and UNcomment the P1AZ configs so we can change the state of our infrastructure. Then run the plan command this time passing in the var values as parameters to the command.

 Passing vars as input params to the `terraform plan` command.

    terraform plan -var="admin_env_id=XXXX" -var="admin_user_id=XXXX" -var="env_name=SKO 23 Lab Test" -var="org_id=XXXX" -var="region=XXXX" -var="worker_id=XXXX" -var="worker_secret=XXXX" -out=skotfplan

Now run the `apply` command passing in the new plan file we created. When it's done, reload your P1 console and view the added and removed services.

    terraform apply -out=skotfplan


Destroying previously created infrastructure. 

While you typically don't want to destroy long-lived objects in a production environment, Terraform is sometimes used to manage ephemeral infrastructure for development or testing purposes, prototypes, PenTest environments, etc, in which case you can use terraform destroy to conveniently clean up all of those temporary objects once you are finished with your work.

Notice the addition of the `-destroy` switch to the command, and the new name we gave our plan to keep it clear what the plan is for.

Plan the destroy

    terraform plan -var="admin_env_id=XXXX" -var="admin_user_id=XXXX" -var="env_name=SKO 23 Lab Test" -var="org_id=XXXX" -var="region=XXXX" -var="worker_id=XXXX" -var="worker_secret=XXXX" -destroy -out=destroyPlan

Destroy all the thingz!

    terraform apply destroyPlan

Any company, especialy a security company like Ping should protect sensitive data. This should not be included in your HCL and .tf files. They way to do that is put sensitive data in .tfvars files and not include them in your repository. Terraform checks for their existance and uses them. So you can add them at runtime with a pipeline or other mechanism to pull them from a vault.

Create and use a tfvars file.

    touch terraform.tfvars

Add all your variable name/value pairs into this file. They should just be in the format,

>varName = "varValue"
varName2 = "varValue2"
*be sure to end the file with a blank new line.*

Create a new plan using tfvars

    terraform plan -var="admin_env_id=XXXXa" -var="admin_user_id=XXXX" -var="env_name=SKO 23 Lab Test" -var="org_id=XXXX" -var="region=XXXX" -var="worker_id=XXXX" -var="worker_secret=XXXX"  -out=skotfvarsplan

Apply it

    terraform apply skotfvarsplan


