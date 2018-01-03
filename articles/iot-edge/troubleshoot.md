---
title: Problemen met Azure IoT-rand | Microsoft Docs
description: Veelvoorkomende problemen oplossen en meer probleemoplossingsvaardigheden voor Azure IoT rand
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 3f61f0bf8234e747ae38146d1a5ea030e3163fa3
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Veelvoorkomende problemen en oplossingen voor Azure IoT rand

Als u problemen hebt met het Azure IoT rand uitvoert in uw omgeving, maken gebruik van dit artikel als richtlijn voor het oplossen van problemen en omzetten. 

## <a name="standard-diagnostic-steps"></a>Standaard diagnostische stappen 

Wanneer u een probleem ondervindt, meer wilt weten over de status van uw IoT-randapparaat aan de hand van de container en de berichten die doorgeven naar en van het apparaat. Gebruik de opdrachten en hulpprogramma's in deze sectie om informatie te verzamelen. 

* Bekijk de logboeken van de docker-containers voor het detecteren van problemen. Beginnen met uw geïmplementeerde containers en vervolgens kijken naar de containers die gezamenlijk de rand van de IoT-runtime: rand Agent en de Edge-Hub. De logboeken van de rand Agent bieden doorgaans info over de levenscyclus van elke container. De logboeken van de rand Hub bevatten informatie over messaging en routering. 

   ```cmd
   docker logs <container name>
   ```

* De berichten gaan via de Edge-Hub kunt weergeven en inzichten op Eigenschappen apparaatupdates met uitgebreide logboeken verzamelen van de runtime-containers.

   ```cmd
   iotedgectl setup --runtime-log-level DEBUG
   ```

* Als u problemen ondervindt, Controleer uw rand apparaat omgevingsvariabelen, zoals de apparaat-verbindingsreeks:

   ```cmd
   docker exec edgeAgent printenv
   ```

U kunt ook de berichten worden verzonden tussen IoT Hub en de rand van de IoT-apparaten controleren. Deze berichten weergeven met behulp van de [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) -extensie voor Visual Studio Code. Zie voor meer instructies [handig hulpmiddel bij het ontwikkelen met Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

Na het onderzoeken van de logboeken en berichten voor meer informatie, kunt u ook proberen opnieuw starten van de rand van Azure IoT-runtime:

   ```cmd
   iotedgectl restart
   ```

## <a name="edge-agent-stops-after-about-a-minute"></a>Rand Agent stoppen na ongeveer een minuut

De Edge-Agent wordt gestart en wordt uitgevoerd voor ongeveer een minuut en vervolgens wordt gestopt. De logboeken aangeven dat de Edge-Agent probeert verbinding maken met IoT Hub via AMQP en vervolgens ongeveer 30 seconden later probeert verbinding maken via de AMQP via websocket. Als dat mislukt, wordt de Edge-Agent wordt afgesloten. 

Voorbeeld van de rand Agent logboeken:

```
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Hoofdoorzaak
Een netwerkconfiguratie op het netwerk host verhindert dat de Agent rand bereikt van het netwerk. De agent probeert eerst verbinding maken via AMQP (poort 5671). Als dit niet lukt, wordt geprobeerd websockets (poort 443).

De rand van de IoT-runtime stelt een netwerk voor elk van de modules communiceren op. Op Linux is dit netwerk een Brugnetwerk. In Windows wordt NAT bevinden. Dit probleem is vaker op Windows-apparaten met behulp van Windows-containers die gebruikmaken van de NAT-netwerk. 

### <a name="resolution"></a>Oplossing
Zorg ervoor dat er een route met het internet voor de IP-adressen toegewezen aan deze brug/NAT-netwerk. Soms een VPN-configuratie op de host heeft voorrang op de rand van de IoT-netwerk. 

## <a name="edge-hub-fails-to-start"></a>Rand Hub niet kan worden gestart

De Edge-Hub niet wordt gestart, en afdrukken bestellen het volgende bericht aan de logboeken: 

```
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Hoofdoorzaak
Poort 443 is afhankelijk van een ander proces op de hostmachine. De Hub rand toegewezen poorten 5671 en 443 voor gebruik in scenario's voor gateway. Deze poorttoewijzing mislukt als een ander proces al aan deze poort gebonden is. 

### <a name="resolution"></a>Oplossing
Zoek en stoppen van het proces dat van poort 443 gebruikmaakt. Dit proces is meestal een webserver.

## <a name="edge-agent-cant-access-a-modules-image-403"></a>Rand Agent geen toegang tot de installatiekopie van een module (403)
Een container niet kan worden uitgevoerd en de logboeken van de rand Agent een fout 403 weergeven. 

### <a name="root-cause"></a>Hoofdoorzaak
De Edge-Agent beschikt niet over machtigingen voor toegang tot de installatiekopie van een module. 

### <a name="resolution"></a>Oplossing
Probeer die wordt uitgevoerd de `iotedgectl login` opdracht opnieuw.

## <a name="next-steps"></a>Volgende stappen
Vindt u dat u een fout in de rand van de IoT-platform gevonden? Controleer [indienen van een probleem](https://github.com/Azure/iot-edge/issues) zodat we verder te verbeteren. 
