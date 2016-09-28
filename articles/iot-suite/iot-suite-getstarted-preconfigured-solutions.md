<properties
    pageTitle="Aan de slag met vooraf geconfigureerde oplossingen | Microsoft Azure"
    description="Volg deze zelfstudie voor meer informatie over het implementeren van een vooraf geconfigureerde Azure IoT Suite-oplossing."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>


# Zelfstudie: Aan de slag met vooraf geconfigureerde oplossingen

## Inleiding

[Vooraf geconfigureerde oplossingen][lnk-preconfigured-solutions] voor Azure IoT-Suite combineren meerdere Azure IoT-services om totaaloplossingen te leveren die algemene IoT-bedrijfsscenario's implementeren. De vooraf geconfigureerde oplossing *externe controle* maakt verbinding met en controleert uw apparaten. U kunt deze oplossing gebruiken om de datastroom van uw apparaten te analyseren en de bedrijfsresultaten te verbeteren door processen automatisch te laten reageren op die datastroom.

In deze zelfstudie leert u hoe de vooraf geconfigureerde oplossing voor externe controle inricht. Dit leidt u door de belangrijkste functies van de oplossing voor externe bewaking. U krijgt toegang tot deze functies via het oplossingsdashboard dat samen met de vooraf geconfigureerde oplossing het volgende implementeert:

![Vooraf geconfigureerd oplossingsdashboard voor externe controle][img-dashboard]

U hebt een actief Azure-abonnement nodig om deze zelfstudie te voltooien.

> [AZURE.NOTE]  Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure][lnk_free_trial] voor meer informatie.

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## Het oplossingsdashboard bekijken

Vanaf het dashboard van de oplossing kunt u de geïmplementeerde oplossing beheren. U kunt er bijvoorbeeld telemetrie weergeven, apparaten toevoegen en regels configureren.

1.  Wanneer het inrichten is voltooid en voor de tegel voor uw vooraf geconfigureerde oplossing de status **Gereed** wordt weergegeven, klikt u op **Starten** om uw oplossingsportal voor externe controle te openen in een nieuw tabblad.

    ![De vooraf geconfigureerde oplossing starten][img-launch-solution]

2.  Standaard ziet u in de oplossingsportal het *dashboard van de oplossing*. In het menu links kunt u andere weergaven selecteren.

    ![Vooraf geconfigureerd oplossingsdashboard voor externe controle][img-dashboard]

Het dashboard bevat de volgende informatie:

- De kaart toont de locatie van elk apparaat dat met de oplossing is verbonden. Wanneer u de oplossing voor het eerst uitvoert, zijn er vier gesimuleerde apparaten. De gesimuleerde apparaten worden geïmplementeerd als Azure WebJobs en de oplossing gebruikt de API van Bing Kaarten om informatie op de kaart te tekenen.
- Het deelvenster **Telemetriegeschiedenis** tekent de vochtigheids- en temperatuurtelemetrie van een geselecteerd apparaat in bijna realtime en geeft statistische gegevens weer, zoals de maximale, minimale en gemiddelde vochtigheid.
- Het deelvenster **Geschiedenis van waarschuwingen** toont recente waarschuwingsgebeurtenissen wanneer voor een telemetriewaarde een drempelwaarde wordt overschreden. Naast de voorbeelden die met de vooraf geconfigureerde oplossing zijn gemaakt, kunt u ook uw eigen alarmen definiëren.

## De lijst met apparaten weergeven

De lijst met apparaten bevat alle geregistreerde apparaten in de oplossing. U kunt metagegevens van apparaten weergeven en bewerken, apparaten toevoegen of verwijderen en opdrachten naar apparaten verzenden.

1.  Klik links in het menu op **Apparaten** om de *lijst met apparaten* voor deze oplossing weer te geven.

    ![Lijst met apparaten in het dashboard][img-devicelist]

