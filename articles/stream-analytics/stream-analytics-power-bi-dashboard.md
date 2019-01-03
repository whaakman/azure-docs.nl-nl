---
title: Power BI-dashboard-integratie met Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u een realtime Power BI-dashboard gebruiken om gegevens uit een Azure Stream Analytics-taak te visualiseren.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: d7f67015d4df20ea39c1225d52be36340b8f65d1
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53556973"
---
# <a name="tutorial-stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Zelfstudie: Stream Analytics en Power BI: Een realtime analytics-dashboard voor het streamen van gegevens
Azure Stream Analytics kunt u profiteren van een van de toonaangevende hulpprogramma's voor bedrijfsinformatie [Microsoft Power BI](https://powerbi.com/). In dit artikel leert u hoe business intelligence-hulpmiddelen te maken met behulp van Power BI als uitvoer voor uw Azure Stream Analytics-taken. U leert ook hoe u het maken en gebruiken van een realtime dashboard.

Dit artikel gaat verder uit de Stream Analytics [fraudebewaking in realtime](stream-analytics-real-time-fraud-detection.md) zelfstudie. Het bouwt voort op de werkstroom in deze zelfstudie hebt gemaakt en wordt toegevoegd een Power BI uitvoeren zodat u frauduleuze telefoongesprekken die zijn gedetecteerd door een stream Analytics-taak kunt visualiseren. 

U kunt bekijken [een video](https://www.youtube.com/watch?v=SGUpT-a99MA) die dit scenario laat zien.


## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u het volgende hebt voordat u begint:

* Een Azure-account.
* Een account voor Power BI. U kunt een werkaccount of school-account gebruiken.
* Een voltooide versie van de [fraudebewaking in realtime](stream-analytics-real-time-fraud-detection.md) zelfstudie. De zelfstudie bevat een app die wordt gegenereerd fictieve telefoongesprek metagegevens. In de zelfstudie een event hub maken en verzenden van de streaminggegevens telefonische oproep naar de event hub. U schrijft een query die frauduleuze gesprekken (aanroepen van het aantal op hetzelfde moment op verschillende locaties) wordt gedetecteerd. 


## <a name="add-power-bi-output"></a>Power BI-uitvoer toevoegen
In de zelfstudie realtime fraude-detectie wordt de uitvoer verzonden naar Azure Blob-opslag. In deze sectie voegt u uitvoer gegevens naar Power BI verzendt.

1. Open de stream Analytics-taak die u eerder hebt gemaakt in de Azure-portal. Als u de voorgestelde naam gebruikt, wordt de taak met de naam `sa_frauddetection_job_demo`.

2. Selecteer de **uitvoer** vak in het midden van het dashboard van de taak en selecteer vervolgens **+ toevoegen**.

3. Voor **uitvoeralias**, voer `CallStream-PowerBI`. U kunt ook een andere naam gebruiken. Als u dit doet, noteer, omdat u de naam van de later nodig. 

4. Onder **Sink**, selecteer **Power BI**.

   ![Uitvoer maken voor Power BI](./media/stream-analytics-power-bi-dashboard/create-power-bi-ouptut.png)

5. Klik op **autoriseren**.

    Een venster geopend waarin u uw Azure-referenties kunt opgeven voor een account voor werk of school. 

    ![Geef referenties voor toegang tot Power BI](./media/stream-analytics-power-bi-dashboard/power-bi-authorization-credentials.png)

6. Voer uw referenties. Klik wanneer u uw referenties invoert, u ook toestemming voor de Streaming Analytics-taak geeft bent voor toegang tot uw Power BI-omgeving, worden op de hoogte.

7. Als u terug naar de **nieuwe uitvoer** blade, voer de volgende informatie:

    * **Werkruimte groep**: Selecteer een werkruimte in uw Power BI-tenant waar u om de gegevensset te maken.
    * **Naam van de gegevensset**:  Voer `sa-dataset` in. U kunt ook een andere naam gebruiken. Als u dit doet, moet u een notitie van deze voor later.
    * **Tabelnaam**: Voer `fraudulent-calls` in. Power BI-uitvoer van Stream Analytics-taken kan op dit moment slechts één tabel hebt in een gegevensset.

    ![Gegevensset van Power BI-werkruimte en tabel](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut-with-dataset-table.png)

    > [!WARNING]
    > Als Power BI een gegevensset en een tabel met dezelfde namen als degene die u in de Stream Analytics-taak opgeeft heeft, worden de bestaande bestanden overschreven.
    > Het is raadzaam dat u geen expliciet deze gegevensset en de tabel in uw Power BI-account maakt. Ze worden automatisch gemaakt wanneer u uw Stream Analytics-taak starten en de taak wordt gestart reageert uitvoer naar Power BI. Als uw job query geen resultaten oplevert, worden de gegevensset en de tabel niet gemaakt.
    >

8. Klik op **Create**.

De gegevensset is gemaakt met de volgende instellingen:

* **defaultRetentionPolicy: BasicFIFO**: Gegevens zijn FIFO, met een maximum van 200.000 rijen.
* **defaultMode: pushStreaming**: De gegevensset ondersteunt zowel streamingtegels en traditionele rapport op basis van visuele elementen (ook wel) push).

