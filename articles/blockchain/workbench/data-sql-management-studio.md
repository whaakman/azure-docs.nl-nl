---
title: Gegevens van Azure Blockchain Workbench gebruiken met SQL Server Management Studio
description: Leer hoe u vanuit SQL Server Management Studio verbinding maakt met een SQL-database van Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 01/14/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 32606f988152b061fa1923c409207b2a650e64ba
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332452"
---
# <a name="using-azure-blockchain-workbench-data-with-sql-server-management-studio"></a>Gegevens van Azure Blockchain Workbench gebruiken met SQL Server Management Studio

Microsoft SQL Server Management Studio biedt de mogelijkheid om snel te schrijven en testen van query's op basis van Azure Blockchain Workbench SQL DB. In dit gedeelte wordt stapsgewijs uitgelegd hoe u vanuit SQL Server Management Studio verbinding maakt met een SQL-database van Azure Blockchain Workbench.

## <a name="prerequisites"></a>Vereisten

* [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) downloaden.

## <a name="connecting-sql-server-management-studio-to-data-in-azure-blockchain-workbench"></a>SQL Server Management Studio verbinden met gegevens in Azure Blockchain Workbench

1. Open SQL Server Management Studio en selecteer **Connect**.
2. Selecteer **Database Engine**.

    ![Database-engine](./media/data-sql-management-studio/database-engine.png)

3. Voer in het dialoogvenster **Verbinding maken met server** de naam van de server en uw databasereferenties in.

    Als u de referenties gebruikt die zijn gemaakt tijdens de implementatie van Azure Blockchain Workbench, is de gebruikersnaam **dbadmin** en het wachtwoord het wachtwoord dat u hebt opgegeven tijdens de implementatie.

    ![SQL-referenties invoeren](./media/data-sql-management-studio/sql-creds.png)

 4. In SQL Server Management Studio ziet u de lijst met databases, databaseweergaven en opgeslagen procedures uit de Azure Blockchain Workbench-database.

    ![Databaselijst](./media/data-sql-management-studio/db-list.png)

5. Als u de gegevens wilt bekijken die aan een databaseweergave zijn gekoppeld, kunt u met de volgende stappen automatisch een select-instructie genereren.
6. Met de rechtermuisknop op een van de databaseweergaven in Object Explorer.
7. Selecteer **Script View as**.
8. Kies **SELECT to**.
9. Selecteer **New Query Editor Window**.
10. U kunt een nieuwe query maken door **New Query** te selecteren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Databaseweergaven in Azure Blockchain Workbench](database-views.md)
