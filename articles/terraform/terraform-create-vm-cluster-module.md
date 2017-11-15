---
title: Terraform-modules gebruiken voor het maken van een VM-cluster in Azure
description: Informatie over het gebruik van Terraform modules maken van een Windows-cluster voor virtuele machine in Azure
keywords: terraform, devops, virtuele machine, netwerk, modules
author: rloutlaw
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure
ms.date: 10/19/2017
ms.custom: devops
ms.author: routlaw
ms.openlocfilehash: 23d79fa4a1794a6dea69e6ae24da714babf54e62
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-vm-cluster-with-terraform-using-the-module-registry"></a>Maak een VM-cluster met Terraform met het register van de Module

Dit artikel begeleidt u bij een klein VM-cluster maken met de Terraform [Azure compute module](https://registry.terraform.io/modules/Azure/compute/azurerm/1.0.2). In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
> * Verificatie met Azure instellen
> * De sjabloon Terraform maken
> * De wijzigingen die u van plan te visualiseren
> * De configuratie voor het maken van de VM-cluster toe te passen

Zie voor meer informatie over Terraform de [Terraform documentatie](https://www.terraform.io/docs/index.html).

## <a name="set-up-authentication-with-azure"></a>Verificatie met Azure instellen

> [!TIP]
> Als u [Terraform omgevingsvariabelen gebruiken](/azure/virtual-machines/linux/terraform-install-configure#set-environment-variables) of het uitvoeren van deze zelfstudie in de [Azure Cloud Shell](/azure/cloud-shell/overview), deze stap overslaan.

 Bekijk [Terraform installeren en configureren van toegang tot Azure](/azure/virtual-machines/linux/terraform-install-configure) een Azure-service-principal maken. Gebruik deze service-principal voor het vullen van een nieuw bestand `azureProviderAndCreds.tf` in een lege map met de volgende code:

```tf
variable subscription_id {}
variable tenant_id {}
variable client_id {}
variable client_secret {}

provider "azurerm" {
    subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    tenant_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_secret = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

## <a name="create-the-template"></a>De sjabloon maken

Maak een nieuwe Terraform sjabloon met de naam `main.tf` met de volgende code:

```tf
module mycompute {
    source = "Azure/compute/azurerm"
    resource_group_name = "myResourceGroup"
    location = "East US 2"
    admin_password = "ComplxP@assw0rd!"
    vm_os_simple = "WindowsServer"
    remote_port = "3389"
    nb_instances = 2
    vnet_subnet_id = "${module.network.vnet_subnets[0]}"
}

module "network" {
    source = "Azure/network/azurerm"
    location = "East US 2"
    resource_group_name = "myResourceGroup"
}

output "vm_public_name" {
    value = "${module.mycompute.public_ip_dns_name}"
}

output = "vm_public_ip" {
    value = "${module.mycompute.public_ip_address}"
}

output "vm_private_ips" {
    value = "${module.mycompute.network_interface_private_ip}"
}
```

Voer `terraform init` in uw directory configuratie. Met een versie Terraform van ten minste 0.10.6 ziet u de volgende uitvoer:

![Terraform Init](media/terraformInitWithModules.png)

## <a name="visualize-the-changes-with-plan"></a>De wijzigingen die u van plan te visualiseren

Voer `terraform plan` voorbeeld van de infrastructuur van de virtuele machine gemaakt door de sjabloon.

![Terraform plannen](media/terraform-create-vm-cluster-with-infrastructure/terraform-plan.png)


## <a name="create-the-virtual-machines-with-apply"></a>De virtuele machines maken met toepassen

Voer `terraform apply` voor het inrichten van de virtuele machines in Azure.

![Terraform toepassen](media/terraform-create-vm-cluster-with-infrastructure/terraform-apply.png)

## <a name="next-steps"></a>Volgende stappen

- Bladeren in de lijst van [Azure Terraform modules](https://registry.terraform.io/modules/Azure)
- Maak een [virtuele-machineschaalset met Terraform](terraform-create-vm-scaleset-network-disks-hcl.md)