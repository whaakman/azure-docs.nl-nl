---
title: Snelstartgids - Build en voer een containerinstallatiekopie in Azure Container Registry
description: Taken snel uitvoeren met Azure Container Registry wilt bouwen en uitvoeren van een container-installatiekopie op aanvraag, in de cloud.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 04/02/2019
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: 61a17842158326c927f049af893a00818f3acc55
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58919691"
---
# <a name="quickstart-build-and-run-a-container-image-using-azure-container-registry-tasks"></a>Quickstart: Bouwen en uitvoeren van een containerinstallatiekopie met behulp van Azure Container Registry-taken

In deze Quick Start, kunt u Azure Container Registry taken opdrachten gebruiken om snel te bouwen, pushen, en uitvoeren van een Docker-containerinstallatiekopie systeemeigen in Azure, waarin wordt getoond hoe voor de offload van de 'inner loop' ontwikkelingscyclus naar de cloud. [ACR taken] [ container-registry-tasks-overview] is een suite met functies in Azure Container Registry om te beheren en wijzigen van containerinstallatiekopieën in de levenscyclus van de container. 

Verken meer geavanceerde functies van de ACR-taken na deze Quick Start. ACR-taken kunt compileren van installatiekopieën op basis van code doorvoeringen of basisinstallatiekopie updates automatiseren, of test van meerdere containers, parallel, onder andere scenario's. 

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account][azure-account] voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om deze Quick Start uit te voeren. Als u wilt gebruiken op het bestand lokaal op, versie 2.0.58 of later wordt aanbevolen. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Als u nog een containerregister hebt, maakt u eerst een resourcegroep met de [az-groep maken] [ az-group-create] opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Een containerregister maken

