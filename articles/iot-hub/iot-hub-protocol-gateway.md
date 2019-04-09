---
title: Azure IoT-protocolgateway | Microsoft Docs
description: Het gebruik van een Azure-IoT-protocol-gateway om uit te breiden van IoT Hub mogelijkheden en protocolondersteuning waarmee apparaten verbinding maken met uw hub met behulp van protocollen die worden niet ondersteund door IoT Hub.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/11/2017
ms.openlocfilehash: 9dbb7905c2a0fed65ede610577e0fa11a1deef92
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59276232"
---
# <a name="support-additional-protocols-for-iot-hub"></a>Aanvullende protocollen ondersteunen voor IoT-Hub

Azure IoT Hub biedt systeemeigen ondersteuning voor communicatie via het MQTT-, AMQP- en HTTPS-protocollen. In sommige gevallen, apparaten of veldgateways niet mogelijk gebruikt u een van de volgende standaardprotocollen en vereisen dat protocol aanpassing. In dergelijke gevallen kunt u een aangepaste gateway. Een aangepaste gateway zorgt voor protocol-aanpassing voor IoT Hub-eindpunten door het verkeer van en naar IoT Hub-bridging. U kunt de [protocolgateway van Azure IoT](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) als een aangepaste gateway om in te schakelen protocol-aanpassing voor IoT-Hub.

## <a name="azure-iot-protocol-gateway"></a>Azure IoT-protocol-gateway

De Azure IoT-protocol-gateway is een framework voor aanpassing van protocol die is ontworpen voor grote schaal, bidirectionele communicatie voor het apparaat met IoT Hub. De protocolgateway is een Pass Through-onderdeel dat apparaatverbindingen via een specifieke protocol accepteert. Deze verbinding van het verkeer naar IoT Hub via AMQP 1.0.

U kunt de protocolgateway in Azure implementeren op een zeer schaalbare manier met behulp van Azure Service Fabric, Azure Cloud Services-werkrollen of Windows Virtual Machines. Bovendien kan de protocolgateway worden geïmplementeerd in on-premises omgevingen, zoals veldgateways.

De Azure IoT-protocol-gateway bevat een MQTT-protocol-adapter, waarmee u het MQTT-protocol gedrag indien nodig aanpassen. Sinds de IoT Hub biedt ingebouwde ondersteuning voor het protocol van MQTT v3.1.1, alleen rekening met behulp van de adapter MQTT-protocol als protocol aanpassingen of specifieke vereisten voor aanvullende functionaliteit vereist zijn.

De adapter MQTT laat ook zien het programmeermodel voor het bouwen van protocol adapters voor andere protocollen. Bovendien kunt het programmeermodel van Azure IoT-protocol-gateway u in aangepaste onderdelen voor de verwerking van gespecialiseerde zoals aangepaste verificatie, bericht transformaties, compressie/decompressie of versleuteling/ontsleuteling van verkeer tussen de apparaten en IoT-Hub.

Voor flexibiliteit, de Azure IoT-protocolgateway en MQTT uitvoering vindt u in een open-source-software-project. U kunt de open source-project het toevoegen van ondersteuning voor verschillende protocollen en -protocolversies gebruiken of aanpassen van de implementatie voor uw scenario. 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure IoT-protocol-gateway en het gebruiken en deze implementeren als onderdeel van uw IoT-oplossing:

* [Protocol Azure IoT gateway-bibliotheek op GitHub](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)

* [Azure IoT gateway developer-protocolhandleiding](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

Zie voor meer informatie over het plannen van de implementatie van uw IoT Hub:

* [Vergelijken met Event Hubs](iot-hub-compare-event-hubs.md)

* [Schalen, hoge beschikbaarheid en herstel na noodgevallen](iot-hub-scaling.md)

* [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md)