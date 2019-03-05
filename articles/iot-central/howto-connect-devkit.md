---
title: Een apparaat DevKit verbinden met uw Azure IoT Central-toepassing | Microsoft Docs
description: Leer hoe u een apparaat MXChip IoT DevKit verbinden met uw Azure IoT Central-toepassing als een apparaat-ontwikkelaar.
author: dominicbetts
ms.author: dobett
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 5412e0a7bae3667ceb29085da566ef3b73c82c35
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57316622"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Een apparaat MXChip IoT DevKit verbinden met uw Azure IoT Central-toepassing

Dit artikel wordt beschreven hoe u, als een apparaat-ontwikkelaar, een MXChip IoT DevKit (DevKit)-apparaat verbinden met uw Microsoft Azure IoT Central-toepassing.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

1. Een Azure IoT Central-toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon. Zie voor meer informatie de [snelstart over het maken van een toepassing](quick-deploy-iot-central.md).
1. Een apparaat DevKit. Als u wilt een apparaat DevKit kopen, gaat u naar [MXChip IoT DevKit](http://mxchip.com/az3166).


## <a name="sample-devkits-application"></a>**Voorbeeld van een Devkits** toepassing

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **MXChip** apparaat sjabloon met de volgende kenmerken: 

- Telemetrie waarin de metingen voor het apparaat **vochtigheid**, **temperatuur**, **druk te verlichten**, **Magnometer** (gemeten langs de X Y, Z-as), **Accelorometer** (gemeten langs de X, Y, Z-as) en **gyroscoop** (gemeten langs de X, Y, Z-as).
- Status die bevat een voorbeeld van de meting voor **Apparaatstatus**.
- Gebeurtenis-meting met een **B gedrukt** gebeurtenis. 
- Instellingen van **spanning**, **huidige**, **ventilatorsnelheid**, en een **IR** in-/ uitschakelen.
- Eigenschappen van de apparaateigenschappen **die nummer** en **locatie van het apparaat** is een locatie-eigenschap ook als in een **geproduceerd In** eigenschap in de cloud. 


Voor volledige informatie over de configuratie van de verwijzen naar [MXChip sjabloon Apparaatdetails](howto-connect-devkit.md#mxchip-device-template-details)


## <a name="add-a-real-device"></a>Echt apparaat toevoegen

Voeg in uw Azure IoT Central-toepassing een echt apparaat uit de **MXChip** apparaat sjabloon en maak een notitie van de verbindingsgegevens van het apparaat (**bereik-ID, apparaat-ID en primaire sleutel**).

1. Voeg een **echt apparaat** Device Explorer, selecteer **+ Nieuw > echte** om toe te voegen een echt apparaat.
    * Voer de apparaat-Id **<span style="color:Red">(moet zich bevinden in kleine letters)</span>** of gebruikt u de voorgestelde apparaat-id.
    * Voer de naam van het apparaat of de voorgestelde naam gebruiken
    
    ![Apparaat toevoegen](media/concepts-connectivity/add-device.png)


1. Zoals Verbindingsdetails ophalen **bereik-ID, apparaat-ID en primaire sleutel** voor het apparaat door het selecteren van **Connect** op de Apparaatpagina.
 
    ![Verbindingsdetails](media/concepts-connectivity/device-connect.PNG)

3. Zorg ervoor dat deze details op te slaan als u temporaritly verbinding verbroken via internet bij het voorbereiden van het apparaat DevKit. 


### <a name="prepare-the-devkit-device"></a>Het apparaat DevKit voorbereiden

> [!NOTE]
> Als u het apparaat eerder hebt gebruikt en Wi-Fi referenties opgeslagen en wilt configureren van het apparaat voor het gebruik van een ander Wi-Fi-netwerk, een verbindingsreeks of een telemetrie-meting hebt, drukt u op zowel de **A** en **B** tegelijkertijd knoppen op het bord. Als dit niet werkt, drukt u op **opnieuw** knop en probeer het opnieuw.



#### <a name="to-prepare-the-devkit-device"></a>Het apparaat DevKit voorbereiden:


1. Download de meest recente vooraf gebouwde Azure IoT Central firmware voor de MXChip uit de [releases](https://aka.ms/iotcentral-docs-MXChip-releases) pagina op GitHub.
1. Verbind het apparaat DevKit naar uw ontwikkelmachine met behulp van een USB-kabel. In Windows, wordt een bestand explorer-venster geopend op een station dat is toegewezen aan de opslag op het apparaat DevKit. Bijvoorbeeld: het station kan worden aangeroepen **AZ3166 (D:)**.
1. Sleep de **iotCentral.bin** -bestand op het station-venster. Wanneer het kopiëren voltooid is, wordt het apparaat opnieuw wordt opgestart met de nieuwe firmware.

1. Wanneer het apparaat DevKit opnieuw wordt opgestart, wordt het volgende scherm wordt weergegeven:

    ```
    Connect HotSpot:
    AZ3166_??????
    go-> 192.168.0.1 
    PIN CODE xxxxx
    ```

    > [!NOTE]
    > Als het scherm wordt weergegeven iets anders, opnieuw instellen van het apparaat en druk op de **A** en **B** knoppen op het apparaat op hetzelfde moment opnieuw opstarten van het apparaat. 

1. Het apparaat is nu in de toegangsmodus punt (AP). U kunt met dit Wi-Fi-toegangspunt voor het verbinden van uw computer of mobiel apparaat.

1. Op uw computer, telefoon of tablet verbinding maken met de naam van het Wi-Fi-netwerk op het scherm van het apparaat weergegeven. Wanneer u verbinding met dit netwerk maakt, hoeft u geen toegang tot internet. Deze status wordt verwacht, en u alleen verbinding hebt met dit netwerk voor een korte periode terwijl u het apparaat configureert.

1. Open uw webbrowser en navigeer naar [ http://192.168.0.1/start ](http://192.168.0.1/start). De volgende webpagina wordt weergegeven:

    ![De configuratiepagina van apparaat](media/howto-connect-devkit/configpage.png)

    In de webpagina wordt weergegeven: 
    - de naam van uw Wi-Fi-netwerk toevoegen 
    - het wachtwoord voor het Wi-Fi-netwerk
    - PIN-CODE die wordt weergegeven op het apparaat LCD 
    - details van de verbinding **bereik-Id, apparaat-Id en primaire sleutel** van uw apparaat (u moet al opgeslagen dit de stappen te volgen)      
    - Selecteer alle beschikbare telemetrie metingen. 

1. Nadat u hebt gekozen **apparaat configureren**, ziet u deze pagina:

    ![Apparaat is geconfigureerd](media/howto-connect-devkit/deviceconfigured.png)

1. Druk op de **opnieuw** knop op uw apparaat.


## <a name="view-the-telemetry"></a>De telemetrie bekijken

Wanneer het apparaat DevKit opnieuw wordt opgestart, ziet u het scherm op het apparaat:

* Het aantal berichten over telemetrie verzonden.
* Het aantal fouten.
* Het aantal gewenste eigenschappen die worden ontvangen en het aantal gerapporteerde eigenschappen die worden verzonden.

> [!NOTE]
> Als het apparaat wordt weergegeven tijdens het verbinding maken met controle worden doorlopen als het apparaat is *geblokkeerd* in IoT Central, en *opheffen van blokkeringen* het apparaat, zodat deze verbinding met de app maken kan.

Schud het apparaat increment het aantal gerapporteerde eigenschappen die worden verzonden. Het apparaat verzendt een willekeurig getal als de **Die nummer** apparaateigenschap.

U kunt de telemetrie metingen en gerapporteerde eigenschapswaarden weergeven en configureren van instellingen in Azure IoT Central:

1. Gebruik **Device Explorer** om te navigeren naar de **metingen** pagina voor het echte MXChip-apparaat dat u hebt toegevoegd:

    ![Navigeer naar een echt apparaat](media/howto-connect-devkit/realdevicenew.png)

1. Op de **metingen** pagina, kunt u de telemetrie die afkomstig zijn van het apparaat MXChip zien:

    ![Telemetrie van een echt apparaat weergeven](media/howto-connect-devkit/devicetelemetrynew.png)

1. Op de **eigenschappen** pagina vindt u de laatste nummer en de locatie van het apparaat gerapporteerd door het apparaat:

    ![Eigenschappen van apparaat weergeven](media/howto-connect-devkit/devicepropertynew.png)

1. Op de **instellingen** pagina, kunt u de instellingen op het apparaat MXChip bijwerken:

    ![Weergave-instellingen voor apparaten](media/howto-connect-devkit/devicesettingsnew.png)

1. Op de **Dashboard** pagina, ziet u de locatie die is toegewezen

    ![Apparaat-dashboard weergeven](media/howto-connect-devkit/devicedashboardnew.png)


## <a name="download-the-source-code"></a>De broncode downloaden

Als u wilt verkennen en te wijzigen van de apparaatcode, kunt u deze downloaden vanuit GitHub. Als u van plan bent om te wijzigen van de code, moet u deze instructies te volgen [de ontwikkelomgeving voorbereiden](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/#step-5-prepare-the-development-environment) voor uw besturingssysteem voor desktopcomputers.

Als u wilt de broncode downloaden, voer de volgende opdracht op uw computer:

```cmd/sh
git clone https://github.com/Azure/iot-central-firmware
```

De vorige opdracht downloadt de broncode naar een map met de naam `iot-central-firmware`. 

> [!NOTE]
> Als **git** is niet geïnstalleerd in uw ontwikkelingsomgeving, u kunt dit ook downloaden via [ https://git-scm.com/download ](https://git-scm.com/download).

## <a name="review-the-code"></a>De code bekijken

Gebruik Visual Studio Code, die is geïnstalleerd bij het voorbereiden van uw ontwikkelomgeving te openen de `AZ3166` map in de `iot-central-firmware` map: 

![Visual Studio Code](media/howto-connect-devkit/vscodeview.png)

Als u wilt zien hoe de telemetrie wordt verzonden naar de toepassing Azure IoT Central, opent u de **main_telemetry.cpp** bestand in de bronmap.

De functie `buildTelemetryPayload` wordt gemaakt van de nettolading van de JSON-telemetrie met behulp van de gegevens van de sensoren op het apparaat.

De functie `sendTelemetryPayload` aanroepen `sendTelemetry` in de **iotHubClient.cpp** de JSON-nettolading verzenden naar de IoT-Hub met uw Azure IoT Central-toepassing gebruikt.

Als u wilt zien hoe eigenschapswaarden worden gerapporteerd aan de toepassing Azure IoT Central, opent u de **main_telemetry.cpp** bestand in de bronmap.

De functie `telemetryLoop` verzendt de **doubleTap** eigenschap gemeld wanneer de versnellingsmeter een Dubbeltik detecteert. Hierbij de `sendReportedProperty` werken in de **iotHubClient.cpp** bronbestand.

De code in de **iotHubClient.cpp** bronbestand gebruikmaakt van functies van de [ Microsoft Azure IoT SDK's en bibliotheken voor C](https://github.com/Azure/azure-iot-sdk-c) om te communiceren met IoT Hub.

Zie voor meer informatie over het wijzigen, bouwen en de voorbeeldcode uploaden naar uw apparaat, de **readme.md** -bestand in de `AZ3166` map.

## <a name="mxchip-device-template-details"></a>MXChip sjabloon Apparaatdetails 

Een toepassing gemaakt op basis van de sjabloon van de toepassing Devkits voorbeeld bevat een sjabloon voor het apparaat van MXChip met de volgende kenmerken:

### <a name="measurements"></a>Metingen

#### <a name="telemetry"></a>Telemetrie 

| Veldnaam     | Eenheden  | Minimum | Maximum | Aantal decimalen |
| -------------- | ------ | ------- | ------- | -------------- |
| vochtigheid       | %      | 0       | 100     | 0              |
| temp           | °C     | -40     | 120     | 0              |
| pressure       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | mg     | -2000   | 2000    | 0              |
| accelerometerY | mg     | -2000   | 2000    | 0              |
| accelerometerZ | mg     | -2000   | 2000    | 0              |
| gyroscopeX     | mdps   | -2000   | 2000    | 0              |
| gyroscopeY     | mdps   | -2000   | 2000    | 0              |
| gyroscopeZ     | mdps   | -2000   | 2000    | 0              |


#### <a name="states"></a>States 
| Name          | Weergavenaam   | NORMAAL | LET OP | GEVAAR | 
| ------------- | -------------- | ------ | ------- | ------ | 
| DeviceState   | Apparaatstatus   | Groen  | Orange  | Rood    | 

#### <a name="events"></a>Gebeurtenissen 
| Name             | Weergavenaam      | 
| ---------------- | ----------------- | 
| ButtonBPressed   | Knop B ingedrukt  | 

### <a name="settings"></a>Instellingen

Numerieke instellingen

| Weergavenaam | Veldnaam | Eenheden | Aantal decimalen | Minimum | Maximum | Oorspronkelijk |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Voltage      | setVoltage | Voltage | 0              | 0       | 240     | 0       |
| Huidige      | setCurrent | Stroom  | 0              | 0       | 100     | 0       |
| Snelheid van ventilator    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Instellingen voor in-/ uitschakelen

| Weergavenaam | Veldnaam | Van tekst | Uit tekst | Oorspronkelijk |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | AAN      | UIT      | Uit     |

### <a name="properties"></a>Properties

| Type            | Weergavenaam | Veldnaam | Gegevenstype |
| --------------- | ------------ | ---------- | --------- |
| Apparaateigenschappen | Aantal die   | dieNumber  | getal    |
| Apparaateigenschappen | Locatie van het apparaat   | location  | location    |
| Tekst            | Geproduceerd In     | manufacturedIn   | N/A       |



## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe een apparaat DevKit verbinden met uw Azure IoT Central-toepassing, vindt hier u de voorgestelde volgende stappen:

* [Raspberry Pi voorbereiden en verbinden](howto-connect-raspberry-pi-python.md)
