---
title: Realtime Twitter-sentimentanalyse met Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u Stream Analytics gebruikt voor realtime Twitter-sentimentanalyse. Stapsgewijze richtlijnen van genereren van gebeurtenissen naar gegevens op een live-dashboard.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
manager: kfile
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/29/2017
ms.openlocfilehash: c34b3fa1597866e4f722fccee84f83f4caab6bfb
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56876293"
---
# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Realtime Twitter-sentimentanalyse in Azure Stream Analytics

Meer informatie over het bouwen van een sentiment-analyse-oplossing voor analyse van sociale media door te brengen van realtime Twitter-gebeurtenissen naar Azure Event Hubs. U kunt vervolgens schrijven die een Azure Stream Analytics-query voor het analyseren van de gegevens en een opslaan van de resultaten voor later gebruik of gebruik een dashboard en [Power BI](https://powerbi.com/) waarmee u inzicht in realtime kunt.

Sociale media analytics-hulpprogramma's ondersteuning van organisaties actuele onderwerpen op. Actuele onderwerpen zijn onderwerpen en houding waarvoor een groot aantal berichten in sociale media. Sentimentanalyse, ook wel genoemd *advies analysestructuur*, sociale media analytics-hulpprogramma's gebruikt om te bepalen van houding richting van een product, idee, enzovoort. 

Analyse van realtime Twitter-trend is een goed voorbeeld van een analytics-hulpprogramma, omdat het abonnement hashtag model kunt u luisteren naar specifieke trefwoorden (hashtags) en sentimentanalyse van de feed ontwikkelen.

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a>Scenario: Analyse van sociale media gevoel in realtime

Een bedrijf dat een website nieuwsmedia is geïnteresseerd in het verkrijgen van een voordeel ten opzichte van zijn concurrenten door site-inhoud die onmiddellijk relevant is voor de lezers. Het bedrijf maakt gebruik van analyse van sociale media op onderwerpen die relevant voor lezers zijn door te doen van een realtime sentimentanalyse van Twitter-gegevens.

Voor het identificeren van actuele onderwerpen in realtime op Twitter, moet het bedrijf realtime analyses over de tweet volume en het gevoel voor belangrijke onderwerpen. Met andere woorden, is de noodzaak een sentiment-analyse analyse-engine die gebaseerd op deze sociale media-feed.

## <a name="prerequisites"></a>Vereisten
In deze zelfstudie gebruikt u een clienttoepassing die verbinding maakt met Twitter en wordt gekeken naar tweets waarvoor bepaalde hashtags (die u kunt instellen). Als u wilt uitvoeren van de toepassing en analyseren van de tweets met behulp van Azure stream Analytics, moet u het volgende hebt:

* Een Azure-abonnement
* Een Twitter-account 
* Een Twitter-toepassing en de [OAuth-toegangstoken](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) voor die toepassing. We bieden op hoog niveau instructies voor het maken van een Twitter-toepassing later opnieuw.
* De toepassing TwitterWPFClient, die de Twitter-feed leest. Als u deze toepassing, downloaden de [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) bestand van GitHub en pak het pakket vervolgens naar een map op uw computer. Als u zien van de bron wilt-code en de toepassing uitvoert in een debugger, krijgt u de broncode van [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/TwitterClient). 

## <a name="create-an-event-hub-for-streaming-analytics-input"></a>Maken van een event hub voor invoer van de stream Analytics

De voorbeeldtoepassing gebeurtenissen genereert en nieuwe versies naar een Azure event hub. Azure eventhubs zijn de voorkeursmethode voor gebeurtenisopname voor Stream Analytics. Zie voor meer informatie de [documentatie voor Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).


### <a name="create-an-event-hub-namespace-and-event-hub"></a>Een event hub-naamruimte en event hub maken
In deze procedure maakt u eerst een event hub-naamruimte maken en vervolgens het toevoegen van een event hub aan die naamruimte. Event hub-naamruimten worden gebruikt voor het groeperen van gerelateerde gebeurtenissen bus exemplaren. 

1. Meld u aan bij de Azure-portal en klikt u op **een resource maken** > **Internet of Things** > **Event Hub**. 

2. In de **-naamruimte maken** blade, voer de naam van een naamruimte zoals `<yourname>-socialtwitter-eh-ns`. U kunt een willekeurige naam voor de naamruimte, maar de naam mag niet geldig voor een URL en deze moet uniek zijn binnen Azure. 
    
3. Selecteer een abonnement en maak of kies een resourcegroep en klik vervolgens op **maken**. 

    ![Een Event Hub-naamruimte maken](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-namespace.png)
 
4. De naamruimte is geïmplementeerd, vindt de event hub-naamruimte in de lijst met Azure-resources. 

5. Klik op de nieuwe naamruimte, en klik op de blade naamruimte  **+ &nbsp;Event Hub**. 

    ![De knop Event Hub toevoegen voor het maken van een nieuwe event hub](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-button.png)    
 
6. Naam van de nieuwe event hub `socialtwitter-eh`. U kunt ook een andere naam gebruiken. Als u dit doet, noteer, omdat u de naam van de later nodig. U hoeft niet te stel eventuele andere opties voor de event hub.

    ![Blade voor het maken van een nieuwe event hub](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub.png)
 
7. Klik op **Create**.


### <a name="grant-access-to-the-event-hub"></a>Toegang verlenen tot de event hub

Voordat een proces u gegevens naar een event hub verzenden kunt, moet de event hub een beleid waarmee u de juiste toegang hebben. Het toegangsbeleid genereert een verbindingsreeks die autorisatiegegevens bevat.

1.  Klik op de blade event naamruimte **Event Hubs** en klik vervolgens op de naam van uw nieuwe event hub.

2.  Klik in de event hub-blade op **beleid voor gedeelde toegang** en klik vervolgens op  **+ &nbsp;toevoegen**.

    >[!NOTE]
    >Zorg ervoor dat u werkt met de event hub, niet de event hub-naamruimte.

3.  Toevoegen van een beleid met de naam `socialtwitter-access` en voor **Claim**, selecteer **beheren**.

    ![Blade voor het maken van een nieuw toegangsbeleid voor event hub](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-shared-access-policy-manage.png)
 
4.  Klik op **Create**.

5.  Nadat het beleid is geïmplementeerd, klikt u erop in de lijst met beleid voor gedeelde toegang.

6.  In het vak **VERBINDINGSREEKS-primaire sleutel** en klikt u op de knop kopiëren naast de verbindingsreeks. 
    
    ![Kopiëren van de sleutel van de tekenreeks primaire verbinding van het toegangsbeleid](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-shared-access-policy-copy-connection-string.png)
 
7.  Plak de verbindingsreeks in een teksteditor. U hebt deze verbindingsreeks nodig voor de volgende sectie, nadat u enkele kleine wijzigingen in het aanbrengt.

    De verbindingsreeks ziet er als volgt:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=;EntityPath=socialtwitter-eh

    U ziet dat de verbindingsreeks meerdere sleutel / waarde-paren, gescheiden door puntkomma's bevat: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, en `EntityPath`.  

    > [!NOTE]
    > Voor beveiliging, zijn delen van de verbindingsreeks in het voorbeeld verwijderd.

8.  In de teksteditor verwijdert de `EntityPath` paar uit de verbindingsreeks (Vergeet niet om te verwijderen van de puntkomma). Wanneer u klaar bent, wordt de verbindingsreeks ziet er als volgt:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=


## <a name="configure-and-start-the-twitter-client-application"></a>Configureren en starten van de Twitter-clienttoepassing
De clienttoepassing worden tweet gebeurtenissen rechtstreeks uit Twitter opgehaald. Als u wilt doen, moet deze machtiging voor de Twitter-Streaming-API-'s aanroepen. Als u wilt configureren die machtiging, maakt u een toepassing in Twitter, dat wordt gegenereerd unieke referenties (zoals een OAuth-token). Vervolgens kunt u de clienttoepassing moet worden deze referenties gebruikt bij het API-aanroepen maakt. 

### <a name="create-a-twitter-application"></a>Een Twitter-toepassing maken
Als u nog geen een Twitter-toepassing die u voor deze zelfstudie gebruiken kunt, kunt u een kunt maken. U moet al een Twitter-account hebben.

> [!NOTE]
> De exacte verwerken in Twitter voor het maken van een toepassing en het ophalen van de sleutels, geheimen en token kan worden gewijzigd. Als deze instructies niet overeenkomen met wat u ziet op de Twitter-site, raadpleegt u de Twitter-documentatie voor ontwikkelaars.

1. Ga naar de [Twitter application management-pagina](https://apps.twitter.com/). 

2. Maak een nieuwe toepassing. 

    * Geef een geldige URL voor de website-URL. Dit heeft geen een live site. (U kunt niet alleen opgeven `localhost`.)
    * De callback-veld leeg laten. De clienttoepassing die u voor deze zelfstudie gebruikt vereist geen callbacks.

    ![Het maken van een toepassing in Twitter](./media/stream-analytics-twitter-sentiment-analysis-trends/create-twitter-application.png)

3. Wijzig eventueel de machtigingen van de toepassing alleen-lezen.

4. Als de toepassing is gemaakt, gaat u naar de **Keys and Access Tokens** pagina.

5. Klik op de knop voor het genereren van een access-token en access token geheim.

Houd deze informatie handig, omdat u deze nodig heeft in de volgende procedure.

>[!NOTE]
>De sleutels en geheimen voor de Twitter-toepassing bieden toegang tot uw Twitter-account. Deze informatie worden behandeld als gevoelig hetzelfde zijn als u uw Twitter-wachtwoord. Bijvoorbeeld, niet in een toepassing die u aan anderen toewijst deze informatie te sluiten. 


### <a name="configure-the-client-application"></a>De clienttoepassing configureren
We hebben gemaakt dat een clienttoepassing die verbinding met Twitter-gegevens met maakt [Twitter van Streaming-API's](https://dev.twitter.com/streaming/overview) voor het verzamelen van gebeurtenissen van de tweet over een specifieke set onderwerpen. De toepassing gebruikt de [Sentiment140](http://help.sentiment140.com/) open-source-programma, waarmee de volgende gevoelswaarde aan elke tweet toegewezen:

* 0 = negatief zijn
* 2 = neutral
* 4 = positief

Nadat de tweet gebeurtenissen gevoelswaarde zijn toegewezen, worden ze doorgegeven naar de event hub die u eerder hebt gemaakt.

Voordat de toepassing wordt uitgevoerd, zijn bepaalde gegevens van u, zoals de Twitter-sleutels en de event hub-verbindingsreeks vereist. U kunt opgeven dat de configuratie-informatie op de volgende manieren:

* De toepassing wordt uitgevoerd, en gebruik vervolgens de gebruikersinterface van de toepassing in te voeren van de sleutels, geheimen en verbindingsreeks. Als u dit doet, wordt de configuratie-informatie wordt gebruikt voor de huidige sessie, maar het is niet opgeslagen.
* Bewerken van de toepassing .config-bestand en stel de waarden bevat. Deze benadering van de configuratie-informatie blijft bestaan, maar dit betekent ook dat deze potentieel gevoelige gegevens worden opgeslagen als tekst zonder opmaak op uw computer.

De volgende procedure worden beide methoden. 

1. Zorg ervoor dat u hebt gedownload en uitgepakt het [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) toepassing, zoals vermeld in de vereisten.

2. Om in te stellen van de uitvoering van de waarden tijdens de uitvoering (en alleen voor de huidige sessie), de `TwitterWPFClient.exe` toepassing. Wanneer u de toepassing wordt gevraagd, voert u de volgende waarden:

    * De consument Twitter sleutel (API-sleutel).
    * De Twitter Consumer Secret (API-geheim).
    * Het Twitter-toegangstoken.
    * De Twitter Access Token Secret.
    * De verbindingsreeksinformatie die u eerder hebt opgeslagen. Zorg ervoor dat u de verbindingsreeks die u hebt verwijderd de `EntityPath` sleutel / waarde-paar uit.
    * De Twitter-trefwoorden die u wilt bepalen sentiment.

   ![TwitterWpfClient toepassing actief is, weergegeven met verborgen instellingen](./media/stream-analytics-twitter-sentiment-analysis-trends/wpfclientlines.png)

3. Om de waarden permanent, gebruik een teksteditor het bestand TwitterWpfClient.exe.config te openen. Klik in de `<appSettings>` -element hiervoor:

    * Stel `oauth_consumer_key` naar de Twitter-consument-sleutel (API-sleutel). 
    * Stel `oauth_consumer_secret` naar de Twitter Consumer Secret (API-geheim).
    * Stel `oauth_token` naar het Twitter-toegangstoken.
    * Stel `oauth_token_secret` naar Twitter Access Token Secret.

    Verderop in de `<appSettings>` -element, deze wijzigingen aanbrengen:

    * Stel `EventHubName` op de naam van de event hub (dat wil zeggen, met de waarde van het pad van de entiteit).
    * Stel `EventHubNameConnectionString` op de verbindingstekenreeks. Zorg ervoor dat u de verbindingsreeks die u hebt verwijderd de `EntityPath` sleutel / waarde-paar uit.

    De `<appSettings>` sectie ziet eruit als in het volgende voorbeeld. (Voor meer duidelijkheid en beveiliging, we verpakt sommige regels en bepaalde tekens verwijderd.)

    ![Configuratiebestand van de TwitterWpfClient-toepassing in een teksteditor, met de Twitter-sleutels en geheimen en de verbindingsinformatie van de event hub](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-tiwtter-app-config.png)
 
4. Als u de toepassing al is niet gestart, nu TwitterWpfClient.exe uitvoeren. 

5. Klik op de groene knop voor het verzamelen van sociale sentiment. Ziet u gebeurtenissen met de Tweet de **CreatedAt**, **onderwerp**, en **SentimentScore** waarden worden verzonden naar uw event hub.

    ![TwitterWpfClient toepassing die wordt uitgevoerd, met een overzicht van tweets](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-app-listing.png)

    >[!NOTE]
    >Als u fouten ziet en u een stream van tweets weergegeven in het onderste gedeelte van het venster niet ziet, Controleer de sleutels en geheimen. Controleer ook of de verbindingsreeks (Zorg ervoor dat deze bevat geen de `EntityPath` sleutel en waarde.)


## <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken

Nu dat tweet gebeurtenissen streaming in realtime van Twitter, kunt u een Stream Analytics-taak instellen voor het analyseren van deze gebeurtenissen in realtime.

1. Klik in de Azure-portal op **een resource maken** > **Internet of Things** > **Stream Analytics-taak**.

2. Naam van de taak `socialtwitter-sa-job` en geeft u een abonnement, resourcegroep en locatie.

    Is het een goed idee om de taak en de event hub in dezelfde regio voor de beste prestaties en zodat u niet betaalt voor het overbrengen van gegevens tussen regio's.

    ![Het maken van een nieuwe Stream Analytics-taak](./media/stream-analytics-twitter-sentiment-analysis-trends/newjob.png)

3. Klik op **Create**.

    De taak is gemaakt en de portal geeft taakdetails weer.


## <a name="specify-the-job-input"></a>Geef de Taakinvoer

1. In de Stream Analytics-taak, onder **Taaktopologie** in het midden van de taakblade klikt u op **invoer**. 

2. In de **invoer** blade, klikt u op  **+ &nbsp;toevoegen** en vul de blade met deze waarden:

    * **Invoeralias**: Gebruik de naam `TwitterStream`. Als u een andere naam gebruikt, moet u een notitie van deze omdat u deze later nodig.
    * **Gegevensbrontype**: Selecteer **gegevensstroom**.
    * **Bron**: Selecteer **Event hub**.
    * **Importoptie**: Selecteer **gebruik event hub van het huidige abonnement**. 
    * **Service bus-naamruimte**: Selecteer de event hub-naamruimte die u eerder hebt gemaakt (`<yourname>-socialtwitter-eh-ns`).
    * **Event hub**: Selecteer de event hub die u eerder hebt gemaakt (`socialtwitter-eh`).
    * **De naam van een Event hub-beleid**: Selecteer het beleid dat u eerder hebt gemaakt (`socialtwitter-access`).

    ![Nieuwe invoer voor Streaming Analytics-taak maken](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-new-input.png)

3. Klik op **Create**.


## <a name="specify-the-job-query"></a>Geef de taakquery op

Stream Analytics ondersteunt een eenvoudig, declaratief querymodel waarin wordt beschreven transformaties. Zie voor meer informatie over de taal, de [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Deze zelfstudie helpt u bij het ontwerpen en testen van verschillende query's via Twitter-gegevens.

Als u wilt vergelijken van het aantal vermeldingen tussen verschillende onderwerpen, kunt u een [tumblingvenster](https://msdn.microsoft.com/library/azure/dn835055.aspx) om op te halen om de vijf seconden het aantal vermeldingen per onderwerp.

1. Sluit de **invoer** blade als u dat nog niet gedaan hebt.

2. In de **overzicht** blade, klikt u op **Query bewerken** in de buurt van de bovenkant van de Query in. Azure geeft een lijst van de invoer en uitvoer die zijn geconfigureerd voor de taak en kunt u een query waarmee u de invoerstroom transformeren zoals deze wordt verzonden naar de uitvoer kunt maken.

3. Zorg ervoor dat de toepassing TwitterWpfClient wordt uitgevoerd. 

3. In de **Query** blade, klik op de puntjes naast de `TwitterStream` invoer en selecteer vervolgens **voorbeeldgegevens van uitvoer**.

    ![Menuopties voor het gebruik van voorbeeldgegevens voor de vermelding van de stream Analytics-taak met 'voorbeeld van gegevens uit de invoer ' geselecteerd](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-sample-data-from-input.png)

    Hiermee opent u een blade waarmee u opgeven hoeveel voorbeeldgegevens ophalen kunt, gedefinieerd in termen van hoe lang de invoerstroom lezen.

4. Stel **minuten** op 3 en klik vervolgens op **OK**. 
    
    ![Opties voor de steekproef van de invoerstroom met "3 minuten" geselecteerd.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-input-create-sample-data.png)

    Azure-voorbeelden voor drie minuten aan gegevens uit de invoerstroom en ontvangt u een melding wanneer de voorbeeldgegevens gereed is. (Dit duurt enkele ogenblikken). 

    De voorbeeldgegevens worden tijdelijk opgeslagen en zijn beschikbaar zolang u het queryvenster geopend houdt. Als u het queryvenster sluit, wordt de voorbeeldgegevens verwijderd en u moet een nieuwe set voorbeeldgegevens maken. 

5. Wijzig de query in de code-editor in het volgende:

    ```
    SELECT System.Timestamp as Time, Topic, COUNT(*)
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY TUMBLINGWINDOW(s, 5), Topic
    ```

    Als niet `TwitterStream` vervangen door uw alias voor als de alias voor de invoer `TwitterStream` in de query.  

    Deze query gebruikt de **TIMESTAMP BY** trefwoord om op te geven van een tijdstempelveld in de nettolading in de tijdelijke berekening moet worden gebruikt. Als dit veld niet wordt opgegeven, wordt de bewerking windowing uitgevoerd met behulp van de tijd die elke gebeurtenis wordt ontvangen op de event hub. Meer informatie in de sectie 'Aankomsttijd vs toepassing de tijd' van [Stream Analytics Query verwijzing](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Deze query ook een tijdstempel voor het einde van elke venster wordt geopend met behulp van de **System.Timestamp** eigenschap.

5. Klik op **Test**. De query wordt uitgevoerd op de gegevens die u voorbeelden worden gemaakt.
    
6. Klik op **Opslaan**. Hiermee wordt de query opgeslagen als onderdeel van de stream Analytics-taak. (Deze kunt de voorbeeldgegevens niet opslaan.)


## <a name="experiment-using-different-fields-from-the-stream"></a>Experimenteer met verschillende velden van de stroom 

De volgende tabel bevat de velden die deel uitmaken van de JSON streamen van gegevens. U kunt een experiment uit in de query-editor.

|JSON-eigenschap | Definitie|
|--- | ---|
|CreatedAt | Het tijdstip waarop de tweet is gemaakt|
|Onderwerp | Het onderwerp dat overeenkomt met het opgegeven trefwoord|
|SentimentScore | De gevoelsscore van Sentiment140|
|Auteur | De Twitter-handle die de tweet verzonden|
|Tekst | De volledige hoofdtekst van de tweet|


## <a name="create-an-output-sink"></a>Maken van een uitvoer-sink

U hebt nu een gebeurtenisstroom, een event hub invoeren voor het opnemen van gebeurtenissen en een query voor het uitvoeren van een transformatie via de stroom gedefinieerd. De laatste stap is het definiëren van een uitvoerlocatie voor de taak.  

In deze zelfstudie hebt schrijven u de samengevoegde tweet-gebeurtenissen van de taakquery naar Azure Blob storage.  U kunt ook uw resultaten worden gepusht naar Azure SQL Database, Azure Table storage, Event Hubs, of Power BI, afhankelijk van uw toepassing nodig heeft.

## <a name="specify-the-job-output"></a>Geef de taakuitvoer

1. In de **Taaktopologie** sectie, klikt u op de **uitvoer** vak. 

2. In de **uitvoer** blade, klikt u op  **+ &nbsp;toevoegen** en vul de blade met deze waarden:

    * **Uitvoeralias**: Gebruik de naam `TwitterStream-Output`. 
    * **Sink**: Selecteer **Blob-opslag**.
    * **Opties voor het importeren**: Selecteer **gebruik blob-opslag van het huidige abonnement**.
    * **Storage-account**. Selecteer **een nieuw opslagaccount maken.**
    * **Storage-account** (tweede vak). Voer `YOURNAMEsa`, waarbij `YOURNAME` is uw naam of een andere unieke tekenreeks. De naam kan alleen kleine letters en cijfers gebruiken en deze moet uniek zijn binnen Azure. 
    * **Container**. Voer `socialtwitter` in.
    De naam van het opslagaccount en de containernaam worden samen gebruikt voor een URI voor de blob-opslag als volgt: 

    `http://YOURNAMEsa.blob.core.windows.net/socialtwitter/...`
    
    ![Blade 'Nieuwe uitvoer' voor Stream Analytics-taak](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-output-blob-storage.png)
    
4. Klik op **Create**. 

    Azure storage-account maakt en automatisch een sleutel is gegenereerd. 

5. Sluit de **uitvoer** blade. 


## <a name="start-the-job"></a>Taak starten

Een Taakinvoer-, query- en uitvoer worden opgegeven. U bent klaar om de Stream Analytics-taak te starten.

1. Zorg ervoor dat de toepassing TwitterWpfClient wordt uitgevoerd. 

2. Klik op de taakblade **Start**.

    ![De Stream Analytics-taak starten](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-output.png)

3. In de **starttaak** blade voor **starttijd voor taakuitvoer**, selecteer **nu** en klik vervolgens op **Start**. 

    ![Blade 'Start de taak' voor de Stream Analytics-taak](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-job-blade.png)

    Azure wordt u gewaarschuwd wanneer de taak is gestart en op de taakblade de status wordt weergegeven als **met**.

    ![Taak die wordt uitgevoerd](./media/stream-analytics-twitter-sentiment-analysis-trends/jobrunning.png)

## <a name="view-output-for-sentiment-analysis"></a>Weergave uitvoer voor sentimentanalyse

Nadat de taak is gestart en de realtime Twitter-stroom wordt verwerkt, kunt u de uitvoer voor sentimentanalyse weergeven.

U kunt een hulpprogramma zoals [Azure Storage Explorer](https://storageexplorer.com/) of [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) om weer te geven van de taakuitvoer in realtime. Hier kunt u [Power BI](https://powerbi.com/) om uit te breiden van uw toepassing om op te nemen van een aangepast dashboard zoals weergegeven in de volgende schermafbeelding:

![Power BI](./media/stream-analytics-twitter-sentiment-analysis-trends/power-bi.png)


## <a name="create-another-query-to-identify-trending-topics"></a>Maken van een andere query voor het identificeren van actuele onderwerpen op

Een andere query die u gebruiken kunt om te begrijpen van Twitter-gevoel is gebaseerd op een [Verschuivend venster](https://msdn.microsoft.com/library/azure/dn835051.aspx). Voor het identificeren van actuele onderwerpen zoeken u naar onderwerpen die de drempelwaarde voor vermeldingen in een opgegeven tijdsduur.

Voor de toepassing van deze zelfstudie wordt zoeken u naar onderwerpen die meer dan 20 keer worden vermeld in de afgelopen 5 seconden.

1. Klik op de taakblade **stoppen** om de taak te stoppen. 

2. In de **Taaktopologie** sectie, klikt u op de **Query** vak. 

3. Wijzig de query in het volgende:

    ```    
    SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY SLIDINGWINDOW(s, 5), topic
    HAVING COUNT(*) > 20
    ```

4. Klik op **Opslaan**.

5. Zorg ervoor dat de toepassing TwitterWpfClient wordt uitgevoerd. 

6. Klik op **Start** opnieuw opstarten van de taak met behulp van de nieuwe query.


## <a name="get-support"></a>Ondersteuning krijgen
Voor verdere ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
