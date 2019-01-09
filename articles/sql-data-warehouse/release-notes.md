---
title: Azure SQL Data Warehouse opmerkingen bij de Release | Microsoft Docs
description: Opmerkingen bij de release voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 11/11/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: a4737f4308f49547178bb216b90589c596f604c9
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118435"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Releaseopmerkingen voor Azure SQL Data Warehouse

Azure SQL Data Warehouse is een cloud-gebaseerde Enterprise Data Warehouse (EDW) die gebruikmaakt van uiterst (Massively Parallel Processing) als u wilt snel complexe query's uitvoeren voor petabytes aan gegevens. SQL Data Warehouse gebruiken als belangrijkste onderdeel van een big data-oplossing. Big data importeren in SQL Data Warehouse met eenvoudige PolyBase T-SQL-query's en vervolgens de kracht van MPP gebruiken om analyses met hoge prestaties uit te voeren. Door te integreren en analyseren, wordt Data Warehouse de centrale bron voor het verkrijgen van zakelijke inzichten door uw bedrijf.

Klik op de onderstaande koppelingen voor meer informatie over de nieuwe functies en verbeteringen die u in de meest recente versie van Azure SQL Data Warehouse verwachten kunt. U kunt verwachten dat voor het ontvangen van deze service-updates tijdens de geïdentificeerde onderhoudsplanning.

- [December 2018](./release-notes-december-2018.md)
- [Oktober 2018](./release-notes-october-2018.md)
- [September 2018](./release-notes-september-2018.md)
- [Augustus 2018](./release-notes-august-2018.md)
- [Juli 2018](./release-notes-july-2018.md)
- [Juni 2018](./release-notes-june-2018.md)
- [Mei 2018](./release-notes-may-2018.md)

## <a name="checking-the-code-version-that-has-been-applied-to-your-data-warehouse"></a>Het controleren van de codeversie die is toegepast op uw datawarehouse

Om te controleren welke versie is aan uw datawarehouse zijn toegepast. Verbinding met uw datawarehouse via SSMS en voer de volgende syntaxis om terug te keren van de huidige versie van SQL Data Warehouse.

```sql
SELECT @@VERSION AS 'SQL Data Warehouse';
```

Voorbeelduitvoer: ![Versie van SQL Data Warehouse](./media/release-notes/sql_data_warehouse_version.png)

Gebruik de datum die is geïdentificeerd als u wilt controleren welke versie is toegepast op uw Azure SQL Data Warehouse. 


## <a name="next-steps"></a>Volgende stappen
- [Meer informatie](https://docs.microsoft.com/azure/sql-data-warehouse/viewing-maintenance-schedule) over het weergeven van een onderhoudsplanning. 
- [Meer informatie](https://docs.microsoft.com/azure/sql-data-warehouse/changing-maintenance-schedule) over het wijzigen van een onderhoudsplanning.
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/alert-metric) over het maken, weergeven en beheren van waarschuwingen met behulp van Azure Monitor.
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) over webhookacties voor waarschuwingsregels.
- [Meer informatie](https://docs.microsoft.com/azure/service-health/service-health-overview) over Azure Service Health
