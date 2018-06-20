---
title: Modules implementeren in IoT Edge-apparaten met IoT-extensie voor Azure CLI 2.0 | Microsoft Docs
description: Modules implementeren in een IoT Edge-apparaat met IoT-extensie voor Azure CLI 2.0
author: chrissie926
manager: ''
ms.author: menchi
ms.date: 03/02/2018
ms.topic: tutorial
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
md.custom: mvc
ms.openlocfilehash: deee54fe5d11d6d1cf5485357f853b1cb078f96d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631578"
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Modules implementeren in een IoT Edge-apparaat met IoT-extensie voor Azure CLI 2.0

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) is een open-source, cross-platform opdrachtregelprogramma voor het beheren van Azure-resources, zoals IoT Edge. Azure CLI 2.0 is beschikbaar voor Windows, Linux en Mac OS.

Met Azure CLI 2.0 kunt u Azure IoT Hub-resources, service-exemplaren voor apparaatinrichting en gebruiksklare gekoppelde hubs beheren. De nieuwe IoT-extensie voorziet Azure CLI 2.0 van extra functies zoals Apparaatbeheer en alle functionaliteit van IoT Edge.

In dit artikel leert u hoe u Azure CLI 2.0 en de IoT-extensie instelt. Vervolgens leert u hoe u modules implementeert in een IoT Edge-apparaat met de beschikbare CLI-opdrachten.

## <a name="prerequisites"></a>Vereisten

* Een Azure-account. Als u nog geen account hebt, kunt u vandaag [een gratis account aanmaken](https://azure.microsoft.com/free/?v=17.39a). 

* [Python 2.7 x of Python 3.x](https://www.python.org/downloads/).

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) in uw omgeving. Uw versie van Azure CLI 2.0 moet minimaal versie 2.0.24 of hoger zijn. Gebruik `az –-version` om de versie te valideren. In deze versie worden az-extensie-opdrachten ondersteund en is voor het eerst het Knack-opdrachtframework opgenomen. Een eenvoudige manier om Azure CLI 2.0 te installeren in Windows is de [MSI](https://aka.ms/InstallAzureCliWindows) te downloaden en installeren.

* [De IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension):
   1. Voer `az extension add --name azure-cli-iot-ext` uit. 
   2. Gebruik na de installatie `az extension list` om de momenteel geïnstalleerde extensies te valideren of `az extension show --name azure-cli-iot-ext` voor informatie over de IoT-extensie.
   3. Gebruik `az extension remove --name azure-cli-iot-ext` om de extensie te verwijderen.


## <a name="create-an-iot-edge-device"></a>Een IoT Edge-apparaat maken
In dit artikel staan instructies voor het maken van een IoT Edge-implementatie. Het voorbeeld laat zien hoe u zich aanmeldt bij uw Azure-account, een Azure-resourcegroep maakt (bestaande uit een container die verwante resources voor een Azure-oplossing bevat), een IoT Hub maakt, drie identiteiten maakt voor IoT Edge-apparaten, labels instelt en een IoT Edge-implementatie maakt voor die apparaten. 

Meld u aan bij uw Azure-account. Nadat u de volgende opdracht hebt ingevoerd, wordt u gevraagd een webbrowser te gebruiken om u met een eenmalige code aan te melden: 

   ```cli
   az login
   ```

Maak een nieuwe resourcegroep met de naam **IoTHubCLI** in de regio US - oost: 

   ```cli
   az group create -l eastus -n IoTHubCLI
   ```

   ![Een resourcegroep maken][2]

Maak een IoT Hub met de naam **CLIDemoHub** onder de nieuwe resourcegroep:

   ```cli
   az iot hub create --name CLIDemoHub --resource-group IoTHubCLI --sku S1
   ```

   >[!TIP]
   >Elk abonnement biedt toegang tot één gratis IoT-hub. Om een gratis hub te maken met een CLI-opdracht, vervangt u de SKU-waarde met `--sku F1`. Als u al een gratis hub in uw abonnement hebt, krijgt u een foutbericht wanneer u een tweede wilt maken. 

Een IoT Edge-apparaat maken:

   ```cli
   az iot hub device-identity create --device-id edge001 --hub-name CLIDemoHub --edge-enabled
   ```

   ![Een IoT Edge-apparaat maken][4]

## <a name="configure-the-iot-edge-device"></a>Een IoT Edge-apparaat configureren

Maak een JSON-sjabloon voor implementatie en sla die lokaal op als TXT-bestand. U hebt het pad naar het bestand nodig wanneer u de opdracht Configuratie toepassen uitvoert.

De implementatie van een JSON-sjabloon moet altijd de twee systeemmodules EdgeAgent en EdgeHub bevatten. Bovendien kunt u dit bestand gebruiken om extra modules te implementeren in het IoT Edge-apparaat. Gebruik het volgende voorbeeld om uw IoT Edge-apparaat te configureren met een tempSensor-module:

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
               "loggingOptions": ""
             }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "microsoft/azureiotedge-agent:1.0-preview",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "microsoft/azureiotedge-hub:1.0-preview",
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
                 "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {},
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

De configuratie op uw IoT Edge-apparaat toepassen:

   ```cli
   az iot hub apply-configuration --device-id edge001 --hub-name CLIDemoHub --content C:\<configuration.txt file path>
   ```

De modules op uw IoT Edge-apparaat bekijken:
    
   ```cli
   az iot hub module-identity list --device-id edge001 --hub-name CLIDemoHub
   ```

   ![Modules in lijst weergeven][6]

## <a name="next-steps"></a>Volgende stappen

* Ontdek hoe u [een IoT Edge-apparaat als gateway kunt gebruiken](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.png
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.png

