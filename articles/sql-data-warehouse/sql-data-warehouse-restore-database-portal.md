---
title: Herstellen van Azure SQL Data Warehouse (Azure-portal) | Microsoft Docs
description: Azure portal taken voor het herstellen van Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: barbkess
editor: 
ms.assetid: b0aef539-7657-4b0e-9899-74098f5c21bc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 09/21/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: f6bc8671410dc7015a8d2a4bea1ba11f9ae526c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="restore-azure-sql-data-warehouse-portal"></a>Herstellen van Azure SQL Data Warehouse (portal)
> [!div class="op_single_selector"]
> * [Overzicht][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
>
>
In dit artikel leert u hoe u Azure SQL Data Warehouse herstelt met behulp van de Azure-portal.

## <a name="before-you-begin"></a>Voordat u begint
**Controleer of de capaciteit van de DTU.** Elk exemplaar van SQL Data Warehouse wordt gehost door een SQL-server (bijvoorbeeld myserver.database.windows.net) waarvoor een standaardquotum gegevensdoorvoer unit (DTU). Voordat u SQL Data Warehouse herstellen kunt, Controleer of uw SQL server onvoldoende resterende DTU-quotum voor de database die u wilt herstellen. Zie voor meer informatie over het DTU-quotum berekenen of dat er meer dtu's, [een wijziging DTU-quotum aanvragen][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Herstel de database van een actieve of onderbroken
Een database te herstellen:

1. Meld u aan bij [Azure Portal][Azure portal].
2. Selecteer in het linkerdeelvenster **Bladeren**, en selecteer vervolgens **SQL-servers**.

    ![Selecteer Bladeren > SQL-servers](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Zoek uw server en selecteert u vervolgens.

    ![Selecteer de server](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. Zoek het exemplaar van SQL Data Warehouse die u herstellen wilt uit en selecteert u vervolgens.

    ![Selecteer het exemplaar van SQL Data Warehouse herstellen](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Selecteer boven aan de blade Data Warehouse **herstellen**.

    ![Selecteer herstellen](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. Geef een nieuwe **databasenaam**.
7. Selecteer de meest recente **herstelpunt**.

   Zorg ervoor dat u de meest recente herstelpunt kiezen. Omdat herstelpunten worden weergegeven in Coordinated Universal Time (UTC), is de standaardoptie mogelijk niet de meest recente herstelpunt.

      ![Selecteer een herstelpunt](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. Selecteer **OK**.
9. Het proces van de database herstellen wordt gestart en u kunt **meldingen** om het proces te bewaken.

> [!NOTE]
> Nadat het herstel is voltooid, kunt u de herstelde database configureren door [uw database na herstel configureren][Configure your database after recovery].
>
>

## <a name="restore-a-deleted-database"></a>Een verwijderde database herstellen
Een verwijderde database herstellen:

1. Meld u aan bij [Azure Portal][Azure portal].
2. Selecteer in het linkerdeelvenster **Bladeren**, en selecteer vervolgens **SQL-servers**.

    ![Selecteer Bladeren > SQL-servers](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Zoek uw server en selecteert u vervolgens.

    ![Selecteer de server](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. Schuif omlaag naar de **Operations** sectie op de blade van uw server.
5. Selecteer de **databases verwijderd** tegel.

    ![Selecteer de verwijderde databases tegel](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. Selecteer de verwijderde database die u wilt herstellen.

    ![Selecteer een database herstellen](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. Geef een nieuwe **databasenaam**.

    ![Voeg een naam voor de database](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. Selecteer **OK**.
9. Het proces van de database herstellen wordt gestart en u kunt **meldingen** om het proces te bewaken.

> [!NOTE]
> Voor het configureren van uw database nadat het herstel is voltooid. Zie [uw database na herstel configureren][Configure your database after recovery].
>
>

## <a name="next-steps"></a>Volgende stappen
Lees voor meer informatie over de zakelijke continuïteit functies van Azure SQL Database-versies, de [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/
