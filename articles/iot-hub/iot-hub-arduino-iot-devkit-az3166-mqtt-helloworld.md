---
title: Berichten verzenden naar een MQTT-server met behulp van de clientbibliotheek van Azure MQTT | Microsoft Docs
description: De DevKit gebruiken als een client berichten te verzenden naar een MQTT-server
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/02/2018
ms.author: liydu
ms.openlocfilehash: 60520f5a72fd7e27d4ea64ac76511a00a727426e
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57552007"
---
# <a name="send-messages-to-an-mqtt-server"></a>Berichten verzenden naar een MQTT-server

Internet of Things (IoT)-systemen vaak omgaan met onregelmatige, slechte kwaliteit of langzame verbindingen via internet. MQTT is een machine-to-machine (M2M) connectiviteit protocol, dat is ontwikkeld met uitdagingen in gedachten. 

De hier gebruikte MQTT-clientbibliotheek maakt deel uit van de [Eclipse Paho](https://www.eclipse.org/paho/) project, waarmee u API's voor het gebruik van MQTT via meerdere vervoermiddel.

## <a name="what-you-learn"></a>Wat u leert

In dit project leert u het volgende:
- Het gebruik van de MQTT-clientbibliotheek om berichten te verzenden naar een MQTT-broker.
- Klik hier voor meer informatie over het configureren van uw MXChip Iot DevKit als een MQTT-client.

## <a name="what-you-need"></a>Wat u nodig hebt

Voltooid de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) aan:

* Hebt u uw DevKit verbonden met Wi-Fi
* De ontwikkelomgeving voorbereiden

## <a name="open-the-project-folder"></a>Open de projectmap

1. Als de DevKit nog verbinding maken met uw computer, wordt deze verbinding verbreken.

2. Start VS Code.

3. De DevKit verbinden met uw computer.

## <a name="open-the-mqttclient-sample"></a>Open het voorbeeld MQTTClient

Vouw linkerkant **ARDUINO voorbeelden** sectie, blader naar **voorbeelden voor MXCHIP AZ3166 > MQTT**, en selecteer **MQTTClient**. Een nieuwe VS Code-venster geopend met een projectmap daarin.

> [!NOTE]
> U kunt voorbeeld ook openen vanuit het opdrachtenpalet. Gebruik `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) om te openen de command palette, typt u **Arduino**, en zoek en selecteer **Arduino: Voorbeelden**.

## <a name="build-and-upload-the-arduino-sketch-to-the-devkit"></a>Bouwen en de schets Arduino uploaden naar de DevKit

Type `Ctrl+P` (Mac OS: `Cmd+P`) om uit te voeren `task device-upload`. Nadat het uploaden is voltooid, wordt DevKit opnieuw is opgestart en wordt het schema wordt uitgevoerd.

![apparaat-upload](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/device-upload.jpg)

> [!NOTE]
> U ontvangt mogelijk een ' fout: AZ3166: Onbekend pakket"foutbericht. Deze fout treedt op wanneer de bord package index niet correct wordt vernieuwd. U kunt deze fout oplossen, raadpleegt u de [ontwikkeling-sectie van de IoT DevKit Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Het project testen

Volg deze procedure om te openen en het instellen van de seriële Monitor in VS Code:

1. Klik op de `COM[X]` word op de statusbalk om in te stellen met de juiste COM-poort `STMicroelectronics`: ![set-com-poort](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-com-port.jpg)

2. Klik op het pictogram power plug op de statusbalk te openen van de seriële Monitor: ![serieel-monitor](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-monitor.jpg)
  
3. Op de statusbalk, klikt u op het getal dat staat voor de baudrate en stel deze in op `115200`: ![set baudrate](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-baud-rate.jpg)

De seriële Monitor weergeven met alle berichten die door de voorbeeld-schets worden verzonden. De schets verbindt de DevKit met Wi-Fi. Zodra de Wi-Fi-verbinding geslaagd is, verzendt het schema van een bericht naar de broker MQTT. Hierna stuurt het voorbeeld herhaaldelijk twee 'iot.eclipse.org'-berichten met QoS-0 en 1 QoS, respectievelijk.

![serieel-uitvoer](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-output.jpg)

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u de [IoT DevKit Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of verbinding maken met behulp van de volgende kanalen:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="see-also"></a>Zie ook

* [IoT DevKit AZ3166 verbinden met Azure IoT Hub in de cloud](iot-hub-arduino-iot-devkit-az3166-get-started.md)
* [Schud, laat een Tweet](iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message.md)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u uw MXChip Iot DevKit configureert als een MQTT-client en de MQTT-clientbibliotheek gebruiken om berichten te verzenden naar een MQTT-broker, vindt hier u de voorgestelde volgende stappen:

* [Overzicht van Azure accelerator voor externe controle IoT-oplossing](https://docs.microsoft.com/azure/iot-suite/)
* [Een apparaat MXChip IoT DevKit verbinden met uw Azure IoT Central-toepassing](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
