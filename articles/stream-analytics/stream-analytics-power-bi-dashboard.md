---
title: Power BI-dashboard op Azure Stream Analytics | Microsoft Docs
description: Gebruik een realtime streaming Power BI-dashboard business intelligence verzamelen en analyseren van grote hoeveelheden gegevens uit een Stream Analytics-taak.
keywords: dashboard met analytische, realtime dashboard
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: fe8db732-4397-4e58-9313-fec9537aa2ad
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: samacha
ms.openlocfilehash: b446e2296f2747012849936b994c4a4a2044869e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Stream Analytics en Power BI: een realtime analytics-dashboard voor het streamen van gegevens
Azure Stream Analytics kunt u profiteren van een van de toonaangevende hulpprogramma's voor bedrijfsinformatie, [Microsoft Power BI](https://powerbi.com/). In dit artikel leert u hoe hulpprogramma's voor bedrijfsinformatie te maken met behulp van Power BI als uitvoer voor uw Azure Stream Analytics-taken. U leert ook hoe maken en gebruiken van een realtime dashboard.

In dit artikel wordt voortgezet vanaf de Stream Analytics [realtime-fraudedetectie](stream-analytics-real-time-fraud-detection.md) zelfstudie. Het is gebaseerd op de werkstroom in deze zelfstudie hebt gemaakt en voegt een Power BI zodat u kunt visualiseren frauduleuze telefoongesprekken die zijn gedetecteerd door een Streaming Analytics-taak uitvoeren. 

