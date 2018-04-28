---
title: Herstellen van een Azure SQL datawarehouse - REST-API | Microsoft Docs
description: Herstellen van een Azure SQL Data Warehouse met REST API's.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 2e1874fdf7c11d98d369072739c5937caffe6e96
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="restore-an-azure-sql-data-warehouse-with-rest-apis"></a>Herstellen van een Azure SQL datawarehouse met REST-API 's
Herstellen van een Azure SQL Data Warehouse met REST API's.

## <a name="before-you-begin"></a>Voordat u begint
**Controleer of de capaciteit van de DTU.** Elke SQL Data Warehouse wordt gehost door een logische SQL-server (bijvoorbeeld myserver.database.windows.net) met een standaard [DTU-quotum](../sql-database/sql-database-what-is-a-dtu.md).  Voordat u een SQL Data Warehouse herstellen kunt, controleren of de SQL server heeft onvoldoende resterende DTU-quotum voor de database wordt hersteld. Om aan te vragen meer DTU, kunt u [Maak een ondersteuningsticket](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="restore-an-active-or-paused-data-warehouse"></a>Herstellen van een actieve of onderbroken datawarehouse
Herstellen van een datawarehouse:

1. De lijst van database herstelpunten zijn gemaakt met de herstelpunten voor Database Get-bewerking ophalen.
2. Het terugzetten starten met behulp van de [Create database terugzetten aanvraag](https://msdn.microsoft.com/library/azure/dn509571.aspx) bewerking.
3. De status van uw terugzetten bijhouden met behulp van de [bewerking databasestatus](https://msdn.microsoft.com/library/azure/dn720371.aspx) bewerking.

> [!NOTE]
> Nadat het herstel is voltooid, kunt u uw herstelde datawarehouse configureren door [uw database na herstel configureren](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="restore-a-deleted-data-warehouse"></a>Herstellen van een verwijderde datawarehouse
Een verwijderde datawarehouse herstellen:

1. Overzicht van het magazijn verwijderde gegevens terug te zetten met behulp van de [lijst terug te zetten databases verwijderd](https://msdn.microsoft.com/library/azure/dn509562.aspx) bewerking.
2. De details voor de verwijderde datawarehouse die u herstellen wilt met behulp van de bewerking van de [Get terug te zetten verwijderde database] [Get terug te zetten verwijderde database] niet ophalen.
3. Het terugzetten starten met behulp van de [Create database terugzetten aanvraag](https://msdn.microsoft.com/library/azure/dn509571.aspx) bewerking.
4. De status van uw terugzetten bijhouden met behulp van de [bewerking databasestatus](https://msdn.microsoft.com/library/azure/dn720371.aspx) bewerking.

> [!NOTE]
> Voor het configureren van uw datawarehouse nadat het herstel is voltooid. Zie [uw database na herstel configureren](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="next-steps"></a>Volgende stappen
Lees voor meer informatie over de zakelijke continuïteit functies van Azure SQL Database-versies, de [Azure SQL Database business continuity overview](../sql-database/sql-database-business-continuity.md).
