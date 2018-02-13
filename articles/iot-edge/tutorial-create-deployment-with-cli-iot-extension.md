---
title: Modules op rand van de IoT-apparaten met IoT-extensie voor Azure CLI 2.0 implementeren | Microsoft Docs
description: Modules implementeren naar een IoT-randapparaat met IoT-extensie voor Azure CLI 2.0
services: iot-edge
keywords: 
author: chrissie926
manager: timlt
ms.author: menchi
ms.date: 02/12/2018
ms.topic: article
ms.service: iot-edge
ms.custom: mvc
ms.reviewer: kgremban
ms.openlocfilehash: ce3e979428233af578d71dee5ed10103e105f4f4
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2018
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Modules implementeren naar een IoT-randapparaat met IoT-extensie voor Azure CLI 2.0

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) is een open source cross-platform-opdrachtregelprogramma voor het beheren van Azure-resources, zoals IoT rand. Azure CLI 2.0 is beschikbaar op Windows, Linux en Mac OS.

Azure CLI 2.0 kunt u voor het beheren van resources, apparaat inrichting service-exemplaren en gekoppelde hubs gebruiksklaar Azure IoT Hub. De nieuwe IoT-extensie verrijkt Azure CLI 2.0 met functies zoals Apparaatbeheer en de mogelijkheid om volledige IoT rand.

In deze zelfstudie maakt stappen u eerst de voor het instellen van Azure CLI 2.0 en de IoT-extensie. Vervolgens leert u hoe modules implementeren op een IoT-randapparaat met behulp van de beschikbare CLI-opdrachten.

## <a name="prerequisites"></a>Vereisten

* Een Azure-account. Als u een nog geen hebt, kunt u [een gratis account maken](https://azure.microsoft.com/free/?v=17.39a) vandaag. 

* [Python 2.7 x of Python 3.x](https://www.python.org/downloads/).

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) in uw omgeving. Uw versie van Azure CLI 2.0 moet ten minste 2.0.24 of hoger. Gebruik `az –-version` om te valideren. Deze versie ondersteunt az extensie opdrachten en maakt u kennis met de opdracht talent framework. Een eenvoudige manier om te installeren op Windows is downloaden en installeren de [MSI](https://aka.ms/InstallAzureCliWindows).

* [De IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension):
   1. Voer `az extension add --name azure-cli-iot-ext` uit. 
   2. Na de installatie met `az extension list` voor het valideren van de momenteel geïnstalleerde extensies of `az extension show --name azure-cli-iot-ext` voor informatie over de IoT-uitbreiding.
   3. U kunt de extensie verwijderen met `az extension remove --name azure-cli-iot-ext`.


## <a name="create-an-iot-edge-device"></a>Een IoT-randapparaat maken
Dit artikel bevat instructies voor het maken van de implementatie van een IoT-rand. In het voorbeeld laat zien hoe u aanmelden bij uw Azure-account, maken van een Azure Resource Group (een container met verwante resources voor een Azure-oplossing), een IoT Hub maken, drie IoT rand apparaten identiteit maken, markeringen en maak vervolgens de implementatie van een IoT-rand die u gericht is op deze apparaten. 

Aanmelden bij uw Azure-account. Nadat u de volgende aanmelding opdracht invoert, wordt gevraagd een webbrowser aan te melden met een eenmalige code gebruiken: 

   ```cli
   az login
   ```

Maak een nieuwe resourcegroep aangeroepen **IoTHubCLI** in de regio VS-Oost: 

   ```cli
   az group create -l eastus -n IoTHubCLI
   ```

   ![Een resourcegroep maken][2]

Maken van een IoT-hub aangeroepen **CLIDemoHub** in de zojuist gemaakte resourcegroep:

   ```cli
   az iot hub create --name CLIDemoHub --resource-group IoTHubCLI --sku S1
   ```

   >[!TIP]
   >Elk abonnement wordt één gratis IoT-hub toegewezen. Vervang de SKU-waarde met een gratis hub maakt met de opdracht CLI, `--sku F1`. Als u al een gratis hub in uw abonnement hebt, krijgt u een foutbericht weergegeven wanneer u een tweede maken. 

Maak een Edge van de IoT-apparaat:

   ```cli
   az iot hub device-identity create --device-id edge001 -hub-name CLIDemoHub --edge-enabled
   ```

   ![Rand van de IoT-apparaat maken][4]

## <a name="configure-the-iot-edge-device"></a>Configureren van de rand van de IoT-apparaat

Maak een JSON-sjabloon voor implementatie en lokaal opslaan als een txt-bestand. U moet het pad naar het bestand wanneer u de opdracht toepassen configuratie uitvoert.

Implementatie van JSON-sjablonen moeten altijd omvatten twee systeemmodules, edgeAgent en edgeHub. Naast deze twee, kunt u dit bestand aanvullende modules op de rand van de IoT-apparaat implementeren. Gebruik het volgende voorbeeld IoT randapparaat configureren met één tempSensor-module:

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
                 "image": "edgepreview.azurecr.io/azureiotedge/edge-agent:1.0-preview",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "edgepreview.azurecr.io/azureiotedge/edge-hub:1.0-preview",
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
                 "image": "edgepreview.azurecr.io/azureiotedge/simulated-temperature-sensor:1.0-preview",
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

De configuratie van toepassing op uw IoT-randapparaat:

   ```cli
   az iot hub apply-configuration --device-id edge001 --hub-name CLIDemoHub --content C:\<configuration.txt file path>
   ```

De modules die op uw IoT-randapparaat bekijken:
    
   ```cli
   az iot hub module-identity list --device-id edge001 --hub-name CLIDemoHub
   ```

   ![Lijst met modules][6]

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over hoe [een IoT-randapparaat als een gateway gebruiken](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.png
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.png

