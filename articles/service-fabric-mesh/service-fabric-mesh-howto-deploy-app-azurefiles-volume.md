---
title: Store status doordat Azure Files op basis van volume in de container in Service Fabric-NET-toepassing | Microsoft Docs
description: Leer hoe u voor het opslaan van status doordat Azure Files op basis van volume in de container in Service Fabric-NET-toepassing met de Azure CLI.
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
ms.openlocfilehash: 94a4e17e6285893520a2f6482b32a69b1229e2fa
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090629"
---
# <a name="store-state-by-mounting-azure-files-based-volume-in-service-fabric-mesh-application"></a>Volume in Service Fabric-NET-toepassing op basis van Store status doordat Azure Files

In dit artikel laat zien hoe status opslaan in Azure-bestanden door het koppelen van een volume in de container van een Service Fabric-NET-toepassing. In dit voorbeeld heeft de teller toepassing een ASP.NET Core-service met een webpagina waarin de waarde van het prestatiemeteritem in een browser. 

De `counterService` perodically een itemwaarde leest uit een bestand, verhoogt het en schrijven naar het bestand weer. Het bestand is opgeslagen in een map die is gekoppeld op het volume dat wordt ondersteund door Azure-bestandsshare. 

## <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric NET CLI instellen 
U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om deze taak te voltooien. Azure Service Fabric-NET CLI-extensie-module installeren Volg hiervoor de volgende [instructies](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Aanmelden bij Azure en uw abonnement instellen.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-file-share"></a>Bestandsshare maken 
Een Azure-bestandsshare maken Volg hiervoor de volgende [instructies](/azure/storage/files/storage-how-to-create-file-share). Naam van het opslagaccount, de sleutel van het opslagaccount en de naam van bestandsshare wordt verwezen als `<storageAccountName>`, `<storageAccountKey>`, en `<fileShareName>` in de volgende instructies.

## <a name="create-resource-group"></a>Een resourcegroep maken
Maak een resourcegroep voor het implementeren van de toepassing. U kunt een bestaande resourcegroep gebruiken en sla deze stap over. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-template"></a>De sjabloon implementeren

Maken van de toepassing en de bijbehorende resources met behulp van de volgende opdracht uit en geef de waarden voor `storageAccountName`, `storageAccountKey` en `fileShareName` uit de vorige stap.

De `storageAccountKey` parameter in de sjabloon is een `securestring`. Deze worden niet weergegeven in de status van de implementatie en `az mesh service show` opdrachten. Zorg ervoor dat deze correct is opgegeven in de volgende opdracht uit.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.linux.json  --parameters "{\"location\": {\"value\": \"eastus\"}, \"fileShareName\": {\"value\": \"<fileShareName>\"}, \"storageAccountName\": {\"value\": \"<storageAccountName>\"}, \"storageAccountKey\": {\"value\": \"<storageAccountKey>\"}}"
```

De voorgaande opdracht wordt geïmplementeerd op een Linux-toepassing met [mesh_rp.linux.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.linux.json). Als u een Windows-toepassing implementeren wilt, gebruikt u [mesh_rp.windows.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.windows.json). Windows-containerinstallatiekopieën groter zijn dan de Linux-containerinstallatiekopieën en kunnen langer duren om te implementeren.

In een paar minuten wordt weer uw opdracht met:

`counterApp has been deployed successfully on counterAppNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Open de toepassing
Zodra de toepassing met succes is geïmplementeerd, het openbare IP-adres voor het service-eindpunt ophalen en openen op een browser. Er wordt een webpagina weergegeven met de itemwaarde die elke seconde wordt bijgewerkt.

De implementatieopdracht retourneert het openbare IP-adres van het service-eindpunt. U kunt eventueel ook de netwerkbron om het openbare IP-adres van het service-eindpunt opvragen. 
 
De naam van de netwerk-resource voor deze toepassing is `counterAppNetwork`, informatie over met behulp van de volgende opdracht ophalen. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name counterAppNetwork
```

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Controleer of de toepassing kan het volume gebruiken
Maakt een bestand met de naam van de toepassing `counter.txt` in het bestand delen binnen `counter/counterService` map. De inhoud van dit bestand is de itemwaarde die wordt weergegeven op de webpagina wordt weergegeven.

Het bestand kan worden gedownload met elk hulpprogramma waarmee een Azure Files-bestandsshare te bladeren. De [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) volgt een voorbeeld van een dergelijk hulpprogramma.

## <a name="delete-the-resources"></a>De resources verwijderen

Om te besparen de beperkte resources voor de preview-programma, vaak de resources verwijderen. Als u wilt verwijderen van resources met betrekking tot het volgende voorbeeld, verwijder de resourcegroep waarin ze zijn geïmplementeerd.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Volgende stappen

- De voorbeeldtoepassing voor Azure Files-volume bekijken op [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Zie voor meer informatie over Service Fabric-Resourcemodel [Resourcemodel voor Service Fabric-NET](service-fabric-mesh-service-fabric-resources.md).
- Lees voor meer informatie over Service Fabric NET de [overzicht Service Fabric NET](service-fabric-mesh-overview.md).
