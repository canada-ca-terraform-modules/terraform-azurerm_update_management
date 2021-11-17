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