---
title: Een app implementeren in Azure Service Fabric net met behulp van een sjabloon | Microsoft Docs
description: Informatie over het implementeren van een .NET Core-toepassing naar Service Fabric-net van een sjabloon met behulp van de Azure CLI.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceputal
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: 8d813669f2f44fd64db669e9750e3bc064c7f916
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090325"
---
# <a name="deploy-a-service-fabric-mesh-application-to-service-fabric-mesh-using-a-template"></a>Een Service Fabric-NET-toepassing in Service Fabric net met behulp van een sjabloon implementeren
Dit artikel leest hoe u een .NET Core-toepassing in Service Fabric NET implementeert met behulp van een sjabloon. Wanneer u klaar bent, hebt u een stemtoepassing met een ASP.NET Core web-front-end die stemresultaten opslaat in een back-endservice in het cluster. De front-end gebruikt DNS om op te lossen, het adres van de back-endservice.

## <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric NET CLI instellen 
U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om deze taak te voltooien. Azure Service Fabric-NET CLI-extensie-module installeren Volg hiervoor de volgende [instructies](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Aanmelden bij Azure en uw abonnement instellen.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-resource-group"></a>Een resourcegroep maken
Maak een resourcegroep voor het implementeren van de toepassing. U kunt een bestaande resourcegroep gebruiken en sla deze stap over. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application"></a>De toepassing implementeren
Uw toepassing maken in het resource-groep met de `deployment create` opdracht.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.windows.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
```

De voorgaande opdracht wordt geïmplementeerd op een Windows-toepassing met [mesh_rp.windows.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.windows.json). Als u een Linux-toepassing implementeren wilt, gebruikt u [mesh_rp.linux.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.linux.json). Windows-containerinstallatiekopieën groter zijn dan de Linux-containerinstallatiekopieën en kunnen langer duren om te implementeren.

In een paar minuten wordt weer uw opdracht met:

`VotingApp has been deployed successfully on VotingAppNetwork with public ip address <IP address>` 

## <a name="open-the-application"></a>Open de toepassing
Zodra de toepassing met succes is geïmplementeerd, het openbare IP-adres voor het service-eindpunt ophalen en openen op een browser. Wordt weergegeven na de webpagina. 

![Stemtoepassing](./media/service-fabric-mesh-howto-deploy-app-template/VotingApplication.png)

U kunt nu stemopties toevoegen aan de toepassing en erop stemmen of verwijderen van de stemopties.

De implementatieopdracht retourneert het openbare IP-adres van het service-eindpunt. U kunt eventueel ook de netwerkbron om het openbare IP-adres van het service-eindpunt opvragen. 

De naam van de netwerk-resource voor deze toepassing is `VotingAppNetwork`, informatie over met behulp van de volgende opdracht ophalen. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name VotingAppNetwork
```

## <a name="check-the-application-details"></a>Controleer de details van de toepassing
U kunt de status van de toepassing controleren met behulp van de `app show` opdracht. De naam van de toepassing voor de geïmplementeerde toepassing is 'VotingApp', dus het ophalen van de details. 

```azurecli-interactive
az mesh app show --resource-group myResourceGroup --name VotingApp
```

## <a name="list-the-deployed-applications"></a>Lijst van de geïmplementeerde toepassingen
U kunt de opdracht ' applijst' gebruiken om op te halen een lijst met toepassingen die u hebt geïmplementeerd op uw abonnement. 

```azurecli-interactive
az mesh app list -o table
```

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de toepassing niet meer nodig hebt en deze gerelateerd resources, verwijder de resourcegroep die deze bevat. 

```azurecli-interactive
az group delete --resource-group myResourceGroup  
``` 

## <a name="next-steps"></a>Volgende stappen
- De voorbeeldstemtoepassing weergeven op [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp).
- Lees voor meer informatie over Service Fabric NET de [overzicht Service Fabric NET](service-fabric-mesh-overview.md).


