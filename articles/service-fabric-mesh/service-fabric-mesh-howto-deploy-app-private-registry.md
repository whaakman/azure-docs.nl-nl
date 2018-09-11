---
title: Een Service Fabric-NET-app implementeren vanuit het installatiekopie van een privécontainerregister naar Azure Service Fabric NET | Microsoft Docs
description: Informatie over het implementeren van een app opschalen van het installatiekopie van een privécontainerregister tot Service Fabric net met Azure CLI.
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
ms.date: 08/20/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: a2791c86512790f36477e562cb82718174df8dc3
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296583"
---
# <a name="deploy-a-service-fabric-mesh-app-from-a-private-container-image-registry"></a>Een Service Fabric-NET-app implementeren vanuit een persoonlijk containerregister-installatiekopie

In dit artikel laat zien hoe een Azure Service Fabric NET-app die gebruikmaakt van het installatiekopie van een privécontainerregister implementeren.

## <a name="prerequisites"></a>Vereiste onderdelen

### <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI instellen

Een lokale installatie van de Azure CLI gebruiken om deze taak te voltooien. Installeer de Azure Service Fabric Mesh CLI-uitbreidingsmodule door de volgende [instructies](service-fabric-mesh-howto-setup-cli.md) te volgen.

## <a name="install-docker"></a>Docker installeren

#### <a name="windows-10"></a>Windows 10

Download en installeer de nieuwste versie van [Docker Community Edition voor Windows][download-docker] ter ondersteuning van de in containers verpakte Service Fabric-apps die door Service Fabric Mesh worden gebruikt.

Tijdens de installatie selecteert u **Use Windows containers instead of Linux containers** als daarom wordt gevraagd. 

Als Hyper-V niet is ingeschakeld op uw computer, kunt u dit doen tijdens de installatie van Docker. Klik op **OK** om dit te doen als u hierom wordt gevraagd.

#### <a name="windows-server-2016"></a>Windows Server 2016

Als Hyper-V niet is ingeschakeld, opent u PowerShell als een beheerder en voert u de volgende opdracht uit om Hyper-V in te schakelen. Start vervolgens uw computer opnieuw op. Zie [Docker Enterprise Edition for Windows Server][download-docker-server] (Docker Enterprise Edition voor Windows Server) voor meer informatie.

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Start de computer opnieuw op.

Open PowerShell als een beheerder en voer de volgende opdracht uit om Docker te installeren:

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Aanmelden bij Azure en het actieve abonnement instellen.

```azurecli-interactive
az login
az account set --subscription "<subscriptionName>"
```

## <a name="create-a-container-registry-and-push-an-image-to-it"></a>Een containerregister maken en pushen van een installatiekopie toe

Gebruik de volgende stappen uit om te maken van een Azure Container Registry.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Gebruik de volgende opdracht om te maken van een resourcegroep met de naam *myResourceGroup* in de *eastus* locatie.

```azurecli
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-container-registry"></a>Een containerregister maken

Maak een Azure container registry (ACR) exemplaar met de `az acr create` opdracht. De registernaam moet uniek zijn binnen Azure en 5 tot 50 alfanumerieke tekens bevatten. In het volgende voorbeeld wordt de naam van de *myContainerRegistry007* wordt gebruikt. Als er een fout optreedt die de registernaam gebruikt wordt, kies een andere naam. Gebruik deze naam overal `<acrName>` wordt weergegeven in deze instructies.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic
```

Wanneer het register is gemaakt, ziet u uitvoer die vergelijkbaar is met het volgende:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-09-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "myContainerRegistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="push-the-image-to-azure-container-registry"></a>De installatiekopie naar Azure Container Registry pushen

Als u wilt een installatiekopie pushen naar een Azure container registry (ACR), moet u eerst een containerinstallatiekopie hebben. Als u nog geen lokale containerinstallatiekopieën hebt, voer de volgende opdracht voor het ophalen van de een installatiekopie van Docker Hub (u moet mogelijk om over te schakelen Docker werken met Linux-installatiekopieën met de rechtermuisknop op de docker-pictogram te selecteren **overschakelen naar de Linux-containers**).

```bash
docker pull seabreeze/azure-mesh-helloworld:1.1-alpine
```

Voordat u een installatiekopie naar het register kunt pushen, moet u deze taggen met de volledige gekwalificeerde naam van de ACR-aanmeldingsserver.

Voer de volgende opdracht om op te halen van de naam van de volledige aanmeldingsserver van het ACR-exemplaar.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

De volledige aanmeldingsnaam die wordt geretourneerd worden aangeduid als `<acrLoginServer>` in de rest van dit artikel.

Nu label uw docker-installatiekopie met de [docker tag] [ docker-tag] opdracht. Vervang in de onderstaande opdracht `<acrLoginServer>` met de naam van de aanmeldingsserver die is gerapporteerd door de bovenstaande opdracht. Het volgende voorbeeld tags seabreeze/azure-mesh-helloworld:1.1-alpine installatiekopie. Als u een andere installatiekopie gebruikt, vervangt u de naam van de installatiekopie in de volgende opdracht.

```bash
docker tag seabreeze/azure-mesh-helloworld:1.1-alpine <acrLoginServer>/seabreeze/azure-mesh-helloworld:1.1-alpine
```

Bijvoorbeeld: `docker tag seabreeze/azure-mesh-helloworld:1.1-alpine myContainerRegistry007.azurecr.io/seabreeze/azure-mesh-helloworld:1.1-alpine`

Meld u aan bij Azure Container Registry.

```bash
az acr login -n <acrName>
```

Bijvoorbeeld: `az acr login -n myContainerRegistry007`

Push de installatiekopie naar azure container registry met de volgende opdracht:

