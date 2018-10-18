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
ms.openlocfilehash: ac729963e63bd97c83719e21dad3ad2cfc9b4fee
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49392791"
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>IoT Edge-runtime installeren op Windows IoT Core - preview

Azure IoT Edge en [Windows IoT Core](https://docs.microsoft.com/windows/iot-core/) werken samen om in te schakelen edge computing op zelfs kleine apparaten. De Azure IoT Edge-Runtime kunt uitvoeren, zelfs op kleine één bord Computer SBC ()-apparaten die zeer in de branche IoT heersen. 

Dit artikel helpt bij het inrichten van de runtime op een ontwikkelbord met Windows IoT Core. 

**Op dit moment ondersteunt Windows IoT Core Azure IoT Edge alleen op Intel x64 64-processors.**

## <a name="install-the-container-runtime"></a>De container-runtime installeren

1. Configureren van het bord met **bouwen 17134 (RS4)** IoT Core-installatiekopie. 
1. Schakel het apparaat vervolgens [aanmelden op afstand met PowerShell](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell).
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

De Daemon van IoT Edge Security installeren en configureren met behulp van instructies in [in dit artikel](how-to-install-iot-edge-windows-with-windows.md)

## <a name="next-steps"></a>Volgende stappen

Nu u een apparaat met IoT Edge-runtime hebt, kunt u leren hoe u [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md).