# Lab Exercise Commands

A working directory must be initialized before Terraform can perform any operations in it. Initialize your Terraform directory including a download of providers. In our case, the PingOne provider.

    terraform init

Reformat your configuration in the standard style.
"In matters of utmost importance, style, not sincerity, is the vital thing."
~ Oscar Wilde

    terraform fmt

Check whether your configuration is valid.

    terraform validate

**Optional** Show output values from your root module.

    terraform output

Show changes required by the current configuration and save it to a plan file. It evaluates a Terraform configuration to determine the desired state of all the resources it declares, then compares that desired state to the real infrastructure objects being managed with the current working directory and workspace.

    terraform plan -out=tfplan

Create or update infrastructure. It performs a plan just like terraform plan does, but then actually carries out the planned changes to each resource using the relevant infrastructure Terraform provider's API

    terraform apply tfplan

Passing vars into terraform plan

    terraform plan -var="admin_env_id=98fcf1dd-b4f9-4bd8-acdf-7292efc3112a" -var="admin_user_id=71bb94f5-89a8-4cad-a506-df5e7ad811ea" -var="env_name=SKO23TFtest2" -var="org_id=0f2ff549-eaba-4515-b278-45097d8bb913" -var="worker_id=b6e3bbcb-fa9c-4808-a420-243f1641557f" -var="worker_secret=PtcUC2~skzkvarbD2dP-o40U0mViTL7LER9O3UdvTzT5-yWQGRxISmOj_tixWio3"

Passing vars into terraform apply

    terraform apply -var="admin_env_id=98fcf1dd-b4f9-4bd8-acdf-7292efc3112a" -var="admin_user_id=71bb94f5-89a8-4cad-a506-df5e7ad811ea" -var="env_name=SKO23TFtest2" -var="org_id=0f2ff549-eaba-4515-b278-45097d8bb913" -var="worker_id=b6e3bbcb-fa9c-4808-a420-243f1641557f" -var="worker_secret=PtcUC2~skzkvarbD2dP-o40U0mViTL7LER9O3UdvTzT5-yWQGRxISmOj_tixWio3"

Destroy previously-created infrastructure. While you typically don't want to destroy long-lived objects in a production environment, Terraform is sometimes used to manage ephemeral infrastructure for development or testing purposes, in which case you can use terraform destroy to conveniently clean up all of those temporary objects once you are finished with your work.

    terraform destroy

*Alias: terraform apply -destroy*




