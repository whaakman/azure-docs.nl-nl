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
ms.openlocfilehash: 413b94c1f1845e0dcda54b04882e5d6664b81380
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48815491"
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Veelvoorkomende problemen en oplossingen voor Azure IoT Edge

Als u problemen hebt met het uitvoeren van Azure IoT Edge in uw omgeving, kunt u dit artikel als richtlijn gebruiken voor het oplossen van problemen. 

## <a name="standard-diagnostic-steps"></a>Standaard diagnostische stappen 

Wanneer u een probleem ondervindt, kunt u meer informatie verzamelen over de status van uw Azure IoT Edge-apparaat aan de hand van de containerlogboeken en -berichten die naar en van het apparaat worden doorgegeven. Gebruik de opdrachten en hulpprogramma's in deze sectie om informatie te verzamelen. 

### <a name="check-the-status-of-the-iot-edge-security-manager-and-its-logs"></a>Controleer de status van de IoT Edge Security Manager en de logboeken:

Op Linux:
- De status van de IoT Edge Security Manager weergeven:

   ```bash
   sudo systemctl status iotedge
   ```

- De logboeken van de IoT Edge Security Manager weergeven:

    ```bash
    sudo journalctl -u iotedge -f
    ```

- Gedetailleerde logboeken van de IoT Edge Security Manager om informatie weer te geven:

   - De instellingen van de daemon iotedge bewerken:

      ```bash
      sudo systemctl edit iotedge.service
      ```
   
   - Werk de volgende regels:
    
      ```
      [Service]
      Environment=IOTEDGE_LOG=edgelet=debug
      ```
    
   - De IoT Edge Security-Daemon opnieuw:
    
      ```bash
      sudo systemctl cat iotedge.service
      sudo systemctl daemon-reload
      sudo systemctl restart iotedge
      ```

In Windows:
- De status van de IoT Edge Security Manager weergeven:

   ```powershell
   Get-Service iotedge
   ```

