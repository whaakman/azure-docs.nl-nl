---
title: Implementeren van een Service Fabric-NET-app die gebruikmaakt van Azure Files-volume | Microsoft Docs
description: Informatie over het implementeren van een toepassing die gebruikmaakt van de Azure Files-volume om Service Fabric net met Azure CLI.
services: service-fabric
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
ms.openlocfilehash: fb9740efaa9a1033d6602180f5750f6fb550c048
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076039"
---
# <a name="deploy-a-service-fabric-mesh-application-that-uses-the-azure-files-volume"></a>Een Service Fabric-NET-toepassing die gebruikmaakt van het volume Azure Files implementeren
In dit voorbeeld ziet het gebruik van opslagvolumes in een container die wordt uitgevoerd in Azure Service Fabric NET. Als onderdeel van dit voorbeeld:

- Maak een bestandsshare met [Azure Files](/azure/storage/files/storage-files-introduction) 
- Verwijzing die als een volume voor een containerexemplaar dat we distribueren delen
  - Wanneer de container wordt gestart, koppelt deze share als een specifieke locatie in de container
- De code die wordt uitgevoerd in de container schrijft een tekstbestand naar die locatie
- Controleer of dat het bestand correct is geschreven in de share die het volume back-ups

## <a name="example-json-templates"></a>Voorbeeld van JSON-sjablonen

Linux: [https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.linux.json](https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.linux.json)

Windows: [https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.windows.json](https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.windows.json)

## <a name="create-the-azure-files-file-share"></a>De bestanden van de Azure-bestandsshare maken

Volg de instructies in de [documentatie voor Azure Files](/azure/storage/files/storage-how-to-create-file-share) te maken van een bestandsshare voor de toepassing te gebruiken.

## <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric NET CLI instellen
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om deze stappen te voltooien. Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, moet u de Azure CLI versie 2.0.35 installeren of hoger. Voer `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden naar de nieuwste versie van de CLI, Zie [Azure CLI 2.0 installeren] [azure-cli-installatie].

Installeer de Azure Service Fabric-NET CLI-extensie-module. Voor de Preview-versie, Azure Service Fabric-NET CLI geschreven als een uitbreiding naar Azure CLI.

```azurecli-interactive
az extension add --source https://sfmeshcli.blob.core.windows.net/cli/mesh-0.8.1-py2.py3-none-any.whl
```

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Meld u aan bij Azure en uw abonnement instellen.

```azurecli-interactive
az login
az account set --subscription "<subscriptionName>"
```

## <a name="create-resource-group"></a>Een resourcegroep maken
Maak een resourcegroep (RG) te implementeren in dit voorbeeld of u kunt een bestaande resourcegroep gebruiken en sla deze stap over. De Preview-versie is alleen beschikbaar in `eastus` locatie.

```azurecli-interactive
az group create --name <resourceGroupName> --location eastus
```

## <a name="deploy-the-template"></a>De sjabloon implementeren
Maak de toepassing en de bijbehorende resources met behulp van een van de volgende opdrachten.

Voor Linux:

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.linux.json
```

Voor Windows:

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.windows.json
```

Volg de aanwijzingen voor het invoeren van de naam van bestandsshare, de accountnaam en de accountsleutel voor de Azure-bestandsshare waarmee het volume. In een minuut of dat de opdracht moet worden geretourneerd met `"provisioningState": "Succeeded"`.

De parameter password in de sjabloon is van het `string` type voor gebruiksgemak. Deze wordt weergegeven op het scherm in niet-versleutelde tekst en de status van de implementatie.

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Controleer of de toepassing kan het volume gebruiken
Maakt een bestand met de naam van de toepassing _data.txt_ in de bestandsshare (als deze niet al bestaat). De inhoud van dit bestand is een getal dat elke 30 seconden wordt verhoogd door de toepassing. Om te controleren dat het voorbeeld goed werkt, opent u de _data.txt_ periodiek bestands- en controleer of dat het getal wordt bijgewerkt.

Het bestand kan worden gedownload met elk hulpprogramma waarmee een Azure Files-bestandsshare te bladeren. De [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) volgt een voorbeeld van een dergelijk hulpprogramma.

## <a name="next-steps"></a>Volgende stappen

- De voorbeeldtoepassing voor Azure Files-volume bekijken op [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/azurefiles-volume).
- Zie voor meer informatie over Service Fabric-Resourcemodel [Resourcemodel voor Service Fabric-NET](service-fabric-mesh-service-fabric-resources.md).
- Lees voor meer informatie over Service Fabric NET de [overzicht Service Fabric NET](service-fabric-mesh-overview.md).
