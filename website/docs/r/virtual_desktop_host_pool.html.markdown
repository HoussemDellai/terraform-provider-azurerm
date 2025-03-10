---
subcategory: "Desktop Virtualization"
layout: "azurerm"
page_title: "Azure Resource Manager: azurerm_virtual_desktop_host_pool"
description: |-
  Manages a Virtual Desktop Host Pool.
---

# virtual_desktop_host_pool

Manages a Virtual Desktop Host Pool.

## Example Usage

```hcl
resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "West Europe"
}

resource "azurerm_virtual_desktop_host_pool" "example" {
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  name                     = "pooleddepthfirst"
  friendly_name            = "pooleddepthfirst"
  validate_environment     = true
  start_vm_on_connect      = true
  custom_rdp_properties    = "audiocapturemode:i:1;audiomode:i:0;"
  description              = "Acceptance Test: A pooled host pool - pooleddepthfirst"
  type                     = "Pooled"
  maximum_sessions_allowed = 50
  load_balancer_type       = "DepthFirst"
}
```

## Argument Reference

The following arguments are supported:

* `name` - (Required) The name of the Virtual Desktop Host Pool. Changing the name
    forces a new resource to be created.

* `resource_group_name` - (Required) The name of the resource group in which to
    create the Virtual Desktop Host Pool. Changing the resource group name forces
    a new resource to be created.

* `location` - (Required) The location/region where the Virtual Desktop Host Pool is
    located. Changing the location/region forces a new resource to be created.

* `type` - (Required) The type of the Virtual Desktop Host Pool. Valid options are
    `Personal` or `Pooled`. Changing the type forces a new resource to be created.

* `load_balancer_type` -  (Optional) `BreadthFirst` load balancing distributes new user sessions across all available session hosts in the host pool.
    `DepthFirst` load balancing distributes new user sessions to an available session host with the highest number of connections but has not reached its maximum session limit threshold.
    `Persistent` should be used if the host pool type is `Personal`

* `friendly_name` - (Optional) A friendly name for the Virtual Desktop Host Pool.

* `description` - (Optional) A description for the Virtual Desktop Host Pool.

* `validate_environment` -  (Optional) Allows you to test service changes before they are deployed to production. Defaults to `false`.  

* `start_vm_on_connect` -  (Optional) Enables or disables the Start VM on Connection Feature. Defaults to `false`.    

* `custom_rdp_properties` - (Optional) A valid custom RDP properties string for the Virtual Desktop Host Pool, available properties can be [found in this article](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files).

* `personal_desktop_assignment_type` - (Optional) `Automatic` assignment – The service will select an available host and assign it to an user.
    `Direct` Assignment – Admin selects a specific host to assign to an user.

~> **NOTE:** `personal_desktop_assignment_type` is required if the `type` of your Virtual Desktop Host Pool is `Personal`

* `maximum_sessions_allowed` (Optional) A valid integer value from 0 to 999999 for the maximum number of users that have concurrent sessions on a session host.
    Should only be set if the `type` of your Virtual Desktop Host Pool is `Pooled`.

* `preferred_app_group_type` (Optional) Option to specify the preferred Application Group type for the Virtual Desktop Host Pool.
    Valid options are `None`, `Desktop` or `RailApplications`. Default is `None`.

* `tags` - (Optional) A mapping of tags to assign to the resource.


## Attributes Reference

In addition to the Arguments listed above - the following Attributes are exported:

* `id` - The ID of the Virtual Desktop Host Pool.

## Timeouts

The `timeouts` block allows you to specify [timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) for certain actions:

* `create` - (Defaults to 60 minutes) Used when creating the Virtual Desktop Application Group.
* `update` - (Defaults to 60 minutes) Used when updating the Virtual Desktop Application Group.
* `read` - (Defaults to 5 minutes) Used when retrieving the Virtual Desktop Application Group.
* `delete` - (Defaults to 60 minutes) Used when deleting the Virtual Desktop Application Group.


## Import

Virtual Desktop Host Pools can be imported using the `resource id`, e.g.

```
terraform import azurerm_virtual_desktop_host_pool.example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myGroup1/providers/Microsoft.DesktopVirtualization/hostpools/myhostpool
```
