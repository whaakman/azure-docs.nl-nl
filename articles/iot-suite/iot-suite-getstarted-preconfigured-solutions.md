---
title: Aan de slag met vooraf geconfigureerde oplossingen | Microsoft Docs
description: Volg deze zelfstudie voor meer informatie over het implementeren van een vooraf geconfigureerde Azure IoT Suite-oplossing.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 6ab38d1a-b564-469e-8a87-e597aa51d0f7
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 9e1bcba086a9f70c689a5d7d7713a8ecdc764492
ms.openlocfilehash: 8248e0a02cb0775a87f0c8130e53b98f8bcfe581
ms.lasthandoff: 02/27/2017


---
# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Zelfstudie: Aan de slag met vooraf geconfigureerde oplossingen
## <a name="introduction"></a>Inleiding
[Vooraf geconfigureerde oplossingen][lnk-preconfigured-solutions] voor Azure IoT Suite zijn voorzien van meerdere Azure IoT-services om totaaloplossingen te leveren voor het implementeren van algemene IoT-bedrijfsscenario's. De vooraf geconfigureerde oplossing *externe controle* maakt verbinding met en controleert uw apparaten. U kunt deze oplossing gebruiken om de datastroom van uw apparaten te analyseren en de bedrijfsresultaten te verbeteren door processen automatisch te laten reageren op die datastroom.

In deze zelfstudie leert u hoe de vooraf geconfigureerde oplossing voor externe controle inricht. Hierbij maakt u ook kennis met de basisfuncties van de vooraf geconfigureerde oplossing. U hebt toegang tot veel van deze functies via het oplossingsdashboard dat als onderdeel van de vooraf geconfigureerde oplossing wordt geïmplementeerd:

![Vooraf geconfigureerd oplossingsdashboard voor externe controle][img-dashboard]

U hebt een actief Azure-abonnement nodig om deze zelfstudie te voltooien.

> [!NOTE]
> Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure][lnk_free_trial] voor meer informatie.
> 
> 

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>Het oplossingsdashboard bekijken
Vanaf het dashboard van de oplossing kunt u de geïmplementeerde oplossing beheren. U kunt er bijvoorbeeld telemetrie weergeven, apparaten toevoegen en regels configureren.

1. Wanneer het inrichten is voltooid en voor de tegel voor uw vooraf geconfigureerde oplossing de status **Gereed** wordt weergegeven, klikt u op **Starten** om uw oplossingsportal voor externe controle te openen in een nieuw tabblad.
   
   ![De vooraf geconfigureerde oplossing starten][img-launch-solution]
2. Standaard ziet u in de oplossingsportal het *dashboard van de oplossing*. In het menu links kunt u andere weergaven selecteren.
   
   ![Vooraf geconfigureerd oplossingsdashboard voor externe controle][img-dashboard]

Het dashboard bevat de volgende informatie:

* De kaart toont de locatie van elk apparaat dat met de oplossing is verbonden. Wanneer u de oplossing voor het eerst uitvoert, zijn er vier gesimuleerde apparaten. De gesimuleerde apparaten worden geïmplementeerd als Azure WebJobs en de oplossing gebruikt de API van Bing Kaarten om informatie op de kaart te tekenen.
* Het deelvenster **Telemetriegeschiedenis** tekent de vochtigheids- en temperatuurtelemetrie van een geselecteerd apparaat in bijna realtime en geeft statistische gegevens weer, zoals de maximale, minimale en gemiddelde vochtigheid.
* Het deelvenster **Geschiedenis van waarschuwingen** toont recente waarschuwingsgebeurtenissen wanneer voor een telemetriewaarde een drempelwaarde wordt overschreden. Naast de voorbeelden die met de vooraf geconfigureerde oplossing zijn gemaakt, kunt u ook uw eigen alarmen definiëren.
* Het deelvenster **Taken** geeft informatie weer over geplande taken. U kunt uw eigen taken plannen op de pagina **Beheertaken**.

## <a name="view-the-device-list"></a>De lijst met apparaten weergeven
De *Apparatenlijst* bevat alle geregistreerde apparaten in de oplossing. Uit de apparatenlijst kunt u metagegevens voor apparaten weergeven en bewerken, apparaten toevoegen of verwijderen en methoden aanroepen op apparaten.

1. Klik links in het menu op **Apparaten** om de apparatenlijst voor deze oplossing weer te geven.
   
   ![Lijst met apparaten in het dashboard][img-devicelist]
