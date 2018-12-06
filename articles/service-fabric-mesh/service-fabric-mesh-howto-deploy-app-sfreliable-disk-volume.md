---
title: Maximaal beschikbare Service Fabric betrouwbare schijfvolume gebruiken in een net van Azure Service Fabric-toepassing | Microsoft Docs
description: Informatie over het status opslaan in een Azure Service Fabric NET toepassing door te koppelen van Service Fabric betrouwbare schijf op basis van volume in de container met de Azure CLI.
services: service-fabric-mesh
documentationcenter: .net
author: ashishnegi
manager: raunakpandya
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/03/2018
ms.author: asnegi
ms.custom: mvc, devcenter
ms.openlocfilehash: 2ed64bbf0da252285184e2ca6fef0555a85ce149
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52955318"
---
# <a name="mount-highly-available-service-fabric-reliable-disk-based-volume-in-a-service-fabric-mesh-application"></a>Maximaal beschikbare Service Fabric betrouwbare schijf op basis van volume in een Service Fabric-NET-toepassing koppelen 
De algemene methode voor permanente status met container-apps is het gebruik van externe opslag, zoals Azure File Storage- of -database, zoals Azure Cosmos DB. Dit leidt tot aanzienlijke lezen en schrijven netwerklatentie naar de externe opslag.

In dit artikel laat zien hoe status opslaan in maximaal beschikbare Service Fabric betrouwbare schijf door het koppelen van een volume in de container van een Service Fabric-NET-toepassing.
Service Fabric betrouwbare schijf biedt volumes voor lokale leesbewerkingen schrijfbewerkingen worden gerepliceerd binnen de Service Fabric-Cluster voor hoge beschikbaarheid. Dit verwijdert netwerkaanroepen voor leesbewerkingen en vermindert netwerklatentie voor schrijfbewerkingen. Als de container opnieuw is opgestart of naar een ander knooppunt verplaatst, ziet nieuwe containerinstantie hetzelfde volume als oudere. Het is dus efficiënte en maximaal beschikbaar.

In dit voorbeeld heeft de toepassing teller een ASP.NET Core-service met een webpagina waarin de waarde van het prestatiemeteritem in een browser.

De `counterService` periodiek een itemwaarde leest uit een bestand, verhoogt het en schrijven naar het bestand weer. Het bestand is opgeslagen in een map die is gekoppeld op het volume dat wordt ondersteund door Service Fabric betrouwbare schijf.

## <a name="prerequisites"></a>Vereisten

U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om deze taak te voltooien. Zorg ervoor dat voor het gebruik van de Azure CLI met dit artikel, `az --version` ten minste retourneert `azure-cli (2.0.43)`.  De module Azure Service Fabric-NET CLI-extensie Volg hiervoor de volgende installeren (of bijwerken) [instructies](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure en stel uw abonnement in.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep waarin u de toepassing wilt implementeren. De volgende opdracht maakt u een resourcegroep met de naam `myResourceGroup` op een locatie in de oostelijke VS. Als u de naam van de resourcegroep in onderstaande opdracht wijzigt, vergeet dan niet te wijzigen in alle opdrachten die volgen.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="deploy-the-template"></a>De sjabloon implementeren

De volgende opdracht wordt geïmplementeerd op een Linux-toepassing met de [counter.sfreliablevolume.linux.json sjabloon](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/counter/counter.sfreliablevolume.linux.json). Voor het implementeren van een Windows-toepassing, gebruikt u de [counter.sfreliablevolume.windows.json sjabloon](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/counter/counter.sfreliablevolume.windows.json). Let erop dat grotere containerinstallatiekopieën duurt langer om te implementeren.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://raw.githubusercontent.com/Azure-Samples/service-fabric-mesh/master/templates/counter/counter.sfreliablevolume.linux.json
```

U ziet ook de status van de implementatie met de opdracht

```azurecli-interactive
az group deployment show --name counter.sfreliablevolume.linux --resource-group myResourceGroup
```

U ziet de naam van de gateway-resource die is, typt u als resource `Microsoft.ServiceFabricMesh/gateways`. Dit wordt gebruikt bij het ophalen van openbare IP-adres van de app.

## <a name="open-the-application"></a>De toepassing openen

Zodra de toepassing met succes is geïmplementeerd, krijgt u het IP-adres van de gateway-resource voor de app. Gebruik de naam van de gateway die u hebt gezien tijdens boven de sectie.
```azurecli-interactive
az mesh gateway show --resource-group myResourceGroup --gateway-name counterGateway
```

De uitvoer moet een eigenschap `ipAddress` is het openbare IP-adres voor de service-eindpunt. Openen vanuit een browser. Een webpagina wordt weergegeven met de itemwaarde die elke seconde wordt bijgewerkt.

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Controleer of de toepassing kan het volume gebruiken

Maakt een bestand met de naam van de toepassing `counter.txt` in het volume op `counter/counterService` map. De inhoud van dit bestand is de itemwaarde die wordt weergegeven op de webpagina wordt weergegeven.

## <a name="delete-the-resources"></a>De resources verwijderen

Vaak verwijdert de resources die u niet langer in Azure gebruikt. Als u wilt verwijderen van de bronnen die betrekking hebben op dit voorbeeld, verwijdert u de resourcegroep waarin ze zijn geïmplementeerd (Hierdoor worden ook verwijderd alles dat is gekoppeld aan de resourcegroep) met de volgende opdracht:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- De voorbeeldtoepassing voor Service Fabric betrouwbare Volume schijf weergeven op [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Zie [Inleiding tot Service Fabric Resource-model](service-fabric-mesh-service-fabric-resources.md) voor meer informatie over het Service Fabric Resource-model.
- Lees [Wat is Service Fabric Mesh?](service-fabric-mesh-overview.md) voor meer informatie over Service Fabric Mesh.
