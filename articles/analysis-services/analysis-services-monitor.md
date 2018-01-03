---
title: Azure Analysis Services server metrische gegevens controleren | Microsoft Docs
description: Informatie over het bewaken van metrische gegevens Analysis Services-server een in Azure portal.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/14/2017
ms.author: owend
ms.openlocfilehash: 8d19f2d29673364c50cb481e375f2c1b5e027467
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="monitor-server-metrics"></a>Monitor server metrische gegevens

Analyseservices biedt metrische gegevens om te controleren van de prestaties en de status van uw servers. Bijvoorbeeld: bewaken geheugen en CPU-gebruik, het aantal clientverbindingen en het verbruik van de resource. Analysis Services dezelfde controle framework wordt gebruikt als de meeste andere Azure-services. Zie voor meer informatie, [metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Als u meer gedetailleerde diagnostische gegevens, prestaties, bijhouden en trends herkennen in meerdere serviceresources in een resourcegroep of abonnement, met [Azure Monitor](https://azure.microsoft.com/services/monitor/). Monitor voor Azure (service) kan leiden tot een factureerbare service.


## <a name="to-monitor-metrics-for-an-analysis-services-server"></a>Voor het bewaken van metrische gegevens voor een Analysis Services-server

1. Selecteer in Azure portal **metrische gegevens**.

    ![Bewaken in Azure-portal](./media/analysis-services-monitor/aas-monitor-portal.png)

2. In **beschikbare metrische gegevens**, selecteert u de metrische gegevens moeten worden opgenomen in de grafiek. 

    ![Monitor-grafiek](./media/analysis-services-monitor/aas-monitor-chart.png)

<a id="#server-metrics"></a>
## <a name="server-metrics"></a>Metrische servergegevens
Deze tabel gebruiken om te bepalen welke metrische gegevens zijn het meest geschikt is voor uw scenario bewaking. Metrische gegevens van de dezelfde eenheid kunnen worden weergegeven in hetzelfde diagram.

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|CommandPoolJobQueueLength|Opdracht taak wachtrijlengte|Count|Gemiddeld|Het aantal taken in de wachtrij van de threadgroep opdracht.|
|CurrentConnections|Verbinding: Actieve verbindingen|Count|Gemiddeld|Huidig aantal verbindingen van clients tot stand gebracht.|
|CurrentUserSessions|Huidige gebruikerssessies|Count|Gemiddeld|Huidige aantal gebruikerssessies.|
|mashup_engine_memory_metric|M-Engine-geheugen|Bytes|Gemiddeld|Geheugengebruik door processen voor mashup-engine|
|mashup_engine_qpu_metric|M-Engine QPU|Count|Gemiddeld|Gebruik QPU door mashup-engine-processen|
|memory_metric|Geheugen|Bytes|Gemiddeld|Geheugen. Bereik van 0-25 GB voor S1, 0-50 GB voor S2 en 0-100 GB voor S4|
|memory_thrashing_metric|Geheugen voorkomen|Procent|Gemiddeld|Gemiddelde geheugen thrashing.|
|CleanerCurrentPrice|Geheugen: Schonere huidige prijs|Count|Gemiddeld|Huidige prijs geheugen, byte-$/ tijd, genormaliseerd tot en met 1000.|
|CleanerMemoryNonshrinkable|Geheugen: Schonere geheugen nonshrinkable|Bytes|Gemiddeld|Hoeveelheid geheugen in bytes niet onderworpen aan een schoner opschonen door de achtergrond.|
|CleanerMemoryShrinkable|Geheugen: Schonere geheugen verkleining|Bytes|Gemiddeld|De hoeveelheid geheugen in bytes, onderworpen aan een schoner opschonen door de achtergrond.|
|MemoryLimitHard|Geheugen: Geheugenlimiet harde|Bytes|Gemiddeld|Vaste geheugenlimiet uit het configuratiebestand.|
|MemoryLimitHigh|Geheugen: Geheugen beperken hoog|Bytes|Gemiddeld|Maximale geheugenlimiet van configuratiebestand.|
|MemoryLimitLow|Geheugen: Geheugen limiet laag|Bytes|Gemiddeld|Lage geheugenlimiet uit het configuratiebestand.|
|MemoryLimitVertiPaq|Geheugen: Geheugen limiet VertiPaq|Bytes|Gemiddeld|In het geheugen limiet uit het configuratiebestand.|
|MemoryUsage|: Geheugengebruik geheugen|Bytes|Gemiddeld|Het geheugengebruik van de server-proces gebruikt bij het berekenen van de prijs van schonere geheugen. Die gelijk is aan het prestatiemeteritem Process\PrivateBytes plus de grootte van het geheugen toegewezen gegevens, eventuele geheugen die is toegewezen of toegewezen door de engine in geheugen analytics (VertiPaq) die groter zijn dan de geheugenlimiet-engine wordt genegeerd.|
|Quota|Geheugen: quotum|Bytes|Gemiddeld|Huidige geheugenquotum, in bytes. Geheugenquotum wordt ook wel een grant of geheugen reservering voor geheugen.|
|QuotaBlocked|: Geheugenquotum geblokkeerd|Count|Gemiddeld|Huidig aantal quotum aanvragen die worden geblokkeerd totdat het andere Geheugenquota zijn vrijgegeven.|
|VertiPaqNonpaged|Geheugen: VertiPaq-wisselbaar geheugen:|Bytes|Gemiddeld|Bytes van geheugen is vergrendeld in de werkset voor gebruik door de engine in het geheugen.|
|VertiPaqPaged|Geheugen: VertiPaq wisselbaar geheugen:|Bytes|Gemiddeld|Bytes wisselbaar geheugen in gebruik voor gegevens in het geheugen.|
|ProcessingPoolJobQueueLength|Wachtrijlengte taak verwerken|Count|Gemiddeld|Het aantal niet-I/O-taken in de wachtrij van de thread-pool.|
|RowsConvertedPerSec|Verwerking: Rijen geconverteerd per seconde|CountPerSecond|Gemiddeld|Het aantal rijen wordt geconverteerd tijdens de verwerking.|
|RowsReadPerSec|Verwerking: Rijen lezen per seconde|CountPerSecond|Gemiddeld|Het aantal rijen van alle relationele databases worden gelezen.|
|RowsWrittenPerSec|Verwerking: Rijen geschreven per seconde|CountPerSecond|Gemiddeld|De frequentie van geschreven tijdens het verwerken van rijen.|
|qpu_metric|QPU|Count|Gemiddeld|QPU. Bereik 0-100 voor S1, 0-200 voor S2 en 0-400 voor S4|
|QueryPoolBusyThreads|Query groepen bezet Threads|Count|Gemiddeld|Het aantal bezet threads in de query-thread-groep.|
|SuccessfullConnectionsPerSec|Geslaagde verbindingen Per seconde|CountPerSecond|Gemiddeld|Verhouding van geslaagde verbinding voltooiingen.|
|CommandPoolBusyThreads|Threads: Opdracht groepen bezet threads|Count|Gemiddeld|Het aantal bezet threads in de opdracht thread-groep.|
|CommandPoolIdleThreads|Threads: Opdracht groep niet-actieve threads|Count|Gemiddeld|Het aantal niet-actieve threads in de opdracht thread-groep.|
|LongParsingBusyThreads|Threads: Long bezet threads parseren|Count|Gemiddeld|Het aantal bezet threads in de lang parseren thread-groep.|
|LongParsingIdleThreads|Threads: Long bij het parseren van niet-actieve threads|Count|Gemiddeld|Het aantal niet-actieve threads in de lang parseren thread-groep.|
|LongParsingJobQueueLength|Threads: Parseren lang taak wachtrijlengte|Count|Gemiddeld|Het aantal taken in de wachtrij van de threadgroep lang parseren.|
|ProcessingPoolIOJobQueueLength|Threads: Verwerken i/o-taak wachtrijlengte van toepassingen|Count|Gemiddeld|Het aantal i/o-taken in de wachtrij van de thread-pool.|
|ProcessingPoolBusyIOJobThreads|Threads: Groepen bezet i/o-taak threads verwerken|Count|Gemiddeld|Het aantal actieve threads i/o-taken in de thread-pool.|
|ProcessingPoolBusyNonIOThreads|Threads: Groep bezet niet-I/O-threads verwerkt|Count|Gemiddeld|Het aantal threads worden uitgevoerd van niet-I/O-taken in de thread-pool.|
|ProcessingPoolIdleIOJobThreads|Threads: Groep niet-actieve i/o-taak threads verwerken|Count|Gemiddeld|Het aantal niet-actieve threads voor i/o-taken in de thread-pool.|
|ProcessingPoolIdleNonIOThreads|Threads: Groep niet-actieve niet-I/O-threads verwerken|Count|Gemiddeld|Het aantal niet-actieve threads in de thread-pool toegewezen aan niet-I/O-taken.|
|QueryPoolIdleThreads|Threads: Query groep niet-actieve threads|Count|Gemiddeld|Het aantal niet-actieve threads voor i/o-taken in de thread-pool.|
|QueryPoolJobQueueLength|Threads: Wachtrijlengte taak query uitvoeren|Count|Gemiddeld|Het aantal taken in de wachtrij van de threadgroep query.|
|ShortParsingBusyThreads|Threads: Korte bezet threads parseren|Count|Gemiddeld|Het aantal bezet threads in de korte parseren thread-groep.|
|ShortParsingIdleThreads|Threads: Korte bij het parseren van niet-actieve threads|Count|Gemiddeld|Het aantal niet-actieve threads in de korte parseren thread-groep.|
|ShortParsingJobQueueLength|Threads: Korte wachtrijlengte taak parseren|Count|Gemiddeld|Het aantal taken in de wachtrij van de threadgroep voor korte parseren.|
|TotalConnectionFailures|Totaal aantal verbindingsfouten|Count|Gemiddeld|Totaal aantal mislukte verbindingspogingen.|
|TotalConnectionRequests|Totaal aantal verbindingsaanvragen|Count|Gemiddeld|Totaal aantal verbindingsaanvragen. |

## <a name="next-steps"></a>Volgende stappen
[Bewaking in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md)   
[Metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)   
[Metrische gegevens in Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931930.aspx)