Maak een container registry met de [az acr maken] [ az-acr-create] opdracht. De registernaam moet uniek zijn binnen Azure en mag 5 tot 50 alfanumerieke tekens bevatten. In het volgende voorbeeld *myContainerRegistry008* wordt gebruikt. Werk deze waarde bij naar een unieke waarde.

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry008 --sku Basic
```

Dit voorbeeld maakt u een *Basic* register, een optie kosten geoptimaliseerd voor meer informatie over Azure Container Registry-ontwikkelaars. Zie [Azure Container Registry-SKU's][container-registry-skus] voor meer informatie over de beschikbare servicelagen.

## <a name="build-an-image-from-a-dockerfile"></a>Bouw een installatiekopie van een docker-bestand

Azure Container Registry nu gebruiken om een installatiekopie te bouwen. Eerst maakt u een werkmap en maak vervolgens een docker-bestand met de naam *Dockerfile* met de volgende inhoud. Dit is een eenvoudig voorbeeld een Linux-containerinstallatiekopie bouwen, maar u kunt maken van uw eigen standard Dockerfile en maken van installatiekopieën voor andere platforms.

```bash
echo "FROM hello-world" > Dockerfile
```

Voer de [az acr build] [ az-acr-build] opdracht om de installatiekopie te bouwen. Wanneer is gemaakt, wordt de installatiekopie naar het register gepusht. Het volgende voorbeeld worden de `sample/hello-world:v1` installatiekopie. De `.` aan het einde van de opdracht stelt de locatie van de docker-bestand, in dit geval de huidige map.

```azurecli-interactive
az acr build --image sample/hello-world:v1 --registry myContainerRegistry008 --file Dockerfile . 
```

Uitvoer van een geslaagde build- en push is vergelijkbaar met het volgende:

```console
Packing source code into tar to upload...
Uploading archived source code from '/tmp/build_archive_b0bc1e5d361b44f0833xxxx41b78c24e.tar.gz'...
Sending context (1.856 KiB) to registry: mycontainerregistry008...
Queued a build with ID: ca8
Waiting for agent...
2019/03/18 21:56:57 Using acb_vol_4c7ffa31-c862-4be3-xxxx-ab8e615c55c4 as the home volume
2019/03/18 21:56:57 Setting up Docker configuration...
2019/03/18 21:56:58 Successfully set up Docker configuration
2019/03/18 21:56:58 Logging in to registry: mycontainerregistry008.azurecr.io
2019/03/18 21:56:59 Successfully logged into mycontainerregistry008.azurecr.io
2019/03/18 21:56:59 Executing step ID: build. Working directory: '', Network: ''
2019/03/18 21:56:59 Obtaining source code and scanning for dependencies...
2019/03/18 21:57:00 Successfully obtained source code and scanned for dependencies
2019/03/18 21:57:00 Launching container with name: build
Sending build context to Docker daemon  13.82kB
Step 1/1 : FROM hello-world
latest: Pulling from library/hello-world
Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586fxxxx21577a99efb77324b0fe535
Successfully built fce289e99eb9
Successfully tagged mycontainerregistry008.azurecr.io/sample/hello-world:v1
2019/03/18 21:57:01 Successfully executed container: build
2019/03/18 21:57:01 Executing step ID: push. Working directory: '', Network: ''
2019/03/18 21:57:01 Pushing image: mycontainerregistry008.azurecr.io/sample/hello-world:v1, attempt 1
The push refers to repository [mycontainerregistry008.azurecr.io/sample/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Layer already exists
v1: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/03/18 21:57:03 Successfully pushed image: mycontainerregistry008.azurecr.io/sample/hello-world:v1
2019/03/18 21:57:03 Step ID: build marked as successful (elapsed time in seconds: 2.543040)
2019/03/18 21:57:03 Populating digests for step ID: build...
2019/03/18 21:57:05 Successfully populated digests for step ID: build
2019/03/18 21:57:05 Step ID: push marked as successful (elapsed time in seconds: 1.473581)
2019/03/18 21:57:05 The following dependencies were found:
2019/03/18 21:57:05
- image:
    registry: mycontainerregistry008.azurecr.io
    repository: sample/hello-world
    tag: v1
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: v1
    digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
  git: {}

Run ID: ca8 was successful after 10s
```

## <a name="run-the-image"></a>De installatiekopie uitvoeren

Nu Voer snel de installatiekopie die u gemaakt en naar het register gepusht. In uw ontwikkelingswerkstroom met containers, kan dit een validatiestap zijn voordat u de installatiekopie implementeert.

Maak een bestand *quickrun.yaml* in een lokale werkmap met de volgende inhoud voor één stap. Vervang de naam van de aanmeldingsserver van het register voor  *\<acrLoginServer\>*. Naam van de aanmeldingsserver is in de indeling  *\<registernaam\>. azurecr.io* (zonder hoofdletters), bijvoorbeeld *mycontainerregistry008.azurecr.io*. In dit voorbeeld wordt ervan uitgegaan dat u gemaakt en gepusht de `sample/hello-world:v1` installatiekopie in de vorige sectie:

```yml
steps:
  - cmd: <acrLoginServer>/sample/hello-world:v1
```

De `cmd` stap in dit voorbeeld kan de container in de standaardconfiguratie wordt uitgevoerd, maar `cmd` ondersteunt aanvullende `docker run` parameters of zelfs andere `docker` opdrachten.

Voer de container met de volgende opdracht uit:

```azurecli-interactive
az acr run --registry myContainerRegistry008 --file quickrun.yaml .
```

De uitvoer lijkt op het volgende:

```console
Packing source code into tar to upload...
Uploading archived source code from '/tmp/run_archive_ebf74da7fcb04683867b129e2ccad5e1.tar.gz'...
Sending context (1.855 KiB) to registry: mycontainerre...
Queued a run with ID: cab
Waiting for an agent...
2019/03/19 19:01:53 Using acb_vol_60e9a538-b466-475f-9565-80c5b93eaa15 as the home volume
2019/03/19 19:01:53 Creating Docker network: acb_default_network, driver: 'bridge'
2019/03/19 19:01:53 Successfully set up Docker network: acb_default_network
2019/03/19 19:01:53 Setting up Docker configuration...
2019/03/19 19:01:54 Successfully set up Docker configuration
2019/03/19 19:01:54 Logging in to registry: mycontainerregistry008.azurecr.io
2019/03/19 19:01:55 Successfully logged into mycontainerregistry008.azurecr.io
2019/03/19 19:01:55 Executing step ID: acb_step_0. Working directory: '', Network: 'acb_default_network'
2019/03/19 19:01:55 Launching container with name: acb_step_0

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

2019/03/19 19:01:56 Successfully executed container: acb_step_0
2019/03/19 19:01:56 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 0.843801)

Run ID: cab was successful after 6s
```

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete][az-group-delete] gebruiken om de resourcegroep, het containerregister en de daar opgeslagen containerinstallatiekopieën te verwijderen wanneer u ze niet meer nodig hebt.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids gebruikt u functies van de ACR-taken snel bouwen, het push-en het uitvoeren van een Docker-containerinstallatiekopie systeemeigen binnen Azure. Doorgaan met de Azure Container Registry-zelfstudies voor meer informatie over het ACR-taken gebruiken voor het compileren van installatiekopieën en updates automatiseren.

> [!div class="nextstepaction"]
> [Azure Container Registry-zelfstudies][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[azure-account]: https://azure.microsoft.com/free/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli]: /cli/azure/install-azure-cli
[container-registry-tasks-overview]: container-registry-tasks-overview.md
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
[azure-cli-install]: /cli/azure/install-azure-cli
