---
title: Bewaking in Azure Database voor MariaDB
description: Dit artikel beschrijft de metrische gegevens voor bewaking en waarschuwingen voor Azure Database voor MariaDB, met inbegrip van CPU, opslag en verbinding statistieken.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/12/2019
ms.openlocfilehash: fb998edffed290bb7bc59945163f0fd48c55cbf5
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612529"
---
# <a name="monitoring-in-azure-database-for-mariadb"></a>Bewaking in Azure Database voor MariaDB
Gegevens over uw servers te controleren, kunt u problemen op te optimaliseren voor uw workload. Azure Database voor MariaDB biedt verschillende metrische gegevens inzicht in het gedrag van uw server geven.

## <a name="metrics"></a>Metrische gegevens
Alle metrische gegevens van Azure hebben een frequentie van één minuut en elke metrische waarde biedt 30 dagen van de geschiedenis. U kunt waarschuwingen configureren op de metrische gegevens. Andere taken omvatten het instellen van geautomatiseerde acties, het uitvoeren van geavanceerde analyses en het archiveren van geschiedenis. Zie voor meer informatie de [overzicht van metrische gegevens van Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Zie voor stapsgewijze instructies [over het instellen van waarschuwingen](howto-alert-metric.md).

### <a name="list-of-metrics"></a>Overzicht van metrische gegevens
Deze metrische gegevens zijn beschikbaar voor Azure Database voor MariaDB:

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Description|
|---|---|---|---|
|cpu_percent|CPU-percentage|Percent|Het percentage van CPU gebruikt.|
|memory_percent|Percentage geheugen|Percent|Het percentage gebruikt geheugen.|
|io_consumption_percent|I/o-percentage|Percent|Het percentage i/o in gebruik.|
|storage_percent|Opslagpercentage|Percent|Het percentage van de opslag die buiten de server wordt gebruikt's maximaal.|
|storage_used|Gebruikte opslag|Bytes|De hoeveelheid opslagruimte wordt gebruikt. De opslag die wordt gebruikt door de service omvat bestanden van de database, transactielogboeken en de serverlogboeken.|
|serverlog_storage_percent|Server Log opslag procent|Percent|Het percentage van de server logboekopslag gebruikt buiten maximale serveropslag logboek van de server.|
|serverlog_storage_usage|Server logboekopslag gebruikt|Bytes|De hoeveelheid opslag van de server-logboek in gebruik.|
|serverlog_storage_limit|Limiet voor opslag van server-logboek|Bytes|De maximale server logboekopslag voor deze server.|
|storage_limit|Limiet voor opslag|Bytes|De maximale opslag voor deze server.|
|active_connections|Actieve verbindingen|Count|Het aantal actieve verbindingen met de server.|
|connections_failed|Mislukte verbindingen|Count|Het aantal mislukte verbindingen met de server.|
|network_bytes_egress|Netwerk uit|Bytes|Uitgaand netwerkverkeer voor actieve verbindingen.|
|network_bytes_ingress|Netwerk in|Bytes|Netwerk In voor de actieve verbindingen.|

## <a name="server-logs"></a>Serverlogboeken

U kunt langzame query's op uw server logboekregistratie inschakelen. Deze logboeken zijn ook beschikbaar via Azure diagnostische logboeken in Logboeken, Event Hubs en Storage-Account van Azure Monitor. Voor meer informatie over logboekregistratie, gaat u naar de [serverlogboeken](concepts-server-logs.md) pagina.

## <a name="query-store"></a>Query Store

[Query Store](concepts-query-store.md) is een openbare preview-functie die boekrecords bijhoudt query prestaties gedurende een bepaalde tijd met inbegrip van runtime-statistieken opvragen en wacht gebeurtenissen. De functie zich blijft voordoen querygegevens over de runtimeprestaties in de **mysql** schema. U kunt de verzameling en opslag van gegevens via verschillende configuratie-knoppen beheren.

## <a name="query-performance-insight"></a>Inzicht in queryprestaties

[Query Performance Insight](concepts-query-performance-insight.md) werkt in combinatie met Query Store te bieden van visualisaties die toegankelijk is vanaf de Azure-portal. Deze diagrammen kunnen u belangrijke query's identificeren die prestaties. Query Performance Insight is in openbare preview en is toegankelijk in de **intelligente prestaties** sectie van uw Azure Database voor MariaDB-server portal-pagina.

## <a name="performance-recommendations"></a>Aanbevelingen voor prestaties

De [aanbevelingen voor prestaties](concepts-performance-recommendations.md) functie identificeert mogelijkheden voor verbetering van de prestaties van de werkbelastingen. De openbare preview-versie van de aanbevelingen voor prestaties beschikt u over aanbevelingen voor het maken van nieuwe indexen die u de mogelijkheid hebt voor het verbeteren van de prestaties van uw workloads. Om te produceren indexaanbevelingen, wordt de functie rekening gehouden met verschillende kenmerken van de database, met inbegrip van het schema en de werkbelasting, zoals gerapporteerd door de Query Store. Na de implementatie van elke aanbeveling prestaties, moeten klanten prestaties voor het evalueren van de impact van deze wijzigingen testen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het openen en exporteren van metrische gegevens met behulp van de Azure portal, de REST-API of de CLI de [overzicht van metrische gegevens van Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
  - Zie [over het instellen van waarschuwingen](howto-alert-metric.md) voor hulp bij het maken van een waarschuwing voor metrische gegevens.
