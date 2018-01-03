---
title: Herstellen van een Azure SQL datawarehouse (REST-API) | Microsoft Docs
description: REST-API-taken voor het herstellen van een Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: fca922c6-b675-49c7-907e-5dcf26d451dd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: 8739429342d3c8bbe0f09041976758276a9c3228
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Herstellen van een Azure SQL datawarehouse (REST-API)
> [!div class="op_single_selector"]
> * [Overzicht][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

In dit artikel leert u hoe u een Azure SQL Data Warehouse met behulp van de REST-API.

## <a name="before-you-begin"></a>Voordat u begint
**Controleer of de capaciteit van de DTU.** Elke SQL Data Warehouse wordt gehost door een SQL-server (bijvoorbeeld myserver.database.windows.net) met een standaard DTU-quotum.  Voordat u een SQL Data Warehouse herstellen kunt, controleren of de SQL server heeft onvoldoende resterende DTU-quotum voor de database wordt hersteld. Zie voor meer informatie over het DTU nodig berekenen of dat er meer DTU, [een wijziging DTU-quotum aanvragen][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Herstel de database van een actieve of onderbroken
Een database te herstellen:

1. De lijst van database herstelpunten zijn gemaakt met de herstelpunten voor Database Get-bewerking ophalen.
2. Het terugzetten starten met behulp van de [Create database terugzetten aanvraag] [ Create database restore request] bewerking.
3. De status van uw terugzetten bijhouden met behulp van de [bewerking databasestatus] [ Database operation status] bewerking.

> [!NOTE]
> Nadat het herstel is voltooid, kunt u de herstelde database configureren door [uw database na herstel configureren][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Een verwijderde database herstellen
Een verwijderde database herstellen:

1. Overzicht van de verwijderde databases met behulp van de [lijst terug te zetten databases verwijderd] [ List restorable dropped databases] bewerking.
2. Ophalen van de details voor de verwijderde database die u herstellen wilt met behulp van de [Get terug te zetten database verwijderd] [ Get restorable dropped database] bewerking.
3. Het terugzetten starten met behulp van de [Create database terugzetten aanvraag] [ Create database restore request] bewerking.
4. De status van uw terugzetten bijhouden met behulp van de [bewerking databasestatus] [ Database operation status] bewerking.

> [!NOTE]
> Voor het configureren van uw database nadat het herstel is voltooid. Zie [uw database na herstel configureren][Configure your database after recovery].
> 
> 

## <a name="next-steps"></a>Volgende stappen
Lees voor meer informatie over de zakelijke continuïteit functies van Azure SQL Database-versies, de [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Create database restore request]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Database operation status]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Get restorable dropped database]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[List restorable dropped databases]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
