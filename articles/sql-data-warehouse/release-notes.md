---
title: Azure SQL Data Warehouse opmerkingen bij de Release | Microsoft Docs
description: Opmerkingen bij de release voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 11/11/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 7a131429d8f70255e47efaac0c4b5fa99a6df7d6
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55460808"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Releaseopmerkingen voor Azure SQL Data Warehouse

Azure SQL Data Warehouse (SQL DW) is een cloud-gebaseerde zakelijke datawarehouse die gebruikmaakt van uiterst (Massively Parallel Processing) als u wilt snel complexe query's uitvoeren voor petabytes aan gegevens. SQL Data Warehouse gebruiken als belangrijkste onderdeel van een big data-oplossing. Big data importeren in SQL Data Warehouse met eenvoudige PolyBase T-SQL-query's en vervolgens de kracht van MPP gebruiken om analyses met hoge prestaties uit te voeren. Door te integreren en analyseren, wordt Data Warehouse de centrale bron voor het verkrijgen van zakelijke inzichten door uw bedrijf.

Klik op de onderstaande koppelingen voor meer informatie over de nieuwe functies en verbeteringen die u in de meest recente versie van Azure SQL Data Warehouse verwachten kunt. U kunt verwachten dat voor het ontvangen van deze service-updates tijdens de geïdentificeerde onderhoudsplanning.

- [Versie van SQL DW 10.0.10106.0](./release-notes-10-0-10106-0.md)
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

Voorbeelduitvoer: ![Versie van SQL Data Warehouse](./media/release-notes/dw-version.png)

Gebruik de versie die is geïdentificeerd als u wilt controleren welke versie is toegepast op uw Azure SQL Data Warehouse. 


## <a name="next-steps"></a>Volgende stappen
- [Meer informatie](https://docs.microsoft.com/azure/sql-data-warehouse/viewing-maintenance-schedule) over het weergeven van een onderhoudsplanning. 
- [Meer informatie](https://docs.microsoft.com/azure/sql-data-warehouse/changing-maintenance-schedule) over het wijzigen van een onderhoudsplanning.
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/alert-metric) over het maken, weergeven en beheren van waarschuwingen met behulp van Azure Monitor.
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) over webhookacties voor waarschuwingsregels.
- [Meer informatie](https://docs.microsoft.com/azure/service-health/service-health-overview) over Azure Service Health
