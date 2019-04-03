---
title: Metrische gegevens in Azure Monitor | Microsoft Docs
description: Beschrijving van metrische gegevens in Azure Monitor die lichtgewicht kan ondersteunen bijna realtime scenario's door gegevens te controleren.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2019
ms.author: bwren
ms.openlocfilehash: 487f70e4055f16c56092f2f970d2a34238e7febe
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851840"
---
# <a name="metrics-in-azure-monitor"></a>Metrische gegevens in Azure Monitor

> [!NOTE]
> Het gegevensplatform van Azure Monitor is gebaseerd op twee fundamentele gegevenstypen: Metrische gegevens en Logboeken. Dit artikel beschrijft de metrische gegevens. Raadpleeg [registreert in Azure Monitor](data-platform-logs.md) voor een gedetailleerde beschrijving van Logboeken en zo [Azure Monitor gegevens platforn](data-platform.md) voor een vergelijking van de twee.


Metrische gegevens in Azure Monitor zijn lichtgewicht en kan ondersteunen bijna realtime scenario's, waardoor ze met name handig voor meldingen en snelle detectie van problemen. In dit artikel wordt beschreven hoe metrische gegevens zijn gestructureerd, wat u ermee kunt doen, en identificeert verschillende gegevensbronnen die gegevens opslaan in metrische gegevens.

## <a name="what-are-metrics"></a>Wat zijn metrische gegevens?
Metrische gegevens zijn numerieke waarden die worden beschreven van een bepaald aspect van een systeem op een bepaald tijdstip. Metrische gegevens worden verzameld met regelmatige tussenpozen en zijn nuttig omdat ze vaak kunnen worden verzameld, en een waarschuwing kan snel worden geactiveerd met relatief eenvoudige logica op waarschuwingen.

## <a name="what-can-you-do-with-azure-monitor-metrics"></a>Wat kunt u doen met Azure Monitor Metrics?
De volgende tabel bevat de verschillende manieren die u kunt metrische gegevens in Azure Monitor gebruiken.

