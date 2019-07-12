---
title: Klassieke Azure-implementatieresources verplaatsen naar een nieuw abonnement of de resource-groep
description: Gebruik Azure Resource Manager klassieke implementatieresources verplaatsen naar een nieuwe resourcegroep of abonnement.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 4770f957b6b9eea75b50776a7491b1ca479e50e2
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723504"
---
# <a name="move-guidance-for-classic-deployment-model-resources"></a>Richtlijnen voor Classic deployment model resources verplaatsen

De stappen voor het verplaatsen van resources die zijn geïmplementeerd via het klassieke model zijn afhankelijk van of u de resources binnen een abonnement of naar een nieuw abonnement verplaatst.

## <a name="move-in-the-same-subscription"></a>In hetzelfde abonnement verplaatsen

Bij het verplaatsen van resources van een resourcegroep naar een andere resourcegroep binnen hetzelfde abonnement, gelden de volgende beperkingen:

* Virtuele netwerken (klassiek) kunnen niet worden verplaatst.
* Virtuele machines (klassiek) moeten worden verplaatst met de cloudservice.
* Cloudservice kan alleen worden verplaatst wanneer de verplaatsing alle virtuele machines bevat.
* Slechts één cloudservice kan tegelijk worden verplaatst.
* Slechts één opslagaccount (klassiek) kan tegelijk worden verplaatst.
* Opslagaccount (klassiek) kan niet worden verplaatst in dezelfde bewerking met een virtuele machine of een service in de cloud.

Gebruikt om klassieke resources verplaatsen naar een nieuwe resourcegroep binnen hetzelfde abonnement, de [standard verplaatsingsbewerkingen](../resource-group-move-resources.md) via de portal, Azure PowerShell, Azure CLI of REST-API. U dezelfde bewerkingen als die u gebruikt voor het verplaatsen van Resource Manager-resources.

## <a name="move-across-subscriptions"></a>Verplaatsen tussen abonnementen

Wanneer u resources verplaatst naar een nieuw abonnement, gelden de volgende beperkingen:

* Alle klassieke resources in het abonnement moeten worden verplaatst in dezelfde bewerking.
* Het doelabonnement moet geen andere klassieke resources.
* De overstap kan alleen worden aangevraagd door een afzonderlijke REST-API voor klassieke verplaatst. De standaardopdrachten verplaatsen van Resource Manager werken niet bij het klassieke resources verplaatsen naar een nieuw abonnement.

Om klassieke resources verplaatsen naar een nieuw abonnement, de REST-bewerkingen die specifiek voor klassieke resources zijn te gebruiken. Voor het gebruik van REST, voer de volgende stappen uit:

1. Controleer als van het bronabonnement deel uitmaken van een verplaatsing van kruislings abonnement. Gebruik de volgende bewerking:

   ```HTTP
   POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     In de hoofdtekst van de aanvraag, zijn onder andere:

   ```json
   {
    "role": "source"
   }
   ```

     Het antwoord voor de validatiebewerking is in de volgende indeling:

   ```json
   {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
   }
   ```

1. Controleer als het doelabonnement deel uitmaken van een verplaatsing van kruislings abonnement. Gebruik de volgende bewerking:

   ```HTTP
   POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     In de hoofdtekst van de aanvraag, zijn onder andere:

   ```json
   {
    "role": "target"
   }
   ```

     Het antwoord is in dezelfde indeling als de validatie van het abonnement.
1. Als beide abonnementen zijn gevalideerd, alle klassieke resources wilt verplaatsen van één abonnement naar een ander abonnement met de volgende bewerking:

   ```HTTP
   POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
   ```

    In de hoofdtekst van de aanvraag, zijn onder andere:

   ```json
   {
    "target": "/subscriptions/{target-subscription-id}"
   }
   ```

De bewerking kan enkele minuten uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

Als u problemen ondervindt bij het verplaatsen van klassieke resources, neem dan contact op met [ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

Zie voor de opdrachten om resources te verplaatsen, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../resource-group-move-resources.md).
