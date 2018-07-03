---
title: Azure IoT Edge op Windows installeren met Windows-containers | Microsoft Docs
description: Azure IoT Edge installatie-instructies op Windows met Windows-containers
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: b2878f9d7aa32b2502487cdc081896e5ba306af1
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37346373"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-windows-containers"></a>Azure IoT Edge-runtime installeren op Windows voor gebruik met Windows-containers

De Azure IoT Edge-runtime wordt geïmplementeerd op alle IoT Edge-apparaten. Het bevat drie onderdelen. De **IoT Edge security daemon** biedt en onderhoudt beveiligingsstandaarden op het Edge-apparaat. De daemon begint op elke keer opstarten en bootstrapt van het apparaat door de IoT Edge-agent wordt gestart. De **IoT Edge agent** vergemakkelijkt de implementatie en bewaking van modules op het Edge-apparaat, met inbegrip van de IoT Edge hub. Met de **IoT Edge-hub** wordt de communicatie tussen modules op het IoT Edge-apparaat en tussen het apparaat en IoT Hub beheerd.

In dit artikel vindt u de stappen voor het installeren van de Azure IoT Edge-runtime op uw Windows x64 (AMD/Intel) systeem. 

Windows-ondersteuning is momenteel in Preview.

## <a name="supported-windows-versions"></a>Ondersteunde versies van Windows
Azure IoT Edge met Windows-containers kunnen worden gebruikt met:
  * Windows 10 IoT Enterprise/IoT Core met April 2018 update (17134 bouwen).
  * WindowsServer 1803

## <a name="install-the-container-runtime"></a>De container-runtime installeren 

>[!NOTE]
>Volg de stappen uit voor container engine-installatie op Windows IoT Core [inrichten van een IoT Core apparaat artikel] [ lnk-iot-core] en ga verder met de onderstaande instructies.

Azure IoT Edge is afhankelijk van een [OCI-compatibele] [ lnk-oci] container runtime (bijvoorbeeld Docker). U kunt [Docker voor Windows] [ lnk-docker-for-windows] voor ontwikkelen en testen. 

**Zorg ervoor dat Docker voor Windows is [geconfigureerd voor het gebruik van Windows-containers][lnk-docker-config]**

## <a name="install-the-azure-iot-edge-security-daemon"></a>De Daemon van de beveiliging met Azure IoT Edge installeren

>[!NOTE]
>Azure IoT Edge-softwarepakketten zijn afhankelijk van de licentievoorwaarden die zich in de pakketten (in de map LICENSE). Lees de licentievoorwaarden voordat u het pakket. De installatie en het gebruik van het pakket wordt verstaan onder uw acceptatie van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, moet u het pakket niet gebruiken.

### <a name="download-the-edge-daemon-package-and-install"></a>Het Edge-daemon-pakket downloaden en installeren

Voer de volgende opdrachten in een beheerder PowerShell-venster:

```powershell
Invoke-WebRequest https://aka.ms/iotedged-windows-latest -o .\iotedged-windows.zip
Expand-Archive .\iotedged-windows.zip C:\ProgramData\iotedge -f
Move-Item c:\ProgramData\iotedge\iotedged-windows\* C:\ProgramData\iotedge\ -Force
rmdir C:\ProgramData\iotedge\iotedged-windows
$env:Path += ";C:\ProgramData\iotedge"
SETX /M PATH "$env:Path"
```

Installeer de vcruntime gebruiken (u kunt deze stap overslaan op een IoT core Edge-apparaat):

```powershell
Invoke-WebRequest -useb https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe -o vc_redist.exe
.\vc_redist.exe /quiet /norestart
 ```

Maak en start **iotedge** service:

```powershell
New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
Start-Service iotedge
```

Firewall-uitzonderingen voor de poorten die door de service toevoegen:

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

## <a name="configure-the-azure-iot-edge-security-daemon"></a>De Daemon van de beveiliging met Azure IoT Edge configureren

De daemon kan worden geconfigureerd met behulp van het configuratiebestand op `C:\ProgramData\iotedge\config.yaml`.