2. De apparatenlijst geeft eerst aan dat bij het inrichtingsproces vier gesimuleerde apparaten zijn gemaakt. U kunt extra gesimuleerde en fysieke apparaten aan de oplossing toevoegen.
3. U kunt de informatie die in de apparatenlijst wordt weergegeven, aanpassen door te klikken op **Kolomeditor**. U kunt kolommen die gerapporteerde eigenschaps- en tagwaarden weergeven, toevoegen en verwijderen. U kunt ook de volgorde en namen van de kolommen wijzigen:
   
   ![Kolomeditor in het dashboard][img-columneditor]
4. Klik op een apparaat in de apparatenlijst om de details ervan weer te geven.
   
   ![Apparaatdetails in het dashboard][img-devicedetails]

Het deelvenster **Apparaatdetails** bevat zes gedeeltes:

* Een verzameling van koppelingen waarmee u het apparaatpictogram kunt aanpassen, het apparaat kunt uitschakelen, een regel kunt toevoegen, een methode kunt aanroepen of een opdracht kunt verzenden. Zie [Cloud-to-device communications guidance][lnk-c2d-guidance] (Richtlijnen voor communicatie tussen cloud en apparaat) voor een vergelijking van opdrachten (berichten van apparaat naar cloud) en methoden (directe methoden).
* In het gedeelte **Apparaatdubbel - Tags** kunt u tagwaarden voor het apparaat bewerken. U kunt tagwaarden in de apparatenlijst weergeven en tagwaarden gebruiken om de apparatenlijst te filteren.
* In het gedeelte **Apparaatdubbel - Gewenste eigenschappen** kunt u eigenschapswaarden instellen die moeten worden verzonden naar het apparaat.
* Het gedeelte **Apparaatdubbel - Gerapporteerde eigenschappen** geeft eigenschapswaarden weer die vanaf het apparaat worden verzonden.
* Het gedeelte **Apparaateigenschappen** geeft informatie weer vanuit het identiteitsregister, zoals het apparaat-id en de verificatiesleutels.
* Het gedeelte **Recente taken** geeft informatie weer over taken die onlangs op dit apparaat zijn gericht.

## <a name="customize-the-device-icon"></a>Het apparaatpictogram aanpassen

U kunt als volgt vanuit het deelvenster **Apparaatdetails** het apparaatpictogram aanpassen dat wordt weergegeven in de apparatenlijst:

1. Klik op het potloodpictogram om het deelvenster **Afb. bewerken** voor een apparaat te openen:
   
   ![De afbeeldingseditor van het apparaat openen][img-startimageedit]
2. Upload een nieuwe afbeelding of gebruik een van de bestaande afbeeldingen en klik vervolgens op **Opslaan**:
   
   ![De afbeelding van het apparaat bewerken in de afbeeldingseditor][img-imageedit]
3. De geselecteerde afbeelding wordt nu weergegeven in de kolom **Pictogram** voor het apparaat.

> [!NOTE]
> De afbeelding wordt opgeslagen in Blob Storage. Een tag in de apparaatdubbel bevat een koppeling naar de afbeelding in Blob Storage.
> 
> 

## <a name="invoke-a-method-on-a-device"></a>Een methode op een apparaat aanroepen
Van het deelvenster **Apparaatdetails** kunt u methoden op het apparaat aanroepen. Wanneer een apparaat voor het eerst wordt gestart, stuurt het naar de oplossing informatie over de methoden die het apparaat ondersteunt.

1. Klik in het deelvenster **Apparaatdetails** op **Methoden** voor het geselecteerde apparaat:
   
   ![Apparaatmethoden in dashboard][img-devicemethods]
2. Selecteer **Opnieuw opstarten** in de methodelijst.
3. Klik op **Methode aanroepen**.
4. U kunt de status van de methodeaanroep bekijken in de methodegeschiedenis.
   
   ![Methodestatus in het dashboard][img-pingmethod]

De oplossing houdt de status van elke methode bij die deze aanroept. Als het apparaat de methode voltooit, wordt er een nieuwe vermelding in de methodegeschiedenistabel weergegeven.

Sommige methoden starten asynchrone taken op het apparaat. De methode **InitiateFirmwareUpdate** start bijvoorbeeld een asynchrone taak om de update uit te voeren. Het apparaat gebruikt gerapporteerde eigenschappen om te rapporteren over de status van de firmware-update terwijl deze wordt uitgevoerd.