U kunt bekijken [video](https://www.youtube.com/watch?v=SGUpT-a99MA) die dit scenario laat zien.


## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg er dan voor dat u hebt het volgende:

* Een Azure-account.
* Een account voor Power BI. U kunt een werk- of een schoolaccount gebruiken.
* Een voltooide versie van de [realtime-fraudedetectie](stream-analytics-real-time-fraud-detection.md) zelfstudie. De zelfstudie omvat een app die fictieve telefoongesprek metagegevens genereert. In de zelfstudie een event hub maken en streaming telefoongesprek gegevens verzenden naar de event hub. U een query schrijven waarmee frauduleuze aanroepen (aanroepen van het aantal op hetzelfde moment op verschillende locaties detecteert). 


## <a name="add-power-bi-output"></a>Power BI-uitvoer toevoegen
In de zelfstudie realtime fraude-detectie wordt de uitvoer verzonden naar Azure Blob-opslag. In deze sectie voegt u een uitvoer die wordt informatie naar Power BI verzonden.

1. Open de Streaming Analytics-taak die u eerder hebt gemaakt in de Azure-portal. Als u de voorgestelde naam gebruikt, de taak is met de naam `sa_frauddetection_job_demo`.

2. Selecteer de **uitvoer** vak in het midden van het dashboard van de taak en selecteer vervolgens **+ toevoegen**.

3. Voor **Uitvoeraliassen**, voer `CallStream-PowerBI`. U kunt een andere naam gebruiken. Als u dit doet, noteer, omdat u de naam van de later nodig. 

4. Onder **Sink**, selecteer **Power BI**.

   ![Maken van een uitvoer voor Power BI](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut.png)

5. Klik op **autoriseren**.

    Een venster wordt geopend waarin u uw Azure-referenties voor een werk- of schoolaccount account kunt opgeven. 

    ![Geef referenties voor toegang met Power BI](./media/stream-analytics-power-bi-dashboard/authorize-area.png)

6. Geef uw referenties. Let en wanneer u uw referenties invoeren, u ook toestemming voor de Streaming Analytics-taak geeft bent voor toegang tot uw Power BI-gebied.

7. Wanneer u wordt teruggeleid naar de **nieuwe uitvoer** blade, voer de volgende informatie:

    * **Werkruimte groep**: Selecteer een werkruimte in uw Power BI-tenant waar u wilt maken van de gegevensset.
    * **Naam van DataSet**: Voer `sa-dataset`. U kunt een andere naam gebruiken. Als u dit doet, noteer deze later.
    * **Tabelnaam**: Voer `fraudulent-calls`. Power BI-uitvoer van de Stream Analytics-taken zijn op dit moment slechts één tabel in een dataset.

    ![PBI-werkruimte](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut-with-dataset-table.png)

    > [!WARNING]
    > Als Power BI heeft een gegevensset en een tabel met dezelfde namen als de waarden die u in de Stream Analytics-taak opgeeft, een bestaande overschreven.
    > U wordt aangeraden dat u niet expliciet deze gegevensset en de tabel in uw Power BI-account maakt. Ze worden automatisch gemaakt wanneer u uw Stream Analytics-taak start en de taak gemalen uitvoer naar Power BI begint. Als de taak query geen resultaten oplevert, worden de gegevensset en de tabel niet gemaakt.
    >

8. Klik op **Create**.

De gegevensset is gemaakt met de volgende instellingen:

* **defaultRetentionPolicy: BasicFIFO**: gegevens zijn FIFO, met een maximum van 200.000 rijen.
* **defaultMode: pushStreaming**: de dataset ondersteunt streaming tegels en traditionele op basis van een rapport visuele elementen (ook push).

U kunt op dit moment gegevenssets maken met andere vlaggen.

Zie voor meer informatie over Power BI-gegevenssets, de [Power BI REST-API](https://msdn.microsoft.com/library/mt203562.aspx) verwijzing.


## <a name="write-the-query"></a>De query schrijven

1. Sluit de **uitvoer** blade en keer terug naar de taakblade.

2. Klik op de **Query** vak. 

3. Voer de volgende query. Deze query is vergelijkbaar met de query self-join die u hebt gemaakt in de zelfstudie fraude te detecteren. Het verschil is dat deze query resultaten verstuurt naar de nieuwe uitvoer die u hebt gemaakt (`CallStream-PowerBI`). 

    >[!NOTE]
    >Als u de invoer heeft niet de naam `CallStream` vervangen door uw naam in de zelfstudie detecteren van fraude `CallStream` in de **FROM** en **JOIN** componenten in de query.

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

4. Klik op **Opslaan**.


## <a name="test-the-query"></a>De query testen
Deze sectie is optioneel maar aanbevolen. 

1. Als de app TelcoStreaming momenteel niet wordt uitgevoerd, start u deze met de volgende stappen:

    * Open een opdrachtvenster.
    * Ga naar de map waarin de telcogenerator.exe en telcodatagen.exe.config gewijzigde bestanden zijn.
    * Voer de volgende opdracht uit:

            telcodatagen.exe 1000 .2 2

2. In de **Query** blade, klik op de puntjes naast de `CallStream` invoer- en selecteer vervolgens **voorbeeldgegevens uit invoer**.

3. U wilt opgeven dat drie minuten aan gegevens en klik op **OK**. Wacht totdat u een bericht dat de gegevens steekproefgewijs verkregen.

4. Klik op **Test** en zorg ervoor dat u resultaten krijgt.


## <a name="run-the-job"></a>De taak uitvoeren

1. Zorg ervoor dat de app TelcoStreaming wordt uitgevoerd.

2. Sluit de **Query** blade.

3. Klik op de taakblade **Start**.

    ![De Stream Analytics-taak starten](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

Uw Streaming Analytics-taak wordt gestart frauduleuze aanroepen in de stroom inkomende zoekt. De taak wordt gemaakt van de gegevensset en de tabel in Power BI ook en begint met het verzenden van gegevens over de frauduleuze aanroepen naar deze.


## <a name="create-the-dashboard-in-power-bi"></a>Het dashboard maken in Power BI

1. Ga naar [Powerbi.com](https://powerbi.com) en meld u aan met uw werk of school-account. Als de taak Stream Analytics query worden de resultaten, ziet u dat uw gegevensset al is gemaakt:

    ![Streaming gegevensset in Power BI](./media/stream-analytics-power-bi-dashboard/streaming-dataset.png)

2. Klik in de werkruimte op  **+ &nbsp;maken**.

    ![De knop maken in Power BI-werkruimte](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. Maak een nieuw dashboard en noem deze `Fraudulent Calls`.

    ![Een dashboard maken en hieraan een naam in Power BI-werkruimte](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. Klik boven in het venster op **toevoegen tegel**, selecteer **aangepaste STREAMINGGEGEVENS**, en klik vervolgens op **volgende**.

    ![Aangepaste gegevensset streaming](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. Onder **uw DATSETS**, selecteer uw gegevensset en klik vervolgens op **volgende**.

    ![Uw streaminggegevensset](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. Onder **visualisatie Type**, selecteer **kaart**, en klik vervolgens in de **velden** selecteert **fraudulentcalls**.

    ![Details van de visualisatie voor nieuwe tegel](./media/stream-analytics-power-bi-dashboard/add-fraud.png)

7. Klik op **Volgende**.

8. Vul in de tegel-gegevens, zoals een titel en subtitel.

    ![Titel en subtitel voor nieuwe tegel](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. Klik op **Toepassen**.

    U hebt nu een teller fraude.

    ![Fraude teller](./media/stream-analytics-power-bi-dashboard/fraud-counter.png)

8. Volg de stappen voor het toevoegen van een tegel (te beginnen bij stap 4). Dit moment is het volgende doen:

    * Wanneer **visualisatie Type**, selecteer **lijndiagram**. 
    * Een as toevoegen en selecteer **windowend**. 
    * Een waarde toevoegen en selecteer **fraudulentcalls**.
    * Voor **tijdvenster om weer te geven**, selecteert u de laatste 10 minuten.

    ![Tegel lijndiagram maken](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. Klik op **volgende**, het toevoegen van een titel en subtitel en klikt u op **toepassen**.

    De Power BI-dashboard kunt u nu twee weergaven van gegevens over frauduleuze aanroepen gevonden in de streaming-gegevens.

    ![Klaar met Power BI-dashboard met twee tegels voor frauduleuze aanroepen](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a>Meer informatie over Power BI

Deze zelfstudie laat zien hoe om slechts een paar soorten visualisaties voor een gegevensset te maken. Power BI kunt u andere klant hulpprogramma's voor bedrijfsinformatie voor uw organisatie te maken. Zie de volgende bronnen voor meer ideeën:

* Bekijk voor een ander voorbeeld van een Power BI-dashboard de [aan de slag met Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) video.
* Taak voor meer informatie over het configureren van Streaming Analytics uitvoer naar Power BI en met behulp van groepen van Power BI, Controleer de [Power BI](stream-analytics-define-outputs.md#power-bi) sectie van de [Stream Analytics levert](stream-analytics-define-outputs.md) artikel. 
* Zie voor meer informatie over het gebruik van Power BI in het algemeen [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).


## <a name="learn-about-limitations-and-best-practices"></a>Meer informatie over de beperkingen en best practices
Op dit moment kunnen Power BI ongeveer één keer per seconde worden aangeroepen. Streaming visuele elementen ondersteuning bieden voor pakketten van 15 KB. Daarna streaming visuele elementen mislukken, maar push blijft werken. Vanwege deze beperkingen Power BI gepaard meest natuurlijk met gevallen waarbij Azure Stream Analytics een aanzienlijke hoeveelheid gegevens laden beperken biedt. U kunt het beste een tumblingvenster of Hopping venster gebruikt om ervoor te zorgen dat gegevens-push hooguit één push per seconde is en dat uw query belandt binnen de doorvoer-vereisten.

U kunt de volgende vergelijking gebruiken voor het berekenen van de waarde zodat uw venster (in seconden):

![Equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Bijvoorbeeld:

* U hebt 1000 apparaten verzenden van gegevens met een interval van één seconde.
* U gebruikt de Power BI Pro-SKU die ondersteuning biedt voor 1.000.000 rijen per uur.
* Wilt u de hoeveelheid gemiddelde gegevens per apparaat publiceren naar Power BI.

Als gevolg hiervan is dat de vergelijking wordt:

![Equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

In deze configuratie kunt u de oorspronkelijke query wijzigen met het volgende:

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


### <a name="renew-authorization"></a>Verificatie vernieuwen
Als het wachtwoord is gewijzigd sinds de taak is gemaakt of laatst geverifieerd, moet u uw Power BI-account te verifiëren. Als u Azure multi-factor Authentication is geconfigureerd op de tenant van uw Azure Active Directory (Azure AD), moet u ook voor het vernieuwen van Power BI-autorisatie elke twee weken. Als u niet verlengt, kan er problemen zoals een gebrek aan taakuitvoer of een `Authenticate user error` in de bewerkingslogboeken.

Als een taak wordt gestart nadat het token is verlopen, een fout optreedt en de taak is mislukt. U lost dit probleem, stop de taak die wordt uitgevoerd en gaat u naar uw Power BI-uitvoer. Om gegevensverlies te voorkomen, selecteert u de **vernieuwen autorisatie** koppeling en start de taak in de **laatste tijd geëindigd**.

Nadat de autorisatie is vernieuwd met Power BI, verschijnt een groen waarschuwing in het gebied autorisatie om weer te geven dat het probleem is opgelost.

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Naslaginformatie over Azure Stream Analytics Management REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
