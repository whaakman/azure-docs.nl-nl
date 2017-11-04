---
title: Real-time Twitter-gevoel-analyse met Azure Stream Analytics | Microsoft Docs
description: Informatie over het gebruik van Stream Analytics voor analyse van realtime Twitter-gevoel. Stapsgewijze richtlijnen van genereren van gebeurtenis gegevens op een live-dashboard.
keywords: het trendanalyse realtime twitter, gevoel analyse, analyse van sociale media, trend analysis-voorbeeld
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 42068691-074b-4c3b-a527-acafa484fda2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: samacha
ms.openlocfilehash: 98230a8b61d1776a9ab23fd416af306efc700959
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Real-time Twitter-gevoel analyse in Azure Stream Analytics

Informatie over het bouwen van een oplossing gevoel analyse voor sociale media analytics door realtime Twitter-gebeurtenissen te brengen in Azure Event Hubs. U kunt vervolgens schrijven een Azure Stream Analytics-query voor het analyseren van de gegevens vallen of opslaan van de resultaten voor later gebruik of gebruik een dashboard en [Power BI](https://powerbi.com/) om inzichten in realtime.

Sociale media analytics-hulpprogramma's waarmee organisaties kunnen begrijpen trends onderwerpen. Trends onderwerpen zijn onderwerpen en houding die een groot aantal berichten in sociale media hebt. Gevoel analyse, ook wel genoemd *advies analysemodel*, sociale media analytics-hulpprogramma's gebruikt om te bepalen van houding ten opzichte van een product, idee, enzovoort. 

Realtime Twitter trendanalyse is een goed voorbeeld van een hulpprogramma voor analyse, omdat het abonnement hashtag model u in staat kunt te luisteren naar specifieke trefwoorden (hashtags) en het gevoel over analyse van de feed ontwikkelen.

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a>Scenario: Sociale media gevoel analyse in realtime

Een bedrijf met een website nieuws media is geïnteresseerd in een voordeel ten opzichte van de concurrenten krijgen door de inhoud van de site die onmiddellijk relevant is voor de lezers. Het bedrijf gebruikt analyse van sociale media op onderwerpen die relevant voor lezers zijn als volgt realtime gevoel analyse van gegevens van Twitter.

Om te identificeren trends onderwerpen in realtime op Twitter, moet het bedrijf realtime analyses over de tweet volume en het gevoel voor belangrijke onderwerpen. Met andere woorden, is de noodzaak een gevoel analysis analytics-engine die gebaseerd op deze sociale media feed.

## <a name="prerequisites"></a>Vereisten
In deze zelfstudie gebruikt u een clienttoepassing die verbinding maakt met Twitter en zoekt tweets waarvoor bepaalde hashtags (die u kunt instellen). Als u wilt uitvoeren van de toepassing en analyseren van de tweets met Azure Streaming Analytics, hebt u het volgende:

* Een Azure-abonnement
* Een Twitter-account 
* Een Twitter-toepassing en de [OAuth-toegangstoken](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) voor die toepassing. We bieden op hoog niveau instructies voor het maken van een toepassing Twitter later.
* De toepassing TwitterWPFClient, wat de Twitter-feed leest. Als u deze toepassing, downloaden de [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) bestand van GitHub en pak vervolgens het pakket naar een map op uw computer. Als u wilt de bron code en de toepassing uitvoert in een foutopsporingsprogramma wilt zien, kunt u de broncode van de downloaden [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator). 

## <a name="create-an-event-hub-for-streaming-analytics-input"></a>Maken van een event hub voor Streaming Analytics invoer

De voorbeeldtoepassing gebeurtenissen gegenereerd en stuurt deze naar een Azure event hub. Azure event hubs zijn de voorkeursmethode voor opname van de gebeurtenis voor Stream Analytics. Zie voor meer informatie de [documentatie van Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).


### <a name="create-an-event-hub-namespace-and-event-hub"></a>Een event hub-naamruimte en event hub maken
In deze procedure maakt u eerst een event hub-naamruimte maken en vervolgens voegt u toe een event hub naar die naamruimte. Event hub-naamruimten worden gebruikt voor het gerelateerde gebeurtenis bus exemplaren logisch groeperen. 

1. Aanmelden bij de Azure-portal en klikt u op **nieuw** > **Internet der dingen** > **Event Hub**. 

2. In de **naamruimte maken** blade, voer de naam van een naamruimte zoals `<yourname>-socialtwitter-eh-ns`. U kunt een naam voor de naamruimte, maar de naam moet geldig zijn voor een URL en deze uniek zijn binnen Azure. 
    
3. Selecteer een abonnement maken of een resourcegroep kiezen, en klikt u op **maken**. 

    ![Een event hub-naamruimte maken](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-namespace.png)
 
4. Wanneer de naamruimte is voltooid implementeren, moet u de event hub-naamruimte vinden in uw lijst met Azure-resources. 

5. Klik op de nieuwe naamruimte en klik op de blade naamruimte  **+ &nbsp;Event Hub**. 

    ![De knop toevoegen Event Hub voor het maken van nieuwe event hub ](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-button.png)    
 
6. Naam van de nieuwe event hub `socialtwitter-eh`. U kunt een andere naam gebruiken. Als u dit doet, noteer, omdat u de naam van de later nodig. U hoeft niet in te stellen andere opties voor de event hub.

    ![Blade voor het maken van nieuwe event hub](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub.png)
 
7. Klik op **Create**.


### <a name="grant-access-to-the-event-hub"></a>Toegang verlenen tot de event hub

Voordat een proces gegevens naar een event hub verzenden kan, moet een beleid waarmee u de juiste toegang hebben in de event hub. Het toegangsbeleid produceert een verbindingsreeks die autorisatie-informatie bevat.

1.  Klik op de blade gebeurtenis naamruimte **Event Hubs** en klik vervolgens op de naam van uw nieuwe event hub.

2.  Klik in de hubblade gebeurtenis **gedeeld toegangsbeleid** en klik vervolgens op  **+ &nbsp;toevoegen**.

    >[!NOTE]
    >Zorg ervoor dat u werkt met de event hub, niet de event hub-naamruimte.

3.  Een beleid met de naam toevoegen `socialtwitter-access` en voor **Claim**, selecteer **beheren**.

    ![Blade voor een nieuwe event hub-beleid maken](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-shared-access-policy-manage.png)
 
4.  Klik op **Create**.

5.  Nadat het beleid is geïmplementeerd, klikt u op het in de lijst met beleidsregels voor gedeelde toegang.

6.  In het vak **CONNECTION STRING-primaire sleutel** en klik op de knop kopiëren naast de verbindingsreeks. 
    
    ![Kopiëren van de primaire verbinding string-sleutel van het toegangsbeleid](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-shared-access-policy-copy-connection-string.png)
 
7.  De verbindingsreeks in een teksteditor plakken. U moet deze verbindingsreeks voor de volgende sectie, nadat u een aantal kleine wijzigingen aangebracht.

    De verbindingsreeks ziet er als volgt:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=;EntityPath=socialtwitter-eh

    U ziet dat de verbindingsreeks meerdere sleutel / waarde-paren bevat, gescheiden door puntkomma's: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, en `EntityPath`.  

    > [!NOTE]
    > Voor beveiliging, zijn de onderdelen van de verbindingsreeks in het voorbeeld verwijderd.

8.  Verwijder in de teksteditor de `EntityPath` paar uit de verbindingsreeks (Vergeet niet om te verwijderen van de voorafgaande puntkomma). Wanneer u bent klaar, ziet de verbindingsreeks er als volgt uit:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=


## <a name="configure-and-start-the-twitter-client-application"></a>Configureren en starten van de clienttoepassing Twitter
De clienttoepassing opgehaald tweet gebeurtenissen rechtstreeks vanuit Twitter. Om te kunnen doen, moet deze machtiging aan te roepen de Twitter Streaming-API. Als u wilt configureren die machtiging, maakt u een toepassing in Twitter, dat wordt gegenereerd unieke referenties (zoals een OAuth-token). Vervolgens kunt u de clienttoepassing gebruikt deze referenties wanneer het API-aanroepen maakt. 

### <a name="create-a-twitter-application"></a>Een Twitter-toepassing maken
Als u nog geen een Twitter-toepassing die u voor deze zelfstudie gebruiken kunt, kunt u een maken. U moet al een Twitter-account hebben.

> [!NOTE]
> De exacte stappen in Twitter voor het maken van een toepassing en de sleutels, geheimen en -token ophalen kan worden gewijzigd. Als deze instructies niet overeenkomen met wat u ziet op de site Twitter, raadpleegt u de Twitter-documentatie voor ontwikkelaars.

1. Ga naar de [Twitter application management-pagina](https://apps.twitter.com/). 

2. Maak een nieuwe toepassing. 

    * Geef een geldige URL voor de website-URL. Dit hoeft niet te worden van een actieve site. (U kunt niet alleen opgeven `localhost`.)
    * De callback-veld leeg laten. De clienttoepassing die u voor deze zelfstudie gebruiken vereist geen retouraanroepen.

    ![Maken van een toepassing in Twitter](./media/stream-analytics-twitter-sentiment-analysis-trends/create-twitter-application.png)

3. Wijzig eventueel de machtigingen van de toepassing op alleen-lezen.

4. Wanneer de toepassing wordt gemaakt, gaat u naar de **sleutels en toegangstokens** pagina.

5. Klik op de knop voor het genereren van een access-token en toegang token geheim.

Houd deze informatie bij de hand hebt, hebt u deze in de volgende procedure.

>[!NOTE]
>De sleutels en geheimen voor de Twitter-toepassing bieden toegang tot uw Twitter-account. Deze informatie behandelen als gevoelig hetzelfde als u uw wachtwoord Twitter doen. Bijvoorbeeld, niet in een toepassing die u aan anderen toewijst deze informatie te sluiten. 


### <a name="configure-the-client-application"></a>De clienttoepassing configureren
Er is een clienttoepassing die verbinding met Twitter-gegevens met maakt gemaakt [Twitter van Streaming-API's](https://dev.twitter.com/streaming/overview) voor het verzamelen van gebeurtenissen tweet over een specifieke verzameling van onderwerpen. De toepassing gebruikt de [Sentiment140](http://help.sentiment140.com/) open-source hulpprogramma, waarmee de waarde van de volgende gevoel wordt toegewezen aan elke tweet:

* 0 = negatief
* 2 = neutral
* 4 = positief

Nadat de gebeurtenissen tweet een waarde van het gevoel over zijn toegewezen, zijn ze geduwd naar de event hub die u eerder hebt gemaakt.

Voordat de toepassing wordt uitgevoerd, is er bepaalde gegevens van u, zoals de Twitter-sleutels en de verbindingsreeks van de event hub. U kunt opgeven dat de configuratie-informatie op de volgende manieren:

* Voer de toepassing en vervolgens met de gebruikersinterface van de toepassing de sleutels, geheimen en verbindingsreeks invoeren. Als u dit doet, wordt de configuratie-informatie wordt gebruikt voor de huidige sessie, maar het is niet opgeslagen.
* De toepassing .config-bestand bewerken en stel de waarden bevat. Deze benadering de configuratie-informatie blijft bestaan, maar het betekent ook dat deze potentieel gevoelige informatie wordt opgeslagen als tekst zonder opmaak op uw computer.

De volgende procedure worden beide benaderingen. 

1. Zorg ervoor dat u hebt gedownload en uitgepakt de [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) toepassing, zoals vermeld in de vereisten.

2. Instellen van de waarden tijdens runtime (en alleen voor de huidige sessie), voer de `TwitterWPFClient.exe` toepassing. Wanneer u de toepassing wordt gevraagd, typt u de volgende waarden:

    * De consument Twitter sleutel (API-sleutel).
    * De consument Twitter geheim (geheim API).
    * Het toegangstoken Twitter.
    * Het Twitter Access Token geheim.
    * De verbindingsinformatie die u eerder hebt opgeslagen. Zorg ervoor dat u de verbindingsreeks die u hebt verwijderd met de `EntityPath` sleutel-waardepaar uit.
    * De Twitter-trefwoorden die u wilt bepalen gevoel voor.

   ![TwitterWpfClient toepassing uitgevoerd, worden weergegeven met verborgen instellingen](./media/stream-analytics-twitter-sentiment-analysis-trends/wpfclientlines.png)

3. Gebruik de waarden permanent stelt een teksteditor TwitterWpfClient.exe.config bestand te openen. Klik in de `<appSettings>` element dit doen:

    * Stel `oauth_consumer_key` naar de Twitter-Consumer-sleutel (API-sleutel). 
    * Stel `oauth_consumer_secret` naar het Twitter consumentgeheim (geheim API).
    * Stel `oauth_token` aan het toegangstoken Twitter.
    * Stel `oauth_token_secret` aan het Token geheim voor Twitter-toegang.

    Verderop in de `<appSettings>` element, deze wijzigingen aanbrengen:

    * Stel `EventHubName` aan de naam van de event hub (dat wil zeggen, met de waarde van het pad van de entiteit).
    * Stel `EventHubNameConnectionString` de verbindingsreeks. Zorg ervoor dat u de verbindingsreeks die u hebt verwijderd met de `EntityPath` sleutel-waardepaar uit.

    De `<appSettings>` sectie lijkt op het volgende voorbeeld. (Voor duidelijkheid en beveiliging we sommige regels ingepakt en bepaalde tekens verwijderd.)

    ![TwitterWpfClient toepassingsconfiguratiebestand in een teksteditor weergegeven met de Twitter-sleutels en geheimen en de verbindingsinformatie van de event hub](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-tiwtter-app-config.png)
 
4. Als u de toepassing al niet hebt gestart, nu TwitterWpfClient.exe uitvoeren. 

5. Klik op de groene knop voor het verzamelen van sociale gevoel. U ziet Tweet gebeurtenissen met de **CreatedAt**, **onderwerp**, en **SentimentScore** waarden die worden verzonden naar uw event hub.

    ![TwitterWpfClient-toepassing wordt uitgevoerd, met een overzicht van tweets](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-app-listing.png)

    >[!NOTE]
    >Als u fouten ziet en u een stream met tweets weergegeven in het onderste gedeelte van het venster niet ziet, Controleer de sleutels en geheimen. Controleer ook de verbindingsreeks (Zorg ervoor dat beschikt niet over de `EntityPath` sleutel en waarde.)


## <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken

Nu dat tweet gebeurtenissen streaming in realtime van Twitter, kunt u een Stream Analytics-taak instellen voor het analyseren van deze gebeurtenissen in realtime.

1. Klik in de Azure-portal op **nieuw** > **Internet der dingen** > **Stream Analytics-taak**.

2. Naam van de taak `socialtwitter-sa-job` en een abonnement, resourcegroep en locatie opgeven.

    Het is een goed idee om de taak en de event hub plaatsen in dezelfde regio voor de beste prestaties en betaalt u niet aan het overbrengen van gegevens tussen regio's.

    ![Er wordt een nieuwe Stream Analytics-taak](./media/stream-analytics-twitter-sentiment-analysis-trends/newjob.png)

3. Klik op **Create**.

    De taak is gemaakt en de portal geeft taakdetails weer.


## <a name="specify-the-job-input"></a>De invoer van de taak opgeven

1. In de Stream Analytics-taak onder **taak topologie** in het midden van de taakblade klikt u op **invoer**. 

2. In de **invoer** blade, klikt u op  **+ &nbsp;toevoegen** en vult u de blade met deze waarden:

    * **Invoeralias**: Gebruik de naam `TwitterStream`. Als u een andere naam gebruikt, noteer het omdat u deze later nodig.
    * **Gegevensbrontype**: Selecteer **gegevensstroom**.
    * **Bron**: Selecteer **Event hub**.
    * **Optie importeren**: Selecteer **gebruik event hub uit het huidige abonnement**. 
    * **Service bus-naamruimte**: Selecteer de event hub-naamruimte die u eerder hebt gemaakt (`<yourname>-socialtwitter-eh-ns`).
    * **Event hub**: Selecteer de event hub die u eerder hebt gemaakt (`socialtwitter-eh`).
    * **Naam Event hub beleid**: Selecteer het beleid dat u eerder hebt gemaakt (`socialtwitter-access`).

    ![Nieuwe invoer voor Streaming Analytics-taak maken](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-new-input.png)

3. Klik op **Create**.


## <a name="specify-the-job-query"></a>De taak query opgeven

Stream Analytics ondersteunt een eenvoudig, declaratief querymodel die transformaties beschrijft. Zie voor meer informatie over de taal, de [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Deze zelfstudie helpt u bij het ontwerpen en testen van meerdere query's via Twitter-gegevens.

Als u wilt vergelijken van het aantal vermeldingen tussen onderwerpen, kunt u een [tumblingvenster](https://msdn.microsoft.com/library/azure/dn835055.aspx) ophalen van het aantal vermeldingen onderwerp elke vijf seconden.

1. Sluit de **invoer** blade als u dat nog niet gedaan hebt.

2. Klik in de taakblade op de **Query** vak. Azure geeft een lijst van de invoer en uitvoer die zijn geconfigureerd voor de taak en kunt u een query maken die u de invoerstroom Transformeren kunt zoals deze wordt verzonden naar de uitvoer.

3. Zorg ervoor dat de toepassing TwitterWpfClient wordt uitgevoerd. 

3. In de **Query** blade, klik op de puntjes naast de `TwitterStream` invoer- en selecteer vervolgens **voorbeeldgegevens uit invoer**.

    ![Menuopties met voorbeeldgegevens voor de vermelding van de Streaming Analytics-taak met 'Voorbeeldgegevens uit invoer' geselecteerd](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-sample-data-from-input.png)

    Hiermee opent u een blade die u opgeven hoeveel voorbeeldgegevens ophalen kunt, gedefinieerd in termen van hoe lang de invoerstroom lezen.

4. Stel **minuten** 3 en klik vervolgens op **OK**. 
    
    ![Opties voor de steekproef van de invoerstroom met '3 minuten' geselecteerd.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-input-create-sample-data.png)

    Azure 3 minuten aan gegevens uit de invoerstroom voorbeelden en waarschuwt u als de voorbeeldgegevens gereed is. (Dit duurt even.) 

    De voorbeeldgegevens worden tijdelijk opgeslagen en is beschikbaar wanneer u hebt de queryvenster geopend. Als u het query-venster sluit, de voorbeeldgegevens worden genegeerd en u moet een nieuwe set met voorbeeldgegevens. 

5. De query in de code-editor wijzigen in het volgende:

    ```
    SELECT System.Timestamp as Time, Topic, COUNT(*)
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY TUMBLINGWINDOW(s, 5), Topic
    ```

    Als niet `TwitterStream` vervangen door uw alias voor als de alias voor de invoer `TwitterStream` in de query.  

    Deze query gebruikt de **TIMESTAMP BY** trefwoord een tijdstempelveld opgeven in de nettolading in de tijdelijke berekening moet worden gebruikt. Als dit veld niet wordt opgegeven, wordt de windowing bewerking uitgevoerd met behulp van de tijd dat elke gebeurtenis in de event hub is aangekomen. Meer informatie in de sectie 'Aankomsttijd tegenover tijd Application' van [Stream Analytics Query verwijzing](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Deze query ook een tijdstempel voor het einde van elke venster wordt geopend met behulp van de **System.Timestamp** eigenschap.

5. Klik op **Test**. De query wordt uitgevoerd op basis van de gegevens die u door actieve.
    
6. Klik op **Opslaan**. Dit bespaart de query als onderdeel van de Streaming Analytics-taak. (Deze niet de voorbeeldgegevens opslaan.)


## <a name="experiment-using-different-fields-from-the-stream"></a>Experimenteren met verschillende velden uit de stroom 

De volgende tabel staan de velden die deel uitmaken van de JSON gegevensstromen. U kunt experimenteren in de query-editor.

|JSON-eigenschap | Definitie|
|--- | ---|
|CreatedAt | Het tijdstip waarop de tweet is gemaakt|
|Onderwerp | Het onderwerp dat overeenkomt met het opgegeven trefwoord|
|SentimentScore | De score gevoel van Sentiment140|
|Auteur | De Twitter-ingang die de tweet wordt verzonden|
|Tekst | De volledige hoofdtekst van de tweet|


## <a name="create-an-output-sink"></a>Uitvoerlocatie maken

U hebt nu een stroom gebeurtenissen, een event hub als invoer gebruikt voor het opnemen van gebeurtenissen en een query uit te voeren van een transformatie via de stroom gedefinieerd. De laatste stap is om te definiëren uitvoerlocatie voor de taak.  

In deze zelfstudie maakt schrijven u de geaggregeerde tweet gebeurtenissen van de taak query naar Azure Blob-opslag.  U kunt ook uw resultaten push naar Azure SQL Database, Azure Table storage, Event Hubs, of, afhankelijk van uw toepassing moet Power BI.

## <a name="specify-the-job-output"></a>De taakuitvoer van de opgeven

1. In de **taak topologie** sectie, klikt u op de **uitvoer** vak. 

2. In de **uitvoer** blade, klikt u op  **+ &nbsp;toevoegen** en vult u de blade met deze waarden:

    * **Uitvoeraliassen**: Gebruik de naam `TwitterStream-Output`. 
    * **Sink**: Selecteer **Blob storage**.
    * **Opties voor het importeren**: Selecteer **gebruik blob storage uit het huidige abonnement**.
    * **Storage-account**. Selecteer **een nieuw opslagaccount maken.**
    * **Storage-account** (tweede vak). Voer `YOURNAMEsa`, waarbij `YOURNAME` uw naam of een andere unieke tekenreeks is. De naam kan alleen kleine letters en cijfers gebruiken en deze uniek zijn binnen Azure. 
    * **Container**. Voer `socialtwitter`.
    De naam van het opslagaccount en de containernaam worden samen gebruikt voor een URI voor de blobopslag, als volgt: 

    `http://YOURNAMEsa.blob.core.windows.net/socialtwitter/...`
    
    !['Nieuw uitvoer' blade voor Stream Analytics-taak](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-output-blob-storage.png)
    
4. Klik op **Create**. 

    Azure storage-account maakt en een sleutel automatisch wordt gegenereerd. 

5. Sluit de **uitvoer** blade. 


## <a name="start-the-job"></a>Start de taak

Een taak invoer-, query- en uitvoer zijn opgegeven. U bent klaar om te beginnen de Stream Analytics-taak.

1. Zorg ervoor dat de toepassing TwitterWpfClient wordt uitgevoerd. 

2. Klik op de taakblade **Start**.

    ![De Stream Analytics-taak starten](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-output.png)

3. In de **starttaak** blade voor **taakuitvoer begintijd**, selecteer **nu** en klik vervolgens op **Start**. 

    ![Blade 'Start de taak' voor de Stream Analytics-taak](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-job-blade.png)

    Azure waarschuwt u als de taak is gestart en op de taakblade de status wordt weergegeven als **met**.

    ![Actieve taak](./media/stream-analytics-twitter-sentiment-analysis-trends/jobrunning.png)

## <a name="view-output-for-sentiment-analysis"></a>Uitvoer weergeven voor gevoel analyse

Nadat uw taak eenmaal is gestart en de real-time Twitter-stroom wordt verwerkt, kunt u de uitvoer voor analyse gevoel weergeven.

U kunt een hulpprogramma zoals [Azure Opslagverkenner](https://http://storageexplorer.com/) of [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) uw taakuitvoer weergeven in realtime. Hier kunt u [Power BI](https://powerbi.com/) uitbreiden van uw toepassing om op te nemen van een aangepaste dashboard zoals weergegeven in de volgende schermafbeelding:

![Power BI](./media/stream-analytics-twitter-sentiment-analysis-trends/power-bi.png)


## <a name="create-another-query-to-identify-trending-topics"></a>Een andere query maken om te identificeren trends onderwerpen

Een andere query die u gebruiken kunt om te begrijpen Twitter-gevoel is gebaseerd op een [Verschuivend venster](https://msdn.microsoft.com/library/azure/dn835051.aspx). Als trends onderwerpen, kijkt u naar de onderwerpen die via een drempelwaarde voor vermeldingen in een opgegeven tijdsduur.

Voor de toepassing van deze zelfstudie wordt zoeken u naar onderwerpen die meer dan 20 keer worden vermeld in de laatste 5 seconden.

1. Klik op de taakblade **stoppen** stoppen van de taak. 

2. In de **taak topologie** sectie, klikt u op de **Query** vak. 

3. De query wijzigen in het volgende:

    ```    
    SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY SLIDINGWINDOW(s, 5), topic
    HAVING COUNT(*) > 20
    ```

4. Klik op **Opslaan**.

5. Zorg ervoor dat de toepassing TwitterWpfClient wordt uitgevoerd. 

6. Klik op **Start** opnieuw opstarten van de taak met de nieuwe query.


## <a name="get-support"></a>Ondersteuning krijgen
Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
