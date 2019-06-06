---
title: Verbinding maken met Azure Data Explorer met behulp van ODBC
description: In dit artikel leert u hoe u voor het instellen van een verbinding Open Database Connectivity (ODBC) naar Azure Data Explorer.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 02ae9673f1dc402ee1500b466d7e259263ef3262
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66494849"
---
# <a name="connect-to-azure-data-explorer-with-odbc"></a>Verbinding maken met Azure Data Explorer met behulp van ODBC

Open Database Connectivity ([ODBC](/sql/odbc/reference/odbc-overview)) is een algemeen geaccepteerde application programming interface (API) voor toegang tot de database. ODBC gebruiken vanaf toepassingen waarvoor een specifieke connector geen verbinding maken met Azure Data Explorer.

Achter de schermen toepassingen aanroepen in de interface ODBC-functies die zijn geïmplementeerd in de database-specifieke modules met de naam *stuurprogramma's*. Azure Data Explorer ondersteunt een subset van de SQL Server-communicatieprotocol ([MS-TDS](/azure/kusto/api/tds/)), zodat deze het ODBC-stuurprogramma voor SQL Server kunt gebruiken.

In dit artikel leert u hoe u het SQL Server ODBC-stuurprogramma gebruiken zodat u kunt verbinding maken met Azure Data Explorer vanuit elke toepassing die ondersteuning biedt voor ODBC.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig:

* [Microsoft ODBC-stuurprogramma voor SQL Server-versie 17.2.0.1 of hoger](/sql/connect/odbc/download-odbc-driver-for-sql-server) voor uw besturingssysteem.

## <a name="configure-the-odbc-data-source"></a>De ODBC-gegevensbron configureren

Volg deze stappen voor het configureren van een ODBC-gegevensbron met behulp van het ODBC-stuurprogramma voor SQL Server.

1. Zoek in Windows, *ODBC-gegevensbronnen*, en opent u de desktop-app voor ODBC-gegevensbronnen.

1. Selecteer **Toevoegen**.

    ![Gegevensbron toevoegen](media/connect-odbc/add-data-source.png)

1. Selecteer **ODBC-stuurprogramma 17 voor SQL Server** vervolgens **voltooien**.

    ![Selecteer stuurprogramma](media/connect-odbc/select-driver.png)

1. Voer een naam en beschrijving voor de verbinding en het cluster dat u wilt verbinden, selecteer vervolgens **volgende**. De URL in het formulier moet cluster  *\<ClusterName\>.\< Regio\>. kusto.windows.net*.

    ![Server selecteren](media/connect-odbc/select-server.png)

1. Selecteer **Active Directory geïntegreerde** vervolgens **volgende**.

    ![Geïntegreerd met Active Directory](media/connect-odbc/active-directory-integrated.png)

1. Vervolgens selecteert u de database met de voorbeeldgegevens **volgende**.

    ![Standaard-database wijzigen](media/connect-odbc/change-default-database.png)

1. In het volgende scherm laat u alle opties als standaardwaarden Selecteer **voltooien**.

1. Selecteer **gegevensbron testen**.

    ![Gegevensbron testen](media/connect-odbc/test-data-source.png)

1. Controleren of de test is geslaagd Selecteer **OK**. Als de test is mislukt, controleert u de waarden die u hebt opgegeven in de vorige stappen en zorg ervoor dat u hebt onvoldoende machtigingen om te verbinden met het cluster.

    ![Test is voltooid](media/connect-odbc/test-succeeded.png)

## <a name="next-steps"></a>Volgende stappen

* [Verbinding maken met Azure Data Explorer vanaf Tableau](tableau.md)