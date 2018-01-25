---
title: 'Quick Start: uw eerste Azure Container Instances-container maken'
description: Azure Container Instances implementeren en ermee aan de slag gaan
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: quickstart
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 4c7f48c993d66dd79538fd73ccaed1355c2e8cdd
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Uw eerste container maken in Azure Container Instances
Met Azure Container Instances kunt u gemakkelijk Docker-containers in Azure maken en beheren, zonder virtuele machines te hoeven inrichten of een service op een hoger niveau te moeten gebruiken. In deze quickstart maakt u een container in Azure en geeft u deze op internet weer via een openbaar IP-adres. Deze bewerking wordt uitgevoerd in één opdracht. Binnen een paar seconden ziet u dit in uw browser:

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-app-browser]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account][azure-account] voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om deze Quick Start uit te voeren. Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze quickstart versie 2.0.21 of hoger van Azure CLI uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Azure Container Instances zijn Azure-resources en moeten worden geplaatst in een Azure-resourcegroep, een logische verzameling waarin Azure resources worden geïmplementeerd en beheerd.

Een resourcegroep maken met de opdracht [az group create][az-group-create].

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Een container maken

U kunt een container maken door een naam, een Docker-installatiekopie en een Azure-resourcegroep op te geven bij de opdracht [az container create][az-container-create]. U kunt de container desgewenst weergeven op internet met een openbaar IP-adres. In deze quickstart implementeert u een container die als host fungeert voor een kleine web-app die is geschreven in [Node.js][node-js].

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --ip-address public --ports 80
```

Binnen enkele seconden krijgt u een reactie op uw aanvraag. In eerste instantie heeft de container nog de status **Maken**, maar de container moet binnen een paar seconden starten. U kunt de status controleren met de opdracht [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
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
    "location:": "eastus",
    "name": "mycontainer",
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

Als de container de status **Voltooid** heeft, kunt u deze in uw browser openen met behulp van het opgegeven IP-adres.

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-app-browser]

## <a name="pull-the-container-logs"></a>De containerlogboeken ophalen

U kunt de logboeken voor de container die u hebt gemaakt ophalen met de opdracht [az container logs][az-container-logs]:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Uitvoer:

```bash
listening on port 80
::ffff:10.240.255.107 - - [29/Nov/2017:20:48:50 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36"
::ffff:10.240.255.107 - - [29/Nov/2017:20:48:50 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://52.224.178.107/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36"
```

## <a name="delete-the-container"></a>De container verwijderen

Wanneer u klaar bent met de container, kunt u deze verwijderen met behulp van de opdracht [az container delete][az-container-delete]:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Om te controleren of de container is verwijderd, voert u de opdracht [az container list](/cli/azure/container#az_container_list) uit:

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

De container **mycontainer** mag dan niet worden weergegeven in de uitvoer van de opdracht. Als u geen andere containers in de resourcegroep hebt, wordt er geen uitvoer weergegeven.

## <a name="next-steps"></a>Volgende stappen

Alle code voor de container die in deze Quick Start wordt gebruikt, is beschikbaar [op GitHub][app-github-repo], samen met het bijbehorende Dockerfile. Als u wilt proberen om deze zelf te bouwen en te implementeren in Azure Container Instances met behulp van het Azure Container Registry, gaat u verder met de zelfstudie van Azure Container Instances.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Container Instances](./container-instances-tutorial-prepare-app.md)

Zie de Quick Start [Service Fabric] [ service-fabric] of [Azure Container Service (AKS)] [ container-service] als u opties wilt uitproberen voor het uitvoeren van containers in een orchestration-systeem op Azure.

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[node-js]: http://nodejs.org

<!-- LINKS - Internal -->
[az-group-create]: /cli/azure/group?view=azure-cli-latest#az_group_create
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-delete]: /cli/azure/container?view=azure-cli-latest#az_container_delete
[az-container-list]: /cli/azure/container?view=azure-cli-latest#az_container_list
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
