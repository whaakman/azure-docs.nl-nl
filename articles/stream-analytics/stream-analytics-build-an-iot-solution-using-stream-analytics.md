---
title: Een IoT-oplossing bouwen met behulp van Azure Stream Analytics
description: Zelfstudie voor de Stream Analytics IoT-oplossing van een scenario tolhuisje aan de slag
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh, sngun
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/21/2018
ms.openlocfilehash: 80e287d09fdc5ab7157b9ee46bc830fd2db4d501
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30912267"
---
# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Een IoT-oplossing bouwen met behulp van de Stream Analytics

## <a name="introduction"></a>Inleiding
In deze oplossing leert u hoe u met Azure Stream Analytics realtime-inzichten verkrijgen van uw gegevens. Ontwikkelaars kunnen eenvoudig gegevensstromen, zoals Klik streams logboeken en gebeurtenissen die door de apparaten worden gegenereerd met historische records of referentiegegevens worden afgeleid van zakelijke inzichten combineren. Als een volledig beheerde, realtime stroom berekening service die wordt gehost in Microsoft Azure, biedt Azure Stream Analytics ingebouwde tolerantie, lage latentie en schaalbaarheid voor bent u in minuten uitgevoerd.

Na het voltooien van deze oplossing, zich u kunt:

* Tijd om uzelf bekend met de Azure Stream Analytics-portal.
* Configureren en implementeren van een streaming-taak.
* Bewoordingen aan echte problemen en ze op te lossen met behulp van de Stream Analytics query language.
* Ontwikkel streaming van oplossingen voor uw klanten met behulp van de Stream Analytics met vertrouwen.
* Gebruik de controle en logboekregistratie ervaring problemen kunt oplossen.

## <a name="prerequisites"></a>Vereisten
U moet de volgende vereisten voor het voltooien van deze oplossing:
* Een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/)

## <a name="scenario-introduction-hello-toll"></a>Scenario Inleiding: "Hallo, Tolstation!"
Een station tolstation is een algemene verschijnsel. Tijdens deze veel autowegen bruggen en tunnels over de hele wereld. Elk station tolstation heeft meerdere tolstation stands. U stoppen als u wilt de tolstation betalen aan een daarmee gepaard gaande op handmatige stands. Een sensor boven op elke stand scant op geautomatiseerde stands een RFID-kaart wordt aangebracht op de voorruit van uw vehicle verwerkt als u de gratis stand doorgeven. Het is gemakkelijk om te visualiseren van het verstrijken van de door middel van deze stations tolstation als een stroom gebeurtenissen waarover interessante bewerkingen kunnen worden uitgevoerd.

