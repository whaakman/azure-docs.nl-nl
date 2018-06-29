---
title: Azure IoT-rand installeren op IoT Core | Microsoft Docs
description: Azure IoT rand runtime installeren op een apparaat met Windows IoT Core
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 03/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ae5644a62b794dc8d6ace52f21a452fa70027d39
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029557"
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Installeren van de rand van de IoT-runtime op Windows IoT Core - voorbeeld

Azure IoT-rand en [Windows IoT Core](https://docs.microsoft.com/windows/iot-core/) werken samen om te schakelen computergebruik op zelfs kleine apparaten rand. De Azure IoT rand Runtime kunt zelfs op kleine één mededelingenbord Computer SBC ()-apparaten die zeer gangbare in de branche IoT zijn uitvoeren. 

Dit artikel begeleidt bij het inrichten van de runtime op een kaart voor ontwikkeling Windows IoT Core uitgevoerd. 

**Op dit moment ondersteunt Windows IoT Core Azure IoT rand alleen op x64-bits Intel-processors.**

## <a name="install-the-container-runtime"></a>Installeren van de container-runtime

1. Configureer het mededelingenbord met **17134 bouwen (RS4)** IoT Core installatiekopie. 
1. Schakel op het apparaat vervolgens [Meld u aan met PowerShell op afstand][lnk-powershell].
1. Installeer de runtime container in de PowerShell-console: 

   ```powershell
   Invoke-WebRequest https://master.dockerproject.org/windows/x86_64/docker-0.0.0-dev.zip -o temp.zip
   Expand-Archive .\temp.zip $env:ProgramFiles -f
   Remove-Item .\temp.zip
   $env:Path += ";$env:programfiles\docker"
   SETX /M PATH "$env:Path"
   dockerd --register-service
   start-service docker
   ```

   >[!NOTE]
   >Deze container runtime is van de server Moby project bouwen en is bedoeld voor evaluatiedoeleinden. Het is niet getest, goedgekeurd door of ondersteund door Docker.

## <a name="finish-installing"></a>De installatie voltooid

De Daemon IoT rand Security installeren en configureren met behulp van de instructies in [in dit artikel][lnk-install-windows-on-windows]

## <a name="next-steps"></a>Volgende stappen

Nu u een apparaat met de runtime IoT rand hebt, kunt u nagaan hoe [implementeren en controleren van de rand van de IoT-modules op grote schaal][lnk-deploy].

<!--Links-->
[lnk-install-windows-on-windows]: how-to-install-iot-edge-windows-with-windows.md#download-the-edge-daemon-package-and-install
[lnk-powershell]: https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell
[lnk-deploy]: how-to-deploy-monitor.md
[lnk-docker-install]: https://docs.docker.com/engine/installation/linux/docker-ce/binaries#install-server-and-client-binaries-on-windows
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers