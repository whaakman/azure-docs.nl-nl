---
title: Aan de slag met de oplossing voor externe controle - Azure | Microsoft Docs
description: In deze zelfstudie maakt gebruik van gesimuleerde scenario's voor externe controle oplossingsverbetering introduceren. Deze scenario's worden gemaakt wanneer u de externe controle oplossingsverbetering voor de eerste keer implementeren.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/01/2018
ms.topic: conceptual
ms.openlocfilehash: e354e110df8d168e153c50de6f7aad08dd416464
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627804"
---
# <a name="explore-the-capabilities-of-the-remote-monitoring-solution-accelerator"></a>De mogelijkheden van de oplossing voor externe controle accelerator verkennen

Deze zelfstudie ziet u de belangrijkste mogelijkheden van de oplossing voor externe controle. Ter introductie van het volgen van deze mogelijkheden worden in de zelfstudie algemene scenario's voor klanten met een gesimuleerde IoT-toepassing voor een bedrijf met de naam Contoso gepresenteerd.

De zelfstudie geeft u inzicht in de typische IoT-scenario's de oplossing voor externe controle biedt een out-of-the-box.

In deze zelfstudie leert u het volgende:

>[!div class="checklist"]
> * Visualiseren en apparaten op het dashboard filteren
> * Reageren op een waarschuwing
> * Werk de firmware in uw apparaten
> * Uw assets ordenen
> * Stoppen en starten van de gesimuleerde apparaten

De volgende video ziet u een overzicht van de oplossing voor externe controle:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Part-28-An-introduction-to-Azure-IoT-through-the-new-Remote-Monitoring-Preconfigured-Solution/Player]

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt voltooid, moet u een geïmplementeerd exemplaar van de oplossing voor externe controle in uw Azure-abonnement.

Als u de oplossing voor externe controle nog niet hebt geïmplementeerd, maar u moet voltooien de [implementeren Remote Monitoring solution accelerator](iot-accelerators-remote-monitoring-deploy.md) zelfstudie.

## <a name="the-contoso-sample-iot-deployment"></a>De Contoso-voorbeeld IoT-implementatie

U kunt de implementatie van Contoso voorbeeld IoT inzicht in de algemene scenario's voor externe controle oplossing biedt out-of-the-box. Deze scenario's zijn gebaseerd op de praktijk IoT-implementaties. Waarschijnlijk, kiest u voor het aanpassen van de oplossing voor externe controle om te voldoen aan uw specifieke vereisten, maar de Contoso-voorbeeld kunt u de basisprincipes uitgelegd.

> [!NOTE]
> Als u de CLI gebruikt voor het implementeren van de oplossingsverbetering, het bestand `deployment-{your deployment name}-output.json` bevat informatie over de implementatie, zoals de URL om de geïmplementeerde voorbeeld te openen.

Het Contoso-voorbeeld levert een set regels om te reageren op deze en gesimuleerde apparaten. Zodra u de standaardscenario begrijpt, kunt u blijven verkennen meer van de functies van de oplossing in [geavanceerde bewaking met behulp van de oplossing voor externe controle uitvoeren](iot-accelerators-remote-monitoring-monitor.md).

Contoso is een bedrijf dat tal van activa in verschillende omgevingen beheert. Contoso wil gebruik van de kracht van cloud-gebaseerde IoT-toepassingen op afstand controleren en beheren van meerdere assets vanuit een centrale toepassing. De volgende secties bevatten een overzicht van de eerste configuratie van de Contoso-voorbeeld:

> [!NOTE]
> De Contoso-demo is slechts één manier om de gesimuleerde apparaten inrichten en regels te maken. Andere inrichting opties zijn onder meer het maken van uw eigen aangepaste apparaten. Zie voor meer informatie over het maken van uw eigen regels en apparaten, [beheren en configureren van uw apparaten](iot-accelerators-remote-monitoring-manage.md) en [detecteer problemen met regels op basis van drempelwaarden](iot-accelerators-remote-monitoring-automate.md).

### <a name="contoso-devices"></a>Contoso-apparaten

Contoso maakt gebruik van verschillende soorten smart-apparaten. Deze apparaten te voldoen aan verschillende rollen voor het bedrijf, verschillende stromen van telemetrie verzenden. Bovendien elk apparaattype heeft een ander apparaateigenschappen en methoden ondersteund.

De volgende tabel bevat een overzicht van de ingerichte apparaattypen:

