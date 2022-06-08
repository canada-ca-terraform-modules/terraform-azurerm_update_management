# Terraform Update Management Schedule

## Introduction

This module deploys Update Management Schedules for Linux and Windows servers.

## Dependancies

Hard:

* Resource Groups
* AutomationAccount

## Usage

```hcl
module "linux-weekly-updates" {
  source                     = "github.com/canada-ca-terraform-modules/terraform-azurerm_update_management?ref=20211117.1"
  count                      = var.deployOptionalFeatures.update_management ? 1 : 0
  name                       = substr("${local.prefix}-${var.project}-linux-weekly-updates", 0, 64)
  resource_group_name        = local.resource_groups_L1.AutomationAccount.name
  azurerm_automation_account = azurerm_automation_account.Project-aa
  operatingSystem            = "Linux"
  scope                      = [data.azurerm_subscription.primary.id]               # Whole subscription
  update_time                = "00:00"
  weekDays                   = ["Sunday"]
  depends_on                 = [azurerm_log_analytics_linked_service.law_link]
}

module "windows-weekly-updates" {
  source                     = "github.com/canada-ca-terraform-modules/terraform-azurerm_update_management?ref=20211117.1"
  count                      = var.deployOptionalFeatures.update_management ? 1 : 0
  name                       = substr("${local.prefix}-${var.project}-windows-weekly-updates", 0, 64)
  resource_group_name        = local.resource_groups_L1.AutomationAccount.name
  azurerm_automation_account = azurerm_automation_account.Project-aa
  operatingSystem            = "Windows"
  scope                      = [data.azurerm_subscription.primary.id]               # Whole subscription
  update_time                = "00:00"
  weekDays                   = ["Sunday"]
  depends_on                 = [azurerm_log_analytics_linked_service.law_link]
}
```

## Requirements

No requirements.

## Providers

| Name | Version |
|------|---------|
| <a name="provider_azurerm"></a> [azurerm](#provider\_azurerm) | n/a |
| <a name="provider_null"></a> [null](#provider\_null) | n/a |
| <a name="provider_time"></a> [time](#provider\_time) | n/a |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [azurerm_resource_group_template_deployment.linux](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/resource_group_template_deployment) | resource |
| [azurerm_resource_group_template_deployment.windows](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/resource_group_template_deployment) | resource |
| [null_resource.linux](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource) | resource |
| [null_resource.windows](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource) | resource |
| [time_offset.tomorrow](https://registry.terraform.io/providers/hashicorp/time/latest/docs/resources/offset) | resource |
| [azurerm_client_config.current](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/data-sources/client_config) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_azurerm_automation_account"></a> [azurerm\_automation\_account](#input\_azurerm\_automation\_account) | azurerm\_automation\_account object | `object({ name = string })` | n/a | yes |
| <a name="input_duration"></a> [duration](#input\_duration) | Time in hour permitted for updates | `string` | `"2"` | no |
| <a name="input_interval"></a> [interval](#input\_interval) | Integer for interval | `number` | `1` | no |
| <a name="input_linux_update_types"></a> [linux\_update\_types](#input\_linux\_update\_types) | Types of updates to apply when OS type is Linux. One or more of: Critical, Other, Security. | `string` | `"Critical, Other, Security"` | no |
| <a name="input_name"></a> [name](#input\_name) | Name of the update management schedule | `string` | n/a | yes |
| <a name="input_operatingSystem"></a> [operatingSystem](#input\_operatingSystem) | Name of the VM OS type. Linux or Windows | `string` | n/a | yes |
| <a name="input_rebootSetting"></a> [rebootSetting](#input\_rebootSetting) | One of: Never, Always, IfRequired | `string` | `"IfRequired"` | no |
| <a name="input_resource_group_name"></a> [resource\_group\_name](#input\_resource\_group\_name) | Name of the resource group where the automation account is located | `string` | n/a | yes |
| <a name="input_scope"></a> [scope](#input\_scope) | Comma seperated string of resource groups id in scope for update | `list(string)` | `[]` | no |
| <a name="input_timeZone"></a> [timeZone](#input\_timeZone) | Timezone where schedule run | `string` | `"Eastern Standard Time"` | no |
| <a name="input_update_time"></a> [update\_time](#input\_update\_time) | String of time when to start schedule | `string` | n/a | yes |
| <a name="input_weekDays"></a> [weekDays](#input\_weekDays) | List of days to run. One or namy of: Sunday, Monday, Tuesday, Wednesday, Thrusday, Friday, Saturday. | `list(string)` | `[]` | no |
| <a name="input_windows_update_types"></a> [windows\_update\_types](#input\_windows\_update\_types) | Types of updates to apply when OS type is Windows. One or more of: Critical, Security, UpdateRollup, ServicePack, Definition, Updates | `string` | `"Critical, Security, UpdateRollup, ServicePack, Definition, Updates"` | no |

## Outputs

No outputs.