U kunt op dit moment gegevenssets maken met andere vlaggen.

Zie voor meer informatie over Power BI-gegevenssets, de [Power BI REST-API](https://msdn.microsoft.com/library/mt203562.aspx) verwijzing.


## <a name="write-the-query"></a>De query schrijven

1. Sluit de **uitvoer** blade en keer terug naar de taakblade.

2. Klik op de **Query** vak. 

3. Voer de volgende query. Deze query is vergelijkbaar met de query self-join die u hebt gemaakt in de zelfstudie van fraude. Het verschil is dat deze query resultaten verstuurt naar de nieuwe uitvoer die u hebt gemaakt (`CallStream-PowerBI`). 

    >[!NOTE]
    >Als u de invoer heeft niet de naam `CallStream` vervangen door uw naam in de zelfstudie van fraude `CallStream` in de **FROM** en **JOIN** componenten in de query.

        ```SQL
        /* Our criteria for fraud:
        Calls made from the same caller to two phone switches in different locations (for example, Australia and Europe) within five seconds */

        SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
        INTO "CallStream-PowerBI"
        FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
        JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime

        /* Where the caller is the same, as indicated by IMSI (International Mobile Subscriber Identity) */
        ON CS1.CallingIMSI = CS2.CallingIMSI

        /* ...and date between CS1 and CS2 is between one and five seconds */
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

        /* Where the switch location is different */
        WHERE CS1.SwitchNum != CS2.SwitchNum
        GROUP BY TumblingWindow(Duration(second, 1))
        ```

4. Klik op **Opslaan**.


## <a name="test-the-query"></a>De query testen
Deze sectie is optioneel, maar wordt aanbevolen. 

1. Als de app TelcoStreaming is niet wordt uitgevoerd, start de service door de volgende stappen:

    * Open een opdrachtvenster.
    * Ga naar de map waar de telcogenerator.exe en de gewijzigde telcodatagen.exe.config bestanden zijn.
    * Voer de volgende opdracht uit:

       `telcodatagen.exe 1000 .2 2`

2. In de **Query** blade, klik op de puntjes naast de `CallStream` invoer en selecteer vervolgens **voorbeeldgegevens van uitvoer**.

3. Opgeven dat u drie minuten aan gegevens en klik op **OK**. Wacht tot u een melding krijgt dat de gegevens zijn verzameld.

4. Klik op **Test** en controleer of u resultaten krijgt.


## <a name="run-the-job"></a>De taak uitvoeren

1. Zorg ervoor dat de TelcoStreaming-app wordt uitgevoerd.

2. Sluit de **Query** blade.

3. Klik op de taakblade **Start**.

    ![De Stream Analytics-taak starten](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

Uw stream Analytics-taak wordt gestart op zoek naar frauduleuze gesprekken in de stroom inkomende. De taak wordt gemaakt van de gegevensset en de tabel in Power BI ook en begint met het verzenden van gegevens over de frauduleuze gesprekken toe.


## <a name="create-the-dashboard-in-power-bi"></a>Maken van het dashboard in Power BI

1. Ga naar [Powerbi.com](https://powerbi.com) en meld u aan met uw werk of school-account. Als de query van de Stream Analytics-taak resultaten produceert, ziet u dat uw gegevensset al is gemaakt:

    ![Streaming van locatie van de gegevensset in Power BI](./media/stream-analytics-power-bi-dashboard/stream-analytics-streaming-dataset.png)

2. Klik in uw werkruimte  **+ &nbsp;maken**.

    ![De knop maken in Power BI-werkruimte](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. Maak een nieuw dashboard en noem dit `Fraudulent Calls`.

    ![Een dashboard maken en geef deze een naam in Power BI-werkruimte](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. Aan de bovenkant van het venster, klikt u op **tegel toevoegen**, selecteer **aangepaste STREAMINGGEGEVENS**, en klik vervolgens op **volgende**.

    ![Aangepaste streaming-gegevensset-tegel in Power BI](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. Onder **uw DATSETS**, selecteert u uw gegevensset en klik vervolgens op **volgende**.

    ![Uw streaming-gegevensset in Power BI](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. Onder **Visualisatietype**, selecteer **kaart**, en klik vervolgens in de **velden** in de lijst met **fraudulentcalls**.

    ![Details van de visualisatie voor nieuwe tegel](./media/stream-analytics-power-bi-dashboard/add-fraudulent-calls-tile.png)

7. Klik op **volgende**.

8. Vul de details van de tegel, zoals een titel en subtitel.

    ![Titel en subtitel van de nieuwe tegel](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. Klik op **Toepassen**.

    U hebt nu een teller fraude.

    ![Fraude teller in Power BI-dashboard](./media/stream-analytics-power-bi-dashboard/power-bi-fraud-counter-tile.png)

8. Volg de stappen voor het toevoegen van een tegel (te beginnen met stap 4). Deze keer het volgende doen:

    * Wanneer **Visualisatietype**, selecteer **lijndiagram**. 
    * Voeg een as toe en selecteer **windowend**. 
    * Voeg een waarde toe en selecteer **fraudulentcalls**.
    * Selecteer bij **Tijdvenster voor weergave** de laatste 10 minuten.

    ![Een tegel voor lijndiagram maken in Power BI](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. Klik op **volgende**, een titel en subtitel toevoegen en klik op **toepassen**.

    De Power BI-dashboard nu kunt u twee weergaven van gegevens over frauduleuze gesprekken als gedetecteerd in de streaming-gegevens.

    ![Power BI-dashboard met twee tegels frauduleuze gesprekken voltooid](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a>Meer informatie over Power BI

Deze zelfstudie wordt gedemonstreerd hoe u slechts een paar soorten visualisaties voor een gegevensset maken. Power BI kunt u bij het maken van andere klanten business intelligence-hulpmiddelen voor uw organisatie. Zie de volgende bronnen voor meer ideeën:

* Bekijk voor een ander voorbeeld van een Power BI-dashboard de [aan de slag met Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) video.
* Voor meer informatie over het configureren van stream Analytics job uitvoer naar Power BI en Power BI-groepen gebruikt, Controleer de [Power BI](stream-analytics-define-outputs.md#power-bi) sectie van de [Stream Analytics levert](stream-analytics-define-outputs.md) artikel. 
* Zie voor meer informatie over het gebruik van Power BI algemeen [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).


## <a name="learn-about-limitations-and-best-practices"></a>Meer informatie over de beperkingen en aanbevolen procedures
Op dit moment kan Power BI ongeveer één keer per seconde worden aangeroepen. Visuele elementen ondersteuning bieden voor pakketten van 15 KB. Verder, mislukt de visuele elementen (maar push blijft werken). Vanwege deze beperkingen, Power BI meest is van nature geschikt in gevallen waarbij Azure Stream Analytics een vermindering van belangrijke gegevens laden biedt. We adviseren een Tumblingvenstertrigger venster of Hopping gebruiken om ervoor te zorgen dat gegevens-push is maximaal één push per seconde en dat uw query binnen de vereisten voor doorvoer terechtkomt.

U kunt de volgende vergelijking gebruiken om de waarde zodat het venster in een paar seconden te berekenen:

![Vergelijking waarde zodat venster in een paar seconden te berekenen](./media/stream-analytics-power-bi-dashboard/compute-window-seconds-equation.png)  

Bijvoorbeeld:

* U hebt 1000 apparaten verzenden van gegevens met tussenpozen van één seconde.
* U kunt de Power BI Pro-SKU die ondersteuning biedt voor 1.000.000 rijen per uur worden gebruikt.
* Wilt u de hoeveelheid gemiddelde gegevens per apparaat publiceren naar Power BI.

Als gevolg hiervan wordt de vergelijking:

![Vergelijking op basis van voorbeeldcriteria](./media/stream-analytics-power-bi-dashboard/power-bi-example-equation.png)  

Deze configuratie worden gegeven, kunt u de oorspronkelijke query wijzigen met de volgende:

```SQL
    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO "CallStream-PowerBI"
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl
```

### <a name="renew-authorization"></a>Autorisatie vernieuwen
Als het wachtwoord is gewijzigd sinds de taak is gemaakt of laatst geverifieerd, moet u uw Power BI-account te verifiëren. Als u Azure multi-factor Authentication is geconfigureerd op uw tenant Azure Active Directory (Azure AD), moet u ook om te vernieuwen van Power BI-autorisatie elke twee weken. Als u niet verlengt, kan er problemen, zoals een gebrek aan taakuitvoer of een `Authenticate user error` in de logboeken voor bewerkingen.

Op dezelfde manier als een taak wordt gestart nadat het token is verlopen, een fout optreedt en de taak is mislukt. U lost dit probleem, de taak die wordt uitgevoerd stoppen en Ga naar de uitvoer van uw Power BI. Om te voorkomen dat gegevens verloren gaan, selecteert u de **autorisatie vernieuwen** koppelen en start de taak van de **gestopt laatst**.

Nadat de autorisatie is vernieuwd met Power BI, wordt een waarschuwing met een groen weergegeven in de autorisatie om weer te geven dat het probleem is opgelost.

## <a name="get-help"></a>Help opvragen
Voor verdere ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language-referentie](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API-naslaginformatie](https://msdn.microsoft.com/library/azure/dn835031.aspx)
