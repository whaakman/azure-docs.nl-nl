---
title: Oplossing voor externe bewaking toevoegen in Edge-apparaat - Azure | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een IoT Edge-apparaat toevoegt aan een oplossingsverbetering voor externe controle
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/09/2018
ms.topic: conceptual
ms.openlocfilehash: 67bfde828287d9892ad404f3d950dbe373503a56
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51828340"
---
# <a name="add-an-iot-edge-device-to-your-remote-monitoring-solution-accelerator"></a>Een IoT Edge-apparaat toevoegt aan uw oplossingsverbetering voor externe controle

Om toe te voegen een [IoT Edge](../iot-edge/about-iot-edge.md) apparaat naar uw oplossingenaccelerator, voltooi de volgende twee stappen:

1. Het Edge-apparaat toevoegen aan de **apparaten** pagina in de Remote Monitoring solution accelerator-Webgebruikersinterface.
1. IoT Edge-runtime installeren op uw Edge-apparaat.

## <a name="add-the-iot-edge-device"></a>De IoT Edge-apparaat toevoegen

Een IoT Edge-apparaat toevoegen aan de oplossingsverbetering voor externe controle, gaat u naar de **apparaten** pagina in de web-UI en op **+ nieuwe apparaat**.

In de **nieuw apparaat** deelvenster, kiest u **IoT Edge-apparaat**. U kunt de standaardwaarden voor de overige instellingen op te laten. Klik vervolgens op **Toepassen**:

![IoT Edge-apparaat toevoegen](media/iot-accelerators-remote-monitoring-add-edge-device/addedgedevice.png)

### <a name="alternative-ways-to-add-an-iot-edge-device"></a>Andere manieren om toe te voegen een IoT Edge-apparaat

Het is ook mogelijk om te registreren van een IoT Edge-apparaat rechtstreeks met de IoT Hub-instantie in uw oplossingenaccelerator. U moet de naam van de IoT hub in uw oplossingenaccelerator weten voordat u een van deze handleidingen:

- [Registreer een nieuwe Azure IoT Edge-apparaat vanuit de Azure-portal](../iot-edge/how-to-register-device-portal.md)
- [Een nieuw Azure IoT Edge-apparaat registreren bij Azure CLI](../iot-edge/how-to-register-device-cli.md)
- [Registreer een nieuwe Azure IoT Edge-apparaat vanuit Visual Studio Code](../iot-edge/how-to-register-device-vscode.md)

Wanneer u een apparaat rechtstreeks met de IoT hub in de oplossingsverbetering voor externe controle hebt geregistreerd, wordt deze weergegeven op de **apparaten** pagina in de web-UI.

## <a name="install-the-iot-edge-runtime"></a>IoT Edge-runtime installeren

Voordat u modules voor uw Edge-apparaat implementeren kunt, moet u de IoT Edge-runtime installeren op het fysieke apparaat. De volgende handleidingen ziet u hoe u de runtime op algemene apparaatplatformen installeert:

- [De Azure IoT Edge-runtime installeren in Linux (x64)](../iot-edge/how-to-install-iot-edge-linux.md)
- [Azure IoT Edge-runtime installeren in Linux (ARM32v7/armhf)](../iot-edge/how-to-install-iot-edge-linux-arm.md)
- [Azure IoT Edge-runtime installeren op Windows voor gebruik met Windows-containers](../iot-edge/how-to-install-iot-edge-windows-with-windows.md)
- [De Azure IoT Edge-runtime installeren op Windows gebruiken met Linux-containers](../iot-edge/how-to-install-iot-edge-windows-with-linux.md)
- [IoT Edge-runtime installeren op Windows IoT Core](../iot-edge/how-to-install-iot-core.md)

## <a name="next-steps"></a>Volgende stappen

Nu dat u uw IoT Edge-apparaat hebt voorbereid, is de volgende stap modules implementeren. Zie [importeren van een IoT Edge-pakket in uw oplossingsverbetering voor externe controle](iot-accelerators-remote-monitoring-import-edge-package.md)
