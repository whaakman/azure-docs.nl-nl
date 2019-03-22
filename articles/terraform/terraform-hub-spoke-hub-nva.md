---
title: Een virtueel netwerkapparaat hub maken met Terraform in Azure
description: Zelfstudie voor het maken van de Hub VNet die als een algemene verbindingspunt tussen de netwerken fungeert worden geïmplementeerd
services: terraform
ms.service: azure
keywords: terraform, hub en spoke, netwerken, hybride netwerken, devops, virtuele machine, azure, VNet-peering, ster-, hub.
author: VaijanathB
manager: jeconnoc
ms.author: vaangadi
ms.topic: tutorial
ms.date: 03/01/2019
ms.openlocfilehash: 4155a67f70ccc238c6046c07dded7f0214689617
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57993514"
---
# <a name="tutorial-create-a-hub-virtual-network-appliance-with-terraform-in-azure"></a>Zelfstudie: Een virtueel netwerkapparaat hub maken met Terraform in Azure

Een **VPN-apparaat** is een apparaat dat externe verbinding met een on-premises netwerk biedt. Het VPN-apparaat mogelijk op een apparaat of een softwareoplossing. Een voorbeeld van een softwareoplossing is Routing and Remote Access Service (RRAS) in Windows Server 2012. Zie voor meer informatie over VPN-apparaten, [over VPN-apparaten voor Site-naar-Site VPN-gatewayverbindingen](/azure/vpn-gateway/vpn-gateway-about-vpn-devices).

