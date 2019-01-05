---
title: Maken van een Azure-IoT-Hub met behulp van Azure IoT-hulpprogramma's voor VS Code | Microsoft Docs
description: Het gebruik van Azure IoT-hulpprogramma's voor VS Code om te maken van een IoT-hub.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: junhan
ms.openlocfilehash: 9138a709cf8a166bbb572e04b082c5b8e6c82949
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54050231"
---
# <a name="create-an-iot-hub-using-the-azure-iot-tools-for-visual-studio-code"></a>Een IoT-hub met behulp van de Azure IoT-Tools voor Visual Studio Code maken

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

In dit artikel leest u hoe u de [Azure IoT-Tools voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) te maken van een Azure-IoT-hub. 

Als u wilt in dit artikel hebt voltooid, hebt u het volgende nodig:

- Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

- [Visual Studio Code](https://code.visualstudio.com/)

- [Hulpprogramma's voor Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) voor Visual Studio Code.

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

U hebt nu een IoT-hub met behulp van de Azure IoT-Tools voor Visual Studio Code geïmplementeerd. Om verder te verkennen, bekijkt u de volgende artikelen:

* [De Azure IoT-Tools voor Visual Studio Code gebruiken om te verzenden en ontvangen van berichten tussen uw apparaat en een IoT-Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

* [Gebruik de Azure IoT-Tools voor Visual Studio Code voor het beheer van Azure IoT Hub-apparaten](iot-hub-device-management-iot-toolkit.md)

* [Zie de pagina met Azure IoT Hub Toolkit wiki](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki).