2.  De lijst met apparaten geeft aan dat bij het inrichtingsproces vier gesimuleerde apparaten zijn gemaakt.

3.  Klik in de lijst op een apparaat om de details ervan weer te geven.

    ![Apparaatdetails in het dashboard][img-devicedetails]

Het deelvenster **Apparaatdetails** bevat drie secties:

- In de sectie **Acties** vindt u de acties die u voor het apparaat kunt uitvoeren. Als u het apparaat uitschakelt, kan dit geen telemetrie meer verzenden of opdrachten ontvangen. Als u een apparaat uitschakelt, kunt u het later opnieuw inschakelen. U kunt een regel toevoegen die is gekoppeld aan het apparaat dat een waarschuwing activeert wanneer voor een telemetriewaarde de drempelwaarde wordt overschreden. U kunt ook een opdracht verzenden naar een apparaat. Wanneer een apparaat voor het eerst verbinding maakt, geeft het aan de oplossing door op welke opdrachten deze kan reageren.
- In de sectie **Apparaateigenschappen** vindt u de metagegevens van apparaten. Sommige van deze metagegevens zijn afkomstig van het apparaat zelf (zoals de fabrikant). Enkele ervan worden gegenereerd door de oplossing (zoals de aanmaaktijd). U kunt de metagegevens van apparaten hier bewerken.
- In de sectie **Verificatiesleutels** vindt u de sleutels die het apparaat kan gebruiken om met de oplossing te verifiëren.

## Een opdracht naar een apparaat verzenden

Het deelvenster Apparaatdetails bevat alle opdrachten die een specifiek apparaat ondersteunt en biedt u de mogelijkheid om opdrachten naar een apparaat te verzenden. Wanneer een apparaat voor het eerst wordt gestart, stuurt het naar de oplossing informatie over de opdrachten die het apparaat ondersteunt.

1.  Klik in het deelvenster Apparaatdetails op **Opdrachten** voor het geselecteerde apparaat.

    ![Apparaatopdrachten in het dashboard][img-devicecommands]

2.  Selecteer **PingDevice** in de lijst met opdrachten.

3.  Klik op **Opdracht verzenden**.

4.  U ziet de status van de opdracht in de opdrachtgeschiedenis.

    ![Opdrachtstatus in het dashboard][img-pingcommand]

De oplossing houdt de status van elke opdracht bij die met de oplossing wordt verzonden. In eerste instantie is het resultaat **In behandeling**. Wanneer het apparaat meldt dat het de opdracht heeft uitgevoerd, wordt het resultaat ingesteld op **Geslaagd**.

## Een nieuw gesimuleerd apparaat toevoegen

Wanneer u de vooraf geconfigureerde oplossing implementeert, voorziet u automatisch de vier proefapparaten die u ziet in de apparatenlijst. Deze apparaten zijn *gesimuleerde apparaten* uitgevoerd in een Azure WebJob. Gesimuleerde apparaten maken het voor u gemakkelijk om te experimenteren met een vooraf geconfigureerde oplossing zonder echte, fysieke apparaten te moeten implementeren. Raadpleeg de zelfstudie [Uw apparaat koppelen aan de vooraf geconfigureerde oplossing voor externe controle ][lnk-connect-rm] als u een echt apparaat op de oplossing wilt aansluiten.

De volgende stappen laten zien hoe u een gesimuleerd apparaat toevoegt aan de oplossing:

1.  Ga terug naar de lijst met apparaten.

2.  Klik linksonder op **+ Een apparaat toevoegen** om een nieuw apparaat toe te voegen.

    ![Een apparaat toevoegen aan de vooraf geconfigureerde oplossing][img-adddevice]

