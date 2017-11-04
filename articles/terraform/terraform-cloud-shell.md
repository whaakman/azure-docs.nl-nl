---
title: Terraform gebruiken met Azure-Cloud-Shell
description: Terraform met Azure Cloud Shell gebruiken om verificatie en sjabloonconfiguratie te vereenvoudigen.
keywords: terraform, devops, schalen ingesteld, virtuele machine, netwerk, opslag, modules
ms.service: virtual-machines-linux
author: dcaro
ms.author: dcaro
ms.date: 10/19/2017
ms.topic: article
ms.openlocfilehash: 253e5d341f93e61d851893eb05832fbf35707cfc
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="terraform-cloud-shell-development"></a>Terraform Cloud Shell-ontwikkeling 

Terraform prima oplossing vanaf een opdrachtregel Bash zoals Mac OS Terminal of Bash op Windows of Linux. Met uw Terraform configuraties in de Bash ervaring van de [Azure Cloud Shell](/azure/cloud-shell/overview) heeft enkele unieke voordelen om ontwikkelingscyclus te versnellen.

In dit artikel concepten bevat informatie over functies waarmee u scripts kan schrijven Terraform waarmee in Azure implementeren Cloud-Shell.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Automatische verwijzingsconfiguratie

Terraform is geïnstalleerd en wordt onmiddellijk beschikbaar in de Cloud-Shell. Terraform scripts verifiëren met Azure wanneer aangemeld bij de Shell Cloud om infrastructuur zonder extra configuratie te beheren. Automatische verificatie wordt overgeslagen moet handmatig een Active Directory service-principal maken en configureren van de provider Azure Terraform variabelen.


## <a name="using-modules-and-providers"></a>Modules en-Providers

Azure Terraform modules nodig referenties voor toegang tot en wijzigingen aanbrengen in de resources in uw Azure-abonnement. Als u werkt in de Cloud-Shell, moet u de volgende code toevoegen aan uw scripts Azure Terraform modules in de Cloud-Shell gebruiken:

```tf
# Configure the Microsoft Azure Provider
provider "azurerm" {
}
```

De Cloud-Shell stuurt de vereiste waarden voor de `azurerm` provider via omgevingsvariabelen wanneer u een van de `terraform` CLI-opdrachten.

## <a name="other-cloud-shell-developer-tools"></a>Andere hulpprogramma's voor ontwikkelaars van Cloud-Shell

Bestanden en shell-statussen bewaard in Azure Storage tussen sessies van Cloud-Shell. Gebruik [Azure Opslagverkenner](/azure/vs-azure-tools-storage-manage-with-storage-explorer) te kopiëren bestanden uploaden naar de Cloud-Shell uit uw lokale computer.

De Azure CLI 2.0 is beschikbaar in de Cloud-Shell en is een uitstekend hulpprogramma voor het testen van configuraties en controleren van uw werk na een `terraform apply` of `terraform destroy` is voltooid.


## <a name="next-steps"></a>Volgende stappen

[Maak een kleine VM-cluster met behulp van het register van de Module](terraform-create-vm-cluster-module.md)
[een kleine VM-cluster met behulp van aangepaste lijst maken](terraform-create-vm-cluster-with-infrastructure.md)
