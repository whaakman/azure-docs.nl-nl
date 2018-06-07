---
title: Een DevKit apparaat aansluit op uw Azure IoT centrale toepassing | Microsoft Docs
description: Informatie over het verbinden van een apparaat MXChip IoT DevKit aan uw Azure IoT centrale toepassing als een ontwikkelaar apparaat.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: af5cfc2f598893328bc8d4acc979f6d777114f99
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628790"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Sluit een apparaat MXChip IoT DevKit aan uw Azure IoT centrale toepassing

Dit artikel wordt beschreven hoe u, als een ontwikkelaar van het apparaat, een apparaat MXChip IoT DevKit (DevKit) aan uw Microsoft Azure IoT centrale toepassing te sluiten.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

1. Een Azure IoT centrale toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon. Zie voor meer informatie [maken van uw Azure IoT centrale toepassing](howto-create-application.md).
1. Een apparaat DevKit. Als u wilt een apparaat DevKit aanschaffen, gaat u naar [MXChip IoT DevKit](http://mxchip.com/az3166).

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **MXChip** apparaat sjabloon met de volgende kenmerken:

### <a name="telemetry-measurements"></a>Telemetrie metingen

| Veldnaam     | Eenheden  | Minimum | Maximum | Aantal decimalen |
| -------------- | ------ | ------- | ------- | -------------- |
| Vochtigheid       | %      | 0       | 100     | 0              |
| TEMP           | ° C     | -40     | 120     | 0              |
| pressure       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | mg     | -2000   | 2000    | 0              |
| accelerometerY | mg     | -2000   | 2000    | 0              |
| accelerometerZ | mg     | -2000   | 2000    | 0              |
| gyroscopeX     | MDP 's   | -2000   | 2000    | 0              |
| gyroscopeY     | MDP 's   | -2000   | 2000    | 0              |
| gyroscopeZ     | MDP 's   | -2000   | 2000    | 0              |

### <a name="settings"></a>Instellingen

Numerieke instellingen

| Weergavenaam | Veldnaam | Eenheden | Aantal decimalen | Minimum | Maximum | Oorspronkelijk |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Spanning      | setVoltage | V | 0              | 0       | 240     | 0       |
| Huidige      | setCurrent | Stroom  | 0              | 0       | 100     | 0       |
| Snelheid van ventilator    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Instellingen voor in-of uitschakelen

| Weergavenaam | Veldnaam | Op tekst | Uit tekst | Oorspronkelijk |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | AAN      | UIT      | Uit     |

### <a name="properties"></a>Eigenschappen

| Type            | Weergavenaam | Veldnaam | Gegevenstype |
| --------------- | ------------ | ---------- | --------- |
| Apparaateigenschap | Aantal die   | dieNumber  | getal    |
| Tekst            | Locatie     | location   | N/A       |

### <a name="states"></a>statussen 

| Naam          | Weergavenaam   | NORMAAL | LET OP | GEVAAR | 
| ------------- | -------------- | ------ | ------- | ------ | 
| DeviceState   | Apparaatstatus   | Groen  | Orange  | Rood    | 

### <a name="events"></a>Gebeurtenissen 

| Naam             | Weergavenaam      | 
| ---------------- | ----------------- | 
| ButtonBPressed   | Knop B ingedrukt  | 

### <a name="add-a-real-device"></a>Echt apparaat toevoegen

In uw Azure IoT centrale toepassing toevoegen een echte apparaat van de **MXChip** apparaat sjabloon en maak een notitie van de verbindingsreeks van het apparaat. Zie voor meer informatie [een echte apparaat toevoegt aan uw Azure IoT centrale toepassing](tutorial-add-device.md).

## <a name="prepare-the-devkit-device"></a>Het apparaat DevKit voorbereiden

> [!TIP]
> Zie voor richtlijnen voor probleemoplossing DevKit apparaat [IoT DevKit aan de slag](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/).

Het apparaat DevKit voorbereiden:

1. Download de meest recente vooraf gemaakte Azure IoT centrale firmware voor de MXChip van de [releases](https://github.com/Azure/iot-central-firmware/releases) pagina op GitHub. De bestandsnaam downloaden op de pagina releases ziet eruit als `AZ3166-IoT-Central-X.X.X.bin`.

1. Verbind het apparaat DevKit op uw ontwikkelcomputer via een USB-kabel. In Windows is een bestand explorer-venster wordt geopend op een station dat is toegewezen aan de opslag op het apparaat DevKit. Bijvoorbeeld: het station kan worden aangeroepen **AZ3166 (D:)**.

1. Sleep de **iotCentral.bin** bestand op het station-venster. Tijdens het kopiëren voltooid is, wordt het apparaat opnieuw opgestart met de nieuwe firmware.

1. Wanneer het apparaat DevKit opnieuw wordt opgestart, wordt het volgende scherm wordt weergegeven:

    ```
    Connect HotSpot:
    AZ3166_??????
    go-> 192.168.0.1 
    PIN CODE xxxxx
    ```

    > [!NOTE]
    > Als het scherm wordt weergegeven in iets anders, drukt u op de **opnieuw** knop op het apparaat. 

1. Het apparaat is nu in de toegangsmodus toegangspunt (AP). U kunt verbinding maken met dit Wi-Fi-toegangspunt van uw computer of mobiel apparaat.

1. Op uw computer, telefoon of tablet verbinding maken met de naam van het Wi-Fi-netwerk op het scherm van het apparaat weergegeven. Als u verbinding met dit netwerk, hoeft u geen toegang tot internet. Deze status is normaal en u bent alleen verbonden met dit netwerk voor een korte periode terwijl u het apparaat configureren.

1. Open uw webbrowser en navigeer naar [ http://192.168.0.1/start ](http://192.168.0.1/start). De volgende webpagina wordt weergegeven:

    ![De configuratiepagina van apparaat](media/howto-connect-devkit/configpage.png)

    Op de webpagina: 
    - de naam van het Wi-Fi-netwerk toevoegen 
    - het wachtwoord voor het Wi-Fi-netwerk 
    - PIN-CODE die wordt weergegeven op het apparaat LCD 
    - de verbindingstekenreeks van uw apparaat. 
      U vindt de verbindingsreeks @ `https://apps.iotcentral.com`  ->  `Device Explorer`  ->  `Device`  ->  `Select or Create a new Real Device`  ->  `Connect this device` (in de rechterbovenhoek) 
    - Selecteer alle beschikbare telemetrie metingen! 

1. Nadat u hebt gekozen **apparaat configureren**, deze pagina wordt weergegeven:

    ![Apparaat is geconfigureerd](media/howto-connect-devkit/deviceconfigured.png)

1. Druk op de **opnieuw** knop op uw apparaat.

> [!NOTE]
> Als u wilt configureren op het apparaat voor het gebruik van een ander Wi-Fi-netwerk, de verbindingsreeks of de telemetrie afmeting, drukt u op zowel de **A** en **B** tegelijkertijd knoppen op het mededelingenbord. Als dit niet werkt, drukt u op **opnieuw** knop en probeer het opnieuw. 

## <a name="view-the-telemetry"></a>De telemetrie weergeven

Wanneer het apparaat DevKit opnieuw wordt opgestart, ziet u het scherm op het apparaat:

* Het aantal telemetrieberichten verzonden.
* Het aantal fouten.
* Het aantal gewenste eigenschappen ontvangen en het aantal gerapporteerde eigenschappen die worden verzonden.

Schud het apparaat increment het aantal gerapporteerde eigenschappen die worden verzonden. Het apparaat verzendt een willekeurig getal als de **Die nummer** apparaateigenschap.

U kunt de telemetrie metingen en gerapporteerde eigenschapswaarden weergeven en configureren van instellingen in Azure IoT centrale:

1. Gebruik **apparaat Explorer** om te navigeren naar de **metingen** pagina voor het echte MXChip-apparaat dat u hebt toegevoegd:

    ![Navigeer naar de werkelijke apparaat](media/howto-connect-devkit/realdevice.png)

1. Op de **metingen** pagina kunt u de telemetrie die afkomstig zijn van het apparaat MXChip zien:

    ![Telemetrie van paginaweergaven van echte apparaat](media/howto-connect-devkit/realtelemetry.png)

1. Op de **eigenschappen** pagina kunt u het nummer van de laatste die gemeld door het apparaat bekijken:

    ![Apparaateigenschappen weergeven](media/howto-connect-devkit/deviceproperties.png)

1. Op de **instellingen** pagina kunt u de instellingen op het apparaat MXChip bijwerken:

    ![Weergave-instellingen voor apparaten](media/howto-connect-devkit/settings.png)

## <a name="download-the-source-code"></a>De broncode downloaden

Als u wilt verkennen en wijzig de apparaatcode, kunt u deze downloaden vanuit GitHub. Als u van plan bent om de code te wijzigen, moet u deze instructies te volgen [voorbereiden van de ontwikkelomgeving](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/#step-5-prepare-the-development-environment) voor uw besturingssysteem voor desktopcomputers.

Voer de volgende opdracht op de computer voor het downloaden van de broncode:

```cmd/sh
git clone https://github.com/Azure/iot-central-firmware
```

De vorige opdracht de broncode downloadt naar een map met de naam `iot-central-firmware`. 

> [!NOTE]
> Als **git** is niet geïnstalleerd in uw ontwikkelomgeving, u kunt dit downloaden van [ https://git-scm.com/download ](https://git-scm.com/download).

## <a name="review-the-code"></a>De code bekijken

Gebruik Visual Studio Code, die is geïnstalleerd bij het voorbereiden van uw ontwikkelomgeving openen de `AZ3166` map in de `iot-central-firmware` map: 

![Visual Studio Code](media/howto-connect-devkit/vscodeview.png)

Als u wilt zien hoe de telemetrie naar de Azure IoT centrale toepassing wordt verzonden, opent u de **main_telemetry.cpp** bestand in de bronmap.

De functie `buildTelemetryPayload` maakt de JSON-telemetrie nettolading met gegevens uit de sensoren op het apparaat.

De functie `sendTelemetryPayload` aanroepen `sendTelemetry` in de **iotHubClient.cpp** de JSON-nettolading verzenden naar de IoT Hub met uw Azure IoT centrale toepassing gebruikt.

Als u wilt zien hoe eigenschapswaarden worden gerapporteerd aan de Azure IoT centrale toepassing, opent u de **main_telemetry.cpp** bestand in de bronmap.

De functie `telemetryLoop` verzendt de **doubleTap** eigenschap gerapporteerd als de versnellingsmeter detecteert dat er een dubbele tikken. Dit maakt gebruik van de `sendReportedProperty` werken in de **iotHubClient.cpp** bronbestand.

De code in de **iotHubClient.cpp** bronbestand gebruikt functies van de [ Microsoft Azure IoT SDK's en bibliotheken voor C](https://github.com/Azure/azure-iot-sdk-c) om te communiceren met IoT Hub.

Zie voor meer informatie over het wijzigen, bouwen en de voorbeeldcode uploaden naar uw apparaat de **readme.md** bestand de `AZ3166` map.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een DevKit apparaat aansluit op uw Azure IoT centrale toepassing, vindt hier u de voorgestelde volgende stappen uit:

* [Raspberry Pi voorbereiden en verbinden](howto-connect-raspberry-pi-python.md)