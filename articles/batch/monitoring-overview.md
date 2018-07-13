---
title: Bewaken met Azure Batch | Microsoft Docs
description: Meer informatie over Azure-bewakingsservices, metrische gegevens, logboeken met diagnostische gegevens en andere bewakingsfuncties voor Azure Batch.
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
ms.openlocfilehash: fd4226254e076957ac8651f9172f1f3f82427a60
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001827"
---
# <a name="monitor-batch-solutions"></a>Batch-oplossingen controleren

Azure en de Batch-service bieden een scala aan services, hulpprogramma's en API's voor het bewaken van uw Batch-oplossingen. In dit overzichtsartikel kunt u kiezen voor een controle methode die past bij uw behoeften.

Zie voor een overzicht van de Azure-onderdelen en services die beschikbaar zijn voor het bewaken van Azure-resources, [bewaking van Azure-toepassingen en bronnen](../monitoring-and-diagnostics/monitoring-overview.md).

## <a name="subscription-level-monitoring"></a>Abonnement bewaking

Op het abonnementsniveau, waaronder Batch-accounts, het [Azure-activiteitenlogboek](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) verzamelt gegevens van operationele gebeurtenissen in [verschillende categorieën](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md#categories-in-the-activity-log).

Voor Batch-accounts verzamelt het activiteitenlogboek specifiek, gebeurtenissen met betrekking tot het maken en verwijderen en de sleutel accountbeheer.

Een manier om op te halen van gebeurtenissen uit uw activiteitenlogboek is het gebruik van Azure portal. Klik op **alle services** > **activiteitenlogboek**. Of de query voor gebeurtenissen met de Azure CLI, PowerShell-cmdlets of de Azure Monitor REST API. U kunt ook het activiteitenlogboek exporteren of configureert [waarschuwingen voor activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-activity-log-alerts-new-experience.md).

## <a name="batch-account-level-monitoring"></a>Bewaking van de batch-account op

Controleer elke Batch-account met behulp van functies van [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure Monitor verzamelt [metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md) en eventueel [diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) voor resources binnen het bereik op het niveau van een Batch-account, zoals pools, jobs en taken. Verzamelen en gebruiken van deze gegevens handmatig of via een programma voor het bewaken van activiteiten in uw Batch-account en om problemen te diagnosticeren. Zie voor meer informatie, [Batch-metrische gegevens, waarschuwingen en logboeken voor diagnostische evaluatie en bewaking](batch-diagnostics.md).
 
> [!NOTE]
> Metrische gegevens in uw Batch-account zonder aanvullende configuratie standaard beschikbaar zijn en ze beschikken over een 30-daagse rolling-overzicht. U moet Diagnostische logboekregistratie inschakelen voor een Batch-account en u mogelijk extra kosten als u wilt opslaan of diagnostische logboekgegevens te verwerken. 

## <a name="batch-resource-monitoring"></a>Controle van de batch-bronnen

In uw Batch-toepassingen, gebruikt u de Batch-API's om te controleren of de status van uw resources inclusief jobs, taken, knooppunten en pools opvragen. Bijvoorbeeld:

* [Aantal taken en rekenknooppunten per staat](batch-get-resource-counts.md)
* [Efficiënt query's naar de lijst met Batch-resources maken](batch-efficient-list-queries.md)
* [Taakafhankelijkheden maken](batch-task-dependencies.md)
* Gebruik een [jobbeheertaak](/rest/api/batchservice/job/add#jobmanagertask)
* Monitor de [taak status](/rest/api/batchservice/task/list#taskstate)
* Monitor de [status clusterknooppunt](/rest/api/batchservice/computenode/list#computenodestate)
* Monitor de [status van toepassingen](/rest/api/batchservice/pool/get#poolstate)
* Monitor [gebruik in het account van toepassingen](/rest/api/batchservice/pool/listusagemetrics)
* [Het aantal pool-knooppunten per staat](/rest/api/batchservice/account/listpoolnodecounts)

## <a name="vm-performance-counters-and-application-monitoring"></a>Prestatiemeteritems voor virtuele machine en de bewaking van toepassingen

* [Application Insights](../application-insights/app-insights-overview.md) is een Azure-service kunt u voor het programmatisch bewaken van de beschikbaarheid, prestaties en gebruik van uw Batch-taken en taken. Eenvoudig compute get-prestatiemeteritems van-knooppunten (VM's) en aangepaste gegevens voor taken af bij de virtuele machines. 

  Zie voor een voorbeeld [bewaken en fouten opsporen een Batch .NET-toepassing met Application Insights](monitor-application-insights.md) en de bijbehorende [codevoorbeeld](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights).

  > [!NOTE]
  > Er worden mogelijk extra kosten voor het gebruik van Application Insights. Zie de [prijsopties](https://azure.microsoft.com/pricing/details/application-insights/). 
  >

* [BatchLabs](https://github.com/Azure/BatchLabs) is een gratis, uitgebreid, zelfstandig clienthulpprogramma voor het maken en bewaken van en opsporen van fouten in Azure Batch-toepassingen. Download een [installatiepakket](https://azure.github.io/BatchLabs/) voor Mac, Linux of Windows. Configureer desgewenst uw Batch-oplossing [Application Insights-gegevens weergeven](https://github.com/Azure/batch-insights) zoals VM-prestatiemeteritems in BatchLabs.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [Batch-API's en -hulpprogramma's](batch-apis-tools.md) die beschikbaar zijn voor het bouwen van Batch-oplossingen.
* Meer informatie over [registratie in diagnoselogboek](batch-diagnostics.md) met Batch.