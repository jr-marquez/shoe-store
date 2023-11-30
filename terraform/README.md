![image](img/confluent-logo-300-2.png)

# Workshop Deployment via terraform

IMPORTANT TO KNOW FOR THE WORKSHOP:
We run in AWS only. Currently we do support [4 Regions](https://docs.confluent.io/cloud/current/flink/reference/op-supported-features-and-limitations.html#cloud-regions) within AWS cloud.
The complete onsite team is working in region: `eu-central-1` (all terraform and manual guide do not need to change)

This is the deployment of confluent cloud infrastructure resources to run the Flink SQL Hands-on Workshop.
We will deploy with terraform:
 - Environment:
     - Name: flink_hands-on+UUID
     - with enabled Schema Registry (essentails) in AWS region (eu-central-1)
 - Confluent Cloud Basic Cloud: cc_handson_cluster
    - in AWS in region (eu-central-1)
 - Connectors:
    - Datagen for shoe_products
    - Datagen for shoe_customers 
    - Datagen for show_orders
 - Service Accounts
    - app_manager-XXXX with Role EnvironmentAdmin
    - sr-XXXX with Role EnvironmentAdmin
    - clients-XXXX with Role CloudClusterAdmin
    - connectors-XXXX
 
![image](img/terraform_deployment.png)

# Pre-requisites
 - Visit the [Cloud API Key](https://confluent.cloud/settings/api-keys) page to create a Cloud API Key for your user, if you don't have any yet.

## Set environment variables
- Add your API key to the Terraform variables by creating a tfvars file
```bash
cat > $PWD/terraform/terraform.tfvars <<EOF
confluent_cloud_api_key = "{Cloud API Key}"
confluent_cloud_api_secret = "{Cloud API Key Secret}"
EOF
```

## Deploy via terraform
run the following commands:
```Bash
cd ./terraform
terraform init
terraform plan
terraform apply
```

Please check whether the terraform execution went without errors.

There are two ways to continue, either over shell or over UI. If you want to start with the shell, please type:

```
eval $(echo -e "confluent flink shell --compute-pool $(terraform output cc_compute_pool_name) --environment $(terraform output cc_hands_env)")
```

You can copy the login instruction also from the UI.

To continue with the UI:
 - Access Confluent Cloud WebUI: https://confluent.cloud/login
 - Access your Environment: `flink_handson_terraform-XXXXXXXX`
 - Start with [lab1](../lab1.md)

You deployed:

![image](img/terraform_deployment.png)

You are ready to [start with LAB1](../lab1.md)

# Destroy the hands.on infrastructure
```bash
terraform destroy
```
There could be a conflict destroying everything with our Tags. In this case destroy again via terraform.
```bash
#╷
#│ Error: error deleting Tag "<tagID>/PII": 409 Conflict
#│ 
#│ 
#╵
#╷
#│ Error: error deleting Tag "<tagID>/Public": 409 Conflict
#│ 
#│ 
#╵
# destroy again
terraform destroy
``` 
