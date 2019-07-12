---
title: 'Visualiseer gegevens uit Azure Data Explorer met een geïmporteerde query in Power BI '
description: 'In dit artikel leert u hoe u een van de drie opties voor het visualiseren van gegevens in Power BI: een query importeren uit Azure Data Explorer.'
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 6c2c8457cee004bf1288656ec9746b703d41aee1
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67806406"
---
# <a name="visualize-data-using-a-query-imported-into-power-bi"></a>Gegevens visualiseren met behulp van een query die is geïmporteerd in Power BI

Azure Data Explorer is een snelle en zeer schaalbare service om gegevens in logboeken en telemetrie te verkennen. Power BI is een business analytics-oplossing waarmee u uw gegevens kunt visualiseren en de gegevens kunt delen in uw organisatie.

Azure Data Explorer biedt drie opties om gegevens te verbinden in Power BI: de ingebouwde connector gebruiken, een query importeren uit Azure Data Explorer, of een SQL-query gebruiken. In dit artikel wordt beschreven hoe u een query importeren, zodat u kunt gegevens ophalen en in een Power BI-rapport visualiseren.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende om te voltooien in dit artikel nodig:

* Een organisatie-e-mailaccount dat lid is van Azure Active Directory, zodat u verbinding kunt maken met het [Azure Data Explorer-helpcluster](https://dataexplorer.azure.com/clusters/help/databases/samples).

* [Power BI Desktop](https://powerbi.microsoft.com/get-started/) (selecteer **GRATIS DOWNLOADEN**)

* [Azure Data Explorer-desktop-app](/azure/kusto/tools/kusto-explorer)

## <a name="get-data-from-azure-data-explorer"></a>Gegevens ophalen uit Azure Data Explorer

Eerst maakt u een query in de Azure Data Explorer-desktop-app en exporteert u deze voor gebruik in Power BI. Vervolgens maakt u verbinding met het Azure Data Explorer-helpcluster en haalt u een subset gegevens op uit de tabel *StormEvents*. [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

1. Ga in een browser naar [https://help.kusto.windows.net/](https://help.kusto.windows.net/) om de Azure Data Explorer-desktop-app te starten.

1. Kopieer in de desktop-app de volgende query in het queryvenster rechtsboven, en voer deze uit.

    ```Kusto
    StormEvents
    | sort by DamageCrops desc
    | take 1000
    ```

    De eerste paar rijen van de resultatenset zien eruit zoals in de volgende afbeelding.

    ![Queryresultaten](media/power-bi-imported-query/query-results.png)

1. Selecteer op het tabblad **Hulpprogramma’s** de optie **Query uitvoeren in Power BI**, en selecteer **OK**.

    ![Query exporteren](media/power-bi-imported-query/export-query.png)

1. Selecteer in Power BI Desktop op het tabblad **Start** de optie **Gegevens ophalen** en vervolgens **Lege query**.

    ![Gegevens ophalen](media/power-bi-imported-query/get-data.png)

1. Selecteer in de Power Query-editor op het tabblad **Start** de optie **Geavanceerde editor**.

1. Plak in het venster **Geavanceerde editor** de query die u hebt geëxporteerd, en selecteer vervolgens **Gereed**.

    ![Query plakken](media/power-bi-imported-query/paste-query.png)

1. Selecteer in het hoofdvenster van de Power Query-editor de optie **Referenties bewerken**. Selecteer **Organisatieaccount**, meld u aan, en selecteer **Verbinding maken**.

    ![Referenties bewerken](media/power-bi-imported-query/edit-credentials.png)

1. Selecteer op het tabblad **Start** de optie **Sluiten en toepassen**.

    ![Sluiten en toepassen](media/power-bi-imported-query/close-apply.png)

## <a name="visualize-data-in-a-report"></a>Gegevens visualiseren in een rapport

[!INCLUDE [data-explorer-power-bi-visualize-basic](../../includes/data-explorer-power-bi-visualize-basic.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u het rapport dat u hebt gemaakt niet meer nodig voor dit artikel, verwijdert u de Power BI Desktop (.pbix)-bestand.

## <a name="next-steps"></a>Volgende stappen

[Gegevens visualiseren met behulp van de Azure Data Explorer-connector voor Power BI](power-bi-connector.md)