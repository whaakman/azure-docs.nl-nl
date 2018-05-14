---
title: Azure Blockchain Workbench probleemoplossing
description: Klik hier voor meer informatie over het oplossen van een toepassing Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/21/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 8a2715666c4fff490f5184b7b8719b412952b9bf
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="azure-blockchain-workbench-troubleshooting"></a>Azure Blockchain Workbench probleemoplossing

Een PowerShell-script is beschikbaar om te helpen bij foutopsporing developer of ondersteunen. Het script genereert een overzicht en verzamelt gedetailleerde logboeken voor het oplossen van problemen. Verzamelde logboeken bevatten:

* Blockchain netwerk, zoals Ethereum
* Blockchain Workbench microservices
* Application Insights
* Azure Monitoring (OMS)

U kunt de informatie gebruiken om te bepalen van de volgende stappen en bepalen de hoofdoorzaak van problemen. 

## <a name="troubleshooting-script"></a>Het script oplossen

Het PowerShell script probleemoplossing is beschikbaar op GitHub. [Download een ZIP-bestand ](https://github.com/Azure-Samples/blockchain/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

```
git clone https://github.com/Azure-Samples/blockchain.git
```

## <a name="run-the-script"></a>Het script uitvoeren
[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

Voer de `collectBlockchainWorkbenchTroubleshooting.ps1` script voor het verzamelen van Logboeken en maak een ZIP-bestand met een map met informatie over probleemoplossing. Bijvoorbeeld:

``` powershell
collectBlockchainWorkbenchTroubleshooting.ps1 -SubscriptionID "<subscription_id>" -ResourceGroupName "workbench-resource-group-name"
```
Het script accepteert de volgende parameters:

| Parameter  | Beschrijving | Vereist |
|---------|---------|----|
| Abonnements-id | Abonnement-id te maken of alle resources zoeken. | Ja |
| ResourceGroupName | Naam van de Azure-resourcegroep waar Blockchain Workbench is geïmplementeerd. | Ja |
| Uitvoermap | Pad voor het maken van de uitvoer. ZIP-bestand. Als u niet opgeeft, wordt standaard ingesteld op de huidige map. | Nee
| OmsSubscriptionId | De abonnements-id waarop OMS wordt geïmplementeerd. Deze parameter alleen doorgeven als de OMS voor het netwerk blockchain buiten Blockchain-Workbench resourcegroep is geïmplementeerd.| Nee |
| OmsResourceGroup |De resourcegroep waar OMS is geïmplementeerd. Deze parameter alleen doorgeven als de OMS voor het netwerk blockchain buiten Blockchain-Workbench resourcegroep is geïmplementeerd.| Nee |
| OmsWorkspaceName | De naam van de OMS-werkruimte. Deze parameter alleen doorgeven als de OMS voor het netwerk blockchain buiten Blockchain-Workbench resourcegroep is geïmplementeerd | Nee |

## <a name="what-is-collected"></a>Wat wordt er verzameld?

Het ZIP-bestand voor uitvoer bevat de volgende mapstructuur:

| Map \ bestand | Beschrijving  |
|---------|---------|
| \Summary.txt | Samenvatting van het systeem |
| \metrics\blockchain | Metrische gegevens over de blockchain |
| \metrics\workbench | Metrische gegevens over de workbench |
| \details\blockchain | Gedetailleerde logboekgegevens over de blockchain |
| \details\workbench | Gedetailleerde logboekgegevens over de workbench |

Het overzichtsbestand biedt u een momentopname van de algehele status van de toepassing en status van de toepassing. De samenvatting biedt een aanbevolen acties, illustreert bovenste fouten en metagegevens over het uitvoeren van services.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench-architectuur](blockchain-workbench-architecture.md)