---
title: Berichten verzenden naar een MQTT-server met behulp van de clientbibliotheek van Azure MQTT | Microsoft Docs.
description: De DevKit als een client gebruiken om berichten te verzenden naar een server MQTT.
services: iot-hub
documentationcenter: ''
author: liydu
manager: timlt
tags: ''
keywords: ''
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2018
ms.author: liydu
ms.openlocfilehash: 62fdd3dae9513973d0284eda5250d9eb29bfde91
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="send-messages-to-an-mqtt-server"></a>Berichten verzenden naar een server MQTT

Internet der dingen (IoT)-systemen vaak bekommeren onregelmatige, slechte kwaliteit of trage internet-verbindingen. MQTT is een machine verschillen (M2M) connectiviteit protocol, dat is ontwikkeld met dergelijke uitdagingen in gedachten. 

De protocollen MQTT clientbibliotheek hier gebruikt maakt deel uit van de [Eclipse Paho](http://www.eclipse.org/paho/) project, waarmee de API's voor het gebruik van de protocollen MQTT via meerdere middelen van transport.

## <a name="what-you-learn"></a>Wat u leert

In dit project leert u de:
- Het gebruik van de clientbibliotheek MQTT berichten verzenden naar een broker MQTT.
- Klik hier voor meer informatie over het configureren van uw Iot MXChip DevKit als een client MQTT.

## <a name="what-you-need"></a>Wat u nodig hebt

Voltooid de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) naar:

* Hebt u uw DevKit verbonden met Wi-Fi
* De ontwikkelomgeving voorbereiden

## <a name="open-the-project-folder"></a>Open de projectmap

1. Verbreek de verbinding met de DevKit van uw computer, als deze al is verbonden.

2. Start VS-Code.

3. De DevKit aansluiten op uw computer.

## <a name="open-the-mqttclient-sample"></a>Open het voorbeeld MQTTClient

Vouw de linkerkant **ARDUINO voorbeelden** sectie, blader naar **voorbeelden voor MXCHIP AZ3166 > MQTT**, en selecteer **MQTTClient**. Een nieuw venster van de VS Code wordt geopend met een projectmap erin.

> [!NOTE]
> U kunt bijvoorbeeld ook openen vanuit opdracht palet. Gebruik `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) typt u de opdracht om palet te openen, **Arduino**, en zoek en selecteer **Arduino: voorbeelden**.

## <a name="build-and-upload-the-arduino-sketch-to-the-devkit"></a>Bouwen en de Arduino schema uploaden naar de DevKit

Type `Ctrl+P` (Mac OS: `Cmd+P`) om uit te voeren `task device-upload`. Nadat het uploaden is voltooid, wordt DevKit opnieuw is opgestart en wordt het schema wordt uitgevoerd.

![apparaat-upload](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/device-upload.jpg)

> [!NOTE]
> U ontvangt mogelijk een ' fout: AZ3166: onbekende pakket ' foutbericht. Deze fout treedt op wanneer de mededelingenbord package index niet correct worden vernieuwd. U lost deze fout, neem deze [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Het project testen

Volg deze procedure om te openen en het instellen van de seriële Monitor in VS-Code:

1. Klik op de `COM[X]` word op de statusbalk om in te stellen met de juiste COM-poort `STMicroelectronics`: ![set-com-poort](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-com-port.jpg)

2. Klik op het power plug-pictogram op de statusbalk om te openen van de seriële Monitor: ![serieel-monitor](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-monitor.jpg)
  
3. Op de statusbalk, klikt u op het getal dat de Baud-Rate en stel deze in op `115200`: ![set baudrate](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-baud-rate.jpg)

De Monitor seriële geeft alle berichten die zijn verzonden door de voorbeeld-schema. Het schema maakt verbinding met de DevKit Wi-Fi. Zodra de Wi-Fi-verbinding geslaagd is, verzendt het schema een bericht naar de protocollen MQTT broker. Daarna verzendt de steekproef herhaaldelijk twee 'iot.eclipse.org'-berichten met QoS 0 en 1 QoS, respectievelijk.

![serieel-uitvoer](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-output.jpg)

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of verbinding maken met behulp van de volgende kanalen:

* [Gitter.IM](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="see-also"></a>Zie ook

* [Verbinding maken met IoT DevKit AZ3166 Azure IoT Hub in de cloud]({{"/docs/getting-started/" | absolute_url }})
* [Schud, schud voor een Tweet]({{"/docs/projects/shake-shake/" | absolute_url }})

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u uw Iot MXChip DevKit configureren als een client MQTT en de protocollen MQTT-clientbibliotheek gebruiken om berichten te verzenden naar een broker MQTT, vindt hier u de voorgestelde volgende stappen uit:

* [Overzicht van Azure accelerator voor externe controle IoT-oplossing](https://docs.microsoft.com/azure/iot-suite/)
* [Sluit een apparaat MXChip IoT DevKit aan uw Azure IoT centrale toepassing](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
