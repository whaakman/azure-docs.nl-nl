---
title: Uw cache Gen2 optimaliseren | Microsoft Docs
description: Informatie over het bewaken van uw Gen2-cache met behulp van de Azure portal.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.subservice: performance
ms.topic: how-to
ms.date: 09/06/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 51c433248feed615684ab5a1273826135d3dea3d
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55456575"
---
# <a name="how-to-monitor-the-gen2-cache"></a>De cache Gen2 bewaken
De opslagarchitectuur Gen2 wordt automatisch de meest aangevraagde columnstore-segmenten in een cache op basis van SSD's die zijn ontworpen voor datawarehouses Gen2 NVMe lagen. Betere prestaties wordt gerealiseerd wanneer uw query's halen segmenten die die zich in de cache bevinden zijn. Dit artikel wordt beschreven hoe u controleert en prestaties van trage query's op te lossen door te bepalen of uw werkbelasting is optimaal gebruik te maken van de cache Gen2.  
## <a name="troubleshoot-using-the-azure-portal"></a>Problemen oplossen met behulp van de Azure-portal
U kunt Azure Monitor gebruiken om Gen2 cache metrische gegevens voor het oplossen van prestaties van query's weer te geven. Eerst Ga naar de Azure portal en klik op de Monitor:

![Azure Monitor](./media/sql-data-warehouse-cache-portal/cache_0.png)

Selecteer de knop metrische gegevens en vul de **abonnement**, **Resource** **groep**, **resourcetype**, en **Resource naam** van uw datawarehouse.

Zijn de belangrijkste metrische gegevens voor het oplossen van de cache Gen2 **percentage treffers in de Cache** en **Cache gebruikt percentage**. Configureer de Azure grafiek met metrische gegevens om deze twee metrische gegevens weer te geven.

![Metrische cachegegevens](./media/sql-data-warehouse-cache-portal/cache_1.png)


## <a name="cache-hit-and-used-percentage"></a>Cache treffers en percentage gebruikt
De matrix die hieronder worden beschreven scenario's op basis van de waarden van de cache metrische gegevens:

|                                | **Percentage treffers in de hoge Cache** | **Percentage treffers in de Cache van de laag** |
| :----------------------------: | :---------------------------: | :--------------------------: |
| **Hoog percentage van de Cache die wordt gebruikt** |          Scenario 1           |          Scenario 2          |
| **Percentage van de laag Cache die wordt gebruikt**  |          Scenario 3           |          Scenario 4          |

**Scenario 1:** U gebruik optimaal van uw cache. [Problemen oplossen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor) andere gebieden die mogelijk uw query's vertragen.

**Scenario 2:** Uw huidige werkset van de gegevens niet passen in de cache die zorgt ervoor een klein aantal dat cachetreffers percentage vanwege fysieke leesbewerkingen. Houd rekening met omhoog schalen van het prestatieniveau en uw workload om in te vullen van de cache opnieuw.

**Scenario 3:** Is het waarschijnlijk dat uw query traag wordt uitgevoerd vanwege redenen die niet gerelateerd aan de cache. [Problemen oplossen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor) andere gebieden die mogelijk uw query's vertragen. U kunt ook overwegen [verkleint uw exemplaar](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor) uw cache verkleinen om kosten te besparen. 

**Scenario 4:** Moest u een cold cache die mogelijk de reden waarom uw query traag is. Houd rekening met uw query opnieuw uitvoeren als uw gegevensset werken moet nu in in de cache. 

**Belangrijk: Als percentage treffers in de cache of als percentage van de cache die wordt gebruikt niet wordt bijgewerkt na het opnieuw uit te voeren in uw werkbelasting, kan al uw werkset zich in het geheugen. Houd er rekening mee alleen geclusterde columnstore tabellen in de cache zijn opgeslagen.**

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over algemene query-prestaties afstemmen, [bewaak de queryuitvoering](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor#monitor-query-execution).


<!--Image references-->

<!--Article references-->
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[System views]: ./sql-data-warehouse-reference-tsql-system-views.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Investigating queries waiting for resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx
