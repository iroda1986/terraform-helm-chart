## Terraform module helm-deploy

This terraform module will help you deploy the helm charts on local.

- [Requirements](#Requirements)
- [Usage](#usage)
- [Variables](#variables)
- [Dependencies](#dependencies)
- [Links](#links)

## Requirements

Terraform >= 0.11.7

Kubernetes  >=  v1.14.8

Tiller >= v2.11.0

## Before you begin

1. Make sure that you have `kubectl` installed and you have configured your `~/.kube/config` 
2. Make sure you have tiller installed on your  kubernetes cluster
3. Make sure that terraform also installed and follows [Requirements](#Requirements)

## Usage

First you will need to create your own helm chart [link](https://docs.bitnami.com/kubernetes/how-to/create-your-first-helm-chart/). If you would like to quickly create helm chart then run

```
╭─ fsadykov ~
╰─() mkdir -p deployments/terraform/charts && cd deployments/terraform
╭─ fsadykov ~
╰─() helm create charts/example
Creating example
╭─ fsadykov ~
╰─() ls charts/example
Chart.yaml  charts      templates   values.yaml
```

After you created your helm chart you can go ahead and do modification inside `values.yaml`

when modification is done for `values.yaml` you will need to create `module.tf` to call the module 

```hcl
cat <<EOF >module.tf
module "helm_deploy" {
  source                 = "git::https://github.com/fuchicorp/helm-deploy.git"
  deployment_name        = "example-deployment"
  deployment_environment = "dev"
  deployment_endpoint    = "example.fuchicorp.com"
  deployment_path        = "example"
}
EOF
```

follow the file path 

```yaml
./module.tf
./charts/
    /example ## Your chart location 
      /Chart.yaml
      /charts
      /templates
      /values.yaml
```



## Variables

For more info, please see the [variables file](variables.tf).

| Variable               | Description                         | Default                                               |
| :--------------------- | :---------------------------------- | :---------------------------------------------------- |
| `deployment_name` | The name of the deployement for helm release | `(Required)` |
| `deployment_environment` | Name of the namespace | `(Required)` |
| `deployment_endpoint` | Ingress endpoint `example.fuchicorp.com` | `(Required)` |
| `deployment_path` | path for helm chart on local | `(Required)` |
| `env_vars` | Environment veriable for the containers takes map | `(Optional)` |

## Dependencies

This module exposes two variables to work around the limitations of modules in Terraform.

| Name               | Type                         | Description                                     |
| :----------------- | :--------------------------- | :---------------------------------------------- |
| `depends_on` | `list variable` | Dummy variable to enable module dependencies. |
| `dependency` | `list output` | Dummy output to enable module dependencies. |
