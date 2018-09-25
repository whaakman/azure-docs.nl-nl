---
title: Terraform installeren en configureren voor gebruik met Azure | Microsoft Docs
description: Meer informatie over het installeren en configureren van Terraform voor het maken van Azure-resources
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/19/2018
ms.author: echuvyrov
ms.openlocfilehash: 0943bd1bffb3df7beda97ea0619f1aced4ca3a41
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946779"
---
# <a name="install-and-configure-terraform-to-provision-vms-and-other-infrastructure-into-azure"></a>Terraform voor het inrichten van VM's en andere infrastructuur in Azure installeren en configureren
 
Terraform biedt een eenvoudige manier om te definiëren, bekijken en cloud-infrastructuur implementeren met behulp van een [eenvoudige templating taal](https://www.terraform.io/docs/configuration/syntax.html). Dit artikel beschrijft de benodigde stappen voor het gebruik van Terraform voor inrichting van resources in Azure.

Voor meer informatie over het gebruik van Terraform met Azure, gaat u naar de [Terraform Hub](/azure/terraform).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Terraform is standaard geïnstalleerd in de [Cloud Shell](/azure/terraform/terraform-cloud-shell). Als u ervoor kiest Terraform lokaal installeren, voert u de volgende stap, anders blijven [instellen van toegang tot Azure Terraform](#set-up-terraform-access-to-azure).

## <a name="install-terraform"></a>Terraform installeren

Voor het installeren van Terraform, [downloaden](https://www.terraform.io/downloads.html) het juiste pakket voor uw besturingssysteem in een afzonderlijke directory installeren. De download bevat één uitvoerbaar bestand, waarvoor u ook een globale-pad moet definiëren. Voor instructies over het instellen van het pad in Linux en Mac, gaat u naar [deze webpagina](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux). Voor instructies over het instellen van het pad op Windows, gaat u naar [deze webpagina](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).

Controleer de configuratie van uw pad met de `terraform` opdracht. Een lijst met beschikbare Terraform opties wordt weergegeven, zoals in de volgende voorbeelduitvoer:

```bash
azureuser@Azure:~$ terraform
Usage: terraform [--version] [--help] <command> [args]
```

## <a name="set-up-terraform-access-to-azure"></a>Terraform toegang tot Azure instellen

Om te schakelen Terraform resources te kunnen inrichten in Azure, maak een [service-principal voor Azure AD](/cli/azure/create-an-azure-service-principal-azure-cli). De service-principal verleent de scripts Terraform inrichting van resources in uw Azure-abonnement.

Als u meerdere Azure-abonnementen hebt, eerst uw account met query [az account show](/cli/azure/account#az-account-show) voor een lijst van abonnement-ID en tenant-id-waarden:

```azurecli-interactive
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

Voor het gebruik van een geselecteerde abonnement, stel het abonnement voor deze sessie met [az account set](/cli/azure/account#az-account-set). Stel de `SUBSCRIPTION_ID` omgevingsvariabele voor het opslaan van de waarde van de geretourneerde `id` veld uit het abonnement dat u wilt gebruiken:

```azurecli-interactive
az account set --subscription="${SUBSCRIPTION_ID}"
```

U kunt nu een service-principal voor gebruik met Terraform maken. Gebruik [az ad sp create-for-rbac]/cli/azure/ad/sp#az-ad-sp-create-for-rbac), en stel de *bereik* aan uw abonnement als volgt te werk:

```azurecli-interactive
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

Uw `appId`, `password`, `sp_name`, en `tenant` worden geretourneerd. Noteer de `appId` en `password`.

## <a name="configure-terraform-environment-variables"></a>Terraform-omgevingsvariabelen configureren

Voor het configureren van Terraform voor het gebruik van uw service-principal voor Azure AD, stelt u de volgende omgevingsvariabelen, die vervolgens worden gebruikt door de [Azure Terraform-modules](https://registry.terraform.io/modules/Azure). U kunt ook de omgeving instellen als met een Azure-cloud dan Azure openbare werkt.

- `ARM_SUBSCRIPTION_ID`
- `ARM_CLIENT_ID`
- `ARM_CLIENT_SECRET`
- `ARM_TENANT_ID`
- `ARM_ENVIRONMENT`

Het volgende voorbeeldscript shell kunt u deze variabelen worden ingesteld:

```bash
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id

# Not needed for public, required for usgovernment, german, china
export ARM_ENVIRONMENT=public
```

## <a name="run-a-sample-script"></a>Een voorbeeld van een script uitvoeren

Maak een bestand `test.tf` in een lege map en plak in het volgende script.

```tf
provider "azurerm" {
}
resource "azurerm_resource_group" "rg" {
        name = "testResourceGroup"
        location = "westus"
}
```

Sla het bestand en deze vervolgens initialiseren de Terraform-implementatie. Deze stap downloadt de Azure-modules vereist voor het maken van een Azure-resourcegroep.

```bash
terraform init
```

De uitvoer lijkt op die in het volgende voorbeeld:

```bash
* provider.azurerm: version = "~> 0.3"

Terraform has been successfully initialized!
```

U kunt bekijken welke acties moeten worden voltooid door het script Terraform met `terraform plan`. Wanneer u klaar bent om de resourcegroep te maken, als volgt uw plan Terraform toepassen:

```bash
terraform apply
```

De uitvoer lijkt op die in het volgende voorbeeld:

```bash
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

In dit artikel hebt u Terraform geïnstalleerd of de Cloud Shell gebruikt voor het Azure-referenties configureren en beginnen met het maken van resources in uw Azure-abonnement. Zie het volgende artikel voor informatie over het maken van een volledig Terraform-implementatie in Azure:

> [!div class="nextstepaction"]
> [Een Azure-VM maken met Terraform](terraform-create-complete-vm.md)