| Apparaattype        | Telemetrie                                  | Eigenschappen                                  | Tags                    | Methoden                                                                                      |
| ------------------ | ------------------------------------------ | ------------------------------------------- | ----------------------- | -------------------------------------------------------------------------------------------- |
| Koelunit            | Temperatuur, vochtigheid, druk            | Type, firmwareversie, Model               | Locatie, Floor, bereik | Opnieuw opstarten, Firmware-Update noodgevallen klep Release toename druk                          |
| Maken van een prototype-apparaat | Temperatuur, belasting, geografische locatie        | Type, firmwareversie, Model               | Locatie, modus          | Computer opnieuw worden opgestart, Firmware-Update Move-apparaat, Stop-apparaat, temperatuur release, temperatuur toename |
| Engine             | Vervangen brandstof niveau, koelvloeistof sensor, trillingen | Type, firmwareversie, Model               | Locatie, Floor, bereik | Firmware-Update, leeg vervangen, opvulling vervangen                                              |
| Vrachtwagen              | Geografische locatie, snelheid, lading temperatuur     | Type, firmwareversie, Model               | Locatie, laden          | Lagere lading temperatuur, toename lading temperatuur, Firmware-update                         |
| Lift           | Floor, trillingen, temperatuur              | Type, firmwareversie Model, geografische locatie | Locatie, bereik        | Lift stoppen, starten lift, Firmware-update                                               |

> [!NOTE]
> De Contoso-demo voorbeeld voorziet in twee apparaten per type. Voor elk type een correct werkt binnen de grenzen die zijn gedefinieerd als normaal door Contoso en heeft de een soort defect. In de volgende sectie leert u over de regels die Contoso voor de apparaten definieert. De grenzen van de juiste werking van deze regels te definiëren.

### <a name="contoso-rules"></a>Contoso-regels

Operators bij Contoso weten de drempelwaarden die bepalen of een apparaat goed werkt. Bijvoorbeeld, werkt een Koelunit niet correct als de druk die rapporteert groter dan 250 PSI is. De volgende tabel bevat de regels op basis van drempelwaarden voor die Contoso voor elk apparaattype definieert:

| Regelnaam | Beschrijving | Drempelwaarde | Severity | Beïnvloede apparaten |
| --------- | ----------- | --------- | -------- | ---------------- |
| Koelunit druk te hoog | Waarschuwingen als chillers hoger dan de normale druk niveaus bereiken   |P > 250 psi       | Kritiek | Chillers            |
| Maken van een prototype apparaat temp te hoog  | Als het maken van een prototype-apparaten naar hoger dan de normale temperatuurniveaus van waarschuwingen  |T>80&deg; F |Kritiek | Maken van een prototype-apparaten |
| Engine vervangen leeg  | Waarschuwingen als brandstoftank engine leeg gaat                     | F < 5 gallon | Info     | Engines             |
| Hoger is dan de normale lading temperatuur | Als de vrachtwagen lading temperatuur hoger dan normaal is waarschuwingen                 | T &LT; 45&deg; F |Waarschuwing  | Vrachtwagens              |
| Lift-trillingen gestopt      | Waarschuwingen als lift volledig stopt (op basis van trillingen niveau)                     | V < 0,1 mm |Waarschuwing  | Liften           |

### <a name="operate-the-contoso-sample-deployment"></a>De implementatie van de steekproef Contoso werkt

U hebt nu de eerste installatie in de Contoso-voorbeeld gezien. De volgende secties worden drie scenario's in de steekproef Contoso die aangeven hoe een operator de oplossingsverbetering zou kunnen gebruiken.

## <a name="respond-to-a-pressure-alert"></a>Reageren op een zware belasting op waarschuwing

Dit scenario laat zien hoe u te identificeren en te reageren op een waarschuwing die door een Koelunit-apparaat wordt geactiveerd. De Koelunit bevindt zich in Redmond, bij het bouwen van 43 floor 2.

Als operator ziet u in het dashboard dat er een waarschuwing die betrekking hebben op de druk van een Koelunit. U kunt pannen en inzoomen op de kaart in meer detail te zien.

1. Op de **Dashboard** pagina in de **waarschuwingen** raster, ziet u de **Koelunit druk te hoog** waarschuwing. De Koelunit ook gemarkeerd op de kaart:

    ![Dashboard druk waarschuwing en het apparaat wordt weergegeven op de kaart](./media/iot-accelerators-remote-monitoring-explore/dashboardalarm.png)

1. Om te navigeren naar de **onderhoud** pagina **onderhoud** in het navigatiemenu. Op de **onderhoud** pagina vindt u de details van de regel die de Koelunit druk waarschuwing heeft geactiveerd.

1. De lijst met waarschuwingen ziet u het aantal keren dat de waarschuwing heeft geactiveerd, bevestigingen en open en gesloten waarschuwingen:

    ![Onderhoudspagina bevat een overzicht van waarschuwingen die zijn geactiveerd](./media/iot-accelerators-remote-monitoring-explore/maintenancealarmlist.png)