|  |  |
|:---|:---|
| Analyseren | Gebruik [metrics explorer](metrics-charts.md) voor het analyseren van de verzamelde metrische gegevens over een grafiek en vergelijken van metrische gegevens uit verschillende bronnen. |
| Visualiseren | Een grafiek van metrics explorer vast een [Azure-dashboard](../learn/tutorial-app-dashboards.md).<br>Maak een [werkmap](../app/usage-workbooks.md) te combineren met meerdere sets met gegevens in een interactieve rapport. De resultaten van een query voor het exporteren [Grafana](grafana-plugin.md) gebruikmaken van de dashboarding en combineren met andere gegevensbronnen. |
| Waarschuwing | Configureer een [waarschuwingsregel voor metrische gegevens](alerts-metric.md) die duurt of verzendt een melding [automatische actie](action-groups.md) wanneer de metrische waarde een drempel overschrijden. |
| Automatiseren |  Gebruik [voor automatisch schalen](autoscale-overview.md) vergroten of verkleinen van resources op basis van een metrische waarde een drempelwaarde overschrijden. |
| Exporteren | [Metrische gegevens routeren naar Logboeken](diagnostic-logs-stream-log-store.md) voor het analyseren van gegevens in Azure Monitor metrische gegevens en de gegevens in Azure Monitor-logboeken en voor het opslaan van de metrische waarden voor langer dan 93 dagen.<br>Metrische gegevens naar Stream een [Event Hub](stream-monitoring-data-event-hubs.md) voor het routeren ze met externe systemen. |
| Ophalen | Toegang tot metrische waarden vanuit een opdrachtregel met [PowerShell-cmdlets](https://docs.microsoft.com/powershell/module/azurerm.insights/?view=azurermps-6.7.0)<br>Toegang tot metrische waarden van het gebruik van aangepaste toepassing [REST-API](rest-api-walkthrough.md).<br>Toegang tot metrische waarden vanuit een opdrachtregel met [CLI](/azure/monitor/metrics). |
| Archiveren | [Archief](..//learn/tutorial-archive-data.md) de geschiedenis van prestaties of de status van uw resource voor naleving, controle- of offline rapportagedoeleinden. |


## <a name="how-is-data-in-azure-monitor-metrics-structured"></a>Hoe zijn de gegevens in Azure Monitor Metrics gestructureerd?
Gegevens die worden verzameld door Azure Monitor metrische gegevens worden opgeslagen in een time series-database die is geoptimaliseerd voor het analyseren van gegevens tijdstempel. Elke set metrische waarden is een tijdreeks met de volgende eigenschappen:

* De tijd die de waarde die is verzameld
* De waarde van de resource is gekoppeld
* Een naamruimte die als een categorie voor de metrische gegevens fungeert
* Een naam van de meetwaarde
* De waarde zelf
* Sommige metrische gegevens kan meerdere dimensies hebben, zoals beschreven in [multi-dimensionale metrische gegevens](#multi-dimensional-metrics). Aangepaste metrische gegevens kan maximaal 10 dimensies hebben.

Metrische gegevens in Azure worden 93 dagen bewaard. U kunt [platform metrische gegevens voor resources van Azure Monitor verzenden naar Log Analytics-werkruimte](diagnostic-logs-stream-log-store.md) voor de lange termijn trends.

## <a name="multi-dimensional-metrics"></a>Multi-dimensionale metrische gegevens
Een van de uitdagingen bij het metrische gegevens is dat het vaak heeft beperkte informatie voor meer context voor waarden die worden verzameld. Azure Monitor lost deze uitdaging met multi-dimensionale metrische gegevens. De grootte van een metrische waarde zijn naam / waarde-paren die aanvullende gegevens om te beschrijven van de metrische waarde bevatten. Bijvoorbeeld, een metrische waarde _beschikbare ruimte op schijf_ kan hebben een dimensie met de naam _station_ met waarden _C:_, _D:_, waarmee zou weergeven de beschikbare schijfruimte op alle stations of voor elk station afzonderlijk.

In het volgende voorbeeld ziet u twee gegevenssets voor een hypothetische metrische gegevens met de naam _netwerkdoorvoer_. De eerste gegevensset heeft geen dimensies. De tweede gegevensset bevat de waarden met twee dimensies _IP-adres_ en _richting_:

### <a name="network-throughput"></a>Netwerkdoorvoer

| Tijdstempel     | Metrische waarde |
| ------------- |:-------------|
| 8/9/2017 8:14 | 1,331.8 kbps |
| 8/9/2017 8:15 | 1,141.4 kbps |
| 8/9/2017 8:16 | 1,110.2 kbps |

Deze niet-dimensionale metrische gegevens kan alleen een eenvoudige vraag beantwoorden, zoals "Wat is mijn netwerkdoorvoer op een bepaald moment?"

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>De netwerkdoorvoer + twee dimensies (IP '-' en "Richting")

| Tijdstempel     | Dimensie '-IP   | Dimensie "Richting" | Metrische waarde|
| ------------- |:-----------------|:------------------- |:-----------|
| 8/9/2017 8:14 | IP = "192.168.5.2" | Richting = "Verzenden"    | 646.5 kbps |
| 8/9/2017 8:14 | IP = "192.168.5.2" | Richting = 'Ontvangen' | 420.1 kbps |
| 8/9/2017 8:14 | IP = "10.24.2.15"  | Richting = "Verzenden"    | 150,0 kbps |
| 8/9/2017 8:14 | IP = "10.24.2.15"  | Richting = 'Ontvangen' | 115,2 kbps |
| 8/9/2017 8:15 | IP = "192.168.5.2" | Richting = "Verzenden"    | 515.2 kbps |
| 8/9/2017 8:15 | IP = "192.168.5.2" | Richting = 'Ontvangen' | 371.1 kbps |
| 8/9/2017 8:15 | IP = "10.24.2.15"  | Richting = "Verzenden"    | 155.0 kbps |
| 8/9/2017 8:15 | IP = "10.24.2.15"  | Richting = 'Ontvangen' | 100.1 kbps |

Deze metrische gegevens kunt beantwoorden vragen zoals "Wat was de doorvoer van het netwerk voor elk IP-adres?" en "hoeveel gegevens is verzonden en ontvangen?" Multi-dimensionale metrische gegevens over uitvoeren als u meer analytische en diagnostische waarde ten opzichte van niet-dimensionale metrische gegevens.

## <a name="interacting-with-azure-monitor-metrics"></a>Interactie met Azure Monitor Metrics
Gebruik [Metrics Explorer](metrics-charts.md) interactief de gegevens in uw database metrische gegevens analyseren en de waarden van meerdere metrische gegevens gedurende een periode van grafiek. U kunt de grafieken aan een dashboard om ze te bekijken met de andere visualisaties vastmaken. U kunt ook metrische gegevens ophalen met behulp van de [Azure REST-API bewaken](rest-api-walkthrough.md).

![Metrics Explorer](media/data-platform/metrics-explorer.png)

## <a name="sources-of-azure-monitor-metrics"></a>Bronnen van Azure Monitor Metrics
Er zijn drie fundamentele bronnen van metrische gegevens die door Azure Monitor worden verzameld. Zodra deze metrische gegevens in de database van Azure Monitor-metrische gegevens zijn verzameld, kunnen ze samen worden geëvalueerd, ongeacht de bron.

**Metrische gegevens platform** worden gemaakt door Azure-resources en geven u inzicht in hun status en prestaties. Elk type resource maakt een [aparte set metrische gegevens](metrics-supported.md) zonder configuratie vereist. Platform metrische gegevens worden verzameld van Azure-resources op één minuut frequentie, tenzij anders aangegeven in de definitie van de metrische gegevens. 

**Metrische gegevens voor Guest OS** worden verzameld van het gastbesturingssysteem van een virtuele machine. Gast OS metrische gegevens voor Windows-machines met inschakelen [Windows diagnostische extensie (WAD),](../platform/diagnostics-extension-overview.md) en voor virtuele Linux-machines met [InfluxData Telegraf Agent](https://www.influxdata.com/time-series-platform/telegraf/).

**Metrische toepassingsgegevens** zijn gemaakt door Application Insights voor uw bewaakte toepassingen en kunt u prestatieproblemen detecteren en bijhouden van trends in hoe uw toepassing wordt gebruikt. Dit omvat ook deze waarden als _serverreactietijd_ en _browseruitzonderingen_.

**Aangepaste metrische gegevens** metrische gegevens die u definieert naast de standaard metrische gegevens die automatisch beschikbaar zijn zijn. U kunt [aangepaste metrische gegevens in uw toepassing definiëren](../app/api-custom-events-metrics.md) die wordt bewaakt door Application Insights of maak aangepaste metrische gegevens voor het gebruik van een Azure-service de [aangepaste metrische gegevens API](metrics-store-custom-rest-api.md).


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [gegevensplatform in de Azure Monitor](data-platform.md).
- Meer informatie over [gegevens vastleggen in Azure Monitor](data-platform-logs.md).
- Meer informatie over de [bewakingsgegevens beschikbaar](data-sources.md) voor verschillende resources in Azure.
