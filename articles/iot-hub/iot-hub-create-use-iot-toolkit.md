---
title: Een Azure-IoT-Hub met behulp van Azure IoT Hub-Toolkit voor Visual Studio-Code maken | Microsoft Docs
description: Het gebruik van Azure IoT Hub-Toolkit voor Visual Studio Code om te maken van een IoT-hub.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: junhan
ms.openlocfilehash: cee71ddfbf1a20cc7417976d60b04bff6f0deac8
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53339022"
---
# <a name="create-an-iot-hub-using-the-azure-iot-hub-toolkit-for-visual-studio-code"></a>Een IoT-hub met behulp van de Azure IoT Hub-Toolkit voor Visual Studio Code maken

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

In dit artikel leest u hoe u de [Azure IoT Hub-Toolkit voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (voorheen Azure IoT Toolkit) te maken van een Azure-IoT-hub. 

Als u wilt in dit artikel hebt voltooid, hebt u het volgende nodig:

- Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

- [Visual Studio Code](https://code.visualstudio.com/)

- [Azure IoT Hub-Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

1. Open in Visual Studio Code, de **Explorer** weergeven.

2. Vouw aan de onderkant van de Verkenner van de **Azure IoT Hub-apparaten** sectie. 

   ![Vouw de Azure IoT Hub-apparaten](./media/iot-hub-create-use-iot-toolkit/azure-iot-hub-devices.png)

3. Klik op de **...**  in de **Azure IoT Hub-apparaten** sectiekop. Als u het beletselteken niet ziet, Beweeg de muisaanwijzer over de header. 

4. Kies **IoT-Hub maken**.

5. Een pop-upvenster wordt weergegeven in de rechterbenedenhoek zodat u voor de eerste keer aanmelden bij Azure.

6. Selecteer de Azure-abonnement. 

7. Resourcegroep selecteren

8. Selecteer de locatie.

9. Prijscategorie selecteren.

10. Voer een unieke naam op voor uw IoT-Hub.

11. Wacht een paar minuten totdat de IoT-Hub is gemaakt.

## <a name="next-steps"></a>Volgende stappen

U hebt nu een IoT-hub met behulp van de Azure IoT Hub-Toolkit voor Visual Studio Code geïmplementeerd. Om verder te verkennen, bekijkt u de volgende artikelen:

* [De Azure IoT Hub Toolkit-extensie voor Visual Studio Code gebruiken voor het verzenden en ontvangen van berichten tussen uw apparaat en een IoT-Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

* [De Azure IoT Hub Toolkit-extensie voor Visual Studio Code gebruiken voor het beheer van Azure IoT Hub-apparaten](iot-hub-device-management-iot-toolkit.md)

* [Zie de pagina met Azure IoT Hub Toolkit wiki](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki).
