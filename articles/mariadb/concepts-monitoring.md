---
title: Bewaking in Azure Database voor MariaDB
description: Dit artikel beschrijft de metrische gegevens voor bewaking en waarschuwingen voor Azure Database voor MariaDB, met inbegrip van CPU, opslag en verbinding statistieken.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 11/10/2018
ms.openlocfilehash: 2ad641ae054f9542ec1ef42f5ebbe724ba4ecf87
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54354022"
---
# <a name="monitoring-in-azure-database-for-mariadb"></a>Bewaking in Azure Database voor MariaDB
Gegevens over uw servers te controleren, kunt u problemen op te optimaliseren voor uw workload. Azure Database voor MariaDB biedt verschillende metrische gegevens inzicht in het gedrag van uw server geven.

## <a name="metrics"></a>Metrische gegevens
Alle metrische gegevens van Azure hebben een frequentie van één minuut en elke metrische waarde biedt 30 dagen van de geschiedenis. U kunt waarschuwingen configureren op de metrische gegevens. Andere taken omvatten het instellen van geautomatiseerde acties, het uitvoeren van geavanceerde analyses en het archiveren van geschiedenis. Voor meer informatie, Zie de [Azure Metrics Overview] (.. /Monitoring-and-Diagnostics/Monitoring-Overview-metrics.MD).

Zie voor stapsgewijze instructies [over het instellen van waarschuwingen](howto-alert-metric.md).

### <a name="list-of-metrics"></a>Overzicht van metrische gegevens
Deze metrische gegevens zijn beschikbaar voor Azure Database voor MariaDB:

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
|network_bytes_egress|Netwerk uit|Bytes|Uitgaand netwerkverkeer voor actieve verbindingen.|
|network_bytes_ingress|Netwerk in|Bytes|Netwerk In voor de actieve verbindingen.|

## <a name="server-logs"></a>Serverlogboeken
U kunt langzame query's op uw server logboekregistratie inschakelen. Voor meer informatie over logboekregistratie, gaat u naar de [serverlogboeken](concepts-server-logs.md) pagina.

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over het openen en exporteren van metrische gegevens met behulp van de Azure portal, de REST-API of de CLI de [overzicht van metrische gegevens van Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
 - Zie [over het instellen van waarschuwingen](howto-alert-metric.md) voor hulp bij het maken van een waarschuwing voor metrische gegevens.