1. De laatste melding in de lijst is de meest recente. Klik op de **Koelunit druk te hoog** waarschuwing om de bijbehorende apparaten en de telemetrie weer te geven. De telemetrie ziet Druk pieken voor de Koelunit:

    ![Onderhoudspagina toont telemetrie voor geselecteerde waarschuwing](./media/iot-accelerators-remote-monitoring-explore/maintenancetelemetry.png)

U hebt het probleem dat de waarschuwing en het gekoppelde apparaat geactiveerd geïdentificeerd. Als operator zijn de volgende stappen uit om te bevestigen van de waarschuwing en het probleem te verhelpen.

1. Om aan te geven dat u nu op de waarschuwing werkt, wijzig de **waarschuwing status** naar **bevestigd**:

    ![Selecteer en bevestigt u de waarschuwing](./media/iot-accelerators-remote-monitoring-explore/maintenanceacknowledge.png)

1. Om te fungeren voor de Koelunit, selecteert u deze en kies vervolgens **taken**. Selecteer **methode Run**, vervolgens **EmergencyValveRelease**, de taaknaam van een toevoegen **ChillerPressureRelease**, en kies **toepassen**. Deze instellingen maken van een taak die wordt uitgevoerd onmiddellijk:

    ![Selecteer het apparaat en het plannen van een actie](./media/iot-accelerators-remote-monitoring-explore/maintenanceschedule.png)

1. Als u wilt weergeven van de taakstatus, terug naar de **onderhoud** pagina en bekijk de lijst met taken in de **taken** weergeven. De taak is uitgevoerd om de klep druk op de Koelunit vrij te geven, kunt u zien:

    ![De status van de taken in de weergave taken](./media/iot-accelerators-remote-monitoring-explore/maintenancerunningjob.png)

Ten slotte Bevestig dat de telemetrie-waarden uit de Koelunit terug naar normaal.

1. Als u wilt het raster waarschuwingen weergeven, gaat u naar de **Dashboard** pagina.

1. De apparaattelemetrie weergeven, selecteert u het apparaat voor de oorspronkelijke waarschuwing op de kaart en Bevestig dat is weer in de normale.

1. Als u wilt het incident sluit, gaat u naar de **onderhoud** pagina, selecteer de waarschuwing en zet de status op **gesloten**:

    ![Selecteer en sluit de waarschuwing](./media/iot-accelerators-remote-monitoring-explore/maintenanceclose.png)

## <a name="update-device-firmware"></a>Apparaatfirmware bijwerken

Contoso is het testen van een nieuw type apparaat dat in het veld. Als onderdeel van de cyclus testen moet u ervoor zorgen dat apparaatfirmware werk correct bijgewerkt. De volgende stappen laten zien hoe de oplossing voor externe controle gebruiken om te werken van de firmware op meerdere apparaten.

Gebruik voor de benodigde apparaatstuurprogramma beheertaken uitvoeren, de **apparaten** pagina. Beginnen met een filter voor alle maken van een prototype-apparaten:

1. Navigeer naar de **apparaten** pagina. Kies de **maken van een prototype apparaten** filteren de **Filters** vervolgkeuzelijst:

    ![Gebruik een filter op de pagina apparaten](./media/iot-accelerators-remote-monitoring-explore/devicesprototypingfilter.png)

    > [!TIP]
    > Klik op **beheren** voor het beheren van de beschikbare filters.

1. Selecteer een van de maken van een prototype-apparaten:

    ![Selecteer een apparaat op de pagina apparaten](./media/iot-accelerators-remote-monitoring-explore/devicesselect.png)

1. Klik op de **taken** knop, kiest u **methode Run**, en kies vervolgens **Firmware-update**. Voer waarden in voor **taaknaam**, **firmwareversie**, en **Firmware URI**. Kies **toepassen** plannen van de taak nu uitvoeren:

    ![Planning firmware-update op apparaat](./media/iot-accelerators-remote-monitoring-explore/devicesschedulefirmware.png)

    > [!NOTE]
    > Met de gesimuleerde apparaten kunt u elke URL die u als dat het **Firmware URI** waarde en een waarde die u voor de configuratie de **firmwareversie**. De URL toegang de gesimuleerde apparaten geen.

1. Houd er rekening mee hoeveel apparaten is van invloed op de taak en kies **toepassen**:

    ![Verzenden van de firmware-update-taak](./media/iot-accelerators-remote-monitoring-explore/devicessubmitupdate.png)

U kunt de **onderhoud** pagina voor het bijhouden van de taak terwijl deze wordt uitgevoerd.

1. De lijst met taken wilt weergeven, gaat u naar de **onderhoud** pagina en klik op **taken**.

1. Zoek de gebeurtenis met betrekking tot de taak die u hebt gemaakt. Controleer of dat het updateproces firmware correct is gestart.

<!-- 05/01 broken 
You can create a filter to verify the firmware version updated correctly.

1. To create a filter, navigate to the **Devices** page and select **Manage device groups**:

    ![Manage device groups](./media/iot-accelerators-remote-monitoring-explore/devicesmanagefilters.png)

