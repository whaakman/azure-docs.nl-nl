---
title: Verbinding maken met Azure Data Explorer met behulp van ODBC
description: In deze procedures leert u hoe u een ODBC-verbinding met Azure Data Explorer instellen en vervolgens die verbinding gebruiken om gegevens met Tableau te visualiseren.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 02/21/2019
ms.openlocfilehash: 0ec1c2f4fc412ca6c81e179d0ad22f781b896357
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58757628"
---
# <a name="connect-to-azure-data-explorer-with-odbc"></a>Verbinding maken met Azure Data Explorer met behulp van ODBC

Open Database Connectivity ([ODBC](/sql/odbc/reference/odbc-overview)) is een algemeen geaccepteerde application programming interface (API) voor toegang tot de database. ODBC gebruiken vanaf toepassingen waarvoor een specifieke connector geen verbinding maken met Azure Data Explorer.

Achter de schermen toepassingen aanroepen in de interface ODBC-functies die zijn geïmplementeerd in de database-specifieke modules met de naam *stuurprogramma's*. Azure Data Explorer ondersteunt een subset van de SQL Server-communicatieprotocol ([MS-TDS](/azure/kusto/api/tds/)); deze kunt daarom het ODBC-stuurprogramma voor SQL Server gebruiken.

In dit artikel leert u hoe u het SQL Server ODBC-stuurprogramma gebruiken zodat u kunt verbinding maken met Azure Data Explorer vanuit elke toepassing die ondersteuning biedt voor ODBC. U kunt vervolgens desgewenst verbinding maken met Azure Data Explorer vanaf Tableau en importeer gegevens vanuit een voorbeeld-cluster.

## <a name="prerequisites"></a>Vereisten

U moet deze het voltooien van de volgende:

* [Microsoft ODBC-stuurprogramma voor SQL Server-versie 17.2.0.1 of hoger](/sql/connect/odbc/download-odbc-driver-for-sql-server) voor uw besturingssysteem.

* Als u volgen in ons voorbeeld Tableau wilt, moet u ook:

  * Tableau Desktop, volledige of [proefversie](https://www.tableau.com/products/desktop/download) versie.

  * Een cluster met de voorbeeldgegevens StormEvents. Zie voor meer informatie, [Quick Start: Maak een Azure Data Explorer-cluster en de database](create-cluster-database-portal.md) en [voorbeeldgegevens worden opgenomen in Azure Data Explorer](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

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

## <a name="visualize-data-in-tableau-optional"></a>Gegevens visualiseren in Tableau (optioneel)

Nu u klaar bent met het configureren van ODBC, kunt u voorbeeldgegevens in Tableau brengen.

1. Tableau Selecteer in Desktop in het menu links **andere Databases (ODBC)**.

    ![Verbinding maken met ODBC](media/connect-odbc/connect-odbc.png)

1. Voor **DSN**, selecteert u de gegevensbron die u hebt gemaakt voor ODBC, vervolgens **aanmelden**.

    ![ODBC-aanmelding](media/connect-odbc/odbc-sign-in.png)

1. Voor **Database**, selecteert u de database op uw voorbeeld-cluster, zoals *TestDatabase*. Voor **Schema**, selecteer *dbo*, en voor **tabel**, selecteer de *StormEvents* voorbeeldtabel.

    ![Database en tabel selecteren](media/connect-odbc/select-database-table.png)

1. Tableau wordt nu het schema voor de voorbeeldgegevens weergegeven. Selecteer **Update nu** om de gegevens in Tableau.

    ![Gegevens bijwerken](media/connect-odbc/update-data.png)

    Wanneer de gegevens worden geïmporteerd, toont Tableau rijen met gegevens die lijkt op de volgende afbeelding.

    ![Resultatenset](media/connect-odbc/result-set.png)

1. Nu kunt u visualisaties maken in Tableau op basis van de gegevens die u voorgelegd aan van Azure Data Explorer. Zie voor meer informatie, [Tableau Learning](https://www.tableau.com/learn).

## <a name="next-steps"></a>Volgende stappen

[Query's schrijven voor Azure Data Explorer](write-queries.md)

[Zelfstudie: Visualiseer gegevens uit Azure Data Explorer in Power BI](visualize-power-bi.md)