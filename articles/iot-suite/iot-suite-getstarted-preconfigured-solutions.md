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
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 466825ab78a5ac9773d8beff69cca90ff9db6c01
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-the-preconfigured-solutions"></a>Aan de slag met vooraf geconfigureerde oplossingen

[Vooraf geconfigureerde oplossingen][lnk-preconfigured-solutions] voor Azure IoT Suite zijn voorzien van meerdere Azure IoT-services om totaaloplossingen te leveren voor het implementeren van algemene IoT-bedrijfsscenario's. De vooraf geconfigureerde oplossing *externe controle* maakt verbinding met en controleert uw apparaten. U kunt deze oplossing gebruiken om de datastroom van uw apparaten te analyseren en de bedrijfsresultaten te verbeteren door processen automatisch te laten reageren op die datastroom.

In deze zelfstudie leert u hoe de vooraf geconfigureerde oplossing voor externe controle inricht. Hierbij maakt u ook kennis met de basisfuncties van de vooraf geconfigureerde oplossing. U hebt toegang tot veel van deze functies via het *oplossingsdashboard* dat als onderdeel van de vooraf geconfigureerde oplossing wordt geïmplementeerd:

![Vooraf geconfigureerd oplossingsdashboard voor externe controle][img-dashboard]

U hebt een actief Azure-abonnement nodig om deze zelfstudie te voltooien.

> [!NOTE]
> Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure][lnk_free_trial] voor meer informatie.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="scenario-overview"></a>Overzicht van scenario's

Wanneer u de vooraf geconfigureerde oplossing voor externe bewaking implementeert, wordt deze vooraf ingevuld met de resources waarmee u een algemeen scenario voor externe controle kunt doorlopen. In dit scenario melden verschillende apparaten die met de oplossing zijn verbonden, onverwachte temperatuurwaarden. De volgende gedeelten laten u zien hoe u:

* De apparaten die de onverwachte temperatuurwaarden melden, kunt identificeren.
* Deze apparaten kunt configureren, zodat ze gedetailleerdere telemetrie verzenden.
* Het probleem kunt oplossen door de firmware op deze apparaten bij te werken.
* Kunt controleren of uw actie het probleem heeft opgelost.

Een belangrijke functie van dit scenario is dat u al deze acties extern kunt uitvoeren vanuit het dashboard van de oplossing. U hebt geen fysieke toegang tot de apparaten nodig.

## <a name="view-the-solution-dashboard"></a>Het oplossingsdashboard bekijken

Vanaf het dashboard van de oplossing kunt u de geïmplementeerde oplossing beheren. U kunt er bijvoorbeeld telemetrie weergeven, apparaten toevoegen en regels configureren.

1. Wanneer het inrichten is voltooid en voor de tegel voor uw vooraf geconfigureerde oplossing de status **Gereed** wordt weergegeven, kiest u **Starten** om uw oplossingsportal voor externe controle te openen op een nieuw tabblad.

    ![De vooraf geconfigureerde oplossing starten][img-launch-solution]

1. Standaard ziet u in de oplossingsportal het *dashboard*. U kunt navigeren naar andere gebieden van de portal van de oplossing met het menu aan de linkerkant van de pagina.

    ![Vooraf geconfigureerd oplossingsdashboard voor externe controle][img-menu]

Het dashboard bevat de volgende informatie:

* Een kaart die de locatie aangeeft van elk apparaat dat met de oplossing is verbonden. Wanneer u de oplossing voor het eerst uitvoert, zijn er 25 gesimuleerde apparaten. De gesimuleerde apparaten worden geïmplementeerd als Azure WebJobs en de oplossing gebruikt de API van Bing Kaarten om informatie op de kaart te tekenen. Zie de [Veelgestelde vragen][lnk-faq] voor meer informatie over hoe u de kaart dynamisch kunt maken.
* Een deelvenster **Telemetriegeschiedenis** dat de vochtigheids- en temperatuurtelemetrie van een geselecteerd apparaat in bijna realtime tekent en statistische gegevens weergeeft, zoals de maximale, minimale en gemiddelde vochtigheid.
* Een deelvenster **Geschiedenis van waarschuwingen** dat recente waarschuwingsgebeurtenissen toont wanneer voor een telemetriewaarde een drempelwaarde wordt overschreden. Naast de voorbeelden die met de vooraf geconfigureerde oplossing zijn gemaakt, kunt u ook uw eigen alarmen definiëren.
* Een deelvenster **Taken** dat informatie weergeeft over geplande taken. U kunt uw eigen taken plannen op de pagina **Beheertaken**.

