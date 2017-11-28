---
title: Installeren en configureren van Terraform voor het inrichten van virtuele machines en andere infrastructuur in Azure | Microsoft Docs
description: Meer informatie over het installeren en configureren van Terraform voor het maken van Azure-resources
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/23/2017
ms.author: echuvyrov
ms.openlocfilehash: c156776103a466af8923ba7249d96835ff339268
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="install-and-configure-terraform-to-provision-vms-and-other-infrastructure-into-azure"></a>Installeren en configureren van Terraform voor het inrichten van virtuele machines en andere infrastructuur in Azure
 
Terraform biedt een eenvoudige manier om te definiëren, weergeven en cloudinfrastructuur implementeren met behulp van een [eenvoudige templating taal](https://www.terraform.io/docs/configuration/syntax.html). Dit artikel worden de benodigde stappen voor het gebruik van Terraform aan inrichten bronnen in Azure. 

> [!TIP]
Voor meer informatie over het gebruik van Terraform met Azure, gaat u naar de [Terraform Hub](/azure/terraform). Terraform is standaard geïnstalleerd in de [Cloud Shell](/azure/terraform/terraform-cloud-shell). Met behulp van Cloud-Shell, kunt u de installatie-instellingen delen van dit document overslaan.

## <a name="install-terraform"></a>Terraform installeren

Voor het installeren van Terraform, [downloaden](https://www.terraform.io/downloads.html) het pakket dat geschikt is voor uw besturingssysteem in een afzonderlijke map installeren. De download bevat één uitvoerbaar bestand, waarvoor moet u ook een globale pad definiëren. Voor instructies over het instellen van het pad in Linux en Mac, gaat u naar [deze webpagina](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux). Voor instructies over het instellen van het pad in Windows, gaat u naar [deze webpagina](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows). 

Controleer of de pad-configuratie met de `terraform` opdracht. U ziet een lijst met beschikbare Terraform opties als uitvoer:

```bash
azureuser@Azure:~$ terraform
Usage: terraform [--version] [--help] <command> [args]
```

## <a name="set-up-terraform-access-to-azure"></a>Toegang tot Azure Terraform instellen

Configureer [een Azure AD-service-principal](/cli/azure/create-an-azure-service-principal-azure-cli) Terraform inrichten bronnen inschakelen in Azure. De service-principal verleent Terraform scripts met referenties voor het inrichten van resources in uw Azure-abonnement.

Er zijn verschillende manieren een Azure AD-toepassing en een Azure AD-service principal te maken. Het snelst en eenvoudigst is vandaag de dag manier voor het gebruik van Azure CLI 2.0, die [u kunt downloaden en installeren op Windows, Linux of een Mac](/cli/azure/install-azure-cli).

Meld u bij het beheren van uw Azure-abonnement door de volgende opdracht:

   `az login`

Als u meerdere Azure-abonnementen hebt, de bijbehorende gegevens worden geretourneerd door de `az login` opdracht. Stel de `SUBSCRIPTION_ID` omgevingsvariabele voor de waarde van de geretourneerde `id` veld van het abonnement dat u wilt gebruiken. 

Het abonnement dat u wilt gebruiken voor deze sessie ingesteld.

```azurecli-interactive
az account set --subscription="${SUBSCRIPTION_ID}"
```

Query uitvoeren op het account voor het abonnement-ID en het tenant-ID-waarden.

```azurecli-interactive
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

Vervolgens maakt u afzonderlijke referenties voor Terraform.

```azurecli-interactive
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

De appId, wachtwoord, sp_name en tenant worden geretourneerd. Noteer de toepassings-id en het wachtwoord.

Uw referenties testen door een nieuwe shell openen en voer de volgende opdracht, met de geretourneerde waarden voor sp_name, wachtwoord en tenant:

```azurecli-interactive
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

## <a name="configure-terraform-environment-variables"></a>Omgevingsvariabelen Terraform configureren

Configureer Terraform voor het gebruik van de tenant-ID, de abonnements-ID, de client-ID en de client geheim van de service-principal bij het maken van Azure-resources. Instellen van de volgende omgevingsvariabelen die gebruikt worden automatisch door de [Azure Terraform modules](https://registry.terraform.io/modules/Azure).

- ARM_SUBSCRIPTION_ID
- ARM_CLIENT_ID
- ARM_CLIENT_SECRET
- ARM_TENANT_ID

Dit voorbeeldscript shell kunt u deze variabelen worden ingesteld:

```bash
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

## <a name="run-a-sample-script"></a>Een voorbeeld van een script uitvoeren

Maak een bestand `test.tf` in een lege map en plakken in het volgende script. 

```tf
provider "azurerm" {
}
resource "azurerm_resource_group" "rg" {
        name = "testResourceGroup"
        location = "westus"
}
```

Sla het bestand en voer vervolgens `terraform init`. Deze opdracht downloadt u de Azure-modules vereist voor het maken van een Azure-resourcegroep. Ziet u de volgende uitvoer:

```
* provider.azurerm: version = "~> 0.3"

Terraform has been successfully initialized!
```

Voorbeeld van het script dat `terraform plan`, en maak vervolgens de `testResouceGroup` resourcegroep met `terraform apply`:

```
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_resource_group.rg
      id:       <computed>
      location: "westus"
      name:     "testResourceGroup"
      tags.%:   <computed>

azurerm_resource_group.rg: Creating...
  location: "" => "westus"
  name:     "" => "testResourceGroup"
  tags.%:   "" => "<computed>"
azurerm_resource_group.rg: Creation complete after 1s
```

## <a name="next-steps"></a>Volgende stappen

U hebt geïnstalleerd Terraform en Azure-referenties geconfigureerd zodat u kunt starten infrastructuur implementeren in uw Azure-abonnement. U kunt vervolgens uw installatie getest door het maken van een leeg Azure-resourcegroep.

> [!div class="nextstepaction"]
> [Een virtuele machine in Azure maken met Terraform](terraform-create-complete-vm.md)

