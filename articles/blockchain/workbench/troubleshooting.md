---
title: Azure Blockchain Workbench oplossen van problemen
description: Klik hier voor meer informatie over het oplossen van een Azure Blockchain Workbench-toepassing.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 01/14/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 9f0f32bc1fb6b88dc85f09e13aebc60ff74ec723
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54329732"
---
# <a name="azure-blockchain-workbench-troubleshooting"></a>Azure Blockchain Workbench oplossen van problemen

Een PowerShell-script is beschikbaar ter ondersteuning van foutopsporing door de ontwikkelaar. Het script genereert een overzicht en verzamelt gedetailleerde logboeken voor het oplossen van problemen. Verzamelde logboeken zijn onder andere:

* Blockchain-netwerk, zoals Ethereum
* Blockchain Workbench microservices
* Application Insights
* Azure Monitoring (Log Analytics)

U kunt de informatie gebruiken om de volgende stappen te bepalen en de hoofdoorzaak van problemen te achterhalen.

## <a name="troubleshooting-script"></a>Script voor het oplossen van problemen

Het PowerShell probleemoplossingsscript is beschikbaar op GitHub. [Download een ZIP-bestand ](https://github.com/Azure-Samples/blockchain/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

```
git clone https://github.com/Azure-Samples/blockchain.git
```

## <a name="run-the-script"></a>Het script uitvoeren
[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

Voer het `collectBlockchainWorkbenchTroubleshooting.ps1` script voor het verzamelen van logboeken uit en maak een ZIP-bestand van de map met informatie over probleemoplossing. Bijvoorbeeld:

``` powershell
collectBlockchainWorkbenchTroubleshooting.ps1 -SubscriptionID "<subscription_id>" -ResourceGroupName "workbench-resource-group-name"
```
Het script accepteert de volgende parameters:

| Parameter  | Beschrijving | Vereist |
|---------|---------|----|
| abonnements-id | Abonnement-id om alle bronnen te maken of te zoeken. | Ja |
| ResourceGroupName | De naam van de Azure-resourcegroep waar Blockchain Workbench is geïmplementeerd. | Ja |
| OutputDirectory | Pad voor het maken van het ZIP-bestand met uitvoer. Als dit niet is opgegeven, wordt standaard de huidige map gebruikt. | Nee |
| LookbackHours | Het aantal uren dat wordt gehanteerd bij het ophalen van telemetrie. Standaardwaarde is 24 uur. De maximumwaarde is 90 uur | Nee |
| OmsSubscriptionId | Abonnements-ID waar Log Analytics is geïmplementeerd. Geef deze parameter alleen op als Log Analytics voor het blockchain-netwerk buiten de resourcegroep van de Blockchain Workbench is geïmplementeerd.| Nee |
| OmsResourceGroup |De resourcegroep waarin Log Analytics is geïmplementeerd. Geef deze parameter alleen op als Log Analytics voor het blockchain-netwerk buiten de resourcegroep van de Blockchain Workbench is geïmplementeerd.| Nee |
| OmsWorkspaceName | De naam van de Log Analytics-werkruimte. Geef deze parameter alleen op als Log Analytics voor het blockchain-netwerk buiten de resourcegroep van de Blockchain Workbench is geïmplementeerd. | Nee |

## <a name="what-is-collected"></a>Wat wordt er verzameld?

Het ZIP-bestand met output bevat de volgende mapstructuur:

| Bestand of map | Beschrijving  |
|---------|---------|
| \Summary.txt | Samenvatting van het systeem |
| \Metrics\blockchain | Metrische gegevens over de blockchain |
| \Metrics\Workbench | Metrische gegevens over de workbench |
| \Details\Blockchain | Gedetailleerde logboekgegevens over de blockchain |
| \Details\Workbench | Gedetailleerde logboeken over de workbench |

Het samenvattingsbestand biedt u een momentopname van de algehele status van de toepassing. De samenvatting bevat aanbevolen acties, markeert de ernstigste fouten en toont metagegevens van actieve services.

De **metrische gegevens** map bevat metrische gegevens van verschillende onderdelen van het systeem in de tijd. Bijvoorbeeld, het uitvoerbestand `\Details\Workbench\apiMetrics.txt` bevat een overzicht van verschillende responscodes en reactietijden gedurende de periode van de verzameling. De **Details** map bevat gedetailleerde logboeken voor het oplossen van specifieke problemen met Workbench of het onderliggende blockchain-netwerk. Bijvoorbeeld, `\Details\Workbench\Exceptions.csv` bevat een overzicht van de meest recente excepties die zijn opgetreden in het systeem, wat nuttig kan zijn voor het oplossen van fouten met slimme contracten of interactie met de blockchain. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Probleemoplossingsgids voor Azure Blockchain Workbench Application Insights](https://aka.ms/workbenchtroubleshooting)
