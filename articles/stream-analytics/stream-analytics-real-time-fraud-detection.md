# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Aan de slag met Azure Stream Analytics: realtime-fraudedetectie

Deze zelfstudie biedt een end-to-end-afbeelding van het gebruik van Azure Stream Analytics. Procedures voor: 

* Breng streaming-gebeurtenissen in een exemplaar van Azure Event Hubs. In deze zelfstudie gebruikt u een app die wij verstrekken die een stream van records met metagegevens van de mobiele telefoon simuleert.

* SQL-achtige Stream Analytics query's voor het omzetten van gegevens, informatie verzamelen of patronen zoekt schrijven. Hier ziet u hoe u een query naar de stroom inkomende onderzoeken en zoekt u naar aanroepen die mogelijk frauduleuze.

* De resultaten verzenden naar uitvoerlocatie (opslag) die u voor extra inzichten kunt analyseren. In dit geval hebt u de verdachte aanroepgegevens verzenden naar Azure Blob-opslag.

In deze zelfstudie gebruiken we het voorbeeld van realtime-fraudedetectie, op basis van gegevens voor telefoongesprek. Maar de techniek die laten we zien is ook geschikt voor andere soorten fraude te detecteren, zoals creditcard fraude of identiteitsdiefstal. 

## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Scenario: Telecommunicatie en SIM fraudedetectie in realtime

Een bedrijf telecommunicatie heeft een grote hoeveelheid gegevens naar binnenkomende oproepen. Het bedrijf wil voor het detecteren van frauduleus aanroepen in realtime, zodat ze kunnen klanten informeren of -service voor een bepaald aantal afgesloten. Een type SIM fraude omvat meerdere aanroepen uit dezelfde identiteit rond dezelfde tijd, maar in geografisch verschillende locaties. Om te detecteren dit soort fraude, moet het bedrijf binnenkomende phone records controleren en te zoeken naar specifieke patronen: in dit geval voor oproepen rond dezelfde tijd in andere landen. Telefoon records die in deze categorie vallen worden geschreven naar de opslag voor analyse.

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie hebt u telefonische oproep gegevens met behulp van een client-app die voorbeeld telefoongesprek metagegevens genereert simuleren. Aantal records die de app produceert eruitzien als frauduleus aanroepen. 

Voordat u begint, zorg er dan voor dat u hebt het volgende:

* Een Azure-account.
* De aanroep-gebeurtenis generator-app. U kunt dit ophalen door het downloaden van de [TelcoGenerator.zip bestand](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) uit het Microsoft Download Center. Pak het pakket naar een map op uw computer. Als u wilt de bron code en voer de app in een foutopsporingsprogramma wilt zien, kunt u de bron-code van de app downloaden [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator). 

    >[!NOTE]
    >Windows kan het gedownloade ZIP-bestand blokkeren. Als u deze kan niet uitpakken, met de rechtermuisknop op het bestand en selecteer **eigenschappen**. Als u het bericht 'dit bestand afkomstig zijn van een andere computer en mogelijk geblokkeerd om deze computer te beschermen' ziet, selecteert u de **blokkering** optie en klik vervolgens op **toepassen**.

