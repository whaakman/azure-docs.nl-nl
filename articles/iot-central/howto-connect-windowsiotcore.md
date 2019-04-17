---
title: Een Windows IoT Core-apparaat verbinden met uw Azure IoT Central-toepassing | Microsoft Docs
description: Leer hoe u een apparaat MXChip IoT DevKit verbinden met uw Azure IoT Central-toepassing als een apparaat-ontwikkelaar.
author: miriambrus
ms.author: miriamb
ms.date: 04/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: af6d66d2e3eae80477a151323578b930dcd7727a
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617849"
---
# <a name="connect-a-windows-iot-core-device-to-your-azure-iot-central-application"></a>Een Windows IoT Core-apparaat verbinden met uw Azure IoT Central-toepassing

Dit artikel wordt beschreven hoe u als ontwikkelaar apparaat een Windows IoT Core-apparaat verbinden met uw Microsoft Azure IoT Central-toepassing.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

- Een Azure IoT Central-toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon. Zie voor meer informatie de [snelstart over het maken van een toepassing](quick-deploy-iot-central.md).

- Een apparaat met het besturingssysteem Windows 10 IoT Core. Zie voor meer informatie, [instellen van het apparaat Windows 10 IoT Core](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup).

- Een ontwikkelcomputer met [Node.js](https://nodejs.org/) versie 8.0.0 of hoger is geïnstalleerd. U kunt uitvoeren `node --version` vanaf de opdrachtregel om uw versie te controleren. Node.js is beschikbaar voor een groot aantal verschillende besturingssystemen.

## <a name="the-sample-devkits-application"></a>De voorbeeld-Devkits-toepassing

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **Windows IoT Core** apparaat sjabloon met de volgende kenmerken:

- Telemetrie-metingen voor het apparaat: **Vochtigheid**, **temperatuur**, en **druk te verlichten**.
- Instelling om te bepalen **ventilatorsnelheid**.
- Een apparaateigenschap **Die nummer** en een cloud-eigenschap **locatie**.

Zie voor volledige informatie over de configuratie van de sjabloon apparaat [details van de sjabloon Windows IoT Core-apparaat](#device-template-details).

## <a name="add-a-real-device"></a>Echt apparaat toevoegen

Gebruik in uw Azure IoT Central-toepassing de **Device Explorer** pagina toe te voegen een echt apparaat uit de **Windows 10 IoT Core** apparaat sjabloon. Maak een notitie van het apparaat verbindingsdetails (**bereik-ID**, **apparaat-ID**, en **primaire sleutel**). Zie voor meer informatie, [verbindingsgegevens ophalen](howto-generate-connection-string.md#get-connection-information).

## <a name="prepare-the-device"></a>Het apparaat voorbereiden

Voor het apparaat verbinding maakt met IoT Central, heeft deze een verbindingsreeks nodig.

[!INCLUDE [iot-central-howto-connection-string](../../includes/iot-central-howto-connection-string.md)]

De apparaat-code voor toegang tot de verbindingsreeks opslaan in een bestand met de naam **connection.string.iothub** in de map `C:\Data\Users\DefaultAccount\Documents\` op uw Windows 10 IoT Core-apparaat.

Kopiëren van de **connection.string.iothub** -bestand van de computer de `C:\Data\Users\DefaultAccount\Documents\` map op uw apparaat, kunt u de [Windows Device Portal](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal):

1. Gebruik uw webbrowser om te navigeren naar de Windows Device Portal op uw apparaat.
1. Als u de bestanden op uw apparaat, kies **Apps > Verkenner**.
1. Navigeer naar **gebruiker Folders\Documents**. Upload de **connection.string.iothub** bestand:

    ![Verbindingsreeks uploaden](media/howto-connect-windowsiotcore/device-portal.png)

## <a name="deploy-and-run"></a>Implementeren en uitvoeren

Als u wilt implementeren en uitvoeren van de voorbeeldtoepassing op uw apparaat, kunt u de [Windows Device Portal](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal):

1. Gebruik uw webbrowser om te navigeren naar de Windows Device Portal op uw apparaat.
1. Te implementeren en uitvoeren van de **Azure IoT Hub-Client** toepassing, kiest u **Apps > snel uitvoeren voorbeelden**. Kies vervolgens **Azure IoT Hub Client**.
1. Kies vervolgens **implementeren en uitvoeren**.

    ![Implementeren en uitvoeren](media/howto-connect-windowsiotcore/quick-run.png)

Na een paar minuten, kunt u de telemetrie van uw apparaat weergeven in uw IoT Central-toepassing.

De [Windows Device Portal](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal) bevat hulpprogramma's die u gebruiken kunt om op te lossen van uw apparaat:

- De **Apps manager** pagina kunt u de apps die worden uitgevoerd op uw apparaat te beheren.
- Als u een monitor die is verbonden met uw apparaat hebt, kunt u de **apparaatinstellingen** pagina voor het vastleggen van schermafbeeldingen van uw apparaat. Bijvoorbeeld:

    ![Schermafbeelding van de App](media/howto-connect-windowsiotcore/iot-hub-foreground-client.png)

## <a name="download-the-source-code"></a>De broncode downloaden

Als u wilt verkennen en te wijzigen van de broncode voor de clienttoepassing, kunt u het downloaden van de [Windows-iotcore-samples-GitHub-opslagplaats](https://github.com/Microsoft/Windows-iotcore-samples/blob/master/Samples/Azure/IoTHubClients).

## <a name="device-template-details"></a>Sjabloon Apparaatdetails

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **Windows IoT Core** apparaat sjabloon met de volgende kenmerken:

### <a name="telemetry-measurements"></a>Telemetrie-metingen

| Veldnaam     | Eenheden  | Minimum | Maximum | Aantal decimalen |
| -------------- | ------ | ------- | ------- | -------------- |
| vochtigheid       | %      | 0       | 100     | 0              |
| temp           | °C     | -40     | 120     | 0              |
| pressure       | hPa    | 260     | 1260    | 0              |

### <a name="settings"></a>Instellingen

Numerieke instellingen

| Weergavenaam | Veldnaam | Eenheden | Aantal decimalen | Minimum | Maximum | Oorspronkelijk |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Snelheid van ventilator    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

### <a name="properties"></a>Properties

| Type            | Weergavenaam | Veldnaam | Gegevenstype |
| --------------- | ------------ | ---------- | --------- |
| Apparaateigenschappen | Aantal die   | dieNumber  | getal    |
| Tekst            | Locatie     | location   | N/A       |

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een Raspberry Pi verbinden met uw Azure IoT Central-toepassing, de voorgestelde volgende stap is te leren hoe u [instellen van een sjabloon aangepast apparaat](howto-set-up-template.md) voor uw eigen IoT-apparaat.