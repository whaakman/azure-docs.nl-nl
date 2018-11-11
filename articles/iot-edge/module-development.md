---
title: Modules ontwikkelen voor Azure IoT Edge | Microsoft Docs
description: Informatie over het maken van aangepaste modules voor Azure IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 2513624aecff652e8a952b3255faf2ab9366f21a
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51288693"
---
# <a name="understand-the-requirements-and-tools-for-developing-iot-edge-modules"></a>Informatie over de vereisten en hulpprogramma's voor het ontwikkelen van IoT Edge-modules

In dit artikel wordt uitgelegd welke functies beschikbaar zijn bij het schrijven van toepassingen die worden uitgevoerd als IoT Edge-module en hoe u kunt profiteren van deze.

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

Zie [ontwikkelen en implementeren van een IoT Edge-module op een gesimuleerd apparaat](tutorial-csharp-module.md) voor een voorbeeld van een module-toepassing die apparaat-naar-cloud-berichten verzendt en maakt gebruik van de moduledubbel.

### <a name="device-to-cloud-messages"></a>Apparaat-naar-cloud-berichten
Om in te schakelen complexe verwerking van apparaat-naar-cloud-berichten, IoT Edge hub biedt declaratieve routering van berichten tussen modules en tussen modules en IoT-Hub. Declaratieve routering kunt modules worden onderschept en verwerken van berichten verzonden door andere modules en het doorgeven ervan in complexe pijplijnen. Het artikel [modulesamenstelling](module-composition.md) wordt uitgelegd hoe u voor het opstellen van modules in complexe pijplijnen met behulp van routes.

Een IoT Edge-module, in plaats van een normale IoT Hub device-toepassing kan apparaat-naar-cloud-berichten die om te worden verwerkt door ze via proxy door de lokale IoT Edge hub worden ontvangen.

IoT Edge hub doorgeven van berichten naar de module op basis van declaratieve routes die worden beschreven in de [modulesamenstelling](module-composition.md) artikel. Bij het ontwikkelen van een IoT Edge-module, kunt u deze berichten ontvangen door in te stellen bericht handlers.

Vereenvoudigt het maken van routes, IoT Edge wordt het concept van module *invoer* en *uitvoer* eindpunten. Een module kan ontvangen van alle apparaat-naar-cloud-berichten doorgestuurd naar het zonder invoer op te geven en apparaat-naar-cloud-berichten kunt verzenden zonder eventuele uitvoer op te geven.
Met behulp van expliciete invoer en uitvoer, maar dan wel maakt regels voor doorsturen eenvoudiger om te begrijpen. Zie voor meer informatie over de regels voor doorsturen en invoer- en eindpunten voor modules [modulesamenstelling](module-composition.md).

Ten slotte worden verwerkt door de Edge hub apparaat-naar-cloud-berichten factureringslabel voor de volgende eigenschappen:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| $connectionDeviceId | De apparaat-ID van de client die het bericht heeft verzonden |
| $connectionModuleId | De module-ID van de module die het bericht heeft verzonden |
| $inputName | De invoer die dit bericht ontvangen. Kan niet leeg zijn. |
| $outputName | De uitvoer die wordt gebruikt voor het verzenden van het bericht. Kan niet leeg zijn. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Verbinding maken met IoT Edge hub vanuit een module
Verbinding maken met de lokale IoT Edge hub vanuit een module bestaat uit twee stappen: 
1. Gebruik de verbindingsreeks die is geleverd door de IoT Edge-runtime wanneer de module wordt gestart.
2. Zorg ervoor dat uw toepassing accepteert het certificaat dat door de IoT Edge hub op het apparaat.

Het maken van verbinding tekenreeks die moet worden gebruikt door de IoT Edge-runtime in de omgevingsvariabele wordt geïnjecteerd `EdgeHubConnectionString`. Dit maakt ze beschikbaar voor alle programma's die wil gebruiken.

Analogously, het certificaat moet worden gebruikt voor het valideren van de IoT Edge hub-verbinding is opgenomen door de IoT Edge-runtime in een bestand waarvan het pad beschikbaar in de omgevingsvariabele is `EdgeModuleCACertificateFile`.

## <a name="next-steps"></a>Volgende stappen

Nadat u een module ontwikkelen, informatie over hoe u [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md).

