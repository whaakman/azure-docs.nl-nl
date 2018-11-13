---
title: Registreer een nieuwe Azure IoT Edge-apparaat (VS-Code) | Microsoft Docs
description: Visual Studio Code gebruiken om te maken van een nieuwe IoT Edge-apparaat in uw Azure-IoT-hub
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/14/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: cf9603c65454f076a494789e784c9352fb7bef33
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578702"
---
# <a name="register-a-new-azure-iot-edge-device-from-visual-studio-code"></a>Registreer een nieuwe Azure IoT Edge-apparaat vanuit Visual Studio Code

Voordat u uw IoT-apparaten met Azure IoT Edge gebruiken kunt, moet u hen registreert bij uw IoT-hub. Wanneer u een apparaat hebt geregistreerd, ontvangt u een verbindingsreeks die kan worden gebruikt voor het instellen van uw apparaat voor Edge-werkbelastingen. 

In dit artikel bevat informatie over het registreren van een nieuwe IoT Edge-apparaat met behulp van Visual Studio Code (VS-Code). Er zijn meerdere manieren om uit te voeren van de meeste bewerkingen in VS Code. In dit artikel wordt gebruikgemaakt van de Explorer, maar u kunt ook het Opdrachtenpalet gebruiken voor de meeste van de stappen uitvoeren. 

## <a name="prerequisites"></a>Vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-through-portal.md) in uw Azure-abonnement
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) voor Visual Studio Code

## <a name="sign-in-to-access-your-iot-hub"></a>Meld u aan voor toegang tot uw IoT-hub

De Azure IoT-uitbreidingen voor Visual Studio Code kunt u bewerkingen uitvoeren met uw IoT-hub. Voor deze bewerkingen om te werken, moet u zich aanmelden bij uw Azure-account en selecteer de IoT-hub die u bezig bent.

1. Open in Visual Studio Code, de **Explorer** weergeven.

2. Vouw aan de onderkant van de Verkenner van de **Azure IoT Hub-apparaten** sectie. 

   ![Vouw de Azure IoT Hub-apparaten](./media/how-to-register-device-vscode/azure-iot-hub-devices.png)

3. Klik op de **...**  in de **Azure IoT Hub-apparaten** sectiekop. Als u het beletselteken niet ziet, klikt u op of Beweeg de muisaanwijzer over de header. 

4. Kies **IoT-Hub selecteren**.

5. Als u niet bent aangemeld bij uw Azure-account, volg de aanwijzingen om dit te doen. 

6. Selecteer uw Azure-abonnement. 

7. Selecteer uw IoT-hub. 

## <a name="create-a-device"></a>Een apparaat maken

1. Vouw in de VS Code Explorer de **Azure IoT Hub-apparaten** sectie. 

2. Klik op de **...**  in de **Azure IoT Hub-apparaten** sectiekop. Als u het beletselteken niet ziet, klikt u op of Beweeg de muisaanwijzer over de header. 

3. Selecteer **IoT Edge-apparaat maken**. 

4. Geef in het tekstvak dat wordt geopend, uw apparaat een ID. 

In het uitvoerscherm ziet u het resultaat van de opdracht. De apparaatgegevens wordt afgedrukt, waaronder de **deviceId** die u hebt opgegeven en de **connectionString** waarmee u kunt uw fysieke apparaat verbinden met uw IoT-hub. 

## <a name="view-all-devices"></a>Alle apparaten weergeven

Alle apparaten die verbinding met uw IoT-hub maken worden vermeld in de **Azure IoT Hub-apparaten** sectie van de Visual Studio Code Explorer. IoT Edge-apparaten worden onderscheiden van niet-Edge-apparaten met een ander pictogram en het feit dat ze kunnen worden uitgebreid om weer te geven van de modules die zijn geïmplementeerd op elk apparaat. 

   ![Apparaten in VS Code weergeven](./media/how-to-register-device-vscode/view-devices.png)

## <a name="retrieve-the-connection-string"></a>De verbindingsreeks ophalen

Wanneer u klaar bent om uw apparaat instellen, moet u de verbindingsreeks die is gekoppeld aan uw fysieke apparaat met de identiteit van de IoT-hub.

1. Met de rechtermuisknop op de ID van uw apparaat in de **Azure IoT Hub-apparaten** sectie. 
2. Selecteer **Apparaatverbindingsreeks kopiëren**.

   De verbindingsreeks is naar het Klembord gekopieerd. 

U kunt ook selecteren **apparaatgegevens ophalen** in het contextmenu om te zien van alle apparaat-informatie, met inbegrip van de verbindingsreeks in het uitvoervenster weergegeven. 


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [modules implementeert op een apparaat met Visual Studio Code](how-to-deploy-modules-vscode.md).
