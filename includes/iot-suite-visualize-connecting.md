---
title: bestand opnemen
description: bestand opnemen
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 73ba80878615f04e1755a4d12014691c5ae2a077
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54453117"
---
## <a name="view-device-telemetry"></a>Telemetrie van apparaten weergeven

U ziet de telemetrie van uw apparaat verzonden op de **apparaten** pagina in de oplossing.

1. Selecteer het apparaat dat u hebt ingericht in de lijst met apparaten op de **apparaten** pagina. Een paneel geeft informatie weer over uw apparaat met inbegrip van een diagram van de telemetrie van apparaten:

    ![Details van apparaat](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Kies **druk te verlichten** om de telemetrie-weergave te wijzigen:

    ![Druk te verlichten-telemetrie weergeven](media/iot-suite-visualize-connecting/devicespressure.png)

1. Als u wilt weergeven van diagnostische gegevens over uw apparaat, schuif omlaag naar **Diagnostics**:

    ![Apparaat diagnostische gegevens over weergeven](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Reageren op uw apparaat

Voor het aanroepen van methoden op uw apparaten, gebruikt u de **apparaten** pagina in de oplossing voor externe controle. Bijvoorbeeld, in de oplossing voor externe controle **Koelunit** apparaten implementeren een **FirmwareUpdate** methode.

1. Kies **apparaten** om te navigeren naar de **apparaten** pagina in de oplossing.

1. Selecteer het apparaat dat u hebt ingericht in de lijst met apparaten op de **apparaten** pagina:

    ![Selecteer uw echte apparaat](media/iot-suite-visualize-connecting/devicesselect.png)

1. Voor een lijst van de methoden die u op uw apparaat aanroepen kunt, kies **taken**, klikt u vervolgens **methode Run**. Als u een taak uit te voeren op meerdere apparaten plannen, kunt u meerdere apparaten selecteren in de lijst. De **taken** deelvenster ziet u de typen van de methode voor alle apparaten die u hebt geselecteerd.

1. Kies **FirmwareUpdate**, naam van de taak ingesteld op **UpdatePhysicalChiller**. Instellen **firmwareversie** naar **2.0.0**, stel **Firmware URI** naar **http://contoso.com/updates/firmware.bin**, en kies vervolgens **toepassen**:

    ![Plannen van de firmware-update](media/iot-suite-visualize-connecting/deviceschedule.png)

1. Een reeks berichten weergegeven in de console die de apparaatcode uitgevoerd terwijl het gesimuleerde apparaat de methode afhandelt.

1. Wanneer de update voltooid is, de nieuwe firmwareversie wordt weergegeven op de **apparaten** pagina:

    ![Update voltooid](media/iot-suite-visualize-connecting/complete.png)

> [!NOTE]
> Kies voor het volgen van de status van de taak in de oplossing, **weergave**.

## <a name="next-steps"></a>Volgende stappen

Het artikel [aanpassen van de oplossingsverbetering voor externe controle](../articles/iot-accelerators/iot-accelerators-remote-monitoring-customize.md) worden enkele manieren beschreven om aan te passen de solution accelerator.