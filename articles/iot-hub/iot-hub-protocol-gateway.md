---
title: Azure IoT-protocolgateway | Microsoft Docs
description: Het gebruik van een protocolgateway IoT Azure IoT Hub uitbreiden mogelijkheden en protocolondersteuning zodat apparaten verbinding maken met uw hub met behulp van protocollen niet ondersteund door de IoT Hub.
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: 555e59ae-3136-4533-8ba8-f3a3b6acf648
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: elioda
ms.openlocfilehash: 1ed8ec28b95bbc91b731fd7bb7b3f1f6654e7fcf
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2017
---
# <a name="support-additional-protocols-for-iot-hub"></a>Ondersteuning voor aanvullende protocollen voor IoT Hub
Azure IoT Hub ondersteunt systeemeigen communicatie via de protocollen MQTT, AMQP en HTTPS. In sommige gevallen apparaten of veld gateways niet mogelijk een van deze standaardprotocollen en moeten worden aangepast protocol. In dergelijke gevallen kunt u een aangepaste gateway. Een aangepaste gateway kunt protocol aanpassing voor eindpunten van IoT Hub met bridging het verkeer van en naar IoT Hub. U kunt de [protocolgateway van Azure IOT](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) als een aangepaste gateway protocol aanpassing voor IoT Hub inschakelen.

## <a name="azure-iot-protocol-gateway"></a>Azure IoT-protocol-gateway
De Azure IoT-protocol-gateway is een framework voor aanpassing protocol die is ontworpen voor bidirectionele communicatie met IoT Hub met hoge schaalbaarheid. De protocolgateway is een Pass Through-onderdeel dat apparaatverbindingen over een specifiek protocol aanvaardt. Deze verbinding van het verkeer naar IoT Hub via AMQP 1.0. 

U kunt de protocolgateway in Azure implementeren in een uiterst schaalbare manier met behulp van Azure Service Fabric, Azure Cloud Services-werkrollen of Windows virtuele Machines. Bovendien kan de protocolgateway worden geïmplementeerd in on-premises omgevingen zoals veld gateways.

De Azure IoT-protocol-gateway bevat een MQTT protocol adapter waarmee u kunt het aanpassen van de protocollen MQTT-gedrag indien nodig. Aangezien IoT Hub ingebouwde ondersteuning voor het protocol van MQTT v3.1.1 biedt, moet u alleen overwegen de protocollen MQTT protocol adapter als protocol aanpassingen of specifieke vereisten voor aanvullende functionaliteit vereist zijn.

De adapter MQTT ziet u ook het programmeermodel voor het bouwen van protocol adapters voor andere protocollen. Bovendien kan het programmeermodel van Azure IoT-protocol-gateway u plug-in aangepaste onderdelen voor speciale verwerking zoals aangepaste verificatie, bericht transformaties, compressie/decompressie of ontsleutelen van verkeer tussen de apparaten en IoT Hub.

Voor flexibiliteit, het protocolgateway van Azure IOT en de implementatie van de protocollen MQTT vindt u in een open source software-project. U kunt het open source-project ondersteuning toevoegen voor verschillende protocollen en adresprotocolversies gebruiken of aanpassen van de implementatie voor uw scenario. 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de Azure IoT-protocol-gateway en het gebruiken en als onderdeel van uw IoT-oplossing te implementeren:

* [Azure IoT-protocol-gateway-opslagplaats op GitHub](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Azure IoT gateway protocol ontwikkelaarshandleiding](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

Zie voor meer informatie over het plannen van de implementatie van uw IoT-Hub:

* [Vergelijken met Event Hubs][lnk-compare]
* [Schalen, hoge beschikbaarheid en herstel na noodgevallen][lnk-scaling]
* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
