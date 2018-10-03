---
title: Azure Blockchain Workbench oplossen van problemen
description: Klik hier voor meer informatie over het oplossen van een Azure Blockchain Workbench-toepassing.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: ba6bfe8240c758806c4ff3e46ab08fdacad83db9
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48242348"
---
# <a name="azure-blockchain-workbench-troubleshooting"></a>Azure Blockchain Workbench oplossen van problemen

Een PowerShell-script is beschikbaar voor ondersteuning bij foutopsporing ontwikkelaar of ondersteunen. Het script genereert een overzicht en verzamelt gedetailleerde logboeken voor het oplossen van problemen. Verzamelde logboeken zijn onder andere:

* Blockchain-netwerk, zoals Ethereum
* Blockchain Workbench microservices
* Application Insights
* Azure Monitoring (OMS)

U kunt de informatie gebruiken om te bepalen van de volgende stappen en bepaalt de hoofdoorzaak van problemen. 

## <a name="troubleshooting-script"></a>Script voor het oplossen van problemen

Het PowerShell script probleemoplossing is beschikbaar op GitHub. [Download een ZIP-bestand ](https://github.com/Azure-Samples/blockchain/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

```
git clone https://github.com/Azure-Samples/blockchain.git
```

## <a name="run-the-script"></a>Het script uitvoeren
[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

Voer de `collectBlockchainWorkbenchTroubleshooting.ps1` script voor het verzamelen van Logboeken en maak een ZIP-bestand met een map met informatie over probleemoplossing. Bijvoorbeeld:

``` powershell
collectBlockchainWorkbenchTroubleshooting.ps1 -SubscriptionID "<subscription_id>" -ResourceGroupName "workbench-resource-group-name"
```
Het script accepteert de volgende parameters:

| Parameter  | Beschrijving | Vereist |
|---------|---------|----|
| abonnements-id | Abonnement-id te maken of te zoeken naar alle bronnen. | Ja |
| ResourceGroupName | De naam van de Azure-resourcegroep wanneer Blockchain Workbench is geïmplementeerd. | Ja |
| Sleutelbestandpad | Pad voor het maken van de uitvoer. ZIP-bestand. Als niet is opgegeven, kunt u standaard naar de huidige map. | Nee |
| LookbackHours | Het aantal uren moet worden gebruikt bij het ophalen van telemetrie. Standaardwaarde is 24 uur. De maximumwaarde is 90 uur | Nee |
| OmsSubscriptionId | De abonnements-ID waarop OMS wordt geïmplementeerd. Geef deze parameter alleen als de OMS voor het netwerk blockchain buiten de resourcegroep van de Blockchain Workbench wordt geïmplementeerd.| Nee |
| OmsResourceGroup |De resourcegroep waarop OMS wordt geïmplementeerd. Geef deze parameter alleen als de OMS voor het netwerk blockchain buiten de resourcegroep van de Blockchain Workbench wordt geïmplementeerd.| Nee |
| OmsWorkspaceName | De naam van de OMS-werkruimte. Alleen deze parameter doorgeven als de OMS voor het netwerk blockchain wordt geïmplementeerd buiten de resourcegroep van de Blockchain Workbench | Nee |

## <a name="what-is-collected"></a>Wat wordt er verzameld?

Het ZIP-bestand voor uitvoer bevat de volgende mapstructuur:

| Bestand of map | Beschrijving  |
|---------|---------|
| \Summary.txt | Samenvatting van het systeem |
| \Metrics\blockchain | Metrische gegevens over de blockchain |
| \Metrics\Workbench | Metrische gegevens over de workbench |
| \Details\Blockchain | Gedetailleerde logboekgegevens over de blockchain |
| \Details\Workbench | Gedetailleerde logboeken over de workbench |

Het bestand overzicht biedt u een momentopname van de algehele status van de toepassing en de status van de toepassing. De samenvatting biedt aanbevolen acties uitvoert, markeert de ernstigste fouten en metagegevens over het uitvoeren van services.

De **metrische gegevens** map metrische gegevens van verschillende onderdelen van het systeem na verloop van tijd bevat. Bijvoorbeeld, het uitvoerbestand `\Details\Workbench\apiMetrics.txt` bevat een overzicht van andere responscodes en reactietijden gedurende de periode van de verzameling. De **Details** map bevat gedetailleerde logboeken voor het oplossen van specifieke problemen met Workbench of het onderliggende blockchain-netwerk. Bijvoorbeeld, `\Details\Workbench\Exceptions.csv` bevat een overzicht van de meest recente uitzonderingen die zijn opgetreden in het systeem, wat nuttig is voor het oplossen van fouten met slimme contracten of interactie met de blockchain. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Architectuur van Azure Blockchain Workbench](architecture.md)