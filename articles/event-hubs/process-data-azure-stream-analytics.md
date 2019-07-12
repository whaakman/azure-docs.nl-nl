---
title: Verwerken van gegevens van Event Hubs Azure met behulp van Stream Analytics | Microsoft Docs
description: In dit artikel wordt beschreven hoe u voor het verwerken van gegevens uit uw Azure event hub met behulp van een Azure Stream Analytics-taak.
services: event-hubs
author: spelluru
manager: ''
ms.author: spelluru
ms.date: 07/09/2019
ms.topic: article
ms.service: event-hubs
ms.openlocfilehash: f179687b0983e145244e228a3d3b06b4eabead48
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723413"
---
# <a name="process-data-from-your-event-hub-using-azure-stream-analytics"></a>Gegevens van uw event hub met behulp van Azure Stream Analytics verwerken
De Azure Stream Analytics-service eenvoudig voor het opnemen, verwerken, en analyseer streaminggegevens van Azure Event Hubs, krachtige inzichten aan acties in realtime waarbij station inschakelen. Deze integratie kunt u snel een hot-pad analytics-pijplijn te maken. De Azure-portal kunt u inkomende gegevens visualiseren en een Stream Analytics-query schrijven. Zodra uw query gereed is, kunt u deze verplaatsen naar productie in slechts enkele klikken. 

## <a name="key-benefits"></a>Belangrijkste voordelen
Hier volgen de belangrijkste voordelen van Azure Event Hubs en Azure Stream Analytics-integratie: 
- **Voorbeeld van gegevens** – u een voorbeeld van binnenkomende gegevens van een event hub in Azure portal.
- **De query testen** : voorbereiden van een transformatiequery en test deze rechtstreeks in Azure portal. Zie voor de syntaxis van de query, [Stream Analytics Query Language](/stream-analytics-query/built-in-functions-azure-stream-analytics) documentatie.
- **Uw query implementeren naar productie** – u kunt de query in productie implementeren met het maken en starten van een Azure Stream Analytics-taak.

## <a name="end-to-end-flow"></a>End-to-end-stroom

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
1. Navigeer naar uw **Event Hubs-naamruimte** en navigeer vervolgens naar de **gebeurtenishub**, die de binnenkomende gegevens heeft. 
1. Selecteer **gegevens verwerken** op de event hub-pagina.  

    ![Tegel voor proces](./media/process-data-azure-stream-analytics/process-data-tile.png)
1. Selecteer **verkennen** op de **realtime inzichten op basis van gebeurtenissen inschakelen** tegel. 

    ![Selecteer de Stream Analytics](./media/process-data-azure-stream-analytics/process-data-page-explore-stream-analytics.png)
1. In dat geval ziet u een querypagina in met waarden is al ingesteld voor de volgende velden:
    1. Uw **gebeurtenishub** als invoer voor de query.
    1. Voorbeeld **SQL-query** met SELECT-instructie. 
    1. Een **uitvoer** alias om te verwijzen naar de testresultaten. 

        ![Query-editor](./media/process-data-azure-stream-analytics/query-editor.png)
        
        > [!NOTE]
        >  Wanneer u deze functie voor het eerst gebruikt, is deze pagina wordt gevraagd uw toestemming voor het maken van een consumentengroep en een beleid voor uw event hub Preview-versie van binnenkomende gegevens.
1. Selecteer **maken** in de **invoer preview** deelvenster zoals wordt weergegeven in de voorgaande afbeelding. 
1. Hier ziet u direct een momentopname van de meest recente binnenkomende gegevens op dit tabblad.
    - Het serialisatietype in uw gegevens automatisch is gedetecteerd (JSON/CSV). U kunt dit wijzigen evenals naar JSON/CSV/AVRO.
    - U kunt een voorbeeld van binnenkomende gegevens in de tabel of onbewerkte indeling. 
    - Als uw gegevens worden weergegeven niet actueel is, selecteert u **vernieuwen** om de meest recente gebeurtenissen te bekijken. 

        Hier volgt een voorbeeld van gegevens in de **tabelindeling**:   ![Resultaten in de tabelindeling](./media/process-data-azure-stream-analytics/snapshot-results.png)

        Hier volgt een voorbeeld van gegevens in de **onbewerkte indeling**: 

        ![Resultaten in een onbewerkte indeling](./media/process-data-azure-stream-analytics/snapshot-results-raw-format.png)
1. Selecteer **testquery** om te zien van de momentopname van de resultaten van de query in de **testresultaten** tabblad. U kunt ook de resultaten downloaden.

    ![De resultaten van query testen](./media/process-data-azure-stream-analytics/test-results.png)
1. Schrijf uw eigen query om de gegevens te transformeren. Zie [: referentie voor querytaal voor Stream Analytics](/stream-analytics-query/stream-analytics-query-language-reference).
1. Als u de query hebt getest en u verplaatsen wilt naar productie, selecteer **implementeren query**. Maak een Azure Stream Analytics-taak waar u kunt uitvoer voor uw project, en start de taak voor het implementeren van de query. Geef een naam voor de taak voor het maken van een Stream Analytics-taak, en selecteer **maken**.

      ![Een Azure Stream Analytics-taak maken](./media/process-data-azure-stream-analytics/create-stream-analytics-job.png)

      > [!NOTE] 
      >  Het is raadzaam dat u maakt een consumentengroep en een beleid voor elke nieuwe Azure Stream Analytics-taak die u van de Event Hubs-pagina maakt. Consumentengroepen toestaan slechts vijf gelijktijdige lezers, zodat een speciale klantengroep leveren voor elke taak zal voorkomen dat eventuele fouten die kunnen ontstaan deze limiet wordt overschreden. Dedicated-beleid kunt u uw sleutel draaien of machtigingen intrekken zonder gevolgen voor andere bronnen. 
1. Uw Stream Analytics-taak is nu gemaakt waarbij de query is dat u getest en input uw event hub. 

9.  Voor het voltooien van de pijplijn, stel de **uitvoer** van de query en selecteer **Start** om de taak te starten.

    > [!NOTE]
    > Voordat u start de taak, vergeet dan niet de outputalias vervangen door de uitvoernaam van de die u hebt gemaakt in Azure Stream Analytics.

      ![Stel uitvoer en start de taak](./media/process-data-azure-stream-analytics/set-output-start-job.png)


## <a name="known-limitations"></a>Bekende beperkingen
Tijdens het testen van uw query, de resultaten van de ongeveer 6 seconden duren om te laden. We werken aan het verbeteren van de prestaties van testen. Echter, wanneer dit wordt geïmplementeerd in een productieomgeving, heeft Azure Stream Analytics subsecond latentie.

## <a name="streaming-units"></a>Streaming-eenheden
Uw Azure Stream Analytics-taak standaard ingesteld op drie streaming-eenheden (su's). Als u wilt deze instelling aanpassen, selecteert u **schaal** in het menu links in de **Stream Analytics-taak** pagina in de Azure portal. Zie voor meer informatie over het streaming-eenheden, [begrijpen en aanpassen van Streaming-eenheden](../stream-analytics/stream-analytics-streaming-unit-consumption.md).

![Schalen van streaming-eenheden](./media/process-data-azure-stream-analytics/scale.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Stream Analytics-query's, [querytaal voor Stream Analytics](/stream-analytics-query/built-in-functions-azure-stream-analytics)