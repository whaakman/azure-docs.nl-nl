---
title: Azure IoT Edge-modules (CLI) implementeren | Microsoft Docs
description: Gebruik van de IoT-extensie voor Azure CLI 2.0 voor modules implementatie naar een IoT-randapparaat
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/08/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 98a4be02188f7e0462979792a6061d535a64a18d
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37095970"
---
# <a name="deploy-azure-iot-edge-modules-with-azure-cli-20"></a>Azure IoT rand modules met Azure CLI 2.0 implementeren

Wanneer u IoT rand modules met uw bedrijfslogica maakt, die u wilt implementeren voor uw apparaten werken aan de rand. Als er meerdere modules die samenwerken om te verzamelen en gegevens verwerken, kunt u deze in één keer te implementeren en declareren de routeringsregels waarmee ze verbinding maken. 

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) is een open-source, cross-platform opdrachtregelprogramma voor het beheren van Azure-resources, zoals IoT Edge. Hiermee kunt u voor het beheren van resources, apparaat inrichting service-exemplaren en gekoppelde hubs gebruiksklaar Azure IoT Hub. De nieuwe IoT-extensie voorziet Azure CLI 2.0 van extra functies zoals Apparaatbeheer en alle functionaliteit van IoT Edge.

In dit artikel laat zien hoe een JSON-deployment manifest maken en vervolgens push-de implementatie naar een IoT-randapparaat met dat bestand. Zie voor meer informatie over het maken van een implementatie die gericht is op meerdere apparaten op basis van hun gedeelde labels [implementeren en controleren van de rand van de IoT-modules op grote schaal](how-to-deploy-monitor-cli.md)

## <a name="prerequisites"></a>Vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-using-cli.md) in uw Azure-abonnement. 
* Een [IoT randapparaat](how-to-register-device-cli.md) met de IoT-rand runtime geïnstalleerd.
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) in uw omgeving. Uw versie van Azure CLI 2.0 moet minimaal versie 2.0.24 of hoger zijn. Gebruik `az –-version` om de versie te valideren. In deze versie worden az-extensie-opdrachten ondersteund en is voor het eerst het Knack-opdrachtframework opgenomen. 
* De [IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Configureren van een manifest voor implementatie

Een implementatiemanifest is een JSON-document dat wordt beschreven welke modules te implementeren en hoe de gegevens tussen de modules en de gewenste eigenschappen van de module horende loopt. Zie voor meer informatie over hoe implementatie manifesten werken en hoe ze worden gemaakt, [begrijpen hoe IoT rand modules kunnen worden gebruikt, geconfigureerd en hergebruikt](module-composition.md).

Sla het implementatiemanifest lokaal als een txt-bestand voor het implementeren van modules met behulp van Azure CLI 2.0. U gebruikt het bestandspad in de volgende sectie tijdens het uitvoeren van de opdracht uit om de configuratie van toepassing op uw apparaat. 

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

## <a name="deploy-to-your-device"></a>Implementeren op uw apparaat

U implementeert modules voor uw apparaat door toe te passen van het implementatiemanifest die u hebt geconfigureerd met de module-informatie. 

Gebruik de volgende opdracht in de configuratie toepassen op een Edge van de IoT-apparaat:

   ```cli
   az iot hub apply-configuration --device-id [device id] --hub-name [hub name] --content [file path]
   ```

De apparaat-id-parameter is hoofdlettergevoelig. De inhoud parameter verwijst naar de deployment manifest-bestand dat u hebt opgeslagen. 

## <a name="view-modules-on-your-device"></a>De modules weergeven op het apparaat

Als u modules hebt geïmplementeerd op het apparaat, kunt u deze met de volgende opdracht bekijken: 

De modules op uw IoT Edge-apparaat bekijken:
    
   ```cli
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

De apparaat-id-parameter is hoofdlettergevoelig.

   ![Modules in lijst weergeven](./media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [implementeren en controleren van de rand van de IoT-modules op grote schaal](how-to-deploy-monitor.md)