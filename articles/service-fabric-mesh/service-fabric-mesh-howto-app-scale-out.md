---
title: Services voor schalen in een toepassing net van Azure Service Fabric | Microsoft Docs
description: Leer hoe u onafhankelijk van elkaar schalen services binnen een toepassing die wordt uitgevoerd op Service Fabric net met Azure CLI.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/08/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: e68bcd135c33c7fd83908b8fed0fd098a698fd36
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42060100"
---
# <a name="scale-services-within-an-application-running-on-service-fabric-mesh"></a>Schaalservices binnen een toepassing die wordt uitgevoerd op het net van Service Fabric

Dit artikel leest hoe u onafhankelijk van elkaar schalen microservices in een toepassing. In dit voorbeeld de toepassing Visual objecten bestaat uit twee microservices: `web` en `worker`.

De `web` -service is een ASP.NET Core-toepassing met een webpagina waarin driehoeken in de browser. Weergegeven in de browser een driehoek voor elk exemplaar van de `worker` service.

De `worker` service verplaatst van de driehoek met een vooraf gedefinieerde interval in de ruimte en stuurt een locatie van de driehoek `web` service. DNS wordt gebruikt om op te lossen, het adres van de `web` service.

## <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI instellen

U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om deze taak te voltooien. Installeer de Azure Service Fabric Mesh CLI-uitbreidingsmodule door de volgende [instructies](service-fabric-mesh-howto-setup-cli.md) te volgen.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure en stel uw abonnement in.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep waarin u de toepassing wilt implementeren.

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application-with-one-worker-service"></a>De toepassing implementeren met een worker-service

Uw toepassing maken in het resource-groep met de `deployment create` opdracht.

Het volgende voorbeeld wordt geïmplementeerd op een Linux-toepassing met de [mesh_rp.base.linux.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json). Voor het implementeren van een Windows-toepassing, gebruikt u de [[mesh_rp.base.windows.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.windows.json). Windows-containerinstallatiekopieën zijn groter dan Linux-containerinstallatiekopieën en het kan langer duren om ze te implementeren.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
```

In een paar minuten weer de opdracht met:

`visualObjectsApp has been deployed successfully on visualObjectsNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>De toepassing openen

De implementatieopdracht retourneert het openbare IP-adres van het service-eindpunt. Zodra de toepassing met succes is geïmplementeerd, wordt het openbare IP-adres voor het service-eindpunt ophalen en vanuit een browser te openen. Er verschijnt een webpagina weergegeven met een zwevend driehoek.

De naam van de netwerk-resource voor deze toepassing is `visualObjectsNetwork`. Ziet u informatie over de app, zoals de beschrijving, de locatie, de resourcegroep, enz. met behulp van de volgende opdracht uit:

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name visualObjectsNetwork
```

## <a name="scale-worker-service"></a>Schaal `worker` service

Schaal de `worker` service naar drie exemplaren met de volgende opdracht. Het volgende voorbeeld wordt geïmplementeerd op een Linux-toepassing met de [mesh_rp.scaleout.linux.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json). Voor het implementeren van een Windows-toepassing, gebruikt u de [mesh_rp.scaleout.windows.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.windows.json). Let erop dat grotere containerinstallatiekopieën duurt langer om te implementeren.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```

Zodra de toepassing met succes is geïmplementeerd, moet de browser een webpagina met drie verplaatsen driehoeken weergegeven.

## <a name="delete-the-resources"></a>De resources verwijderen

Vaak verwijdert de resources die u niet langer in Azure gebruikt. Als u wilt verwijderen van de bronnen die betrekking hebben op dit voorbeeld, verwijdert u de resourcegroep waarin ze zijn geïmplementeerd (Hierdoor worden ook verwijderd alles dat is gekoppeld aan de resourcegroep) met de volgende opdracht:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- De voorbeeldtoepassing Visual objecten weergeven op [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects).
- Zie voor meer informatie over Service Fabric-Resourcemodel [Resourcemodel voor Service Fabric-NET](service-fabric-mesh-service-fabric-resources.md).
- Lees voor meer informatie over Service Fabric NET de [overzicht Service Fabric NET](service-fabric-mesh-overview.md).