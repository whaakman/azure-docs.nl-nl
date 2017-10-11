---
title: Gesimuleerde frambozen Pi naar cloud (Node.js) - web-simulator frambozen Pi verbinding maken met Azure IoT Hub | Microsoft Docs
description: Frambozen Pi web simulator verbinding met Azure IoT Hub voor frambozen Pi gegevens verzenden naar de Azure-cloud.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Raspberry pi simulator, azure iot raspberry pi, raspberry pi iothub, raspberry pi verzenden gegevens naar de cloud, raspberry pi naar cloud
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/28/2017
ms.author: xshi
ms.openlocfilehash: 3b80bf35d6af91d5bdb196d97668dc0f837b92cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="connect-raspberry-pi-online-simulator-to-azure-iot-hub-nodejs"></a>Online simulator frambozen Pi verbinden met Azure IoT Hub (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

In deze zelfstudie maakt beginnen u door te leren over het werken met frambozen Pi online simulator. Vervolgens leert u hoe de simulator Pi naadloos verbinding naar de cloud via [Azure IoT Hub](iot-hub-what-is-iot-hub.md). 

Als u fysieke apparaten hebt, gaat u naar [frambozen Pi verbinding maken met Azure IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md) aan de slag. 

<p>
<div id="diag" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/3_banner.png" alt="Connect Raspberry Pi web simulator to Azure IoT Hub" width="400">
</div>
<p>
<div id="button" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/6_button_default.png" alt="Start Raspberry Pi simulator" width="400" onmouseover="this.src='media/iot-hub-raspberry-pi-web-simulator/5_button_click.png';" onmouseout="this.src='media/iot-hub-raspberry-pi-web-simulator/6_button_default.png';">
</div>

## <a name="what-you-do"></a>Wat u doet

* Leer de basisbeginselen van frambozen Pi online simulator.
* Een iothub maken.
* Een apparaat registreren voor Pi in uw IoT-hub.
* Een voorbeeld van een toepassing uitvoeren op Pi gesimuleerde sensorgegevens verzenden naar uw IoT-hub.

Verbinding maken met gesimuleerde frambozen Pi naar een IoT-hub die u maakt. Voer vervolgens een voorbeeldtoepassing met de simulator sensorgegevens genereren. U kunt ten slotte de sensorgegevens verzendt naar uw IoT-hub.

## <a name="what-you-learn"></a>Wat u leert

* Het maken van een Azure-IoT-hub en het nieuwe apparaat-verbindingsreeks ophalen. Als u een Azure-account geen [maken van een gratis Azure-proefaccount](https://azure.microsoft.com/free/) over een paar minuten.
* Klik hier voor meer informatie over het werken met frambozen Pi online simulator.
* Hoe sensorgegevens verzendt naar uw IoT-hub.

## <a name="overview-of-raspberry-pi-web-simulator"></a>Overzicht van frambozen Pi web simulator

Klik op de knop Start frambozen Pi online simulator.

> [!div class="button"]
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted" target="_blank">Starten van de Simulator Raspberry Pi</a>

Er zijn drie gebieden in de web-simulator.
1. Assemblagegebied - het circuit standaard is dat een Pi verbinding met een sensor BME280 en een LED maakt. Het gebied is vergrendeld in de preview-versie dus momenteel niet aanpassen.
2. Het coderen van gebied - een online code-editor voor u code met frambozen Pi. De voorbeeldtoepassing standaard helpt om sensorgegevens te verzamelen van BME280 sensor en verzendt naar uw Azure-IoT-Hub. De toepassing is volledig compatibel is met echte Pi-apparaten. 
3. Geïntegreerde consolevenster - wordt de uitvoer van uw code. Er zijn drie knoppen aan de bovenkant van dit venster.
   * **Voer** -de toepassing uitvoeren in het gebied van programmeren nodig.
   * **Opnieuw instellen** -opnieuw instellen van het gebied van programmeren nodig tot de voorbeeldtoepassing standaard.
   * **Vouw/uitvouwen** -aan de rechterkant bevindt zich een knop voor u het consolevenster vouwen/uitbreiden.

> [!NOTE] 
De web Pi frambozen simulator is nu beschikbaar in de preview-versie. Wij willen graag uw horen in de [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator). De broncode is openbare op [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator).

![Overzicht van online simulator Pi](media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]


## <a name="run-a-sample-application-on-pi-web-simulator"></a>Een voorbeeld van een toepassing uitvoeren op Pi web simulator

1. Controleer of u bezig bent met de voorbeeldtoepassing standaard in het coderen van gebied. Vervang de tijdelijke aanduiding in regel 15 met de verbindingsreeks voor Azure IoT hub-apparaat.
   ![Vervang de apparaat-verbindingsreeks](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)

2. Klik op **uitvoeren** of type `npm start` de toepassing uit te voeren.


U ziet de volgende uitvoer waarin de sensorgegevens en de berichten die worden verzonden naar uw IoT-hub ![uitvoer - sensorgegevens verzonden van frambozen Pi naar uw IoT-hub](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)


## <a name="next-steps"></a>Volgende stappen

U kunt een voorbeeldtoepassing sensorgegevens verzamelen en verzenden naar uw IoT-hub hebt uitgevoerd.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
