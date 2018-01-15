---
title: Realtime IoT-gegevensstromen en Azure Stream Analytics | Microsoft Docs
description: IoT-sensortags en -gegevensstromen met Stream Analytics en realtime-gegevensverwerking
keywords: IOT-oplossing, aan de slag met iot
services: stream-analytics
documentationcenter: 
author: SnehaGunda
manager: jhubbard
editor: cgronlun
ms.assetid: 3e829055-75ed-469f-91f5-f0dc95046bdb
ms.service: stream-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: sngun
ms.openlocfilehash: a4b2fda6c5cc5ea341618ec5fa8638a5c887bf84
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="get-started-with-azure-stream-analytics-to-process-data-from-iot-devices"></a>Aan de slag met Azure Stream Analytics om gegevens te verwerken van IoT-apparaten
In deze zelfstudie leert u hoe u stroomverwerkingslogica schrijft om gegevens te verzamelen van IoT-apparaten (Internet of Things). We gebruiken hier een echte IoT-gebruikstoepassing (Internet of Things) om aan te tonen hoe u snel en economisch een oplossing maakt.

## <a name="prerequisites"></a>Vereisten
* [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/)
* Voorbeeldquery en gegevensbestanden die u kunt downloaden van [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## <a name="scenario"></a>Scenario
Contoso, een bedrijf op het gebied van industriële automatisering, heeft hun fabricageproces volledig geautomatiseerd. De machines in dit bedrijf hebben sensoren die in realtime gegevensstromen kunnen genereren. In dit scenario wil een werkvloerbeheerder realtime-inzichten op basis van de sensorgegevens om naar patronen te zoeken en hierop te reageren. We passen de SAQL (Stream Analytics Query Language) op de sensorgegevens toe om interessante patronen te vinden in de stroom inkomende gegevens.

Hier worden gegevens gegenereerd met een Texas Instrument Sensor Tag-apparaat. De nettolading van de gegevens heeft de JSON-indeling en ziet er ongeveer als volgt uit:

    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

In een werkelijk scenario hebt u honderden van dit soort sensoren die gebeurtenissen als een stroom kunnen genereren. In het ideale geval is er een gateway-apparaat waarop bepaalde code wordt uitgevoerd die deze gebeurtenissen pusht naar [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) of [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/). Uw Stream Analytics-taak zou deze gebeurtenissen opnemen van Event Hubs en realtime analysequery’s uitvoeren tegen de stromen. Dan kunt u de resultaten verzenden naar één van de [ondersteunde outputs](stream-analytics-define-outputs.md).

Voor het gebruiksgemak biedt deze introductiehandleiding een bestand met voorbeeldgegevens dat is verkregen uit echte Sensor Tag-apparaten. U kunt query's uitvoeren op de voorbeeldgegevens en resultaten weergeven. In volgende zelfstudies leert u hoe u een taak verbindt met in- en uitvoer, en deze implementeert in de Azure-service.

## <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken
1. Klik in [Azure Portal](http://portal.azure.com) op het plusteken en typ vervolgens **STREAM ANALYTICS** in het tekstvenster aan de rechterkant. Selecteer **Stream Analytics-taak** in de lijst met resultaten.
   
    ![Een nieuwe Stream Analytics-taak maken](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)
2. Voer een unieke taaknaam in en controleer of het abonnement het juiste abonnement is voor de taak. Maak vervolgens een nieuwe resourcegroep of selecteer een bestaande in uw abonnement.
3. Selecteer een locatie voor uw taak. Voor de verwerkingssnelheid en lagere kosten voor gegevensoverdracht wordt het aanbevolen voor de resourcegroep en de gewenste opslagaccount dezelfde locatie te kiezen.
   
    ![Details van het maken van een nieuwe Stream Analytics-taak](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)
   
   > [!NOTE]
   > U maakt dit opslagaccount slechts één keer per regio. Deze opslag wordt gedeeld tussen alle Stream Analytics-taken die zijn gemaakt in deze regio.
   > 
   > 
4. Schakel het selectievakje in om uw taak op het dashboard te maken en klik vervolgens op **MAKEN**.
   
    ![taak wordt gemaakt](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)
5. In de rechterbovenhoek van het browservenster moet een bericht staan dat de implementatie is gestart. Dit wordt snel gewijzigd naar het venster 'Voltooid', zoals hieronder wordt weergegeven.
   
    ![taak wordt gemaakt](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

## <a name="create-an-azure-stream-analytics-query"></a>Een Azure Stream Analytics-query maken
Nadat de taak is gemaakt, is het tijd om deze te openen en een query te bouwen. U kunt de taak eenvoudig openen door op de bijbehorende tegel te klikken.

![Taaktegel](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Klik in het deelvenster **Taaktopologie** op het vak **QUERY** om naar de Query-editor te gaan. De **QUERY**-editor biedt u de mogelijkheid een T-SQL-query in te voeren die de transformatie van de binnenkomende gebeurtenisgegevens uitvoert.

![Queryvak](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Query: de onbewerkte gegevens archiveren
De meest eenvoudige vorm van een query is het doorgeven van gegevens waarmee alle invoergegevens worden gearchiveerd op de aangewezen uitvoer. Download het voorbeeldgegevensbestand van [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) naar een locatie op uw computer. 

1. Plak de query uit het bestand PassThrough.txt. 
   
    ![Test invoerstroom](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)
2. Klik op de drie puntjes naast uw invoer en selecteer het vak **Voorbeeldgegevens uit het bestand uploaden**.
   
    ![Test invoerstroom](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)
3. Er wordt nu een deelvenster aan de rechterkant geopend. Selecteer hierin het gegevensbestand HelloWorldASA InputStream.json van uw downloadlocatie en klik onder aan het deelvenster op **OK**.
   
    ![Test invoerstroom](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)
4. Klik vervolgens op het tandwiel **Testen** links boven in het venster en voer uw testquery uit op de voorbeeldgegevensset. Als de verwerking is voltooid, wordt onder uw query een resultatenvenster geopend.
   
    ![Testresultaten](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Query: gegevens filteren op basis van een voorwaarde
We gaan de resultaten filteren op basis van een voorwaarde. Wij willen graag alleen de resultaten van SensorA weergeven. De query bevindt zich in het bestand Filtering.txt.

![Een gegevensstroom filteren](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Houd er rekening mee dat de hoofdlettergevoelige query een stringwaarde vergelijkt. Klik nogmaals op het tandwiel **Testen** om de query uit te voeren. De query zou 389 rijen van de 1860 gebeurtenissen moeten retourneren.

![Tweede uitvoerresultaten van querytest](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Query: waarschuwingen om zakelijke werkstromen te activeren
We gaan onze query gedetailleerder maken. Voor elk type sensor willen we de gemiddelde temperatuur per tijdvenster van 30 seconden controleren en alleen resultaten weergeven als de gemiddelde temperatuur hoger is dan 100 graden. We schrijven de volgende query en klikken vervolgens op **Testen** om de resultaten te bekijken. U vindt de query in het bestand ThresholdAlerting.txt.

![30-secondenfilterquery](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Nu bevatten de resultaten nog maar 245 rijen en namen van sensoren die een gemiddelde temperatuur aangeven die hoger is dan 100 graden. In deze query wordt de stroom gebeurtenissen gegroepeerd op **dspl**, de sensornaam, en voor een **tumblingvenster** van 30 seconden. In tijdelijke query's moet worden vermeld hoe we willen dat de tijd wordt uitgevoerd. Met de **TIMESTAMP BY**-clausule hebben we de kolom **OUTPUTTIME** opgegeven om tijden te associëren met alle tijdelijke berekeningen. Voor gedetailleerde informatie leest u de MSDN-artikelen over [Time Management](https://msdn.microsoft.com/library/azure/mt582045.aspx) en [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx)-functies.

### <a name="query-detect-absence-of-events"></a>Query: detecteren van afwezigheid van gebeurtenissen
Hoe kunnen we een query schrijven om een gebrek aan invoergebeurtenissen te vinden? We willen weten wanneer een sensor het laatst gegevens heeft verstuurd en daarna vijf seconden lang geen gegevens meer heeft verstuurd. U vindt de query in bestand AbsenseOfEvent.txt.

![Detecteren van afwezigheid van gebeurtenissen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

We gebruiken hier een **LEFT OUTER**-join voor dezelfde gegevensstroom (self-join). Voor een **INNER**-join wordt alleen een resultaat geretourneerd wanneer een overeenkomst is gevonden.  Als in geval van een **LEFT OUTER**-join geen overeenkomst wordt gevonden met een gebeurtenis vanaf de linkerkant van de join, wordt een rij met NULL voor alle kolommen in de rechterkant geretourneerd. Deze techniek is heel nuttig om de afwezigheid van gebeurtenissen op te sporen. Raadpleeg onze MSDN-documentatie voor meer informatie over [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx).

## <a name="conclusion"></a>Conclusie
Het doel van deze zelfstudie is aantonen hoe u verschillende Stream Analytics-querytaalquery’s schrijft en de resultaten bekijkt in de browser. Maar dit slechts een begin. Met Stream Analytics hebt u nog veel meer mogelijkheden. Stream Analytics ondersteunt tal van in- en uitvoer en kan zelfs gebruikmaken van functies in Azure Machine Learning, waardoor het een krachtig hulpprogramma wordt om gegevensstromen te analyseren. Met ons [leeroverzicht](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/) kunt u nog meer te weten komen over Stream Analytics. Lees voor meer informatie over het schrijven van query's het artikel over [algemene querypatronen](stream-analytics-stream-analytics-query-patterns.md).

