---
title: Modules ontwikkelen voor Azure IoT Edge | Microsoft Docs
description: Aangepaste modules ontwikkelen voor Azure IoT Edge die met de runtime en IoT-Hub communiceren kan
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 3dd5005312e1c59fa53a0d917951c89033374192
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54200629"
---
# <a name="develop-your-own-iot-edge-modules"></a>Ontwikkelen van uw eigen IoT Edge-modules

Azure IoT Edge-modules kunnen verbinding maken met andere Azure-services en dragen bij aan uw pijplijn voor grotere cloud. Dit artikel wordt beschreven hoe u modules om te communiceren met IoT Edge-runtime en IoT Hub, en dus de rest van de Azure-cloud kunt ontwikkelen. 

## <a name="iot-edge-runtime-environment"></a>IoT Edge-runtime-omgeving
IoT Edge-runtime biedt de infrastructuur voor het integreren van de functionaliteit van meerdere IoT Edge-modules en deze te implementeren op IoT Edge-apparaten. Op hoog niveau, kan elk programma worden geleverd als een IoT Edge-module. Echter, als u wilt profiteren van IoT Edge communicatie- en beheerfuncties, een programma wordt uitgevoerd in een module kunt verbinding maken met lokale IoT Edge hub, geïntegreerd in de IoT Edge-runtime.

## <a name="using-the-iot-edge-hub"></a>Met behulp van de IoT Edge hub
De IoT Edge hub biedt twee belangrijke functies: proxy naar IoT Hub en lokale communicatie.

### <a name="iot-hub-primitives"></a>IoT Hub primitieven
IoT Hub ziet u een exemplaar van de module analogously aan een apparaat, in de zin dat:

* Er is een moduledubbel van die is uniek en geïsoleerd van de [apparaatdubbel](../iot-hub/iot-hub-devguide-device-twins.md) en de andere moduledubbels van het apparaat;
* Er kunnen worden verzonden [apparaat-naar-cloud-berichten](../iot-hub/iot-hub-devguide-messaging.md);
* kan de ontvangen [directe methoden](../iot-hub/iot-hub-devguide-direct-methods.md) speciaal bedoeld voor de identiteit.

Een module kan niet op dit moment ontvangt berichten van cloud-naar-apparaat en de functionaliteit voor het uploaden niet gebruiken.

Bij het schrijven van een module, kunt u de [Azure IoT Device SDK](../iot-hub/iot-hub-devguide-sdks.md) verbinding maken met de IoT Edge hub en de bovenstaande functionaliteit gebruiken net als bij het gebruik van IoT-Hub met de apparaattoepassing van een, het enige verschil dat afkomstig is van uw toepassing back-end die u hebt om te verwijzen naar de module-id in plaats van de apparaat-id.

### <a name="device-to-cloud-messages"></a>Apparaat-naar-cloud-berichten
Om in te schakelen complexe verwerking van apparaat-naar-cloud-berichten, IoT Edge hub biedt declaratieve routering van berichten tussen modules en tussen modules en IoT-Hub. Declaratieve routering kunt modules worden onderschept en verwerken van berichten verzonden door andere modules en het doorgeven ervan in complexe pijplijnen. Zie voor meer informatie, [implementeren modules en routes instellen in IoT Edge](module-composition.md).

Een IoT Edge-module, in plaats van een normale IoT Hub device-toepassing kan apparaat-naar-cloud-berichten die om te worden verwerkt door ze via proxy door de lokale IoT Edge hub worden ontvangen.

IoT Edge hub doorgeven van berichten naar de module op basis van declaratieve routes die worden beschreven in de [implementatie manifest](module-composition.md). Bij het ontwikkelen van een IoT Edge-module, kunt u deze berichten ontvangen door in te stellen bericht handlers.

Ter vereenvoudiging van het maken van routes, IoT Edge wordt het concept van module *invoer* en *uitvoer* eindpunten. Een module kan ontvangen van alle apparaat-naar-cloud-berichten doorgestuurd naar het zonder invoer op te geven en apparaat-naar-cloud-berichten kunt verzenden zonder eventuele uitvoer op te geven. Met behulp van expliciete invoer en uitvoer, maar dan wel maakt regels voor doorsturen eenvoudiger om te begrijpen. 

Ten slotte worden verwerkt door de Edge hub apparaat-naar-cloud-berichten factureringslabel voor de volgende eigenschappen:

| Eigenschap | Description |
| -------- | ----------- |
| $connectionDeviceId | De apparaat-ID van de client die het bericht heeft verzonden |
| $connectionModuleId | De module-ID van de module die het bericht heeft verzonden |
| $inputName | De invoer die dit bericht ontvangen. Kan niet leeg zijn. |
| $outputName | De uitvoer die wordt gebruikt voor het verzenden van het bericht. Kan niet leeg zijn. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Verbinding maken met IoT Edge hub vanuit een module
Verbinding maken met de lokale IoT Edge hub vanuit een module bestaat uit twee stappen: 
1. Maakt een exemplaar ModuleClient in uw toepassing.
2. Zorg ervoor dat uw toepassing accepteert het certificaat dat door de IoT Edge hub op het apparaat.

Een exemplaar ModuleClient om uw module met de IoT Edge hub die wordt uitgevoerd op het apparaat, vergelijkbaar met hoe DeviceClient exemplaren IoT-apparaten met IoT-Hub verbinden verbinding te maken. Zie voor meer informatie over de klasse ModuleClient en de bijbehorende communicatiemethoden, de API-naslaginformatie voor de taal van uw voorkeur SDK: [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet), [C en Python](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable), of [Node.js](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest).


## <a name="next-steps"></a>Volgende stappen

Nadat u een module ontwikkelen, informatie over hoe u [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md).

