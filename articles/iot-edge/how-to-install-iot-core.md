---
title: Azure IoT Edge op IoT Core installeren | Microsoft Docs
description: De Azure IoT Edge-runtime installeren op een Windows IoT Core-apparaat
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 03/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: f57db00894dab80f96f45111331d47a173520ced
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39575995"
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>IoT Edge-runtime installeren op Windows IoT Core - preview

Azure IoT Edge en [Windows IoT Core](https://docs.microsoft.com/windows/iot-core/) werken samen om in te schakelen edge computing op zelfs kleine apparaten. De Azure IoT Edge-Runtime kunt uitvoeren, zelfs op kleine één bord Computer SBC ()-apparaten die zeer in de branche IoT heersen. 

Dit artikel helpt bij het inrichten van de runtime op een ontwikkelbord met Windows IoT Core. 

**Op dit moment ondersteunt Windows IoT Core Azure IoT Edge alleen op Intel x64 64-processors.**

## <a name="install-the-container-runtime"></a>De container-runtime installeren

1. Configureren van het bord met **bouwen 17134 (RS4)** IoT Core-installatiekopie. 
1. Schakel het apparaat vervolgens [aanmelden op afstand met PowerShell][lnk-powershell].
1. In de PowerShell-console installeert u de container-runtime: 

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
   >Deze container-runtime afkomstig is van de Moby project build-server en is bedoeld voor evaluatiedoeleinden. Het is niet getest, goedgekeurd of ondersteund door Docker.

## <a name="finish-installing"></a>De installatie voltooid

De Daemon van IoT Edge Security installeren en configureren met behulp van instructies in [in dit artikel][lnk-install-windows-on-windows]

## <a name="next-steps"></a>Volgende stappen

Nu u een apparaat met IoT Edge-runtime hebt, kunt u leren hoe u [implementeren en controleren van IoT Edge-modules op schaal][lnk-deploy].

<!--Links-->
[lnk-install-windows-on-windows]: how-to-install-iot-edge-windows-with-windows.md
[lnk-powershell]: https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell
[lnk-deploy]: how-to-deploy-monitor.md
[lnk-docker-install]: https://docs.docker.com/engine/installation/linux/docker-ce/binaries#install-server-and-client-binaries-on-windows
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
