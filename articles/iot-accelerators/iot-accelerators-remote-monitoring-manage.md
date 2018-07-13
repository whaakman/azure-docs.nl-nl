---
title: Apparaten beheren in een op Azure gebaseerde oplossing voor externe bewaking | Microsoft Docs
description: In deze zelfstudie leert u hoe u apparaten kunt beheren die verbonden zijn met de oplossingsversneller voor externe bewaking.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/12/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 63baf6397b2542311525bac740c50b5eacbd35cf
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097424"
---
# <a name="tutorial-configure-and-manage-devices-connected-to-your-monitoring-solution"></a>Zelfstudie: Apparaten configureren en beheren die zijn verbonden met uw bewakingsoplossing

In deze zelfstudie gebruikt u de oplossingsversneller voor externe bewaking om de verbonden IoT-apparaten te configureren en bewaken. U voegt een nieuw apparaat toe aan de oplossingsversneller, configureert het apparaat en werkt de firmware van het apparaat bij.

Contoso heeft nieuwe machines besteld om de capaciteit van een van de faciliteiten uit te breiden. Terwijl u wacht op de levering van de nieuwe machines, wilt u een simulatie uitvoeren om het gedrag van uw oplossing te testen. Om de simulatie uit te voeren, voegt u een nieuw gesimuleerd apparaat toe aan de oplossingsversneller voor externe bewaking. Vervolgens test u of dit gesimuleerde apparaat goed op acties en configuratie-updates reageert.

Voor een uitbreidbare manier om apparaten te configureren en beheren, gebruikt de oplossingsversneller voor externe bewaking IoT Hub-functies zoals [taken](../iot-hub/iot-hub-devguide-jobs.md) en [directe methoden](../iot-hub/iot-hub-devguide-direct-methods.md). Hoewel in deze zelfstudie gebruik wordt gemaakt van gesimuleerde apparaten, kan een ontwikkelaar directe methoden implementeren op een [fysiek apparaat dat is verbonden met de oplossingsversneller voor externe bewaking](iot-accelerators-connecting-devices.md).

In deze zelfstudie doet u het volgende:

>[!div class="checklist"]
> * Een gesimuleerd apparaat inrichten.
> * Een gesimuleerd apparaat testen.
> * De firmware van een apparaat bijwerken.
> * Een apparaat opnieuw configureren.
> * Uw apparaten organiseren.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u een geïmplementeerd exemplaar van de oplossingsversneller voor externe bewaking in uw Azure-abonnement nodig.

Als u de oplossingsverbetering voor externe controle nog niet hebt geïmplementeerd, voltooit u eerst de snelstart [Een cloudoplossing voor externe controle implementeren](quickstart-remote-monitoring-deploy.md).

## <a name="add-a-simulated-device"></a>Een gesimuleerd apparaat toevoegen

Navigeer naar de pagina **Apparaten** in de oplossing en klik op **+ Nieuw apparaat**:

[![Een gesimuleerd apparaat inrichten](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-expanded.png#lightbox)

In het deelvenster **Nieuw apparaat** kiest u **Gesimuleerd**, laat u het aantal in te richten apparaten op **1** staan, kiest u **Faulty Engine** als apparaatmodel en kiest u **Toepassen** om het gesimuleerde apparaat te maken:

[![Een gesimuleerd apparaat inrichten](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-expanded.png#lightbox)

## <a name="test-the-simulated-device"></a>Het gesimuleerde apparaat testen

Om te testen of het gesimuleerde apparaat telemetrie verzendt en eigenschapswaarden meldt, selecteert u het apparaat in de lijst met apparaten op de pagina **Apparaten**. Live informatie over uw apparaat wordt weergegeven in het deelvenster **Apparaatdetails**:

[![Het gesimuleerde apparaat weergeven](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-expanded.png#lightbox)

Controleer in **Apparaatdetails** of het nieuwe apparaat telemetrie verzendt. Als u de andere trillingstelemetriestroom van uw apparaat wilt weergeven, klikt u op **Trillingen**:

[![Selecteer een telemetriestroom om weer te geven](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-expanded.png#lightbox)

In het deelvenster **Apparaatdetails** ziet u andere informatie over het apparaat, zoals tagwaarden, de methoden die worden ondersteund en de eigenschappen die zijn gerapporteerd door het apparaat.

Als u gedetailleerde diagnostische gegevens wilt weergeven, bladert u omlaag om **Diagnostische gegevens** weer te geven:

[![Diagnostische gegevens over het apparaat weergeven](./media/iot-accelerators-remote-monitoring-manage/devicediagnostics-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicediagnostics-expanded.png#lightbox)

## <a name="act-on-a-device"></a>Reageren op een apparaat

Om te testen of het gesimuleerde apparaat correct reageert op acties die worden gestart door de oplossingsversneller, voert u de **FirmwareUpdate**-methode uit. Als u een apparaat test door een methode uit te voeren, selecteert u het apparaat in de lijst met apparaten en klikt u op **Taken**. U kunt meer dan één apparaat selecteren om te testen. In het deelvenster **Taken** selecteert u **Methode uitvoeren**. Het **Engine**-apparaatmodel werkt met drie methoden: **FirmwareUpdate**, **FillTank** en **EmptyTank**:

[![Engine-methoden](./media/iot-accelerators-remote-monitoring-manage/devicesmethods-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmethods-expanded.png#lightbox)

Kies **FirmwareUpdate**, stel de naam van de taak in op **UpdateEngineFirmware**, de firmwareversie op **2.0.0**, de firmware-URI op **http://contoso.com/engine.bin** en klik tot slot op **Toepassen**:

[![De methode van de firmware-update plannen](./media/iot-accelerators-remote-monitoring-manage/firmwareupdatejob-inline.png)](./media/iot-accelerators-remote-monitoring-manage/firmwareupdatejob-expanded.png#lightbox)

Als u de status van de taak wilt volgen, klikt u op **Taakstatus bekijken**:

[![De geplande taak voor de firmware-update bewaken](./media/iot-accelerators-remote-monitoring-manage/firmwareupdatestatus-inline.png)](./media/iot-accelerators-remote-monitoring-manage/firmwareupdatestatus-expanded.png#lightbox)

Nadat de taak is voltooid, gaat u terug naar de pagina **Apparaten**. De nieuwe firmwareversie wordt weergegeven voor het apparaat.

Als u meerdere apparaten van verschillende typen selecteert op de pagina **Apparaten**, kunt u nog steeds een taak maken om een methode voor die verschillende apparaten uit te voeren. Het deelvenster **Taken** geeft alleen de methoden weer die voor alle geselecteerde apparaten worden gebruikt.

## <a name="reconfigure-a-device"></a>Een apparaat opnieuw configureren

Als u wilt testen of u configuratie-eigenschappen van het apparaat kunt bijwerken, selecteert u het apparaat in de lijst met apparaten op de pagina **Apparaten**. Klik vervolgens op **Taken** en kies **Opnieuw configureren**. Het deelvenster Taken geeft voor het geselecteerde apparaat de eigenschapswaarden weer die kunnen worden bijgewerkt:

[![Een apparaat opnieuw configureren](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-expanded.png#lightbox)

Voor het bijwerken van de locatie van het apparaat stelt u de taaknaam in op **UpdateEngineLocation**, de lengtegraad op **-122,15**, de locatie op **Factory 2**, de breedtegraad op **47,62** en klikt u op **Toepassen**:

[![De waarde van een apparaateigenschap bijwerken](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-expanded.png#lightbox)

Als u de status van de taak wilt volgen, klikt u op **Taakstatus bekijken**:

[![De waarde van een apparaateigenschap bijwerken](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-inline.png)](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-expanded.png#lightbox)

Nadat de taak is voltooid, gaat u naar de pagina **Dashboard**. Het apparaat wordt weergegeven op de kaart in de nieuwe locatie:

[![Locatie van het apparaat weergeven](./media/iot-accelerators-remote-monitoring-manage/enginelocation-inline.png)](./media/iot-accelerators-remote-monitoring-manage/enginelocation-expanded.png#lightbox)

## <a name="organize-your-devices"></a>Uw apparaten organiseren

Om het als operator gemakkelijker te maken uw apparaten te organiseren en beheren, kunt u ze markeren met de naam van het desbetreffende team. Contoso heeft twee verschillende teams voor activiteiten van de buitendienst:

* Het team Smart Vehicle beheert trucks en prototypen van apparaten.
* Het team Smart Building beheert koelers (chillers), liften (elevators) en motoren (engines).

Als u al uw apparaten wilt weergeven, gaat u naar de pagina **Apparaten** en kiest u **Alle apparaten** als filter:

[![Alle apparaten weergeven](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-expanded.png#lightbox)

### <a name="add-tags"></a>Tags toevoegen

Selecteer alle **Trucks** en **Prototyping**-apparaten. Klik vervolgens op **Taken**:

[![Prototyping- en truck-apparaten selecteren](./media/iot-accelerators-remote-monitoring-manage/devicesmultiselect-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmultiselect-expanded.png#lightbox)

Selecteer **Tag**, stel de naam van de taak in op **AddConnectedVehicleTag** en voeg een tekstlabel met de naam **FieldService** in met de waarde **ConnectedVehicle**. Klik vervolgens op **Toepassen**:

[![Tag toevoegen aan prototyping- en truck-apparaten](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-expanded.png#lightbox)

Selecteer op de apparaatpagina alle **Chiller**-, **Elevator**- en **Engine**-apparaten. Klik vervolgens op **Taken**:

[![Chiller-, elevator- en engine-apparaten selecteren](./media/iot-accelerators-remote-monitoring-manage/devicesmultiselect2-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmultiselect2-expanded.png#lightbox)

Selecteer **Tag**, stel de naam van de taak in op **AddSmartBuildingTag** en voeg een teksttag met de naam **FieldService** in met de waarde **SmartBuilding**. Klik vervolgens op **Toepassen**:

[![Tag toevoegen aan chiller-, elevator- en engine-apparaten](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-expanded.png#lightbox)

### <a name="create-filters"></a>Filters maken

U kunt nu de tagwaarden gebruiken om filters te maken. Op de pagina **Apparaten** klikt u op **Apparaatgroepen beheren**:

[![Apparaatgroepen beheren](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-expanded.png#lightbox)

Maak een tekstfilter die de tagnaam **FieldService** en de waarde **SmartBuilding** in de voorwaarde gebruikt. Sla het filter op als **Smart Building**:

[![Filter Smart Building maken](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-expanded.png#lightbox)

Maak een tekstfilter die de tagnaam **FieldService** en de waarde **ConnectedVehicle** in de voorwaarde gebruikt. Sla het filter op als **Connected Vehicle**.

[![Filter Connected Vehicle maken](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-expanded.png#lightbox)

Nu kan de Contoso-operator apparaten opvragen op basis van het operationele team:

[![Filter Connected Vehicle maken](./media/iot-accelerators-remote-monitoring-manage/filterinaction-inline.png)](./media/iot-accelerators-remote-monitoring-manage/filterinaction-expanded.png#lightbox)

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent verder te gaan naar de volgende zelfstudie, laat u de oplossingsversneller voor externe bewaking geïmplementeerd. Als u de kosten wilt verminderen voor het uitvoeren van de oplossingsversneller terwijl u deze niet gebruikt, kunt u de gesimuleerde apparaten in het deelvenster Instellingen stopzetten:

[![Telemetrie onderbreken](./media/iot-accelerators-remote-monitoring-manage/togglesimulation-inline.png)](./media/iot-accelerators-remote-monitoring-manage/togglesimulation-expanded.png#lightbox)

Wanneer u bent klaar om te beginnen met de volgende zelfstudie, kunt u de gesimuleerde apparaten opnieuw opstarten.

Als u de oplossingsversneller niet meer nodig hebt, verwijdert u deze van de pagina [Ingerichte oplossingen](https://www.azureiotsolutions.com/Accelerators#dashboard):

![Oplossing verwijderen](media/iot-accelerators-remote-monitoring-manage/deletesolution.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd u hoe u apparaten kunt configureren en beheren die verbonden zijn met de oplossingsversneller voor externe bewaking. Voor informatie over het gebruik van de oplossingsversneller voor het detecteren van problemen met uw verbonden apparaten, gaat u verder met de volgende zelfstudie.

> [!div class="nextstepaction"]
> [Apparaatwaarschuwingen gebruiken om problemen te identificeren en verhelpen met apparaten die zijn verbonden met uw bewakingsoplossing](iot-accelerators-remote-monitoring-maintain.md)
