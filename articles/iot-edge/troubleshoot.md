---
title: Problemen met Azure IoT Edge oplossen | Microsoft Docs
description: Veelvoorkomende problemen oplossen en informatie over probleemoplossingsvaardigheden voor Azure IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: efe3e31a1a92e21f2c3a3461deba248d2a8c97fa
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029438"
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Veelvoorkomende problemen en oplossingen voor Azure IoT Edge

Als u problemen hebt met het uitvoeren van Azure IoT Edge in uw omgeving, kunt u dit artikel als richtlijn gebruiken voor het oplossen van problemen. 

## <a name="standard-diagnostic-steps"></a>Standaard diagnostische stappen 

Wanneer u een probleem ondervindt, kunt u meer informatie verzamelen over de status van uw Azure IoT Edge-apparaat aan de hand van de containerlogboeken en -berichten die naar en van het apparaat worden doorgegeven. Gebruik de opdrachten en hulpprogramma's in deze sectie om informatie te verzamelen. 

### <a name="check-the-status-of-the-iot-edge-security-manager-and-its-logs"></a>Controleer de status van de beveiligingsinstellingen van de IoT-rand en de logboeken:

Op Linux:
- De status van de beveiligingsinstellingen van IoT-rand weergeven:

   ```bash
   sudo systemctl status iotedge
   ```

- De logboeken van de beveiligingsinstellingen van IoT-rand weergeven:

    ```bash
    sudo journalctl -u iotedge -f
    ```

- Gedetailleerdere logboeken van de beveiligingsinstellingen van IoT-rand weergeven:

   - De iotedge daemon-instellingen bewerken:

      ```bash
      sudo systemctl edit iotedge.service
      ```
   
   - Update de volgende regels:
    
      ```
      [Service]
      Environment=IOTEDGE_LOG=edgelet=debug
      ```
    
   - Start opnieuw op de rand van de IoT-Daemon beveiliging:
    
      ```bash
      sudo systemctl cat iotedge.service
      sudo systemctl daemon-reload
      sudo systemctl restart iotedge
      ```

In Windows:
- De status van de beveiligingsinstellingen van IoT-rand weergeven:

   ```powershell
   Get-Service iotedge
   ```

- De logboeken van de beveiligingsinstellingen van IoT-rand weergeven:

   ```powershell
   # Displays logs from today, newest at the bottom.
 
   Get-WinEvent -ea SilentlyContinue `
   -FilterHashtable @{ProviderName= "iotedged";
     LogName = "application"; StartTime = [datetime]::Today} |
   select TimeCreated, Message |
   sort-object @{Expression="TimeCreated";Descending=$false}
   ```

### <a name="if-the-iot-edge-security-manager-is-not-running-verify-your-yaml-configuration-file"></a>Als de beveiligingsinstellingen van de IoT-rand wordt niet uitgevoerd, controleert u of uw yaml-configuratiebestand

> [!WARNING]
> YAML bestanden mogen geen tabs als inspringing. Gebruik in plaats daarvan 2 spaties.

Op Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

In Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

### <a name="check-container-logs-for-issues"></a>Controleer de logboeken van de container voor problemen

Zodra de IoT rand Security-Daemon wordt uitgevoerd, bekijkt u de logboeken van de containers voor het detecteren van problemen. Begin met uw geïmplementeerde containers en kijk vervolgens naar de containers die gezamenlijk de IoT Edge-runtime vormen: Edge Agent en Edge Hub. De Edge Agent-logboeken bieden doorgaans informatie over de levenscyclus van elke container. De Edge Hub-logboeken bieden informatie over berichten en routering. 

   ```cmd
   iotedge logs <container name>
   ```

### <a name="view-the-messages-going-through-the-edge-hub"></a>De berichten gaan via de Edge-hub weergeven

De berichten gaan via de Edge-hub kunt weergeven en inzichten op Eigenschappen apparaatupdates met uitgebreide logboeken verzamelen van de runtime edgeAgent en edgeHub containers. Om in te schakelen uitgebreide logboeken op deze containers, stel de `RuntimeLogLevel` omgevingsvariabele: 

Op Linux:
    
   ```cmd
   export RuntimeLogLevel="debug"
   ```
    
In Windows:
    
   ```powershell
   [Environment]::SetEnvironmentVariable("RuntimeLogLevel", "debug")
   ```

U kunt ook de berichten controleren die worden verzonden tussen IoT Hub en de IoT Edge-apparaten. Bekijk deze berichten met behulp van de [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)-extensie voor Visual Studio Code. Zie [Handig hulpprogramma bij het ontwikkelen met Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/) voor meer richtlijnen.

### <a name="restart-containers"></a>Opnieuw opstarten van containers
Na het onderzoeken van de logboeken en berichten voor meer informatie, kunt u proberen opnieuw te starten containers:

```
iotedge restart <container name>
```

Start opnieuw op de rand van de IoT-runtime-containers:

```
iotedge restart edgeAgent && iotedge restart edgeHub
```

### <a name="restart-the-iot-edge-security-manager"></a>Opnieuw opstarten van de rand van de IoT security manager

Als het probleem nog steeds in de persistent maken, kunt u proberen opnieuw starten van de rand van de IoT security manager.

Op Linux:

   ```cmd
   sudo systemctl restart iotedge
   ```

In Windows:

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
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
Zorg ervoor dat uw referenties register correct zijn opgegeven in uw deployment manifest

## <a name="iot-edge-security-daemon-fails-with-an-invalid-hostname"></a>IoT-rand security-daemon is mislukt met een ongeldige hostnaam

De opdracht `sudo journalctl -u iotedge` mislukt en wordt het volgende bericht: 

```output
Error parsing user input data: invalid hostname. Hostname cannot be empty or greater than 64 characters
```

### <a name="root-cause"></a>Hoofdoorzaak
De rand van de IoT-runtime biedt slechts ondersteuning voor hostnamen die korter dan 64 tekens zijn. Dit meestal geen belemmering vormt voor fysieke computers, maar kan zich voordoen bij het instellen van de runtime op een virtuele machine. De automatisch gegenereerde hostnamen voor Windows virtuele machines die worden gehost in Azure, met name hebben vaak lang zijn. 

### <a name="resolution"></a>Oplossing
Wanneer u deze fout ziet, kunt u deze kunt oplossen door de DNS-naam van uw virtuele machine configureren en de DNS-naam in te stellen als de hostnaam in de setup-opdracht.

1. Ga naar de overzichtspagina van uw virtuele machine in de Azure-portal. 
2. Selecteer **configureren** met DNS-naam. Als uw virtuele machine is al een DNS-naam is geconfigureerd, hoeft u niet te configureren van een nieuwe. 

   ![Configureer DNS-naam](./media/troubleshoot/configure-dns.png)

3. Geef een waarde voor **label DNS-naam** en selecteer **opslaan**.
4. Kopiëren van de nieuwe DNS-naam, de indeling moet  **\<DNSnamelabel\>.\< VM-locatie\>. cloudapp.azure.com**.
5. Gebruik de volgende opdracht voor het instellen van de rand van de IoT-runtime met uw DNS-naam in de virtuele machine:

   - Op Linux:

      ```bash
      sudo nano /etc/iotedge/config.yaml
      ```

   - In Windows:

      ```cmd
      notepad C:\ProgramData\iotedge\config.yaml
      ```

## <a name="next-steps"></a>Volgende stappen
Denkt u dat u een fout op het IoT Edge-platform hebt gevonden? [Verzend een probleem](https://github.com/Azure/iot-edge/issues) zodat we het product verder kunnen blijven verbeteren. 