## <a name="view-alarms"></a>Waarschuwingen weergeven

Het deelvenster Waarschuwingsgeschiedenis laat u zien dat vijf apparaten telemetriewaarden melden die hoger zijn dan verwacht.

![Waarschuwingsgeschiedenistaken op het dashboard van de oplossing][img-alarms]

> [!NOTE]
> Deze waarschuwingen worden gegenereerd door een regel die is opgenomen in de vooraf geconfigureerde oplossing. Deze regel genereert een waarschuwing als de temperatuurwaarde die door een apparaat is verzonden, groter is dan 60. U kunt uw eigen regels en acties definiëren door [Regels](#add-a-rule) en [Acties](#add-an-action) te kiezen in het menu aan de linkerzijde.

## <a name="view-devices"></a>Apparaten weergeven

De *Apparatenlijst* bevat alle geregistreerde apparaten in de oplossing. Uit de apparatenlijst kunt u metagegevens voor apparaten weergeven en bewerken, apparaten toevoegen of verwijderen en methoden aanroepen op apparaten. U kunt de apparatenlijst filteren en sorteren. U kunt ook de weergave van de kolommen in de apparatenlijst aanpassen.

1. Kies **Apparaten** om de apparatenlijst voor deze oplossing weer te geven.

   ![De apparatenlijst weergeven in de portal van de oplossing][img-devicelist]

1. De apparatenlijst geeft eerst aan dat bij het inrichtingsproces 25 gesimuleerde apparaten zijn gemaakt. U kunt extra gesimuleerde en fysieke apparaten aan de oplossing toevoegen.

1. Kies in de lijst een apparaat om de details ervan weer te geven.

   ![De details van een apparaat weergeven in de portal van de oplossing][img-devicedetails]

Het deelvenster **Apparaatdetails** bevat zes gedeeltes:

* Een verzameling van koppelingen waarmee u het apparaatpictogram kunt aanpassen, het apparaat kunt uitschakelen, een regel kunt toevoegen, een methode kunt aanroepen of een opdracht kunt verzenden. Zie [Cloud-to-device communications guidance][lnk-c2d-guidance] (Richtlijnen voor communicatie tussen cloud en apparaat) voor een vergelijking van opdrachten (berichten van apparaat naar cloud) en methoden (directe methoden).
* In het gedeelte **Apparaatdubbel - Tags** kunt u tagwaarden voor het apparaat bewerken. U kunt tagwaarden in de apparatenlijst weergeven en tagwaarden gebruiken om de apparatenlijst te filteren.
* In het gedeelte **Apparaatdubbel - Gewenste eigenschappen** kunt u eigenschapswaarden instellen die moeten worden verzonden naar het apparaat.
* Het gedeelte **Apparaatdubbel - Gerapporteerde eigenschappen** geeft eigenschapswaarden weer die vanaf het apparaat worden verzonden.
* Het gedeelte **Apparaateigenschappen** geeft informatie weer vanuit het identiteitsregister, zoals het apparaat-id en de verificatiesleutels.
* Het gedeelte **Recente taken** geeft informatie weer over taken die onlangs op dit apparaat zijn gericht.

## <a name="filter-the-device-list"></a>De apparatenlijst filteren

U kunt een filter gebruiken om alleen de apparaten weer te geven die onverwachte temperatuurwaarden verzenden. De vooraf geconfigureerde oplossing voor externe bewaking bevat het filter **Beschadigde apparaten** om apparaten weer te geven met een gemiddelde temperatuurwaarde die groter is dan 60. U kunt ook [uw eigen filters maken](#add-a-filter).

1. Kies **Opgeslagen filter openen** om een lijst met beschikbare filters weer te geven. Kies vervolgens **Beschadigde apparaten** om het filter toe te passen:

    ![De lijst met filters weergeven][img-unhealthy-filter]

1. De apparatenlijst geeft nu alleen apparaten weer met een gemiddelde temperatuurwaarde die groter is dan 60.

    ![De gefilterde apparatenlijst met beschadigde apparaten weergeven][img-filtered-unhealthy-list]

## <a name="update-desired-properties"></a>Gewenste eigenschappen bijwerken

U hebt nu een set apparaten geïdentificeerd die mogelijk moeten worden hersteld. U besluit echter dat de gegevensfrequentie van 15 seconden niet voldoende is voor een duidelijke diagnose van het probleem. Als u de telemetrische frequentie wijzigt in vijf seconden, hebt u meer gegevenspunten waarmee u het probleem beter kunt vaststellen. U kunt deze configuratiewijziging pushen naar uw externe apparaten vanuit de portal van de oplossing. U kunt de wijziging eenmaal doorvoeren, de impact controleren en vervolgens actie ondernemen op de resultaten.

Volg deze stappen voor het uitvoeren van een taak die de gewenste eigenschap **TelemetryInterval** voor de betreffende apparaten wijzigt. Wanneer de apparaten de nieuwe eigenschapswaarde **TelemetryInterval** ontvangen, wijzigen ze hun configuratie dusdanig dat er elke vijf seconden telemetrie wordt verzonden, in plaats van elke 15 seconden:

1. Wanneer de lijst met beschadigde apparaten wordt weergegeven in de lijst met apparaten, kiest u **Taakplanner** en vervolgens **Apparaatdubbel bewerken**.

1. Roep de taak **Telemetrie-interval wijzigen** aan.

1. Wijzig de waarde van de **Gewenste eigenschap** met de naam **desired.Config.TelemetryInterval** in vijf seconden.

1. Kies **Planning**.

    ![De eigenschap TelemetryInterval in vijf seconden wijzigen][img-change-interval]

1. U kunt de voortgang bewaken van de taken op de pagina **Beheertaken** in de portal.

> [!NOTE]
> Als u een gewenste eigenschapswaarde voor een afzonderlijk apparaat wilt wijzigen, gebruikt u het gedeelte **Gewenste eigenschappen** in het deelvenster **Apparaatdetails** in plaats van een taak.

Deze taak stelt de waarde in van de gewenste eigenschap **TelemetryInterval** in het dubbele apparaat voor alle apparaten die door het filter zijn geselecteerd. De apparaten halen deze waarde op van de dubbele apparaten en werken hun gedrag bij. Wanneer een apparaat een gewenste eigenschap ophaalt uit een dubbel apparaat, wordt de bijbehorende gerapporteerde eigenschapswaarde ingesteld.

## <a name="invoke-methods"></a>Aanroepmethodes

Terwijl de taak wordt uitgevoerd, ziet u in de lijst met beschadigde apparaten dat al deze apparaten oude firmwareversies hebben (ouder dan versie 1.6).

![De gemelde firmwareversie voor beschadigde apparaten weergeven][img-old-firmware]

Deze firmwareversie is mogelijk de basisoorzaak voor de onverwachte temperatuurwaarden als u weet dat andere apparaten die wel goed functioneren, onlangs zijn bijgewerkt naar versie 2.0. U kunt het ingebouwde filter **Apparaten met oude firmware** gebruiken om apparaten met oude firmwareversies te identificeren. Via de portal kunt u vervolgens alle apparaten die nog een oude firmwareversie hebben, extern bijwerken:

1. Kies **Opgeslagen filter openen** om een lijst met beschikbare filters weer te geven. Kies vervolgens **Apparaten met oude firmwareversies** om het filter toe te passen:

    ![De lijst met filters weergeven][img-old-filter]

1. In de lijst met apparaten worden nu alleen apparaten met oude firmwareversies weergegeven. Deze lijst bevat de vijf apparaten die zijn geïdentificeerd door het filter **Beschadigde apparaten** en drie extra apparaten:

    ![De gefilterde apparatenlijst met oude apparaten weergeven][img-filtered-old-list]

1. Kies **Job Scheduler** en vervolgens **Aanroepmethode**.

1. Stel **Taaknaam** in op **Firmware bijwerken naar versie 2.0**.

1. Kies **InitiateFirmwareUpdate** als de **Methode**.

1. Stel de parameter **FwPackageUri** in op **https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.

1. Kies **Planning**. De standaardwaarde voor de taak is nu uitvoeren.

    ![Taak voor het bijwerken van de firmware van de geselecteerde apparaten maken][img-method-update]

> [!NOTE]
> Als u een aanroepmethode voor een bepaald apparaat wilt gebruiken, kiest u **Methoden** in het deelvenster **Apparaatdetails** in plaats van dat u de taak uitvoert.

Deze taak roept de directe methode **InitiateFirmwareUpdate** aan op alle apparaten die door het filter zijn geselecteerd. Apparaten reageren onmiddellijk op IoT Hub en starten het updateproces voor de firmware asynchroon. De apparaten bieden statusinformatie over het updateproces voor de firmware via gemelde eigenschapswaarden, zoals wordt weergegeven in de volgende schermafbeeldingen. Kies het pictogram **Vernieuwen** om de informatie in de apparaat- en takenlijsten bij te werken:

![Taaklijst waarin de lijst met firmware die wordt bijgewerkt, wordt weergegeven][img-update-1]
![Apparatenlijst met de status van de firmware-update][img-update-2]
![Takenlijst met voltooide firmware-updates][img-update-3]

> [!NOTE]
> U kunt taken volgens planning laten uitvoeren tijdens aangewezen onderhoudsperioden in een productieomgeving.

## <a name="scenario-review"></a>Samenvatting van scenario

In dit scenario hebt u een mogelijk probleem met enkele van uw externe apparaten geïdentificeerd met behulp van de waarschuwingsgeschiedenis op het dashboard en een filter. Vervolgens hebt u het filter en een taak gebruikt om de apparaten extern te configureren om meer informatie te geven teneinde u te helpen bij het vaststellen van het probleem. Ten slotte hebt u een filter en een taak gebruikt om onderhoud op de beschadigde apparaten te plannen. Als u naar het dashboard terugkeert, kunt u zien dat er geen waarschuwingen meer worden weergegeven voor apparaten in uw oplossing. U kunt een filter gebruiken om te controleren of de firmware op alle apparaten in uw oplossing up-to-date is en er geen beschadigde apparaten meer zijn:

![Filter dat laat zien dat alle apparaten bijgewerkte firmware hebben][img-updated]

![Filter dat laat zien dat alle apparaten in orde zijn][img-healthy]

## <a name="other-features"></a>Andere functies

De volgende gedeelten beschrijven een aantal extra functies van de vooraf geconfigureerde oplossing voor externe controle die niet zijn beschreven als onderdeel van het voorgaande scenario.

### <a name="customize-columns"></a>Kolommen aanpassen

U kunt de informatie die in de apparatenlijst wordt weergegeven, aanpassen door **Kolomeditor** te kiezen. U kunt kolommen die gerapporteerde eigenschaps- en tagwaarden weergeven, toevoegen en verwijderen. U kunt ook de volgorde en namen van de kolommen wijzigen:

   ![Kolomeditor in de apparatenlijst][img-columneditor]

### <a name="customize-the-device-icon"></a>Het apparaatpictogram aanpassen

U kunt als volgt vanuit het deelvenster **Apparaatdetails** het apparaatpictogram aanpassen dat wordt weergegeven in de apparatenlijst:

1. Kies het potloodpictogram om het deelvenster **Installatiekopie bewerken** voor een apparaat te openen:

   ![De afbeeldingseditor van het apparaat openen][img-startimageedit]

1. Upload een nieuwe installatiekopie of gebruik een van de bestaande installatiekopieën en klik vervolgens op **Opslaan**:

   ![De afbeelding van het apparaat bewerken in de afbeeldingseditor][img-imageedit]

1. De geselecteerde afbeelding wordt nu weergegeven in de kolom **Pictogram** voor het apparaat.

> [!NOTE]
> De afbeelding wordt opgeslagen in Blob Storage. Een tag in de apparaatdubbel bevat een koppeling naar de afbeelding in Blob Storage.

### <a name="add-a-device"></a>Een apparaat toevoegen

Wanneer u de vooraf geconfigureerde oplossing implementeert, richt u automatisch de 25 proefapparaten in die u in de apparatenlijst ziet. Deze apparaten zijn *gesimuleerde apparaten* uitgevoerd in een Azure WebJob. Gesimuleerde apparaten maken het voor u gemakkelijk om te experimenteren met een vooraf geconfigureerde oplossing zonder echte, fysieke apparaten te moeten implementeren. Raadpleeg de zelfstudie [Uw apparaat koppelen aan de vooraf geconfigureerde oplossing voor externe bewaking][lnk-connect-rm] als u een echt apparaat op de oplossing wilt aansluiten.

De volgende stappen laten zien hoe u een gesimuleerd apparaat toevoegt aan de oplossing:

1. Ga terug naar de lijst met apparaten.

1. Kies linksonder **+ Een apparaat toevoegen** om een nieuw apparaat toe te voegen.

   ![Een apparaat toevoegen aan de vooraf geconfigureerde oplossing][img-adddevice]

1. Kies **Nieuwe toevoegen** op de tegel **Gesimuleerd apparaat**.

   ![Nieuwe apparaatgegevens in het dashboard instellen][img-addnew]

   U kunt niet alleen een nieuw gesimuleerd apparaat maken, maar ook een fysiek apparaat toevoegen als u ervoor kiest om een **aangepast apparaat** te maken. Zie [Uw apparaat aansluiten op de vooraf geconfigureerde IoT Suite-oplossing voor externe bewaking][lnk-connect-rm] voor meer informatie over het koppelen van fysieke apparaten aan de oplossing.

1. Selecteer **Laat mij mijn eigen apparaat-id definiëren** en voer de unieke id van een apparaatnaam in, zoals **mijnapparaat_01**.

1. Kies **Maken**.

   ![Een nieuw apparaat opslaan][img-definedevice]

1. Kies in stap 3 van **Een gesimuleerd apparaat toevoegen** de optie **Gereed** om terug te keren naar de apparatenlijst.

1. In de lijst met apparaten kunt u zien dat uw apparaat **wordt uitgevoerd**.

    ![Nieuw apparaat weergeven in de apparatenlijst][img-runningnew]

1. U kunt op het dashboard ook de gesimuleerde telemetrie van het nieuwe apparaat bekijken:

    ![Telemetrie voor nieuw apparaat weergeven][img-runningnew-2]

### <a name="disable-and-delete-a-device"></a>Een apparaat uitschakelen en verwijderen

U kunt een apparaat uitschakelen en nadat het is uitgeschakeld kunt u het verwijderen:

![Een apparaat uitschakelen en verwijderen][img-disable]

### <a name="add-a-rule"></a>Een regel toevoegen

Er zijn geen regels voor het nieuwe apparaat dat u zojuist hebt toegevoegd. In deze sectie voegt u een regel toe die een waarschuwing activeert wanneer de door het nieuwe apparaat gemelde temperatuur 47 graden overschrijdt. Voordat u begint, kunt u al zien dat de telemetriegeschiedenis voor het nieuwe apparaat op het dashboard aantoont dat de temperatuur van het apparaat nooit meer dan 45 graden bedraagt.

1. Ga terug naar de lijst met apparaten.

1. Selecteer het nieuwe apparaat in de **apparatenlijst** en kies daarna **Regel toevoegen** om een regel voor het apparaat toe te voegen.

1. Maak een regel die **Temperatuur** als gegevensveld gebruikt en **AlarmTemp** gebruikt als de uitvoer wanneer de temperatuur 47 graden overschrijdt:

    ![Een apparaatregel toevoegen][img-adddevicerule]

1. Kies **Regels opslaan en weergeven** om uw wijzigingen op te slaan.

1. Kies **Opdrachten** in het deelvenster Apparaatdetails voor het nieuwe apparaat.

    ![Een apparaatregel toevoegen][img-adddevicerule2]

1. Selecteer **ChangeSetPointTemp** in de opdrachtlijst en stel **SetPointTemp** in op 45. Kies vervolgens **Opdracht verzenden**:

    ![Een apparaatregel toevoegen][img-adddevicerule3]

1. Ga terug naar het dashboard. Na korte tijd verschijnt in het deelvenster **Geschiedenis van waarschuwingen** een nieuwe vermelding wanneer de door het nieuwe apparaat gemelde temperatuur 47 graden overschrijdt:

    ![Een apparaatregel toevoegen][img-adddevicerule4]

1. Op de pagina **Regels** van het dashboard kunt u al uw regels bekijken en bewerken:

    ![Lijst met apparaatregels][img-rules]

1. Op de pagina **Acties** van het dashboard kunt u alle acties die worden uitgevoerd in reactie op een regel bekijken en bewerken:

    ![Lijst met apparaatacties][img-actions]

> [!NOTE]
> Het is mogelijk om acties te definiëren die een e-mailbericht of sms kunnen verzenden in antwoord op een regel of die met een Line-Of-Business-systeem kunnen worden geïntegreerd via een [logische app][lnk-logic-apps]. Zie [De logische app koppelen aan uw vooraf geconfigureerde oplossing Azure IoT Suite Remote Monitoring ][lnk-logicapptutorial].

### <a name="manage-filters"></a>Filters beheren

In de apparatenlijst kunt u filters maken, opslaan en opnieuw laden om een aangepaste lijst met apparaten die zijn verbonden met uw hub weer te geven. Een filter maken:

1. Kies het pictogram Filter bewerken boven de apparatenlijst:

    ![Open de filtereditor][img-editfiltericon]

1. Voeg in de **Filtereditor** de velden, operators en waarden toe om de apparatenlijst te filteren. U kunt meerdere clausules toevoegen om uw filter te verfijnen. Kies **Filteren** om het filter toe te passen:

    ![Een filter maken][img-filtereditor]

1. In dit voorbeeld wordt de lijst gefilterd op fabrikant en modelnummer:

    ![Gefilterde lijst][img-filterelist]

1. Als u uw filter met een aangepaste naam wilt opslaan, kiest u het pictogram **Opslaan als**:

    ![Een filter opslaan][img-savefilter]

1. Als u een eerder opgeslagen filter opnieuw wilt toepassen, kiest u het pictogram **Opgeslagen filter openen**:

    ![Een filter openen][img-openfilter]

U kunt filters maken op basis van apparaat-id, apparaatstatus gewenste eigenschappen, gerapporteerde eigenschappen en tags. U kunt uw eigen aangepaste tags toevoegen aan een apparaat in het gedeelte **Tags** van het deelvenster **Apparaatdetails** of een taak voor het bijwerken van tags op meerdere apparaten uitvoeren.

> [!NOTE]
> U kunt de **weergave Geavanceerd** in de **Filtereditor** gebruiken om de querytekst rechtstreeks te bewerken.

### <a name="commands"></a>Opdrachten

Vanuit het deelvenster **Apparaatdetails** kunt u opdrachten naar het apparaat verzenden. Wanneer een apparaat voor het eerst wordt gestart, stuurt het naar de oplossing informatie over de opdrachten die het apparaat ondersteunt. Zie [Opties van Azure IoT-Hub cloud-naar-apparaat][lnk-c2d-guidance] voor een beschrijving van de verschillen tussen opdrachten en methoden.

1. Kies in het deelvenster **Apparaatdetails** de optie **Opdrachten** voor het geselecteerde apparaat:

   ![Apparaatopdrachten in het dashboard][img-devicecommands]

1. Selecteer **PingDevice** in de lijst met opdrachten.

1. Kies **Opdracht verzenden**.

1. U ziet de status van de opdracht in de opdrachtgeschiedenis.

   ![Opdrachtstatus in het dashboard][img-pingcommand]

De oplossing houdt de status van elke opdracht bij die met de oplossing wordt verzonden. In eerste instantie is het resultaat **In behandeling**. Wanneer het apparaat meldt dat het de opdracht heeft uitgevoerd, wordt het resultaat ingesteld op **Geslaagd**.

## <a name="behind-the-scenes"></a>Achter de schermen

Wanneer u een vooraf geconfigureerde oplossing implementeert, maakt het implementatieproces meerdere resources in het door u geselecteerde Azure-abonnement. U kunt deze resources weergeven in Azure [Portal][lnk-portal]. Het implementatieproces maakt een **resourcegroep** met een naam die is gebaseerd op de naam die u voor uw vooraf geconfigureerde oplossing hebt gekozen:

![Vooraf geconfigureerde oplossing in Azure Portal][img-portal]

U kunt de instellingen van elke resource weergeven door deze te selecteren in de lijst met resources in de resourcegroep.

U kunt ook de broncode voor de vooraf geconfigureerde oplossing weergeven. De broncode van de vooraf geconfigureerde oplossing voor externe bewaking bevindt zich in de GitHub-opslagplaats [azure-iot-remote-monitoring][lnk-rmgithub]:

* De map **DeviceAdministration** bevat de broncode voor het dashboard.
* De map **Simulator** bevat de broncode voor het gesimuleerde apparaat.
* De map **EventProcessor** bevat de broncode voor het back-endproces dat de binnenkomende telemetrie verwerkt.

Wanneer u klaar bent, kunt u de vooraf geconfigureerde oplossing verwijderen uit uw Azure-abonnement op de site [azureiotsuite.com][lnk-azureiotsuite]. Met de site kunt u gemakkelijk resources verwijderen die werden aangevoerd toen u de vooraf geconfigureerde oplossing hebt gemaakt.

> [!NOTE]
> Verwijder de resourcegroep niet uit de portal, maar verwijder de oplossing op de site [azureiotsuite.com][lnk-azureiotsuite]. Zo zorgt u ervoor dat alles met betrekking tot de vooraf geconfigureerde oplossing wordt verwijderd.

## <a name="next-steps"></a>Volgende stappen

Nu u een werkende vooraf geconfigureerde oplossing hebt geïmplementeerd, kunt u doorgaan met IoT Suite door de volgende artikels te lezen:

* [Walkthrough over de vooraf geconfigureerde oplossing voor externe bewaking][lnk-rm-walkthrough]
* [Uw apparaat koppelen aan de vooraf geconfigureerde oplossing voor externe bewaking][lnk-connect-rm]
* [Machtigingen op de site azureiotsuite.com][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-menu]: media/iot-suite-getstarted-preconfigured-solutions/menu.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-alarms]: media/iot-suite-getstarted-preconfigured-solutions/alarms.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
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
[img-editfiltericon]: media/iot-suite-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-getstarted-preconfigured-solutions/openfilter.png
[img-unhealthy-filter]: media/iot-suite-getstarted-preconfigured-solutions/unhealthyfilter.png
[img-filtered-unhealthy-list]: media/iot-suite-getstarted-preconfigured-solutions/unhealthylist.png
[img-change-interval]: media/iot-suite-getstarted-preconfigured-solutions/changeinterval.png
[img-old-firmware]: media/iot-suite-getstarted-preconfigured-solutions/noticeold.png
[img-old-filter]: media/iot-suite-getstarted-preconfigured-solutions/oldfilter.png
[img-filtered-old-list]: media/iot-suite-getstarted-preconfigured-solutions/oldlist.png
[img-method-update]: media/iot-suite-getstarted-preconfigured-solutions/methodupdate.png
[img-update-1]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate1.png
[img-update-2]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate2.png
[img-update-3]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate3.png
[img-updated]: media/iot-suite-getstarted-preconfigured-solutions/updated.png
[img-healthy]: media/iot-suite-getstarted-preconfigured-solutions/healthy.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-faq]: iot-suite-faq.md