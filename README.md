<!-- BEGIN_TF_DOCS -->
# keyvault

This module manages Azure Keyvault Configuration.

<-- This file is autogenerated, please do not change. -->

## Requirements

| Name | Version |
|------|---------|
| terraform | >=0.12 |
| azurerm | >=2.19.0 |

## Providers

| Name | Version |
|------|---------|
| azurerm | >=2.19.0 |

## Resources

| Name | Type |
|------|------|
| azurerm_key_vault.keyvault | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| location | location where the resource should be created | `string` | n/a | yes |
| keyvault | resource definition, default settings are defined within locals and merged with var settings | `any` | `{}` | no |
| keyvault_config | resource configuration, default settings are defined within locals and merged with var settings | `any` | `{}` | no |
| resource_name | Azure Keyvault | `set(string)` | `[]` | no |
| tags | mapping of tags to assign, default settings are defined within locals and merged with var settings | `any` | `{}` | no |

## Outputs

| Name | Description |
|------|-------------|
| keyvault | azurerm_keyvault results |

## Examples

```hcl
module "keyvault" {
  source              = "../terraform-keyvault"
  location            = "westeurope"
  resource_name = [
    "service-mgmt-kv",
  ]
  keyvault        = {
    resource_group_name = "service-mgmt-rg"
    tenant_id           = data.azurerm_subscription.current.tenant_id
  }
  keyvault_config = {
    mgmt = {
      access_policies = {
        frontdoor = {
          object_id               = data.azuread_service_principal.frontdoor.object_id
          key_permissions         = []
          certificate_permissions = ["get", ]
          secret_permissions      = ["get", ]
        }
      }
    }
    env = {
      access_policies = {
        admin       = {
          object_id = data.azuread_group.grp-admin.object_id
        }
      }
    }
  }
  tags = {
    service = "service_name"
  }
}
```
<!-- END_TF_DOCS -->