## <a name="send-a-command-to-a-device"></a>Een opdracht naar een apparaat verzenden
Vanuit het deelvenster **Apparaatdetails** kunt u opdrachten naar het apparaat verzenden. Wanneer een apparaat voor het eerst wordt gestart, stuurt het naar de oplossing informatie over de opdrachten die het apparaat ondersteunt.

1. Klik in het deelvenster **Apparaatdetails** op **Opdrachten** voor het geselecteerde apparaat:
   
   ![Apparaatopdrachten in het dashboard][img-devicecommands]
2. Selecteer **PingDevice** in de lijst met opdrachten.
3. Klik op **Opdracht verzenden**.
4. U ziet de status van de opdracht in de opdrachtgeschiedenis.
   
   ![Opdrachtstatus in het dashboard][img-pingcommand]

De oplossing houdt de status van elke opdracht bij die met de oplossing wordt verzonden. In eerste instantie is het resultaat **In behandeling**. Wanneer het apparaat meldt dat het de opdracht heeft uitgevoerd, wordt het resultaat ingesteld op **Geslaagd**.

## <a name="add-a-new-simulated-device"></a>Een nieuw gesimuleerd apparaat toevoegen
Wanneer u de vooraf geconfigureerde oplossing implementeert, voorziet u automatisch de vier proefapparaten die u ziet in de apparatenlijst. Deze apparaten zijn *gesimuleerde apparaten* uitgevoerd in een Azure WebJob. Gesimuleerde apparaten maken het voor u gemakkelijk om te experimenteren met een vooraf geconfigureerde oplossing zonder echte, fysieke apparaten te moeten implementeren. Raadpleeg de zelfstudie [Uw apparaat koppelen aan de vooraf geconfigureerde oplossing voor externe bewaking][lnk-connect-rm] als u een echt apparaat op de oplossing wilt aansluiten.

De volgende stappen laten zien hoe u een gesimuleerd apparaat toevoegt aan de oplossing:

1. Ga terug naar de lijst met apparaten.
2. Klik linksonder op **+ Een apparaat toevoegen** om een nieuw apparaat toe te voegen.
   
   ![Een apparaat toevoegen aan de vooraf geconfigureerde oplossing][img-adddevice]
3. Klik op **Nieuwe toevoegen** op de tegel **Gesimuleerd apparaat**.
   
   ![Nieuwe apparaatgegevens in het dashboard instellen][img-addnew]
   
   U kunt niet alleen een nieuw gesimuleerd apparaat maken, maar ook een fysiek apparaat toevoegen als u ervoor kiest om een **aangepast apparaat** te maken. Zie [Uw apparaat aansluiten op de vooraf geconfigureerde IoT Suite-oplossing voor externe bewaking][lnk-connect-rm] voor meer informatie over het koppelen van fysieke apparaten aan de oplossing.
4. Selecteer **Laat mij mijn eigen apparaat-id definiëren** en voer de unieke id van een apparaatnaam in, zoals **mijnapparaat_01**.
5. Klik op **Create**.
   
   ![Een nieuw apparaat opslaan][img-definedevice]
6. Klik in stap 3 van **Een gesimuleerd apparaat toevoegen** op **Gereed** om terug te keren naar de lijst met apparaten.
7. In de lijst met apparaten kunt u zien dat uw apparaat **wordt uitgevoerd**.
   
    ![Nieuw apparaat weergeven in de apparatenlijst][img-runningnew]
8. U kunt op het dashboard ook de gesimuleerde telemetrie van het nieuwe apparaat bekijken:
   
    ![Telemetrie voor nieuw apparaat weergeven][img-runningnew-2]

