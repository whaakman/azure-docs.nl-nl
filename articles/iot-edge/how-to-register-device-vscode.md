---
title: Registreren van een nieuwe Azure IoT randapparaat (VS-Code) | Microsoft Docs
description: Visual Studio Code gebruiken om te maken van een nieuwe IoT Edge-apparaat in uw Azure-IoT-hub
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/14/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 7902461f58df1b4fe0c3ed3b577f668fe8be4cc2
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035951"
---
# <a name="register-a-new-azure-iot-edge-device-from-visual-studio-code"></a>Een nieuwe Azure IoT-randapparaat vanuit Visual Studio Code registreren

Voordat u uw IoT-apparaten met Azure IoT rand gebruiken kunt, moet u deze registreren met uw IoT-hub. Wanneer u een apparaat registreert, krijgt u een verbindingsreeks die kan worden gebruikt voor het instellen van uw apparaat voor werkbelastingen van de rand. 

In dit artikel laat zien hoe u een nieuwe IoT Edge-apparaat met behulp van Visual Studio Code (VS-Code) registreren. Er zijn meerdere manieren om de meeste bewerkingen uitvoeren in VS-Code. Dit artikel wordt de Explorer gebruikt, maar u kunt ook het palet opdracht gebruiken om uit te voeren van de meeste van de stappen. 

## <a name="prerequisites"></a>Vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-through-portal.md) in uw Azure-abonnement
* [Visual Studio Code](https://code.visualstudio.com/) 
* [De extensie Azure IoT rand](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) voor Visual Studio Code

## <a name="sign-in-to-access-your-iot-hub"></a>Meld u aan voor toegang tot uw IoT-hub

De Azure IoT-uitbreidingen voor Visual Studio Code kunt u bewerkingen uitvoeren met uw IoT-hub. Voor deze bewerkingen om te werken, moet u zich aanmelden bij uw Azure-account en selecteer de IoT-hub waarmee u werkt op.

1. Open in Visual Studio Code, de **Explorer** weergeven.

2. Aan de onderkant van de Explorer, vouw de **Azure IoT Hub-apparaten** sectie. 

   ![Vouw de Azure IoT Hub-apparaten](./media/how-to-register-device-vscode/azure-iot-hub-devices.png)

3. Klik op de **...**  in de **Azure IoT Hub-apparaten** sectiekop. Als u het weglatingsteken niet ziet, houd de muisaanwijzer op de koptekst. 

4. Kies **IoT-Hub Selecteer**.

5. Als u niet bent aangemeld bij uw Azure-account, volg de aanwijzingen om dit te doen. 

6. Selecteer uw Azure-abonnement. 

7. Selecteer uw IoT-hub. 

## <a name="create-a-device"></a>Een apparaat maken

1. Vouw in de VS Code Explorer de **Azure IoT Hub-apparaten** sectie. 

2. Klik op de **...**  in de **Azure IoT Hub-apparaten** sectiekop. Als u het weglatingsteken niet ziet, houd de muisaanwijzer op de koptekst. 

3. Selecteer **IoT randapparaat maken**. 

4. Geef in het tekstvak uw apparaat ID. 

In het uitvoerscherm ziet u het resultaat van de opdracht. De apparaatgegevens wordt afgedrukt, waaronder de **deviceId** die u hebt opgegeven en de **connectionString** waarmee u kunt uw fysieke apparaat aan uw IoT-hub te sluiten. 

## <a name="view-all-devices"></a>Alle apparaten weergeven

Alle apparaten die verbinding met uw IoT-hub maken worden weergegeven in de **Azure IoT Hub-apparaten** sectie van de Visual Studio Code Explorer. IoT Edge-apparaten zijn te onderscheiden zijn van niet-Edge-apparaten met een ander pictogram en het feit dat ze kunnen worden uitgebreid om weer te geven van de modules die zijn geïmplementeerd op elk apparaat. 

   ![Apparaten weergeven in VS-Code](./media/how-to-register-device-vscode/view-devices.png)

## <a name="retrieve-the-connection-string"></a>De verbindingsreeks ophalen

Wanneer u klaar bent voor het instellen van uw apparaat, moet u de verbindingsreeks die is gekoppeld aan uw fysieke apparaat met de identiteit van de IoT-hub.

1. Met de rechtermuisknop op de ID van uw apparaat in de **Azure IoT Hub-apparaten** sectie. 
2. Selecteer **Kopieer de verbindingsreeks apparaat**.

   De verbindingsreeks wordt gekopieerd naar het Klembord. 

U kunt ook selecteren **apparaatgegevens ophalen** in het contextmenu voor een overzicht van de apparaatgegevens, met inbegrip van de verbindingsreeks in het uitvoervenster. 


## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [modules implementeert op een apparaat met Visual Studio Code](how-to-deploy-modules-vscode.md).
