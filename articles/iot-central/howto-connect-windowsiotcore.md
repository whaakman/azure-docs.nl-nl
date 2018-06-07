---
title: Een Windows-IoT Core-apparaat aansluit op uw Azure IoT centrale toepassing | Microsoft Docs
description: Informatie over het verbinden van een apparaat MXChip IoT DevKit aan uw Azure IoT centrale toepassing als een ontwikkelaar apparaat.
author: miriamb
ms.author: mriamb
ms.date: 04/09/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 55c0d9c26cd4e7f8ae9f6ff2359e0f2d2a9970c9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629140"
---
# <a name="connect-a-windows-iot-core-device-to-your-azure-iot-central-application"></a>Een Windows-IoT Core-apparaat aansluit op uw Azure IoT centrale toepassing

Dit artikel wordt beschreven hoe u, als een ontwikkelaar apparaat, een Windows-IoT Core-apparaat aansluit op uw Microsoft Azure IoT centrale toepassing.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

1. Een Azure IoT centrale toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon. Zie voor meer informatie [maken van uw Azure IoT centrale toepassing](howto-create-application.md).
2. Een apparaat waarop het besturingssysteem Windows 10 IoT Core. Voor dit scenario gebruiken we een Pi frambozen

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **Windows IoT Core** apparaat sjabloon met de volgende kenmerken:

### <a name="telemetry-measurements"></a>Telemetrie metingen

| Veldnaam     | Eenheden  | Minimum | Maximum | Aantal decimalen |
| -------------- | ------ | ------- | ------- | -------------- |
| Vochtigheid       | %      | 0       | 100     | 0              |
| TEMP           | ° C     | -40     | 120     | 0              |
| pressure       | hPa    | 260     | 1260    | 0              |

### <a name="settings"></a>Instellingen

Numerieke instellingen

| Weergavenaam | Veldnaam | Eenheden | Aantal decimalen | Minimum | Maximum | Oorspronkelijk |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Snelheid van ventilator    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |


### <a name="properties"></a>Eigenschappen

| Type            | Weergavenaam | Veldnaam | Gegevenstype |
| --------------- | ------------ | ---------- | --------- |
| Apparaateigenschap | Aantal die   | dieNumber  | getal    |
| Tekst            | Locatie     | location   | N/A       |

## <a name="add-a-real-device"></a>Echt apparaat toevoegen

In uw Azure IoT centrale toepassing toevoegen een echte apparaat van de **Windows IoT Core** apparaat sjabloon en maak een notitie van de verbindingsreeks van het apparaat. Zie voor meer informatie [een echte apparaat toevoegt aan uw Azure IoT centrale toepassing](tutorial-add-device.md).

### <a name="prepare-the-windows-iot-core-device"></a>Het apparaat Windows IoT Core voorbereiden

Voor het instellen van een apparaat met Windows IoT Core Volg de stapsgewijze handleiding op [instellen van een apparaat met Windows IoT Core] (https://github.com/Microsoft/microsoft-iot-central-firmware/tree/master/WindowsIoT#setup-a-physical-device).

### <a name="add-a-real-device"></a>Echt apparaat toevoegen

In uw Azure IoT centrale toepassing toevoegen een echte apparaat van de **Windows IoT Core** apparaat sjabloon en maak een notitie van de verbindingsreeks van het apparaat. Zie voor meer informatie [een echte apparaat toevoegt aan uw Azure IoT centrale toepassing](tutorial-add-device.md).

## <a name="prepare-the-windows-10-iot-core-device"></a>Het apparaat met Windows 10 IoT Core voorbereiden

### <a name="what-youll-need"></a>Wat u nodig hebt

Als u een fysiek apparaat met Windows 10 IoT Core instelt, moet u eerst een apparaat met Windows 10 IoT Core hebben. Meer informatie over het instellen van een apparaat met Windows 10 IoT Core [hier](https://developer.microsoft.com/en-us/windows/iot/getstarted/prototype/setupdevice).

U moet ook een clienttoepassing die met Azure IoT centrale communiceren kan. U kunt uw eigen aangepaste toepassing met behulp van de Azure SDK bouwen en implementeren op uw apparaat met Visual Studio of u kunt downloaden via een [vooraf samengestelde voorbeeld](https://developer.microsoft.com/en-us/windows/iot/samples) gewoon implementeren en uitvoeren op het apparaat. 

### <a name="deploying-the-sample-client-application"></a>De voorbeeldtoepassing client implementeren

De clienttoepassing van de vorige stap op uw Windows 10-IoT-apparaat om te kunnen voorbereiden implementeren:

**Zorg ervoor dat de verbindingsreeks is opgeslagen op het apparaat voor de clienttoepassing gebruiken**
* Op het bureaublad van de verbindingstekenreeks opslaan in een tekstbestand met de naam connection.string.iothub.
* Kopieer het tekstbestand naar de map document van het apparaat: `[device-IP-address]\C$\Data\Users\DefaultAccount\Documents\connection.string.iothub`

Zodra u dat hebt gedaan, moet u opent de [Windows Device Portal](https://docs.microsoft.com/en-us/windows/iot-core/manage-your-device/deviceportal) door http://[device-IP-address]:8080 in elke browser te typen.

Vanuit er, zoals weergegeven in de if hieronder, moet u:
1. Vouw het knooppunt 'Apps' aan de linkerkant.
2. Klik op 'voorbeelden snelle uitvoeren'.
3. Klik op 'Azure IoT Hub Client'.
4. Klik op 'Implementeren en uitvoeren'.

![GIF van Azure IoT Hub-Client op Windows Device-Portal](./media/howto-connect-windowsiotcore/iothubapp.gif)

Als dat lukt, wordt de toepassing starten op het apparaat en wordt er als volgt:

![Schermopname van Azure IoT Hub Client-app](./media/howto-connect-windowsiotcore/IoTHubForegroundClientScreenshot.png)

In Azure IoT Central, kunt u zien hoe de code die wordt uitgevoerd op de Pi frambozen samenwerkt met de toepassing:

* Op de **metingen** pagina voor het echte apparaat ziet u de telemetrie.
* Op de **eigenschappen** pagina ziet u de waarde van het gerapporteerde aantal Die eigenschap.
* Op de **instellingen** pagina kunt u verschillende instellingen op de Pi frambozen zoals spanning en ventilator snelheid.

## <a name="download-the-source-code"></a>De broncode downloaden

Als u wilt bekijken en wijzigen van de broncode voor de clienttoepassing, kunt u deze downloaden vanuit GitHub [hier](https://github.com/Microsoft/Windows-iotcore-samples/tree/develop/Samples/Azure/IoTHubClients). Als u van plan bent om de code te wijzigen, moet u deze instructies in het bestand Leesmij [hier](https://github.com/Microsoft/Windows-iotcore-samples) voor uw besturingssysteem voor desktopcomputers.

> [!NOTE]
> Als **git** is niet geïnstalleerd in uw ontwikkelomgeving, u kunt dit downloaden van [ https://git-scm.com/download ](https://git-scm.com/download).
