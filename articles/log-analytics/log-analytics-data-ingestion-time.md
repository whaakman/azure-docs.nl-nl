---
title: Tijd van de gegevens opnemen in Azure Log Analytics | Microsoft Docs
description: Legt uit de verschillende factoren die invloed hebben op latentie in het verzamelen van gegevens in Azure Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2018
ms.author: bwren
ms.openlocfilehash: f40c8ed7eb6bfae958b3b57c4b7d525963ab9741
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955240"
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
- IIS-logboeken en aangepaste logboeken worden verzameld als hun timestamp wordt gewijzigd. Voor IIS-logboeken, dit wordt beïnvloed door de [rollover schema dat is geconfigureerd voor IIS](log-analytics-data-sources-iis-logs.md). 
- Oplossing voor Active Directory-replicatie wordt de evaluatie van elke vijf dagen uitgevoerd terwijl de oplossing Active Directory-evaluatie wordt uitgevoerd voor een wekelijkse beoordeling van uw Active Directory-infrastructuur. De agent verzamelt deze logboeken alleen wanneer de evaluatie is voltooid.

### <a name="agent-upload-frequency"></a>De uploadfrequentie agent
Om ervoor te zorgen voor Log Analytics-agent is lichtgewicht, wordt de agent buffert logboeken en ze regelmatig worden geüpload naar Log Analytics. Uploaden frequentie varieert tussen 30 seconden en 2 minuten, afhankelijk van het type gegevens. De meeste gegevens worden geüpload in onder 1 minuut. Netwerkomstandigheden mogelijk negatieve invloed hebben op de latentie van deze gegevens naar Log Analytics punt bereikt.

### <a name="azure-logs-and-metrics"></a>Azure-logboeken en metrische gegevens 
Gegevens van een activiteitenlogboek duurt circa 5 minuten beschikbaar in Log Analytics. 1-5 minuten beschikbaar zijn, afhankelijk van de Azure-service kan duren voordat gegevens van diagnostische logboeken en metrische gegevens. Het duurt vervolgens een extra 30 tot 60 seconden voor logboeken en de 3 minuten gedurende metrische gegevens voor de gegevens worden verzonden naar Log Analytics punt.

### <a name="management-solutions-collection"></a>Management solutions verzameling
Sommige oplossingen geen hun gegevens worden verzameld uit een agent en een methode voor gebruikersstatusverzameling die resulteert in extra latentie kunnen gebruiken. Een aantal oplossingen voor het verzamelen van gegevens met regelmatige tussenpozen zonder dat wordt geprobeerd near-real-time-verzameling. Specifieke voorbeelden omvatten het volgende:

- Office 365-oplossing worden opgevraagd met behulp van de Office 365-activiteit-API, die op dit moment biedt geen garanties van eventuele latentie near-real-time-activiteitenlogboeken.
- Windows Analytics-oplossingen (bijvoorbeeld Updatevereisten) gegevens worden verzameld door de oplossing in een dagelijkse frequentie.

Raadpleeg de documentatie voor elke oplossing om te bepalen van de verzamelingsfrequentie.

### <a name="pipeline-process-time"></a>Tijd van de pijplijn-proces
Wanneer records in logboek registreren in Log Analytics-pijplijn zijn opgenomen, wordt deze zijn geschreven naar de tijdelijke opslag om te controleren of isolatie van tenants en om ervoor te zorgen dat gegevens niet verloren gaan. Dit proces wordt doorgaans 5 tot 15 seconden. Sommige oplossingen implementeren zwaardere algoritmen voor het samenvoegen van gegevens en inzichten worden afgeleid omdat gegevensstromen. Bijvoorbeeld, aggregeert de Netwerkprestatiemeter binnenkomende gegevens gedurende 3 minuten durende intervallen, effectief toe te voegen 3 minuten latentie. Een ander proces dat wordt toegevoegd latentie is het proces die verantwoordelijk is voor aangepaste logboeken. In sommige gevallen kan dit proces enkele minuten met een latentie van toevoegen aan de logboeken die worden verzameld van bestanden door de agent.

### <a name="new-custom-data-types-provisioning"></a>Nieuwe aangepaste gegevenstypen inrichten
Wanneer een nieuw type aangepaste gegevens is gemaakt op basis van een [aangepast logboek](../log-analytics/log-analytics-data-sources-custom-logs.md) of de [Data Collector API](../log-analytics/log-analytics-data-collector-api.md), een toegewezen opslagcontainer wordt gemaakt. Dit is een eenmalige overhead die wordt uitgevoerd alleen op de eerste weergave van dit gegevenstype.

### <a name="surge-protection"></a>Piek-beveiliging
De hoogste prioriteit van Log Analytics is om ervoor te zorgen dat er geen gegevens van de klant verbroken is, zodat het systeem beschikt over ingebouwde beveiliging voor gegevens pieken. Dit omvat buffers om ervoor te zorgen dat zelfs bij zeer grote belasting kan het systeem blijft gewoon functioneren. Bij normale belasting kan deze besturingselementen toevoegen van minder dan een minuut, maar in extreme omstandigheden en fouten die ze kunnen veel tijd en gegevens tegelijkertijd toevoegen veilig is.

### <a name="indexing-time"></a>Indexeren van tijd
Er is een ingebouwde taakverdeling voor elk platform voor Big Data tussen analytics en geavanceerde zoekfuncties in plaats van direct toegang tot de gegevens. Azure Log Analytics kunt u krachtige query's uitvoeren op miljarden records en resultaten krijgt binnen een paar seconden. Dit wordt mogelijk gemaakt, omdat de infrastructuur van de gegevens tijdens de opname aanzienlijk worden getransformeerd en slaat ze op in de unieke compacte structuur. Het systeem buffert de gegevens tot voldoende is beschikbaar voor het maken van deze structuren. Dit moet worden voltooid voordat de logboekrecord wordt weergegeven in zoekresultaten.

Dit proces momenteel ongeveer 5 minuten plaats wanneer er weinig volume van gegevens, maar minder tijd hogere tarief voor gegevens. Dit lijkt onlogisch, maar dit proces kan optimalisatie van de latentie voor werkbelastingen voor productie met hoog volume.



## <a name="checking-ingestion-time"></a>Opname-tijd controleren
U kunt de **Heartbeat** tabel om een schatting van de latentie voor gegevens van agents. Aangezien Heartbeat wordt verzonden, één keer per minuut, het verschil tussen de huidige tijd en de laatste heartbeat-record in het ideale geval worden zo dicht bij een minuut mogelijk.

Gebruik de volgende query om de computers met de hoogste latentie weer te geven.

    Heartbeat 
    | summarize IngestionTime = now() - max(TimeGenerated) by Computer 
    | top 50 by IngestionTime asc

 
Gebruik de volgende query in grote omgevingen geven een overzicht van de latentie voor verschillende percentage van totaal aantal computers.

    Heartbeat 
    | summarize IngestionTime = now() - max(TimeGenerated) by Computer 
    | summarize percentiles(IngestionTime, 50,95,99)



## <a name="next-steps"></a>Volgende stappen
* Lees de [Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) voor Log Analytics.

