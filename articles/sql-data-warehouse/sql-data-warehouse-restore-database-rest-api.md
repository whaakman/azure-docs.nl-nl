---
title: Een Azure Sql0datawarehouse herstellen - REST-API | Microsoft Docs
description: Herstel een Azure SQL Data Warehouse met behulp van REST-API's.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 71236afd3f72497887f42667f971539ed294bef1
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307533"
---
# <a name="restore-an-azure-sql-data-warehouse-with-rest-apis"></a>Herstellen van een Azure SQL datawarehouse met REST-API 's
Herstel een Azure SQL Data Warehouse met behulp van REST-API's.

## <a name="before-you-begin"></a>Voordat u begint
**Controleer of de capaciteit van uw DTU.** Elke SQL Data Warehouse wordt gehost door een logische SQL-server (bijvoorbeeld myserver.database.windows.net) met een standaard [DTU-quotum](../sql-database/sql-database-what-is-a-dtu.md).  Voordat u een SQL Data Warehouse herstelt kunt, controleren of de SQL server heeft onvoldoende resterende DTU-quotum voor de database wordt hersteld. Als u wilt meer DTU aanvragen, kunt u [een ondersteuningsticket maken](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="restore-an-active-or-paused-data-warehouse"></a>Een actieve of onderbroken datawarehouse herstellen
Een datawarehouse herstellen:

1. De lijst met herstelpunten voor databases met behulp van de herstelpunten voor databases van Get-bewerking ophalen.
2. Beginnen met het herstellen met behulp van de [Create database restore-aanvraag](https://msdn.microsoft.com/library/azure/dn509571.aspx) bewerking.
3. Bijhouden van de status van het herstellen met behulp van de [bewerking databasestatus](https://msdn.microsoft.com/library/azure/dn720371.aspx) bewerking.

> [!NOTE]
> Nadat het herstel is voltooid, kunt u uw herstelde datawarehouse configureren door [configureren van uw database na het herstel](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="restore-a-deleted-data-warehouse"></a>Een verwijderd datawarehouse herstellen
Een verwijderd datawarehouse herstellen:

1. Overzicht van het magazijn verwijderde gegevens terug te zetten met behulp van de [actuele lijst met databases verwijderd](https://msdn.microsoft.com/library/azure/dn509562.aspx) bewerking.
2. De details ophalen voor de verwijderde datawarehouse dat u herstellen wilt met behulp van [de Get die kunnen worden hersteld verwijderde database] [Get die kunnen worden hersteld verwijderde database]-bewerking.
3. Beginnen met het herstellen met behulp van de [Create database restore-aanvraag](https://msdn.microsoft.com/library/azure/dn509571.aspx) bewerking.
4. Bijhouden van de status van het herstellen met behulp van de [bewerking databasestatus](https://msdn.microsoft.com/library/azure/dn720371.aspx) bewerking.

> [!NOTE]
> Zie configureren van uw datawarehouse nadat het herstel is voltooid, [configureren van uw database na het herstel](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="next-steps"></a>Volgende stappen
Lees voor meer informatie over de functies voor bedrijfscontinuïteit van Azure SQL Database-edities, de [Azure SQL Database-overzicht voor zakelijke continuïteit](../sql-database/sql-database-business-continuity.md).
