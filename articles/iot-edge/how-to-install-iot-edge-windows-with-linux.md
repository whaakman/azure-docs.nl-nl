---
title: Over het installeren van Azure IoT Edge op Windows met Linux-containers | Microsoft Docs
description: Azure IoT Edge installatie-instructies op Windows met Linux-containers
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/06/2018
ms.author: kgremban
ms.openlocfilehash: ea576c0d434d4db7077fc41bc1f5bbbc89e7779e
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576644"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-linux-containers"></a>Azure IoT Edge-runtime installeren op Windows gebruiken met Linux-containers

De Azure IoT Edge-runtime wordt geïmplementeerd op alle IoT Edge-apparaten. Deze bevat drie onderdelen. De **IoT Edge security daemon** biedt en onderhoudt beveiligingsstandaarden op het Edge-apparaat. De daemon begint op elke keer opstarten en bootstrapt van het apparaat door de IoT Edge-agent wordt gestart. De **IoT Edge agent** vergemakkelijkt de implementatie en bewaking van modules op het Edge-apparaat, met inbegrip van de IoT Edge hub. Met de **IoT Edge-hub** wordt de communicatie tussen modules op het IoT Edge-apparaat en tussen het apparaat en IoT Hub beheerd.

In dit artikel vindt u de stappen voor het installeren van de Azure IoT Edge-runtime op uw Windows x64 (AMD/Intel) systeem. Windows-ondersteuning is momenteel in Preview.

>[!NOTE]
Met behulp van Linux-containers op Windows sytems wordt productieconfiguratie niet aanbevolen of ondersteund voor Azure IoT Edge. Het kan echter worden gebruikt voor ontwikkeling en tests.

## <a name="supported-windows-versions"></a>Ondersteunde versies van Windows
Azure IoT Edge kunnen worden gebruikt voor de ontwikkeling en testen in de volgende versies van Windows, bij het gebruik van Linux-containers:
  * Windows 10 of nieuwere pc-besturingssystemen.
  * Windows Server 2016 of nieuwe server-besturingssystemen.

## <a name="install-the-container-runtime"></a>De container-runtime installeren 

Azure IoT Edge is afhankelijk van een [OCI-compatibele] [ lnk-oci] container runtime (bijvoorbeeld Docker). 

U kunt [Docker voor Windows] [ lnk-docker-for-windows] voor ontwikkelen en testen. Docker voor Windows configureren [Linux-containers gebruiken][lnk-docker-config]

## <a name="install-the-azure-iot-edge-security-daemon"></a>De Daemon van de beveiliging met Azure IoT Edge installeren

>[!NOTE]
>Azure IoT Edge-softwarepakketten zijn afhankelijk van de licentievoorwaarden die zich in de pakketten (in de map LICENSE). Lees de licentievoorwaarden voordat u het pakket. De installatie en het gebruik van het pakket wordt verstaan onder uw acceptatie van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, moet u het pakket niet gebruiken.

Een enkele IoT Edge-apparaat kan worden ingericht handmatig met behulp van een apparaat verbindingen tekenreeks opgegeven door de IoT Hub. Of u de Device Provisioning Service kunt gebruiken voor het automatisch inrichten van apparaten, dit is handig wanneer er veel apparaten zijn om in te richten. Afhankelijk van inrichting keuze, kiest u het script voor de juiste installatie. 

### <a name="install-and-manually-provision"></a>Installeren en handmatig inrichten

1. Volg de stappen in [een nieuwe Azure IoT Edge-apparaat registreren] [ lnk-dcs] op uw apparaat registreren en de verbindingsreeks op te halen. 

2. Voer PowerShell op uw IoT Edge-apparaat uit als administrator. 

3. Download en installeer de IoT Edge-runtime. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Manual -ContainerOs Linux
   ```

4. Als u wordt gevraagd een **DeviceConnectionString**, de verbindingsreeks die u hebt opgehaald via IoT Hub opgeven. Geen aanhalingstekens rond de verbindingsreeks. 

### <a name="install-and-automatically-provision"></a>Installeren en automatisch inrichten

1. Volg de stappen in [maken en inrichten van een gesimuleerd TPM-Edge-apparaat op Windows] [ lnk-dps] de Device Provisioning Service instellen en ophalen van de **bereik-ID**, een TPM simuleren apparaat- en halen de **registratie-ID**, maakt u een afzonderlijke inschrijving. Nadat het apparaat is geregistreerd in uw IoT-Hub, gaat u verder met de installatie.  

   >[!TIP]
   >Houd het venster met de TPM-simulator openen tijdens het installeren en testen. 

2. Voer PowerShell op uw IoT Edge-apparaat uit als administrator. 

3. Download en installeer de IoT Edge-runtime. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Dps -ContainerOs Linux
   ```

4. Als u hierom wordt gevraagd, geeft u de **ScopeID** en **registratie-id** voor uw provisioning-service en het apparaat.

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

U kunt de status van de IoT Edge-service door te controleren: 

```powershell
Get-Service iotedge
```

Controleer de servicelogboeken van de laatste 5 minuten met behulp van:

```powershell

# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false} |
  format-table -autosize -wrap
```

En lijst met modules met:

```powershell
iotedge list
```

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT Edge-apparaat dat is ingericht met de runtime geïnstalleerd hebt, kunt u [IoT Edge-modules implementeren][lnk-modules].

Als u hebt met het Edge-runtime niet goed geïnstalleerd problemen, bekijk de [probleemoplossing] [ lnk-trouble] pagina.


<!-- Images -->
[img-docker-nat]: ./media/how-to-install-iot-edge-windows-with-linux/dockernat.png

<!-- Links -->
[lnk-docker-config]: https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-windows.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
[lnk-docker-for-windows]: https://www.docker.com/docker-windows
[lnk-modules]: how-to-deploy-modules-portal.md
