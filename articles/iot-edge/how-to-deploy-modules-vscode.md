---
title: Azure IoT Edge-modules (VS-Code) implementeren | Microsoft Docs
description: Visual Studio Code gebruiken voor het implementeren van modules voor een IoT-randapparaat
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: conceptual
ms.reviewer: ''
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 7e75f2ff5e2df3189683d084a315ad6c8730be84
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035906"
---
# <a name="deploy-azure-iot-edge-modules-from-visual-studio-code"></a>Azure IoT rand modules vanuit Visual Studio Code implementeren

Wanneer u IoT rand modules met uw bedrijfslogica maakt, die u wilt implementeren voor uw apparaten werken aan de rand. Als er meerdere modules die samenwerken om te verzamelen en gegevens verwerken, kunt u deze in één keer te implementeren en declareren de routeringsregels waarmee ze verbinding maken. 

In dit artikel laat zien hoe een JSON-deployment manifest maken en vervolgens push-de implementatie naar een IoT-randapparaat met dat bestand. Zie voor meer informatie over het maken van een implementatie die gericht is op meerdere apparaten op basis van hun gedeelde labels [implementeren en controleren van de rand van de IoT-modules op grote schaal](how-to-deploy-monitor.md)

## <a name="prerequisites"></a>Vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-through-portal.md) in uw Azure-abonnement. 
* Een [IoT randapparaat](how-to-register-device-portal.md) met de IoT-rand runtime geïnstalleerd. 
* [Visual Studio Code](https://code.visualstudio.com/).
* [De extensie Azure IoT rand](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) voor Visual Studio Code. 

## <a name="configure-a-deployment-manifest"></a>Configureren van een manifest voor implementatie

Een implementatiemanifest is een JSON-document dat wordt beschreven welke modules te implementeren en hoe de gegevens tussen de modules en de gewenste eigenschappen van de module horende loopt. Zie voor meer informatie over hoe implementatie manifesten werken en hoe ze worden gemaakt, [begrijpen hoe IoT rand modules kunnen worden gebruikt, geconfigureerd en hergebruikt](module-composition.md).

Als u wilt implementeren modules met behulp van Visual Studio Code, sla het implementatiemanifest lokaal als een. JSON-bestand. U gebruikt het bestandspad in de volgende sectie tijdens het uitvoeren van de opdracht uit om de configuratie van toepassing op uw apparaat.

Hier volgt een manifest eenvoudige implementatie met één module als een voorbeeld:

   ```json
   {
     "moduleContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": "",
               "registryCredentials": {
                 "registryName": {
                   "username": "",
                   "password": "",
                   "address": ""
                 }
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
                 "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
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

De Azure IoT-uitbreidingen voor Visual Studio Code kunt u bewerkingen uitvoeren met uw IoT-hub. Voor deze bewerkingen om te werken, moet u zich aanmelden bij uw Azure-account en selecteer de IoT-hub waarmee u werkt op.

1. Open in Visual Studio Code, de **Explorer** weergeven.

2. Aan de onderkant van de Explorer, vouw de **Azure IoT Hub-apparaten** sectie. 

   ![Vouw de Azure IoT Hub-apparaten](./media/how-to-deploy-modules-vscode/azure-iot-hub-devices.png)

3. Klik op de **...**  in de **Azure IoT Hub-apparaten** sectiekop. Als u het weglatingsteken niet ziet, houd de muisaanwijzer op de koptekst. 

4. Kies **IoT-Hub Selecteer**.

5. Als u niet bent aangemeld bij uw Azure-account, volg de aanwijzingen om dit te doen. 

6. Selecteer uw Azure-abonnement. 

7. Selecteer uw IoT-hub. 


## <a name="deploy-to-your-device"></a>Implementeren op uw apparaat

U implementeert modules voor uw apparaat door toe te passen van het implementatiemanifest die u hebt geconfigureerd met de module-informatie. 

1. Vouw in de weergave van de Visual Studio Code Verkenner de **Azure IoT Hub-apparaten** sectie. 

2. Klik met de rechtermuisknop op het apparaat dat u wilt configureren met het implementatiemanifest. 

3. Selecteer **implementatie creëert voor IoT randapparaat**. 

4. Navigeer naar de implementatie van JSON-manifestbestand die u wilt gebruiken en klik op **Selecteer rand Deployment Manifest**. 

   ![Selecteer rand Deployment Manifest](./media/how-to-deploy-modules-vscode/select-deployment-manifest.png)


De resultaten van uw implementatie worden afgedrukt in de VS Code-uitvoer. Geslaagde implementaties worden toegepast binnen enkele minuten als het doelapparaat wordt uitgevoerd en is verbonden met internet. 

## <a name="view-modules-on-your-device"></a>De modules weergeven op het apparaat

Als u modules hebt geïmplementeerd op het apparaat, kunt u alle afbeeldingen bekijken de **Azure IoT Hub-apparaten** sectie. Selecteer de pijl naast uw IoT-randapparaat uit te vouwen. De huidige actieve modules worden weergegeven. 

Als u onlangs nieuwe modules voor een apparaat hebt geïmplementeerd, Beweeg de muisaanwijzer over de **Azure IoT Hub-apparaten** sectie header en selecteer het pictogram vernieuwen van de weergave bijwerken. 

Met de rechtermuisknop op de naam van een module die u wilt weergeven en bewerken van de module-twin. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [implementeren en controleren van de rand van de IoT-modules op grote schaal](how-to-deploy-monitor.md)