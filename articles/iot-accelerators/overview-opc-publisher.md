---
title: Wat is er OPC Publisher - Azure | Microsoft Docs
description: Overzicht van OPC Publisher
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: c738e927a352373d7f5a4aeb5697e07134a98cba
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603671"
---
# <a name="what-is-opc-publisher"></a>Wat is een OPC-uitgever?

OPC Publisher is een referentie-implementatie die u laat zien hoe u:

- Verbinding maken met bestaande OPC UA-servers.
- Publiceer JSON gecodeerd telemetriegegevens van OPC UA-servers in de OPC UA Pub/Sub-indeling, met behulp van een JSON-nettolading voor Azure IoT Hub.

U kunt een van de transportprotocollen die ondersteuning biedt voor de client-SDK van Azure IoT Hub gebruiken: HTTPS-, AMQP- en MQTT.

De referentie-implementatie bevat:

- Een OPC UA *client* voor het verbinden met bestaande OPC UA-servers die u op uw netwerk hebt.
- Een OPC UA *server* op poort 62222 die u gebruiken kunt voor het beheren van wat wordt gepubliceerd en IoT Hub biedt directe methoden hetzelfde te doen.

U kunt downloaden de [OPC Publisher referentie-implementatie](https://github.com/Azure/iot-edge-opc-publisher) vanuit GitHub.

De toepassing wordt geïmplementeerd met behulp van .NET Core-technologie en kan worden uitgevoerd op elk platform wordt ondersteund door .NET Core.

OPC Publisher implementeert logica voor opnieuw proberen om verbindingen naar eindpunten die niet op een bepaald aantal keep alive aanvragen reageren te maken. Bijvoorbeeld, als een OPC UA-server reageert niet meer vanwege een stroomstoring.

De toepassing maakt voor elke afzonderlijke publicatie-interval voor een OPC UA-server een afzonderlijk abonnement waarover alle knooppunten met deze publicatie-interval worden bijgewerkt.

OPC Publisher biedt ondersteuning voor batchverwerking van de gegevens die worden verzonden naar IoT Hub om netwerkbelasting te verminderen. Deze batchverwerking verzendt een pakket naar IoT Hub alleen als de geconfigureerde pakketgrootte is bereikt.

Deze toepassing maakt gebruik van de OPC Foundation OPC UA-stack verwijzing als NuGet-pakketten. Zie [ https://opcfoundation.org/license/redistributables/1.3/ ](https://opcfoundation.org/license/redistributables/1.3/) voor de licentievoorwaarden.

### <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd wat OPC Publisher is, wordt de voorgestelde volgende stap is te leren hoe u [configureren OPC Publisher](howto-opc-publisher-configure.md).
