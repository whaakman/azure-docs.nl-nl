---
title: Een spoke-netwerk maken met Terraform in Azure
description: Meer informatie over het implementeren van twee knooppunt VNets die zijn verbonden met een hub in een hub-spoke-topologie
services: terraform
ms.service: azure
keywords: terraform, hub en spoke, netwerken, hybride netwerken, devops, virtuele machine, azure, VNet-peering, knooppunt, ster-
author: VaijanathB
manager: jeconnoc
ms.author: vaangadi
ms.topic: tutorial
ms.date: 03/01/2019
ms.openlocfilehash: 9cce809401a26eb2b45b11303afcd4818a1f950b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58009951"
---
# <a name="tutorial-create-a-spoke-virtual-network-with-terraform-in-azure"></a>Zelfstudie: Een knooppunt virtueel netwerk maken met Terraform in Azure

In deze zelfstudie implementeert u twee afzonderlijke spoke-netwerken om te demonstreren scheiding van workloads. De netwerken delen algemene resources met behulp van de virtuele hub-netwerk. Knooppunten kunnen worden gebruikt om werkbelastingen te isoleren in hun eigen VNets, afzonderlijk beheerd vanaf andere knooppunten. Elke workload kan meerdere lagen bevatten, met meerdere subnetten die zijn verbonden via Azure-load balancers.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * HCL (HashiCorp Language) gebruiken voor het implementeren van de Spoke-VNets in hub-spoke-topologie
> * Virtuele machines in de spoke-netwerken maken met behulp van Terraform
> * Peerings van virtuele netwerken met de netwerken hub tot stand brengen met behulp van Terraform

## <a name="prerequisites"></a>Vereisten

1. [Maken van een hub- en -spokenetwerktopologie hybride met Terraform in Azure](./terraform-hub-spoke-introduction.md).
1. [On-premises virtueel netwerk maken met Terraform in Azure](./terraform-hub-spoke-on-prem.md).
1. [Een virtuele hub-netwerk maken met Terraform in Azure](./terraform-hub-spoke-hub-network.md).
1. [Een virtueel netwerkapparaat hub maken met Terraform in Azure](./terraform-hub-spoke-hub-nva.md).

## <a name="create-the-directory-structure"></a>De directorystructuur maken

Twee spoke-scripts worden in deze sectie gemaakt. Elk script een spoke virtueel netwerk en een virtuele machine voor de werkbelasting gedefinieerd. Een gekoppeld virtueel netwerk van hub voor knooppunt wordt vervolgens gemaakt.

