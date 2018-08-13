---
title: Een Azure-IoT-Hub met behulp van Azure IoT Toolkit voor Visual Studio-Code maken | Microsoft Docs
description: Het gebruik van Azure IoT Toolkit voor Visual Studio Code om te maken van een IoT-hub.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: junhan
ms.openlocfilehash: af31f9375d6a41e13a9122e9730ba9532d3d52c5
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003066"
---
# <a name="create-an-iot-hub-using-the-azure-iot-toolkit-for-visual-studio-code"></a>Een IoT-hub met behulp van de Azure IoT Toolkit voor Visual Studio Code maken

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

U kunt [Azure IoT Toolkit voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) om Azure IoT hubs te maken. In dit artikel wordt beschreven hoe u een IoT-hub maken met Azure IoT Toolkit.

Als u wilt in dit artikel hebt voltooid, hebt u het volgende nodig:

* Een actief Azure-account.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

1. Open in Visual Studio Code, de **Explorer** weergeven.

2. Vouw aan de onderkant van de Verkenner van de **Azure IoT Hub-apparaten** sectie. 

   ![Vouw de Azure IoT Hub-apparaten](./media/iot-hub-create-use-iot-toolkit/azure-iot-hub-devices.png)

3. Klik op de **... ** in de **Azure IoT Hub-apparaten** sectiekop. Als u het beletselteken niet ziet, Beweeg de muisaanwijzer over de header. 

4. Kies **IoT-Hub maken**.

5. Een pop-upvenster wordt weergegeven in de rechterbenedenhoek zodat u voor de eerste keer aanmelden bij Azure.

6. Selecteer de Azure-abonnement. 

7. Resourcegroep selecteren

8. Selecteer de locatie.

9. Prijscategorie selecteren.

10. Voer een unieke naam op voor uw IoT-Hub.

11. Wacht een paar minuten totdat de IoT-Hub is gemaakt.

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT-hub met behulp van Azure IoT Toolkit voor Visual Studio Code hebt geïmplementeerd, kunt u verder te verkennen:

* [Azure IoT Toolkit-extensie voor Visual Studio Code gebruiken om te verzenden en ontvangen van berichten tussen uw apparaat en IoT-Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).
* [Gebruik Azure IoT Toolkit-extensie voor Visual Studio Code voor het beheer van Azure IoT Hub-apparaten](iot-hub-device-management-iot-toolkit.md)
* [Wiki-pagina](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki) voor Azure IoT Toolkit.
