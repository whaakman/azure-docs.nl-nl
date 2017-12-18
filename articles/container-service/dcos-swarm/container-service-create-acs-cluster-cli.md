---
title: Een Docker-containercluster implementeren - Azure CLI
description: Een Kubernetes-, DC/OS-, of Docker Swarm-oplossing implementeren in Azure Container Service met behulp van de Azure CLI 2.0
services: container-service
author: sauryadas
manager: timlt
ms.service: container-service
ms.topic: quickstart
ms.date: 03/01/2017
ms.author: saudas
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 40d5ea0e7abce165659219db8842ab64ac75fda7
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="deploy-a-docker-container-hosting-solution-using-the-azure-cli-20"></a>Een Docker-containerhostingoplossing implementeren met behulp van de Azure CLI 2.0

Gebruik de `az acs`-opdrachten in Azure CLI 2.0 om clusters te maken en te beheren in Azure Container Service. U kunt ook een Azure Container Service-cluster implementeren met behulp van [Azure Portal](container-service-deployment.md) of de Azure Container Service-API’s.

Voor meer informatie over `az acs`-opdrachten, gebruikt u de parameter `-h` voor een opdracht. Bijvoorbeeld: `az acs create -h`.



## <a name="prerequisites"></a>Vereisten
Als u een Azure Container Service-cluster wilt maken met Azure CLI 2.0, moet u:
* beschikken over een Azure-account ([krijg een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/))
* [Azure CLI 2.0](/cli/azure/install-az-cli2) hebben geïnstalleerd en ingesteld

## <a name="get-started"></a>Aan de slag 
### <a name="log-in-to-your-account"></a>Meld u aan bij uw account
```azurecli
az login 
```

Volg de aanwijzingen om u interactief aan te melden. Raadpleeg [Aan de slag met Azure CLI 2.0](/cli/azure/get-started-with-az-cli2) voor informatie over andere aanmeldingsmethoden.

### <a name="set-your-azure-subscription"></a>Uw Azure-abonnement instellen

Als u meer dan één Azure-abonnement hebt, stelt u een standaardabonnement in. Bijvoorbeeld:

```
az account set --subscription "f66xxxxx-xxxx-xxxx-xxx-zgxxxx33cha5"
```


### <a name="create-a-resource-group"></a>Een resourcegroep maken
Het wordt aangeraden om een resourcegroep te maken voor elk cluster. Geef een Azure-regio op waarin Azure Container Service [beschikbaar](https://azure.microsoft.com/en-us/regions/services/) is. Bijvoorbeeld:

```azurecli
az group create -n acsrg1 -l "westus"
```
De uitvoer lijkt op het volgende:

![Een resourcegroep maken](./media/container-service-create-acs-cluster-cli/rg-create.png)


## <a name="create-an-azure-container-service-cluster"></a>Een Azure Container Service-cluster maken

Gebruik `az acs create` om een cluster te maken.
Een naam voor het cluster en de naam van de resourcegroep die is gemaakt in de vorige stap zijn verplichte parameters. 

Andere invoer is ingesteld op standaardwaarden (zie het volgende scherm), tenzij deze wordt overschreven met behulp van de respectieve switches. De orchestrator is bijvoorbeeld standaard ingesteld op DC/OS. Als u er geen opgeeft, wordt een DNS-naamvoorvoegsel gemaakt op basis van de clusternaam.

![Azure ACS gebruiken en maken](./media/container-service-create-acs-cluster-cli/create-help.png)


### <a name="quick-acs-create-using-defaults"></a>Snel `acs create` met standaardinstellingen
Als u een openbaar SSH RSA-sleutelbestand (`id_rsa.pub`) hebt op de standaardlocatie (of als u er een hebt gemaakt voor [OS X en Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) of [Windows](../../virtual-machines/linux/ssh-from-windows.md)), gebruikt u een opdracht als de volgende:

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789
```
Als u geen openbare SSH-sleutel hebt, gebruikt u deze tweede opdracht. Met deze opdracht met de `--generate-ssh-keys`-switch wordt er een voor u gemaakt.

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789 --generate-ssh-keys
```

Wanneer u de opdracht hebt ingevoerd, duurt het ongeveer 10 minuten voor het cluster is gemaakt. De uitvoer van de opdracht bevat de volledig gekwalificeerde domeinnamen (FQDN’s) van de hoofd- en agentknooppunten en een SSH-opdracht om verbinding te maken met het eerste hoofdknooppunt. Hier volgt de verkorte uitvoer:

![Afbeelding van ACS maken](./media/container-service-create-acs-cluster-cli/cluster-create.png)

> [!TIP]
> In de [Kubernetes-walkthrough](../kubernetes/container-service-kubernetes-walkthrough.md) ziet u hoe u `az acs create` gebruikt met standaardwaarden om een Kubernetes-cluster te maken.
>

## <a name="manage-acs-clusters"></a>ACS-clusters beheren

Gebruik aanvullende `az acs`-opdrachten voor het beheren van uw cluster. Hier volgen enkele voorbeelden.

### <a name="list-clusters-under-a-subscription"></a>Clusters groeperen onder een abonnement

```azurecli
az acs list --output table
```

### <a name="list-clusters-in-a-resource-group"></a>Clusters groeperen onder een resourcegroep

```azurecli
az acs list -g acsrg1 --output table
```

![ACS-lijst](./media/container-service-create-acs-cluster-cli/acs-list.png)


### <a name="display-details-of-a-container-service-cluster"></a>Details van een Container Service-cluster weergeven

```azurecli
az acs show -g acsrg1 -n acs-cluster --output list
```

![ACS weergeven](./media/container-service-create-acs-cluster-cli/acs-show.png)


### <a name="scale-the-cluster"></a>Het cluster schalen
Zowel het opschalen als het uitschalen van agentknooppunten is toegestaan. De parameter `new-agent-count` is het nieuwe aantal agents in het ACS-cluster.

```azurecli
az acs scale -g acsrg1 -n acs-cluster --new-agent-count 4
```

![ACS-schaal](./media/container-service-create-acs-cluster-cli/acs-scale.png)

## <a name="delete-a-container-service-cluster"></a>Een Container Service-cluster verwijderen
```azurecli
az acs delete -g acsrg1 -n acs-cluster 
```
Met deze verwijderopdracht verwijdert u niet alle resources (netwerk en opslag) die zijn gemaakt tijdens het maken van de Container Service. Als u alle resources gemakkelijk wilt verwijderen, wordt het aangeraden om elk cluster in een afzonderlijke resourcegroep te implementeren. Verwijder vervolgens de resourcegroep wanneer het cluster niet langer nodig is.

## <a name="next-steps"></a>Volgende stappen
Nu u een werkend cluster hebt, kunt u deze documenten lezen voor meer informatie over verbinding en beheer:

* [Verbinding maken met een Azure Container Service-cluster](../container-service-connect.md)
* [Werken met de Azure Container Service en DC/OS](container-service-mesos-marathon-rest.md)
* [Werken met de Azure Container Service en Docker Swarm](container-service-docker-swarm.md)
* [Werken met de Azure Container Service en Kubernetes](../kubernetes/container-service-kubernetes-walkthrough.md)