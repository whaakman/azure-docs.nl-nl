---
title: Over het gebruik van Azure CLI en de IoT-extensie voor het beheren van de IoT Hub Device Provisioning Service | Microsoft Docs
description: Informatie over het gebruik van Azure CLI en de IoT-extensie voor het beheren van de IoT Hub Device Provisioning Service
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: cfbebf8570ee044698b0f4e0abdd58370b04f759
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992861"
---
# <a name="how-to-use-azure-cli-and-the-iot-extension-to-manage-the-iot-hub-device-provisioning-service"></a>Over het gebruik van Azure CLI en de IoT-extensie voor het beheren van de IoT Hub Device Provisioning Service

[Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) is een open-source, cross-platform opdrachtregelprogramma voor het beheren van Azure-resources, zoals IoT Edge. Azure CLI is beschikbaar op Windows, Linux en MacOS. Azure CLI kunt u voor het beheren van resources voor Azure IoT Hub Device Provisioning service-exemplaren en gekoppelde hubs buiten het vak.

Azure CLI verrijkt de IoT-extensie met functies zoals Apparaatbeheer en de volledige functionaliteit van IoT Edge.

In deze zelfstudie maakt stappen u eerst de om Azure CLI en de IoT-extensie in te stellen. Vervolgens leert u hoe u CLI-opdrachten om uit te voeren van de Device Provisioning Service basisbewerkingen uitvoeren. 

## <a name="installation"></a>Installatie 

### <a name="step-1---install-python"></a>Stap 1: Python installeren

[Python 2.7x of Python 3.x](https://www.python.org/downloads/) is vereist.

### <a name="step-2---install-the-azure-cli"></a>Stap 2: de Azure CLI installeren

Ga als volgt de [installatie-instructie](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) voor het instellen van Azure CLI in uw omgeving. Uw Azure CLI-versie moet ten minste 2.0.24 of hoger. Gebruik `az –version` om de versie te valideren. In deze versie worden az-extensie-opdrachten ondersteund en is voor het eerst het Knack-opdrachtframework opgenomen. Een eenvoudige manier om Azure CLI 2.0 te installeren in Windows is de [MSI](https://aka.ms/InstallAzureCliWindows) te downloaden en installeren.

### <a name="step-3---install-iot-extension"></a>Stap 3: IoT-extensie installeren

[In het Leesmij-bestand bij de IoT-extensie](https://github.com/Azure/azure-iot-cli-extension) worden verschillende manieren voor het installeren van de extensie beschreven. De eenvoudigste manier is `az extension add --name azure-cli-iot-ext` uit te voeren. Na de installatie kunt u gebruikmaken van `az extension list` om de momenteel geïnstalleerde extensies te valideren of van `az extension show --name azure-cli-iot-ext` voor informatie over de IoT-extensie. U kunt `az extension remove --name azure-cli-iot-ext` gebruiken om de extensie te verwijderen.


## <a name="basic-device-provisioning-service-operations"></a>Basisbewerkingen van de Device Provisioning Service
Het voorbeeld ziet u hoe u Meld u aan bij uw Azure-account, maakt een Azure Resource Group (een container die verwante resources voor een Azure-oplossing bevat), een IoT-Hub maken, maken van een Device Provisioning service, lijst van de bestaande services voor Apparaatinrichting en een gekoppelde IoT-hub maken met CLI-opdrachten. 

Voer de hierboven beschreven installatiestappen uit voordat u begint. Als u geen Azure-account hebt, kunt u nu [een gratis account maken](https://azure.microsoft.com/free/?v=17.39a). 


### <a name="1-log-in-to-the-azure-account"></a>1. Aanmelden bij het Azure-account
  
    az login

![aanmelding][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Een resourcegroep IoTHubBlogDemo in eastus maken

    az group create -l eastus -n IoTHubBlogDemo

![Een resourcegroep maken][2]


### <a name="3-create-two-device-provisioning-services"></a>3. Twee Device Provisioning Services

    az iot dps create --resource-group IoTHubBlogDemo --name demodps

![Device Provisioning Service maken][3]

    az iot dps create --resource-group IoTHubBlogDemo --name demodps2

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. Lijst van alle bestaande Device Provisioning services onder deze resourcegroep

    az iot dps list --resource-group IoTHubBlogDemo

![Services voor Apparaatinrichting lijst][4]


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. Een IoT Hub-blogDemoHub onder de nieuwe resourcegroep maken

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![IoT Hub maken][5]

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. Een bestaande IoT-Hub koppelen aan een Device Provisioning service

    az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l westus

![Hub koppelen][5]

<!-- Images -->
[1]: ./media/how-to-manage-dps-with-cli/login.jpg
[2]: ./media/how-to-manage-dps-with-cli/create-resource-group.jpg
[3]: ./media/how-to-manage-dps-with-cli/create-dps.jpg
[4]: ./media/how-to-manage-dps-with-cli/list-dps.jpg
[5]: ./media/how-to-manage-dps-with-cli/create-hub.jpg
[6]: ./media/how-to-manage-dps-with-cli/link-hub.jpg


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het apparaat inschrijven
> * Het apparaat starten
> * Controleren of het apparaat is geregistreerd

Ga naar de volgende zelfstudie voor informatie over het inrichten van meerdere apparaten via hubs met gelijke taakverdeling. 

> [!div class="nextstepaction"]
> [Apparaten inrichten in meerdere IoT-hubs met gelijke taakverdeling](./tutorial-provision-multiple-hubs.md)
