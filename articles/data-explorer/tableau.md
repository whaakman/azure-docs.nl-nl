---
title: Open Database Connectivity (ODBC) verbinding naar Azure Data Explorer gebruiken om gegevens visualiseren met Tableau
description: In dit artikel leert u hoe u een verbinding Open Database Connectivity (ODBC) met Azure Data Explorer verbinding met gegevens visualiseren met Tableau.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: a0948ae35a5c23768df117979db819861ac64529
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66499082"
---
# <a name="visualize-data-from-azure-data-explorer-in-tableau"></a>Visualiseer gegevens uit Azure Data Explorer in Tableau

 [Tableau](https://www.tableau.com/) is een visual analyseplatform voor business intelligence. Als u wilt verbinding maken met Azure Data Explorer vanaf Tableau en importeer gegevens vanuit een voorbeeld-cluster, gebruikt u het stuurprogramma voor SQL Server Open Database Connectivity (ODBC). 

## <a name="prerequisites"></a>Vereisten

U hebt het volgende om te voltooien in dit artikel nodig:

* [Verbinding maken met Azure Data Explorer met behulp van ODBC](connect-odbc.md) met behulp van het SQL Server ODBC-stuurprogramma verbinding maken met Azure Data Explorer in Tableau. 

* Tableau Desktop, vol is, of [proefversie](https://www.tableau.com/products/desktop/download) versie.

* Een cluster met de voorbeeldgegevens StormEvents. Zie voor meer informatie, [maken van een cluster van Azure Data Explorer en -database](create-cluster-database-portal.md) en [voorbeeldgegevens worden opgenomen in Azure Data Explorer](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="visualize-data-in-tableau"></a>Gegevens visualiseren in Tableau 

Wanneer u klaar bent met het configureren van ODBC, kunt u voorbeeldgegevens in Tableau brengen.

1. Tableau Selecteer in Desktop in het menu links **andere Databases (ODBC)** .

    ![Verbinding maken met ODBC](media/tableau/connect-odbc.png)

1. Voor **DSN**, selecteert u de gegevensbron die u hebt gemaakt voor ODBC, vervolgens **aanmelden**.

    ![ODBC-aanmelding](media/tableau/odbc-sign-in.png)

1. Voor **Database**, selecteert u de database op uw voorbeeld-cluster, zoals *TestDatabase*. Voor **Schema**, selecteer *dbo*, en voor **tabel**, selecteer de *StormEvents* voorbeeldtabel.

    ![Database en tabel selecteren](media/tableau/select-database-table.png)

1. Tableau wordt nu het schema voor de voorbeeldgegevens weergegeven. Selecteer **Update nu** om de gegevens in Tableau.

    ![Gegevens bijwerken](media/tableau/update-data.png)

    Wanneer de gegevens worden geïmporteerd, toont Tableau rijen met gegevens die lijkt op de volgende afbeelding.

    ![Resultatenset](media/tableau/result-set.png)

1. Nu kunt u visualisaties maken in Tableau op basis van de gegevens die u voorgelegd aan van Azure Data Explorer. Zie voor meer informatie, [Tableau Learning](https://www.tableau.com/learn).

## <a name="next-steps"></a>Volgende stappen

* [Query's schrijven voor Azure Data Explorer](write-queries.md)