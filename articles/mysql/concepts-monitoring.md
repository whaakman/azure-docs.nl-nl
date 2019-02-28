---
title: Bewaking in Azure Database for MySQL
description: Dit artikel beschrijft de metrische gegevens voor bewaking en waarschuwingen voor Azure Database voor MySQL, met inbegrip van CPU, opslag en verbinding statistieken.
author: rachel-msft
ms.author: raagyema
ms.service: mysql
ms.topic: conceptual
ms.date: 11/05/2018
ms.openlocfilehash: 05af196d115376d744df1976c6a3d9fe9cab74e0
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56958895"
---
# <a name="monitoring-in-azure-database-for-mysql"></a>Bewaking in Azure Database for MySQL
Gegevens over uw servers te controleren, kunt u problemen op te optimaliseren voor uw workload. Azure Database for MySQL biedt verschillende metrische gegevens inzicht in het gedrag van uw server geven.

## <a name="metrics"></a>Metrische gegevens
Alle metrische gegevens van Azure hebben een frequentie van één minuut en elke metrische waarde biedt 30 dagen van de geschiedenis. U kunt waarschuwingen configureren op de metrische gegevens. Zie voor stapsgewijze instructies [over het instellen van waarschuwingen](howto-alert-on-metric.md). Andere taken omvatten het instellen van geautomatiseerde acties, het uitvoeren van geavanceerde analyses en het archiveren van geschiedenis. Zie voor meer informatie de [overzicht van metrische gegevens van Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Overzicht van metrische gegevens
Deze metrische gegevens zijn beschikbaar voor Azure Database voor MySQL:

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Description|
|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Het percentage van CPU gebruikt.|
|memory_percent|Percentage geheugen|Procent|Het percentage gebruikt geheugen.|
|io_consumption_percent|I/o-percentage|Procent|Het percentage i/o in gebruik.|
|storage_percent|Opslagpercentage|Procent|Het percentage van de opslag die buiten de server wordt gebruikt's maximaal.|
|storage_used|Gebruikte opslag|Bytes|De hoeveelheid opslagruimte wordt gebruikt. De opslag die wordt gebruikt door de service omvat bestanden van de database, transactielogboeken en de serverlogboeken.|
|serverlog_storage_percent|Server Log opslag procent|Procent|Het percentage van de server logboekopslag gebruikt buiten maximale serveropslag logboek van de server.|
|serverlog_storage_usage|Server logboekopslag gebruikt|Bytes|De hoeveelheid opslag van de server-logboek in gebruik.|
|serverlog_storage_limit|Limiet voor opslag van server-logboek|Bytes|De maximale server logboekopslag voor deze server.|
|storage_limit|Limiet voor opslag|Bytes|De maximale opslag voor deze server.|
|active_connections|Actieve verbindingen|Count|Het aantal actieve verbindingen met de server.|
|connections_failed|Mislukte verbindingen|Count|Het aantal mislukte verbindingen met de server.|
|seconds_behind_master|Vertraging van replicatie in een paar seconden|Count|Het aantal seconden dat de replica-server is achtergebleven op basis van de hoofd-server.|
|network_bytes_egress|Netwerk uit|Bytes|Uitgaand netwerkverkeer voor actieve verbindingen.|
|network_bytes_ingress|Netwerk in|Bytes|Netwerk In voor de actieve verbindingen.|
|backup_storage_used|Back-upopslag gebruikt|Bytes|De hoeveelheid back-upopslag gebruikt.|

## <a name="server-logs"></a>Serverlogboeken
U kunt langzame query's op uw server logboekregistratie inschakelen. Deze logboeken zijn ook beschikbaar via Azure diagnostische logboeken in Logboeken, Event Hubs en Storage-Account van Azure Monitor. Voor meer informatie over logboekregistratie, gaat u naar de [serverlogboeken](concepts-server-logs.md) pagina.

## <a name="next-steps"></a>Volgende stappen
- Zie [over het instellen van waarschuwingen](howto-alert-on-metric.md) voor hulp bij het maken van een waarschuwing voor metrische gegevens.
- Zie voor meer informatie over het openen en exporteren van metrische gegevens met behulp van de Azure portal, de REST-API of de CLI de [overzicht van metrische gegevens van Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- Lees onze blog op [aanbevolen procedures voor het bewaken van uw server](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-mysql-monitoring/).
