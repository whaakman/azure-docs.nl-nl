---
title: 'Quick Start: uw eerste Azure Container Instances-container maken'
description: In deze snelstart gebruikt u de Azure-CLI om een container in Azure Container Instances te implementeren
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: quickstart
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: b68468cd8174d658d04d8e67433a8f18884493bd
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="quickstart-create-your-first-container-in-azure-container-instances"></a>Snelstart: Uw eerste container maken in Azure Container Instances

Met Azure Container Instances kunt u gemakkelijk Docker-containers in Azure maken en beheren, zonder virtuele machines te hoeven inrichten of een service op een hoger niveau te moeten gebruiken. In deze quickstart maakt u een container in Azure en publiceert u deze op internet via een FQDN (Fully Qualified Domain Name). Deze bewerking wordt uitgevoerd in één opdracht. Binnen een paar seconden ziet u dit in uw browser:

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-app-browser]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account][azure-account] voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om deze Quick Start uit te voeren. Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de Azure CLI versie 2.0.27 of later uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Azure Container Instances moeten, zoals alle Azure-resources, in een resourcegroep worden geplaatst, een logische verzameling waarin Azure-resources worden geïmplementeerd en beheerd.

Een resourcegroep maken met de opdracht [az group create][az-group-create].

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Een container maken

U kunt een container maken door een naam, een Docker-installatiekopie en een Azure-resourcegroep op te geven bij de opdracht [az container create][az-container-create]. U kunt de container desgewenst weergeven op internet door een DNS-naamlabel op te geven. In deze quickstart implementeert u een container die als host fungeert voor een kleine web-app die is geschreven in [Node.js][node-js].

Voer de volgende opdracht uit om een exemplaar van de container te starten. De waarde `--dns-name-label` moet uniek zijn binnen de Azure-regio waar u het exemplaar maakt, dus u zult deze waarde mogelijk moeten wijzigen om ervoor te zorgen dat deze uniek is.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --dns-name-label aci-demo --ports 80
```

Binnen enkele seconden krijgt u een reactie op uw aanvraag. In eerste instantie heeft de container nog de status **Maken**, maar de container moet binnen een paar seconden starten. U kunt de status controleren met de opdracht [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

Als u de opdracht uitvoert, worden de FQDN (Fully Qualified Domain Name) en de inrichtingsstatus weergegeven:

```console
$ az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
FQDN                               ProvisioningState
---------------------------------  -------------------
aci-demo.eastus.azurecontainer.io  Succeeded
```

Als de container de status **Voltooid** heeft, gaat u naar de FQDN-naam in de browser:

![Schermafbeelding van browser met toepassing die wordt uitgevoerd in een exemplaar van een Azure-container][aci-app-browser]

## <a name="pull-the-container-logs"></a>De containerlogboeken ophalen

Het weergeven van de logboeken voor een exemplaar van de container is handig bij het oplossen van problemen met de container of de toepassing die wordt uitgevoerd.

Haal de logboeken van de container op met de opdracht [az container logs][az-container-logs]:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

De uitvoer geeft de logboeken voor de container weer, evenals de HTTP GET-aanvragen die worden gegenereerd wanneer u de toepassing in uw browser weergeeft.

```console
$ az container logs --resource-group myResourceGroup -n mycontainer
listening on port 80
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
```

## <a name="attach-output-streams"></a>Uitvoerstromen koppelen

Naast het toevoegen van de logboeken, kunt u uw lokale standaarduitvoer- en standaardfoutstromen koppelen aan die van de container.

Voer eerst de opdracht [az container attach][az-container-attach] uit om uw lokale console aan de uitvoerstromen van de container te koppelen:

```azurecli-interactive
az container attach --resource-group myResourceGroup -n mycontainer
```

Vernieuw de browser na het koppelen een paar keer om wat extra uitvoer te genereren. Ontkoppel ten slotte de console met `Control+C`. De uitvoer ziet er als volgt uit:

```console
$ az container attach --resource-group myResourceGroup -n mycontainer
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-03-15 21:17:59+00:00) pulling image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Successfully pulled image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Created container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45
(count: 1) (last timestamp: 2018-03-15 21:18:06+00:00) Started container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45

Start streaming logs:
listening on port 80
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:44 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:47 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met de container, verwijdert u deze met de opdracht [az container delete][az-container-delete]:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Om te controleren of de container is verwijderd, voert u de opdracht [az container list](/cli/azure/container#az_container_list) uit:

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

De container **mycontainer** mag dan niet worden weergegeven in de uitvoer van de opdracht. Als u geen andere containers in de resourcegroep hebt, wordt er geen uitvoer weergegeven.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een Azure-containerinstantie van een installatiekopie gemaakt in een openbaar Docker Hub-regiser. Als u zelf een containerinstallatiekopie wilt bouwen en deze wilt implementeren in Azure Container Instances met behulp van privé Azure Container Registry, gaat u verder met de zelfstudie voor Azure Container Instances.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Container Instances](./container-instances-tutorial-prepare-app.md)

Zie de snelstart [Service Fabric][service-fabric] of [Azure Kubernetes Service (AKS)][container-service] als u opties wilt uitproberen voor het uitvoeren van containers in een orchestration-systeem op Azure.

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/
[node-js]: http://nodejs.org

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az_container_attach
[az-container-create]: /cli/azure/container#az_container_create
[az-container-delete]: /cli/azure/container#az_container_delete
[az-container-list]: /cli/azure/container#az_container_list
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show
[az-group-create]: /cli/azure/group#az_group_create
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
