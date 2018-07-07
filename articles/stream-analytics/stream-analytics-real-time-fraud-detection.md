---
title: Fraudebewaking in realtime met Azure Stream Analytics
description: Informatie over het maken van een oplossing voor het detecteren van realtime fraude met Stream Analytics. Gebruik een event hub voor gebeurtenissen in realtime verwerken.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: e0d430ced1dbddbfca79806591c83c33e732eefd
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901711"
---
# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Aan de slag met Azure Stream Analytics: fraudebewaking in realtime

Deze zelfstudie biedt een end-to-end-illustratie van het gebruik van Azure Stream Analytics. In deze zelfstudie leert u procedures om het volgende te doen: 

* Breng het streamen van gebeurtenissen naar een exemplaar van Azure Event Hubs. In deze zelfstudie gebruikt u een app die een stream van records met metagegevens van de mobiele telefoon simuleert.

* SQL-achtige Stream Analytics-query's voor het transformeren van gegevens, het samenvoegen van gegevens of op zoek naar patronen schrijven. Hier ziet u hoe u een query voor de stroom inkomende onderzoeken en zoekt u aanroepen die mogelijk frauduleuze.

* De resultaten verzenden naar een uitvoer-sink (opslag) die u meer inzicht te verkrijgen kunt analyseren. In dit geval ontvangt u de aanroepgegevens van verdachte op Azure Blob-opslag.

In deze zelfstudie wordt het voorbeeld van op basis van gegevens van telefoongesprekken fraudebewaking in realtime. De techniek geïllustreerd is ook geschikt voor andere typen fraudedetectie, zoals diefstal van fraude of creditcard is geregistreerd. 

## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Scenario: Telecommunicatie en SIM fraudebewaking in realtime

Een telecombedrijf heeft een groot volume aan gegevens voor binnenkomende oproepen. Het bedrijf wil dat voor het detecteren van frauduleuze gesprekken in realtime, zodat ze kunnen klanten informeren of -service voor een specifiek getal afsluiten. Een type SIM fraude omvat meerdere aanroepen van de dezelfde identiteit rond dezelfde tijd, maar in geografisch verschillende locaties. Voor het detecteren van dit type fraude, het bedrijf kunt binnenkomende phone records bekijken en zoeken naar specifieke patronen: in dit geval voor aanroepen die rond dezelfde tijd worden gedaan in andere landen. Telefoon records die kunnen worden onderverdeeld in deze categorie worden geschreven naar de opslag voor analyse.

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie hebt u gegevens van telefoongesprekken simuleren met behulp van een client-app die voorbeeld telefonische oproep metagegevens genereert. Aantal records dat de app produceert frauduleuze gesprekken eruit. 

Zorg ervoor dat u het volgende hebt voordat u begint:

* Een Azure-account.
* De gebeurtenis gesprek generator-app, [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip), die kan worden gedownload vanaf het Microsoft Download Center. Pak dit pakket naar een map op uw computer. Als u zien van de bron wilt-code en de app uitvoeren in een debugger, krijgt u de broncode van de app uit de [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator). 

    >[!NOTE]
    >Windows blokkeren mogelijk het gedownloade ZIP-bestand. Als u deze kan niet uitpakken, met de rechtermuisknop op het bestand en selecteert u **eigenschappen**. Als u het bericht 'dit bestand afkomstig is van een andere computer en ter bescherming van deze computer kan worden geblokkeerd' ziet, selecteert u de **opheffen van blokkeringen** optie en klik vervolgens op **toepassen**.

Als u controleren van de resultaten van de Streaming Analytics-taak wilt, moet u ook een hulpprogramma voor het weergeven van de inhoud van een Azure Blob Storage-container. Als u Visual Studio gebruikt, kunt u [Azure Tools voor Visual Studio](https://docs.microsoft.com/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) of [Visual Studio Cloud Explorer](https://docs.microsoft.com/azure/vs-azure-tools-resources-managing-with-cloud-explorer). U kunt ook zelfstandige hulpprogramma's zoals installeren [Azure Storage Explorer](http://storageexplorer.com/) of [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction). 

## <a name="create-an-azure-event-hubs-to-ingest-events"></a>Maken van een Azure Event Hubs voor opname van gebeurtenissen

Voor het analyseren van een gegevensstroom u *opnemen* in Azure. Een gebruikelijke manier voor opname van gegevens is het gebruik [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md), waarmee u miljoenen gebeurtenissen per seconde opnemen en vervolgens verwerken en opslaan van gegevens van de gebeurtenis. Voor deze zelfstudie maakt u een event hub maken en vervolgens de gebeurtenis gesprek-generator-app verzenden naar die event hub hebt. Zie voor meer informatie over eventhubs, de [documentatie voor Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

>[!NOTE]
>Zie voor een meer gedetailleerde-versie van deze procedure [maken van een Event Hubs-naamruimte en een event hub met behulp van de Azure-portal](../event-hubs/event-hubs-create.md). 

### <a name="create-a-namespace-and-event-hub"></a>Een naamruimte en event hub maken
In deze procedure maakt u eerst een event hub-naamruimte maken en vervolgens het toevoegen van een event hub aan die naamruimte. Event hub-naamruimten worden gebruikt voor het groeperen van gerelateerde gebeurtenissen bus exemplaren. 

1. Meld u aan bij de Azure-portal en klikt u op **een resource maken** > **Internet of Things** > **Event Hub**. 

2. In de **-naamruimte maken** deelvenster, voer de naam van een naamruimte zoals `<yourname>-eh-ns-demo`. U kunt een willekeurige naam voor de naamruimte, maar de naam mag niet geldig voor een URL en deze moet uniek zijn binnen Azure. 
    
3. Selecteer een abonnement en maak of kies een resourcegroep en klik vervolgens op **maken**.

    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-namespace-new-portal.png" alt="drawing" width="300px"/>

4. De naamruimte is geïmplementeerd, vindt de event hub-naamruimte in de lijst met Azure-resources. 

5. Klik op de nieuwe naamruimte, en klik in het deelvenster naamruimte **Event Hub**.

   ![De knop Event Hub toevoegen voor het maken van een nieuwe event hub ](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-button-new-portal.png)    
 
6. Naam van de nieuwe event hub `asa-eh-frauddetection-demo`. U kunt ook een andere naam gebruiken. Als u dit doet, noteer, omdat u de naam van de later nodig. U hoeft niet te eventuele andere opties voor de event hub nu instellen.

    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-new-portal.png" alt="drawing" width="400px"/>
    
 
7. Klik op **Create**.

### <a name="grant-access-to-the-event-hub-and-get-a-connection-string"></a>Toegang verlenen tot de event hub en een verbindingsreeks ophalen

Voordat een proces u gegevens naar een event hub verzenden kunt, moet de event hub een beleid waarmee u de juiste toegang hebben. Het toegangsbeleid genereert een verbindingsreeks die autorisatiegegevens bevat.

1.  Klik in het deelvenster van de naamruimte gebeurtenis **Event Hubs** en klik vervolgens op de naam van uw nieuwe event hub.

2.  Klik in het deelvenster event hub, **beleid voor gedeelde toegang** en klik vervolgens op  **+ &nbsp;toevoegen**.

    >[!NOTE]
    >Zorg ervoor dat u werkt met de event hub, niet de event hub-naamruimte.

3.  Toevoegen van een beleid met de naam `sa-policy-manage-demo` en voor **Claim**, selecteer **beheren**.

    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-shared-access-policy-manage-new-portal.png" alt="drawing" width="300px"/>
 
4.  Klik op **Create**.

5.  Nadat het beleid is geïmplementeerd, klikt u erop in de lijst met beleid voor gedeelde toegang.

6.  In het vak **VERBINDINGSREEKS-primaire sleutel** en klikt u op de knop kopiëren naast de verbindingsreeks. 

    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-shared-access-policy-copy-connection-string-new-portal.png" alt="drawing" width="300px"/>
 
7.  Plak de verbindingsreeks in een teksteditor. U hebt deze verbindingsreeks nodig voor de volgende sectie, nadat u enkele kleine wijzigingen in het aanbrengt.

    De verbindingsreeks ziet er als volgt:

        Endpoint=sb://YOURNAME-eh-ns-demo.servicebus.windows.net/;SharedAccessKeyName=asa-policy-manage-demo;SharedAccessKey=Gw2NFZwU1Di+rxA2T+6hJYAtFExKRXaC2oSQa0ZsPkI=;EntityPath=asa-eh-frauddetection-demo

    U ziet dat de verbindingsreeks meerdere sleutel / waarde-paren, gescheiden door puntkomma's bevat: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, en `EntityPath`.  

## <a name="configure-and-start-the-event-generator-application"></a>Configureren en de gebeurtenisgenerator te starten

Voordat u de app TelcoGenerator start, moet u deze configureren zodat gespreksrecords wordt verzonden naar de event hub die u hebt gemaakt.

### <a name="configure-the-telcogenerator-app"></a>De app TelcoGenerator configureren

1.  In de editor waarin u de verbindingsreeks hebt gekopieerd, maak een notitie van de `EntityPath` waarde, en verwijder vervolgens de `EntityPath` paar (Vergeet niet om te verwijderen van de puntkomma). 

2.  In de map waar u het bestand TelcoGenerator.zip zijn uitgepakt, open het bestand telcodatagen.exe.config in een teksteditor. (Er is meer dan één .config-bestand, dus zorg ervoor dat u opent u de juiste is.)

3.  In de `<appSettings>` element:

    * Stel de waarde van de `EventHubName` sleutel op de naam van de event hub (dat wil zeggen, de waarde van het pad van de entiteit).
    * Stel de waarde van de `Microsoft.ServiceBus.ConnectionString` sleutel op de verbindingstekenreeks. 

    De `<appSettings>` sectie eruit als in het volgende voorbeeld. (Voor alle duidelijkheid worden de regels worden verpakt en bepaalde tekens zijn verwijderd uit het verificatietoken.)

   ![De event hub-naam en de verbindingsreeks met app-configuratiebestand voor TelcoGenerator](./media/stream-analytics-real-time-fraud-detection/stream-analytics-telcogenerator-config-file-app-settings.png)
 
4.  Sla het bestand op. 

### <a name="start-the-app"></a>De app te starten
1.  Open een opdrachtvenster en wijzig in de map waarin de app TelcoGenerator uitgepakt is.
2.  Voer de volgende opdracht in:

        telcodatagen.exe 1000 0.2 2

    De parameters zijn: 

    * Aantal CDR per uur. 
    * SIM-kaart fraude kans: Hoe vaak als een percentage van alle aanroepen, dat de app een frauduleus gesprek moet simuleren. De waarde 0.2 betekent dat ongeveer 20% van de gespreksrecords er frauduleus uitziet.
    * Duur in uren. Het aantal uren dat de app moet worden uitgevoerd. U kunt ook de app elk gewenst moment stoppen door op Ctrl + C te drukken op de opdrachtregel.

    Na enkele seconden begint de app met het weergeven van telefoongesprekrecords op het scherm wanneer deze naar de event hub worden gestuurd.

Enkele van de velden voor sleutels die u in deze realtime fraude detectie van toepassing gebruiken wilt zijn als volgt:

|**Record**|**Definitie**|
|----------|--------------|
|`CallrecTime`|Het tijdstempel voor de begintijd van de oproep. |
|`SwitchNum`|Het schakelnummer van de oproep. In dit voorbeeld zijn de schakelnummers tekenreeksen die staan voor het land van herkomst (VS, China, UK, Duitsland of Australië). |
|`CallingNum`|Het telefoonnummer van de beller. |
|`CallingIMSI`|De International Mobile Subscriber Identity (IMSI). Dit is de unieke id van de oproepende functie. |
|`CalledNum`|Het telefoonnummer van de ontvanger. |
|`CalledIMSI`|De International Mobile Subscriber Identity (IMSI). Dit is de unieke id van de ontvanger. |


## <a name="create-a-stream-analytics-job-to-manage-streaming-data"></a>Een Stream Analytics-taak voor het beheren van streaminggegevens maken

Nu dat u een stream van gesprekgebeurtenissen hebt, kunt u een Stream Analytics-taak instellen. De taak leest gegevens uit de event hub die u hebt ingesteld. 

### <a name="create-the-job"></a>De taak maken 

1. Klik in de Azure-portal op **een resource maken** > **Internet of Things** > **Stream Analytics-taak**.

2. Naam van de taak `asa_frauddetection_job_demo`, een abonnement, resourcegroep en locatie opgeven.

    Is het een goed idee om de taak en de event hub in dezelfde regio voor de beste prestaties en zodat u niet betaalt voor het overbrengen van gegevens tussen regio's.

    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-job-new-portal.png" alt="drawing" width="300px"/>

3. Klik op **Create**.

    De taak is gemaakt en de portal geeft taakdetails weer. Niets echter nog wordt uitgevoerd: u moet de taak configureren voordat het kan worden gestart.

### <a name="configure-job-input"></a>Taakinvoer configureren

1. In het dashboard of de **alle resources** deelvenster, zoek en selecteer de `asa_frauddetection_job_demo` Stream Analytics-taak. 
2. In de **overzicht** sectie van de Stream Analytics-taakdeelvenster, klikt u op de **invoer** vak.

   ![Invoervak onder topologie in het deelvenster voor stream Analytics-taak](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-input-box-new-portal.png)
 
3. Klik op **Stroominvoer toevoegen** en selecteer **Event Hub**. Vul vervolgens de nieuwe pagina met invoer met de volgende informatie:

   |**Instelling**  |**Voorgestelde waarde**  |**Beschrijving**  |
   |---------|---------|---------|
   |Invoeralias  |  CallStream   |  Voer een unieke naam in voor de invoer van de taak.   |
   |Abonnement   |  \<Uw abonnement\> |  Selecteer het Azure-abonnement waaraan de Event Hub die u hebt gemaakt.   |
   |Event Hub-naamruimte  |  asa-eh-ns-demo |  Voer de naam van de Event Hub-naamruimte.   |
   |Event Hub-naam  | asa-eh-frauddetection-demo | Selecteer de naam van uw Event Hub.   |
   |Naam van het Event Hub-beleid  | asa-beleid-beheren-demo | Selecteer het beleid dat u eerder hebt gemaakt.   |
    </br>
    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-input-new-portal.png" alt="drawing" width="300px"/>


4. Klik op **Create**.

## <a name="create-queries-to-transform-real-time-data"></a>Maken van query's om realtime gegevens te transformeren

Op dit moment hebt u een Stream Analytics-taak die is ingesteld om te lezen van een binnenkomende gegevensstroom. De volgende stap is het maken van een query die de gegevens in realtime geanalyseerd. Stream Analytics ondersteunt een eenvoudig, declaratief querymodel waarin wordt beschreven transformaties voor verwerking in realtime. De query's gebruikt een SQL-achtige taal waarvoor bepaalde extensies die specifiek zijn voor Stream Analytics. 

Een eenvoudige query kan alleen de binnenkomende gegevens lezen. U maken vaak echter query's die voor specifieke gegevens of voor relaties in de gegevens. In deze sectie van de zelfstudie kunt u maken en testen van verschillende query's uit om informatie over een aantal manieren waarop u een invoerstroom voor analyse kunt transformeren. 

De query's die u hier maakt, wordt alleen de getransformeerde gegevens naar het scherm weergegeven. In een volgende sectie configureert u een uitvoer-sink- en een query die de getransformeerde gegevens naar die sink schrijft.

Zie voor meer informatie over de taal, de [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/dn834998.aspx).

### <a name="get-sample-data-for-testing-queries"></a>Van voorbeeldgegevens ophalen voor het testen van query 's

De app TelcoGenerator gespreksrecords verzendt naar de event hub en uw Stream Analytics-taak is geconfigureerd voor het lezen van de event hub. U kunt een query gebruiken voor het testen van de taak om ervoor te zorgen dat deze correct wordt gelezen. Als u wilt testen een query in de Azure-console, moet u voorbeeldgegevens. Voor dit scenario extraheert u voorbeeldgegevens uit de stroom die in de event hub binnenkomt.

1. Zorg ervoor dat de app TelcoGenerator wordt uitgevoerd en gespreksrecords produceren.
2. Terug naar het deelvenster voor stream Analytics-taak in de portal. (Als u het deelvenster hebt gesloten, zoekt u `asa_frauddetection_job_demo` in de **alle resources** deelvenster.)
3. Klik op de **Query** vak. Azure geeft een lijst van de invoer en uitvoer die zijn geconfigureerd voor de taak en kunt u een query waarmee u de invoerstroom transformeren zoals deze wordt verzonden naar de uitvoer kunt maken.
4. In de **Query** deelvenster, klikt u op de puntjes naast de `CallStream` invoer en selecteer vervolgens **voorbeeldgegevens van uitvoer**.

   ![Menuopties voor het gebruik van voorbeeldgegevens voor de vermelding van de stream Analytics-taak met 'voorbeeld van gegevens uit de invoer ' geselecteerd](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sample-data-from-input.png)


5. Stel **minuten** op 3 en klik vervolgens op **OK**. 
    
   ![Opties voor de steekproef van de invoerstroom met "3 minuten" geselecteerd.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-input-create-sample-data.png)

    Azure-voorbeelden voor drie minuten aan gegevens uit de invoerstroom en ontvangt u een melding wanneer de voorbeeldgegevens gereed is. (Dit duurt enkele ogenblikken). 

De voorbeeldgegevens worden tijdelijk opgeslagen en zijn beschikbaar zolang u het queryvenster geopend houdt. Als u het queryvenster sluit, worden de voorbeeldgegevens verwijderd en zult u een nieuwe set voorbeeldgegevens moeten maken. 

Als alternatief, krijgt u een .json-bestand met voorbeeldgegevens in het [vanuit GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json), en vervolgens uploaden dat .json-bestand om te gebruiken als voorbeeldgegevens voor de `CallStream` invoer. 

### <a name="test-using-a-pass-through-query"></a>Test met behulp van een Pass Through-query

Als u archiveren van elke gebeurtenis wilt, kunt u een Pass Through-query gebruiken om te lezen van alle velden in de nettolading van de gebeurtenis.

1. Voer deze query in het queryvenster:

        SELECT 
            *
        FROM 
            CallStream

    >[!NOTE]
    >Net als bij SQL, trefwoorden zijn niet hoofdlettergevoelig en witruimte is niet significant.

    In deze query `CallStream` is de alias die u hebt opgegeven tijdens het maken van de invoer. Als u een andere alias gebruikt, in plaats daarvan deze naam gebruiken.

2. Klik op **Test**.

    De Stream Analytics-taak wordt de query uitgevoerd op de voorbeeldgegevens en de uitvoer wordt weergegeven aan de onderkant van het venster. De resultaten blijkt dat de Event Hub en de stream Analytics-taak correct zijn geconfigureerd. (Zoals aangegeven, later maakt u een uitvoer-sink dat de query kan de gegevens te schrijven.)

   ![Stream Analytics-taakuitvoer, 73 records die zijn gegenereerd worden weergegeven](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output.png)

    Het exacte aantal records dat er afhankelijk van hoeveel records zijn vastgelegd in het voorbeeld 3 minuten.
 
### <a name="reduce-the-number-of-fields-using-a-column-projection"></a>Verminder het aantal velden met behulp van een kolom-projectie

In veel gevallen hoeft uw analyse niet alle kolommen uit de invoerstroom. U kunt een query gebruiken om een kleiner aantal geretourneerde velden dan in de Pass Through-query.

1. Wijzig de query in de code-editor in het volgende:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum 
        FROM 
            CallStream

2. Klik op **Test** opnieuw. 

   ![Stream Analytics job output voor projectie met 25 records die zijn gegenereerd](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-projection.png)
 
### <a name="count-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Aantal inkomende aanroepen per regio: tumblingvenster met aggregatie op

Stel dat u wilt het aantal inkomende aanroepen per regio. Als u uitvoeren van statistische functies wilt, zoals tellen, in de streaming-gegevens, moet u de stroom segmenteren in tijdelijke eenheden (omdat de gegevensstroom zelf is van een effectief eindeloze). U doet dit met behulp van een stream Analytics [vensterfunctie](stream-analytics-window-functions.md). Vervolgens kunt u werken met de gegevens in dit venster als een eenheid.

Voor deze transformatie die u wilt een opeenvolging van tijdelijke windows die elkaar niet overlappen, elk venster heeft een afzonderlijke set met gegevens die u kunt groeperen en statistische functie. Dit type venster wordt aangeduid als een *tumblingvenster*. Binnen de tumblingvenster, krijgt u een aantal van de binnenkomende oproepen gegroepeerd op `SwitchNum`, die staat voor het land waarvan de oproep afkomstig is. 

1. Wijzig de query in de code-editor in het volgende:

        SELECT 
            System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount 
        FROM
            CallStream TIMESTAMP BY CallRecTime 
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Deze query gebruikt de `Timestamp By` sleutelwoord in de `FROM` component om op te geven welke tijdstempelveld in de invoerstroom te gebruiken voor het definiëren van de tumblingvenster. In dit geval de gegevens in het venster wordt opgedeeld in segmenten van de `CallRecTime` veld zijn in elke record. (Als er geen veld is opgegeven, wordt de bewerking windowing wordt het tijdstip waarop elke gebeurtenis wordt ontvangen op de event hub. Zie 'Aankomst tijd Vs toepassing Time' in [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx). 

    De projectie bevat `System.Timestamp`, die een tijdstempel voor het einde van elke venster geeft als resultaat. 

    Als u wilt opgeven dat u wilt een tumblingvenster gebruiken, gebruikt u de [TUMBLINGWINDOW](https://msdn.microsoft.com/library/dn835055.aspx) werken in de `GROUP BY `component. In de functie geeft u een tijdseenheid (een willekeurige plaats in een microseconde op een dag) en een venstergrootte (hoeveel eenheden). In dit voorbeeld bestaat de tumblingvenster van 5 seconden intervallen, zodat u een telling per land voor elke 5 seconden aan aanroepen krijgt.

2. Klik op **Test** opnieuw. In de resultaten ziet u dat de tijdstempels onder **WindowEnd** in stappen van 5 seconden zijn.

   ![Stream Analytics job output voor aggregatie, 13 records die zijn gegenereerd worden weergegeven](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-aggregation.png)
 
### <a name="detect-sim-fraud-using-a-self-join"></a>Met behulp van een self-join SIM-fraude te detecteren

Overweeg het gebruik van frauduleuze gesprekken die afkomstig uit dezelfde gebruiker, maar in verschillende locaties binnen 5 seconden van elkaar zijn worden voor dit voorbeeld. Zo kan dezelfde gebruiker niet op rechtmatige wijze op hetzelfde moment een telefoongesprek vanuit de Verenigde Staten en Australië initiëren. 

Als u wilt controleren voor deze gevallen, kunt u een self-join van de streaminggegevens toevoegen aan de stroom op basis van de `CallRecTime` waarde. U kunt vervolgens zoeken naar records waarin de `CallingIMSI` waarde (het oorspronkelijke aantal) is hetzelfde, maar de `SwitchNum` waarde (land van oorsprong) is niet hetzelfde.

Wanneer u een join gebruiken met het streamen van gegevens, dient de join dat enkele beperkingen met betrekking tot hoe ver de overeenkomende rijen in de tijd kunnen worden gescheiden. (Als u eerder hebt genoteerd, het streaming-gegevens is effectief eindeloos.) De tijdsgrenzen voor de relatie zijn opgegeven in de `ON` -component van de join, met behulp van de `DATEDIFF` functie. In dit geval is de join gebaseerd op een interval van 5 seconden van de van aanroepgegevens.

1. Wijzig de query in de code-editor in het volgende: 

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

    Deze query is, zoals een SQL-join, behalve voor de `DATEDIFF` functie in de join. Deze versie van `DATEDIFF` is specifiek voor Streaming Analytics, en deze moet worden weergegeven de `ON...BETWEEN` component. De parameters zijn een tijdseenheid (seconden in dit voorbeeld) en de aliassen van de twee bronnen voor de join. Dit wijkt af van de standaard SQL `DATEDIFF` functie.

    De `WHERE` -component bevat de voorwaarde die de frauduleus gesprek vlaggen: de oorspronkelijke switches zijn niet hetzelfde. 

2. Klik op **Test** opnieuw. 

   ![Stream Analytics-taakuitvoer voor self-join, zichtbaar 6 records die zijn gegenereerd](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-self-join.png)

3. Klik op **opslaan** om op te slaan van de query self-join als onderdeel van de stream Analytics-taak. (Deze kunt de voorbeeldgegevens niet opslaan.)

    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-save-button-new-portal.png" alt="drawing" width="300px"/>

## <a name="create-an-output-sink-to-store-transformed-data"></a>Maken van een uitvoerlocatie voor het opslaan van de getransformeerde gegevens

U kunt een gebeurtenissenstroom, een event hub invoeren voor het opnemen van gebeurtenissen en een query voor het uitvoeren van een transformatie via de stroom hebt gedefinieerd. De laatste stap is het definiëren van een uitvoerlocatie voor de taak, dat wil zeggen, een plaats om de getransformeerde stroom om te schrijven. 

U kunt veel bronnen gebruiken als uitvoerlocaties: een SQL Server-database, table storage, Data Lake-opslag, Power BI en zelfs een ander event hub. Voor deze zelfstudie schrijft u de stroom naar Azure Blob Storage, dit een gebruikelijke keuze is voor het verzamelen van informatie over de gebeurtenis voor latere analyse, omdat deze geschikt is voor niet-gestructureerde gegevens.

Als u een bestaande blob storage-account hebt, kunt u die kunt gebruiken. Voor deze zelfstudie leert u hoe u een nieuw opslagaccount maken.

### <a name="create-an-azure-blob-storage-account"></a>Een Azure Blob Storage-account maken

1. Selecteer in de linkerbovenhoek in Azure Portal **Een resource maken** > **Storage** > **Storage-account**. Vul de taak van de opslagaccountpagina met **naam** ingesteld op "asaehstorage" **locatie** ingesteld op "East US", **resourcegroep** ingesteld op 'asa-eh-ns-rg' (host het opslagaccount in dezelfde resourcegroep bevinden als de streamingtaak voor betere prestaties). De overige instellingen kunnen op de standaardwaarden blijven staan.  

   ![Een opslagaccount maken](./media/stream-analytics-real-time-fraud-detection/stream-analytics-storage-account-create.png)

2. Terug naar het deelvenster voor stream Analytics-taak in de Azure-portal. (Als u het deelvenster hebt gesloten, zoekt u `asa_frauddetection_job_demo` in de **alle resources** deelvenster.)

3. In de **Taaktopologie** sectie, klikt u op de **uitvoer** vak.

4. In de **uitvoer** deelvenster, klikt u op **toevoegen** en selecteer **Blob storage**. Vul vervolgens de nieuwe pagina in de uitvoer met de volgende informatie:

   |**Instelling**  |**Voorgestelde waarde**  |**Beschrijving**  |
   |---------|---------|---------|
   |Uitvoeralias  |  CallStream-FraudulentCalls   |  Voer een unieke naam in voor de uitvoer van de taak.   |
   |Abonnement   |  \<Uw abonnement\> |  Selecteer het Azure-abonnement met het opslagaccount dat u hebt gemaakt. Het opslagaccount kan voor hetzelfde of een ander abonnement gelden. Voor dit voorbeeld wordt aangenomen dat u een opslagaccount voor hetzelfde abonnement hebt gemaakt. |
   |Storage-account  |  asaehstorage |  Voer de naam van het opslagaccount dat u hebt gemaakt. |
   |Container  | asa-fraudulentcalls-demo | Kies Nieuw en voer de containernaam van een. |
    <br/>
    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-output-blob-storage-new-console.png" alt="drawing" width="300px"/>
    
5. Klik op **Opslaan**. 


## <a name="start-the-streaming-analytics-job"></a>De stream Analytics-taak starten

De taak is nu geconfigureerd. U hebt opgegeven invoer (de event hub), een transformatie (de query om te zoeken naar frauduleuze gesprekken) en uitvoer (blob storage). U kunt nu de taak starten. 

1. Zorg ervoor dat de app TelcoGenerator wordt uitgevoerd.

2. Klik in het taakdeelvenster **Start**. In de **starttaak** in het deelvenster voor uitvoer begintijd van taak, selecteer **nu**. 

   ![De Stream Analytics-taak starten](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start.png)



## <a name="examine-the-transformed-data"></a>De getransformeerde gegevens controleren

U hebt nu een volledige Streaming Analytics-taak. De taak is onderzoek van een stream van metagegevens van de telefonische oproep, zoeken voor frauduleuze gesprekken in realtime en informatie over deze frauduleuze gesprekken naar opslag schrijven. 

Voor deze zelfstudie, is het raadzaam om te kijken naar de gegevens die wordt vastgelegd door de stream Analytics-taak. De gegevens worden geschreven naar Azure BLOB-opslag in segmenten (bestanden). U kunt een hulpprogramma dat wordt gelezen van Azure Blob-opslag gebruiken. Zoals vermeld in de sectie vereisten, kunt u Azure-extensies in Visual Studio of kunt u een hulpprogramma zoals [Azure Storage Explorer](http://storageexplorer.com/) of [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction). 

Als u de inhoud van een bestand in blob-opslag bekijkt, ziet u ongeveer als volgt:

   ![Azure blob-opslag met stream Analytics-uitvoer](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-blob-storage-view.png)
 

## <a name="clean-up-resources"></a>Resources opschonen

Er zijn aanvullende artikelen die doorgaan met het detecteren van fraude scenario en de resources die u hebt gemaakt in deze zelfstudie gebruiken. Als u doorgaan wilt, raadpleegt u de suggesties onder **Vervolgstappen**.

Echter, als u klaar bent en u hoeft niet de resources die u hebt gemaakt, u kunt ze verwijderen zodat u hiervoor geen onnodige Azure kosten in rekening gebracht. In dat geval is het raadzaam dat u het volgende doen:

1. De stream Analytics-taak stoppen. In de **taken** deelvenster, klikt u op **stoppen** aan de bovenkant.
2. Stop de Telco Generator-app. In het opdrachtvenster waar u de app gestart, drukt u op Ctrl + C.
3. Als u een nieuwe blob storage-account alleen voor deze zelfstudie hebt gemaakt, het verwijderen. 
4. Verwijder de Streaming Analytics-taak.
5. Verwijder de event hub.
6. De event hub-naamruimte verwijderen.

## <a name="get-support"></a>Ondersteuning krijgen

Voor verdere ondersteuning kunt u proberen de [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen

U kunt deze zelfstudie met het volgende artikel:

* [Stream Analytics en Power BI: een realtime analytics-dashboard voor het streamen van gegevens](stream-analytics-power-bi-dashboard.md). Dit artikel leest u hoe de TelCo uitvoer van de Stream Analytics-taak verzenden naar Power BI voor realtime visualisatie en analyse.

Zie voor meer informatie over Stream Analytics in het algemeen in deze artikelen:

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
