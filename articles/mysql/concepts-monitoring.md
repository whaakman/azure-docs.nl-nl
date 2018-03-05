---
title: Bewaken in Azure-Database voor MySQL
description: In dit artikel beschrijft de metrische gegevens voor bewaking en waarschuwingen voor de Azure-Database voor MySQL, met inbegrip van CPU, limieten, opslag en verbindingsstatistieken.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 7ecfb8151cd81fb588f964fdfa3a74aacab24874
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="monitoring-in-azure-database-for-mysql"></a>Bewaken in Azure-Database voor MySQL
Het bewaken van gegevens over uw servers, kunt u problemen op te optimaliseren voor uw workload. Azure MySQL-Database biedt verschillende metrische gegevens die geven inzicht in het gedrag van de resources die de MySQL-server te ondersteunen. 

## <a name="metrics"></a>Metrische gegevens
Alle Azure metrische gegevens hebben een frequentie van één minuut en elke metriek biedt 30 dagen van de geschiedenis. 

U kunt waarschuwingen configureren op de metrische gegevens. Zie voor stapsgewijze instructies [waarschuwingen instellen](howto-alert-on-metric.md). 

Andere taken omvatten het instellen van automatische acties, geavanceerde analyses uitvoeren en archiveren van de geschiedenis. Zie voor meer informatie de [overzicht van Azure metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Lijst met metrische gegevens
Deze metrische gegevens zijn beschikbaar voor Azure-Database voor MySQL:

|Gegevens|Metrische weergavenaam|Eenheid|Beschrijving|
|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Het percentage van CPU in gebruik.|
|compute_limit|COMPUTE eenheid limiet|Count|Deze server kunt u het maximum aantal compute-eenheden|
|compute_consumption_percent|Percentage van de eenheid berekenen|Procent|Het percentage van de compute-eenheden gebruikt buiten de server's maximale.|
|memory_percent|Percentage geheugen|Procent|Het percentage gebruikt geheugen.|
|io_consumption_percent|I/o-percentage|Procent|Het percentage IO in gebruik.|
|storage_percent|Opslagpercentage|Procent|Het percentage van de opslag die worden gebruikt buiten de server's maximale.|
|storage_used|Gebruikte opslag|Bytes|De hoeveelheid opslagruimte in gebruik. De opslag die wordt gebruikt door de service bevat de bestanden van de database, transactielogboeken en de server-Logboeken.|
|storage_limit|Opslaglimiet bereikt|Bytes|De maximale opslagcapaciteit voor deze server.|
|active_connections|Totaal aantal actieve verbindingen|Count|Het aantal actieve verbindingen met de server.|
|connections_failed|Totaal aantal mislukte verbindingen|Count|Het aantal mislukte verbindingen met de server.|


## <a name="next-steps"></a>Volgende stappen
- Zie voor stapsgewijze instructies [waarschuwingen instellen](howto-alert-on-metric.md). 
- Zie voor meer informatie over de toegang tot en met de Azure-portal, de REST-API of de CLI metrische gegevens exporteren, de [overzicht van Azure metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
