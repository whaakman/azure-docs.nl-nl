---
title: Services voor schalen in een toepassing net van Azure Service Fabric | Microsoft Docs
description: Leer hoe u onafhankelijk van elkaar schalen services binnen een toepassing die wordt uitgevoerd op Service Fabric net met Azure CLI.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: a4260fd808643971036ad87c01bd2fdec299ccc6
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089740"
---
# <a name="scale-services-within-an-application-running-on-service-fabric-mesh"></a>Schaalservices binnen een toepassing die wordt uitgevoerd op het net van Service Fabric

Dit artikel leest hoe u onafhankelijk van elkaar schalen microservices in een toepassing. In dit voorbeeld Visual objecten toepassing bestaat uit twee microservices; `web` en `worker`. 

De `web` -service is een ASP.NET Core-toepassing met een webpagina waarin driehoeken in de browser. Weergegeven in de browser een driehoek voor elk exemplaar van de `worker` service. 

De `worker` service verplaatst van de driehoek met een vooraf gedefinieerde interval in de ruimte en stuurt een locatie van de driehoek `web` service. DNS wordt gebruikt om op te lossen, het adres van de `web` service.

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

## <a name="deploy-the-application-with-one-worker-service"></a>De toepassing implementeren met een worker-service
Uw toepassing maken in het resource-groep met de `deployment create` opdracht.

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```
De voorgaande opdracht wordt geïmplementeerd op een Linux-toepassing met [mesh_rp.base.linux.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json). Als u een Windows-toepassing implementeren wilt, gebruikt u [mesh_rp.base.windows.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.windows.json). Windows-containerinstallatiekopieën groter zijn dan de Linux-containerinstallatiekopieën en kunnen langer duren om te implementeren.

In een paar minuten wordt weer uw opdracht met:

`visualObjectsApp has been deployed successfully on visualObjectsNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Open de toepassing
Zodra de toepassing met succes is geïmplementeerd, het openbare IP-adres voor het service-eindpunt ophalen en openen op een browser. Er wordt een webpagina weergegeven met een driehoek verplaatsen via de ruimte.

De implementatieopdracht retourneert het openbare IP-adres van het service-eindpunt. U kunt eventueel ook de netwerkbron om het openbare IP-adres van het service-eindpunt opvragen. 
 
De naam van de netwerk-resource voor deze toepassing is `visualObjectsNetwork`, informatie over met behulp van de volgende opdracht ophalen. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name visualObjectsNetwork
```

## <a name="scale-worker-service"></a>Schaal `worker` service

Schaal de `worker` service naar drie exemplaren met de volgende opdracht. 

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```
De voorgaande opdracht wordt geïmplementeerd op een Linux met [mesh_rp.scaleout.linux.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json). Als u een Windows-toepassing implementeren wilt, gebruikt u [mesh_rp.scaleout.windows.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.windows.json). Windows-containerinstallatiekopieën groter zijn dan de Linux-containerinstallatiekopieën en kunnen langer duren om te implementeren.

Zodra de toepassing met succes is geïmplementeerd, kan de browser moet een webpagina met drie driehoeken verplaatsen via de ruimte worden weergegeven.

## <a name="delete-the-resources"></a>De resources verwijderen

Om te besparen de beperkte resources voor de preview-programma, vaak de resources verwijderen. Als u wilt verwijderen van resources met betrekking tot het volgende voorbeeld, verwijder de resourcegroep waarin ze zijn geïmplementeerd.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Volgende stappen
- De voorbeeldtoepassing Visual objecten weergeven op [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects).
- Zie voor meer informatie over Service Fabric-Resourcemodel [Resourcemodel voor Service Fabric-NET](service-fabric-mesh-service-fabric-resources.md).
- Lees voor meer informatie over Service Fabric NET de [overzicht Service Fabric NET](service-fabric-mesh-overview.md).