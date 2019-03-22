---
title: Azure SQL Data Warehouse-beheer en controle - activiteit, Resourcegebruik query | Microsoft Docs
description: Meer informatie over welke mogelijkheden zijn beschikbaar voor het beheren en controleren van Azure SQL Data Warehouse. Gebruik de Azure-portal en de dynamische beheerweergaven (DMV's) om te begrijpen van de query-activiteiten en het Resourcegebruik van uw datawarehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 03/21/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 69b33b9629863702d576eac39fb4097a2bff2e57
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58337563"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-sql-data-warehouse"></a>Controleren van de resource-gebruik en query-activiteit in Azure SQL Data Warehouse
Azure SQL Data Warehouse biedt een uitgebreide controle-ervaring in Azure portal om inzichten te kunnen verkrijgen voor uw datawarehouse-workload. De Azure portal is het aanbevolen hulpmiddel bij het bewaken van uw datawarehouse, aangezien deze configureerbare bewaarperioden, waarschuwingen, aanbevelingen, en aanpasbare grafieken en dashboards voor metrische gegevens en Logboeken biedt. Via de portal kunt u integreren met andere Azure-bewakingsservices zoals Operations Management Suite (OMS) en Azure Monitor (Logboeken) voor een holistische controle-ervaring voor het niet alleen uw datawarehouse, maar ook uw hele Azure analytics platform voor een geïntegreerde ervaring voor de bewaking. Deze documentatie wordt beschreven welke mogelijkheden voor bewaking zijn beschikbaar om te optimaliseren en beheren van uw platform voor streaminganalyse met SQL Data Warehouse. 

## <a name="resource-utilization"></a>Resourcegebruik 
De volgende metrische gegevens zijn beschikbaar in de Azure-portal voor SQL Data Warehouse. Deze metrische gegevens zijn opgehaald via de [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics).

> [!NOTE]
> Vanaf November 2018 is het technische team is een probleem veroorzaakt door CPU-percentage en gegevens-IO-percentage en underreport-adressering. Dit zorgt ervoor dat de gebruikte DWU en het percentage aan underreport ook. 

| Naam van meetwaarde                           | Description     | Aggregatietype |
| --------------------------------------- | ---------------- | --------------------------------------- |
| CPU-percentage                          | CPU-gebruik voor alle knooppunten voor het datawarehouse | Maximum      |
| Gegevens-I/O-percentage                      | I/o-gebruik voor alle knooppunten voor het datawarehouse | Maximum   |
| Geslaagde verbindingen                  | Aantal geslaagde verbindingen met de gegevens | Totaal            |
| Mislukte verbindingen                      | Aantal mislukte verbindingen naar het datawarehouse | Totaal            |
| Geblokkeerd door Firewall                     | Aantal aanmeldingen naar het datawarehouse dat is geblokkeerd | Totaal            |
| DWU-limiet                              | Serviceniveaudoelstelling van het datawarehouse | Maximum   |
| DWU-percentage                          | Maximale tussen CPU-percentage en gegevens-IO-percentage | Maximum   |
| Gebruikte DWU                                | DWU-limiet * DWU-percentage | Maximum   |
| Percentage treffers in cache | (treffers in de cache / ontbreekt in de cache) * 100 waarbij treffers in cache is de som van alle columnstore-segmenten treffers in de lokale SSD-cache en Cachemisser de columnstore-segmenten missers in de lokale SSD-cache bij elkaar opgeteld voor alle knooppunten | Maximum |
| Percentage gebruikte cache | (cache gebruikt / capaciteit in de cache) * 100 waar cache die wordt gebruikt, is de som van alle bytes in de lokale SSD-cache voor alle knooppunten en capaciteit van de cache is de som van de opslagcapaciteit van de lokale SSD voor alle knooppunten in de cache | Maximum |
| Lokale tempdb-percentage | Lokale tempdb-gebruik op alle rekenknooppunten - waarden om de vijf minuten worden verzonden | Maximum |

## <a name="query-activity"></a>Queryactiviteit
De service biedt voor een programmatische ervaring bij het bewaken van SQL Data Warehouse via T-SQL, een set van dynamische beheerweergaven (DMV's). Deze weergaven zijn handig wanneer u actief oplossen van problemen en het identificeren van knelpunten met uw werkbelasting.

Als u wilt weergeven in de lijst met DMV's die SQL Data Warehouse biedt, verwijzen naar dit [documentatie](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views#sql-data-warehouse-dynamic-management-views-dmvs). 

## <a name="metrics-and-diagnostics-logging"></a>Metrische gegevens en logboekregistratie van diagnostische gegevens
Logboeken en metrische gegevens kunnen worden geëxporteerd naar Azure Monitor, specifiek de [logboeken van Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) onderdeel en via een programma kunnen worden benaderd met [query's bijgehouden](https://docs.microsoft.com/azure/log-analytics/log-analytics-tutorial-viewdata). De latentie van het logboek voor SQL Data Warehouse is ongeveer 10-15 minuten. Ga naar de volgende documentatie voor meer informatie over de factoren die invloed hebben op de latentie.


## <a name="next-steps"></a>Volgende stappen
De volgende handleidingen worden algemene scenario's beschreven en use-cases bij het controleren en beheren van uw datawarehouse:

- [Bewaken van uw datawarehouse-workload met DMV 's](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor)