1. Create a filter that includes only devices with the new firmware version:

    ![Create device filter](./media/iot-accelerators-remote-monitoring-explore/devicescreatefilter.png)

1. Return to the **Devices** page and verify that the device has the new firmware version. -->

## <a name="organize-your-assets"></a>Uw assets ordenen

Contoso heeft twee verschillende teams voor serviceactiviteiten voor veld:

* Het bouwen van de smartcard-team beheert chillers, liften en -engines.
* Het team Smart Vehicle beheert vrachtwagens en maken van een prototype-apparaten.

Als een operator te organiseren en beheren van uw apparaten te vereenvoudigen wilt u ze met de teamnaam van het juiste te labelen.

U kunt labelnamen te gebruiken met apparaten kunt maken.

1. Als u wilt alle apparaten weergeven, gaat u naar de **apparaten** pagina en kies de **alle apparaten** filter:

    ![Alle apparaten weergeven](./media/iot-accelerators-remote-monitoring-explore/devicesalldevices.png)

1. Selecteer de **vrachtwagens** en **maken van een prototype** apparaten. Kies vervolgens **taken**:

    ![Selecteer de apparaten prototype en vrachtwagen](./media/iot-accelerators-remote-monitoring-explore/devicesmultiselect.png)

1. Kies **Tag** en maak vervolgens een nieuwe tekst tag aangeroepen **FieldService** met een waarde **ConnectedVehicle**. Kies een naam voor de taak. Klik vervolgens op **toepassen**:

    ![Label voor prototype en vrachtwagen apparaten toevoegen](./media/iot-accelerators-remote-monitoring-explore/devicesaddtag.png)

1. Selecteer de **Koelunit**, **lift**, en **Engine** apparaten. Kies vervolgens **taken**:

    ![Selecteer Koelunit-engine en lift-apparaten](./media/iot-accelerators-remote-monitoring-explore/devicesmultiselect2.png)

1. Kies **Tag** en maak vervolgens een nieuwe tekst tag aangeroepen **FieldService** met een waarde **SmartBuilding**. Kies een naam voor de taak. Klik vervolgens op **toepassen**:

    ![Label toevoegen aan Koelunit, -engine en lift-apparaten](./media/iot-accelerators-remote-monitoring-explore/devicesaddtag2.png)

U kunt de tagwaarden gebruiken om filters te maken.

1. Op de **apparaten** pagina **beheren apparaatgroepen**:

    ![Apparaatgroepen beheren](./media/iot-accelerators-remote-monitoring-explore/devicesmanagefilters.png)

1. Maak een nieuw filter die gebruikmaakt van de tagnaam **FieldService** en waarde **SmartBuilding**. Het filter opslaan als **smartcard gebouw**.

1. Maak een nieuw filter die gebruikmaakt van de tagnaam **FieldService** en waarde **ConnectedVehicle**. Het filter opslaan als **verbonden voertuigen**.

De Contoso-operator kunt nu een query uitvoeren op basis van het operationele team zonder te hoeven wijzigen op de apparaten niets apparaten.

## <a name="stop-simulated-devices"></a>Gesimuleerde apparaten stoppen

Stoppen van de gesimuleerde apparaten kunt u het menu instellingen. Dit helpt de kosten van testen en verkennen van de oplossing te verlagen. Starten of stoppen van de gesimuleerde apparaten:

1. Kies de **instellingen** pictogram.

1. Klik in-of uitschakelen **Flowing** in- of uitschakelen:

    ![Menu Instellingen](./media/iot-accelerators-remote-monitoring-explore/settings.png)

## <a name="customize-the-ui"></a>De gebruikersinterface aanpassen

In het menu instellingen kunt u eenvoudige aanpassingen toepassen op de oplossingsverbetering externe controle. U kunt:

- Schakelen tussen het lichte en donkere thema's.
- Wijzig de naam van de oplossing.
- Een aangepast logo te uploaden.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd:

>[!div class="checklist"]
> * Visualiseren en apparaten op het dashboard filteren
> * Reageren op een waarschuwing
> * Werk de firmware in uw apparaten
> * Uw assets ordenen
> * Stoppen en starten van de gesimuleerde apparaten

Nu dat u hebt de oplossing voor externe controle verkend, zijn de voorgestelde volgende stappen uit voor meer informatie over de geavanceerde functies van de oplossing voor externe controle:

* [Controleer uw apparaten](./iot-accelerators-remote-monitoring-monitor.md).
* [Beheren van uw apparaten](./iot-accelerators-remote-monitoring-manage.md).
* [Uw oplossing met regels automatiseren](./iot-accelerators-remote-monitoring-automate.md).
* [Onderhouden van uw oplossing](iot-accelerators-remote-monitoring-maintain.md).
