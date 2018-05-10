---
title: Azure Blockchain Workbench gegevens met SQL Server Management Studio gebruiken
description: Informatie over het verbinding maken met Azure Blockchain Workbench SQL-Database van SQL Server Management Studio.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 1c9ee1e94ba1195db027c3edfab2e2a96f181ca3
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="using-azure-blockchain-workbench-data-with-sql-server-management-studio"></a>Azure Blockchain Workbench gegevens gebruiken met SQL Server Management Studio

Microsoft SQL Server Management Studio biedt de mogelijkheid om snel in te schrijven en testen van een query uitgevoerd naar Azure Blockhain Workbench SQL-database. Deze sectie bevat een overzicht van de stap voor stap van het verbinding maken met Azure Blockchain Workbench SQL-Database van SQL Server Management Studio.

## <a name="prerequisites"></a>Vereisten

* Download [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).

## <a name="connecting-sql-server-management-studio-to-data-in-azure-blockchain-workbench"></a>SQL Server Management Studio verbinding te maken met gegevens in Azure Blockchain Workbench

1. Open SQL Server Management Studio en selecteer **Connect**.
2. Selecteer **Database-Engine**.

    ![Database-engine](media/blockchain-workbench-data-sql-management-studio/database-engine.png)

3. In de **verbinding maken met Server** dialoogvenster voert u de servernaam en databasereferenties van uw.

    Als u de referenties die zijn gemaakt door het proces Azure Blockchain Workbench-implementatie gebruikt, de gebruikersnaam niet **dbadmin** en het wachtwoord die u hebt opgegeven tijdens de implementatie.

    ![SQL-referenties invoeren](media/blockchain-workbench-data-sql-management-studio/sql-creds.png)

 4. SQL Server Management Studio geeft de lijst met databases, weergaven en opgeslagen procedures in de database Azure Blockchain Workbench.

    ![databaselijst](media/blockchain-workbench-data-sql-management-studio/db-list.png)

5. Bekijk de gegevens die zijn gekoppeld aan een van de databaseweergaven, kunt u automatisch genereren van een select-instructie met de volgende stappen.
6. Klik met de rechtermuisknop op een van de databaseweergaven in Object Explorer.
7. Selecteer **weergeven als een Script**.
8. Kies **selecteren om te**.
9. Selecteer **nieuwe Query-editorvenster**.
10. Een nieuwe query kan worden gemaakt door het selecteren van **nieuwe Query**.

## <a name="next-steps"></a>Volgende stappen

* [Databaseweergaven in Azure Blockchain Workbench](blockchain-workbench-database-views.md)