- De logboeken van de IoT Edge Security Manager weergeven:

   ```powershell
   # Displays logs from today, newest at the bottom.
 
   Get-WinEvent -ea SilentlyContinue `
   -FilterHashtable @{ProviderName= "iotedged";
     LogName = "application"; StartTime = [datetime]::Today} |
   select TimeCreated, Message |
   sort-object @{Expression="TimeCreated";Descending=$false} |
   format-table -autosize -wrap
   ```

### <a name="if-the-iot-edge-security-manager-is-not-running-verify-your-yaml-configuration-file"></a>Als de IoT Edge Security Manager wordt niet uitgevoerd, controleert u of uw yaml-configuratiebestand

> [!WARNING]
> YAML-bestanden mag geen tabbladen als inspringing bevatten. Gebruik in plaats daarvan 2 spaties.

Op Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

In Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

### <a name="check-container-logs-for-issues"></a>Raadpleeg de containerlogboeken voor problemen

Zodra de IoT Edge Security-Daemon wordt uitgevoerd, bekijkt u de logboeken van de containers om problemen te detecteren. Begin met uw geïmplementeerde containers en kijk vervolgens naar de containers die gezamenlijk de IoT Edge-runtime vormen: Edge Agent en Edge Hub. De Edge Agent-logboeken bieden doorgaans informatie over de levenscyclus van elke container. De Edge Hub-logboeken bieden informatie over berichten en routering. 

   ```cmd
   iotedge logs <container name>
   ```

### <a name="view-the-messages-going-through-the-edge-hub"></a>Bekijk de berichten die via Edge hub gaan

Bekijk de berichten die via Edge hub gaan en verzamel inzichten over updates van apparaateigenschappen met uitgebreide Logboeken uit de runtimecontainers edgeAgent en edgeHub. Als u wilt inschakelen op de uitgebreide logboeken op deze containers, instellen `RuntimeLogLevel` in uw configuratiebestand yaml. Open het bestand:

Op Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

In Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

Standaard de `agent` element ziet er ongeveer als volgt:

   ```yaml
   agent:
     name: edgeAgent
     type: docker
     env: {}
     config:
       image: mcr.microsoft.com/azureiotedge-agent:1.0
       auth: {}
   ```

Vervang `env: {}` met:

> [!WARNING]
> YAML-bestanden mag geen tabbladen als inspringing bevatten. Gebruik in plaats daarvan 2 spaties.

   ```yaml
   env:
     RuntimeLogLevel: debug
   ```

Sla het bestand op en start de IoT Edge security manager opnieuw.

U kunt ook de berichten controleren die worden verzonden tussen IoT Hub en de IoT Edge-apparaten. Bekijk deze berichten met behulp van de [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)-extensie voor Visual Studio Code. Zie [Handig hulpprogramma bij het ontwikkelen met Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/) voor meer richtlijnen.

### <a name="restart-containers"></a>Opnieuw opstarten van containers
Na onderzoek van de logboeken en berichten voor meer informatie, kunt u proberen opnieuw te starten containers:

```
iotedge restart <container name>
```

Start opnieuw op de IoT Edge-runtime-containers:

```
iotedge restart edgeAgent && iotedge restart edgeHub
```

### <a name="restart-the-iot-edge-security-manager"></a>Opnieuw opstarten van de IoT Edge-beveiligingsbeheer

Als het probleem nog steeds in de persistent maken, kunt u proberen de IoT Edge security manager opnieuw te starten.

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
Zorg ervoor dat de registerreferenties van uw juist zijn opgegeven in uw manifest implementatie

## <a name="iot-edge-security-daemon-fails-with-an-invalid-hostname"></a>IoT Edge security-daemon is mislukt met een ongeldige hostnaam

De opdracht `sudo journalctl -u iotedge` mislukt dat en wordt het volgende bericht: 

```output
Error parsing user input data: invalid hostname. Hostname cannot be empty or greater than 64 characters
```

### <a name="root-cause"></a>Hoofdoorzaak
IoT Edge-runtime biedt alleen ondersteuning voor hostnamen die korter zijn dan 64 tekens lang zijn. Dit meestal is een probleem voor fysieke computers, maar kan zich voordoen bij het instellen van de runtime op een virtuele machine. De automatisch gegenereerde hostnamen voor Windows virtuele machines die worden gehost in Azure, meestal in het bijzonder te lang. 

### <a name="resolution"></a>Oplossing
Wanneer u deze fout ziet, kunt u deze kunt oplossen door de DNS-naam van uw virtuele machine configureren en vervolgens de DNS-naam als de hostnaam in de setup-opdracht in te stellen.

1. Navigeer naar de overzichtspagina van de virtuele machine in Azure portal. 
2. Selecteer **configureren** onder DNS-naam. Als uw virtuele machine is al een DNS-naam die is geconfigureerd, hoeft u niet te configureren van een nieuwe. 

   ![Configureren van DNS-naam](./media/troubleshoot/configure-dns.png)

3. Geef een waarde voor **DNS-naamlabel** en selecteer **opslaan**.
4. Kopiëren van de nieuwe DNS-naam, de indeling moet  **\<DNSnamelabel\>.\< VM-locatie\>. cloudapp.azure.com**.
5. Gebruik de volgende opdracht voor het instellen van de IoT Edge-runtime met uw DNS-naam in de virtuele machine:

   - Op Linux:

      ```bash
      sudo nano /etc/iotedge/config.yaml
      ```

   - In Windows:

      ```cmd
      notepad C:\ProgramData\iotedge\config.yaml
      ```

## <a name="stability-issues-on-resource-constrained-devices"></a>Problemen met de stabiliteit van bron beperkte apparaten 
U kunt de stabiliteitsproblemen op beperkte apparaten, zoals de Raspberry Pi, ondervinden met name wanneer die wordt gebruikt als een gateway. Symptomen zijn buiten het geheugen uitzonderingen in de edge hub-module, downstream apparaten kunnen geen verbinding maken of het apparaat reageert verzenden van berichten over telemetrie na een paar uur.

### <a name="root-cause"></a>Hoofdoorzaak
De edge hub, die deel van de edge-runtime uitmaakt, standaard is geoptimaliseerd voor prestaties en grote hoeveelheden geheugen wordt toegewezen. Dit is niet ideaal voor beperkte edge-apparaten en stabiliteitsproblemen kan veroorzaken.

### <a name="resolution"></a>Oplossing
Voor de edge hub stelt de omgevingsvariabele **OptimizeForPerformance** naar **false**. Er zijn twee manieren om dit te doen:

In de gebruikersinterface: 

In de portal van *Apparaatdetails*->*Modules instellen*->*geavanceerde instellingen voor Edge-Runtime configureren*, maken van een omgevingsvariabele met de naam *OptimizeForPerformance* die is ingesteld op *false* voor de *Edge Hub*.

![optimizeforperformance][img-optimize-for-perf]

**OR**

In het manifest van de implementatie:

```json
  "edgeHub": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
      "createOptions": <snipped>
    },
    "env": {
      "OptimizeForPerformance": {
          "value": "false"
      }
    },
