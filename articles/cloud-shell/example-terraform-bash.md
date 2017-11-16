---
title: Met Terraform met Bash in de Azure-Cloud-Shell implementeren | Microsoft Docs
description: Azure-resources met Terraform in Bash implementeren
services: Azure
documentationcenter: 
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
ms.openlocfilehash: c75b5d521dc3eacaf5c5921c35442b1afeb4fa13
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="terraform-and-bash-in-cloud-shell"></a>Terraform en Bash in de Cloud-Shell
Dit artikel begeleidt u bij het maken van een resourcegroep met de [Terraform AzureRM provider](https://www.terraform.io/docs/providers/azurerm/index.html). 

[Hashicorp Terraform](https://www.terraform.io/) is een open-source hulpprogramma dat API's in declaratieve configuratiebestanden die worden kunnen gedeeld onder teamleden kunnen worden bewerkt, wordt gecontroleerd en samengestelde codifies. De Microsoft-AzureRM-provider wordt gebruikt om te communiceren met de resources die worden ondersteund door Azure Resource Manager via de AzureRM APIs. 

## <a name="automatic-authentication"></a>Automatische verificatie
Terraform wordt standaard geïnstalleerd in Bash in de Cloud-Shell. Bovendien verifieert Cloud Shell automatisch voor uw abonnement van de Azure CLI 2.0 standaard voor het implementeren van resources via de modules Terraform Azure.

Terraform maakt gebruik van het standaard Azure CLI 2.0-abonnement dat is ingesteld. Voor het bijwerken van abonnementen standaard uitvoeren:

```azurecli-interactive
az account set --subscription mySubscriptionName
```

## <a name="walkthrough"></a>Walkthrough
### <a name="launch-bash-in-cloud-shell"></a>Bash in de Cloud-Shell starten
1. Cloud-Shell vanaf de locatie van uw voorkeur starten
2. Controleer of dat het gewenste abonnement is ingesteld

```azurecli-interactive
az account show
```

### <a name="create-a-terraform-template"></a>Een sjabloon Terraform maken
Maak een nieuwe Terraform sjabloon met de naam main.tf met uw voorkeur teksteditor.

```
vim main.tf
```

De volgende code, kopiëren en plakken in de Cloud-Shell.

```
resource "azurerm_resource_group" "myterraformgroup" {
    name = "myRgName"
    location = "West US"
}
```

Sla het bestand en uw teksteditor af te sluiten.

### <a name="terraform-init"></a>Terraform init
Begin door te voeren `terraform init`.

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

De [terraform init opdracht](https://www.terraform.io/docs/commands/init.html) wordt gebruikt voor het initialiseren van een werkmap met Terraform configuratiebestanden. De `terraform init` opdracht is de eerste opdracht die moet worden uitgevoerd na het schrijven van een nieuwe Terraform-configuratie of het klonen van een bestaande van versiebeheer. Het is veilig worden meerdere keren voor deze opdracht uitvoeren.

### <a name="terraform-plan"></a>Terraform plannen
Voorbeeld bekijken van de bronnen worden gemaakt door de sjabloon Terraform met `terraform plan`.

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

De [terraform plan opdracht](https://www.terraform.io/docs/commands/plan.html) gebruikt voor het maken van een plan kan worden uitgevoerd. Terraform voert een vernieuwing, tenzij expliciet worden uitgeschakeld en wordt vervolgens bepaald welke acties voor de gewenste status die is opgegeven in de configuratiebestanden die nodig zijn. Het plan kan worden opgeslagen met de-out, en vervolgens voor terraform toepassen om te controleren of alleen de vooraf geplande acties worden uitgevoerd.

### <a name="terraform-apply"></a>Terraform toepassen
Met de Azure-resources inrichten `terraform apply`.

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

### <a name="verify-deployment-with-azure-cli-20"></a>Controleer of de implementatie met Azure CLI 2.0
Voer `az group show -n myRgName` om te controleren of de resource inrichten is voltooid.

```azcliinteractive
az group show -n myRgName
```

### <a name="clean-up-with-terraform-destroy"></a>Opschonen met terraform vernietigen
Opruimen van de resourcegroep gemaakt met de [Terraform vernietigen opdracht](https://www.terraform.io/docs/commands/destroy.html) om op te schonen infrastructuur Terraform gemaakt.

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

U hebt een Azure-resource via Terraform gemaakt. Ga naar de volgende stappen uit om door te gaan leren over Cloud Shell.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de Terraform Azure provider](https://www.terraform.io/docs/providers/azurerm/#)<br>
[In de Cloud Shell snel Bash](quickstart.md)