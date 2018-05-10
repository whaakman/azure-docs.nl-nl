---
title: Azure Blockchain Workbench gegevens in Microsoft Excel gebruiken
description: Informatie over het laden en Azure Blockchain Workbench SQL DB-gegevens weergeven in Microsoft Excel.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 70297bd0af6322d0f3ac2c719d1827e4bc5898cd
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="view-azure-blockchain-workbench-data-with-microsoft-excel"></a>Azure Blockchain Workbench gegevens met Microsoft Excel weergeven

Gegevens weergeven in Azure Blockchain Workbench SQL-database kunt u Microsoft Excel. Dit artikel bevat de stappen die u wilt:

* Verbinding maken met de database Blockchain Workbench van Microsoft Excel
* Bekijk Blockchain Workbench databasetabellen en weergaven
* Laden van gegevens in de Blockchain Workbench in Excel

## <a name="connect-to-the-blockchain-workbench-database"></a>Verbinding maken met de database Blockchain Workbench

Verbinding maken met een database Blockchain Workbench:

1. Open Microsoft Excel.
2. Op de **gegevens** Kies **gegevens ophalen**.
3. Selecteer **van Azure** en selecteer vervolgens **van Azure SQL Database**.

   ![Verbinding maken met Azure SQL Database](media/blockchain-workbench-data-excel/connect-sql-db.png)

4. In de **SQL Server-database** in het dialoogvenster:

    * Voor **Server**, voer de naam van de server Blockchain Workbench.
    * Voor **Database (optioneel)**, voer de naam van de database.

   ![Database-server en database opgeven](media/blockchain-workbench-data-excel/provide-server-db.png)

5. In de **SQL Server-database** dialoogvenster navigatiebalk, selecteer **Database**. Voer uw **gebruikersnaam** en **wachtwoord**, en selecteer vervolgens **Connect**.

    > [!NOTE]
    > Als u de referenties die zijn gemaakt tijdens de implementatie van Azure Blockchain Workbench de **gebruikersnaam** is `dbadmin`. De **wachtwoord** wordt de website die u tijdens de implementatie van de Blockchain Workbench gemaakt.
    
   ![Referenties opgeven voor toegang tot de database](media/blockchain-workbench-data-excel/provide-credentials.png)

## <a name="look-at-database-tables-and-views"></a>Databasetabellen en weergaven bekijken

Het dialoogvenster Navigator Excel wordt geopend nadat u verbinding met de database maakt. U kunt de Navigator gebruiken om te kijken naar de tabellen en weergaven in de database. De weergaven zijn ontworpen voor rapportage en hun namen worden voorafgegaan door **code**.

   ![Voorbeeld van een weergave Excel Navigator](media/blockchain-workbench-data-excel/excel-navigator.png)

## <a name="load-view-data-into-an-excel-workbook"></a>Gegevens van de Load-weergeven in een Excel-werkmap

Het volgende voorbeeld ziet hoe u gegevens kunt laden vanuit een weergave in een Excel-werkmap.

1. In de **Navigator** schuifbalk, selecteer de **vwContractAction** weergeven. De **vwContractAction** voorbeeld worden de acties die zijn gekoppeld aan een contract in de database Blockchain Workbench.
2. Selecteer **Load** op te halen van de gegevens in de weergave en plaats deze in uw Excel-werkmap.

   ![Gegevens uit een weergave wordt geladen](media/blockchain-workbench-data-excel/view-data.png)

Nu dat u de gegevens die zijn geladen hebt, kunt u Excel-functies om uw eigen rapporten met behulp van de metagegevens en transactie gegevens uit de database Azure Blockchain Workbench te maken.

## <a name="next-steps"></a>Volgende stappen

* [Databaseweergaven in Azure Blockchain Workbench](blockchain-workbench-database-views.md)