3.  Klik op **Nieuwe toevoegen** op de tegel **Gesimuleerd apparaat**.

    ![Nieuwe apparaatgegevens in het dashboard instellen][img-addnew]
    
    U kunt niet alleen een nieuw gesimuleerd apparaat maken, maar ook een fysiek apparaat toevoegen als u ervoor kiest om een **aangepast apparaat** te maken. Zie [Uw apparaat aansluiten op de vooraf geconfigureerde IoT Suite-oplossing voor externe controle][lnk-connect-rm] voor meer informatie over het koppelen van fysieke apparaten aan de oplossing.

4.  Selecteer **Laat mij mijn eigen apparaat-id definiëren** en voer de unieke id van een apparaatnaam in, zoals **mijnapparaat_01**.

5.  Klik op **Create**.

    ![Een nieuw apparaat opslaan][img-definedevice]

6. Klik in stap 3 van **Een gesimuleerd apparaat toevoegen** op **Gereed** om terug te keren naar de lijst met apparaten.

7. In de lijst met apparaten kunt u zien dat uw apparaat **wordt uitgevoerd**.

    ![Nieuw apparaat weergeven in de apparatenlijst][img-runningnew]

8. U kunt op het dashboard ook de gesimuleerde telemetrie van het nieuwe apparaat bekijken:

    ![Telemetrie voor nieuw apparaat weergeven][img-runningnew-2]

## De metagegevens van een apparaat bewerken

Wanneer een apparaat voor het eerst verbinding maakt met de oplossing, stuurt het zijn metagegevens naar de oplossing. Wanneer u de metagegevens van het apparaat verwerkt via het oplossingsdashboard, stuurt het nieuwe metagegevenswaarden naar het apparaat en slaat het nieuwe waarden op in de DocumentDB-database van de oplossing. Zie [Apparaatidentiteitsregister en DocumentDB][lnk-devicemetadata] voor meer informatie.

1.  Ga terug naar de lijst met apparaten.

2.  Selecteer het nieuwe apparaat in de **lijst met apparaten** en klik daarna op **Bewerken** om de **apparaateigenschappen** te bewerken:

    ![Metagegevens van het apparaat bijwerken][img-editdevice]

3. Schuif omlaag en wijzig de waarden voor breedtegraad en lengtegraad. Klik daarna op **Wijzigingen aan het apparaatregister opslaan**.

    ![Metagegevens van het apparaat bijwerken][img-editdevice2]

4. Ga terug naar het dashboard. De locatie van het apparaat op de kaart is gewijzigd:

    ![Metagegevens van het apparaat bijwerken][img-editdevice3]

## Een regel voor het nieuwe apparaat toevoegen

Er zijn geen regels voor het nieuwe apparaat dat u zojuist hebt toegevoegd. In deze sectie voegt u een regel toe die een waarschuwing activeert wanneer de door het nieuwe apparaat gemelde temperatuur 47 graden overschrijdt. Voordat u begint, kunt u al zien dat de telemetriegeschiedenis voor het nieuwe apparaat op het dashboard aantoont dat de temperatuur van het apparaat nooit meer dan 45 graden bedraagt.

1.  Ga terug naar de lijst met apparaten.

2.  Selecteer het nieuwe apparaat in de **lijst met apparaten** en klik daarna op **Regel toevoegen** om een regel voor het apparaat toe te voegen.

3. Maak een regel die **Temperatuur** als gegevensveld gebruikt en **AlarmTemp** gebruikt als de uitvoer wanneer de temperatuur 47 graden overschrijdt:

    ![Een apparaatregel toevoegen][img-adddevicerule]

4. Klik op **Regels opslaan en weergeven** om uw wijzigingen op te slaan.

5.  Klik op **Opdrachten** in het deelvenster Apparaatdetails voor het nieuwe apparaat.

    ![Een apparaatregel toevoegen][img-adddevicerule2]

6.  Selecteer **ChangeSetPointTemp** in de opdrachtlijst en stel **SetPointTemp** in op 45. Klik daarna op **Opdracht verzenden**:

    ![Een apparaatregel toevoegen][img-adddevicerule3]