Azure ondersteunt een breed scala virtuele netwerkapparaten te selecteren. Voor deze zelfstudie wordt een Ubuntu-installatiekopie gebruikt. Zie voor meer informatie over de brede variëteit aan oplossingen voor apparaten worden ondersteund in Azure, de [netwerkapparaten startpagina](https://azure.microsoft.com/solutions/network-appliances/).

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * HCL (HashiCorp Language) gebruiken voor het implementeren van de Hub VNet in hub-spoke-topologie
> * Gebruik Terraform om Hub netwerk virtuele Machine die als een apparaat fungeert te maken
> * Gebruik Terraform om in te schakelen van routes met behulp van de CustomScript-extensies
> * Hub en Spoke-gateway-routetabellen maken met behulp van Terraform

## <a name="prerequisites"></a>Vereisten

1. [Maken van een hub- en -spokenetwerktopologie hybride met Terraform in Azure](./terraform-hub-spoke-introduction.md).
1. [On-premises virtueel netwerk maken met Terraform in Azure](./terraform-hub-spoke-on-prem.md).
1. [Een virtuele hub-netwerk maken met Terraform in Azure](./terraform-hub-spoke-hub-network.md).

## <a name="create-the-directory-structure"></a>De directorystructuur maken

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

## <a name="declare-the-hub-network-appliance"></a>Het netwerkapparaat hub declareren

Het Terraform-configuratiebestand dat On-Premises virtueel netwerk verklaart maken.

1. Maak een nieuw bestand met de naam in Cloud Shell `hub-nva.tf`.

    ```bash
    code hub-nva.tf
    ```

1. Plak de volgende code in de editor:
    
    ```JSON
    locals {
      prefix-hub-nva         = "hub-nva"
      hub-nva-location       = "CentralUS"
      hub-nva-resource-group = "hub-nva-rg"
    }

    resource "azurerm_resource_group" "hub-nva-rg" {
      name     = "${local.prefix-hub-nva}-rg"
      location = "${local.hub-nva-location}"

      tags {
        environment = "${local.prefix-hub-nva}"
      }
    }

    resource "azurerm_network_interface" "hub-nva-nic" {
      name                 = "${local.prefix-hub-nva}-nic"
      location             = "${azurerm_resource_group.hub-nva-rg.location}"
      resource_group_name  = "${azurerm_resource_group.hub-nva-rg.name}"
      enable_ip_forwarding = true

      ip_configuration {
        name                          = "${local.prefix-hub-nva}"
        subnet_id                     = "${azurerm_subnet.hub-dmz.id}"
        private_ip_address_allocation = "Static"
        private_ip_address            = "10.0.0.36"
      }

      tags {
        environment = "${local.prefix-hub-nva}"
      }
    }

    resource "azurerm_virtual_machine" "hub-nva-vm" {
      name                  = "${local.prefix-hub-nva}-vm"
      location              = "${azurerm_resource_group.hub-nva-rg.location}"
      resource_group_name   = "${azurerm_resource_group.hub-nva-rg.name}"
      network_interface_ids = ["${azurerm_network_interface.hub-nva-nic.id}"]
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
        computer_name  = "${local.prefix-hub-nva}-vm"
        admin_username = "${var.username}"
        admin_password = "${var.password}"
      }

      os_profile_linux_config {
        disable_password_authentication = false
      }

      tags {
        environment = "${local.prefix-hub-nva}"
      }
    }

    resource "azurerm_virtual_machine_extension" "enable-routes" {
      name                 = "enable-iptables-routes"
      location             = "${azurerm_resource_group.hub-nva-rg.location}"
      resource_group_name  = "${azurerm_resource_group.hub-nva-rg.name}"
      virtual_machine_name = "${azurerm_virtual_machine.hub-nva-vm.name}"
      publisher            = "Microsoft.Azure.Extensions"
      type                 = "CustomScript"
      type_handler_version = "2.0"

      settings = <<SETTINGS
        {
            "fileUris": [
            "https://raw.githubusercontent.com/mspnp/reference-architectures/master/scripts/linux/enable-ip-forwarding.sh"
            ],
            "commandToExecute": "bash enable-ip-forwarding.sh"
        }
    SETTINGS

      tags {
        environment = "${local.prefix-hub-nva}"
      }
    }

    resource "azurerm_route_table" "hub-gateway-rt" {
      name                          = "hub-gateway-rt"
      location                      = "${azurerm_resource_group.hub-nva-rg.location}"
      resource_group_name           = "${azurerm_resource_group.hub-nva-rg.name}"
      disable_bgp_route_propagation = false

      route {
        name           = "toHub"
        address_prefix = "10.0.0.0/16"
        next_hop_type  = "VnetLocal"
      }

      route {
        name                   = "toSpoke1"
        address_prefix         = "10.1.0.0/16"
        next_hop_type          = "VirtualAppliance"
        next_hop_in_ip_address = "10.0.0.36"
      }

      route {
        name                   = "toSpoke2"
        address_prefix         = "10.2.0.0/16"
        next_hop_type          = "VirtualAppliance"
        next_hop_in_ip_address = "10.0.0.36"
      }

      tags {
        environment = "${local.prefix-hub-nva}"
      }
    }

    resource "azurerm_subnet_route_table_association" "hub-gateway-rt-hub-vnet-gateway-subnet" {
      subnet_id      = "${azurerm_subnet.hub-gateway-subnet.id}"
      route_table_id = "${azurerm_route_table.hub-gateway-rt.id}"
      depends_on = ["azurerm_subnet.hub-gateway-subnet"]
    }

    resource "azurerm_route_table" "spoke1-rt" {
      name                          = "spoke1-rt"
      location                      = "${azurerm_resource_group.hub-nva-rg.location}"
      resource_group_name           = "${azurerm_resource_group.hub-nva-rg.name}"
      disable_bgp_route_propagation = false

      route {
        name                   = "toSpoke2"
        address_prefix         = "10.2.0.0/16"
        next_hop_type          = "VirtualAppliance"
        next_hop_in_ip_address = "10.0.0.36"
      }

      route {
        name           = "default"
        address_prefix = "0.0.0.0/0"
        next_hop_type  = "vnetlocal"
      }

      tags {
        environment = "${local.prefix-hub-nva}"
      }
    }

    resource "azurerm_subnet_route_table_association" "spoke1-rt-spoke1-vnet-mgmt" {
      subnet_id      = "${azurerm_subnet.spoke1-mgmt.id}"
      route_table_id = "${azurerm_route_table.spoke1-rt.id}"
      depends_on = ["azurerm_subnet.spoke1-mgmt"]
    }

    resource "azurerm_subnet_route_table_association" "spoke1-rt-spoke1-vnet-workload" {
      subnet_id      = "${azurerm_subnet.spoke1-workload.id}"
      route_table_id = "${azurerm_route_table.spoke1-rt.id}"
      depends_on = ["azurerm_subnet.spoke1-workload"]
    }

    resource "azurerm_route_table" "spoke2-rt" {
      name                          = "spoke2-rt"
      location                      = "${azurerm_resource_group.hub-nva-rg.location}"
      resource_group_name           = "${azurerm_resource_group.hub-nva-rg.name}"
      disable_bgp_route_propagation = false

      route {
        name                   = "toSpoke1"
        address_prefix         = "10.1.0.0/16"
        next_hop_in_ip_address = "10.0.0.36"
        next_hop_type          = "VirtualAppliance"
      }

      route {
        name           = "default"
        address_prefix = "0.0.0.0/0"
        next_hop_type  = "vnetlocal"
      }

      tags {
        environment = "${local.prefix-hub-nva}"
      }
    }

    resource "azurerm_subnet_route_table_association" "spoke2-rt-spoke2-vnet-mgmt" {
      subnet_id      = "${azurerm_subnet.spoke2-mgmt.id}"
      route_table_id = "${azurerm_route_table.spoke2-rt.id}"
      depends_on = ["azurerm_subnet.spoke2-mgmt"]
    }

    resource "azurerm_subnet_route_table_association" "spoke2-rt-spoke2-vnet-workload" {
      subnet_id      = "${azurerm_subnet.spoke2-workload.id}"
      route_table_id = "${azurerm_route_table.spoke2-rt.id}"
      depends_on = ["azurerm_subnet.spoke2-workload"]
    }

    ```

1. Sla het bestand op en sluit de editor af.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een knooppunt virtuele netwerken maken met Terraform in Azure](./terraform-hub-spoke-spoke-network.md)