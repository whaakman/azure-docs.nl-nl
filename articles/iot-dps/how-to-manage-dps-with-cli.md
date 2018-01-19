---
title: Het gebruik van Azure CLI 2.0 en de IoT-extensie voor het beheren van apparaten inrichten van services | Microsoft Docs
description: Informatie over het gebruik van Azure CLI 2.0 en de IoT-extensie voor het beheren van netwerkservices van apparaat
services: iot-dps
keywords: 
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 674245f1e284e7308474fed0f6c53b350ec1c819
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-use-azure-cli-20-and-the-iot-extension-to-manage-device-provisioning-services"></a>Het gebruik van Azure CLI 2.0 en de IoT-extensie voor het beheren van apparaten inrichten van services

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) is een open source cross-platform-opdrachtregelprogramma voor het beheren van Azure-resources, zoals IoT rand. Azure CLI 2.0 is beschikbaar op Windows, Linux en Mac OS. Azure CLI 2.0 kunt u voor het beheren van resources, apparaat inrichting service-exemplaren en gekoppelde hubs gebruiksklaar Azure IoT Hub.

De extensie IoT verrijkt Azure CLI 2.0 met functies zoals Apparaatbeheer en de mogelijkheid om volledige IoT rand.

In deze zelfstudie maakt stappen u eerst de om Azure CLI 2.0 en de IoT-uitbreiding in te stellen. Vervolgens leert u hoe CLI-opdrachten kunt u eenvoudige inrichting servicebewerkingen uitvoeren uit te voeren. 

## <a name="installation"></a>Installatie 

### <a name="step-1---install-python"></a>Stap 1 - Install Python

[Python 2.7 x of Python 3.x](https://www.python.org/downloads/) is vereist.

### <a name="step-2---install-azure-cli-20"></a>Stap 2: Installeer Azure CLI 2.0

Ga als volgt de [installatie-instructie](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) voor het instellen van Azure CLI 2.0 in uw omgeving. Uw versie van Azure CLI 2.0 moet ten minste 2.0.24 of hoger. Gebruik `az –version` om te valideren. Deze versie ondersteunt az extensie opdrachten en maakt u kennis met de opdracht talent framework. Een eenvoudige manier om te installeren op Windows is downloaden en installeren de [MSI](https://aka.ms/InstallAzureCliWindows).

### <a name="step-3---install-iot-extension"></a>Stap 3 - installatie IoT-uitbreiding

[Het Leesmij-bestand extensie IoT](https://github.com/Azure/azure-iot-cli-extension) beschrijft de verschillende manieren voor het installeren van de extensie. De eenvoudigste manier is om uit te voeren `az extension add --name azure-cli-iot-ext`. Na de installatie, kunt u `az extension list` voor het valideren van de momenteel geïnstalleerde extensies of `az extension show --name azure-cli-iot-ext` voor informatie over de IoT-uitbreiding. U kunt gebruiken voor het verwijderen van de extensie `az extension remove --name azure-cli-iot-ext`.


## <a name="basic-device-provisioning-service-operations"></a>Basic apparaat servicebewerkingen inrichten
Het voorbeeld ziet u hoe u aanmelden bij uw Azure-account, maak een Azure-resourcegroep (een container met verwante resources voor een Azure-oplossing), een IoT Hub maken, maken van een apparaat inrichten gevolgd, lijst van de bestaande apparaten inrichten van services en een gekoppelde IoT-hub maken met CLI-opdrachten. 

Voltooi de installatiestappen die hierboven is beschreven voordat u begint. Als u een Azure-account niet hebt, maar u kunt [een gratis account maken](https://azure.microsoft.com/free/?v=17.39a) vandaag. 


### <a name="1-log-in-to-the-azure-account"></a>1. Aanmelden bij de Azure-account
  
    az login

![aanmelding][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Een resourcegroep IoTHubBlogDemo in eastus maken

    az group create -l eastus -n IoTHubBlogDemo

![Een resourcegroep maken][2]


### <a name="3-create-two-device-provisioning-services"></a>3. Twee apparaat inrichting services maken

    az iot dps create --resource-group IoTHubBlogDemo --name demodps

![DP's maken][3]

    az iot dps create --resource-group IoTHubBlogDemo --name demodps2

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. Lijst van alle bestaande apparaten inrichten van services onder deze resourcegroep

    az iot dps list --resource-group IoTHubBlogDemo

![Lijst DP 's][4]


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. Een IoT Hub blogDemoHub onder de nieuwe resourcegroep maken

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![IoT Hub maken][5]

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. Een bestaande IoT-Hub koppelen aan een apparaat-service inricht

    az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l westus

![Link Hub][5]

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
> * Start het apparaat
> * Controleer of dat het apparaat is geregistreerd

Ga naar de volgende zelfstudie voor informatie over het inrichten van meerdere apparaten via hubs taakverdeling. 

> [!div class="nextstepaction"]
> [Inrichten van apparaten via IoT hubs Netwerktaakverdeling](./tutorial-provision-multiple-hubs.md)