1. Blader naar de [Azure-portal](https://portal.azure.com).

1. Open [Azure Cloud Shell](/azure/cloud-shell/overview). Als u nog geen omgeving hebt geselecteerd, selecteert u **Bash** als uw omgeving.

    ![Cloud Shell-prompt](./media/terraform-common/azure-portal-cloud-shell-button-min.png)

1. Ga naar de map `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Ga naar de nieuwe map:

    ```bash
    cd hub-spoke
    ```

## <a name="declare-the-two-spoke-networks"></a>Declareer de twee spoke-netwerken

1. Open een nieuw bestand met de naam in Cloud Shell `spoke1.tf`.

    ```bash
    code spoke1.tf
    ```

1. Plak de volgende code in de editor:

    ```JSON
    locals {
      spoke1-location       = "CentralUS"
      spoke1-resource-group = "spoke1-vnet-rg"
      prefix-spoke1         = "spoke1"
    }

    resource "azurerm_resource_group" "spoke1-vnet-rg" {
      name     = "${local.spoke1-resource-group}"
      location = "${local.spoke1-location}"
    }

    resource "azurerm_virtual_network" "spoke1-vnet" {
      name                = "spoke1-vnet"
      location            = "${azurerm_resource_group.spoke1-vnet-rg.location}"
      resource_group_name = "${azurerm_resource_group.spoke1-vnet-rg.name}"
      address_space       = ["10.1.0.0/16"]

      tags {
        environment = "${local.prefix-spoke1 }"
      }
    }

    resource "azurerm_subnet" "spoke1-mgmt" {
      name                 = "mgmt"
      resource_group_name  = "${azurerm_resource_group.spoke1-vnet-rg.name}"
      virtual_network_name = "${azurerm_virtual_network.spoke1-vnet.name}"
      address_prefix       = "10.1.0.64/27"
    }

    resource "azurerm_subnet" "spoke1-workload" {
      name                 = "workload"
      resource_group_name  = "${azurerm_resource_group.spoke1-vnet-rg.name}"
      virtual_network_name = "${azurerm_virtual_network.spoke1-vnet.name}"
      address_prefix       = "10.1.1.0/24"
    }

    resource "azurerm_virtual_network_peering" "spoke1-hub-peer" {
      name                      = "spoke1-hub-peer"
      resource_group_name       = "${azurerm_resource_group.spoke1-vnet-rg.name}"
      virtual_network_name      = "${azurerm_virtual_network.spoke1-vnet.name}"
      remote_virtual_network_id = "${azurerm_virtual_network.hub-vnet.id}"

      allow_virtual_network_access = true
      allow_forwarded_traffic = true
      allow_gateway_transit   = false
      use_remote_gateways     = true
      depends_on = ["azurerm_virtual_network.spoke1-vnet", "azurerm_virtual_network.hub-vnet" , "azurerm_virtual_network_gateway.hub-vnet-gateway"]
    }

    resource "azurerm_network_interface" "spoke1-nic" {
      name                 = "${local.prefix-spoke1}-nic"
      location             = "${azurerm_resource_group.spoke1-vnet-rg.location}"
      resource_group_name  = "${azurerm_resource_group.spoke1-vnet-rg.name}"
      enable_ip_forwarding = true

      ip_configuration {
        name                          = "${local.prefix-spoke1}"
        subnet_id                     = "${azurerm_subnet.spoke1-mgmt.id}"
        private_ip_address_allocation = "Dynamic"
      }
    }

    resource "azurerm_virtual_machine" "spoke1-vm" {
      name                  = "${local.prefix-spoke1}-vm"
      location              = "${azurerm_resource_group.spoke1-vnet-rg.location}"
      resource_group_name   = "${azurerm_resource_group.spoke1-vnet-rg.name}"
      network_interface_ids = ["${azurerm_network_interface.spoke1-nic.id}"]
      vm_size               = "${var.vmsize}"

      storage_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "16.04-LTS"
        version   = "latest"
      }

      storage_os_disk {
        name              = "myosdisk1"
        caching           = "ReadWrite"
        create_option     = "FromImage"
        managed_disk_type = "Standard_LRS"
      }

      os_profile {
        computer_name  = "${local.prefix-spoke1}-vm"
        admin_username = "${var.username}"
        admin_password = "${var.password}"
      }

      os_profile_linux_config {
        disable_password_authentication = false
      }

      tags {
        environment = "${local.prefix-spoke1}"
      }
    }

    resource "azurerm_virtual_network_peering" "hub-spoke1-peer" {
      name                      = "hub-spoke1-peer"
      resource_group_name       = "${azurerm_resource_group.hub-vnet-rg.name}"
      virtual_network_name      = "${azurerm_virtual_network.hub-vnet.name}"
      remote_virtual_network_id = "${azurerm_virtual_network.spoke1-vnet.id}"
      allow_virtual_network_access = true
      allow_forwarded_traffic   = true
      allow_gateway_transit     = true
      use_remote_gateways       = false
      depends_on = ["azurerm_virtual_network.spoke1-vnet", "azurerm_virtual_network.hub-vnet", "azurerm_virtual_network_gateway.hub-vnet-gateway"]
    }
    ```

1. Sla het bestand op en sluit de editor af.

1. Maak een nieuw bestand met de naam `spoke2.tf`.

      ```bash
      code spoke2.tf
      ```
    
1. Plak de volgende code in de editor:
    
    ```JSON
    locals {
      spoke2-location       = "CentralUS"
      spoke2-resource-group = "spoke2-vnet-rg"
      prefix-spoke2         = "spoke2"
    }

    resource "azurerm_resource_group" "spoke2-vnet-rg" {
      name     = "${local.spoke2-resource-group}"
      location = "${local.spoke2-location}"
    }

    resource "azurerm_virtual_network" "spoke2-vnet" {
      name                = "${local.prefix-spoke2}-vnet"
      location            = "${azurerm_resource_group.spoke2-vnet-rg.location}"
      resource_group_name = "${azurerm_resource_group.spoke2-vnet-rg.name}"
      address_space       = ["10.2.0.0/16"]

      tags {
        environment = "${local.prefix-spoke2}"
      }
    }

    resource "azurerm_subnet" "spoke2-mgmt" {
      name                 = "mgmt"
      resource_group_name  = "${azurerm_resource_group.spoke2-vnet-rg.name}"
      virtual_network_name = "${azurerm_virtual_network.spoke2-vnet.name}"
      address_prefix       = "10.2.0.64/27"
    }

    resource "azurerm_subnet" "spoke2-workload" {
      name                 = "workload"
      resource_group_name  = "${azurerm_resource_group.spoke2-vnet-rg.name}"
      virtual_network_name = "${azurerm_virtual_network.spoke2-vnet.name}"
      address_prefix       = "10.2.1.0/24"
    }

    resource "azurerm_virtual_network_peering" "spoke2-hub-peer" {
      name                      = "${local.prefix-spoke2}-hub-peer"
      resource_group_name       = "${azurerm_resource_group.spoke2-vnet-rg.name}"
      virtual_network_name      = "${azurerm_virtual_network.spoke2-vnet.name}"
      remote_virtual_network_id = "${azurerm_virtual_network.hub-vnet.id}"

      allow_virtual_network_access = true
      allow_forwarded_traffic = true
      allow_gateway_transit   = false
      use_remote_gateways     = true
      depends_on = ["azurerm_virtual_network.spoke2-vnet", "azurerm_virtual_network.hub-vnet", "azurerm_virtual_network_gateway.hub-vnet-gateway"]
    }

    resource "azurerm_network_interface" "spoke2-nic" {
      name                 = "${local.prefix-spoke2}-nic"
      location             = "${azurerm_resource_group.spoke2-vnet-rg.location}"
      resource_group_name  = "${azurerm_resource_group.spoke2-vnet-rg.name}"
      enable_ip_forwarding = true

      ip_configuration {
        name                          = "${local.prefix-spoke2}"
        subnet_id                     = "${azurerm_subnet.spoke2-mgmt.id}"
        private_ip_address_allocation = "Dynamic"
      }

      tags {
        environment = "${local.prefix-spoke2}"
      }
    }

    resource "azurerm_virtual_machine" "spoke2-vm" {
      name                  = "${local.prefix-spoke2}-vm"
      location              = "${azurerm_resource_group.spoke2-vnet-rg.location}"
      resource_group_name   = "${azurerm_resource_group.spoke2-vnet-rg.name}"
      network_interface_ids = ["${azurerm_network_interface.spoke2-nic.id}"]
      vm_size               = "${var.vmsize}"

      storage_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "16.04-LTS"
        version   = "latest"
      }

      storage_os_disk {
        name              = "myosdisk1"
        caching           = "ReadWrite"
        create_option     = "FromImage"
        managed_disk_type = "Standard_LRS"
      }

      os_profile {
        computer_name  = "${local.prefix-spoke2}-vm"
        admin_username = "${var.username}"
        admin_password = "${var.password}"
      }

      os_profile_linux_config {
        disable_password_authentication = false
      }

      tags {
        environment = "${local.prefix-spoke2}"
      }
    }

    resource "azurerm_virtual_network_peering" "hub-spoke2-peer" {
      name                      = "hub-spoke2-peer"
      resource_group_name       = "${azurerm_resource_group.hub-vnet-rg.name}"
      virtual_network_name      = "${azurerm_virtual_network.hub-vnet.name}"
      remote_virtual_network_id = "${azurerm_virtual_network.spoke2-vnet.id}"
      allow_virtual_network_access = true
      allow_forwarded_traffic   = true
      allow_gateway_transit     = true
      use_remote_gateways       = false
      depends_on = ["azurerm_virtual_network.spoke2-vnet", "azurerm_virtual_network.hub-vnet", "azurerm_virtual_network_gateway.hub-vnet-gateway"]
    }
    ```
     
1. Sla het bestand op en sluit de editor af.
  
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Valideren van een hub en spoke-netwerk met Terraform in Azure](./terraform-hub-spoke-validation.md)