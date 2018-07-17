---
title: -App implementeren vanuit een persoonlijk register naar Azure Service Fabric NET | Microsoft Docs
description: Informatie over het implementeren van een app die gebruikmaakt van een privécontainerregister voor Service Fabric net met Azure CLI.
services: service-fabric
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
ms.date: 07/16/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: 0a70cd1bd8cd7df099250ca59b3f00b1cab29e5c
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076143"
---
# <a name="deploy-a-service-fabric-mesh-app-from-a-private-container-image-registry"></a>Een Service Fabric-NET-app implementeren vanuit een persoonlijk containerregister-installatiekopie

In dit artikel laat zien hoe een Azure Service Fabric NET-app die gebruikmaakt van het installatiekopie van een privécontainerregister implementeren.

## <a name="prerequisites"></a>Vereisten

### <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric NET CLI instellen 
U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om deze taak te voltooien. Azure Service Fabric-NET CLI-extensie-module installeren Volg hiervoor de volgende [instructies](service-fabric-mesh-howto-setup-cli.md).

### <a name="install-docker"></a>Docker installeren

Docker ter ondersteuning van de beperkte Service Fabric-apps die worden gebruikt door de Service Fabric NET installeren.

### <a name="windows-10"></a>Windows 10

Download en installeer de nieuwste versie van [Docker Community Edition van Windows][download-docker]. 

Tijdens de installatie, selecteert u **gebruik Windows-containers in plaats van Linux-containers** wanneer u wordt gevraagd. U moet vervolgens meldt u zich af en meld u opnieuw aan. Nadat u zich weer aanmeldt, als u Hyper-V, eerder niet hebt ingeschakeld u mogelijk gevraagd om in te schakelen van Hyper-V. U moet Hyper-V inschakelen en de computer opnieuw opstarten.

Nadat de computer opnieuw is opgestart, Docker wordt u gevraagd om in te schakelen de **Containers** presenteren, inschakelen en op de computer opnieuw opstarten.

### <a name="windows-server-2016"></a>Windows Server 2016

Gebruik de volgende PowerShell-opdrachten om Docker te installeren. Zie voor meer informatie, [Docker Enterprise Edition voor Windows Server][download-docker-server].

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

Uw computer opnieuw opstarten.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Aanmelden bij Azure en het actieve abonnement instellen:

```azurecli-interactive
az login
az account set --subscription "<subscriptionName>"
```

## <a name="create-a-container-registry-and-push-an-image-to-it"></a>Een containerregister maken en pushen van een installatiekopie toe

Een Azure Container Registry maken door de instructies in [een privé dockerregister in Azure maken met de Azure CLI](../container-registry/container-registry-get-started-azure-cli.md). Voer de stappen uit tot de [aanmelden bij ACR](../container-registry/container-registry-get-started-azure-cli.md#log-in-to-acr) stap. 

### <a name="push-image-to-acr"></a>Installatiekopie naar ACR overdragen met een push-bewerking

Als u een installatiekopie naar een Azure Container Registry wilt pushen, moet u eerst over een installatiekopie beschikken. Als u nog geen lokale containerinstallatiekopieën hebt, voert u de volgende opdracht uit om een bestaande installatiekopie op te halen uit Docker Hub.

```bash
docker pull seabreeze/azure-mesh-helloworld:1.1-alpine
```

Voordat u een installatiekopie naar het register kunt pushen, moet u deze taggen met de volledige gekwalificeerde naam van de ACR-aanmeldingsserver. Voer de volgende opdracht uit om de volledig naam van de aanmeldingsserver van het ACR-exemplaar op te halen.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Label de installatiekopie met de opdracht [docker tag][docker-tag]. Vervang `<acrLoginServer>` door de aanmeldingsnaam van het ACR-exemplaar.

```bash
docker tag seabreeze/azure-mesh-helloworld:1.1-alpine <acrLoginServer>/azure-mesh-helloworld:1.1-alpine
```
### <a name="list-container-images"></a>Containerinstallatiekopieën opvragen

In het volgende voorbeeld worden de opslagplaatsen in een register vermeld:

```azurecli
az acr repository list --name <acrName> --output table
```

Uitvoer:

```bash
Result
----------------
azure-mesh-helloworld
```

Het volgende voorbeeld worden de tags in de **azure-mesh-helloworld** opslagplaats.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-mesh-helloworld --output table
```

Uitvoer:

```bash
Result
--------
1.1-alpine
```

Dit ziet u dat `azure-mesh-helloworld:1.1-alpine` installatiekopie aanwezig is in het persoonlijke container registry.

## <a name="retrieve-credentials-for-the-registry"></a>Referenties voor het register worden opgehaald

Als u wilt implementeren op een containerexemplaar uit het register die is gemaakt, moeten er referenties worden opgegeven tijdens de implementatie. Schakel de gebruiker met beheerdersrechten op uw register met de volgende opdracht uit:

```azurecli-interactive
az acr update --name <acrName> --admin-enabled true
```

Haal de naam van de registry, gebruikersnaam en wachtwoord met behulp van de volgende opdrachten:

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
az acr credential show --name <acrName> --query username
az acr credential show --name <acrName> --query "passwords[0].value"
```

De waarden geleverd door de voorgaande opdrachten wordt verwezen als `<acrLoginServer>`, `<acrUserName>`, en `<acrPassword>` in de volgende opdracht.

## <a name="deploy-the-template"></a>De sjabloon implementeren

Maken van de toepassing en de bijbehorende resources met behulp van de volgende opdracht uit en geef de referenties van de vorige stap.

De `registry-password` parameter in de sjabloon is een `securestring`. Deze worden niet weergegeven in de status van de implementatie en `az mesh service show` opdrachten. Zorg ervoor dat deze correct is opgegeven in de volgende opdracht uit.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.private_registry.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}, \"registry-server\": {\"value\": \"<acrLoginServer>\"}, \"registry-username\": {\"value\": \"<acrUserName>\"}, \"registry-password\": {\"value\": \"<acrPassword>\"}}"
```

In een paar minuten wordt weer uw opdracht met:

`helloWorldPrivateRegistryApp has been deployed successfully on helloWorldPrivateRegistryNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Open de toepassing
Zodra de toepassing met succes is geïmplementeerd, het openbare IP-adres voor het service-eindpunt ophalen en openen op een browser. Een webpagina met Service Fabric-NET-logo moet worden weergegeven.

De implementatieopdracht retourneert het openbare IP-adres van het service-eindpunt. U kunt ook een query de netwerkbron om het openbare IP-adres van het service-eindpunt.
 
De naam van de netwerk-resource voor deze toepassing is `helloWorldPrivateRegistryNetwork`, informatie over met behulp van de volgende opdracht ophalen. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name helloWorldPrivateRegistryNetwork
```

## <a name="delete-the-resources"></a>De resources verwijderen

Om te besparen de beperkte resources voor de preview-programma, vaak de resources verwijderen. Als u wilt verwijderen van resources met betrekking tot het volgende voorbeeld, verwijder de resourcegroep waarin ze zijn geïmplementeerd.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Volgende stappen
- Weergeven van de toepassing Hello World-voorbeeld op [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld).
- Zie voor meer informatie over Service Fabric-Resourcemodel [Resourcemodel voor Service Fabric-NET](service-fabric-mesh-service-fabric-resources.md).
- Lees voor meer informatie over Service Fabric NET de [overzicht Service Fabric NET](service-fabric-mesh-overview.md).

[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/