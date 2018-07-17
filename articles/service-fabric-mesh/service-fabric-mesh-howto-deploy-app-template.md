---
title: Een app implementeren in Azure Service Fabric net met behulp van een sjabloon | Microsoft Docs
description: Informatie over het implementeren van een .NET Core-toepassing naar Service Fabric-net van een sjabloon met behulp van de Azure CLI.
services: service-fabric
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
ms.openlocfilehash: 11b3ad6bf83eebfc94ead6f8a730b8e6a6b8de2f
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076152"
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

`VotingApp has been deployed successfully on VotingAppNetwork with public ip address <IP address>.` 

Het IP-adres is bijvoorbeeld 13.68.129.22.

## <a name="open-the-application"></a>Open de toepassing
Zodra de toepassing met succes is geïmplementeerd, moet u een verbinding maken met het eindpunt van de service (13.68.129.22 uit het vorige voorbeeld) in een browser.  

![Stemtoepassing](./media/service-fabric-mesh-howto-deploy-app-template/VotingApplication.png)

U kunt nu stemopties toevoegen aan de toepassing en erop stemmen of verwijderen van de stemopties.

## <a name="check-the-application-details"></a>Controleer de details van de toepassing
U kunt de status van de toepassing controleren met behulp van de `app show` opdracht. De naam van de toepassing voor de geïmplementeerde toepassing is 'VotingApp', dus het ophalen van de details. 

```azurecli-interactive
az mesh app show --resource-group myResourceGroup --name VotingApp
```

U kunt ook de netwerkbron om het IP-adres van de container waar de service is geïmplementeerd met de opdracht 'az NET network show' een query:

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name VotingAppNetwork
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


