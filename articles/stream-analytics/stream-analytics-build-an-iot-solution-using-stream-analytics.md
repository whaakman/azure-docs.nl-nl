---
title: Een IoT-oplossing bouwen met behulp van de Stream Analytics | Microsoft Docs
description: Zelfstudie voor de Stream Analytics IoT-oplossing van een scenario tolhuisje aan de slag
keywords: IOT-oplossing, vensterfuncties
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: kfile
editor: cgronlun
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/12/2018
ms.author: sngun
ms.openlocfilehash: cc84a34a410a750ddf2acb8f19b3bb809d269098
ms.sourcegitcommit: a0d2423f1f277516ab2a15fe26afbc3db2f66e33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Een IoT-oplossing bouwen met behulp van de Stream Analytics

## <a name="introduction"></a>Inleiding
In deze zelfstudie leert u hoe u met Azure Stream Analytics realtime-inzichten verkrijgen van uw gegevens. Ontwikkelaars kunnen eenvoudig gegevensstromen, zoals Klik streams logboeken en gebeurtenissen die door de apparaten worden gegenereerd met historische records of referentiegegevens worden afgeleid van zakelijke inzichten combineren. Als een volledig beheerde, realtime stroom berekening service die wordt gehost in Microsoft Azure, biedt Azure Stream Analytics ingebouwde tolerantie, lage latentie en schaalbaarheid voor bent u in minuten uitgevoerd.

Na het voltooien van deze zelfstudie, kunt u zich kunt:

* Tijd om uzelf bekend met de Azure Stream Analytics-portal.
* Configureren en implementeren van een streaming-taak.
* Bewoordingen aan echte problemen en ze op te lossen met behulp van de Stream Analytics query language.
* Ontwikkel streaming van oplossingen voor uw klanten met behulp van de Stream Analytics met vertrouwen.
* Gebruik de controle en logboekregistratie ervaring problemen kunt oplossen.

## <a name="prerequisites"></a>Vereisten
U moet de volgende vereisten om deze zelfstudie te voltooien:

