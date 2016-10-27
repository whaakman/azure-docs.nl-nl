<properties
    pageTitle="Aan de slag met Azure Stream Analytics om gegevens te verwerken van IoT-apparaten | Microsoft Azure"
    description="IoT-sensortags en -gegevensstromen met Stream Analytics en realtime-gegevensverwerking"
    keywords="IOT-oplossing, aan de slag met iot"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# Aan de slag met Azure Stream Analytics om gegevens te verwerken van IoT-apparaten

In deze zelfstudie leert u hoe u stroomverwerkingslogica schrijft om gegevens te verzamelen van IoT-apparaten (Internet der dingen). We gebruiken hier een echte IoT-gebruikstoepassing (Internet der dingen) om aan te tonen hoe u snel en economisch een oplossing maakt.

## Vereisten

-   [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/)
-   Voorbeeldquery en gegevensbestanden die u kunt downloaden van [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## Scenario

Contoso, een bedrijf op het gebied van industriële automatisering, heeft hun fabricageproces volledig geautomatiseerd. De machines in dit bedrijf hebben sensoren die in realtime gegevensstromen kunnen genereren. In dit scenario wil een werkvloerbeheerder realtime-inzichten op basis van de sensorgegevens om naar patronen te zoeken en hierop te reageren. We passen de SAQL (Stream Analytics Query Language) op de sensorgegevens toe om interessante patronen te vinden in de stroom inkomende gegevens.

Hier worden gegevens gegenereerd met een Texas Instrument Sensor Tag-apparaat.

![Texas Instruments Sensor Tag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

De nettolading van de gegevens heeft de JSON-indeling en ziet er ongeveer als volgt uit:


    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

In een werkelijk scenario hebt u honderden van dit soort sensoren die gebeurtenissen als een stroom kunnen genereren. In het ideale geval is er een gateway-apparaat waarop bepaalde code wordt uitgevoerd die deze gebeurtenissen pusht naar [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) of [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/). Uw Stream Analytics-taak zou deze gebeurtenissen opnemen van Event Hubs en realtime analysequery’s uitvoeren tegen de stromen. Dan kunt u de resultaten verzenden naar één van de [ondersteunde outputs](stream-analytics-define-outputs.md).

Voor het gebruiksgemak biedt deze introductiehandleiding een bestand met voorbeeldgegevens dat is verkregen uit echte Sensor Tag-apparaten. U kunt query's uitvoeren op de voorbeeldgegevens en resultaten weergeven. In volgende zelfstudies leert u hoe u een taak verbindt met in- en uitvoer, en deze implementeert in de Azure-service.

## Een Stream Analytics-taak maken

1. Klik in de [Azure Portal](http://manage.windowsazure.com) op **STREAM ANALYTICS** en klik vervolgens op **NIEUW** linksonder op de pagina om een nieuwe analysetaak te maken.

    ![Een nieuwe Stream Analytics-taak maken](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

2. Klik op **SNELLE INVOER**.

3. Selecteer voor instelling **OPSLAGACCOUNT REGIONALE CONTROLE** klikt u op **NIEUW OPSLAGACCOUNT MAKEN** en geeft u het een unieke naam. Dit account wordt in Azure Stream Analytics gebruikt om de controlegegevens voor alle toekomstige taken op te slaan.

    > [AZURE.NOTE] U maakt dit opslagaccount slechts één keer per regio. Deze opslag wordt gedeeld tussen alle Stream Analytics-taken die zijn gemaakt in deze regio.

4. Klik onder aan de pagina op **STREAM ANALYTICS-TAAK MAKEN**.

    ![Opslagaccountconfiguratie](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.jpg)

## Azure Stream Analytics-query

Klik op het tabblad **QUERY** om naar de Queryeditor te gaan. Het tabblad **QUERY** bevat een T-SQL-query die de transformatie van de binnenkomende gebeurtenisgegevens uitvoert.

## De onbewerkte gegevens archiveren

De meest eenvoudige vorm van een query is het doorgeven van gegevens waarmee alle invoergegevens worden gearchiveerd op de aangewezen uitvoer.

![Archieftaakquery](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Download nu het voorbeeldgegevensbestand van [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) naar een locatie op uw computer. Plak de query uit het bestand PassThrough.txt. Klik op de knop **Testen** en selecteer het gegevensbestand met de naam HalloWereldASA-InputStream.json van uw downloadlocatie.

![Testknop in Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

![Test invoerstroom](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

U kunt de resultaten van de query in de volgende schermafbeelding zien.

![Testresultaten](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

## Gegevens filteren op basis van een voorwaarde

We gaan de resultaten filteren op basis van een voorwaarde. Wij willen graag alleen de resultaten van SensorA weergeven. De query bevindt zich in het bestand Filtering.txt.

![Een gegevensstroom filteren](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Houd er rekening mee dat de hoofdlettergevoelige query een stringwaarde vergelijkt. Klik op de knop **Opnieuw uitvoeren** om de query uit te voeren. De query zou 389 rijen van de 1860 gebeurtenissen moeten retourneren.

![Tweede uitvoerresultaten van querytest](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

## Waarschuwingen om zakelijke werkstromen te activeren

We gaan onze query gedetailleerder maken. Voor elk type sensor willen we de gemiddelde temperatuur per tijdvenster van 30 seconden controleren en alleen resultaten weergeven als de gemiddelde temperatuur hoger is dan 100 graden. We schrijven de volgende query en klikken vervolgens op **Opnieuw uitvoeren** om de resultaten te bekijken. U vindt de query in het bestand ThresholdAlerting.txt.

![30-secondenfilterquery](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Nu bevatten de resultaten nog maar 245 rijen en namen van sensoren die een gemiddelde temperatuur aangeven die hoger is dan 100 graden. In deze query wordt de stroom gebeurtenissen gegroepeerd op **dspl**, de sensornaam, en voor een **tumblingvenster** van 30 seconden. In tijdelijke query's moet worden vermeld hoe we willen dat de tijd wordt uitgevoerd. Met de **TIMESTAMP BY**-clausule hebben we de kolom **OUTPUTTIME** opgegeven om tijden te associëren met alle tijdelijke berekeningen. Voor gedetailleerde informatie leest u de MSDN-artikelen over [Time Management](https://msdn.microsoft.com/library/azure/mt582045.aspx) en [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx)-functies.

![Tijdelijke meer dan 100](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

## Detecteren van afwezigheid van gebeurtenissen

Hoe kunnen we een query schrijven om een gebrek aan invoergebeurtenissen te vinden? We willen weten wanneer een sensor het laatst gegevens heeft verstuurd en daarna een minuut lang geen gegevens meer heeft verstuurd. U vindt de query in bestand AbsenseOfEvent.txt.

![Detecteren van afwezigheid van gebeurtenissen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-12.png)

We gebruiken hier een **LEFT OUTER**-join voor dezelfde gegevensstroom (self-join). Voor een **INNER**-join wordt alleen een resultaat geretourneerd wanneer een overeenkomst is gevonden.  Als in geval van een **LEFT OUTER**-join geen overeenkomst wordt gevonden met een gebeurtenis vanaf de linkerkant van de join, wordt een rij met NULL voor alle kolommen in de rechterkant geretourneerd. Deze techniek is heel nuttig om de afwezigheid van gebeurtenissen op te sporen. Raadpleeg onze MSDN-documentatie voor meer informatie over [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx).

![Join-resultaten](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-13.png)

## Conclusie

Het doel van deze zelfstudie is aantonen hoe u verschillende Stream Analytics-querytaalquery’s schrijft en de resultaten bekijkt in de browser. Maar dit slechts een begin. Met Stream Analytics hebt u nog veel meer mogelijkheden. Stream Analytics ondersteunt tal van in- en uitvoer en kan zelfs gebruikmaken van functies in Azure Machine Learning, waardoor het een krachtig hulpprogramma wordt om gegevensstromen te analyseren. Met ons [leeroverzicht](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/) kunt u nog meer te weten komen over Stream Analytics. Lees voor meer informatie over het schrijven van query's het artikel over [algemene querypatronen](./stream-analytics-stream-analytics-query-patterns.md).



<!--HONumber=Oct16_HO3-->


