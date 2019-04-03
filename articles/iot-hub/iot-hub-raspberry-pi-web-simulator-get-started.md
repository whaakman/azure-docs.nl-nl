---
title: Raspberry Pi gesimuleerde naar cloud (Node.js) - verbinding maken met Raspberry Pi-websimulator voor Azure IoT Hub | Microsoft Docs
description: Raspberry Pi-websimulator verbinden met Azure IoT Hub voor Raspberry Pi gegevens verzenden naar de Azure-cloud.
author: rangv
manager: ''
keywords: Raspberry pi simulator, azure iot raspberry pi, raspberry pi iot-hub, raspberry pi verzenden gegevens naar de cloud, raspberry pi naar de cloud
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 3f8fbf571efafe22d7d0c5ccfd71dded381ba6fb
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58847215"
---
# <a name="connect-raspberry-pi-online-simulator-to-azure-iot-hub-nodejs"></a>Raspberry Pi online simulator verbinden met Azure IoT Hub (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

In deze zelfstudie begint u door te leren van de basisbeginselen van het werken met Raspberry Pi online simulator. Vervolgens leert u hoe u naadloos verbinding maken met de simulator Pi naar de cloud met behulp van [Azure IoT Hub](about-iot-hub.md). 

Als u fysieke apparaten hebt, gaat u naar [Raspberry Pi verbinding maken met Azure IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md) aan de slag. 

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

## <a name="what-you-do"></a>Wat u allemaal doen

* Leer de basisprincipes van Raspberry Pi online simulator.
* Maak een IoT-hub.
* Registreer een apparaat voor Pi in uw IoT-hub.
* Een voorbeeldtoepassing uitvoert op Pi gesimuleerde om sensorgegevens te verzenden naar uw IoT hub.

Gesimuleerde Raspberry Pi verbinden met een IoT-hub die u maakt. U kunt vervolgens een voorbeeldtoepassing uitvoert met de simulator voor het genereren van sensorgegevens. Ten slotte, stuurt u de sensorgegevens naar uw IoT-hub.

## <a name="what-you-learn"></a>Wat u leert

* Over het maken van een Azure-IoT-hub en uw nieuwe apparaat-verbindingsreeks ophalen. Als u een Azure-account geen [Azure een gratis proefaccount maken](https://azure.microsoft.com/free/) in een paar minuten.
* Klik hier voor meer informatie over het werken met Raspberry Pi online simulator.
* Klik hier voor meer informatie over het verzenden van gegevens naar uw IoT hub.

## <a name="overview-of-raspberry-pi-web-simulator"></a>Overzicht van Raspberry Pi-websimulator

Klik op de knop Start Raspberry Pi online simulator.

> [!div class="button"]
> <a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted" target="_blank">Starten van de Simulator Raspberry Pi</a>

Er zijn drie gebieden in de websimulator.

1. Assemblagegebied - het circuit standaard is dat een Pi verbinding moet met een sensor BME280 en een LED maken. Het gebied in de preview-versie is vergrendeld zodat momenteel niet mogelijk is aanpassingen.

2. Gebied - een online code-editor voor u code met Raspberry Pi coderen. De standaard-voorbeeldtoepassing helpt om sensorgegevens te verzamelen BME280 sensor en verzendt naar uw Azure-IoT-Hub. De toepassing is volledig compatibel met echte Pi-apparaten. 

3. Geïntegreerde consolevenster - er wordt de uitvoer van uw code. Er zijn drie knoppen aan de bovenkant van het venster.

   * **Voer** -de toepassing uitvoert in het gebied met coderen.
   * **Opnieuw instellen van** -opnieuw instellen van het coderen gebied met de standaard-voorbeeldtoepassing.
   * **Vouw/uit te breiden** -aan de rechterkant is voor u het consolevenster vouwen/uitbreiden om een knop.

> [!NOTE]
> De Raspberry Pi-websimulator is nu beschikbaar in preview-versie. We willen graag horen uw in de [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator). De broncode is openbare op [GitHub](https://github.com/Azure-Samples/raspberry-pi-web-simulator).

![Overzicht van Pi online simulator](media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Verbindingsreeks voor IoT-hub ophalen

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Een nieuw apparaat registreren in de IoT-hub

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="run-a-sample-application-on-pi-web-simulator"></a>Een voorbeeld-App uitvoeren op Pi-websimulator

1. Controleer in het gebied met coderen, of dat u bezig bent de standaard-voorbeeldtoepassing. Vervang de tijdelijke aanduiding in regel 15 door de verbindingsreeks voor Azure IoT hub-apparaat.
   ![Vervang de verbindingsreeks van apparaat](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)

2. Klik op **uitvoeren** of type `npm start` de toepassing uit te voeren.

Ziet u de volgende uitvoer ziet u de sensorgegevens en de berichten die worden verzonden naar uw IoT-hub ![uitvoer - sensorgegevens van Raspberry Pi verzonden naar uw IoT-hub](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)

## <a name="read-the-messages-received-by-your-hub"></a>De berichten ontvangen door uw hub lezen

Een manier voor het bewaken van berichten ontvangen door uw IoT-hub van het gesimuleerde apparaat is met de Azure IoT-hulpprogramma's voor Visual Studio Code. Zie voor meer informatie, [gebruik Azure IoT-Tools voor Visual Studio-Code voor het verzenden en ontvangen van berichten tussen uw apparaat en IoT-Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

Ga verder met de volgende sectie voor meer manieren voor het verwerken van gegevens die worden verzonden door uw apparaat.

## <a name="next-steps"></a>Volgende stappen

U kunt een voorbeeld van toepassing voor het verzamelen van gegevens en verzenden naar uw IoT-hub hebt uitgevoerd.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