## <a name="device-properties"></a>Apparaateigenschappen
De vooraf geconfigureerde oplossing voor externe bewaking gebruikt [apparaatdubbels][lnk-device-twin] om apparaatmetagegevens tussen apparaten en de back-end van de oplossing te synchroniseren. Een apparaatdubbel is een JSON-document dat is opgeslagen in IoT Hub waarmee eigenschapswaarden worden opgeslagen voor een individueel apparaat. Apparaten verzenden regelmatig metagegevens als *gerapporteerde eigenschappen* naar de back-end van de oplossing om in de apparaatdubbel op te slaan. De back-end van de oplossing kan *gewenste eigenschappen* instellen in de apparaatdubbel om metagegevensupdates te verzenden naar apparaten. De gerapporteerde eigenschappen geven de meest recente waarden voor metagegevens die zijn verzonden door het apparaat. Zie [Register voor apparaat-id's, apparaatdubbel en DocumentDB][lnk-devicemetadata] voor meer informatie.

> [!NOTE]
> De oplossing gebruikt ook een DocumentDB-database voor het opslaan van apparaatspecifieke gegevens met betrekking tot opdrachten en methoden.
> 
> 

1. Ga terug naar de lijst met apparaten.
2. Selecteer het nieuwe apparaat in de **apparatenlijst** en klik daarna op **Bewerken** om **Apparaatdubbel - Gewenste eigenschappen** te bewerken:
   
   ![Gewenste eigenschappen van apparaat bewerken][img-editdevice]
3. Stel de **Naam van de gewenste eigenschap** in op **Breedtegraad** en stel de waarde in op **47.639521**. Klik daarna op **Wijzigingen aan het apparaatregister opslaan**:
   
    ![Gewenste eigenschappen van apparaat bewerken][img-editdevice2]
4. In het deelvenster **Apparaatdetails** wordt de nieuwe breedtegraadwaarde eerst weergegeven als een gewenste eigenschap en de oude breedtegraadwaarde als een gerapporteerde eigenschap:
   
    ![Gerapporteerde eigenschap weergeven][img-editdevice3]
5. Op dit moment kunnen gesimuleerde apparaten in de vooraf geconfigureerde oplossing alleen de gewenste eigenschappen **Desired.Config.TemperatureMeanValue** en **Desired.Config.TelemetryInterval** verwerken. Een echt apparaat zou alle gewenste eigenschappen van de IoT Hub moeten lezen, wijzigingen moeten aanbrengen in de configuratie en de nieuwe waarden als gerapporteerde eigenschappen moeten rapporteren aan de hub.

U kunt in het deelvenster **Apparaatdetails** ook **Apparaatdubbel - Tags** op dezelfde manier bewerken als waarop u **Apparaatdubbel - Gewenste eigenschappen** bewerkt. In tegenstelling tot de gewenste eigenschappen worden tags echter niet met het apparaat gesynchroniseerd. Tags bestaan alleen in de apparaatdubbel in de IoT Hub. Tags zijn handig voor het bouwen van aangepaste filters in de apparatenlijst.

## <a name="sort-the-device-list"></a>De apparatenlijst sorteren

U kunt de apparatenlijst sorteren door naast een kolomkop te klikken. De eerste klik wordt gesorteerd in oplopende volgorde, de tweede in aflopende volgorde:

![Apparatenlijst sorteren][img-sortdevices]

## <a name="filter-the-device-list"></a>De apparatenlijst filteren

In de apparatenlijst kunt u filters maken, opslaan en opnieuw laden om een aangepaste lijst met apparaten die zijn verbonden met uw hub weer te geven. Een filter maken:

1. Klik op het pictogram Filter bewerken boven de apparatenlijst:
   
   ![Open de filtereditor][img-editfiltericon]
2. Voeg in de **Filtereditor** de velden, operators en waarden toe om de apparatenlijst te filteren. U kunt meerdere clausules toevoegen om uw filter te verfijnen. Klik op **Filteren** om het filter toe te passen:
   
   ![Een filter maken][img-filtereditor]
3. In dit voorbeeld wordt de lijst gefilterd op fabrikant en modelnummer:
   
   ![Gefilterde lijst][img-filterelist]
4. Als u uw filter met een aangepaste naam wilt opslaan, klikt u op het pictogram **Opslaan als**:
   
   ![Een filter opslaan][img-savefilter]
5. Als u een filter dat u eerder hebt opgeslagen opnieuw wilt toepassen, klikt u op het pictogram **Opgeslagen filter openen**:
   
   ![Een filter openen][img-openfilter]

U kunt filters maken op basis van apparaat-id, apparaatstatus gewenste eigenschappen, gerapporteerde eigenschappen en tags.

> [!NOTE]
> U kunt de **weergave Geavanceerd** in de **Filtereditor** gebruiken om de querytekst rechtstreeks te bewerken.
> 
> 

## <a name="add-a-rule-for-the-new-device"></a>Een regel voor het nieuwe apparaat toevoegen
Er zijn geen regels voor het nieuwe apparaat dat u zojuist hebt toegevoegd. In deze sectie voegt u een regel toe die een waarschuwing activeert wanneer de door het nieuwe apparaat gemelde temperatuur 47 graden overschrijdt. Voordat u begint, kunt u al zien dat de telemetriegeschiedenis voor het nieuwe apparaat op het dashboard aantoont dat de temperatuur van het apparaat nooit meer dan 45 graden bedraagt.

1. Ga terug naar de lijst met apparaten.
2. Selecteer het nieuwe apparaat in de **apparatenlijst** en klik daarna op **Regel toevoegen** om een regel voor het apparaat toe te voegen.
3. Maak een regel die **Temperatuur** als gegevensveld gebruikt en **AlarmTemp** gebruikt als de uitvoer wanneer de temperatuur 47 graden overschrijdt:
   
    ![Een apparaatregel toevoegen][img-adddevicerule]
4. Klik op **Regels opslaan en weergeven** om uw wijzigingen op te slaan.
5. Klik op **Opdrachten** in het deelvenster Apparaatdetails voor het nieuwe apparaat.
   
   ![Een apparaatregel toevoegen][img-adddevicerule2]
6. Selecteer **ChangeSetPointTemp** in de opdrachtlijst en stel **SetPointTemp** in op 45. Klik daarna op **Opdracht verzenden**:
   
   ![Een apparaatregel toevoegen][img-adddevicerule3]
7. Ga terug naar het dashboard van de oplossing. Na korte tijd verschijnt in het deelvenster **Geschiedenis van waarschuwingen** een nieuwe vermelding wanneer de door het nieuwe apparaat gemelde temperatuur 47 graden overschrijdt:
   
   ![Een apparaatregel toevoegen][img-adddevicerule4]
8. Op de pagina **Regels** van het dashboard kunt u al uw regels bekijken en bewerken:
   
    ![Lijst met apparaatregels][img-rules]
9. Op de pagina **Acties** van het dashboard kunt u alle acties die worden uitgevoerd in reactie op een regel bekijken en bewerken:
   
    ![Lijst met apparaatacties][img-actions]

> [!NOTE]
> Het is mogelijk om acties te definiëren die een e-mailbericht of sms kunnen verzenden in antwoord op een regel of die met een Line-Of-Business-systeem kunnen worden geïntegreerd via een [logische app][lnk-logic-apps]. Zie [De logische app koppelen aan uw vooraf geconfigureerde oplossing Azure IoT Suite Remote Monitoring ][lnk-logicapptutorial].
> 
> 

## <a name="disable-and-remove-devices"></a>Apparaten uitschakelen en verwijderen
U kunt een apparaat uitschakelen en nadat het is uitgeschakeld kunt u het verwijderen:

![Een apparaat uitschakelen en verwijderen][img-disable]

## <a name="run-jobs"></a>Taken uitvoeren
U kunt taken plannen om bulksgewijs bewerkingen uit te voeren op uw apparaten. U maakt een taak voor een lijst met apparaten. Deze lijst kan al uw apparaten bevatten of kan een gefilterde lijst zijn die u hebt gemaakt met de [filterhulpprogramma's](#filter-the-device-list) in de apparatenlijst. Een taak kan een methode aanroepen voor elk apparaat in de lijst of de apparaatdubbel voor elk apparaat in de apparatenlijst bijwerken.

### <a name="create-a-job-to-invoke-a-method"></a>Een taak maken voor het aanroepen van een methode

De volgende stappen laten zien hoe u een taak maakt die de updatemethode voor de firmware aanroept op elk apparaat in een lijst. De methode wordt alleen aangeroepen op apparaten die de methode ondersteunen:

1. Gebruik de filterhulpprogramma's voor de apparatenlijst om een lijst met apparaten te maken voor het ontvangen van de firmware-update:
   
   ![Open de filtereditor][img-editfiltericon]
2. Klik op **Taakplanner** op uw gefilterde lijst:
   
   ![Open de Taakplanner][img-clickjobscheduler]
3. Klik in het deelvenster **Taak plannen** op **Aanroepmethode**.
4. Voer op de pagina **Aanroepmethode** vervolgens de gegevens van de aanroepmethode in en klik op **Plannen**:
   
   ![Methodetaak configureren][img-invokemethodjob]

De methode **InitiateFirmwareUpdate** start asynchroon een taak op het apparaat en retourneert onmiddellijk. Het firmware-updateproces gebruikt gerapporteerde eigenschappen voor het rapporteren over het updateproces terwijl het wordt uitgevoerd.

### <a name="create-a-job-to-edit-the-device-twin"></a>Een taak maken voor het bewerken van de apparaatdubbel

In de volgende stappen ziet u hoe u een taak maakt die de apparaatdubbel bewerkt op elk apparaat in een lijst:

1. Gebruik de filterhulpprogramma's voor de apparatenlijst om een lijst met apparaten te maken voor het ontvangen van bewerkingen voor apparaatdubbels:
   
   ![Open de filtereditor][img-editfiltericon]
2. Klik op **Taakplanner** op uw gefilterde lijst:
   
   ![Open de Taakplanner][img-clickjobscheduler]
3. Klik in het deelvenster **Taak plannen** op **Apparaatdubbel bewerken**.
4. Voer vervolgens op de pagina **Apparaatdubbel bewerken** de gegevens in van de **Gewenste eigenschappen** en **Tags** die u wilt bewerken en klik vervolgens op **Plannen**:
   
   ![Methodetaak configureren][img-edittwinjob]

### <a name="monitor-the-job"></a>De taak bewaken
U kunt de status bewaken van de taken die u plant op de pagina **Beheertaken**. Het deelvenster **Taakdetails** geeft informatie weer over de geselecteerde taak:
   
   ![Taakstatus weergeven][img-jobstatus]

U kunt ook informatie over taken weergeven op het **Dashboard**:
   
   ![Taken weergeven op het dashboard][img-jobdashboard]


## <a name="behind-the-scenes"></a>Achter de schermen
Wanneer u een vooraf geconfigureerde oplossing implementeert, maakt het implementatieproces meerdere resources in het door u geselecteerde Azure-abonnement. U kunt deze resources weergeven in Azure [Portal][lnk-portal]. Het implementatieproces maakt een **resourcegroep** met een naam die is gebaseerd op de naam die u voor uw vooraf geconfigureerde oplossing hebt gekozen:

![Vooraf geconfigureerde oplossing in de Azure-portal][img-portal]

U kunt de instellingen van elke resource weergeven door deze te selecteren in de lijst met resources in de resourcegroep.

U kunt ook de broncode voor de vooraf geconfigureerde oplossing weergeven. De broncode van de vooraf geconfigureerde oplossing voor externe bewaking bevindt zich in de GitHub-opslagplaats [azure-iot-remote-monitoring][lnk-rmgithub]:

* De map **DeviceAdministration** bevat de broncode voor het dashboard.
* De map **Simulator** bevat de broncode voor het gesimuleerde apparaat.
* De map **EventProcessor** bevat de broncode voor het back-endproces dat de binnenkomende telemetrie verwerkt.

Wanneer u klaar bent, kunt u de vooraf geconfigureerde oplossing verwijderen uit uw Azure-abonnement op de site [azureiotsuite.com][lnk-azureiotsuite]. Met de site kunt u gemakkelijk resources verwijderen die werden aangevoerd toen u de vooraf geconfigureerde oplossing hebt gemaakt.

> [!NOTE]
> Verwijder de resourcegroep niet uit de portal, maar verwijder de oplossing op de site [azureiotsuite.com][lnk-azureiotsuite]. Zo zorgt u ervoor dat alles met betrekking tot de vooraf geconfigureerde oplossing wordt verwijderd.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Nu u een werkende vooraf geconfigureerde oplossing hebt geïmplementeerd, kunt u doorgaan met IoT Suite door de volgende artikels te lezen:

* [Walkthrough over de vooraf geconfigureerde oplossing voor externe bewaking][lnk-rm-walkthrough]
* [Uw apparaat koppelen aan de vooraf geconfigureerde oplossing voor externe bewaking][lnk-connect-rm]
* [Machtigingen op de site azureiotsuite.com][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-devicemethods]: media/iot-suite-getstarted-preconfigured-solutions/devicemethods.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-pingmethod]: media/iot-suite-getstarted-preconfigured-solutions/pingmethod.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png
[img-columneditor]: media/iot-suite-getstarted-preconfigured-solutions/columneditor.png
[img-startimageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit1.png
[img-imageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit2.png
[img-sortdevices]: media/iot-suite-getstarted-preconfigured-solutions/sortdevices.png
[img-editfiltericon]: media/iot-suite-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-getstarted-preconfigured-solutions/openfilter.png
[img-clickjobscheduler]: media/iot-suite-getstarted-preconfigured-solutions/clickscheduler.png
[img-invokemethodjob]: media/iot-suite-getstarted-preconfigured-solutions/invokemethodjob.png
[img-edittwinjob]: media/iot-suite-getstarted-preconfigured-solutions/edittwinjob.png
[img-jobstatus]: media/iot-suite-getstarted-preconfigured-solutions/jobstatus.png
[img-jobdashboard]: media/iot-suite-getstarted-preconfigured-solutions/jobdashboard.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-device-twin-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
