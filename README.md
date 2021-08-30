# Big Bang Terraform Start

## Prerequisites

- Linux or MacOS (runs a bash script to return information about Istio's LoadBalancers)
- `terraform`
- `kubectl`
- `jq`
- `base64`

## Getting Started

An example of a project that uses this Terraform module can be found [here](https://repo1.dso.mil/platform-one/quick-start/big-bang).

## Instructions

### Using a vars file:

```bash
# Copy and edit with your values
cp terraform.tfvars.example terraform.tfvars 

terraform init
terraform plan
terraform apply
```

### Using environment variables:

```bash
# Update with your values
export TF_VAR_registry_credentials='[{registry="registry1.dso.mil",username="REPLACE_ME",password="REPLACE_ME"}]'

#Optional: Reduce flux resource requests for edge/resource constrained environments
export TF_VAR_reduce_flux_resources=true

terraform init
terraform plan
terraform apply
```

### Using inline arguments:

```bash
# Update with your values inline
terraform init
terraform plan -var 'registry_credentials=[{registry="registry1.dso.mil",username="REPLACE_ME",password="REPLACE_ME"}]'
terraform apply -var 'registry_credentials=[{regsitry="registry1.dso.mil",username="REPLACE_ME",password="REPLACE_ME"}]'
```

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.13 |
| <a name="requirement_kubectl"></a> [kubectl](#requirement\_kubectl) | >= 1.7.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_external"></a> [external](#provider\_external) | n/a |
| <a name="provider_kubectl"></a> [kubectl](#provider\_kubectl) | >= 1.7.0 |
| <a name="provider_kubernetes"></a> [kubernetes](#provider\_kubernetes) | n/a |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_registry_credentials"></a> [registry\_credentials](#input\_registry\_credentials) | System-wide registry credentials to be applied so Kubernetes can pull container images. Creds for registry1.dso.mil are required, and you can optionally provide any other creds for other private registries as well | <pre>list(object({<br>    registry = string<br>    username = string<br>    password = string<br>  }))</pre> | n/a | yes |
| <a name="input_big_bang_manifest_file"></a> [big\_bang\_manifest\_file](#input\_big\_bang\_manifest\_file) | Path to the root k8s yaml manifest. Typically contains a Namespace, GitRepository, and Kustomization. See https://repo1.dso.mil/platform-one/quick-start/big-bang/-/blob/master/bigbang/start.yaml for example | `string` | `"k8s/start.yaml"` | no |
| <a name="input_kube_conf_file"></a> [kube\_conf\_file](#input\_kube\_conf\_file) | Path to the KUBECONFIG file to use to connect to the cluster. If the file passed has multiple contexts in it the correct context is expected to already be set in `contexts.current-context`. | `string` | `"~/.kube/config"` | no |
| <a name="input_reduce_flux_resources"></a> [reduce\_flux\_resources](#input\_reduce\_flux\_resources) | DEPRECATED - Used to tweak resource settings to fit on a smaller machine, but the new Flux deployment already uses the smaller values. `flux.yaml` and `flux_light.yaml` are now identical files. | `bool` | `false` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_external_load_balancer"></a> [external\_load\_balancer](#output\_external\_load\_balancer) | JSON array with information on all LoadBalancer services in the istio-system namespace. Example output:<pre>[<br>  {<br>    "name": "public-ingressgateway",<br>    "ip": "192.0.2.0",<br>    "hostname": "null"<br>  },<br>  {...}<br>]</pre> |
<!-- END_TF_DOCS -->
