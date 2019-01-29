---
title: Controleren en afstemmen in Azure Database for PostgreSQL
description: Dit artikel wordt beschreven voor bewaking en afstemming van functies in Azure Database voor PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/05/2018
ms.openlocfilehash: c936d7946398183f0b0aac025503bb172bc147af
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55101002"
---
# <a name="monitor-and-tune"></a>Controleren en afstemmen
Gegevens over uw servers te controleren, kunt u problemen op te optimaliseren voor uw workload. Azure Database voor PostgreSQL biedt verschillende controle-opties te bieden inzicht in het gedrag van uw server.

## <a name="metrics"></a>Metrische gegevens
Azure Database voor PostgreSQL biedt verschillende metrische gegevens geven inzicht in het gedrag van de resources voor het ondersteunen van de PostgreSQL-server. Alle gegevens wordt verzonden met een frequentie van één minuut en maximaal 30 dagen van de geschiedenis heeft. U kunt waarschuwingen configureren op de metrische gegevens. Zie voor stapsgewijze instructies [over het instellen van waarschuwingen](howto-alert-on-metric.md). Andere taken omvatten het instellen van geautomatiseerde acties, het uitvoeren van geavanceerde analyses en het archiveren van geschiedenis. Zie voor meer informatie de [overzicht van metrische gegevens van Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Overzicht van metrische gegevens
Deze metrische gegevens zijn beschikbaar voor Azure Database voor PostgreSQL:

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Beschrijving|
|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Het percentage van CPU gebruikt.|
|memory_percent|Percentage geheugen|Procent|Het percentage gebruikt geheugen.|
|io_consumption_percent|I/o-percentage|Procent|Het percentage i/o in gebruik.|
|storage_percent|Opslagpercentage|Procent|Het percentage van de opslag die buiten de server wordt gebruikt's maximaal.|
|storage_used|Gebruikte opslag|Bytes|De hoeveelheid opslagruimte wordt gebruikt. De opslag die wordt gebruikt door de service omvat bestanden van de database, transactielogboeken en de serverlogboeken.|
|storage_limit|Limiet voor opslag|Bytes|De maximale opslag voor deze server.|
|serverlog_storage_percent|Server Log opslag procent|Procent|Het percentage van de server logboekopslag gebruikt buiten maximale serveropslag logboek van de server.|
|serverlog_storage_usage|Server logboekopslag gebruikt|Bytes|De hoeveelheid opslag van de server-logboek in gebruik.|
|serverlog_storage_limit|Limiet voor opslag van server-logboek|Bytes|De maximale server logboekopslag voor deze server.|
|active_connections|Actieve verbindingen|Count|Het aantal actieve verbindingen met de server.|
|connections_failed|Mislukte verbindingen|Count|Het aantal mislukte verbindingen met de server.|
|network_bytes_egress|Netwerk uit|Bytes|Uitgaand netwerkverkeer voor actieve verbindingen.|
|network_bytes_ingress|Netwerk in|Bytes|Netwerk In voor de actieve verbindingen.|
|backup_storage_used|Back-upopslag gebruikt|Bytes|De hoeveelheid back-upopslag gebruikt.|

## <a name="server-logs"></a>Serverlogboeken
U kunt logboekregistratie op uw server inschakelen. Deze logboeken zijn ook beschikbaar via Azure diagnostische logboeken in [Log Analytics](../azure-monitor/log-query/log-query-overview.md), Event Hubs en Storage-Account. Voor meer informatie over logboekregistratie, gaat u naar de [serverlogboeken](concepts-server-logs.md) pagina.

## <a name="query-store"></a>Query Store
[Query Store](concepts-query-store.md) is een openbare preview-functie die boekrecords bijhoudt query prestaties gedurende een bepaalde tijd met inbegrip van runtime-statistieken opvragen en wacht gebeurtenissen. De functie zich blijft voordoen querygegevens over de runtimeprestaties in een systeemdatabase met de naam **azure_sys** onder het query_store-schema. U kunt de verzameling en opslag van gegevens via verschillende configuratie-knoppen beheren.

## <a name="query-performance-insight"></a>Inzicht in queryprestaties
[Query Performance Insight](concepts-query-performance-insight.md) werkt in combinatie met Query Store te bieden van visualisaties die toegankelijk is vanaf de Azure-portal. Deze diagrammen kunnen u belangrijke query's identificeren die prestaties. Query Performance Insight is in openbare preview en is toegankelijk in de **ondersteuning en probleemoplossing** sectie van uw Azure Database for PostgreSQL-server portal-pagina.

## <a name="performance-recommendations"></a>Aanbevelingen voor prestaties
De [aanbevelingen voor prestaties](concepts-performance-recommendations.md) functie identificeert mogelijkheden voor verbetering van de prestaties van de werkbelastingen. De openbare preview-versie van de aanbevelingen voor prestaties beschikt u over aanbevelingen voor het maken van nieuwe indexen die u de mogelijkheid hebt voor het verbeteren van de prestaties van uw workloads. Om te produceren indexaanbevelingen, wordt de functie rekening gehouden met verschillende kenmerken van de database, met inbegrip van het schema en de werkbelasting, zoals gerapporteerd door de Query Store. Na de implementatie van elke aanbeveling prestaties, moeten klanten prestaties voor het evalueren van de impact van deze wijzigingen testen. 

## <a name="next-steps"></a>Volgende stappen
- Zie [over het instellen van waarschuwingen](howto-alert-on-metric.md) voor hulp bij het maken van een waarschuwing voor metrische gegevens.
- Zie voor meer informatie over het openen en exporteren van metrische gegevens met behulp van de Azure portal, de REST-API of de CLI de [overzicht van metrische gegevens van Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- Lees onze blog op [aanbevolen procedures voor het bewaken van uw server](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-postgresql-monitoring/).