7.  Ga terug naar het dashboard van de oplossing. Na korte tijd verschijnt in het deelvenster **Geschiedenis van waarschuwingen** een nieuwe vermelding wanneer de door het nieuwe apparaat gemelde temperatuur 47 graden overschrijdt:

    ![Een apparaatregel toevoegen][img-adddevicerule4]

8. Op de pagina **Regels** van het dashboard kunt u al uw regels bekijken en bewerken:

    ![Lijst met apparaatregels][img-rules]

9. Op de pagina **Acties** van het dashboard kunt u alle acties die worden uitgevoerd in reactie op een regel bekijken en bewerken:

    ![Lijst met apparaatacties][img-actions]

> [AZURE.NOTE] Het is mogelijk om acties te definiëren die een e-mailbericht of sms kunnen verzenden in antwoord op een regel of die met een Line-Of-Business-systeem kunnen worden geïntegreerd via een [logische app][lnk-logic-apps]. Zie [De Logic App koppelen aan uw vooraf geconfigureerde oplossing Azure IoT Suite Remote Monitoring ][lnk-logicapptutorial].

## Andere functies

Op de portal van de oplossing kunt u naar apparaten zoeken met specifieke kenmerken zoals een modelnummer:

![Een apparaat zoeken][img-search]

U kunt een apparaat uitschakelen en nadat het is uitgeschakeld kunt u het verwijderen:

![Een apparaat uitschakelen en verwijderen][img-disable]

## Achter de schermen

Wanneer u een vooraf geconfigureerde oplossing implementeert, maakt het implementatieproces meerdere resources in het door u geselecteerde Azure-abonnement. U kunt deze resources weergeven in Azure [Portal][lnk-portal]. Het implementatieproces maakt een **resourcegroep** met een naam die is gebaseerd op de naam die u voor uw vooraf geconfigureerde oplossing hebt gekozen:

![Vooraf geconfigureerde oplossing in de Azure-portal][img-portal]

U kunt de instellingen van elke resource weergeven door deze te selecteren in de lijst met resources in de resourcegroep.

U kunt ook de broncode voor de vooraf geconfigureerde oplossing weergeven. De broncode van de vooraf geconfigureerde oplossing voor externe controle bevindt zich in [azure-iot-remote-monitoring][lnk-rmgithub] GitHub-opslagplaats:

- De map **DeviceAdministration** bevat de broncode voor het dashboard.
- De map **Simulator** bevat de broncode voor het gesimuleerde apparaat.
- De map **EventProcessor** bevat de broncode voor het back-endproces dat de binnenkomende telemetrie verwerkt.

Wanneer u klaar bent, kunt u de vooraf geconfigureerde oplossing verwijderen uit uw Azure-abonnement op de [azureiotsuite.com][lnk azureiotsuite] site. Met de site kunt u gemakkelijk resources verwijderen die werden aangevoerd toen u de vooraf geconfigureerde oplossing hebt gemaakt.

> [AZURE.NOTE] Om ervoor te zorgen dat u alles met betrekking tot de vooraf geconfigureerde oplossing verwijdert, verwijdert u niet gewoon de resourcegroep uit de portal, maar verwijdert u de oplossing op de [azureiotsuite.com][lnk azureiotsuite].

## Volgende stappen

Nu u een werkende vooraf geconfigureerde oplossing hebt geïmplementeerd, kunt u doorgaan met IoT Suite door de volgende artikels te lezen:

- [Walkthrough over vooraf geconfigureerde oplossing voor externe controle][lnk-rm-walkthrough]
- [Connect your device to the remote monitoring preconfigured solution][lnk-connect-rm] (Uw apparaat koppelen aan de vooraf geconfigureerde oplossing voor externe controle)
- [Permissions on the azureiotsuite.com site][lnk-permissions] (Machtigingen op de site azureiotsuite.com)

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
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
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md



<!--HONumber=Sep16_HO3-->


