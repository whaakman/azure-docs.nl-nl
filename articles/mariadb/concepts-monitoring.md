---
title: Bewaking in Azure Database voor MariaDB
description: Dit artikel beschrijft de metrische gegevens voor bewaking en waarschuwingen voor Azure Database voor MariaDB, met inbegrip van CPU, opslag en verbinding statistieken.
services: mariadb
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mariadb
ms.topic: article
ms.date: 11/10/2018
ms.openlocfilehash: 3daa4eb07e626e53d9106abf978174f505e785ab
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2018
ms.locfileid: "51516172"
---
# <a name="monitoring-in-azure-database-for-mariadb"></a>Bewaking in Azure Database voor MariaDB
Gegevens over uw servers te controleren, kunt u problemen op te optimaliseren voor uw workload. Azure Database voor MariaDB biedt verschillende metrische gegevens inzicht in het gedrag van uw server geven.

## <a name="metrics"></a>Metrische gegevens
Alle metrische gegevens van Azure hebben een frequentie van één minuut en elke metrische waarde biedt 30 dagen van de geschiedenis. U kunt waarschuwingen configureren op de metrische gegevens. Andere taken omvatten het instellen van geautomatiseerde acties, het uitvoeren van geavanceerde analyses en het archiveren van geschiedenis. Voor meer informatie, Zie de [Azure Metrics Overview] (.. /Monitoring-and-Diagnostics/Monitoring-Overview-metrics.MD).

<!--For step by step guidance, see [How to set up alerts](howto-alert-on-metric.md). -->

### <a name="list-of-metrics"></a>Overzicht van metrische gegevens
Deze metrische gegevens zijn beschikbaar voor Azure Database voor MariaDB:

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Beschrijving|
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

<!-- - See [How to set up alerts](howto-alert-on-metric.md) for guidance on creating an alert on a metric.-->