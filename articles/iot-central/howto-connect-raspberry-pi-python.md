---
title: Processtappen verbinden een Pi frambozen aan uw Azure IoT centrale toepassing (Python) | Microsoft Docs
description: Als een ontwikkelaar van het apparaat een Pi frambozen verbinding met uw Azure IoT centrale toepassing met behulp van Python.
author: dominicbetts
ms.author: dobett
ms.date: 01/23/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: e9c2d18a518bd5c98fcc35efdb0dff36970a49b2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629062"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Verbinding maken met een Pi frambozen aan uw Azure IoT centrale toepassing (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Dit artikel wordt beschreven hoe u, als een ontwikkelaar apparaat een Pi frambozen verbinding met uw Microsoft Azure IoT centrale toepassing met behulp van de programmeertaal Python.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

* Een Azure IoT centrale toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon. Zie voor meer informatie [maken van uw Azure IoT centrale toepassing](howto-create-application.md).
* Een frambozen Pi-apparaat waarop het besturingssysteem Raspbian. U moet een monitor, toetsenbord en muis verbonden met uw Pi frambozen voor toegang tot de GUI-omgeving. De Pi frambozen moet kunnen [verbinding met internet](https://www.raspberrypi.org/learning/software-guide/wifi/).
* Eventueel een [zin Hat](https://www.raspberrypi.org/products/sense-hat/) invoegtoepassing board voor de frambozen Pi. Deze mededelingenbord verzamelt telemetriegegevens van verschillende sensoren verzendt naar uw Azure IoT centrale toepassing. Als u hebt een **zin Hat** mededelingenbord, kunt u een emulator in plaats daarvan.

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **frambozen Pi** apparaat sjabloon met de volgende kenmerken:

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

### <a name="add-a-real-device"></a>Echt apparaat toevoegen

In uw Azure IoT centrale toepassing toevoegen een echte apparaat van de **frambozen Pi** apparaat sjabloon en maak een notitie van de verbindingsreeks van het apparaat. Zie voor meer informatie [een echte apparaat toevoegt aan uw Azure IoT centrale toepassing](tutorial-add-device.md).

## <a name="configure-the-raspberry-pi"></a>De Raspberry Pi configureren

De volgende stappen beschrijven het downloaden en configureren van de voorbeeldtoepassing Python vanuit GitHub. Deze voorbeeldtoepassing:

* Telemetrie- en -waarden verzendt naar Azure IoT centrale.
* Reageert op wijzigingen aangebracht in Azure IoT centrale instellen.

> [!NOTE]
> Zie voor meer informatie over het voorbeeld frambozen Pi Python de [Leesmij](https://github.com/Microsoft/microsoft-iot-central-firmware/blob/master/RaspberryPi/README.md) -bestand op GitHub.

1. Gebruik de webbrowser op het bureaublad frambozen Pi om te navigeren naar de [releases van Azure IoT centrale firmware](https://github.com/Microsoft/microsoft-iot-central-firmware/releases) pagina.

1. Download het zip-bestand dat de meest recente firmware naar uw basismap op de Pi frambozen bevat. De bestandsnaam ziet eruit als `RaspberryPi-IoTCentral-X.X.X.zip`.

1. Als u wilt uitpakken van de firmware-bestand gebruiken de **Bestandsbeheer** op het bureaublad frambozen Pi. Met de rechtermuisknop op het zip-bestand en kies **extraheren hier**. Hiermee maakt u een map met de naam `RaspberryPi-IoTCentral-X.X.X` in uw basismap.

1. Als u geen een **zin Hat** mededelingenbord gekoppeld aan uw Pi frambozen, moet u de emulator inschakelen:
    1. In **Bestandsbeheer**, in de `RaspberryPi-IoTCentral-X.X.X` map met de rechtermuisknop op de **config.iot** -bestand en kies **teksteditor**.
    1. Wijzig de regel `"simulateSenseHat": false,` naar `"simulateSenseHat": true,`.
    1. De wijzigingen opslaan en sluiten **teksteditor**.

1. Start een **Terminal** sessie en gebruik de `cd` opdracht om te navigeren naar de map die u in de vorige stap hebt gemaakt.

1. Typ voor het starten van de voorbeeldtoepassing met `./start.sh` in de **Terminal** venster. Als u de **zin HAT Emulator**, de gebruikersinterface weergegeven. De grafische interface kunt u de telemetrie-waarden verzonden naar uw Azure IoT centrale toepassing wijzigen.

1. De **Terminal** venster wordt een bericht weergegeven dat lijkt op `Device information being served at http://192.168.0.60:8080`. De URL kan afwijken in uw omgeving. Kopieer de URL en de webbrowser gebruiken om te navigeren naar de configuratiepagina:

    ![Apparaat configureren](media/howto-connect-raspberry-pi-python/configure.png)

1. De apparaat-verbindingsreeks die u hebt wanneer u een echte apparaat toegevoegd aan uw Azure IoT centrale toepassing invoeren. Kies vervolgens **apparaat configureren**. Er wordt een bericht **apparaat geconfigureerd, het apparaat moet beginnen met het verzenden van gegevens naar Azure IoT Central tijdelijk worden**.

1. In uw Azure IoT centrale toepassing ziet u hoe de code die wordt uitgevoerd op de Pi frambozen samenwerkt met de toepassing:

    * Op de **metingen** pagina voor het echte apparaat ziet u de telemetrie die is verzonden vanaf de frambozen Pi. Als u de **zin HAT Emulator**, kunt u de telemetrie-waarden in de GUI op de Pi frambozen wijzigen.
    * Op de **eigenschappen** pagina ziet u de waarde van de gerapporteerde **Die nummer** eigenschap.
    * Op de **instellingen** pagina kunt u verschillende instellingen op de Pi frambozen zoals spanning en ventilator snelheid. Wanneer de Pi frambozen de wijziging bevestigt, de instelling wordt weergegeven als **gesynchroniseerd** in Azure IoT centrale.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een Pi frambozen verbinding te maken met uw Azure IoT centrale toepassing, vindt hier u de voorgestelde volgende stappen uit:

* [Verbinding maken met een algemene Node.js-clienttoepassing naar Azure IoT Central](howto-connect-nodejs.md)