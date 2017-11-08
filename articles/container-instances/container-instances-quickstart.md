---
title: Quick Start - uw eerste Azure-Containerexemplaren container maken
description: Azure Container Instances implementeren en ermee aan de slag gaan
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: dc8a94e998b36331a6a42253a68b43d76be6657c
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Uw eerste container maken in Azure Container Instances
Azure Containerexemplaren gemakkelijk maken en beheren van Docker-containers in Azure, zonder te hoeven inrichten van virtuele machines of aannemen van een service op een hoger niveau. In deze snelstartgids een container maken in Azure en tijdens het blootgesteld aan internet met een openbaar IP-adres. Deze bewerking wordt uitgevoerd in één opdracht. Binnen een paar seconden ziet u dit in uw browser:

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-app-browser]

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

U kunt de Azure-Cloud-Shell of een lokale installatie van de Azure CLI gebruiken voor het voltooien van deze snelstartgids. Als u wilt installeren en gebruiken van de CLI lokaal, is deze snelstartgids vereist dat u de Azure CLI versie 2.0.20 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Azure Container Instances zijn Azure-resources en moeten worden geplaatst in een Azure-resourcegroep, een logische verzameling waarin Azure resources worden geïmplementeerd en beheerd.

Maak een resourcegroep met de [az groep maken] [ az-group-create] opdracht.

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Een container maken

U kunt een container maken door op te geven van een naam, een Docker-installatiekopie en een Azure-resourcegroep voor de [az container maken] [ az-container-create] opdracht. U kunt de container desgewenst weergeven op internet met een openbaar IP-adres. In deze snelstartgids die u implementeert een container die als host fungeert voor een kleine web-app die is geschreven in [Node.js](http://nodejs.org).

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public
```

Binnen enkele seconden krijgt u een reactie op uw aanvraag. In eerste instantie is de container is in de **maken** staat, maar moet worden gestart binnen een paar seconden. U kunt ook het gebruik van de status controleren de [az container weergeven] [ az-container-show] opdracht:

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

Aan de onderkant van de uitvoer ziet u de inrichtingsstatus en het IP-adres van de container:

```json
...
"ipAddress": {
      "ip": "13.88.176.27",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

Nadat de container wordt verplaatst naar de **geslaagd** staat, kunt u deze bereiken in uw browser met behulp van het opgegeven IP-adres.

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-app-browser]

## <a name="pull-the-container-logs"></a>De containerlogboeken ophalen

U kunt ophalen van de logboeken voor de container die u hebt gemaakt met de [az container logboeken] [ az-container-logs] opdracht:

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

Uitvoer:

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-the-container"></a>De container verwijderen

Wanneer u klaar bent met de container, kunt u verwijderen met behulp van de [az container verwijderen] [ az-container-delete] opdracht:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

Uitvoeren om te bevestigen dat de container is verwijderd, de [az container lijst](/cli/azure/container#az_container_list) opdracht:

```azurecli-interactive
az container list --resource-group myResourceGroup -o table
```

De **mycontainer** container moet niet worden weergegeven in de uitvoer van de opdracht. Als u geen andere containers in de resourcegroep hebt, wordt geen uitvoer wordt weergegeven.

## <a name="next-steps"></a>Volgende stappen

Alle code voor de container die wordt gebruikt in deze snelstartgids is beschikbaar [op GitHub][app-github-repo], samen met de Dockerfile. Als u wilt proberen om deze zelf te bouwen en te implementeren in Azure Container Instances met behulp van het Azure Container Registry, gaat u verder met de zelfstudie van Azure Container Instances.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Container Instances](./container-instances-tutorial-prepare-app.md)

Als u wilt uitproberen opties voor het uitvoeren van containers in een systeem orchestration op Azure, Zie de [Service Fabric] [ service-fabric] of [Azure Container Service (AKS)] [ container-service] snelstartgidsen.

<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[az-group-create]: /cli/azure/group?view=azure-cli-latest#az_group_create
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-delete]: /cli/azure/container?view=azure-cli-latest#az_container_delete
[az-container-list]: /cli/azure/container?view=azure-cli-latest#az_container_list
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
[container-service]: ../aks/kubernetes-walkthrough.md


<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