![Afbeelding van auto's op een tolstation stands](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Binnenkomende gegevens
Deze oplossing werkt met twee streams gegevens. Sensoren die zijn geïnstalleerd in de in- en afsluiten van de stations tolstation produceren de eerste stroom. De tweede stroom is een statische lookup gegevensset die vehicle registratiegegevens heeft.

### <a name="entry-data-stream"></a>Gegevensstroom van vermelding
De gegevensstroom vermelding bevat informatie over auto's wanneer deze stations tolstation binnengaat. De afsluitcode Gegevensgebeurtenissen zijn live gestreamd in een wachtrij Event Hub van een Web-App die is opgenomen in de voorbeeld-app.

| TollID | EntryTime | LicensePlate | Status | Maken | Model | VehicleType | VehicleWeight | Gratis | Label |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 |2014-09-10 12:01:00.000 |JNB 7001 |NY |Honda |CRV |1 |0 |7 | |
| 1 |2014-09-10 12:02:00.000 |YXZ 1001 |NY |Toyota |Camry |1 |0 |4 |123456789 |
| 3 |2014-09-10 12:02:00.000 |ABC 1004 |CT |Ford |Taurus |1 |0 |5 |456789123 |
| 2 |2014-09-10 12:03:00.000 |XYZ 1003 |CT |Toyota |Corolla |1 |0 |4 | |
| 1 |2014-09-10 12:03:00.000 |BNJ 1007 |NY |Honda |CRV |1 |0 |5 |789123456 |
| 2 |2014-09-10 12:05:00.000 |CDE 1007 |NJ |Toyota |4x4 |1 |0 |6 |321987654 |

Hier volgt een korte beschrijving van de kolommen:

| Kolom | Beschrijving |
| --- | --- |
| TollID |De tolstation stand-ID die een unieke identificatie van een stand tolstation |
| EntryTime |De datum en tijd van de vermelding van de drager naar de stand tolstation in UTC |
| LicensePlate |De sofi-nummer van de drager |
| Status |Een status in de Verenigde Staten |
| Maken |De fabrikant van de auto |
| Model |Het modelnummer van het auto |
| VehicleType |1 voor passagiers voertuigen of 2 voor commerciële voertuigen |
| WeightType |Toegestaan gewicht in ton; 0 voor passagiers voertuigen |
| Gratis |De waarde tolstation in USD |
| Label |De e-code op de auto die betaling automatiseert. lege wanneer de betaling handmatig is uitgevoerd |

### <a name="exit-data-stream"></a>De gegevensstroom afsluiten
De gegevensstroom afsluiten bevat informatie over het station tolstation verlaten auto's. De afsluitcode Gegevensgebeurtenissen zijn live gestreamd in een wachtrij Event Hub van een Web-App die is opgenomen in de voorbeeld-app.

| **TollId** | **ExitTime** | **LicensePlate** |
| --- | --- | --- |
| 1 |2014-09-10T12:03:00.0000000Z |JNB 7001 |
| 1 |2014-09-10T12:03:00.0000000Z |YXZ 1001 |
| 3 |2014-09-10T12:04:00.0000000Z |ABC 1004 |
| 2 |2014-09-10T12:07:00.0000000Z |XYZ 1003 |
| 1 |2014-09-10T12:08:00.0000000Z |BNJ 1007 |
| 2 |2014-09-10T12:07:00.0000000Z |CDE 1007 |

Hier volgt een korte beschrijving van de kolommen:

| Kolom | Beschrijving |
| --- | --- |
| TollID |De tolstation stand-ID die een unieke identificatie van een stand tolstation |
| ExitTime |De datum en tijd van af te sluiten van de drager van tolstation stand in UTC |
| LicensePlate |De sofi-nummer van de drager |

### <a name="commercial-vehicle-registration-data"></a>De registratiegegevens commerciële vehicle
De oplossing maakt gebruik van een momentopname van een registratiedatabase commerciële vehicle verwerkt. Deze gegevens wordt opgeslagen als een JSON-bestand in Azure blob-opslag, opgenomen in de steekproef.

| LicensePlate | Registratie-id | Verlopen |
| --- | --- | --- |
| SVT 6023 |285429838 |1 |
| XLZ 3463 |362715656 |0 |
| BAC 1005 |876133137 |1 |
| RIV 8632 |992711956 |0 |
| SNY 7188 |592133890 |0 |
| ELH 9896 |678427724 |1 |

Hier volgt een korte beschrijving van de kolommen:

| Kolom | Beschrijving |
| --- | --- |
| LicensePlate |De sofi-nummer van de drager |
| Registratie-id |De drager registratie-ID |
| Verlopen |De registratiestatus van de drager: 0 of vehicle registratie actief is, is 1 als de registratie is verlopen |

## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Instellen van de omgeving voor Azure Stream Analytics
Voor het voltooien van deze oplossing, moet u een Microsoft Azure-abonnement. Als u geen Azure-account hebt, kunt u [aanvragen van een gratis evaluatieversie](http://azure.microsoft.com/pricing/free-trial/).

Zorg ervoor dat de stappen in de sectie 'Opschonen van uw Azure-account' aan het einde van dit artikel zodat u kunt het beste gebruik van uw Azure-tegoed.

## <a name="deploy-the-sample"></a>Het voorbeeld implementeren 
Er zijn verschillende bronnen die gemakkelijk kunnen worden geïmplementeerd in een resourcegroep samen met een paar klikken. De definitie van de oplossing wordt gehost in github-opslagplaats op [ https://github.com/Azure/azure-stream-analytics/tree/master/Samples/TollApp ](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/TollApp).

### <a name="deploy-the-tollapp-template-in-the-azure-portal"></a>De sjabloon TollApp in de Azure portal implementeren
1. Als u wilt implementeren de TollApp-omgeving op Azure, gebruik je deze link naar [TollApp Azure-sjabloon implementeren](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-stream-analytics%2Fmaster%2FSamples%2FTollApp%2FVSProjects%2FTollAppDeployment%2Fazuredeploy.json).

2. Meld u aan bij de Azure portal als u wordt gevraagd.

3. Kies het abonnement waarin de verschillende resources worden gefactureerd.

4. Geef een nieuwe resourcegroep met een unieke naam, bijvoorbeeld `MyTollBooth`. 

5. Selecteer een Azure-locatie.

6. Geef een **Interval** als een aantal seconden. Deze waarde wordt gebruikt in de voorbeeld-web-app, voor hoe vaak gegevens worden verzonden naar de Event Hub. 

7. **Controleer** om de voorwaarden en bepalingen te accepteren.

8. Selecteer **vastmaken aan dashboard** zodat u de resources later kan vinden.

9. Selecteer **aankoop** om de voorbeeldsjabloon te implementeren.

10. Na enkele ogenblikken wordt een melding weergegeven om te bevestigen dat de **implementatie is voltooid**.

### <a name="review-the-azure-stream-analytics-tollapp-resources"></a>Controleer de Azure Stream Analytics TollApp resources
1. Aanmelden bij Azure Portal

2. Zoek de resourcegroep die u met de naam in de vorige sectie.

3. Controleer of dat de volgende bronnen worden vermeld in de resourcegroep:
   - Een Cosmos DB-Account
   - Een Azure Stream Analytics-taak
   - Een Azure Storage-Account
   - One Azure Event Hub
   - Twee Web-Apps

## <a name="examine-the-sample-tollapp-job"></a>Bekijk het voorbeeld TollApp taak 
1. Vanaf de resourcegroep in de vorige sectie, selecteert u de Stream Analytics-taak streaming begint met de naam **tollapp** (naam bevat willekeurige tekens voor uniekheid).

2. Op de **overzicht** pagina van de taak, kennisgeving de **Query** vak om de querysyntaxis weer te geven.

   ```sql
   SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
   INTO CosmosDB
   FROM EntryStream TIMESTAMP BY EntryTime
   GROUP BY TUMBLINGWINDOW(minute, 3), TollId
   ```

   Om de bedoeling van de query parafraseren, Stel dat u moet het aantal voertuigen die een tolstation stand invoeren. Omdat een snelweg tolstation stand een continue stroom van voertuigen heeft, zijn de ingang gebeurtenissen zijn vergelijkbaar met een stroom die nooit wordt gestopt. Als u wilt de stroom kwantificeren, die u hebt voor het definiëren van een 'periode' om te meten via. Laten we verfijnen bovendien de vraag naar 'hoeveel voertuigen Geef een tolstation stand elke drie minuten?' Dit wordt vaak aangeduid als het aantal daling.

   Zoals u ziet, Azure Stream Analytics maakt gebruik van een querytaal die lijkt op SQL en enkele uitbreidingen om op te geven tijd gerelateerde aspecten van de query toegevoegd.  Lees voor meer informatie over [Time Management](https://msdn.microsoft.com/library/azure/mt582045.aspx) en [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) constructies die worden gebruikt in de query.

3. Controleer de invoer van de taak TollApp-voorbeeld. Alleen de invoer EntryStream wordt gebruikt in de huidige query.
   - **EntryStream** invoer is een Event Hub-verbinding die telkens wanneer een auto krijgt een tolhuisje op de weg die gegevens in de wachtrij geplaatst. Een web-app die deel uitmaakt van het voorbeeld maakt de gebeurtenissen en die gegevens in de wachtrij in deze Event Hub. Houd er rekening mee dat deze invoer in de component FROM van de streaming-query wordt opgevraagd.
   - **ExitStream** invoer is een Event Hub-verbinding die gegevens over elke keer dat een auto wordt een tolhuisje afgesloten op de weg in de wachtrij geplaatst. Deze streaming invoer wordt in latere variaties van de syntaxis van de query gebruikt.
   - **Registratie** invoer is een Azure Blob storage-verbinding, die verwijst naar een bestand statische registration.json, dat wordt gebruikt voor zoekopdrachten indien nodig. Deze verwijzing gegevensinvoer wordt in latere variaties van de syntaxis van de query gebruikt.

4. Bekijk de uitvoer van de taak TollApp-voorbeeld.
   - **Cosmos DB** uitvoer is een Cosmos-database-verzameling die de uitvoer sink gebeurtenissen ontvangt. Houd er rekening mee dat deze uitvoer wordt gebruikt in een component van de streaming-query.

## <a name="start-the-tollapp-streaming-job"></a>Start de TollApp streaming-taak
Volg deze stappen voor het starten van de streaming-taak:

1. Op de **overzicht** pagina van de taak, selecteer **Start**.

2. Op de **starttaak** deelvenster **nu**.

3. Na enkele ogenblikken, zodra de taak wordt uitgevoerd, op de **overzicht** pagina van de streaming-taak, weergave de **bewaking** grafiek. De grafiek moet worden enkele duizenden invoervelden en tientallen uitvoergebeurtenissen dat wordt weergegeven.

## <a name="review-the-cosmosdb-output-data"></a>Bekijk de uitvoergegevens CosmosDB
1. Zoek de resourcegroep met de resources TollApp.

2. Selecteer de Azure Cosmos DB rekening met het naampatroon **tollapp<random>-cosmos**.

3. Selecteer de **Data Explorer** kolomkop klikken om de pagina Data Explorer te openen.

4. Vouw de **tollAppDatabase** > **tollAppCollection** > **documenten**.

5. In de lijst met de id's worden verschillende docs weergegeven zodra de uitvoer beschikbaar is.

6. Selecteer elke id om de JSON-document te controleren. Let op elke tollid, windowend tijd en het aantal auto's uit dit venster.

7. Na een extra drie minuten een andere verzameling vier documenten beschikbaar is, een document per tollid. 


## <a name="report-total-time-for-each-car"></a>Totale tijd voor elke auto rapport
De gemiddelde tijd die zijn voor een auto vereist doorgeven aan de tolstation helpt bij het beoordelen van de efficiëntie van het proces en de gebruikerservaring.

Ga voor de totale tijd lid worden van de stroom ExitTime EntryTime stream. Voeg de twee ingevoerde stromen op gelijke overeenkomende TollId en LicencePlate kolommen. De **JOIN** operator moet u tijdelijke eenheidsprofiel die het toegestane tijdverschil de gekoppelde gebeurtenissen beschrijft opgeven. Gebruik de **DATEDIFF** functie opgeven die gebeurtenissen langer zijn dan 15 minuten van elkaar moet. Ook van toepassing op de **DATEDIFF** functie om af te sluiten en tijden van vermelding voor het berekenen van de werkelijke tijd waarop een auto doorbrengt in het station tolstation. Noteer het verschil van het gebruik van **DATEDIFF** wanneer het wordt gebruikt in een **Selecteer** instructie in plaats van een **JOIN** voorwaarde.

```sql
SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute, EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
INTO CosmosDB
FROM EntryStream TIMESTAMP BY EntryTime
JOIN ExitStream TIMESTAMP BY ExitTime
ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15
```

### <a name="to-update-the-tollapp-streaming-job-query-syntax"></a>De streaming-taak querysyntaxis TollApp bijwerken:

1. Op de **overzicht** pagina van de taak, selecteer **stoppen**.

2. Wacht enige tijd voor de melding dat de taak is gestopt.

3. Selecteer onder de kop taak TOPOLOGIE **haken Query**

4. Plak de gecorrigeerde streaming SQL-query.

5. Selecteer **opslaan** de query op te slaan. Bevestig **Ja** de wijzigingen wilt opslaan.

6. Op de **overzicht** pagina van de taak, selecteer **Start**.

7. Op de **starttaak** deelvenster **nu**.

### <a name="review-the-total-time-in-the-output"></a>Bekijk de totale tijd in de uitvoer
Herhaal de stappen in de vorige sectie om te controleren van de CosmosDB uitvoergegevens van de streaming-taak. Bekijk de meest recente JSON-documenten. 

Dit document ziet u bijvoorbeeld een auto voorbeeld met een bepaalde nummerbord en de tijd entrytime naar en uitgang uit het DATEDIFF durationinminutes berekende veld de duur van de stand tolstation als twee minuten weergegeven: 
```JSON
{
    "tollid": 4,
    "entrytime": "2018-04-05T06:51:39.0491173Z",
    "exittime": "2018-04-05T06:53:09.0491173Z",
    "licenseplate": "JVR 9425",
    "durationinminutes": 2,
    "id": "ff52eb25-d580-7566-2879-1f52bba6601e",
    "_rid": "+8E4AI1DZgBjAAAAAAAAAA==",
    "_self": "dbs/+8E4AA==/colls/+8E4AI1DZgA=/docs/+8E4AI1DZgBjAAAAAAAAAA==/",
    "_etag": "\"ad02f6b8-0000-0000-0000-5ac5c8330000\"",
    "_attachments": "attachments/",
    "_ts": 1522911283
}
```

## <a name="report-vehicles-with-expired-registration"></a>Rapport voertuigen met verlopen registratie
Azure Stream Analytics kunt statische momentopnamen van referentiegegevens samen te voegen met de tijdelijke gegevensstromen. Gebruik de volgende voorbeeldquery vraag ter illustratie van deze mogelijkheid. De registratie-invoer is een statische blob json-bestand met een lijst met de accountwachtwoorden van licentie-codes. Door op de nummerbord, wordt de referentiegegevens vergeleken met elke vehicle verwerkt de tolstation te doorlopen. 

Als een commerciële-medium is geregistreerd bij het bedrijf tolstation, kan deze de tolstation stand passeren zonder te zijn gestopt voor inspectie. Gebruik de opzoektabel registratie voor het identificeren van alle commerciële voertuigen die registraties zijn verlopen.

```sql
SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
INTO CosmosDB
FROM EntryStream TIMESTAMP BY EntryTime
JOIN Registration
ON EntryStream.LicensePlate = Registration.LicensePlate
WHERE Registration.Expired = '1'
```

1. Herhaal de stappen in de vorige sectie de streaming-taak querysyntaxis TollApp bijwerken.

2. Herhaal de stappen in de vorige sectie om te controleren van de CosmosDB uitvoergegevens van de streaming-taak. 

Voorbeelduitvoer:
```json
    {
        "entrytime": "2018-04-05T08:01:28.0252168Z",
        "licenseplate": "GMT 3221",
        "tollid": 1,
        "registrationid": "763220582",
        "id": "47db0535-9716-4eb2-db58-de7886966cbf",
        "_rid": "y+F8AJ9QWACSAQAAAAAAAA==",
        "_self": "dbs/y+F8AA==/colls/y+F8AJ9QWAA=/docs/y+F8AJ9QWACSAQAAAAAAAA==/",
        "_etag": "\"88007d8d-0000-0000-0000-5ac5d7e20000\"",
        "_attachments": "attachments/",
        "_ts": 1522915298
    }
```

## <a name="scale-out-the-job"></a>De taak uitbreiden
Azure Stream Analytics is ontworpen voor elastisch schalen zodat deze kan grote hoeveelheden gegevens verwerken. De Azure Stream Analytics query kunt gebruiken een **PARTITION BY** component zien of het systeem in deze stap uitgeschaald. **PartitionId** is een speciale kolom die het systeem wordt toegevoegd aan de partitie-ID van de invoer (event hub).

Als u wilt uitbreiden de query voor partities, bewerk de syntaxis van de query naar de volgende code:
```sql
SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
INTO CosmosDB
FROM EntryStream 
TIMESTAMP BY EntryTime 
PARTITION BY PartitionId
GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId
```

Voor de streaming-taak naar meer eenheden voor streaming opschalen:

1. **Stop** de huidige taak. 

2. Bijwerken van de syntaxis van de query in de **haken Query** pagina en de wijzigingen opslaan.

3. Selecteer onder de kop configureren op de streaming-taak **Scale**.
   
4. Schuif de **Streamingeenheden** schuifregelaar van 1 tot en met 6. Streaming-eenheden definiëren de hoeveelheid rekenkracht die de taak kan ontvangen. Selecteer **Opslaan**.

5. **Start** de streaming-taak voor het demonstreren van de aanvullende schaal. Azure Stream Analytics werk verdeelt over meer computerresources en betere doorvoer behalen partitionering van het werk tussen resources met behulp van de kolom aangegeven in de component PARTITION BY. 

## <a name="monitor-the-job"></a>De taak bewaken
De **MONITOR** gebied statistieken over actieve taak bevat. Configuratie van de eerste keer is nodig voor het gebruik van het opslagaccount in dezelfde regio (tolstation naam als de rest van dit document).   

![Schermopname van monitor](media/stream-analytics-build-an-iot-solution-using-stream-analytics/monitoring.png)

U hebt toegang tot **activiteitenlogboeken** vanuit het dashboard taak **instellingen** ook gebied.

## <a name="clean-up-the-tollapp-resources"></a>De resources TollApp opschonen
1. Stop de Stream Analytics-taak in de Azure portal.

2. Zoek de resourcegroep met acht bronnen die betrekking hebben op de sjabloon TollApp.

3. Selecteer **Resourcegroep verwijderen**. Typ de naam van de resourcegroep verwijderen te bevestigen.

## <a name="conclusion"></a>Conclusie
Deze oplossing is geïntroduceerd met de Azure Stream Analytics-service. Het configureren van de invoer en uitvoer voor de Stream Analytics-taak gedemonstreerd. Met behulp van het scenario niet gratis gegevens, de oplossing voorkomende problemen die optreden in de ruimte van de gegevens in beweging en hoe ze kunnen worden opgelost met eenvoudige SQL-achtige query's in Azure Stream Analytics uitgelegd. De oplossing beschreven SQL extensie constructs voor het werken met tijdelijke gegevens. Deze hebt u geleerd hoe gegevensstromen kunt koppelen, hoe de gegevensstroom met statische referentiegegevens sitmuleren en hoe moet worden uitgebreid met een query naar een hogere doorvoer te bereiken.

Hoewel deze oplossing een goede inleiding biedt, is het niet voltooid door middel van. U vindt meer querypatronen met de taal SAQL op [voorbeelden voor het algemene gebruikspatronen van de Stream Analytics Query](stream-analytics-stream-analytics-query-patterns.md).
