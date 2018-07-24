---
title: Connnect een Raspberry Pi tot uw Azure IoT Central-toepassing (Python) | Microsoft Docs
description: Als ontwikkelaar in een apparaat, hoe u verbinding maken met een Raspberry Pi aan uw Azure IoT Central-toepassing met behulp van Python.
author: dominicbetts
ms.author: dobett
ms.date: 01/23/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: a3d6ad9f2f442481908bc02252fcc8ab1a74419e
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205585"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Verbinding maken met een Raspberry Pi aan uw Azure IoT Central-toepassing (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Dit artikel wordt beschreven hoe u, als een apparaat-ontwikkelaar, een Raspberry Pi verbinden met uw Microsoft Azure IoT Central-toepassing met behulp van de Python programmeertaal.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

* Een Azure IoT Central-toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon. Zie voor meer informatie, [maken van uw Azure IoT Central Application](howto-create-application.md).
* Een Raspberry Pi-apparaat met het besturingssysteem Raspbian. U moet een monitor, toetsenbord en muis die zijn verbonden met uw Raspberry Pi voor toegang tot de GUI-omgeving. De Raspberry Pi moet kunnen [verbinding maken met het internet](https://www.raspberrypi.org/learning/software-guide/wifi/).
* (Optioneel) een [Sense Hat](https://www.raspberrypi.org/products/sense-hat/) bestuur van de invoegtoepassing voor de Raspberry Pi. Deze bord verzamelt telemetriegegevens van diverse sensoren tot verzendt naar uw Azure IoT Central-toepassing. Als u geen een **Sense Hat** -bord, kunt u in plaats daarvan een emulator gebruiken.

## <a name="sample-devkits-application"></a>**Voorbeeld van een Devkits** toepassing

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **Raspberry Pi** apparaat sjabloon met de volgende kenmerken: 

- Telemetrie waarin de metingen voor het apparaat **vochtigheid**, **temperatuur**, **druk te verlichten**, **Magnometer** (gemeten langs de X Y, Z-as), **Accelorometer** (gemeten langs de X, Y, Z-as) en **gyroscoop** (gemeten langs de X, Y, Z-as).
- Instellingen van **spanning**, **huidige**,**ventilatorsnelheid** en een **IR** in-/ uitschakelen.
- Eigenschappen van de apparaateigenschappen **die nummer** en **locatie** eigenschap in de cloud.


Raadpleeg voor volledige informatie van de configuratie van de sjabloon apparaat [Raspberry PI Apparaatdetails sjabloon](howto-connect-raspberry-pi-python.md#raspberry-pi-device-template-details)
    

## <a name="add-a-real-device"></a>Echt apparaat toevoegen

Voeg in uw Azure IoT Central-toepassing een echt apparaat uit de **Raspberry Pi** apparaat sjabloon en maak een notitie van de verbindingsreeks van het apparaat. Zie voor meer informatie, [een echt apparaat toevoegen aan uw Azure IoT Central toepassing](tutorial-add-device.md).

### <a name="configure-the-raspberry-pi"></a>Configureer de Raspberry Pi

De volgende stappen wordt beschreven hoe u downloaden en configureren van de Python-voorbeeldtoepassing vanuit GitHub. Deze voorbeeldtoepassing:

* Hiermee verzendt Telemetrie en eigenschapwaarden naar Azure IoT Central.
* Reageert op wijzigingen die u in Azure IoT Central instellen.

> [!NOTE]
> Zie voor meer informatie over de Raspberry Pi Python-voorbeeld de [Leesmij-bestand](https://github.com/Microsoft/microsoft-iot-central-firmware/blob/master/RaspberryPi/README.md) bestand op GitHub.

1. De webbrowser op het bureaublad Raspberry Pi gebruiken om te navigeren naar de [releases van Azure IoT Central firmware](https://github.com/Microsoft/microsoft-iot-central-firmware/releases) pagina.

1. Download het zipbestand met de meest recente firmware naar uw basismap op de Raspberry Pi. Naam van het bestand ziet eruit als `RaspberryPi-IoTCentral-X.X.X.zip`.

1. Gebruik daarvoor de firmware-bestand, de **File Manager** in desktop Raspberry Pi. Met de rechtermuisknop op het zip-bestand en kies **hier extraheren**. Hiermee maakt u een map met de naam `RaspberryPi-IoTCentral-X.X.X` in de basismap.

1. Als u geen een **Sense Hat** bord die is gekoppeld aan uw Raspberry Pi, moet u de emulator inschakelen:
    1. In **File Manager**, in de `RaspberryPi-IoTCentral-X.X.X` map met de rechtermuisknop op de **config.iot** -bestand en kies **teksteditor**.
    1. Wijzig de regel `"simulateSenseHat": false,` naar `"simulateSenseHat": true,`.
    1. De wijzigingen opslaan en sluiten **teksteditor**.

1. Start een **Terminal** sessie en gebruik de `cd` opdracht om te navigeren naar de map die u in de vorige stap hebt gemaakt.

1. Voor het starten van de voorbeeldtoepassing die wordt uitgevoerd, typt u `./start.sh` in de **Terminal** venster. Als u de **Sense HAT Emulator**, de GUI-weergegeven. De gebruikersinterface kunt u de telemetriewaarden verzonden naar uw Azure IoT Central-toepassing te wijzigen.

1. De **Terminal** venster wordt een bericht weergegeven dat lijkt op `Device information being served at http://192.168.0.60:8080`. De URL is mogelijk anders in uw omgeving. Kopieer de URL en de webbrowser gebruiken om te navigeren naar de configuratiepagina:

    ![Apparaat configureren](media/howto-connect-raspberry-pi-python/configure.png)

1. Voer de apparaatverbindingsreeks die u hebt wanneer u een echt apparaat toegevoegd aan uw Azure IoT Central-toepassing. Kies vervolgens **apparaat configureren**. U ziet een bericht **apparaat geconfigureerd, wordt uw apparaat moet worden gestart voor het verzenden van gegevens naar Azure IoT Central is het tijdelijk**.

1. In uw Azure IoT Central-toepassing ziet u hoe de code die wordt uitgevoerd op de Raspberry Pi communiceert met de toepassing:

    * Op de **metingen** pagina voor uw echte apparaten, ziet u de telemetrie die is verzonden vanaf de Raspberry Pi. Als u de **Sense HAT Emulator**, kunt u de telemetriewaarden in de gebruikersinterface op de Raspberry Pi.
    * Op de **eigenschappen** pagina, ziet u de waarde van de gerapporteerde **Die nummer** eigenschap.
    * Op de **instellingen** pagina, kunt u verschillende instellingen op de Raspberry Pi zoals spanning- en fan-snelheid. Wanneer de Raspberry Pi de wijziging erkent, de instelling wordt weergegeven als **gesynchroniseerd** in Azure IoT Central.


## <a name="raspberry-pi-device-template-details"></a>Details van de sjabloon Raspberry PI-apparaat

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **Raspberry Pi** apparaat sjabloon met de volgende kenmerken:

### <a name="telemetry-measurements"></a>Telemetrie-metingen

| Veldnaam     | Eenheden  | Minimum | Maximum | Aantal decimalen |
| -------------- | ------ | ------- | ------- | -------------- |
| vochtigheid       | %      | 0       | 100     | 0              |
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
| Voltage      | setVoltage | Voltage | 0              | 0       | 240     | 0       |
| Huidige      | setCurrent | Stroom  | 0              | 0       | 100     | 0       |
| Snelheid van ventilator    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Instellingen voor in-/ uitschakelen

| Weergavenaam | Veldnaam | Van tekst | Uit tekst | Oorspronkelijk |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | AAN      | UIT      | Uit     |

### <a name="properties"></a>Eigenschappen

| Type            | Weergavenaam | Veldnaam | Gegevenstype |
| --------------- | ------------ | ---------- | --------- |
| Apparaateigenschappen | Aantal die   | dieNumber  | getal    |
| Tekst            | Locatie     | location   | N/A       |

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe een Raspberry Pi verbinden met uw Azure IoT Central-toepassing, vindt hier u de voorgestelde volgende stappen:

* [Verbinding maken met een algemene Node.js-clienttoepassing op Azure IoT Central](howto-connect-nodejs.md)
