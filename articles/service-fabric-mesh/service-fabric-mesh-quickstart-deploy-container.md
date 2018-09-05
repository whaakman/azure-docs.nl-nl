---
title: Snelstart - Hallo wereld implementeren in Azure Service Fabric Mesh | Microsoft Docs
description: In deze snelstart ziet u hoe u een Service Fabric Mesh-toepassing in Azure Service Fabric Mesh implementeert.
services: service-fabric-mesh
keywords: Voeg geen sleutelwoorden toe en bewerk ze niet zonder overleg met uw SEO-expert.
author: rwike77
ms.author: ryanwi
ms.date: 08/24/2018
ms.topic: quickstart
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: f5b834f92b2a126f68780a7647fda4d8b35dfe43
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886441"
---
# <a name="quickstart-deploy-hello-world-to-service-fabric-mesh"></a>Snelstart: Hallo wereld implementeren in Service Fabric Mesh

Met [Service Fabric Mesh](service-fabric-mesh-overview.md) kunt u eenvoudig microservicetoepassingen in Azure maken en beheren, zonder dat u daarvoor virtuele machines hoeft in te richten. In deze snelstart gaat u een Hallo wereld-toepassing maken in Azure en deze op internet beschikbaar maken. Deze bewerking wordt uitgevoerd in één opdracht. In slechts een paar minuten ziet u deze weergave in uw browser:

![Hello world-app in browser][sfm-app-browser]

[!INCLUDE [preview note](./includes/include-preview-note.md)]

Als u nog geen Azure-account hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI instellen 
U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om deze Quick Start uit te voeren. Installeer de Azure Service Fabric Mesh CLI-uitbreidingsmodule door de volgende [instructies](service-fabric-mesh-howto-setup-cli.md) te volgen.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Meld u aan bij Azure en stel uw abonnement in.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-resource-group"></a>Een resourcegroep maken
Maak een resourcegroep waarin u de toepassing wilt implementeren. U kunt een bestaande resourcegroep gebruiken en deze stap overslaan. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application"></a>De toepassing implementeren
Maak uw toepassing in de resourcegroep met de opdracht `az mesh deployment create`.  In een Bash-console voert u de volgende opdracht uit:

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}" 
```

In een PowerShell-console voert u de volgende opdracht uit:

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.linux.json --parameters "{'location': {'value': 'eastus'}}"
```

Met de voorgaande opdracht wordt een Linux-toepassing geïmplementeerd met [mesh_rp.linux.json template](https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.linux.json). Gebruik [mesh_rp.windows.json template](https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.windows.json) als u een Windows-toepassing wilt implementeren. Windows-containerinstallatiekopieën zijn groter dan Linux-containerinstallatiekopieën en het kan langer duren om ze te implementeren.

Een paar minuten later retourneert de opdracht het volgende:

`helloWorldApp has been deployed successfully on helloWorldNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>De toepassing openen
Zodra de toepassing is geïmplementeerd, kopieert u het openbare IP-adres voor het service-eindpunt van de CLI-uitvoer. Open het IP-adres in een webbrowser. Een webpagina met het logo van Azure Service Fabric Mesh wordt weergegeven.

## <a name="check-the-application-details"></a>De toepassingsgegevens controleren
U kunt de status van de toepassing controleren met de opdracht `az mesh app show`. Deze opdracht retourneert nuttige informatie op basis waarvan u actie kunt ondernemen.

Voor deze snelstart is `helloWorldApp` de naam van de toepassing. Door de volgende opdracht uit te voeren kunt u gegevens over de toepassing verzamelen:

```azurecli-interactive
az mesh app show --resource-group myResourceGroup --name helloWorldApp
```

## <a name="see-the-application-logs"></a>De toepassingslogboeken bekijken

Bekijk de logboeken voor de geïmplementeerde toepassing via de opdracht `az mesh code-package-log get`:

```azurecli-interactive
az mesh code-package-log get --resource-group myResourceGroup --application-name helloWorldApp --service-name helloWorldService --replica-name 0 --code-package-name helloWorldCode
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u zover bent dat u de toepassing wilt verwijderen, voert u de opdracht [az group delete][az-group-delete] uit om de resourcegroep evenals de toepassings- en netwerkresources te verwijderen die er deel van uitmaken.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het maken en implementeren van Service Fabric Mesh-toepassingen, gaat u naar de zelfstudie.
> [!div class="nextstepaction"]
> [Een webtoepassing met meerdere services maken en implementeren](service-fabric-mesh-tutorial-create-dotnetcore.md)

<!-- Images -->
[sfm-app-browser]: ./media/service-fabric-mesh-quickstart-deploy-container/HelloWorld.png

<!-- Links / Internal -->
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
