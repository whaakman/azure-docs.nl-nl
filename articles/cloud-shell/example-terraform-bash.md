---
title: Implementeren met Terraform in Bash in Azure Cloudshell | Microsoft Docs
description: Implementeren met Terraform in Bash in Azure Cloudshell
services: Azure
documentationcenter: ''
author: tomarcher
manager: routlaw
tags: azure-cloud-shell
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: tarcher
ms.openlocfilehash: 8512c04cb0efc698ca688724c3806291bb02d200
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46947636"
---
# <a name="deploy-with-terraform-from-bash-in-azure-cloud-shell"></a>Implementeren met Terraform in Bash in Azure Cloudshell
Dit artikel begeleidt u bij het maken van een resourcegroep met de [Terraform AzureRM provider](https://www.terraform.io/docs/providers/azurerm/index.html). 

[Hashicorp Terraform](https://www.terraform.io/) is een open-source-hulpprogramma dat codifies API's in declaratieve configuratiebestanden die worden kunnen gedeeld onder teamleden om te worden bewerkt, gecontroleerd en is samengesteld. De provider van Microsoft-AzureRM wordt gebruikt om te communiceren met resources die worden ondersteund door Azure Resource Manager via de APIs AzureRM. 

## <a name="automatic-authentication"></a>Automatische verificatie
Terraform is standaard geïnstalleerd in Bash in Cloud Shell. Cloud Shell wordt bovendien automatisch geverifieerd uw standaardabonnement van Azure CLI voor het implementeren van resources via de Terraform Azure-modules.

Terraform maakt gebruik van de standaard-Azure CLI-abonnement dat is ingesteld. Om bij te werken abonnementen standaard, voert u de volgende uit:

```azurecli-interactive
az account set --subscription mySubscriptionName
```

## <a name="walkthrough"></a>Walkthrough
### <a name="launch-bash-in-cloud-shell"></a>Bash in Cloudshell starten
1. Open Cloud Shell vanaf de locatie van uw voorkeur
2. Controleer of dat het gewenste abonnement is ingesteld

```azurecli-interactive
az account show
```

### <a name="create-a-terraform-template"></a>Een Terraform-sjabloon maken
Maak een nieuwe Terraform-sjabloon met de naam main.tf met uw favoriete teksteditor.

```
vim main.tf
```

De volgende code, kopiëren en plakken in Cloud Shell.

```
resource "azurerm_resource_group" "myterraformgroup" {
    name = "myRgName"
    location = "West US"
}
```

Sla het bestand en uw teksteditor af te sluiten.

### <a name="terraform-init"></a>Terraform init
Begin door uit te voeren `terraform init`.

```
justin@Azure:~$ terraform init

Initializing provider plugins...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.azurerm: version = "~> 0.2"

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

De [terraform init opdracht](https://www.terraform.io/docs/commands/init.html) wordt gebruikt voor het initialiseren van een werkmap met Terraform-configuratiebestanden. De `terraform init` opdracht is de eerste opdracht die moet worden uitgevoerd na het schrijven van een nieuwe Terraform-configuratie of een bestaande naam uit versiebeheer klonen. Het is veilig om uit te voeren met deze opdracht meerdere keren.

### <a name="terraform-plan"></a>Terraform Plan
Voorbeeld van de resources zijn gemaakt door de sjabloon Terraform met `terraform plan`.

```
justin@Azure:~$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_resource_group.demo
      id:       <computed>
      location: "westus"
      name:     "myRGName"
      tags.%:   <computed>


Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

De opdracht [terraform plan](https://www.terraform.io/docs/commands/plan.html) wordt gebruikt om een uitvoeringsplan te maken. Terraform vernieuwen wordt uitgevoerd, tenzij expliciet worden uitgeschakeld en vervolgens bepaalt welke acties nodig zijn voor het bereiken van de gewenste status die is opgegeven in de configuratiebestanden. Het plan kan worden opgeslagen met de-out, en vervolgens wordt er op terraform toepassen om te controleren of alleen de vooraf geplande acties worden uitgevoerd.

### <a name="terraform-apply"></a>Terraform Apply
De Azure-resources inrichten `terraform apply`.

```
justin@Azure:~$ terraform apply
azurerm_resource_group.demo: Creating...
  location: "" => "westus"
  name:     "" => "myRGName"
  tags.%:   "" => "<computed>"
azurerm_resource_group.demo: Creation complete after 0s (ID: /subscriptions/mySubIDmysub/resourceGroups/myRGName)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

De [terraform opdracht toepassen](https://www.terraform.io/docs/commands/apply.html) wordt gebruikt voor het toepassen van de wijzigingen die zijn vereist voor het bereiken van de gewenste status van de configuratie.

### <a name="verify-deployment-with-azure-cli"></a>Controleer of de implementatie met Azure CLI
Voer `az group show -n myRgName` om te controleren of de resource is voltooid, wordt ingericht.

```azcliinteractive
az group show -n myRgName
```

### <a name="clean-up-with-terraform-destroy"></a>Opschonen met terraform vernietigen
Opschonen van de resourcegroep gemaakt met de [Terraform opdracht vernietigen](https://www.terraform.io/docs/commands/destroy.html) voor het opschonen van de infrastructuur met Terraform gemaakt.

```
justin@Azure:~$ terraform destroy
azurerm_resource_group.demo: Refreshing state... (ID: /subscriptions/mySubID/resourceGroups/myRGName)

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  - azurerm_resource_group.demo


Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

azurerm_resource_group.demo: Destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName)
azurerm_resource_group.demo: Still destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName, 10s elapsed)
azurerm_resource_group.demo: Still destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName, 20s elapsed)
azurerm_resource_group.demo: Still destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName, 30s elapsed)
azurerm_resource_group.demo: Still destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName, 40s elapsed)
azurerm_resource_group.demo: Destruction complete after 45s

Destroy complete! Resources: 1 destroyed.
```

U hebt een Azure-resource via Terraform gemaakt. Ga naar de volgende stappen uit om te leren over Cloud Shell.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de Terraform Azure-provider](https://www.terraform.io/docs/providers/azurerm/#)<br>
[Bash in Cloud Shell-snelstartgids](quickstart.md)