Als u onderzoeken van de resultaten van de Streaming Analytics-taak wilt, moet u ook een hulpprogramma voor het weergeven van de inhoud van een Azure Blob Storage-container. Als u Visual Studio gebruikt, kunt u [Azure-Tools voor Visual Studio](https://docs.microsoft.com/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) of [Visual Studio Cloud Explorer](https://docs.microsoft.com/azure/vs-azure-tools-resources-managing-with-cloud-explorer). U kunt ook zelfstandige hulpprogramma's zoals installeren [Azure Opslagverkenner](http://storageexplorer.com/) of [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction). 

## <a name="create-an-azure-event-hubs-to-ingest-events"></a>Maken van een Azure event hubs om op te nemen van gebeurtenissen

Voor het analyseren van een gegevensstroom u *opnemen* in Azure. Een gangbare manier voor het opnemen van gegevens is met [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md), waarmee u miljoenen gebeurtenissen per seconde opnemen, en vervolgens verwerken en opslaan van gegevens van de gebeurtenis. Voor deze zelfstudie een event hub maken en vervolgens de aanroep-gebeurtenis-generator-app aanroepgegevens verzenden naar event hub hebt. Zie voor meer informatie over event hubs de [Azure Service Bus-documentatie](https://docs.microsoft.com/azure/service-bus/).

>[!NOTE]
>Zie voor een meer gedetailleerde versie van deze procedure [een Event Hubs-naamruimte en een event hub met de Azure portal maken](../event-hubs/event-hubs-create.md). 

### <a name="create-a-namespace-and-event-hub"></a>Een naamruimte en event hub maken
In deze procedure maakt u eerst een event hub-naamruimte maken en vervolgens voegt u toe een event hub naar die naamruimte. Event hub-naamruimten worden gebruikt voor het gerelateerde gebeurtenis bus exemplaren logisch groeperen. 

1. Aanmelden bij de Azure-portal en klikt u op **nieuw** > **Internet der dingen** > **Event Hub**. 

2. In de **naamruimte maken** deelvenster, voer de naam van een naamruimte zoals `<yourname>-eh-ns-demo`. U kunt een naam voor de naamruimte, maar de naam moet geldig zijn voor een URL en deze uniek zijn binnen Azure. 
    
3. Selecteer een abonnement maken of een resourcegroep kiezen, en klikt u op **maken**. 

    ![Een event hub-naamruimte maken](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-namespace-new-portal.png)
 
4. Wanneer de naamruimte is voltooid implementeren, moet u de event hub-naamruimte vinden in uw lijst met Azure-resources. 

5. Klik op de nieuwe naamruimte, en in het deelvenster naamruimte op **Event Hub**.

    ![De knop toevoegen Event Hub voor het maken van nieuwe event hub ](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-button-new-portal.png)    
 
6. Naam van de nieuwe event hub `sa-eh-frauddetection-demo`. U kunt een andere naam gebruiken. Als u dit doet, noteer, omdat u de naam van de later nodig. U hoeft niet alle andere opties voor de event hub nu instellen.

    ![Blade voor het maken van nieuwe event hub](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-new-portal.png)
    
 
7. Klik op **Create**.
### <a name="grant-access-to-the-event-hub-and-get-a-connection-string"></a>Toegang verlenen tot de event hub en een verbindingsreeks ophalen

Voordat een proces gegevens naar een event hub verzenden kan, moet een beleid waarmee u de juiste toegang hebben in de event hub. Het toegangsbeleid produceert een verbindingsreeks die autorisatie-informatie bevat.

1.  Klik in het deelvenster van de naamruimte gebeurtenis **Event Hubs** en klik vervolgens op de naam van uw nieuwe event hub.

2.  Klik in het deelvenster met event hub **gedeeld toegangsbeleid** en klik vervolgens op  **+ &nbsp;toevoegen**.

    >[!NOTE]
    >Zorg ervoor dat u werkt met de event hub, niet de event hub-naamruimte.

3.  Een beleid met de naam toevoegen `sa-policy-manage-demo` en voor **Claim**, selecteer **beheren**.

    ![Blade voor een nieuwe event hub-beleid maken](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-shared-access-policy-manage-new-portal.png)
 
4.  Klik op **Create**.

5.  Nadat het beleid is geïmplementeerd, klikt u op het in de lijst met beleidsregels voor gedeelde toegang.

6.  In het vak **CONNECTION STRING-primaire sleutel** en klik op de knop kopiëren naast de verbindingsreeks. 
    
    ![Kopiëren van de primaire verbinding string-sleutel van het toegangsbeleid](./media/stream-analytics-real-time-fraud-detection/stream-analytics-shared-access-policy-copy-connection-string-new-portal.png)
 
7.  De verbindingsreeks in een teksteditor plakken. U moet deze verbindingsreeks voor de volgende sectie, nadat u een aantal kleine wijzigingen aangebracht.

    De verbindingsreeks ziet er als volgt:

        Endpoint=sb://YOURNAME-eh-ns-demo.servicebus.windows.net/;SharedAccessKeyName=sa-policy-manage-demo;SharedAccessKey=Gw2NFZwU1Di+rxA2T+6hJYAtFExKRXaC2oSQa0ZsPkI=;EntityPath=sa-eh-frauddetection-demo

    U ziet dat de verbindingsreeks meerdere sleutel / waarde-paren bevat, gescheiden door puntkomma's: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, en `EntityPath`.  

## <a name="configure-and-start-the-event-generator-application"></a>Configureren en starten van de gebeurtenis generator-toepassing

Voordat u de app TelcoGenerator, configureren u deze zo dat deze aanroep records wordt verzonden naar de event hub die u hebt gemaakt.

### <a name="configure-the-telcogeneratorapp"></a>De TelcoGeneratorapp configureren

1.  In de editor waar u de verbindingsreeks hebt gekopieerd, noteert u de `EntityPath` waarde, en verwijder vervolgens de `EntityPath` paar (Vergeet niet om te verwijderen van de voorafgaande puntkomma). 

2.  In de map waar u het bestand TelcoGenerator.zip zijn uitgepakt, open het bestand telcodatagen.exe.config in een teksteditor. (Er is meer dan een .config-bestand, dus wees zeker dat u de juiste is geopend).

3.  In de `<appSettings>` element:

    * Stel de waarde van de `EventHubName` sleutel op de naam van de event hub (dat wil zeggen, met de waarde van de entiteit-pad).
    * Stel de waarde van de `Microsoft.ServiceBus.ConnectionString` sleutel op de verbindingsreeks. 

    De `<appSettings>` sectie eruit als in het volgende voorbeeld. (Voor de duidelijkheid we ingepakt de regels en bepaalde tekens verwijderd uit het verificatietoken.)

    ![App-configuratiebestand voor de TelcoGenerator weergegeven van de event hub en de verbindingsreeks](./media/stream-analytics-real-time-fraud-detection/stream-analytics-telcogenerator-config-file-app-settings.png)
 
4.  Sla het bestand op. 

### <a name="start-the-app"></a>De app te starten
1.  Open een opdrachtvenster en Ga naar de map waar de app TelcoGenerator uitgepakt is.
2.  Voer de volgende opdracht in:

        telcodatagen.exe 1000 .2 2

    De parameters zijn: 

    * Aantal CDR per uur. 
    * SIM-kaart fraude waarschijnlijkheid: Hoe vaak als een percentage van alle aanroepen, dat de app een aanroep van frauduleuze te simuleren. De waarde.2 betekent dat ongeveer 20% van de aanroep van records eruit frauduleuze.
    * De duur in uren. Het aantal uren dat de app moet worden uitgevoerd. U kunt ook de app elk gewenst moment stoppen door op Ctrl + C drukken op de opdrachtregel.

    Na enkele seconden, wordt de app gestart telefoongesprek records weergeven op het scherm, zoals het zendt deze naar de event hub.

Enkele van de sleutelvelden die u in deze toepassing realtime fraude-detectie gebruiken wilt zijn de volgende:

|**Record**|**Definitie**|
|----------|--------------|
|`CallrecTime`|Het tijdstempel voor de aanroep van de begintijd. |
|`SwitchNum`|De telefoon switch waarmee verbinding wordt gemaakt de aanroep. In dit voorbeeld zijn de switches tekenreeksen die staan voor het land van oorsprong (VS, China, UK, Duitsland of Australië). |
|`CallingNum`|Het telefoonnummer van de aanroepfunctie. |
|`CallingIMSI`|De International Mobile Subscriber Identity (IMSI). Dit is de unieke id van de aanroepfunctie. |
|`CalledNum`|Het telefoonnummer van de ontvanger aanroep. |
|`CalledIMSI`|International Mobile Subscriber Identity (IMSI). Dit is de unieke id van de ontvanger. |


## <a name="create-a-stream-analytics-job-to-manage-streaming-data"></a>Maken van een Stream Analytics-taak voor het beheren van streaminggegevens

Nu dat u een stream van de aanroep van gebeurtenissen hebt, kunt u een Stream Analytics-taak instellen. De taak leest de gegevens van de event hub die u hebt ingesteld. 

### <a name="create-the-job"></a>De taak maken 

1. Klik in de Azure-portal op **nieuw** > **Internet der dingen** > **Stream Analytics-taak**.

2. Naam van de taak `sa_frauddetection_job_demo`, een abonnement, resourcegroep en locatie opgeven.

    Het is een goed idee om de taak en de event hub plaatsen in dezelfde regio voor de beste prestaties en betaalt u niet aan het overbrengen van gegevens tussen regio's.

    ![Nieuwe Stream Analytics-taak maken](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-job-new-portal.png)

3. Klik op **Create**.

    De taak is gemaakt en de portal geeft taakdetails weer. Er is niets echter nog wordt uitgevoerd: u moet de taak configureren voordat deze kan worden gestart.

### <a name="configure-job-input"></a>Taak invoer configureren

1. In het dashboard of de **alle resources** deelvenster, zoek en selecteer de `sa_frauddetection_job_demo` Stream Analytics-taak. 
2. In de **taak topologie** sectie van het deelvenster Stream Analytics-taak klikt u op de **invoer** vak.

    ![Invoervak onder topologie in het deelvenster Streaming Analytics-taak](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-input-box-new-portal.png)
 
3. Klik op  **+ &nbsp;toevoegen** en vult u het deelvenster met deze waarden:

    * **Invoeralias**: Gebruik de naam `CallStream`. Als u een andere naam gebruikt, noteer het omdat u hebt deze later nodig.
    * **Gegevensbrontype**: Selecteer **gegevensstroom**. (**Referentiegegevens** verwijst naar statische lookup-gegevens, die u in deze zelfstudie niet gebruikt.)
    * **Bron**: Selecteer **Event hub**.
    * **Optie importeren**: Selecteer **gebruik event hub uit het huidige abonnement**. 
    * **Service bus-naamruimte**: Selecteer de event hub-naamruimte die u eerder hebt gemaakt (`<yourname>-eh-ns-demo`).
    * **Event hub**: Selecteer de event hub die u eerder hebt gemaakt (`sa-eh-frauddetection-demo`).
    * **Naam Event hub beleid**: Selecteer het beleid dat u eerder hebt gemaakt (`sa-policy-manage-demo`).

    ![Nieuwe invoer voor Streaming Analytics-taak maken](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-input-new-portal.png)

4. Klik op **Create**.

## <a name="create-queries-to-transform-real-time-data"></a>Query's voor het transformeren van real-time gegevens maken

U hebt op dit moment een Stream Analytics-taak lezen van een binnenkomende gegevensstroom instellen. De volgende stap is het maken van een transformatie waarin de gegevens in realtime worden geanalyseerd. U doen dit door een query te maken. Stream Analytics ondersteunt een eenvoudig, declaratief querymodel die transformaties voor realtime-verwerking beschrijft. De query's een SQL-achtige taal gebruiken die bepaalde extensies die specifiek zijn voor stream analytics heeft. 

Een eenvoudige query eventueel de binnenkomende gegevens alleen lezen. U maken vaak echter query's die eruit voor specifieke gegevens of relaties in de gegevens zien. In deze sectie van de zelfstudie, moet u maken en testen van meerdere query's voor meer informatie over een aantal manieren waarop u kunt een invoerstroom voor analyse te transformeren. 

De query's die u hier maakt, wordt alleen de getransformeerde gegevens naar het scherm weergeven. In een volgende sectie configureert u uitvoerlocatie en een query die de getransformeerde gegevens naar die sink schrijft.

Zie voor meer informatie over de taal, de [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/dn834998.aspx).

### <a name="get-sample-data-for-testing-queries"></a>Voorbeeldgegevens ophalen voor het testen van query 's

De app TelcoGenerator aanroep records verzendt naar de event hub en uw Stream Analytics-taak is geconfigureerd voor het lezen van de event hub. U kunt een query voor het testen van de taak om ervoor te zorgen dat deze correct wordt gelezen. Als u wilt testen een query in de Azure-console, moet u voorbeeldgegevens. Voor dit scenario moet u voorbeeldgegevens extraheren uit de stroom die afkomstig is in de event hub.

1. Zorg ervoor dat de app TelcoGenerator wordt uitgevoerd en aanroep records produceren.
2. Terug naar het deelvenster Streaming Analytics-taak in de portal. (Als u het deelvenster hebt gesloten, zoekt u naar `sa_frauddetection_job_demo` in de **alle resources** deelvenster.)
3. Klik op de **Query** vak. Azure geeft een lijst van de invoer en uitvoer die zijn geconfigureerd voor de taak en kunt u een query maken die u de invoerstroom Transformeren kunt zoals deze wordt verzonden naar de uitvoer.
4. In de **Query** deelvenster, klik op de puntjes naast de `CallStream` invoer- en selecteer vervolgens **voorbeeldgegevens uit invoer**.

    ![Menuopties met voorbeeldgegevens voor de vermelding van de Streaming Analytics-taak met 'Voorbeeldgegevens uit invoer' geselecteerd](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sample-data-from-input.png)

    Hiermee opent u een deelvenster waarmee u opgeven hoeveel voorbeeldgegevens ophalen kunt, gedefinieerd in termen van hoe lang de invoerstroom lezen.

5. Stel **minuten** 3 en klik vervolgens op **OK**. 
    
    ![Opties voor de steekproef van de invoerstroom met '3 minuten' geselecteerd.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-input-create-sample-data.png)

    Azure 3 minuten aan gegevens uit de invoerstroom voorbeelden en waarschuwt u als de voorbeeldgegevens gereed is. (Dit duurt even.) 

De voorbeeldgegevens worden tijdelijk opgeslagen en is beschikbaar wanneer u hebt de queryvenster geopend. Als u het query-venster sluit, wordt de voorbeeldgegevens worden genegeerd en hebt u een nieuwe set met voorbeeldgegevens. 

Als alternatief kunt u een .json-bestand met voorbeeldgegevens erin krijgen [vanuit GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json), en vervolgens uploaden dat .json-bestand te gebruiken als u voorbeeldgegevens voor de `CallStream` invoer. 

### <a name="test-using-a-pass-through-query"></a>Testen met behulp van een Pass Through-query

Als u archiveren van elke gebeurtenis wilt, kunt u een Pass Through-query om te lezen van de velden in de nettolading van de gebeurtenis.

1. Voer deze query in het queryvenster:

        SELECT 
            *
        FROM 
            CallStream

    >[!NOTE]
    >Net als bij SQL, trefwoorden zijn niet hoofdlettergevoelig en maakt niet uit witruimte.

    In deze query `CallStream` is de alias die u hebt opgegeven toen u de invoer hebt gemaakt. Als u een andere alias gebruikt, gebruikt u de naam.

2. Klik op **Test**.

    De Stream Analytics-taak wordt de query uitgevoerd op de voorbeeldgegevens en de uitvoer wordt weergegeven aan de onderkant van het venster. Dit geeft aan of de event hub en de Streaming Analytics-taak juist zijn geconfigureerd. (Zoals is aangegeven, later maakt u uitvoerlocatie dat de query kan gegevens te schrijven.)

    ![Stream Analytics-taak van uitvoer met 73 records die zijn gegenereerd](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output.png)

    Het exacte aantal records dat er afhankelijk van hoeveel records zijn opgenomen in uw voorbeeld 3 minuten.
 
### <a name="reduce-the-number-of-fields-using-a-column-projection"></a>Verminder het aantal velden met behulp van een kolom-projectie

In veel gevallen hoeft uw analyse niet alle kolommen uit de invoerstroom. U kunt een query gebruiken om een kleiner aantal geretourneerde velden dan in de Pass Through-query te projecteren.

1. De query in de code-editor wijzigen in het volgende:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum 
        FROM 
            CallStream

2. Klik op **Test** opnieuw. 

    ![Stream Analytics-taak uitvoer geproduceerd voor projectie, met 25 records die zijn gegenereerd](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-projection.png)
 
### <a name="count-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Aantal binnenkomende aanroepen per regio: tumblingvenster met aggregatie

Stel dat u wilt het aantal inkomende aanroepen per regio. Wanneer u uitvoeren van statistische functies wilt zoals telling, in het streamen van gegevens, moet u de stroom segmenteren in tijdelijke eenheden (omdat de gegevensstroom zelf effectief eindeloze is). Hiervoor gebruikt een Streaming Analytics [venster functie](stream-analytics-window-functions.md). Vervolgens kunt u werken met de gegevens in dit venster als eenheid.

Voor deze transformatie die u wilt een reeks van tijdelijke windows die elkaar niet overlappen, elk venster heeft een aparte set gegevens die u kunt groeperen en statistische functie. Dit type venster wordt aangeduid als een *tumblingvenster* . Binnen de tumblingvenster krijgt u een aantal van de binnenkomende oproepen gegroepeerd op `SwitchNum`, die staat voor het land waar de oproep afkomstig is. 

1. De query in de code-editor wijzigen in het volgende:

        SELECT 
            System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount 
        FROM
            CallStream TIMESTAMP BY CallRecTime 
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Deze query gebruikt de `Timestamp By` -sleutelwoord in de `FROM` component om op te geven welk tijdstempelveld in de invoerstroom moet worden gebruikt voor het definiëren van de tumblingvenster. In dit geval de gegevens in het venster wordt verdeeld in segmenten door de `CallRecTime` veld in elke record. (Als er is geen veld is opgegeven, wordt de windowing-bewerking wordt de tijd dat elke gebeurtenis op de event hub binnenkomt. Zie 'Tijd tegenover toepassing aankomsttijd' in [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx). 

    De projectie omvat `System.Timestamp`, die een tijdstempel voor het einde van elke venster retourneert. 

    Als u wilt opgeven dat u wilt gebruiken een tumblingvenster, gebruikt u de [TUMBLINGWINDOW](https://msdn.microsoft.com/library/dn835055.aspx) werken in de `GROUP BY `component. In de functie geeft u een tijdseenheid (van een microseconden naar een dag) en een venstergrootte (hoeveel units). In dit voorbeeld wordt bestaat de tumblingvenster uit 5 seconden intervallen, zodat u een telling per land voor elke vijf seconden aan aanroepen krijgt.

2. Klik op **Test** opnieuw. U ziet dat in de resultaten van de tijdstempels onder **WindowEnd** in stappen van 5 seconden zijn.

    ![Stream Analytics-taak uitvoer geproduceerd voor aggregatie, met 13 records die zijn gegenereerd](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-aggregation.png)
 
### <a name="detect-sim-fraud-using-a-self-join"></a>Met behulp van een self-join SIM-fraude te detecteren

In dit voorbeeld kunt we frauduleuze gebruik aanroepen die afkomstig van dezelfde gebruiker, maar in verschillende locaties binnen 5 seconden van elkaar zijn worden overwegen. Bijvoorbeeld, kan niet dezelfde gebruiker daadwerkelijk moet een aanroep van de Verenigde Staten en Australië op hetzelfde moment. 

Om te controleren of deze gevallen, kunt u een self-join van de streaminggegevens toevoegen aan de stroom op basis van de `CallRecTime` waarde. U kunt vervolgens zoeken aanroep registreert waar de `CallingIMSI` waarde (het oorspronkelijke aantal) is hetzelfde, maar de `SwitchNum` waarde (land van oorsprong) is niet hetzelfde.

Wanneer u een join gebruikt met het streamen van gegevens, moet de join bieden dat een aantal beperkingen met betrekking tot hoe ver overeenkomende rijen in de tijd kunnen worden gescheiden. (Zoals eerder opgemerkt, de streaminggegevens is effectief eindeloze.) De grenzen van de tijd voor de relatie zijn opgegeven in de `ON` -component van de join, met behulp van de `DATEDIFF` functie. In dit geval is de join gebaseerd op een interval van 5 seconden van de van aanroepgegevens.

1. De query in de code-editor wijzigen in het volgende: 

        SELECT  System.Timestamp as Time, 
            CS1.CallingIMSI, 
            CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, 
            CS1.SwitchNum as Switch1, 
            CS2.SwitchNum as Switch2 
        FROM CallStream CS1 TIMESTAMP BY CallRecTime 
            JOIN CallStream CS2 TIMESTAMP BY CallRecTime 
            ON CS1.CallingIMSI = CS2.CallingIMSI 
            AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5 
        WHERE CS1.SwitchNum != CS2.SwitchNum

    Deze query is vergelijkbaar met een SQL-join behalve voor de `DATEDIFF` functie in de join. Dit is een versie van `DATEDIFF` die specifiek zijn voor de Streaming Analytics, en deze moet worden weergegeven de `ON...BETWEEN` component. De parameters zijn een tijdseenheid (seconden in dit voorbeeld) en de aliassen van de twee bronnen voor de join. (Dit verschilt van de standaard SQL `DATEDIFF` functie.) 

    De `WHERE` -component bevat de conditie van de aanroep van frauduleuze vlaggen: de oorspronkelijke switches komen niet overeen. 

2. Klik op **Test** opnieuw. 

    ![Stream Analytics-taak uitvoer geproduceerd voor self-join, zichtbaar 6 records gegenereerd](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-self-join.png)

3. Klik op **Opslaan**. Dit bespaart de zelf join-query als onderdeel van de Streaming Analytics-taak. (Deze niet de voorbeeldgegevens opslaan.)

    ![Opslaan van Stream Analytics-taak](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-save-button-new-portal.png)

## <a name="create-an-output-sink-to-store-transformed-data"></a>Uitvoerlocatie voor het opslaan van de getransformeerde gegevens maken

Een stroom gebeurtenissen, een event hub als invoer gebruikt voor het opnemen van gebeurtenissen en een query uit te voeren van een transformatie via de stroom die u hebt gedefinieerd. De laatste stap is het definiëren van de uitvoerlocatie voor de taak, dat wil zeggen, een plaats de getransformeerde stroom te schrijven. 

U kunt veel resources gebruiken als uitvoer put: een SQL Server-database, -tabelopslag Data Lake storage, Power BI en zelfs een ander event hub. Voor deze zelfstudie hebt u de stroom schrijven naar Azure Blob Storage, een typische keuze is voor het verzamelen van gebeurtenisgegevens voor latere analyse, omdat het is geschikt voor niet-gestructureerde gegevens.

Als u een bestaande blob storage-account hebt, kunt u die kunt gebruiken. Voor deze zelfstudie leert u hoe u een nieuw opslagaccount voor deze zelfstudie maakt.

### <a name="create-an-azure-blob-storage-account"></a>Een Azure Blob Storage-account maken

1. Terug naar het deelvenster Streaming Analytics-taak in de Azure portal. (Als u het deelvenster hebt gesloten, zoekt u naar `sa_frauddetection_job_demo` in de **alle resources** deelvenster.)
2. In de **taak topologie** sectie, klikt u op de **uitvoer** vak. 
3. In de **uitvoer** deelvenster, klikt u op  **+ &nbsp;toevoegen** en vult u het deelvenster met deze waarden:

    * **Uitvoeraliassen**: Gebruik de naam `CallStream-FraudulentCalls`. 
    * **Sink**: Selecteer **Blob storage**.
    * **Opties voor het importeren**: Selecteer **gebruik blob storage uit het huidige abonnement**.
    * **Storage-account**. Selecteer **nieuw opslagaccount maken.**
    * **Storage-account** (tweede vak). Voer `YOURNAMEsademo`, waarbij `YOURNAME` uw naam of een andere unieke tekenreeks is. De naam kan alleen kleine letters en cijfers gebruiken en deze uniek zijn binnen Azure. 
    * **Container**. Voer `sa-fraudulentcalls-demo`.
    De naam van het opslagaccount en de containernaam worden samen gebruikt voor een URI voor de blobopslag, als volgt: 

    `http://yournamesademo.blob.core.windows.net/sa-fraudulentcalls-demo/...`
    
    !['Nieuw uitvoer' deelvenster voor Stream Analytics-taak](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-output-blob-storage-new-console.png)
    
4. Klik op **Create**. 

    Azure storage-account maakt en een sleutel automatisch wordt gegenereerd. 

5. Sluit de **uitvoer** deelvenster. 

## <a name="start-the-streaming-analytics-job"></a>Start de Streaming Analytics-taak

De taak is nu geconfigureerd. U hebt opgegeven invoer (de event hub), een transformatie (de query voor het zoeken naar frauduleuze aanroepen) en uitvoer (blobopslag). U kunt nu de taak starten. 

1. Zorg ervoor dat de app TelcoGenerator wordt uitgevoerd.

2. Klik in het taakdeelvenster op **Start**.

    ![De Stream Analytics-taak starten](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start-output.png)

3. In de **starttaak** venster voor de taak uitvoer begintijd, selecteer **nu**. 

4. Klik op **Start**. 

    ![Deelvenster 'Start de taak' voor de Stream Analytics-taak](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start-job-blade.png)

    Azure waarschuwt u als de taak is gestart en in het taakdeelvenster de status wordt weergegeven als **met**.

    ![Stream Analytics-taakstatus 'Actief' wordt weergegeven](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-running-status.png)
    

## <a name="examine-the-transformed-data"></a>De getransformeerde gegevens controleren

U hebt nu een volledige Streaming Analytics-taak. De taak is vindt u in een stream van telefoongesprek metagegevens van frauduleuze telefoongesprekken in realtime zoekt en informatie over deze frauduleuze aanroepen naar opslag schrijven. 

Deze zelfstudie is het raadzaam om te kijken naar de gegevens die door de Streaming Analytics-taak wordt vastgelegd. De gegevens wordt geschreven naar Azure Blog van Storage in segmenten (bestanden). U kunt elk hulpprogramma dat Azure Blob Storage gebruiken. Zoals beschreven in de sectie vereisten, kunt u Azure-extensies in Visual Studio of kunt u een hulpprogramma zoals [Azure Opslagverkenner](http://storageexplorer.com/) of [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction). 

Wanneer u de inhoud van een bestand in blob storage bekijkt, ziet u ongeveer als volgt:

![Azure blob storage met Streaming Analytics-uitvoer](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-blob-storage-view.png)
 

## <a name="clean-up-resources"></a>Resources opschonen

We hebben meer artikelen die doorgaan met het detecteren van fraude scenario en die gebruikmaken van de resources die u in deze zelfstudie hebt gemaakt. Als u doorgaan wilt, raadpleegt u de suggesties onder **Vervolgstappen** later.

Als u bent klaar en u hoeft niet de resources die u hebt gemaakt, kunt u echter verwijderen ze zodat u geen onnodige Azure kosten. In dat geval is het raadzaam dat u het volgende doen:

1. Stop de Streaming Analytics-taak. In de **taken** deelvenster, klikt u op **stoppen** aan de bovenkant.
2. Stop de Telco Generator-app. In het opdrachtvenster waar u de app gestart, drukt u op Ctrl + C.
3. Als u een blob storage-account voor deze zelfstudie hebt gemaakt, verwijderen. 
4. Verwijder de Streaming Analytics-taak.
5. Verwijder de event hub.
6. Verwijder de event hub-naamruimte.

## <a name="get-support"></a>Ondersteuning krijgen

Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie met het volgende artikel kan worden voortgezet:

* [Stream Analytics en Power BI: een realtime analytics-dashboard voor het streamen van gegevens](stream-analytics-power-bi-dashboard.md). In dit artikel leest u hoe de TelCo uitvoer van de Stream Analytics-taak verzenden naar Power BI voor realtime visualisatie en analyse.

Zie voor meer informatie over Stream Analytics in het algemeen deze artikelen:

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
