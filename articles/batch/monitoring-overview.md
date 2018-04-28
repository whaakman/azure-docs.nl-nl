---
title: Azure Batch bewaken | Microsoft Docs
description: Meer informatie over Azure services voor bewaking, metrische gegevens, logboeken met diagnostische gegevens en andere bewakingsfuncties voor Azure Batch.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 04/05/2018
ms.author: danlep
ms.openlocfilehash: 29ac86ed5c744d37150b0f1b2db17f60306fe77e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="monitor-batch-solutions"></a>Batch-oplossingen controleren

Azure en de Batch-service bieden een aantal services, hulpprogramma's en API's voor het bewaken van uw Batch-oplossingen. In dit overzichtsartikel kunt u kiezen voor een controle methode die past bij uw behoeften.

Zie voor een overzicht van de Azure-onderdelen en services die beschikbaar zijn voor het bewaken van de Azure-resources [bewaking Azure-toepassingen en bronnen](../monitoring-and-diagnostics/monitoring-overview.md).

## <a name="subscription-level-monitoring"></a>Abonnement bewaking

Op het abonnementsniveau, waaronder Batch-accounts, het [Azure activiteitenlogboek](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) verzamelt gegevens van de operationele gebeurtenissen in [verscheidene categorieën](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md#categories-in-the-activity-log).

Voor Batch-accounts in het bijzonder het activiteitenlogboek worden verzameld gebeurtenissen met betrekking tot accountbeheer maken en verwijderen en de sleutel.

Een manier om het ophalen van gebeurtenissen van uw activiteitenlogboek is het gebruik van de Azure-portal. Klik op **alle services** > **activiteitenlogboek**. Of de query voor gebeurtenissen met de Azure CLI, PowerShell-cmdlets of de Monitor REST-API van Azure. U kunt ook het activiteitenlogboek exporteren of configureren [activiteit logboek waarschuwingen](../monitoring-and-diagnostics/monitoring-activity-log-alerts-new-experience.md).

## <a name="batch-account-level-monitoring"></a>Batch-account niveau bewaking

Controleer elke Batch-account gebruikt functies van [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Monitor voor Azure verzamelt [metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md) en optioneel [diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) voor resources binnen het bereik op het niveau van een Batch-account, zoals pools, jobs en taken. Verzamelen en deze gegevens handmatig of programmatisch gebruiken om te controleren van activiteiten in uw Batch-account en om problemen te diagnosticeren. Zie voor meer informatie [Batch metrische gegevens, meldingen en logboeken voor de bewaking en evaluatie van diagnostische](batch-diagnostics.md).
 
> [!NOTE]
> Metrische gegevens zijn standaard beschikbaar in uw Batch-account zonder aanvullende configuratie en ze hebben een rolling geschiedenis van 30 dagen. U moet Diagnostische logboekregistratie voor een Batch-account inschakelen en u mogelijk extra kosten wordt opgeslagen of diagnostische logboekgegevens te verwerken. 

## <a name="batch-resource-monitoring"></a>Broncontrole voor batch

Gebruik de Batch-API's om te controleren of de status van uw resources inclusief jobs, taken, knooppunten en groepen opvragen in uw Batch-toepassingen. Bijvoorbeeld:

* [Taken tellen aan de hand van de status](batch-get-task-counts.md)
* [Query's op de lijst met Batch-resources efficiënt maken](batch-efficient-list-queries.md)
* [Taakafhankelijkheden maken](batch-task-dependencies.md)
* Gebruik een [jobbeheertaak](/rest/api/batchservice/job/add#jobmanagertask)
* Monitor voor de [status van de taak](/rest/api/batchservice/task/list#taskstate)
* Monitor voor de [status van knooppunt](/rest/api/batchservice/computenode/list#computenodestate)
* Monitor voor de [status groep](/rest/api/batchservice/pool/get#poolstate)
* Monitor [gebruik in het account van toepassingen](/rest/api/batchservice/pool/listusagemetrics)
* [Aantal toepassingen knooppunten op basis van status](/rest/api/batchservice/account/listpoolnodecounts)

## <a name="vm-performance-counters-and-application-monitoring"></a>VM-prestatiemeteritems en toepassingsbewaking

* [Application Insights](../application-insights/app-insights-overview.md) is een Azure-service kunt u programmatisch bewaken van de beschikbaarheid, prestaties en gebruik van uw Batch-jobs en taken. Get-prestatiemeteritems van rekenknooppunten eenvoudig (VM's) en aangepaste gegevens voor taken op de virtuele machines. 

  Zie voor een voorbeeld [Monitor en foutopsporing een Batch .NET-toepassing met Application Insights](monitor-application-insights.md) en de bijbehorende [codevoorbeeld](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights).

  > [!NOTE]
  > U mogelijk extra kosten voor het gebruik van Application Insights. Zie de [prijzen opties](https://azure.microsoft.com/pricing/details/application-insights/). 
  >

* [BatchLabs](https://github.com/Azure/BatchLabs) is een gratis, uitgebreid, zelfstandig clienthulpprogramma voor het maken en bewaken van en opsporen van fouten in Azure Batch-toepassingen. Download een [installatiepakket](https://azure.github.io/BatchLabs/) voor Mac, Linux of Windows. Configureer desgewenst uw Batch-oplossing [Application Insights-gegevens weer](https://github.com/Azure/batch-insights) zoals VM-prestatiemeters in BatchLabs.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [Batch-API's en -hulpprogramma's](batch-apis-tools.md) die beschikbaar zijn voor het bouwen van Batch-oplossingen.
* Meer informatie over [Diagnostische logboekregistratie](batch-diagnostics.md) met Batch.