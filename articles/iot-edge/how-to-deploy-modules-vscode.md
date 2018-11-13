---
title: Implementeren van Azure IoT Edge-modules (VS-Code) | Microsoft Docs
description: Visual Studio Code gebruiken om te implementeren van modules voor een IoT Edge-apparaat
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: conceptual
ms.reviewer: ''
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a7f0fc0c3169aadac78609830dda55ae1ec7ea46
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567840"
---
# <a name="deploy-azure-iot-edge-modules-from-visual-studio-code"></a>Azure IoT Edge-modules van Visual Studio Code implementeren

Als u IoT Edge modules met uw zakelijke logica maken, wilt u deze implementeren op uw apparaten te werken aan de rand. Als u meerdere modules die samenwerken om te verzamelen en verwerken van gegevens hebt, kunt u ze in één keer implementeren en declareert de routeringsregels waarmee ze zijn verbonden. 

In dit artikel laat zien hoe een manifest van de implementatie JSON maken en vervolgens pusht u de implementatie naar een IoT Edge-apparaat met dat bestand. Zie voor meer informatie over het maken van een implementatie die gericht is op meerdere apparaten op basis van hun gedeelde labels [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md)

## <a name="prerequisites"></a>Vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-through-portal.md) in uw Azure-abonnement. 
* Een [IoT Edge-apparaat](how-to-register-device-portal.md) met IoT Edge-runtime geïnstalleerd. 
* [Visual Studio Code](https://code.visualstudio.com/).
* [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) voor Visual Studio Code. 

## <a name="configure-a-deployment-manifest"></a>Een manifest van de implementatie configureren

Het manifest voor een implementatie is een JSON-document waarin wordt beschreven welke modules te implementeren, hoe gegevens stromen tussen de modules, en de gewenste eigenschappen van de moduledubbels. Zie voor meer informatie over hoe implementatie werk manifesten en hoe ze worden gemaakt, [te begrijpen hoe IoT Edge-modules kunnen worden gebruikt, geconfigureerd en opnieuw gebruikt](module-composition.md).

Voor het implementeren van modules met behulp van Visual Studio Code, sla het manifest implementatie lokaal als een. JSON-bestand. Wanneer u de opdracht uit om de configuratie van toepassing op het apparaat uitvoert, wordt u het pad in de volgende sectie.

Hier volgt een manifest eenvoudige implementatie met één module als een voorbeeld:

   ```json
   {
     "modulesContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": "",
               "registryCredentials": {}
             }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                 "createOptions": "{}"
               }
             }
           },
           "modules": {
             "tempSensor": {
               "version": "1.0",
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
               "route": "FROM /* INTO $upstream"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       },
       "tempSensor": {
         "properties.desired": {}
       }
     }
   }
   ```
   
## <a name="sign-in-to-access-your-iot-hub"></a>Meld u aan voor toegang tot uw IoT-hub

De Azure IoT-uitbreidingen voor Visual Studio Code kunt u bewerkingen uitvoeren met uw IoT-hub. Voor deze bewerkingen om te werken, moet u zich aanmelden bij uw Azure-account en selecteer de IoT-hub die u bezig bent.

1. Open in Visual Studio Code, de **Explorer** weergeven.

2. Vouw aan de onderkant van de Verkenner van de **Azure IoT Hub-apparaten** sectie. 

   ![Vouw de Azure IoT Hub-apparaten](./media/how-to-deploy-modules-vscode/azure-iot-hub-devices.png)

3. Klik op de **...**  in de **Azure IoT Hub-apparaten** sectiekop. Als u het beletselteken niet ziet, Beweeg de muisaanwijzer over de header. 

4. Kies **IoT-Hub selecteren**.

5. Als u niet bent aangemeld bij uw Azure-account, volg de aanwijzingen om dit te doen. 

6. Selecteer uw Azure-abonnement. 

7. Selecteer uw IoT-hub. 


## <a name="deploy-to-your-device"></a>Uw apparaat implementeren

U implementeren modules naar uw apparaat door het toepassen van het manifest implementatie die u hebt geconfigureerd met de modulegegevens. 

1. Vouw in de weergave van Visual Studio Code explorer de **Azure IoT Hub-apparaten** sectie. 

2. Met de rechtermuisknop op het apparaat dat u wilt configureren met de implementatie van het manifest. 

3. Selecteer **implementatie voor één apparaat maken**. 

4. Navigeer naar de implementatie van JSON-manifestbestand die u wilt gebruiken en klikt u op **Selecteer Edge-implementatie Manifest**. 

   ![Manifest van de Edge-implementatie selecteren](./media/how-to-deploy-modules-vscode/select-deployment-manifest.png)


De resultaten van uw implementatie zijn vermeld in de VS Code-uitvoer. Geslaagde implementaties worden toegepast binnen een paar minuten als het doelapparaat wordt uitgevoerd en die zijn verbonden met internet. 

## <a name="view-modules-on-your-device"></a>Modules weergeven op uw apparaat

Nadat u hebt modules geïmplementeerd op uw apparaat, vindt u alle mappen in de **Azure IoT Hub-apparaten** sectie. Selecteer de pijl naast uw IoT Edge-apparaat uit te vouwen. De huidige actieve modules worden weergegeven. 

Als u onlangs nieuwe modules geïmplementeerd op een apparaat, Beweeg de muisaanwijzer over de **Azure IoT Hub-apparaten** sectie header en selecteer het vernieuwingspictogram op de weergave bijwerken. 

Met de rechtermuisknop op de naam van een module bekijken en bewerken van de moduledubbel. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md)
