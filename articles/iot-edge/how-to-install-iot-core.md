---
title: Azure IoT-rand installeren op IoT Core | Microsoft Docs
description: Azure IoT rand runtime installeren op een apparaat met Windows IoT Core
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: be2a80645d23e709d6c5cfb3978498bbe85eca34
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Installeren van de rand van de IoT-runtime op Windows IoT Core - voorbeeld

De Azure IoT rand Runtime kunt zelfs op kleine één mededelingenbord Computer SBC ()-apparaten die zeer gangbare in de branche IoT zijn uitvoeren. Dit artikel begeleidt u bij het inrichten van de runtime op een [MinnowBoard tarbot] [ lnk-minnow] ontwikkeling mededelingenbord Windows IoT Core uitgevoerd.

## <a name="install-the-runtime"></a>Installeer de runtime

1. Installeer [Windows 10 IoT Core Dashboard] [ lnk-core] op een hostsysteem.
1. Volg de stappen in [instellen van uw apparaat] [ lnk-board] het mededelingenbord configureren met de installatiekopie van het MinnowBoard tarbot/MAX bouwen 16299. 
1. Schakel op het apparaat vervolgens [Meld u aan met PowerShell op afstand][lnk-powershell].
1. In de PowerShell-console [Docker binaire bestanden installeren][lnk-docker-install].
1. Voer de volgende opdracht in de PowerShell-console installeren van de rand van de IoT-runtime en controleer de configuratie:

   ```powershell
   Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
   ```

   Dit script biedt het volgende: 
   * Python 3.6
   * Het script in IoT rand besturingselement (iotedgectl.exe)

Mogelijk ziet u informatie van de uitvoer van het hulpprogramma iotedgectl.exe in rood in het externe PowerShell-venster. Dit wordt niet per se duiden op fouten. 

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