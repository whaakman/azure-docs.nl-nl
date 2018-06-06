---
title: 'Zelfstudie: Een Stream Analytics-taak maken en beheren met behulp van Azure Portal | Microsoft Docs'
description: Deze zelfstudie biedt een end-to-end-illustratie van het gebruik van Azure Stream Analytics voor het analyseren van frauduleuze gesprekken in een reeks telefoongesprekken.
services: stream-analytics
author: SnehaGunda
ms.author: sngun
manager: kfile
ms.service: stream-analytics
ms.workload: data-services
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/04/2018
ms.openlocfilehash: 524b15747a275c76fec6c529e4f00d0da1b41420
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "32778186"
---
# <a name="create-a-stream-analytics-job-to-analyze-phone-call-data-and-visualize-results-in-a-power-bi-dashboard"></a>Een Stream Analytics-taak maken om gegevens van telefoongesprekken te analyseren en de resultaten te visualiseren in een Power BI-dashboard

Deze zelfstudie laat zien hoe u Azure Stream Analytics kunt gebruiken om een voorbeeld van een telefoongesprek te analyseren dat door een clienttoepassing wordt gegenereerd. De gegevens van telefoongesprekken die door de clienttoepassing worden gegenereerd, bevatten enkele frauduleuze gesprekken. We gaan een Stream Analytics-taak definiëren om dergelijke gesprekken eruit te filteren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gegevens van een voorbeeld van een telefoongesprek genereren en deze verzenden naar Azure Event Hubs  
> * Een Stream Analytics-taak maken   
> * Invoer en uitvoer naar de taak configureren  
> * Een query definiëren om frauduleuze gesprekken eruit te filteren  
> * De taak testen en starten  
> * Resultaten visualiseren in Power BI 

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u het volgende hebt voordat u begint:

* Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) aan.  
* Meld u aan bij [Azure Portal](https://portal.azure.com/).  
* Download de app voor het genereren van telefoongesprekgebeurtenissen [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) uit het Microsoft Downloadcentrum of haal de broncode op uit [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator).  

## <a name="create-an-azure-event-hub"></a>Een Azure Event Hub maken 

Voordat Stream Analytics de gegevensstroom van frauduleuze gesprekken kan analyseren, moet u de gegevens naar Azure verzenden. In deze zelfstudie verzendt u gegevens naar Azure met behulp van [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs). Voor deze zelfstudie maakt u een event hub en laat u de app voor het genereren van gebeurtenissen gesprekgegevens naar die event hub verzenden. Voer de volgende stappen uit om een event hub te maken:

1. Meld u aan bij Azure Portal.  
2. Selecteer **Een resource maken** > **Internet of Things** > **Event Hubs**.  

   ![Naar event hub zoeken](media/stream-analytics-manage-job/find-eh.png)
3. Voer in het deelvenster **Naamruimte maken** de volgende waarden in:  

   |**Instelling**  |**Voorgestelde waarde** |**Beschrijving**  |
   |---------|---------|---------|
   |Naam     | myEventHubNS        |  Een unieke naam voor het identificeren van de event hub-naamruimte.       |
   |Abonnement     |   \<Uw abonnement\>      |   Selecteer een Azure-abonnement waarvoor u de event hub wilt maken.      |
   |Resourcegroep     |   MyASADemoRG      |  Selecteer **Nieuwe maken** en voer een naam voor de nieuwe resourcegroep voor uw account in.       |
   |Locatie     |   VS - west 2      |    De locatie waar de event hub-naamruimte kan worden geïmplementeerd.     |

4. Gebruik standaardopties voor de resterende instellingen en selecteer **Maken**.  

   ![Event hub-naamruimte maken](media/stream-analytics-manage-job/create-ehns.png)

5. Nadat de naamruimte is geïmplementeerd, gaat u naar **Alle resources** > zoekt u naar 'myEventHubNS' in de lijst met Azure-resources > selecteer om dit te openen.  
6. Selecteer vervolgens **+Event Hub** > **en noem** de event hub 'MyEventHub'. U kunt ook een andere naam gebruiken. Gebruik standaardopties voor resterende instellingen, selecteer **Maken** en wacht tot de implementatie is voltooid.

   ![Event hub maken](media/stream-analytics-manage-job/create-eh.png)

### <a name="grant-access-to-the-event-hub-and-get-a-connection-string"></a>Toegang verlenen tot de event hub en een verbindingsreeks ophalen

Voordat een toepassing gegevens naar Azure Event Hubs kan verzenden, moet de event hub een beleid hebben waarmee de juiste toegang wordt verleend. Het toegangsbeleid genereert een verbindingsreeks die autorisatiegegevens bevat.

1. Navigeer naar de **Event Hub** 'MyEventHub', die u in de vorige stap hebt gemaakt > selecteer **Gedeeld toegangsbeleid** in het deelvenster van de event hub > selecteer **+Toevoegen**.  
2. Stel de naam van het beleid in op **Mypolicy** > en selecteer **Beheren** > selecteer **Maken**.  

   ![Gedeeld toegangsbeleid voor event hub maken](media/stream-analytics-manage-job/create-ehpolicy.png)

3. Nadat het beleid is geïmplementeerd, selecteert u het beleid om dat te openen. Zoek vervolgens naar **Verbindingsreeks–primaire sleutel** en selecteer de **kopie** naast de verbindingsreeks.  
4. Plak de verbindingsreeks in een teksteditor. U hebt deze verbindingsreeks nodig in de volgende sectie.  

   De verbindingsreeks ziet er als volgt uit:

   `Endpoint=sb://<Your event hub namespace>.servicebus.windows.net/;SharedAccessKeyName=<Your shared access policy name>;SharedAccessKey=<generated key>;EntityPath=<Your event hub name>` 

   U ziet dat de verbindingsreeks meerdere sleutel-waardeparen bevat, gescheiden door puntkomma's: Endpoint, SharedAccessKeyName, SharedAccessKey en EntityPath.  

5. Verwijder het EntityPath-paar uit de verbindingsreeks (vergeet daarbij niet de voorafgaande puntkomma te verwijderen).

## <a name="start-the-event-generator-application"></a>De app voor het genereren van gebeurtenissen starten

Voordat u de app TelcoGenerator start, moet u deze configureren voor het verzenden van gegevens naar de Azure Event Hubs die u eerder hebt gemaakt.

1. Pak de inhoud van het bestand [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) uit.  
2. Open het bestand `TelcoGenerator\TelcoGenerator\telcodatagen.exe.config` in een teksteditor van uw keuze (er is meer dan één .config-bestand, let er dus op dat u het juiste bestand opent).  

3. Update het <appSettings>-element in het configuratiebestand met de volgende details:

   * Stel de waarde van de sleutel EventHubName in op de waarde van het EntityPath in de verbindingsreeks.  
   * Stel de waarde van de sleutel Microsoft.ServiceBus.ConnectionString in op de verbindingsreeks zonder de EntityPath-waarde. Dat is de waarde die u hebt verkregen in stap 5 in de vorige sectie.

4. Sla het bestand op.  
5. Open vervolgens een opdrachtvenster, ga naar de map waar u de toepassing TelcoGenerator hebt uitgepakt en voer de volgende opdracht in:

   ```
   telcodatagen.exe 1000 .2 2
   ```

   Voor deze opdracht worden de volgende parameters gebruikt:
   * **Aantal records met gespreksgegevens per uur**.  
   * **Percentage van fraudekans**: hoe vaak de app een frauduleus gesprek moet simuleren. Een waarde van .2 betekent dat ongeveer 20% van de gespreksrecords er frauduleus uitziet.  
   * **De tijd in uren**: het aantal uren dat de app moet worden uitgevoerd. U kunt de app op elk gewenst moment stoppen door het proces te beëindigen (met Ctrl+C) op de opdrachtregel.

   Na enkele seconden begint de app met het weergeven van telefoongesprekrecords op het scherm wanneer deze naar de event hub worden gestuurd. De telefoongesprekgegevens bevatten de volgende velden:

   |**Record**  |**Definitie**  |
   |---------|---------|
   |CallrecTime    |  Het tijdstempel voor de begintijd van de oproep.       |
   |SwitchNum     |  Het schakelnummer van de oproep. In dit voorbeeld zijn de schakelnummers tekenreeksen die staan voor het land van herkomst (VS, China, UK, Duitsland of Australië).       |
   |CallingNum     |  Het telefoonnummer van de beller.       |
   |CallingIMSI     |  De International Mobile Subscriber Identity (IMSI). Dit is een unieke id van de beller.       |
   |CalledNum     |   Het telefoonnummer van de ontvanger.      |
   |CalledIMSI     |  De International Mobile Subscriber Identity (IMSI). Dit is een unieke id van de ontvanger.       |

## <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken 

Nu u een stream van gesprekgebeurtenissen hebt, kunt u een Stream Analytics-taak maken die gegevens uit de event hub leest.

1. Als u een Stream Analytics-taak wilt maken, gaat u naar Azure Portal  

2. Selecteer **Een resource maken** > **Internet of Things** > **Stream Analytics-taak**.  

3. Voer in het deelvenster **Nieuwe Stream Analytics-taak** de volgende waarden in:  

   |**Instelling**  |**Voorgestelde waarde**  |**Beschrijving**  |
   |---------|---------|---------|
   |Taaknaam     |  ASATutorial       |   Een unieke naam voor het identificeren van de event hub-naamruimte.      |
   |Abonnement    |  \<Uw abonnement\>   |   Selecteer een Azure-abonnement waarvoor u de taak wilt maken.       |
   |Resourcegroep   |   MyASADemoRG      |   Selecteer **Bestaande gebruiken** en voer een naam voor de nieuwe resourcegroep voor uw account in.      |
   |Locatie   |    VS - west 2     |      De locatie waar de taak kan worden geïmplementeerd. Het is raadzaam de taak en de event hub in dezelfde regio te plaatsen om de best mogelijke prestaties te realiseren en u niet hoeft te betalen voor het overbrengen van gegevens tussen regio's.      |
   |Hostingomgeving    | Cloud        |     Stream Analytics-taken kunnen worden geïmplementeerd in Cloud of in Edge. Met Cloud kunt u taken implementeren naar Azure Cloud en met Edge kunt u taken implementeren naar een IoT-randapparaat.    |
   |Streaming-eenheden     |    1       |      Streaming-eenheden vertegenwoordigen de computerresources die nodig zijn om een taak uit te voeren. Deze waarde is standaard ingesteld op 1. Zie het artikel [Streaming-eenheden begrijpen en aanpassen](stream-analytics-streaming-unit-consumption.md) voor meer informatie over het schalen van streaming-eenheden.      |

   ![Een taak maken](media/stream-analytics-manage-job/create-a-job.png)   

4. Gebruik standaardopties voor de resterende instellingen, selecteer **Maken** en wacht tot de implementatie is voltooid.

## <a name="configure-job-input"></a>Taakinvoer configureren

De volgende stap is het definiëren van een invoerbron voor de taak om gegevens te lezen. Voor deze zelfstudie gebruikt u de event hub die u in de vorige sectie als invoer hebt gemaakt. Voer de volgende stappen uit om de invoer voor de taak te configureren:

1. Open het deelvenster **Alle resources** vanuit Azure Portal en zoek de ASATutorial Stream Analytics-taak.  

2. Selecteer in de sectie **Taaktopologie** van het deelvenster van de Stream Analytics-taak de optie **Invoer**.  

3. Selecteer **+Stroominvoer toevoegen** (referentie-invoer verwijst naar statische opzoekgegevens, die u in deze zelfstudie niet gebruikt), **Event hub** en voer in het deelvenster de volgende waarden in:  

   |**Instelling**  |**Voorgestelde waarde**  |**Beschrijving**  |
   |---------|---------|---------|
   |Invoeralias     |  CallStream       |  Geef een beschrijvende naam op om uw invoer te identificeren. De invoeralias mag alleen alfanumerieke tekens, afbreekstreepjes en onderstrepingstekens bevatten en moet 3 tot 63 tekens lang zijn.       |
   |Abonnement    |   \<Uw abonnement\>      |   Selecteer het Azure-abonnement waarvoor u de event hub hebt gemaakt. De event hub kan zich in dezelfde of een ander abonnement als de Stream Analytics-taak bevinden.       |
   |Event hub-naamruimte    |  MyEventHubNS       |  Selecteer de event hub-naamruimte die u in de vorige sectie hebt gemaakt. Alle in uw huidige abonnement beschikbare event hub-naamruimten worden weergegeven in de vervolgkeuzelijst.       |
   |Event Hub-naam    |   MyEventHub      |  Selecteer de event hub die u in de vorige sectie hebt gemaakt. Alle in uw huidige abonnement beschikbare event hubs worden weergegeven in de vervolgkeuzelijst.       |
   |Naam van het Event Hub-beleid   |  Mypolicy       |  Selecteer het door de event hub gedeelde toegangsbeleid dat u in de vorige sectie hebt gemaakt. Alle in uw huidige abonnement beschikbare beleidsregels voor event hubs worden weergegeven in de vervolgkeuzelijst.       |

   ![Invoer configureren](media/stream-analytics-manage-job/configure-input.png) 

4. Gebruik standaardopties voor de resterende instellingen, selecteer **Opslaan** en wacht tot de implementatie is voltooid.

## <a name="configure-job-output"></a>Taakuitvoer configureren 

De laatste stap is bedoeld voor het definiëren van een uitvoerlocatie voor de taak waar de getransformeerde gegevens naartoe kunnen worden geschreven. Voor deze zelfstudie voert u de resultaten uit naar Power BI en visualiseert u de datum. Voer de volgende stappen uit om de uitvoer voor de taak te configureren:

1. Open het deelvenster **Alle resources** vanuit Azure Portal en zoek de ASATutorial Stream Analytics-taak.  

2. Selecteer in de sectie **Taaktopologie** van het deelvenster van de Stream Analytics-taak de optie **Uitvoer**.  

3. Selecteer **+Toevoegen** > **Power BI** en vul in het formulier de volgende details in (u kunt een beschrijvende naam opgeven voor de uitvoeralias, de naam van de gegevensset en de tabelnaam zoals weergegeven in de tabel) en selecteer **Autoriseren**:  

   |**Instelling**  |**Voorgestelde waarde**  |
   |---------|---------|---------|
   |Uitvoeralias  |  MyPBIoutput  |
   |Naam van de gegevensset  |   ASAdataset  | 
   |Tabelnaam |  ASATable  | 

   ![Uitvoer configureren](media/stream-analytics-manage-job/configure-output.png)  

4. Nadat u hebt geselecteerd op **Autoriseren**, wordt er een pop-upvenster geopend en wordt u gevraagd referenties te verstrekken als verificatie voor uw Power BI-account. Zodra de autorisatie geslaagd is, kunt u de instellingen **Opslaan**. 

## <a name="define-a-query-to-analyze-input-data"></a>Een query voor het analyseren van invoergegevens definiëren

Nadat u een Stream Analytics-taak zodanig hebt ingesteld dat een binnenkomende gegevensstroom kan worden gelezen, is de volgende stap het maken van een transformatie waarmee gegevens in real-time worden geanalyseerd. U definieert de transformatie-query met [Stream Analytics Query Language](https://msdn.microsoft.com/library/dn834998.aspx). In deze zelfstudie definieert u een query die frauduleuze gesprekken in de telefoongegevens detecteert. 

In dit voorbeeld beschouwen we frauduleuze gesprekken als gesprekken die afkomstig zijn van dezelfde gebruiker, maar uitgevoerd vanaf verschillende locaties en waarvan de duur tussen beide gesprekken 5 seconden bedraagt. Zo kan dezelfde gebruiker niet op rechtmatige wijze op hetzelfde moment een telefoongesprek vanuit de Verenigde Staten en Australië initiëren. Als u de transformatie-query voor uw Stream Analytics-taak wilt definiëren, voert u de volgende stappen uit:

1. Open het deelvenster **Alle resources** vanuit Azure Portal en open de **ASATutorial** Stream Analytics-taak die u eerder hebt gemaakt.  

2. Selecteer in de sectie **Taaktopologie** van het deelvenster van de Stream Analytics-taak de optie **Query**. Het pop-upvenster vermeldt de invoer en uitvoer die voor de taak zijn geconfigureerd en u kunt een query voor het transformeren van de invoerstroom maken.  

3. Vervang vervolgens de bestaande query in de editor door de volgende gegevens, waarbij deze query een self-join uitvoert met een interval van 5 seconden aan gesprekgegevens:

   ```sql
   SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
   INTO "MyPBIoutput"
   FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
   JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime
   ON CS1.CallingIMSI = CS2.CallingIMSI
   AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
   WHERE CS1.SwitchNum != CS2.SwitchNum
   GROUP BY TumblingWindow(Duration(second, 1))
   ```

   Als u wilt controleren op frauduleuze gesprekken, moet u een self-join uitvoeren voor de streaminggegevens op basis van de `CallRecTime`-waarde. U kunt vervolgens zoeken naar gespreksrecords waarvan de `CallingIMSI`-waarde (het nummer waarmee de oproep is uitgevoerd) hetzelfde is, maar waarvan de `SwitchNum`-waarde (land van herkomst) verschilt. Wanneer u een JOIN-bewerking voor streaminggegevens gebruikt, moet de join enkele beperkingen bevatten met betrekking tot hoe ver de overeenkomende rijen in tijd kunnen worden gescheiden. Als er sprake is van een eindeloze gegevensstroom, worden de tijdsgrenzen voor de relatie opgegeven binnen de ON-component van de join met behulp van de [DATEDIFF](https://msdn.microsoft.com/azure/stream-analytics/reference/datediff-azure-stream-analytics)-functie. 

   Deze query is net als een normale SQL-join, met als enig verschil de DATEDIFF-functie. De in deze query gebruikte DATEDIFF-functie is specifiek voor Streaming Analytics en moet worden opgenomen in de `ON...BETWEEN`-component.  

4. U moet de query **Opslaan**.  

   ![query definiëren](media/stream-analytics-manage-job/define-query.png) 

## <a name="test-your-query"></a>De query testen

U kunt een query testen vanuit de query-editor en u hebt daarvoor voorbeeldgegevens nodig. Voor dit scenario extraheert u voorbeeldgegevens uit de stroom van telefoongesprekken die binnenkomt in de event hub. Voer de volgende stappen uit om de query te testen:

1. Zorg ervoor dat de app TelcoGenerator wordt uitgevoerd en records van telefoongesprekken produceert.  

2. Selecteer in het deelvenster **Query** de puntjes naast de CallStream-invoer en selecteer vervolgens **Voorbeeldgegevens van uitvoer**. Hiermee opent u een deelvenster waar u kunt opgeven hoeveel voorbeeldgegevens van de invoerstroom moeten worden gelezen.  

   ![Voorbeeldinvoergegevens](media/stream-analytics-manage-job/sample-input-data.png)  

3. Stel **Minuten** in op 3 en selecteer **OK**. Drie minuten aan gegevens wordt opgehaald uit de invoerstroom en u krijgt een melding wanneer de voorbeeldgegevens gereed zijn. U kunt de status van het samplen bijhouden op de meldingsbalk. 

   De voorbeeldgegevens worden tijdelijk opgeslagen en zijn beschikbaar zolang u het queryvenster geopend houdt. Als u het queryvenster sluit, worden de voorbeeldgegevens verwijderd en zult u een nieuwe set voorbeeldgegevens moeten maken. Als alternatief kunt u een .json-bestand met voorbeeldgegevens ophalen uit [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample Data/telco.json) en dat .json-bestand uploaden om te gebruiken als set met voorbeeldgegevens voor de CallStream-invoer.  

4. Selecteer **Testen** om de query te testen. De uitvoerresultaten moeten eruitzien zoals in deze schermafbeelding:  

   ![Testuitvoer](media/stream-analytics-manage-job/test-output.png)

## <a name="start-the-job-and-visualize-output"></a>De taak starten en uitvoer visualiseren

1. Als u de taak wilt starten, gaat u naar het deelvenster **Overzicht** van de taak en selecteert u **Starten**.  

2. Selecteer **Nu** voor de starttijd van de taakuitvoer en selecteer **Starten**. De taak wordt na enkele minuten gestart en u kunt de status bekijken in de meldingsbalk.  

3. Nadat de taak is voltooid, gaat u naar [Powerbi.com](https://powerbi.com/) en meldt u zich aan met uw werk- of schoolaccount. Als de query van de Stream Analytics-taak resultaten produceert, ziet u dat de gegevensset al is gemaakt. Ga naar het tabblad **Gegevenssets**, waar een gegevensset met de naam 'ASAdataset' wordt weergegeven.  

4. Selecteer **+Maken** vanuit de werkruimte. Maak een nieuw dashboard en noem dat Frauduleuze gesprekken. U gaat twee tegels toevoegen aan dit dashboard, waarbij de ene tegel wordt gebruikt om de telling van frauduleuze gesprekken voor een bepaald geval te laten zien en de andere tegel een lijngrafiekweergave bevat.  

5. Selecteer boven aan het venster **Tegel toevoegen** > en selecteer **Aangepaste streaminggegevens** > Volgende > kies de **ASAdataset** > selecteer **Kaart** als Visualisatietype > en Velden als **fraudulentcalls**. Selecteer **Volgende** > voer een naam in voor de tegel en selecteer **Toepassen**.  

   ![Tegels maken](media/stream-analytics-manage-job/create-tiles.png)

6. Volg nogmaals de stappen 4, met de volgende opties:
   * Selecteer in Type visualisatie de optie Lijndiagram.  
   * Voeg een as toe en selecteer **windowend**.  
   * Voeg een waarde toe en selecteer **fraudulentcalls**.  
   * Selecteer bij **Tijdvenster voor weergave** de laatste 10 minuten.  

7. Uw dashboard ziet eruit als de volgende schermafbeelding nadat beide tegels zijn toegevoegd. U zult zien dat als uw event hub-zendtoepassing en de Streaming Analytics-toepassing worden uitgevoerd, uw Power BI-dashboard regelmatig wordt bijgewerkt met nieuw ontvangen gegevens.  

   ![Power BI-resultaten](media/stream-analytics-manage-job/power-bi-results.png)

## <a name="embedding-your-powerbi-dashboard-in-a-web-application"></a>Uw Power BI-dashboard insluiten in een webtoepassing

Voor dit gedeelte van de zelfstudie maakt u gebruik van een voorbeeld van een [ASP.NET](http://asp.net/)-webtoepassing die gemaakt is door het Power BI-team en waarmee u uw dashboard kunt insluiten. Zie het artikel [Insluiten met Power BI](https://docs.microsoft.com/power-bi/developer/embedding) voor meer informatie over het insluiten van dashboards.

In deze zelfstudie volgen we de stappen voor de toepassing Gebruiker is eigenaar van gegevens. Stel de toepassing in door naar de Github-opslagplaats [Power BI-Developer-Samples](https://github.com/Microsoft/PowerBI-Developer-Samples) te gaan en volg de instructies in de sectie **Gebruiker is eigenaar van gegevens** (gebruik de omleidings- en startpagina-URL in de subsectie **integrate-dashboard-web-app**). Omdat we werken met het Dashboard-voorbeeld, gebruiken we de voorbeeldcode voor integrate-dashboard-web-app in de [GitHub-opslagplaats](https://github.com/Microsoft/PowerBI-Developer-Samples/tree/master/User Owns Data/integrate-dashboard-web-app).
Zodra de toepassing in uw browser wordt uitgevoerd, volgt u deze stappen voor het insluiten van het dashboard dat u eerder in de webpagina hebt gemaakt:

1. Selecteer **Aanmelden bij Power BI**, waarmee de toepassing toegang wordt verleend tot de dashboards in uw Power BI-account.  

2. Selecteer de knop **Dashboards ophalen**, waardoor de dashboards van uw account in een tabel worden weergegeven. Zoek de naam van het dashboard dat u eerder hebt gemaakt (powerbi-embedded-dashboard) en kopieer de bijbehorende **EmbedUrl**.  

3. Plak tot slot de **EmbedUrl** in het bijbehorende tekstveld en selecteer **Dashboard insluiten**. U kunt nu hetzelfde dashboard zien dat is ingesloten in een webtoepassing.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een eenvoudige Stream Analytics-taak gemaakt, de binnenkomende gegevens geanalyseerd en de resultaten gepresenteerd in een Power BI-dashboard. Ga voor meer informatie over Stream Analytics-taken verder met de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Azure Functions uitvoeren binnen Stream Analytics-taken](stream-analytics-with-azure-functions.md)
