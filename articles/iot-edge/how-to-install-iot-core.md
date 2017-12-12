---
title: Azure IoT-rand installeren op IoT Core | Microsoft Docs
description: Azure IoT rand runtime installeren op een apparaat met Windows IoT Core
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: cc34e5cecafe485608ba428395b690ba57f71e9c
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Installeren van de rand van de IoT-runtime op Windows IoT Core - voorbeeld

Azure IoT-rand en [Windows IoT Core](https://docs.microsoft.com/windows/iot-core/) werken samen om te schakelen computergebruik op zelfs kleine apparaten rand. De Azure IoT rand Runtime kunt zelfs op kleine één mededelingenbord Computer SBC ()-apparaten die zeer gangbare in de branche IoT zijn uitvoeren. 

Dit artikel begeleidt u bij het inrichten van de runtime op een [MinnowBoard tarbot] [ lnk-minnow] ontwikkeling mededelingenbord Windows IoT Core uitgevoerd. Azure IoT rand ondersteunt Windows IoT Core alleen op x64-bits Intel-processors. 

## <a name="install-the-runtime"></a>Installeer de runtime

1. Installeer [Windows 10 IoT Core Dashboard] [ lnk-core] op een hostsysteem.
1. Volg de stappen in [instellen van uw apparaat] [ lnk-board] het mededelingenbord configureren met de installatiekopie van het MinnowBoard tarbot/MAX bouwen 16299. 
1. Schakel op het apparaat vervolgens [Meld u aan met PowerShell op afstand][lnk-powershell].
1. Installeer de runtime container in de PowerShell-console: 

   ```powershell
   Invoke-WebRequest https://master.dockerproject.org/windows/x86_64/docker-17.06.0-dev.zip -o temp.zip
   Expand-Archive .\temp.zip $env:ProgramFiles -f
   Remove-Item .\temp.zip
   $env:Path += ";$env:programfiles\docker"
   SETX /M PATH "$env:Path"
   dockerd --register-service
   start-service docker
   ```

   >[!NOTE]
   >Deze container runtime is van de server Moby project bouwen en is bedoeld voor evaluatiedoeleinden. Het is niet getest, goedgekeurd door of ondersteund door Docker.

1. Installeren van de rand van de IoT-runtime en controleer de configuratie:

   ```powershell
   Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
   ```

   Dit script biedt het volgende: 
   * Python 3.6
   * Het script in IoT rand besturingselement (iotedgectl.exe)

Mogelijk ziet u informatie van de uitvoer van het hulpprogramma iotedgectl.exe in groen in het externe PowerShell-venster. Dit wordt niet per se duiden op fouten. 

## <a name="next-steps"></a>Volgende stappen

Nu u een apparaat met de runtime IoT rand hebt, kunt u nagaan hoe [implementeren en controleren van de rand van de IoT-modules op grote schaal][lnk-deploy].

<!--Links-->
[lnk-minnow]: https://minnowboard.org/ 
[lnk-core]: https://docs.microsoft.com/windows/iot-core/connect-your-device/iotdashboard
[lnk-board]: https://developer.microsoft.com/windows/iot/Docs/GetStarted/mbm/sdcard/stable/getstartedstep2
[lnk-powershell]: https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell
[lnk-deploy]: how-to-deploy-monitor.md
[lnk-docker-install]: https://docs.docker.com/engine/installation/linux/docker-ce/binaries#install-server-and-client-binaries-on-windows
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