Het edge-apparaat kan worden geconfigureerd handmatig met behulp van een [apparaatverbindingsreeks] [ lnk-dcs] of [automatisch via Device Provisioning Service] [ lnk-dps].

* Handmatige configuratie, verwijder opmerkingen bij de **handmatige** Inrichtingsmethode. Werk de waarde van **device_connection_string** door de verbindingsreeks van uw IoT Edge-apparaat.

   ```yaml
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # provisioning: 
   #   source: "dps"
   #   global_endpoint: "https://global.azure-devices-provisioning.net"
   #   scope_id: "{scope_id}"
   #   registration_id: "{registration_id}"
   ```

* Voor automatische configuratie, verwijder de opmerking bij de **dps** Inrichtingsmethode. De waarden voor **scope_id** en **registration_id** door de waarden van uw IoT Hub-DPS-exemplaar en uw IoT Edge-apparaat met TPM. 

   ```yaml
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   provisioning: 
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     registration_id: "{registration_id}"
   ```

Haal de naam van het gebruik van edge-apparaat `hostname` opdracht in PowerShell en stel deze in als de waarde voor **hostnaam:** in de configuratie van yaml. Bijvoorbeeld:

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

Vervolgens geeft u het ip-adres en poort voor **workload_uri** en **management_uri** in de **verbinding:** en **luisteren:** secties van de de configuratie.

Als u wilt ophalen van uw ip-adres, voer `ipconfig` in uw PowerShell-venster en kopieert u het ip-adres van de **vEthernet (nat)** interface zoals wordt weergegeven in het volgende voorbeeld (het ip-adres op uw systeem kan afwijken):  

![nat][img-nat]

Update de **workload_uri** en **management_uri** in de **verbinding:** gedeelte van het configuratiebestand. Vervang **\<GATEWAY_ADDRESS\>** met het IP-adres dat u hebt gekopieerd. 

```yaml
connect:
  management_uri: "http://<GATEWAY_ADDRESS>:15580"
  workload_uri: "http://<GATEWAY_ADDRESS>:15581"
```

Geef de dezelfde adressen in de **luisteren:** sectie van de configuratie, met behulp van uw IP-adres als de gateway-adres.

```yaml
listen:
  management_uri: "http://<GATEWAY_ADDRESS>:15580"
  workload_uri: "http://<GATEWAY_ADDRESS>:15581"
```

In het PowerShell-venster, maakt u een omgevingsvariabele **IOTEDGE_HOST** met de **management_uri** adres.

```powershell
[Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://<GATEWAY_ADDRESS>:15580")
```

Ten slotte, zorg ervoor dat de **netwerk:** bij **moby_runtime:** commentaarteken en ingesteld op **nat**

```yaml
moby_runtime:
  docker_uri: "npipe://./pipe/docker_engine"
  network: "nat"
```

Sla het configuratiebestand en de service opnieuw starten:

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

Als u gebruikt de **handmatige configuratie** stappen in de vorige sectie, IoT Edge-runtime moet is ingericht en wordt uitgevoerd op uw apparaat. Als u gebruikt de **automatische configuratie** stappen, moet u enkele extra stappen uitvoeren zodat de runtime uw apparaat met uw IoT-hub uit uw naam registreren kan. Zie voor de volgende stappen [maken en inrichten van een gesimuleerd TPM-Edge-apparaat op Windows](how-to-auto-provision-simulated-device-windows.md#create-a-tpm-environment-variable).

U kunt de status van de IoT Edge-service door te controleren: 

```powershell
Get-Service iotedge
```

Controleer Logboeken voor de laatste 5 minuten met behulp van:

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

Als u problemen ondervindt met het Edge-runtime installeren goed, bekijk de [probleemoplossing] [ lnk-trouble] pagina.


<!-- Images -->
[img-nat]: ./media/how-to-install-iot-edge-windows-with-windows/nat.png

<!-- Links -->
[lnk-docker-config]: https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-windows.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
[lnk-docker-for-windows]: https://www.docker.com/docker-windows
[lnk-iot-core]: how-to-install-iot-core.md

