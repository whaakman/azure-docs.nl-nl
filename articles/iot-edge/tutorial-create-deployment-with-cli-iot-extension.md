---
title: Modules op rand van de IoT-apparaten met IoT-extensie voor Azure CLI 2.0 implementeren | Microsoft Docs
description: Modules implementeren naar een IoT-randapparaat met IoT-extensie voor Azure CLI 2.0
services: iot-edge
keywords: 
author: chrissie926
manager: timlt
ms.author: menchi
ms.date: 01/11/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 26067187864f9a2a4c85c953ae8aca888458d245
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Modules implementeren naar een IoT-randapparaat met IoT-extensie voor Azure CLI 2.0

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) is een open-source cross-platform-opdrachtregelprogramma voor het beheren van Azure-resources, zoals IoT rand. Azure CLI 2.0 is beschikbaar op Windows, Linux en Mac OS.

Azure CLI 2.0 kunt u voor het beheren van resources, apparaat inrichting service-exemplaren en gekoppelde hubs gebruiksklaar Azure IoT Hub. De nieuwe IoT-extensie verrijkt Azure CLI 2.0 met functies zoals Apparaatbeheer en de mogelijkheid om volledige IoT rand.

In deze zelfstudie maakt stappen u eerst de voor het instellen van Azure CLI 2.0 en de IoT-extensie. Vervolgens leert u hoe modules implementeren op een IoT-randapparaat met behulp van de beschikbare CLI-opdrachten.

## <a name="installation"></a>Installatie 

### <a name="step-1---install-python"></a>Stap 1 - Install Python

[Python 2.7 x of Python 3.x](https://www.python.org/downloads/) is vereist.

### <a name="step-2---install-azure-cli-20"></a>Stap 2: Installeer Azure CLI 2.0

Ga als volgt de [installatie-instructie](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) voor het instellen van Azure CLI 2.0 in uw omgeving. Uw versie van Azure CLI 2.0 moet ten minste 2.0.24 of hoger. Gebruik `az –version` om te valideren. Deze versie ondersteunt az extensie opdrachten en maakt u kennis met de opdracht talent framework. Een eenvoudige manier om te installeren op Windows is downloaden en installeren de [MSI](https://aka.ms/InstallAzureCliWindows).

### <a name="step-3---install-iot-extension"></a>Stap 3 - installatie IoT-uitbreiding

[Het Leesmij-bestand extensie IoT](https://github.com/Azure/azure-iot-cli-extension) beschrijft de verschillende manieren voor het installeren van de extensie. De eenvoudigste manier is om uit te voeren `az extension add --name azure-cli-iot-ext`. Na de installatie, kunt u `az extension list` voor het valideren van de momenteel geïnstalleerde extensies of `az extension show --name azure-cli-iot-ext` voor informatie over de IoT-uitbreiding. U kunt gebruiken voor het verwijderen van de extensie `az extension remove --name azure-cli-iot-ext`.


## <a name="deploy-modules-to-an-iot-edge-device"></a>Modules implementeren naar een IoT-randapparaat
In deze zelfstudie leert u hoe u een IoT-Edge-implementatie te maken. In het voorbeeld laat zien hoe u aanmelden bij uw Azure-account, maken van een Azure Resource Group (een container met verwante resources voor een Azure-oplossing), een IoT Hub maken, drie IoT rand apparaten identiteit maken, markeringen en maak vervolgens de implementatie van een IoT-rand die u gericht is op deze apparaten. Voltooi de installatiestappen die hierboven is beschreven voordat u begint. Als u een Azure-account niet hebt, maar u kunt [een gratis account maken](https://azure.microsoft.com/free/?v=17.39a) vandaag. 


### <a name="1-login-to-the-azure-account"></a>1. Meld u aan bij de Azure-account
  
    az login

![aanmelding][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Een resourcegroep IoTHubBlogDemo in eastus maken

    az group create -l eastus -n IoTHubBlogDemo

![Een resourcegroep maken][2]


### <a name="3-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>3. Een IoT Hub blogDemoHub onder de nieuwe resourcegroep maken

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![IoT Hub maken][3]


### <a name="4-create-an-iot-edge-device"></a>4. Een IoT-randapparaat maken

    az iot hub device-identity create -d edge001 -n blogDemoHub --edge-enabled

![Rand van de IoT-apparaat maken][4]

### <a name="5-apply-configuration-to-the-iot-edge-device"></a>5. Configuratie toepassen op de rand van de IoT-apparaat

Uw implementatie JSON-sjabloon lokaal opslaan als een txt-bestand. U moet het pad naar het bestand wanneer u de opdracht toepassen configuratie uitvoert.

Hieronder volgt een voorbeeld JSON implementatiesjabloon waarin één tempSensor-module:

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

    az iot hub apply-configuration --device-id edge001 --hub-name blogDemoHub --content C:\<yourLocation>\edgeconfig.txt

![Configuratie toepassen][5]

### <a name="6-list-modules"></a>6. Lijst met modules
    
    az iot hub module-identity list --device-id edge001 --hub-name blogDemoHub

![Lijst met modules][6]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een Azure-functie met code voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT-randapparaat gemaakt. Informatie over het gebruik van een IoT-randapparaat als een gateway om te blijven verkennen van Azure IoT rand. 

> [!div class="nextstepaction"]
> [Een IoT-Edge gateway-apparaat maken](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-create-deployment-with-cli-iot-extension/login.jpg
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.jpg
[3]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-hub.jpg
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[5]: ./media/tutorial-create-deployment-with-cli-iot-extension/apply-configuration.PNG
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.PNG