```
## <a name="cant-get-the-iot-edge-daemon-logs-on-windows"></a>Kan niet op Windows op de IoT Edge daemon-logboeken ophalen
Als u een EventLogException ontvangt wanneer u `Get-WinEvent` op Windows, controleert u de Register-invoer.

### <a name="root-cause"></a>Hoofdoorzaak
De `Get-WinEvent` PowerShell-opdracht is afhankelijk van een registervermelding moet aanwezig zijn om te zoeken van Logboeken op een specifieke `ProviderName`.

### <a name="resolution"></a>Oplossing
Een registervermelding voor de IoT Edge-daemon instellen. Maak een **iotedge.reg** bestand met de volgende inhoud en importeren naar het Windows-register door erop te dubbelklikken of met behulp van de `reg import iotedge.reg` opdracht:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="iot-edge-module-fails-to-send-a-message-to-the-edgehub-with-404-error"></a>IoT Edge-module is mislukt voor het verzenden van een bericht naar de edgeHub met 404-fout

Een aangepaste IoT Edge-module is mislukt voor het verzenden van een bericht naar de edgeHub met een 404-fout `Module not found` fout. De IoT Edge-daemon afdrukken van het volgende bericht naar de logboeken: 

```output
Error: Time:Thu Jun  4 19:44:58 2018 File:/usr/sdk/src/c/provisioning_client/adapters/hsm_client_http_edge.c Func:on_edge_hsm_http_recv Line:364 executing HTTP request fails, status=404, response_buffer={"message":"Module not found"}u, 04 ) 
```

### <a name="root-cause"></a>Hoofdoorzaak
De IoT Edge-daemon wordt afgedwongen proces-id in voor alle modules die verbinding maken met de edgeHub uit veiligheidsoverwegingen. Hiermee wordt gecontroleerd dat alle berichten worden verzonden door een module afkomstig van de belangrijkste proces-id van de module zijn. Als een bericht wordt verzonden door een module van de id van een ander proces dan in eerste instantie tot stand gebracht, wordt deze het bericht met een 404-fout-bericht negeren.

### <a name="resolution"></a>Oplossing
Zorg ervoor dat de hetzelfde proces-id altijd door de aangepaste IoT Edge-module gebruikt wordt om berichten te verzenden naar de edgeHub. Bijvoorbeeld, zorg ervoor dat u `ENTRYPOINT` in plaats van `CMD` opdracht in uw Docker-bestand, aangezien `CMD` zal leiden tot een proces-id voor de module en een ander proces-id voor de bash-opdracht uitvoeren van de belangrijkste programma dat `ENTRYPOINT` zal leiden tot een een enkel proces-id.


## <a name="next-steps"></a>Volgende stappen
Denkt u dat u een fout op het IoT Edge-platform hebt gevonden? [Verzend een probleem](https://github.com/Azure/iotedge/issues) zodat we het product verder kunnen blijven verbeteren. 

<!-- Images -->
[img-optimize-for-perf]: ./media/troubleshoot/OptimizeForPerformanceFalse.png