* De nieuwste versie van [Azure PowerShell](/powershell/azure/overview)
* Visual Studio 2017 2015, of het gratis [Visual Studio Community](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
* Een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/)
* Beheerdersbevoegdheden op de computer
* Het downloaden van de [TollApp.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TollApp/TollApp.zip) uit het Microsoft Downloadcentrum
* Optioneel: De broncode voor de generator van de gebeurtenis TollApp in [GitHub](https://aka.ms/azure-stream-analytics-toll-source)

## <a name="scenario-introduction-hello-toll"></a>Scenario Inleiding: "Hallo, Tolstation!"
Een station tolstation is een algemene verschijnsel. Tijdens deze veel autowegen bruggen en tunnels over de hele wereld. Elk station tolstation heeft meerdere tolstation stands. U stoppen als u wilt de tolstation betalen aan een daarmee gepaard gaande op handmatige stands. Een sensor boven op elke stand scant op geautomatiseerde stands een RFID-kaart wordt aangebracht op de voorruit van uw vehicle verwerkt als u de gratis stand doorgeven. Het is gemakkelijk om te visualiseren van het verstrijken van de door middel van deze stations tolstation als een stroom gebeurtenissen waarover interessante bewerkingen kunnen worden uitgevoerd.

![Afbeelding van auto's op een tolstation stands](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Binnenkomende gegevens
Deze zelfstudie werkt met twee streams gegevens. Sensoren die zijn geïnstalleerd in de in- en afsluiten van de stations tolstation produceren de eerste stroom. De tweede stroom is een statische lookup gegevensset die vehicle registratiegegevens heeft.

### <a name="entry-data-stream"></a>Gegevensstroom van vermelding
De gegevensstroom vermelding bevat informatie over auto's wanneer deze stations tolstation binnengaat.

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
De gegevensstroom afsluiten bevat informatie over het station tolstation verlaten auto's.

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
De zelfstudie maakt gebruik van een momentopname van een registratiedatabase commerciële vehicle verwerkt.

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
Voor deze zelfstudie hebt voltooid, moet u een Microsoft Azure-abonnement. Microsoft biedt een gratis proefversie van Microsoft Azure-services.

Als u geen Azure-account hebt, kunt u [aanvragen van een gratis evaluatieversie](http://azure.microsoft.com/pricing/free-trial/).

> [!NOTE]
> Als u wilt aanmelden voor een gratis proefversie, moet u een mobiel apparaat dat u kunt tekstberichten en een geldige creditcard ontvangen.
> 
> 

Zorg ervoor dat de stappen in de sectie 'Opschonen van uw Azure-account' aan het einde van dit artikel zodat u kunt het beste gebruik van uw Azure-tegoed.

## <a name="provision-azure-resources-required-for-the-tutorial"></a>Azure-resources nodig zijn voor de zelfstudie inrichten
Deze zelfstudie vereist twee event hubs ontvangen *vermelding* en *sluiten* gegevensstromen. Azure SQL Database worden de resultaten van de Stream Analytics-taken. Azure Storage worden opgeslagen referentiegegevens over vehicle registraties.

U kunt het script Setup.ps1 in de map TollApp op GitHub gebruiken om alle vereiste resources te maken. Met het oog op tijd, is het raadzaam dat u het uitvoert. Als u meer informatie over het configureren van deze resources in de Azure portal wilt, raadpleegt u de bijlage 'Zelfstudie resources in Azure-portal configureren'.

Download en sla de ondersteunende [TollApp](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TollApp/TollApp.zip) map en bestanden.

Open een **Microsoft Azure PowerShell** venster *als beheerder*. Als u nog geen Azure PowerShell, volg de instructies in [Azure PowerShell installeren en configureren](/powershell/azure/overview) om deze te installeren.

Omdat Windows wordt automatisch geblokkeerd .ps1-, dll- en .exe-bestanden, moet u het uitvoeringsbeleid instellen voordat u het script uitvoeren. Zorg ervoor dat de Azure PowerShell-venster wordt uitgevoerd *als beheerder*. Voer **Set-ExecutionPolicy unrestricted**. Wanneer u wordt gevraagd, typt u **Y**.

![Schermopname van 'Set-ExecutionPolicy unrestricted' uitgevoerd in Azure PowerShell-venster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Voer **Get-ExecutionPolicy** om ervoor te zorgen dat de opdracht gewerkt.

![Schermopname van 'Get-ExecutionPolicy' uitgevoerd in Azure PowerShell-venster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Ga naar de map waarin de scripts en de generator van toepassing is.

![Schermopname van 'cd .\TollApp\TollApp' in de Azure PowerShell-venster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Type **.\\ Setup.ps1** als u uw Azure-account instelt, maken en alle vereiste bronnen configureren en starten gebeurtenissen genereren. Het script willekeurig neemt over een regio om uw resources te maken. Als u expliciet een regio, u kunt doorgeven de **-locatie** parameter zoals in het volgende voorbeeld:

**. \\Setup.ps1-locatie 'VS-midden'**

![Schermafbeelding van de Azure-aanmeldingspagina](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

Hiermee opent u het script de **aanmelden** pagina voor Microsoft Azure. Voer de referenties van uw account.

> [!NOTE]
> Als uw account toegang tot meerdere abonnementen heeft, wordt u gevraagd de naam van het abonnement dat u wilt gebruiken voor de zelfstudie invoeren.
> 
> 

Het script kan enige tijd duren om uit te voeren. Nadat deze is voltooid, wordt de uitvoer moet eruitzien als in de volgende schermafbeelding.

![Schermopname van het resultaat van het script in de Azure PowerShell-venster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

U ziet ook een ander venster die vergelijkbaar is met de volgende schermopname. Deze toepassing is het verzenden van gebeurtenissen naar Azure Event Hubs, die is vereist voor het uitvoeren van de zelfstudie. Dus geen stop de toepassing of dit venster sluiten totdat u de zelfstudie hebt voltooid.

![Schermopname van 'Verzenden event hub gegevens'](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

U moet nu zien van uw resources in Azure-portal. Ga naar <https://portal.azure.com>, en meld u aan met referenties van uw account. Houd er rekening mee dat momenteel bepaalde functionaliteit maakt gebruik van de klassieke portal. Deze stappen worden duidelijk vermeld.

### <a name="azure-event-hubs"></a>Azure Event Hubs

Klik in de Azure-portal op **meer services** aan de onderkant van het deelvenster links management. Type **Event hubs** in het veld dat is opgegeven, ziet u een nieuwe Event Hub-naamruimte die met begint **tolldata**. Deze namesapce wordt gemaakt door het script Setup.ps1. Ziet u twee event hubs met de naam **vermelding** en **sluiten** gemaakt in deze naamruimte.

### <a name="azure-storage-container"></a>Azure Storage-container
Vanuit de Azure-portal bladert u naar de storage-accounts, ziet u een opslagaccount die met begint **tolldata**. Klik op de **tolldata** container om te zien van het geüploade JSON-bestand met registratiegegevens vehicle verwerkt.

### <a name="azure-sql-database"></a>Azure SQL Database
1. Ga terug naar de Azure-portal op het eerste tabblad die is geopend in de browser. Klik op **SQL-DATABASES** aan de linkerkant van de Azure portal om te zien van de SQL-database die wordt gebruikt in de zelfstudie en klik op **tolldatadb**.
   
    ![Schermafbeelding van de SQL-database gemaakt](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)
2. Kopieer de naam van de server zonder het poortnummer (*servername*. database.windows.net bijvoorbeeld).
    ![Schermafbeelding van de gemaakte database van SQL database](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15a.png)

## <a name="connect-to-the-database-from-visual-studio"></a>Verbinding maken met de database vanuit Visual Studio
Visual Studio gebruiken voor toegang tot de resultaten van de query in de Uitvoerdatabase.

Verbinding maken met de SQL-database (doel) vanuit Visual Studio:

1. Open Visual Studio en klik vervolgens op **extra** > **verbinding maken met Database**.
2. Als u wordt gevraagd, klikt u op **Microsoft SQL Server** als gegevensbron.
   
    ![Het dialoogvenster gegevensbron wijzigen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)
3. In de **servernaam** veld, plak de naam die u in de vorige sectie hebt gekopieerd vanuit de Azure-portal (dat wil zeggen, *servername*. database.windows.net).
4. Klik op **SQL Server-verificatie gebruiken**.
5. Voer **tolladmin** in de **gebruikersnaam** veld en **123toll!** in de **wachtwoord** veld.
6. Klik op **Selecteer of voer een databasenaam**, en selecteer **TollDataDB** als de database.
   
    ![Het dialoogvenster verbinding toevoegen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)
7. Klik op **OK**.
8. Open in Server Explorer.
   
    ![Server Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)
9. Zie vier tabellen in de database TollDataDB.
   
    ![Tabellen in de database TollDataDB](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Gebeurtenis generator: TollApp-voorbeeldproject
Het PowerShell-script wordt automatisch gestart voor het verzenden van gebeurtenissen met behulp van de toepassing van TollApp voorbeeld. U hoeft niet te eventuele extra stappen uitvoeren.

Als u geïnteresseerd in de implementatiegegevens bent, kunt u de broncode van de toepassing TollApp vinden in GitHub [samples/TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Schermopname van voorbeeldcode weergegeven in Visual Studio](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken
1. Klik op het groen plusteken in de linkerbovenhoek van de pagina voor het maken van een nieuwe Stream Analytics-taak in de Azure portal. Selecteer **Intelligence en analyse** en klik vervolgens op **Stream Analytics-taak**.
   
    ![Knop Nieuw](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)
2. Geef de taaknaam van een, het valideren van het abonnement is corrigeren en vervolgens een nieuwe resourcegroep maken in dezelfde regio bevinden als de Event hub-opslag (is standaard Zuid-centraal VS voor het script).
3. Klik op **vastmaken aan dashboard** en vervolgens **maken** aan de onderkant van de pagina.
   
    ![Optie voor Stream Analytics-taak maken](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Invoerbronnen definiëren
1. De taak maakt en open de pagina van de taak. U kunt ook op de gemaakte analytics-taak op het portal-dashboard.

2. Klik op de **invoer** tabblad de brongegevens definiëren.
   
    ![Het tabblad invoer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.png)
3. Klik op **invoer toevoegen**.
   
    ![Het toevoegen van een invoer-optie](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)
4. Voer **EntryStream** als **INVOERALIAS**.
5. Brontype is **gegevensstroom**
6. Bron is **Event hub**.
7. **Service bus-naamruimte** moet u de TollData een in de vervolgkeuzelijst.
8. **Naam Event hub** moet worden ingesteld op **vermelding**.
9. **Naam Event hub beleid*is **RootManageSharedAccessKey** (de standaardwaarde).
10. Selecteer **JSON** voor **GEBEURTENIS SERIALISATIE-indeling** en **UTF8** voor **codering**.
   
    Uw instellingen, ziet er als:
   
    ![Event hub-instellingen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

10. Klik op **maken** onder aan de pagina om de wizard te voltooien.
    
    Nu u de stroom van de vermelding hebt gemaakt, wordt u dezelfde stappen voor het maken van de stroom afsluiten volgt. Moet u waarden op te geven als op de volgende schermafbeelding.
    
    ![Instellingen voor de stroom afsluiten](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)
    
    U hebt twee invoer streams gedefinieerd:
    
    ![Invoer streams gedefinieerd in de Azure-portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.png)
    
    Vervolgens voegt u gegevensinvoer verwijzing voor het blob-bestand dat de registratiegegevens auto bevat.
11. Klik op **toevoegen**, en doe hetzelfde voor de invoer stroom maar selecteer **REFERENTIEGEGEVENS** in plaats van **gegevensstroom** en de **invoer Alias** is **registratie**.

12. Storage-account die met begint **tolldata**. De containernaam moet **tolldata**, en de **pad PATROON** moet **registration.json**. Deze naam is hoofdlettergevoelig en moet **kleine**.
    
    ![Instellingen voor de opslag blog](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)
13. Klik op **maken** om de wizard te voltooien.

Nu worden alle invoer gedefinieerd.

## <a name="define-output"></a>Uitvoer definiëren
1. Selecteer in het deelvenster Beheeroverzicht van Stream Analytics-taak **uitvoer**.
   
    ![De optie 'Uitvoer toevoegen' en het tabblad uitvoer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.png)
2. Klik op **Add**.
3. Stel de **uitvoeraliassen** aan 'uitvoer' en vervolgens **Sink** naar **SQL-database**.
3. Selecteer de naam van de server die is gebruikt in de sectie 'Verbinding naar Database vanuit Visual Studio' van het artikel. De databasenaam is **TollDataDB**.
4. Voer **tolladmin** in de **gebruikersnaam** veld **123toll!** in de **wachtwoord** veld en **TollDataRefJoin** in de **tabel** veld.
   
    ![SQL Database-instellingen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.png)
5. Klik op **Create**.

## <a name="azure-stream-analytics-query"></a>Azure Stream analytics query
De **QUERY** tabblad bevat een SQL-query waarmee de inkomende gegevens worden omgezet.

![Een query die wordt toegevoegd aan het tabblad Query](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.png)

Deze zelfstudie probeert te beantwoorden verschillende bedrijfsvragen die gerelateerd zijn aan tolbruggen gegevens en constructies Stream Analytics query's die kunnen worden gebruikt in Azure Stream Analytics een relevant antwoord te geven.

Voordat u uw eerste Stream Analytics-taak kijken dan enkele scenario's en de syntaxis van de query.

## <a name="introduction-to-azure-stream-analytics-query-language"></a>Inleiding tot Azure Stream Analytics query language
- - -
Stel dat u moet het aantal voertuigen die een tolstation stand invoeren. Omdat dit een continue stroom van gebeurtenissen, hebt u voor het definiëren van een 'tijdsperiode." Laten we wijzigen de vraag om 'hoeveel voertuigen Geef een tolstation stand elke drie minuten?'. Dit wordt vaak aangeduid als het aantal daling.

Bekijk de Azure Stream Analytics query waarmee deze vraag worden beantwoord:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Zoals u ziet, Azure Stream Analytics maakt gebruik van een querytaal die lijkt op SQL en enkele uitbreidingen om op te geven tijd gerelateerde aspecten van de query toegevoegd.

Lees voor meer informatie over [Time Management](https://msdn.microsoft.com/library/azure/mt582045.aspx) en [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) constructies die worden gebruikt in de query van MSDN.

## <a name="testing-azure-stream-analytics-queries"></a>Azure Stream Analytics query's testen
Nu u uw eerste Azure Stream Analytics query hebt geschreven, is het tijd om deze te testen met behulp van bestanden met voorbeeldgegevens zich in de map TollApp in het volgende pad:

**..\\TollApp\\TollApp\\gegevens**

Deze map bevat de volgende bestanden:

* Entry.json
* Exit.json
* Registration.json

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>Vraag 1: Het aantal voertuigen een tolstation stand
1. Open de Azure-portal en gaat u naar uw gemaakte Azure Stream Analytics-taak. Klik op de **QUERY** tabblad en plak de query uit de vorige sectie.

2. Voor het valideren van deze query op de voorbeeldgegevens, de gegevens uploaden naar de invoer EntryStream door te klikken op de... symbool en het selecteren van **voorbeeldgegevens uit bestand uploaden**.

    ![Schermopname van het bestand Entry.json](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)
3. Selecteer het bestand (Entry.json) op uw lokale computer in het deelvenster die wordt weergegeven en klik op **OK**. De **Test** pictogram wordt nu verlichten en worden geklikt.
   
    ![Schermopname van het bestand Entry.json](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.png)
3. Valideren dat de uitvoer van de query is zoals verwacht:
   
    ![Resultaten van de test](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.png)

## <a name="question-2-report-total-time-for-each-car-to-pass-through-the-toll-booth"></a>Vraag 2: Rapport totale tijd voor elke auto doorgeven aan de stand tolstation
De gemiddelde tijd die zijn voor een auto vereist doorgeven aan de tolstation helpt bij het beoordelen van de efficiëntie van het proces en de gebruikerservaring.

Ga voor de totale tijd die u wilt deelnemen aan de stroom ExitTime EntryTime stream. U kunt de stromen voor TollId en LicencePlate kolommen wordt toegevoegd. De **JOIN** operator moet u tijdelijke eenheidsprofiel die het toegestane tijdverschil de gekoppelde gebeurtenissen beschrijft opgeven. U gaat gebruiken **DATEDIFF** functie opgeven die gebeurtenissen langer zijn dan 15 minuten van elkaar moet. U wordt ook van toepassing de **DATEDIFF** functie om af te sluiten en tijden van vermelding voor het berekenen van de werkelijke tijd waarop een auto doorbrengt in het station tolstation. Noteer het verschil van het gebruik van **DATEDIFF** wanneer het wordt gebruikt in een **Selecteer** instructie in plaats van een **JOIN** voorwaarde.

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. Als u wilt deze query testen, kunt u de query bijwerken op de **QUERY** voor de taak. Toevoegen van het testbestand voor **ExitStream** net als bij **EntryStream** hierboven is opgegeven.
   
2. Klik op **Test**.

3. Schakel het selectievakje in voor het testen van de query en de uitvoer weergeven:
   
    ![Uitvoer van de test](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>Vraag 3: Alle commerciële voertuigen met verlopen registratie rapporteren
Azure Stream Analytics kunt statische momentopnamen van gegevens samen te voegen met de tijdelijke gegevensstromen. Gebruik de volgende voorbeeldquery vraag ter illustratie van deze mogelijkheid.

Als een commerciële-medium is geregistreerd bij het bedrijf tolstation, kan deze de tolstation stand passeren zonder te zijn gestopt voor inspectie. U gebruikt opzoektabel commerciële Vehicle registratie voor het identificeren van alle commerciële voertuigen die registraties zijn verlopen.

```
SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
FROM EntryStream TIMESTAMP BY EntryTime
JOIN Registration
ON EntryStream.LicensePlate = Registration.LicensePlate
WHERE Registration.Expired = '1'
```

Als u wilt een query testen met behulp van referentiegegevens, moet u definiëren invoerbron voor de verwijzingsgegevens, die u al hebt gedaan.

Als u wilt deze query testen, plakt u de query in de **QUERY** tabblad **testen**, en geeft u de twee invoerbronnen en de registratie van voorbeeldgegevens en klikt u op **testen**.  
   
![Uitvoer van de test](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

## <a name="start-the-stream-analytics-job"></a>De Stream Analytics-taak starten
Nu is het tijd om de configuratie te voltooien en start de taak. Sla de query van vraag 3, waarmee die wordt geproduceerd uitvoer die overeenkomt met het schema van de **TollDataRefJoin** uitvoertabel.

Ga naar de taak **DASHBOARD**, en klik op **START**.

![Schermafbeelding van de knop Start in het dashboard van de taak](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.png)

Wijzig in het dialoogvenster de **START uitvoer** tijd **aangepaste tijd**. Wijzig het uur in één uur vóór de huidige tijd. Deze wijziging zorgt ervoor dat alle gebeurtenissen van de event hub worden verwerkt sinds het begin van de gebeurtenissen aan het begin van de zelfstudie te genereren. Klik nu op de **Start** knop om de taak te starten.

![Selectie van aangepaste tijd](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

Start de taak kan enkele minuten duren. U kunt de status weergegeven op de pagina op het hoogste niveau voor Stream Analytics.

![Schermafbeelding van de status van de taak](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.png)

## <a name="check-results-in-visual-studio"></a>Controleer de resultaten in Visual Studio
1. Open Visual Studio Server Explorer en met de rechtermuisknop op de **TollDataRefJoin** tabel.
2. Klik op **tabelgegevens weergeven** om te zien van de uitvoer van de taak.
   
    ![Selectie van 'Tabelgegevens weergeven' in Server Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Azure Stream Analytics uitbreiden met taken
Azure Stream Analytics is ontworpen voor elastisch schalen zodat deze kan grote hoeveelheden gegevens verwerken. De Azure Stream Analytics query kunt gebruiken een **PARTITION BY** component zien of het systeem in deze stap wordt uitgebreid. **PartitionId** is een speciale kolom die het systeem wordt toegevoegd aan de partitie-ID van de invoer (event hub).

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. Stoppen van de huidige taak, het bijwerken van de query in de **QUERY** tabblad en open de **instellingen** stemmen in het dashboard van de taak. Klik op **Scale**.
   
    **STREAMING-eenheden** definiëren van de hoeveelheid rekenkracht die de taak kan ontvangen.
2. Wijzig de vervolgkeuzelijst omlaag van 1 6.
   
    ![Schermopname van het selecteren van 6 streaming-eenheden](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.png)
3. Ga naar de **uitvoer** tabblad en de naam van de SQL-tabel om te wijzigen **TollDataTumblingCountPartitioned**.

Als u de taak start nu kunt Azure Stream Analytics werk verdelen over meer computerresources en een betere doorvoer behalen. Houd er rekening mee dat de toepassing TollApp ook gebeurtenissen gepartitioneerd door TollId verzendt.

## <a name="monitor"></a>Controleren
De **MONITOR** gebied statistieken over actieve taak bevat. Eerste keer configuratie nodig is voor het gebruik van de opslag-account in dezelfde regio (tolstation naam als de rest van dit document).   

![Schermopname van monitor](media/stream-analytics-build-an-iot-solution-using-stream-analytics/monitoring.png)

U hebt toegang tot **activiteitenlogboeken** vanuit het dashboard taak **instellingen** ook gebied.


## <a name="conclusion"></a>Conclusie
Deze zelfstudie is geïntroduceerd met de Azure Stream Analytics-service. Het configureren van de invoer en uitvoer voor de Stream Analytics-taak gedemonstreerd. Met behulp van het scenario niet gratis gegevens, de zelfstudie voorkomende problemen die optreden in de ruimte van de gegevens in beweging en hoe ze kunnen worden opgelost met eenvoudige SQL-achtige query's in Azure Stream Analytics uitgelegd. De zelfstudie beschreven SQL extensie constructs voor het werken met tijdelijke gegevens. Deze hebt u geleerd hoe gegevensstromen kunt koppelen, hoe de gegevensstroom met statische referentiegegevens sitmuleren en hoe moet worden uitgebreid met een query naar een hogere doorvoer te bereiken.

Hoewel deze zelfstudie een goede inleiding biedt, is het niet voltooid door middel van. U vindt meer querypatronen met de taal SAQL op [voorbeelden voor het algemene gebruikspatronen van de Stream Analytics Query](stream-analytics-stream-analytics-query-patterns.md).
Raadpleeg de [onlinedocumentatie](https://azure.microsoft.com/documentation/services/stream-analytics/) voor meer informatie over Azure Stream Analytics.

## <a name="clean-up-your-azure-account"></a>Opschonen van uw Azure-account
1. Stop de Stream Analytics-taak in de Azure portal.
   
    Het script Setup.ps1 maakt twee event hubs en een SQL-database. De volgende instructies helpen u aan het einde van de zelfstudie opschonen van resources.
2. Typ in een PowerShell-venster **.\\ CleanUp.ps1** start het script waarmee bronnen die worden gebruikt in de zelfstudie wordt verwijderd.
   
   > [!NOTE]
   > Resources worden aangeduid met de naam. Zorg ervoor dat u elk item zorgvuldig controleren vóór de verwijdering te bevestigen.
   > 
   > 


