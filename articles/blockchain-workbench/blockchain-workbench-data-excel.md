---
title: Gegevens van Azure Blockchain Workbench gebruiken in Microsoft Excel
description: Leer hoe u gegevens uit een SQL-database van Azure Blockchain Workbench laadt en weergeeft in Microsoft Excel.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: e8c20f4b8e39615e2a8c486130d7c8bec655a936
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076913"
---
# <a name="view-azure-blockchain-workbench-data-with-microsoft-excel"></a>Gegevens van Azure Blockchain Workbench weergeven met Microsoft Excel

U kunt Microsoft Excel gebruiken om gegevens in een SQL-database van Azure Blockchain Workbench weer te geven. Dit artikel bevat de stappen die nodig zijn voor deze bewerkingen:

* Verbinding maken met een database van Blockchain Workbench vanuit Microsoft Excel
* Databasetabellen en -weergaven van Blockchain Workbench bekijken
* Gegevens van Blockchain Workbench laden in Excel

## <a name="connect-to-the-blockchain-workbench-database"></a>Verbinding maken met een database van Blockchain Workbench

U kunt als volgt verbinding maken met een database van Blockchain Workbench:

1. Open Microsoft Excel.
2. Ga naar het tabblad **Gegevens** en kies **Gegevens ophalen**.
3. Selecteer **Uit Azure** en selecteer vervolgens **Uit Azure SQL Database**.

   ![Verbinding maken met Azure SQL Database](media/blockchain-workbench-data-excel/connect-sql-db.png)

4. Ga als volgt te werk in het dialoogvenster **SQL Server-database**:

    * Voer bij **Server** de naam van de Blockchain Workbench-server in.
    * Voer bij **Database (optioneel)** de naam van de database in.

   ![Databaseserver en database opgeven](media/blockchain-workbench-data-excel/provide-server-db.png)

5. Selecteer **Database** in de navigatiebalk van het dialoogvenster **SQL Server-database**. Geef bij **Gebruikersnaam** en **Wachtwoord** uw gebruikersnaam en wachtwoord op en selecteer vervolgens **Verbinding maken**.

    > [!NOTE]
    > Als u de referenties gebruikt die zijn gemaakt tijdens de implementatie van Azure Blockchain Workbench, is `dbadmin` de **gebruikersnaam**. Gebruik voor **Wachtwoord** het wachtwoord dat u hebt gemaakt tijdens de implementatie van Blockchain Workbench.
    
   ![Referenties opgeven voor toegang tot de database](media/blockchain-workbench-data-excel/provide-credentials.png)

## <a name="look-at-database-tables-and-views"></a>Databasetabellen en -weergaven bekijken

Het dialoogvenster Navigator van Excel wordt geopend zodra er verbinding is met de database. Gebruik dit dialoogvenster om de tabellen en weergaven in de database te bekijken. De weergaven zijn ontworpen voor rapportagedoeleinden en hun namen worden voorafgegaan door **vw**.

   ![Voorbeeld van een weergave in Excel Navigator](media/blockchain-workbench-data-excel/excel-navigator.png)

## <a name="load-view-data-into-an-excel-workbook"></a>Gegevens van een weergave laden in een Excel-werkmap

Het volgende voorbeeld laat zien hoe u gegevens vanuit een weergave kunt laden in een Excel-werkmap.

1. Selecteer op de schuifbalk van **Navigator** de weergave **vwContractAction**. In het voorbeeld van **vwContractAction** worden alle acties weergegeven die zijn gerelateerd aan een contract in de database van Blockchain Workbench.
2. Selecteer **Laden** om alle gegevens in de weergave op te halen en deze over te brengen naar de Excel-werkmap.

   ![Gegevens geladen uit een weergave](media/blockchain-workbench-data-excel/view-data.png)

Nu de gegevens geladen zijn, kunt u functies van Excel gebruiken om uw eigen rapporten te maken met behulp van de metagegevens en transactiegegevens uit de database van Azure Blockchain Workbench.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Databaseweergaven in Azure Blockchain Workbench](blockchain-workbench-database-views.md)