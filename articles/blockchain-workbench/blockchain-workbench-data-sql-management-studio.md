---
title: Gegevens van Azure Blockchain Workbench gebruiken met SQL Server Management Studio
description: Leer hoe u vanuit SQL Server Management Studio verbinding maakt met een SQL-database van Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: b640aec322d6dc184a428e67d9576cf162c8f394
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075252"
---
# <a name="using-azure-blockchain-workbench-data-with-sql-server-management-studio"></a>Gegevens van Azure Blockchain Workbench gebruiken met SQL Server Management Studio

Microsoft SQL Server Management Studio biedt de mogelijkheid om snel query's te schrijven voor een SQL-database van Azure Blockhain Workbench en deze te testen. In dit gedeelte wordt stapsgewijs uitgelegd hoe u vanuit SQL Server Management Studio verbinding maakt met een SQL-database van Azure Blockchain Workbench.

## <a name="prerequisites"></a>Vereisten

* [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) downloaden.

## <a name="connecting-sql-server-management-studio-to-data-in-azure-blockchain-workbench"></a>SQL Server Management Studio verbinden met gegevens in Azure Blockchain Workbench

1. Open SQL Server Management Studio en selecteer **Connect**.
2. Selecteer **Database Engine**.

    ![Database-engine](media/blockchain-workbench-data-sql-management-studio/database-engine.png)

3. Voer in het dialoogvenster **Verbinding maken met server** de naam van de server en uw databasereferenties in.

    Als u de referenties gebruikt die zijn gemaakt tijdens de implementatie van Azure Blockchain Workbench, is de gebruikersnaam **dbadmin** en het wachtwoord het wachtwoord dat u hebt opgegeven tijdens de implementatie.

    ![SQL-referenties invoeren](media/blockchain-workbench-data-sql-management-studio/sql-creds.png)

 4. In SQL Server Management Studio ziet u de lijst met databases, databaseweergaven en opgeslagen procedures uit de Azure Blockchain Workbench-database.

    ![Databaselijst](media/blockchain-workbench-data-sql-management-studio/db-list.png)

5. Als u de gegevens wilt bekijken die aan een databaseweergave zijn gekoppeld, kunt u met de volgende stappen automatisch een select-instructie genereren.
6. Klik met de rechtermuisknop op een van de databaseweergaven in Object Explorer.
7. Selecteer **Script View as**.
8. Kies **SELECT to**.
9. Selecteer **New Query Editor Window**.
10. U kunt een nieuwe query maken door **New Query** te selecteren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Databaseweergaven in Azure Blockchain Workbench](blockchain-workbench-database-views.md)