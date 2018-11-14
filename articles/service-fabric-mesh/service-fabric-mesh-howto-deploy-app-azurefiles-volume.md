---
title: Status in een toepassing met Azure Service Fabric NET Store door het koppelen van een volume op basis van Azure-bestanden in de container | Microsoft Docs
description: Informatie over het status opslaan in een Azure Service Fabric NET toepassing door te koppelen van een volume op basis van Azure-bestanden in de container met de Azure CLI.
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
ms.date: 08/09/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: cb5b421c1bcfe888d65335f3ab7f67bed80eec34
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614256"
---
# <a name="store-state-in-an-azure-service-fabric-mesh-application-by-mounting-an-azure-files-based-volume-inside-the-container"></a>Volume in de container op basis van status van de Store in een Azure Service Fabric NET toepassing door te koppelen van een Azure-bestanden

In dit artikel laat zien hoe status opslaan in Azure-bestanden door het koppelen van een volume in de container van een Service Fabric-NET-toepassing. In dit voorbeeld heeft de toepassing teller een ASP.NET Core-service met een webpagina waarin de waarde van het prestatiemeteritem in een browser. 

De `counterService` periodiek een itemwaarde leest uit een bestand, verhoogt het en schrijven naar het bestand weer. Het bestand is opgeslagen in een map die is gekoppeld op het volume dat wordt ondersteund door Azure-bestandsshare.

## <a name="prerequisites"></a>Vereisten

U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om deze taak te voltooien. Zorg ervoor dat voor het gebruik van de Azure CLI met dit artikel, `az --version` ten minste retourneert `azure-cli (2.0.43)`.  De module Azure Service Fabric-NET CLI-extensie Volg hiervoor de volgende installeren (of bijwerken) [instructies](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure en stel uw abonnement in.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-file-share"></a>Een bestandsshare maken

Een Azure-bestandsshare maken Volg hiervoor de volgende [instructies](/azure/storage/files/storage-how-to-create-file-share). Naam van het opslagaccount, de sleutel van het opslagaccount en de naam van bestandsshare wordt verwezen als `<storageAccountName>`, `<storageAccountKey>`, en `<fileShareName>` in de volgende instructies. Deze waarden zijn beschikbaar in uw Azure-portal:
* <storageAccountName> -Onder **Opslagaccounts**, dit is de naam van het opslagaccount dat u hebt gebruikt toen u de bestandsshare gemaakt.
* <storageAccountKey> -Selecteer uw opslagaccount onder **Opslagaccounts** en selecteer vervolgens **toegangssleutels** en gebruikt u de waarde onder **key1**.
* <fileShareName> -Selecteer uw opslagaccount onder **Opslagaccounts** en selecteer vervolgens **bestanden**. De naam moet worden gebruikt, is de naam van de bestandsshare die u zojuist hebt gemaakt.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep waarin u de toepassing wilt implementeren. De volgende opdracht maakt u een resourcegroep met de naam `myResourceGroup` op een locatie in de oostelijke VS.

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-template"></a>De sjabloon implementeren

Maken van de toepassing en de bijbehorende resources met behulp van de volgende opdracht uit en geef de waarden voor `storageAccountName`, `storageAccountKey` en `fileShareName` uit de eerdere [een bestandsshare maken](#create-a-file-share) stap.

De `storageAccountKey` parameter in de sjabloon is een beveiligde tekenreeks. Deze worden niet weergegeven in de status van de implementatie en `az mesh service show` opdrachten. Zorg ervoor dat deze correct is opgegeven in de volgende opdracht uit.

De volgende opdracht wordt geïmplementeerd op een Linux-toepassing met de [counter.azurefilesvolume.linux.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/counter/counter.azurefilesvolume.linux.json). Voor het implementeren van een Windows-toepassing, gebruikt u de [counter.azurefilesvolume.windows.json sjabloon](https://sfmeshsamples.blob.core.windows.net/templates/counter/counter.azurefilesvolume.windows.json). Let erop dat grotere containerinstallatiekopieën duurt langer om te implementeren.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/counter/counter.azurefilesvolume.linux.json  --parameters "{\"location\": {\"value\": \"eastus\"}, \"fileShareName\": {\"value\": \"<fileShareName>\"}, \"storageAccountName\": {\"value\": \"<storageAccountName>\"}, \"storageAccountKey\": {\"value\": \"<storageAccountKey>\"}}"
```

In een paar minuten, de opdracht moet worden geretourneerd met `counterApp has been deployed successfully on counterAppNetwork with public ip address <IP Address>`

## <a name="open-the-application"></a>De toepassing openen

De implementatieopdracht retourneert het openbare IP-adres van het service-eindpunt. Zodra de toepassing met succes is geïmplementeerd, wordt het openbare IP-adres voor het service-eindpunt ophalen en vanuit een browser te openen. Een webpagina wordt weergegeven met de itemwaarde die elke seconde wordt bijgewerkt.

De naam van de netwerk-resource voor deze toepassing is `counterAppNetwork`. Ziet u informatie over de app, zoals de beschrijving, de locatie, de resourcegroep, enz. met behulp van de volgende opdracht uit:

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name counterAppNetwork
```

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Controleer of de toepassing kan het volume gebruiken

Maakt een bestand met de naam van de toepassing `counter.txt` in het bestand delen binnen `counter/counterService` map. De inhoud van dit bestand is de itemwaarde die wordt weergegeven op de webpagina wordt weergegeven.

Het bestand kan worden gedownload met elk hulpprogramma waarmee een Azure Files-bestandsshare, zoals bladeren door de [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

## <a name="delete-the-resources"></a>De resources verwijderen

Vaak verwijdert de resources die u niet langer in Azure gebruikt. Als u wilt verwijderen van de bronnen die betrekking hebben op dit voorbeeld, verwijdert u de resourcegroep waarin ze zijn geïmplementeerd (Hierdoor worden ook verwijderd alles dat is gekoppeld aan de resourcegroep) met de volgende opdracht:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- De voorbeeldtoepassing voor Azure Files-volume bekijken op [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Zie [Inleiding tot Service Fabric Resource-model](service-fabric-mesh-service-fabric-resources.md) voor meer informatie over het Service Fabric Resource-model.
- Lees [Wat is Service Fabric Mesh?](service-fabric-mesh-overview.md) voor meer informatie over Service Fabric Mesh.