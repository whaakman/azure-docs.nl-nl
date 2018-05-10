---
title: Azure Blockchain Workbench gegevens gebruiken voor Microsoft Power BI
description: Informatie over het laden en Azure Blockchain Workbench SQL DB-gegevens weergeven in Microsoft Power BI.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 2b909c0a8441010b87c913e5937d25c8127058f1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="using-azure-blockchain-workbench-data-with-microsoft-power-bi"></a>Met behulp van Azure Blockchain Workbench-gegevens met Microsoft Power BI

Microsoft Power BI biedt de mogelijkheid om eenvoudig krachtige rapporten genereren uit met behulp van Power BI Desktop SQL DB-databases en ze vervolgens publiceert [ https://www.powerbi.com ](http://www.powerbi.com).

Dit artikel bevat een stapsgewijs overzicht van het verbinding maken met Azure Blockchain Workbench SQL-Database van in Power BI desktop, een rapport maken en implementeren van het rapport op powerbi.com.

## <a name="prerequisites"></a>Vereisten

* Download [Power BI Desktop](https://aka.ms/pbidesktopstore).

## <a name="connecting-powerbi-to-data-in-azure-blockchain-workbench"></a>Verbinding maken met Power BI-gegevens in Azure Blockchain Workbench

1.  Power BI Desktop openen.
2.  Selecteer **gegevens ophalen**.

    ![Gegevens ophalen](media/blockchain-workbench-data-powerbi/get-data.png)
3.  Selecteer **SQL Server** uit de lijst met typen gegevensbronnen.

4.  Geef de naam van de server en database in het dialoogvenster. Opgeven of u wilt de gegevens importeren of uit te voeren een **DirectQuery**. Selecteer **OK**.

    ![SQL-server selecteren](media/blockchain-workbench-data-powerbi/select-sql.png)

5.  Geef de databasereferenties voor toegang tot Azure Blockchain Workbench. Selecteer **Database** en voer uw referenties.

    Als u de referenties die zijn gemaakt door het proces Azure Blockchain Workbench-implementatie gebruikt, de gebruikersnaam is **dbadmin** is en het wachtwoord die u hebt opgegeven tijdens de implementatie.

    ![SQL-database-instellingen](media/blockchain-workbench-data-powerbi/db-settings.png)

6.  Eenmaal zijn verbonden met de database, de **Navigator** dialoogvenster wordt weergegeven voor de tabellen en weergaven die beschikbaar zijn in de database. De weergaven die zijn ontworpen voor rapportage en worden voorafgegaan **code**.

    ![Navigator](media/blockchain-workbench-data-powerbi/navigator.png)

7.  Selecteer de weergaven die u wilt opnemen. We opnemen voor demonstratiedoeleinden **vwContractAction**, die worden details weergegeven over alle acties die hebben plaatsgevonden voor een contract.

    ![Weergaven selecteren](media/blockchain-workbench-data-powerbi/select-views.png)

U kunt nu maken en publiceren van rapporten, zoals u dat gewend met Power BI bent.

## <a name="next-steps"></a>Volgende stappen

* [Databaseweergaven in Azure Blockchain Workbench](blockchain-workbench-database-views.md)