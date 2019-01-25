---
title: Tijd van de gegevens opnemen in Azure Log Analytics | Microsoft Docs
description: Legt uit de verschillende factoren die invloed hebben op latentie in het verzamelen van gegevens in Azure Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2019
ms.author: bwren
ms.openlocfilehash: 329472f3edee66db6b12e369ee8f944546ad4734
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54900439"
---
# <a name="data-ingestion-time-in-log-analytics"></a>Tijd van de gegevens opnemen in Log Analytics
Azure Log Analytics is een service voor grote schaal in Azure Monitor die duizenden klanten terabytes aan gegevens per maand verzenden in een groeiende tempo fungeert. Er zijn vaak vragen over de tijd die nodig zijn voor gegevens beschikbaar in Log Analytics nadat deze verzameld. In dit artikel wordt uitgelegd dat de verschillende factoren die invloed hebben op deze latentie.

## <a name="typical-latency"></a>Normale latentie
Latentie verwijst naar de tijd waarop gegevens worden gemaakt op het bewaakte systeem en de tijd die wordt geleverd beschikbaar om te analyseren in Log Analytics. De typische latentie voor het opnemen van gegevens in Log Analytics is tussen 2 en 5 minuten. De specifieke latentie voor een bepaalde gegevens varieert, afhankelijk van diverse factoren die hieronder wordt uitgelegd.


## <a name="factors-affecting-latency"></a>Factoren die invloed hebben op latentie
De totale opname-tijd voor een bepaalde set gegevens kan worden onderverdeeld in de volgende algemene gebieden ingedeeld. 

- Tijd van de agent - de tijd voor het detecteren van een gebeurtenis, verzamelen en deze vervolgens te verzenden naar Log Analytics-gegevensopname verwijzen als een logboekrecord. Dit proces wordt in de meeste gevallen afgehandeld door een agent.
- Tijd voor pipeline - de tijd voor de pijplijn opname voor het verwerken van de logboekrecord. Dit omvat het parseren van de eigenschappen van de gebeurtenis en mogelijk berekende gegevens toe te voegen.
- Indexering tijd – de tijd wordt gebruikt voor het opnemen van een logboekrecord in Log Analytics Big Data-archief.

Meer informatie over de verschillende latentie die is geïntroduceerd in dit proces worden hieronder beschreven.

### <a name="agent-collection-latency"></a>Agent verzameling latentie
Agents en oplossingen voor gebruik van verschillende strategieën voor het verzamelen van gegevens van een virtuele machine, die mogelijk invloed hebben op de latentie. Enkele specifieke voorbeelden omvatten het volgende:

- Windows-gebeurtenissen, syslog-gebeurtenissen en metrische gegevens voor prestaties worden onmiddellijk verzameld. Linux-prestatiemeteritems zijn gepeild intervallen van 30 seconden.
- IIS-logboeken en aangepaste logboeken worden verzameld als hun timestamp wordt gewijzigd. Voor IIS-logboeken, dit wordt beïnvloed door de [rollover schema dat is geconfigureerd voor IIS](../../azure-monitor/platform/data-sources-iis-logs.md). 
- Oplossing voor Active Directory-replicatie wordt de evaluatie van elke vijf dagen uitgevoerd terwijl de oplossing Active Directory-evaluatie wordt uitgevoerd voor een wekelijkse beoordeling van uw Active Directory-infrastructuur. De agent verzamelt deze logboeken alleen wanneer de evaluatie is voltooid.

### <a name="agent-upload-frequency"></a>De uploadfrequentie agent
Om ervoor te zorgen voor Log Analytics-agent is lichtgewicht, wordt de agent buffert logboeken en ze regelmatig worden geüpload naar Log Analytics. Uploaden frequentie varieert tussen 30 seconden en 2 minuten, afhankelijk van het type gegevens. De meeste gegevens worden geüpload in onder 1 minuut. Netwerkomstandigheden mogelijk negatieve invloed hebben op de latentie van deze gegevens naar Log Analytics punt bereikt.