```bash
docker push <acrLoginServer>/seabreeze/azure-mesh-helloworld:1.1-alpine
```

Bijvoorbeeld: `docker push myContainerRegistry007.azurecr.io/seabreeze/azure-mesh-helloworld:1.1-alpine`

### <a name="list-container-images"></a>Containerinstallatiekopieën opvragen

Het volgende voorbeeld worden de opslagplaatsen in een register. De voorbeelden wordt ervan uitgegaan dat u gebruikt de azure-NET-helloworld:1.1-alpine installatiekopie. Als u een andere installatiekopie gebruikt, vervang de naam van waar de installatiekopie van het azure-mesh-helloworld wordt gebruikt.

```azurecli
az acr repository list --name <acrName> --output table
```
Bijvoorbeeld: `az acr repository list --name myContainerRegistry007 --output table`

Uitvoer:

```bash
Result
-------------------------------
seabreeze/azure-mesh-helloworld
```

Het volgende voorbeeld worden de tags in de **azure-mesh-helloworld** opslagplaats.

```azurecli
az acr repository show-tags --name <acrName> --repository seabreeze/azure-mesh-helloworld --output table
```

Bijvoorbeeld: `az acr repository show-tags --name myContainerRegistry007 --repository seabreeze/azure-mesh-helloworld --output table`

Uitvoer:

```bash
Result
--------
1.1-alpine
```

De bovenstaande uitvoer bevestigt de aanwezigheid van `azure-mesh-helloworld:1.1-alpine` in het persoonlijke container registry.

## <a name="retrieve-credentials-for-the-registry"></a>Referenties voor het register worden opgehaald

> [!IMPORTANT]
> Inschakelen van de gebruiker met beheerdersrechten op een Azure container registry wordt niet aanbevolen voor productiescenario's. Dit wordt hier gedaan om deze demonstratie kort te houden. Voor productiescenario's gebruiken een [service-principal](https://docs.microsoft.com/azure/container-registry/container-registry-auth-service-principal) voor zowel gebruikers- en -verificatie in productiescenario's.

Als u wilt implementeren op een containerexemplaar uit het register die is gemaakt, moet u referenties opgeven tijdens de implementatie. Schakel de gebruiker met beheerdersrechten op uw register met de volgende opdracht uit:

```azurecli-interactive
az acr update --name <acrName> --admin-enabled true
```

Bijvoorbeeld: `az acr update --name myContainerRegistry007 --admin-enabled true`

Haal de naam van de registry, gebruikersnaam en wachtwoord met behulp van de volgende opdrachten:

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
az acr credential show --name <acrName> --query username
az acr credential show --name <acrName> --query "passwords[0].value"
```

De waarden geleverd door de voorgaande opdrachten wordt verwezen als `<acrLoginServer>`, `<acrUserName>`, en `<acrPassword>` hieronder.

## <a name="deploy-the-template"></a>De sjabloon implementeren

Maken van de toepassing en de bijbehorende resources met behulp van de volgende opdracht uit en geef de referenties van de vorige stap.

De `registry-password` parameter in de sjabloon is een beveiligde tekenreeks. Deze worden niet weergegeven in de status van de implementatie en `az mesh service show` opdrachten.

In een Bash-console voert u de volgende opdracht uit:

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.private_registry.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}, \"registry-server\": {\"value\": \"<acrLoginServer>\"}, \"registry-username\": {\"value\": \"<acrUserName>\"}, \"registry-password\": {\"value\": \"<acrPassword>\"}}"
```

In een PowerShell-console voert u de volgende opdracht uit:

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.private_registry.linux.json --parameters "{'location': {'value': 'eastus'}, 'registry-server': {'value': '<acrLoginServer>'}, 'registry-username': {'value': '<acrUserName>'}, 'registry-password': {'value': '<acrPassword>'}}"
```

In enkele minuten ziet u het:

`helloWorldPrivateRegistryApp has been deployed successfully on helloWorldPrivateRegistryNetwork with public ip address <IP Address>`

## <a name="open-the-application"></a>De toepassing openen

Zodra de toepassing met succes is geïmplementeerd, het openbare IP-adres voor het service-eindpunt ophalen en openen op een browser. Er wordt een webpagina weergegeven met Service Fabric-NET-logo.

De implementatieopdracht retourneert het openbare IP-adres van het service-eindpunt. U kunt eventueel ook de netwerkbron om het openbare IP-adres van het service-eindpunt opvragen. 
 
De naam van de netwerk-resource voor deze toepassing is `helloWorldPrivateRegistryNetwork`, informatie over met behulp van de volgende opdracht ophalen. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name helloWorldPrivateRegistryNetwork
```

## <a name="delete-the-resources"></a>De resources verwijderen

Vaak verwijdert de resources die u niet langer in Azure gebruikt. Als u wilt verwijderen van de bronnen die betrekking hebben op dit voorbeeld, verwijdert u de resourcegroep waarin ze zijn geïmplementeerd (Hierdoor worden ook verwijderd alles dat is gekoppeld aan de resourcegroep) met de volgende opdracht:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

Als u bent overgeschakeld naar Docker voor het werken met Linux-installatiekopieën die voor deze oefening en gaat terug naar het werken met Windows-installatiekopieën, met de rechtermuisknop op de docker-pictogram en selecteer **overschakelen naar de Windows-containers**

## <a name="next-steps"></a>Volgende stappen

- Weergeven van de toepassing Hello World-voorbeeld op [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld).
- Zie voor meer informatie over Service Fabric-Resourcemodel [Resourcemodel voor Service Fabric-NET](service-fabric-mesh-service-fabric-resources.md).
- Lees voor meer informatie over Service Fabric NET de [overzicht Service Fabric NET](service-fabric-mesh-overview.md).

[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/