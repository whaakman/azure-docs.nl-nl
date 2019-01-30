---
title: Herstellen van Azure SQL Data Warehouse (Azure portal) | Microsoft Docs
description: Azure portal taken voor het herstellen van Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 0d8bb451c821a4883c81e0e51c5d98e146e3008e
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55243535"
---
# <a name="restore-azure-sql-data-warehouse-portal"></a>Herstellen van Azure SQL Data Warehouse (portal)
> [!div class="op_single_selector"]
> * [Overzicht][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
>
>
In dit artikel leert u hoe u kunt het herstellen van Azure SQL Data Warehouse met behulp van de Azure-portal.

## <a name="before-you-begin"></a>Voordat u begint
**Controleer of de capaciteit van uw DTU.** Elk exemplaar van SQL Data Warehouse wordt gehost door een SQL-server (bijvoorbeeld myserver.database.windows.net) waarvoor een standaardquotum gegevens throughput unit (DTU). Voordat u SQL Data Warehouse herstellen kunt, moet u controleren of uw SQL-server genoeg resterende DTU-quotum voor de database die u wilt terugzetten heeft. Zie voor meer informatie over het berekenen van de DTU-quotum of om aan te vragen van meer dtu's, [een wijziging van de DTU-quotum aanvragen][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Herstellen van een database actief of onderbroken
Een database te herstellen:

1. Meld u aan bij [Azure Portal][Azure portal].
2. Selecteer in het linkerdeelvenster **Bladeren**, en selecteer vervolgens **SQL-servers**.

    ![Selecteer Bladeren > SQL-servers](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Zoeken naar uw server en selecteer dit.

    ![Selecteer uw server](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. Vinden van het exemplaar van SQL Data Warehouse die u herstellen wilt uit en selecteer vervolgens het.

    ![Selecteer het exemplaar van SQL Data Warehouse om terug te zetten](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Selecteer aan de bovenkant van de blade Data Warehouse, **herstellen**.

    ![Selecteer herstellen](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. Geef een nieuwe **databasenaam**.
7. Selecteer de meest recente **herstelpunt**.

   Zorg ervoor dat u de meest recente herstelpunt kiezen. Omdat de herstelpunten worden weergegeven in Coordinated Universal Time (UTC), is de standaardoptie mogelijk niet de meest recente herstelpunt.

      ![Selecteer een herstelpunt](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. Selecteer **OK**.
9. Het proces van de database herstellen wordt gestart en kunt u **meldingen** voor het bewaken van het proces.

> [!NOTE]
> Nadat het herstel is voltooid, kunt u de herstelde database configureren door [configureren van uw database na het herstel][Configure your database after recovery].
>
>

## <a name="restore-a-deleted-database"></a>Een verwijderde database herstellen
Een verwijderde database herstellen:

1. Meld u aan bij [Azure Portal][Azure portal].
2. Selecteer in het linkerdeelvenster **Bladeren**, en selecteer vervolgens **SQL-servers**.

    ![Selecteer Bladeren > SQL-servers](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Zoeken naar uw server en selecteer dit.

    ![Selecteer uw server](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. Schuif omlaag naar de **Operations** sectie op de blade van uw server.
5. Selecteer de **verwijderde databases** tegel.

    ![Selecteer de tegel van de databases verwijderd](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. Selecteer de verwijderde database die u wilt herstellen.

    ![Selecteer een database herstellen](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. Geef een nieuwe **databasenaam**.

    ![Een naam voor de database toevoegen](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. Selecteer **OK**.
9. Het proces van de database herstellen wordt gestart en kunt u **meldingen** voor het bewaken van het proces.

> [!NOTE]
> Zie configureren van uw database nadat het herstel is voltooid, [configureren van uw database na het herstel][Configure your database after recovery].
>
>

## <a name="next-steps"></a>Volgende stappen
Lees voor meer informatie over de functies voor bedrijfscontinuïteit van Azure SQL Database-edities, de [Azure SQL Database-overzicht voor zakelijke continuïteit][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/
