---
title: Problemen met Azure IoT Edge oplossen | Microsoft Docs
description: Veelvoorkomende problemen oplossen en informatie over probleemoplossingsvaardigheden voor Azure IoT Edge
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 7b9f9f8295aac0920ae4726289c535aae12c4482
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Veelvoorkomende problemen en oplossingen voor Azure IoT Edge

Als u problemen hebt met het uitvoeren van Azure IoT Edge in uw omgeving, kunt u dit artikel als richtlijn gebruiken voor het oplossen van problemen. 

## <a name="standard-diagnostic-steps"></a>Standaard diagnostische stappen 

Wanneer u een probleem ondervindt, kunt u meer informatie verzamelen over de status van uw Azure IoT Edge-apparaat aan de hand van de containerlogboeken en -berichten die naar en van het apparaat worden doorgegeven. Gebruik de opdrachten en hulpprogramma's in deze sectie om informatie te verzamelen. 

* Bekijk de logboeken van de Docker-containers om problemen te detecteren. Begin met uw geïmplementeerde containers en kijk vervolgens naar de containers die gezamenlijk de IoT Edge-runtime vormen: Edge Agent en Edge Hub. De Edge Agent-logboeken bieden doorgaans informatie over de levenscyclus van elke container. De Edge Hub-logboeken bieden informatie over berichten en routering. 

   ```cmd
   docker logs <container name>
   ```

* Bekijk de berichten die via Edge Hub gaan en verzamel inzichten over updates van apparaateigenschappen met uitgebreide logboeken uit de runtimecontainers. U kunt de optie '--auto-cert-gen-force-no-passwords' toevoegen als u de artikelen met snelstartgidsen volgt.

   ```cmd
   iotedgectl setup --connection-string "{device connection string}" --runtime-log-level debug
   ```

* Als u verbindingsproblemen ondervindt, controleert u de omgevingsvariabelen van uw Edge-apparaat, zoals de apparaatverbindingsreeks:

   ```cmd
   docker exec edgeAgent printenv
   ```

U kunt ook de berichten controleren die worden verzonden tussen IoT Hub en de IoT Edge-apparaten. Bekijk deze berichten met behulp van de [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)-extensie voor Visual Studio Code. Zie [Handig hulpprogramma bij het ontwikkelen met Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/) voor meer richtlijnen.

Nadat u de logboeken en berichten hebt onderzocht voor meer informatie, kunt u ook proberen de Azure IoT Edge-runtime opnieuw te starten:

   ```cmd
   iotedgectl restart
   ```

## <a name="edge-agent-stops-after-about-a-minute"></a>Edge Agent stopt na ongeveer een minuut

De Edge Agent wordt gestart en wordt gedurende ongeveer een minuut uitgevoerd en vervolgens gestopt. De logboeken geven aan dat de Edge Agent verbinding probeert te maken met IoT Hub via AMQP en vervolgens ongeveer 30 seconden later verbinding probeert te maken met behulp van AMQP via websocket. Als dat mislukt, wordt de Edge Agent afgesloten. 

Voorbeeld van Edge Agent-logboeken:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Hoofdoorzaak
Een netwerkconfiguratie op het hostnetwerk verhindert dat de Edge Agent het netwerk bereikt. De agent probeert eerst verbinding maken via AMQP (poort 5671). Als dit niet lukt, probeert deze websockets (poort 443).

De IoT Edge-runtime stelt een netwerk in voor elk van de modules waarmee moet worden gecommuniceerd. In Linux is dit netwerk een brugnetwerk. In Windows wordt NAT gebruikt. Dit probleem komt vaker voor op Windows-apparaten die gebruikmaken van Windows-containers die het NAT-netwerk gebruiken. 

### <a name="resolution"></a>Oplossing
Zorg ervoor dat er een route naar internet is voor de IP-adressen die aan deze brug/dit NAT-netwerk zijn toegewezen. Soms heeft een VPN-configuratie op de host voorrang op het IoT Edge-netwerk. 

## <a name="edge-hub-fails-to-start"></a>Edge Hub kan niet worden gestart

De Edge Hub wordt niet gestart en drukt het volgende bericht af naar de logboeken: 

```output
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Hoofdoorzaak
Poort 443 is bezig met een ander proces op de hostmachine. De Edge Hub heeft poorten 5671 en 443 toegewezen voor gebruik in gatewayscenario's. Deze poorttoewijzing mislukt als er al een ander proces bezig is op deze poort. 

### <a name="resolution"></a>Oplossing
Zoek en stop het proces dat poort 443 gebruikt. Dit proces is meestal een webserver.

## <a name="edge-agent-cant-access-a-modules-image-403"></a>Edge Agent heeft geen toegang tot de installatiekopie van een module (403)
Een container kan niet worden uitgevoerd en in de logboeken van de Edge Agent wordt fout 403 weergegeven. 

### <a name="root-cause"></a>Hoofdoorzaak
De Edge Agent beschikt niet over machtigingen voor toegang tot de installatiekopie van een module. 

### <a name="resolution"></a>Oplossing
Probeer de opdracht `iotedgectl login` opnieuw uit te voeren.

## <a name="iotedgectl-cant-find-docker"></a>iotedgectl can't find Docker
De opdracht setup of start kan niet worden uitgevoerd door iotedgectl en het volgende bericht wordt vastgelegd in de logboeken:
```output
File "/usr/local/lib/python2.7/dist-packages/edgectl/host/dockerclient.py", line 98, in get_os_type
  info = self._client.info()
File "/usr/local/lib/python2.7/dist-packages/docker/client.py", line 174, in info
  return self.api.info(*args, **kwargs)
File "/usr/local/lib/python2.7/dist-packages/docker/api/daemon.py", line 88, in info
  return self._result(self._get(self._url("/info")), True)
```

### <a name="root-cause"></a>Hoofdoorzaak
iotedgectl can't find Docker, which is a pre-requisite.

### <a name="resolution"></a>Oplossing
Docker installeren, controleren of het programma actief is en het nogmaals proberen.

## <a name="next-steps"></a>Volgende stappen
Denkt u dat u een fout op het IoT Edge-platform hebt gevonden? [Verzend een probleem](https://github.com/Azure/iot-edge/issues) zodat we het product verder kunnen blijven verbeteren. 