### <a name="azure-activity-logs-diagnostic-logs-and-metrics"></a>Azure-activiteitenlogboeken, diagnostische logboeken en metrische gegevens
Azure data voegt extra tijd beschikbaar is op het punt voor de verwerking van Log Analytics:

- Gegevens uit logboeken met diagnostische gegevens 2-15 minuten duren, afhankelijk van de Azure-service. Zie de [onderstaande query](#checking-ingestion-time) deze latentie in uw omgeving onderzoeken
- Azure-platform metrische gegevens duren worden verzonden naar Log Analytics punt de 3 minuten.
- Gegevens van een activiteitenlogboek duurt ongeveer 10-15 minuten worden verzonden naar Log Analytics punt.

Zodra dit beschikbaar is op het punt, duurt het voordat gegevens extra 2-5 minuten beschikbaar moeten zijn voor het uitvoeren van query's.


### <a name="management-solutions-collection"></a>Management solutions verzameling
Sommige oplossingen geen hun gegevens worden verzameld uit een agent en een methode voor gebruikersstatusverzameling die resulteert in extra latentie kunnen gebruiken. Een aantal oplossingen voor het verzamelen van gegevens met regelmatige tussenpozen zonder dat wordt geprobeerd near-real-time-verzameling. Specifieke voorbeelden omvatten het volgende:

- Office 365-oplossing worden opgevraagd met behulp van de Office 365-activiteit-API, die op dit moment biedt geen garanties van eventuele latentie near-real-time-activiteitenlogboeken.
- Windows Analytics-oplossingen (bijvoorbeeld Updatevereisten) gegevens worden verzameld door de oplossing in een dagelijkse frequentie.

Raadpleeg de documentatie voor elke oplossing om te bepalen van de verzamelingsfrequentie.

### <a name="pipeline-process-time"></a>Tijd van de pijplijn-proces
Wanneer records in logboek registreren in Log Analytics-pijplijn zijn opgenomen, wordt deze zijn geschreven naar de tijdelijke opslag om te controleren of isolatie van tenants en om ervoor te zorgen dat gegevens niet verloren gaan. Dit proces wordt doorgaans 5 tot 15 seconden. Sommige oplossingen implementeren zwaardere algoritmen voor het samenvoegen van gegevens en inzichten worden afgeleid omdat gegevensstromen. Bijvoorbeeld, aggregeert de Netwerkprestatiemeter binnenkomende gegevens gedurende 3 minuten durende intervallen, effectief toe te voegen 3 minuten latentie. Een ander proces dat wordt toegevoegd latentie is het proces die verantwoordelijk is voor aangepaste logboeken. In sommige gevallen kan dit proces enkele minuten met een latentie van toevoegen aan de logboeken die worden verzameld van bestanden door de agent.

### <a name="new-custom-data-types-provisioning"></a>Nieuwe aangepaste gegevenstypen inrichten
Wanneer een nieuw type aangepaste gegevens is gemaakt op basis van een [aangepast logboek](data-sources-custom-logs.md) of de [Data Collector API](data-collector-api.md), een toegewezen opslagcontainer wordt gemaakt. Dit is een eenmalige overhead die wordt uitgevoerd alleen op de eerste weergave van dit gegevenstype.

### <a name="surge-protection"></a>Piek-beveiliging
De hoogste prioriteit van Log Analytics is om ervoor te zorgen dat er geen gegevens van de klant verbroken is, zodat het systeem beschikt over ingebouwde beveiliging voor gegevens pieken. Dit omvat buffers om ervoor te zorgen dat zelfs bij zeer grote belasting kan het systeem blijft gewoon functioneren. Bij normale belasting kan deze besturingselementen toevoegen van minder dan een minuut, maar in extreme omstandigheden en fouten die ze kunnen veel tijd en gegevens tegelijkertijd toevoegen veilig is.

### <a name="indexing-time"></a>Indexeren van tijd
Er is een ingebouwde taakverdeling voor elk platform voor Big Data tussen analytics en geavanceerde zoekfuncties in plaats van direct toegang tot de gegevens. Azure Log Analytics kunt u krachtige query's uitvoeren op miljarden records en resultaten krijgt binnen een paar seconden. Dit wordt mogelijk gemaakt, omdat de infrastructuur van de gegevens tijdens de opname aanzienlijk worden getransformeerd en slaat ze op in de unieke compacte structuur. Het systeem buffert de gegevens tot voldoende is beschikbaar voor het maken van deze structuren. Dit moet worden voltooid voordat de logboekrecord wordt weergegeven in zoekresultaten.

Dit proces momenteel ongeveer 5 minuten plaats wanneer er weinig volume van gegevens, maar minder tijd hogere tarief voor gegevens. Dit lijkt onlogisch, maar dit proces kan optimalisatie van de latentie voor werkbelastingen voor productie met hoog volume.



## <a name="checking-ingestion-time"></a>Opname-tijd controleren
Opname tijd kan variëren voor andere resources onder verschillende omstandigheden. Logboeken-query's kunt u bepaald gedrag van uw omgeving identificeren.

### <a name="ingestion-latency-delays"></a>Opname latentie vertragingen
U kunt de latentie van een bepaalde record meten door het vergelijken van het resultaat van de [ingestion_time()](/azure/kusto/query/ingestiontimefunction) functie de _TimeGenerated_ veld. Deze gegevens kunnen worden gebruikt met verschillende aggregaties te vinden hoe opnamelatentie zich gedraagt. Bekijk enkele percentiel van de tijd opname om inzichten te verkrijgen voor een grote hoeveelheid gegevens. 

Bijvoorbeeld, ziet de volgende query u welke computers had de hoogste tijd voor opname in de huidige dag: 

``` Kusto
Heartbeat
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95) by Computer 
| top 20 by percentile_E2EIngestionLatency_95 desc  
```
 
Als u inzoomen op het moment van opname voor een specifieke computer gedurende een bepaalde periode wilt, gebruikt u de volgende query ook visualisatie van de gegevens in een grafiek: 

``` Kusto
Heartbeat 
| where TimeGenerated > ago(24h) and Computer == "ContosoWeb2-Linux"  
| extend E2EIngestionLatencyMin = todouble(datetime_diff("Second",ingestion_time(),TimeGenerated))/60 
| summarize percentiles(E2EIngestionLatencyMin,50,95) by bin(TimeGenerated,30m) 
| render timechart  
```
 
Gebruik de volgende query om weer te geven van gegevensopname computertijd door het land, dat ze bevinden zich op die is gebaseerd op de IP-adres: 

``` Kusto
Heartbeat 
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95) by RemoteIPCountry 
```
 
Verschillende gegevenstypen die afkomstig zijn van de agent mogelijk verschillende opname latentie time, zodat de vorige query's kunnen worden gebruikt met andere typen. Gebruik de volgende query om te controleren van de tijd van de opname van verschillende Azure-services: 

``` Kusto
AzureDiagnostics 
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95) by ResourceProvider
```

### <a name="resources-that-stop-responding"></a>Resources die niet meer reageert 
In sommige gevallen kan een resource stoppen gegevens worden verzonden. Om te begrijpen als een resource is verzenden van gegevens of niet, kijken naar de meest recente record waarmee kan worden geïdentificeerd door de standaard _TimeGenerated_ veld.  

Gebruik de _Heartbeat_ tabel om te controleren of de beschikbaarheid van een virtuele machine, aangezien een heartbeat eens per minuut door de agent wordt verzonden. Gebruik de volgende query om de actieve computers die heartbeat onlangs nog niet gerapporteerd weer te geven: 

``` Kusto
Heartbeat  
| where TimeGenerated > ago(1d) //show only VMs that were active in the last day 
| summarize NoHeartbeatPeriod = now() - max(TimeGenerated) by Computer  
| top 20 by NoHeartbeatPeriod desc 
```

## <a name="next-steps"></a>Volgende stappen
* Lees de [Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) voor Log Analytics.

