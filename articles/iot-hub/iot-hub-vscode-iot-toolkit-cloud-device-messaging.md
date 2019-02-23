---
title: Berichten met Azure IoT-hulpprogramma's voor Visual Studio Code op Azure IoT Hub cloudapparaten beheren | Microsoft Docs
description: Informatie over het gebruik van Azure IoT-Tools voor Visual Studio Code om te controleren van apparaat naar cloud-berichten en cloud naar apparaat-berichten in Azure IoT Hub verzenden.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 01/18/2019
ms.author: junhan
ms.openlocfilehash: 0455dafaa39f9f6bc19399fab31663d6b2e06e95
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56726343"
---
# <a name="use-azure-iot-tools-for-visual-studio-code-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Azure IoT-Tools voor Visual Studio Code gebruiken voor het verzenden en ontvangen van berichten tussen uw apparaat en IoT-Hub

![Diagram voor end-to-end](media/iot-hub-get-started-e2e-diagram/2.png)

[Azure IoT-hulpprogramma's](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) is een nuttig Visual Studio Code-extensie die IoT Hub beheer en ontwikkeling van IoT-toepassingen gemakkelijker maakt. In dit artikel richt zich op het Azure IoT-Tools voor Visual Studio Code gebruiken om te verzenden en ontvangen van berichten tussen uw apparaat en uw IoT-hub.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-you-will-learn"></a>U leert

Leert u hoe u Azure IoT-Tools voor Visual Studio Code gebruiken voor het bewaken van apparaat-naar-cloud-berichten en het verzenden van berichten van cloud-naar-apparaat. Apparaat-naar-cloud-berichten mogelijk sensorgegevens die uw apparaat verzamelt en verzendt vervolgens naar uw IoT-hub. Cloud-naar-apparaat-berichten kunnen worden opdrachten die uw IoT-hub naar uw apparaat verzendt laten knipperen een LED die is verbonden met uw apparaat.

## <a name="what-you-will-do"></a>Wat u doet

- Azure IoT Tools voor Visual Studio Code gebruiken voor het bewaken van apparaat-naar-cloud-berichten.
- Azure IoT-Tools voor Visual Studio Code gebruiken om cloud-naar-apparaat-berichten te verzenden.

## <a name="what-you-need"></a>Wat u nodig hebt

- Een actief Azure-abonnement.
- Een Azure IoT-hub in uw abonnement.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Azure IoT-hulpprogramma's voor VS Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

## <a name="sign-in-to-access-your-iot-hub"></a>Meld u aan voor toegang tot uw IoT-hub

1. In **Explorer** weergave van VS Code uit, vouw **Azure IoT Hub-apparaten** sectie in de linkerbenedenhoek.
1. Klik op **IoT-Hub selecteren** in het contextmenu.
1. Een pop-upvenster wordt weergegeven in de rechterbenedenhoek zodat u voor de eerste keer aanmelden bij Azure.
1. Nadat u zich hebt aangemeld, wordt de lijst met uw Azure-abonnement worden weergegeven en selecteer Azure-abonnement en IoT-Hub.
1. De lijst met apparaten wordt weergegeven in **Azure IoT Hub-apparaten** tabblad in een paar seconden.

   > [!Note]
   > U kunt de installatie ook voltooien door **IoT Hub-verbindingsreeks instellen** te kiezen. Geef de verbindingsreeks voor de IoT-hub die uw IoT-apparaat in verbinding met het pop-upvenster maakt.
   
## <a name="monitor-device-to-cloud-messages"></a>Bewaken van apparaat-naar-cloud-berichten

Volg deze stappen voor het bewaken van berichten die van uw apparaat worden verzonden naar uw IoT-hub:

1. Met de rechtermuisknop op uw apparaat en selecteer **Start Monitoring D2C Message**.
1. De bewaakte berichten worden weergegeven **uitvoer** > **Azure IoT Hub Toolkit** weergeven.
1. Als u wilt de bewaking stopt, met de rechtermuisknop op de **uitvoer** weergeven en selecteren **Stop Monitoring D2C Message**.

## <a name="send-cloud-to-device-messages"></a>Cloud-naar-apparaat-berichten verzenden

Berichten van uw IoT-hub worden verzonden naar uw apparaat, de volgende stappen uit:

1. Met de rechtermuisknop op uw apparaat en selecteer **C2D-bericht verzenden naar apparaat**. 
1. Geef het bericht in het invoervak.
1. Resultaten worden weergegeven **uitvoer** > **Azure IoT Hub Toolkit** weergeven.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u bewaken van apparaat-naar-cloud-berichten en cloud-naar-apparaat-berichten tussen uw IoT-apparaat en Azure IoT Hub verzendt.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
