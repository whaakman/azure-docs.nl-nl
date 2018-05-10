---
title: Azure CLI 2.0 en de IoT-extensie gebruiken om services voor het inrichten van apparaten te beheren | Microsoft Docs
description: Azure CLI 2.0 en de IoT-extensie leren gebruiken om services voor het inrichten van apparaten te beheren
services: iot-dps
keywords: ''
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 8e8bbf5808c11709a49f1cb6ebeba410837e5810
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="how-to-use-azure-cli-20-and-the-iot-extension-to-manage-device-provisioning-services"></a>Azure CLI 2.0 en de IoT-extensie gebruiken om services voor het inrichten van apparaten te beheren

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) is een open-source, cross-platform opdrachtregelprogramma voor het beheren van Azure-resources, zoals IoT Edge. Azure CLI 2.0 is beschikbaar voor Windows, Linux en Mac OS. Met Azure CLI 2.0 kunt u Azure IoT Hub-resources, service-exemplaren voor apparaatinrichting en gebruiksklare gekoppelde hubs beheren.

De IoT-extensie voorziet Azure CLI 2.0 van extra functies zoals Apparaatbeheer en alle functionaliteit van IoT Edge.

In deze zelfstudie voert u eerst de stappen uit om Azure CLI 2.0 en de IoT-extensie in te stellen. Vervolgens leert u CLI-opdrachten te gebruiken om eenvoudige servicebewerkingen voor apparaatinrichting uit te voeren. 

## <a name="installation"></a>Installatie 

### <a name="step-1---install-python"></a>Stap 1: Python installeren

[Python 2.7x of Python 3.x](https://www.python.org/downloads/) is vereist.

### <a name="step-2---install-azure-cli-20"></a>Stap 2: Azure CLI 2.0 installeren

Volg de [installatie-instructie](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) voor het instellen van Azure CLI 2.0 in uw omgeving. Uw versie van Azure CLI 2.0 moet minimaal versie 2.0.24 of hoger zijn. Gebruik `az –version` om de versie te valideren. In deze versie worden az-extensie-opdrachten ondersteund en is voor het eerst het Knack-opdrachtframework opgenomen. Een eenvoudige manier om Azure CLI 2.0 te installeren in Windows is de [MSI](https://aka.ms/InstallAzureCliWindows) te downloaden en installeren.

### <a name="step-3---install-iot-extension"></a>Stap 3: IoT-extensie installeren

[In het Leesmij-bestand bij de IoT-extensie](https://github.com/Azure/azure-iot-cli-extension) worden verschillende manieren voor het installeren van de extensie beschreven. De eenvoudigste manier is `az extension add --name azure-cli-iot-ext` uit te voeren. Na de installatie kunt u gebruikmaken van `az extension list` om de momenteel geïnstalleerde extensies te valideren of van `az extension show --name azure-cli-iot-ext` voor informatie over de IoT-extensie. U kunt `az extension remove --name azure-cli-iot-ext` gebruiken om de extensie te verwijderen.


## <a name="basic-device-provisioning-service-operations"></a>Eenvoudige servicebewerkingen voor apparaatinrichting
Het voorbeeld laat zien hoe u zich aanmeldt bij uw Azure-account, een Azure-resourcegroep maakt (bestaande uit een container die verwante resources voor een Azure-oplossing bevat), een IoT Hub maakt, een service voor apparaatinrichting maakt, de bestaande services voor apparaatinrichting weergeeft en een gekoppelde IoT-hub maakt met behulp van CLI-opdrachten. 

Voer de hierboven beschreven installatiestappen uit voordat u begint. Als u geen Azure-account hebt, kunt u nu [een gratis account maken](https://azure.microsoft.com/free/?v=17.39a). 


### <a name="1-log-in-to-the-azure-account"></a>1. Aanmelden bij het Azure-account
  
    az login

![aanmelding][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Een resourcegroep IoTHubBlogDemo in eastus maken

    az group create -l eastus -n IoTHubBlogDemo

![Een resourcegroep maken][2]


### <a name="3-create-two-device-provisioning-services"></a>3. Twee services voor apparaatinrichting maken

    az iot dps create --resource-group IoTHubBlogDemo --name demodps

![DPS maken][3]

    az iot dps create --resource-group IoTHubBlogDemo --name demodps2

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. Alle bestaande services voor apparaatinrichting onder deze resourcegroep weergeven

    az iot dps list --resource-group IoTHubBlogDemo

![DPS weergeven][4]


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. Een IoT Hub-blogDemoHub onder de nieuwe resourcegroep maken

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![IoT Hub maken][5]

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. Eén bestaande IoT-hub koppelen aan een service voor apparaatinrichting

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
