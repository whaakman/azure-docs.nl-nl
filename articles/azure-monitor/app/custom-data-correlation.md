---
title: Azure Application Insights | Microsoft Docs
description: ''
services: application-insights
documentationcenter: ''
author: eternovsky
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/08/2018
ms.reviewer: mbullwin
ms.author: Evgeny.Ternovsky
ms.openlocfilehash: cbb144cc8aac6dc8e90d196147b0c154471b7239
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58102057"
---
# <a name="correlating-application-insights-data-with-custom-data-sources"></a>Application Insights-gegevens te correleren met aangepaste gegevensbronnen

Application Insights verzamelt diverse verschillende gegevenstypen: uitzonderingen, traceringen, paginaweergaven en anderen. Dit is vaak voldoende zijn voor het onderzoeken van prestaties, betrouwbaarheid en gebruik van uw toepassing, zijn er gevallen wanneer dit is handig om te correleren met andere volledig aangepaste gegevenssets de gegevens in Application Insights.

Sommige situaties waar u dat aangepaste gegevens wilt zijn onder andere:

- Verrijking of een opzoekfunctie gegevenstabellen: bijvoorbeeld vormen een aanvulling op de naam van een server met de eigenaar van de server en de locatie van het lab waarin deze kan worden gevonden 
- Correlatie met Application Insights-gegevensbronnen: bijvoorbeeld correleren gegevens over een aankoop van een webwinkel met informatie van uw aankoop vervulling-service om te bepalen hoe nauwkeurig de verzendtijd schattingen zijn 
- Volledig aangepaste gegevens: veel van onze klanten graag de querytaal en prestaties van de Azure Monitor log-platform die Application Insights maakt back-ups en deze wilt gebruiken om gegevens te doorzoeken die helemaal niet gerelateerd is aan Application Insights. Bijvoorbeeld, voor het bijhouden van de prestaties microturbines deelvenster als onderdeel van een slimme home installatie als die worden beschreven [hier]( https://blogs.catapultsystems.com/cfuller/archive/2017/10/04/using-log-analytics-and-a-special-guest-to-forecast-electricity-generation/).

## <a name="how-to-correlate-custom-data-with-application-insights-data"></a>Hoe u aangepaste gegevens met Application Insights-gegevens te conrreleren 

Omdat Application Insights wordt geschraagd door het krachtige Azure Monitor log-platform, zijn we kunnen de volledige kracht van Azure Monitor gebruiken om op te nemen van de gegevens. Vervolgens wordt er query's met behulp van de joinoperator '', die wordt deze aangepaste gegevens met de gegevens die beschikbaar is voor ons in Azure Monitor logboeken correleren schrijven. 

## <a name="ingesting-data"></a>Ophalen van gegevens

In deze sectie wordt besproken hoe u uw gegevens in Logboeken van Azure Monitor krijgt.

Als u dit niet al hebt, richt u een nieuwe Log Analytics-werkruimte door [deze instructies](../learn/quick-collect-azurevm.md) en met de stap 'een werkruimte maken'.

Om te beginnen met het verzenden van gegevens in Azure Monitor. Er zijn diverse opties:

- Een synchrone mechanisme, kunt u ofwel rechtstreeks aanroepen de [gegevensverzamelaar-API](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api) of gebruik onze logische App-connector: gewoon zoek naar "Azure Log Analytics" en kies de optie 'Gegevens verzenden':

  ![Schermafbeelding kiezen en de actie](./media/custom-data-correlation/01-logic-app-connector.png)  

- Gebruik de Collector-API om een pijplijn voor een asynchrone optie. Zie [in dit artikel](https://docs.microsoft.com/azure/log-analytics/log-analytics-create-pipeline-datacollector-api) voor meer informatie.

## <a name="correlating-data"></a>Correleren van gegevens

Application Insights is gebaseerd op de Azure Monitor log-platform. We kunnen daarom gebruiken [meerdere bronnen joins](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search) correleren van gegevens we die zijn opgenomen in Azure Monitor met onze Application Insights-gegevens.

Bijvoorbeeld, kunnen we onze lab-inventaris en de locaties in een tabel met de naam 'LabLocations_CL' in een Log Analytics-werkruimte met de naam 'myLA' opnemen. Als we vervolgens wilden onze aanvragen worden bijgehouden in Application Insights-app met de naam 'myAI' bekijken en de namen van de machines die de aanvragen naar de locatie van deze machines die zijn opgeslagen in de eerder genoemde aangepaste tabel behandeld correleren, kunnen we de volgende query uit uitvoeren de Application Insights of Azure Monitor-context:

```
app('myAI').requests
| join kind= leftouter (
    workspace('myLA').LabLocations_CL
    | project Computer_S, Owner_S, Lab_S
) on $left.cloud_RoleInstance == $right.Computer
```

## <a name="next-steps"></a>Volgende stappen

- Bekijk de [Data Collector API](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api) verwijzing.
- Voor meer informatie over [meerdere bronnen joins](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search).
