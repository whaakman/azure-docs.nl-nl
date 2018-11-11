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
ms.openlocfilehash: 90837092390cd2550805658471ff7aa884773371
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51239590"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Verbinding maken met een Raspberry Pi aan uw Azure IoT Central-toepassing (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Dit artikel wordt beschreven hoe u, als een apparaat-ontwikkelaar, een Raspberry Pi verbinden met uw Microsoft Azure IoT Central-toepassing met behulp van de Python programmeertaal.

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt de stappen in dit artikel hebt voltooid, moet u de volgende onderdelen:

* Een Azure IoT Central-toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon. Zie voor meer informatie de [maken van een toepassing Quick Start](quick-deploy-iot-central.md).
* Een Raspberry Pi-apparaat met het besturingssysteem Raspbian. U moet een monitor, toetsenbord en muis die zijn verbonden met uw Raspberry Pi voor toegang tot de GUI-omgeving. De Raspberry Pi moet kunnen [verbinding maken met het internet](https://www.raspberrypi.org/learning/software-guide/wifi/).
* (Optioneel) een [Sense Hat](https://www.raspberrypi.org/products/sense-hat/) bestuur van de invoegtoepassing voor de Raspberry Pi. Deze bord verzamelt telemetriegegevens van diverse sensoren tot verzendt naar uw Azure IoT Central-toepassing. Als u geen een **Sense Hat** -bord, kunt u in plaats daarvan een emulator gebruiken (beschikbaar als onderdeel van de installatiekopie Raspberry Pi).

## <a name="sample-devkits-application"></a>**Voorbeeld van een Devkits** toepassing

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **Raspberry Pi** apparaat sjabloon met de volgende kenmerken: 

- Telemetrie, waaronder de volgende metingen die het apparaat wordt verzameld:
    - Vochtigheid
    - Temperatuur
    - Druk
    - Magnetometer (X, Y, Z)
    - Versnellingsmeter (X, Y, Z)
    - Gyroscoop (X, Y, Z)
- Instellingen
    - Voltage
    - Huidige
    - Snelheid van ventilator
    - IR in-/ uitschakelen.
- Eigenschappen
    - Aantal apparaateigenschappen die
    - Cloud-locatie-eigenschap

Raadpleeg voor de volledige details van de configuratie van de sjabloon apparaat [Raspberry PI Apparaatdetails sjabloon](howto-connect-raspberry-pi-python.md#raspberry-pi-device-template-details)
    

## <a name="add-a-real-device"></a>Echt apparaat toevoegen

In uw Azure IoT Central-toepassing toevoegen een echt apparaat uit de **Raspberry Pi** apparaat sjabloon en bijhouden van de verbindingsgegevens van het apparaat (**bereik-ID, apparaat-ID, primaire sleutel**). Zie voor meer informatie, [een echt apparaat toevoegen aan uw Azure IoT Central toepassing](tutorial-add-device.md).


### <a name="configure-the-raspberry-pi"></a>Configureer de Raspberry Pi

De volgende stappen wordt beschreven hoe u downloaden en configureren van de Python-voorbeeldtoepassing vanuit GitHub. Deze voorbeeldtoepassing:

* Hiermee verzendt Telemetrie en eigenschapwaarden naar Azure IoT Central.
* Reageert op wijzigingen die u in Azure IoT Central instellen.

Het configureren van het apparaat [Volg de stapsgewijze instructies op GitHub.](https://aka.ms/iotcentral-docs-Raspi-releases)


> [!NOTE]
> Zie voor meer informatie over de Raspberry Pi Python-voorbeeld de [Leesmij-bestand](https://aka.ms/iotcentral-docs-Raspi-releases) bestand op GitHub.


1. Nadat het apparaat is geconfigureerd, moet uw apparaat starten die gegevens tijdelijk naar Azure IoT Central verzenden.
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
