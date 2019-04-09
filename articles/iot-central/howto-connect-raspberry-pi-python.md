---
title: Verbinding maken met een Raspberry Pi aan uw Azure IoT Central-toepassing (Python) | Microsoft Docs
description: Als de ontwikkelaar van een apparaat, hoe u verbinding maken met een Raspberry Pi aan uw Azure IoT Central-toepassing met behulp van Python.
author: dominicbetts
ms.author: dobett
ms.date: 04/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 6ac16651e2d49dd903ff994b18a8f571bd92fbf6
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59272356"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Verbinding maken met een Raspberry Pi aan uw Azure IoT Central-toepassing (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Dit artikel wordt beschreven hoe u, als een apparaat-ontwikkelaar, een Raspberry Pi verbinden met uw Microsoft Azure IoT Central-toepassing met behulp van de Python programmeertaal.

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt de stappen in dit artikel hebt voltooid, moet u de volgende onderdelen:

* Een Azure IoT Central-toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon. Zie voor meer informatie de [snelstart over het maken van een toepassing](quick-deploy-iot-central.md).
* Een Raspberry Pi-apparaat met het besturingssysteem Raspbian. De Raspberry Pi moet geen verbinding maken met internet. Zie voor meer informatie, [instellen van uw Raspberry Pi](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/3).

## <a name="sample-devkits-application"></a>**Voorbeeld van een Devkits** toepassing

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **Raspberry Pi** apparaat sjabloon met de volgende kenmerken:

- Telemetrie, waaronder de volgende metingen die het apparaat wordt verzameld:
  - Vochtigheid
  - Temperatuur
  - Druk
  - Magnetometer (X, Y, Z)
  - Accelerometer (X, Y, Z)
  - Gyroscope (X, Y, Z)
- Instellingen
  - Voltage
  - Huidige
  - Snelheid van ventilator
  - IR in-/ uitschakelen.
- Properties
  - Aantal apparaateigenschappen die
  - Cloud-locatie-eigenschap

Zie voor de volledige details van de configuratie van de sjabloon van het apparaat, de [Raspberry Pi Apparaatdetails sjabloon](howto-connect-raspberry-pi-python.md#raspberry-pi-device-template-details).

## <a name="add-a-real-device"></a>Echt apparaat toevoegen

Voeg in uw Azure IoT Central-toepassing een echt apparaat uit de **Raspberry Pi** apparaat sjabloon. Maak een notitie van het apparaat verbindingsdetails (**bereik-ID**, **apparaat-ID**, en **primaire sleutel**). Zie voor meer informatie, [een echt apparaat toevoegen aan uw Azure IoT Central toepassing](tutorial-add-device.md).

### <a name="configure-the-raspberry-pi"></a>Configureer de Raspberry Pi

De volgende stappen wordt beschreven hoe u downloaden en configureren van de Python-voorbeeldtoepassing vanuit GitHub. Deze voorbeeldtoepassing:

* Hiermee verzendt Telemetrie en eigenschapwaarden naar Azure IoT Central.
* Reageert op wijzigingen die u in Azure IoT Central instellen.

Het configureren van het apparaat [Volg de stapsgewijze instructies op GitHub](https://github.com/Azure/iot-central-firmware/blob/master/RaspberryPi/README.md).

1. Wanneer het apparaat is geconfigureerd, begint uw apparaat verzenden van telemetrie metingen naar Azure IoT Central.
1. U kunt zien hoe de code die wordt uitgevoerd op de Raspberry Pi communiceert met de toepassing in uw Azure IoT Central-toepassing:

    * Op de **metingen** pagina voor uw echte apparaten, ziet u de telemetrie die is verzonden vanaf de Raspberry Pi.
    * Op de **instellingen** pagina, kunt u de instellingen op de Raspberry Pi zoals spanning- en fan-snelheid. Wanneer de Raspberry Pi de wijziging erkent, de instelling wordt weergegeven als **gesynchroniseerd**.

## <a name="raspberry-pi-device-template-details"></a>Details van de sjabloon Raspberry Pi-apparaat

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **Raspberry Pi** apparaat sjabloon met de volgende kenmerken:

### <a name="telemetry-measurements"></a>Telemetrie-metingen

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
| Tekst            | Locatie     | location   | N/A       |

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een Raspberry Pi verbinden met uw Azure IoT Central-toepassing, vindt hier u de voorgestelde volgende stappen:

* [Verbinding maken met een algemene Node.js-clienttoepassing op Azure IoT Central](howto-connect-nodejs.md)
