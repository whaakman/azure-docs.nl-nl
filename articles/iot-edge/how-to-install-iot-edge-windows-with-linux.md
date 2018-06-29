---
title: Azure IoT rand installeren op Windows met Linux containers | Microsoft Docs
description: Azure IoT rand installatie-instructies in Windows met Linux-containers
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: cd517d7e652b38c7ecf28a17657936698416413a
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035874"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-linux-containers"></a>Azure IoT rand runtime installeren op Windows gebruiken met Linux-containers

De runtime Azure IoT-rand wordt geïmplementeerd op alle rand van de IoT-apparaten. Er zijn drie onderdelen gedefinieerd. De **IoT rand beveiliging daemon** biedt en onderhoudt beveiligingsstandaarden op het apparaat aan de rand. De daemon begint op elke keer opstarten en het apparaat bootstraps door de rand van de IoT-agent wordt gestart. De **IoT rand agent** vereenvoudigt de implementatie en controle van modules op het apparaat aan de rand, met inbegrip van de rand van de IoT-hub. Met de **IoT Edge-hub** wordt de communicatie tussen modules op het IoT Edge-apparaat en tussen het apparaat en IoT Hub beheerd.

In dit artikel vindt u de stappen voor het installeren van de rand van Azure IoT-runtime op uw Windows-x64 (AMD/Intel) systeem. Ondersteuning voor Windows is momenteel in Preview.

>[!NOTE]
Met behulp van Linux-containers op Windows sytems wordt productconfiguratie niet aanbevolen of ondersteunde voor Azure IoT rand. Het kan echter worden gebruikt voor ontwikkel- en testdoeleinden.

## <a name="supported-windows-versions"></a>Ondersteunde Windows-versies
Azure IoT-rand kan worden gebruikt voor ontwikkeling en testen op de volgende versies van Windows, bij gebruik van Linux-containers:
  * Windows 10 of nieuwere besturingssystemen bureaublad.
  * Windows Server 2016 of nieuwe server-besturingssystemen.

## <a name="install-the-container-runtime"></a>Installeren van de container-runtime 

Azure IoT-rand is afhankelijk van een [OCI-compatibele] [ lnk-oci] container runtime (bijvoorbeeld Docker). 

U kunt [Docker voor Windows] [ lnk-docker-for-windows] voor ontwikkeling en testen. Zorg ervoor dat Docker voor Windows [geconfigureerd voor het gebruik van Linux-containers][lnk-docker-config]

## <a name="install-the-azure-iot-edge-security-daemon"></a>De Daemon van de beveiliging Azure IoT-rand installeren

>[!NOTE]
>Azure IoT rand softwarepakketten zijn onderworpen aan de licentievoorwaarden die zich in de pakketten (in de map LICENSE). Lees de licentievoorwaarden voordat het pakket. De installatie en het gebruik van het pakket wordt verstaan onder de bevestiging van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, gebruik niet het pakket.

### <a name="download-the-edge-daemon-package-and-install"></a>De Edge-daemon downloaden en installeren

Voer de volgende opdrachten in een beheerder PowerShell-venster:

```powershell
Invoke-WebRequest https://aka.ms/iotedged-windows-latest -o .\iotedged-windows.zip
Expand-Archive .\iotedged-windows.zip C:\ProgramData\iotedge -f
Move-Item c:\ProgramData\iotedge\iotedged-windows\* C:\ProgramData\iotedge\ -Force
rmdir C:\ProgramData\iotedge\iotedged-windows
$env:Path += ";C:\ProgramData\iotedge"
SETX /M PATH "$env:Path"
```

Installeer de vcruntime met:

```powershell
Invoke-WebRequest -useb https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe -o vc_redist.exe
.\vc_redist.exe /quiet /norestart
 ```

Maak en start *iotedge* service:

```powershell
New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
Start-Service iotedge
```

Firewalluitzonderingen voor de poorten gebruikt door de service toevoegen:

```powershell
New-NetFirewallRule -DisplayName "iotedged allow inbound 15580,15581" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 15580-15581 -Program "C:\programdata\iotedge\iotedged.exe" -InterfaceType Any
```

Maak een **iotedge.reg** bestand met de volgende inhoud en importeren naar het Windows-register door erop te dubbelklikken of met behulp van de `reg import iotedge.reg` opdracht:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>De Daemon van Azure IoT-rand beveiliging configureren

De daemon kan worden geconfigureerd met behulp van het configuratiebestand op `C:\ProgramData\iotedge\config.yaml` apparaat aan de rand kan worden geconfigureerd <!--[automatically via Device Provisioning Service][lnk-dps] or--> handmatig met een [apparaat verbindingsreeks][lnk-dcs].

Voer de apparaat-verbindingsreeks in voor de handmatige configuratie **inrichten:** sectie van **config.yaml**

```yaml
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
```

De naam van het edge-apparaten met `hostname` opdracht in PowerShell en stel dit in als de waarde voor **hostnaam:** in de configuratie yaml. Bijvoorbeeld:

```yaml
  ###############################################################################
  # Edge device hostname
  ###############################################################################
  #
  # Configures the environment variable 'IOTEDGE_GATEWAYHOSTNAME' injected into
  # modules.
  #
  ###############################################################################

  hostname: "edgedevice-1"
```

Vervolgens moet u het IP-adres en poort voor **workload_uri** en **management_uri** in de **verbinding:** sectie van de configuratie.

Voer voor het IP-adres, `ipconfig` in uw PowerShell-venster en selecteert u het IP-adres van de **vEthernet (DockerNAT)**' interface zoals weergegeven in het onderstaande voorbeeld (mogelijk is het IP-adres op uw systeem verschillende):

![DockerNat][img-docker-nat]

```yaml
connect:
  management_uri: "http://10.0.75.1:15580"
  workload_uri: "http://10.0.75.1:15581"
```

Voer dezelfde adressen in de **luisteren:** sectie van de configuratie. Bijvoorbeeld:

```yaml
listen:
  management_uri: "http://10.0.75.1:15580"
  workload_uri: "http://10.0.75.1:15581"
```

In de PowerShell-venster maken van een omgevingsvariabele **IOTEDGE_HOST** met de **management_uri** adres, bijvoorbeeld:

```powershell
[Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://10.0.75.1:15580")
```

Controleer ten slotte de **netwerk:** onder **moby_runtime:** zonder opmerkingen en ingesteld op **azure-iot-edge**

```yaml
moby_runtime:
  docker_uri: "npipe://./pipe/docker_engine"
  network: "azure-iot-edge"
```

Sla het configuratiebestand en de service opnieuw starten:

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

U kunt de status van de rand van de IoT-service controleren: 

```powershell
Get-Service iotedge
```

Controleer de servicelogboeken van de laatste vijf minuten met:

```powershell

# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false}
```

En lijst met modules met:

```powershell
iotedge list
```

## <a name="next-steps"></a>Volgende stappen

Als u problemen ondervindt met de installatie naar behoren afhandeling Edge-runtime de [probleemoplossing] [ lnk-trouble] pagina.


<!-- Images -->
[img-docker-nat]: ./media/how-to-install-iot-edge-windows-with-linux/dockernat.png

<!-- Links -->
[lnk-docker-config]: https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers
[lnk-dcs]: ../iot-hub/quickstart-send-telemetry-dotnet.md#register-a-device
[lnk-dps]: how-to-simulate-dps-tpm.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
[lnk-docker-for-windows]: https://www.docker.com/docker-windows

