---
title: 'Snelstartgids: Azure Docker Swarm-cluster voor Linux | Microsoft Docs'
description: Leer snel hoe u een Docker Swarm-cluster voor Linux-containers in Azure Container Service maakt met de Azure CLI.
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, Docker, Swarm
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/14/2017
ms.author: nepeters
ms.custom: 
ms.openlocfilehash: 876135d62d548e155f4ebefd8bbd9d9cca8b87d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-docker-swarm-cluster"></a>Docker Swarm-cluster implementeren

In deze snelstartgids wordt een Docker Swarm-cluster geïmplementeerd met behulp van de Azure CLI. Vervolgens wordt er een toepassing met meerdere containers die bestaat uit een web-front-end en een Redis-exemplaar, geïmplementeerd en uitgevoerd op het cluster. Zodra de toepassing is voltooid, is deze toegankelijk via internet.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor deze snelstartgids moet u de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische groep waarin Azure-resources worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *westus*.

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

Uitvoer:

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "westcentralus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-docker-swarm-cluster"></a>Docker Swarm-cluster maken

Maak een Dock Swarm-cluster in Azure Container Service met de opdracht [az acs create](/cli/azure/acs#create). 

In het volgende voorbeeld wordt een cluster gemaakt met de naam *mySwarmCluster* met een Linux-hoofdknooppunt en drie knooppunten van de Linux-agent.

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type Swarm --resource-group myResourceGroup --generate-ssh-keys
```

In sommige gevallen, zoals met een beperkte proefversie, heeft een Azure-abonnement beperkte toegang tot Azure-resources. Als de implementatie mislukt vanwege beperkte beschikbare kernen, verminder dan het aantal standaardagenten door `--agent-count 1` toe te voegen aan de opdracht [az acs create](/cli/azure/acs#create). 

Na enkele minuten is de opdracht voltooid en retourneert deze informatie over het cluster in json-indeling.

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

In deze snelstartgids hebt u het IP-adres nodig van zowel de Docker Swarm-master als de Docker-agentpool. Voer de volgende opdracht uit om beide IP-adressen te retourneren.


```bash
az network public-ip list --resource-group myResourceGroup --query "[*].{Name:name,IPAddress:ipAddress}" -o table
```

Uitvoer:

```bash
Name                                                                 IPAddress
-------------------------------------------------------------------  -------------
swarmm-agent-ip-myswarmcluster-myresourcegroup-d5b9d4agent-66066781  52.179.23.131
swarmm-master-ip-myswarmcluster-myresourcegroup-d5b9d4mgmt-66066781  52.141.37.199
```

Maak een SSH-tunnel naar de Swarm-master. Vervang `IPAddress` door het IP-adres van de Swarm-master.

```bash
ssh -p 2200 -fNL 2375:localhost:2375 azureuser@IPAddress
```

Stel de omgevingsvariabele `DOCKER_HOST` in. Hierdoor kunt u Docker-opdrachten voor de Docker-swarm uitvoeren zonder dat u de hostnaam hoeft op te geven.

```bash
export DOCKER_HOST=:2375
```

U bent nu klaar om Docker-services uit te voeren op de Docker-swarm.


## <a name="run-the-application"></a>De toepassing uitvoeren

Maak een bestand met de naam `docker-compose.yaml` en kopieer de volgende inhoud in het bestand.

```yaml
version: '3'
services:
  azure-vote-back:
    image: redis
    container_name: azure-vote-back
    ports:
        - "6379:6379"

  azure-vote-front:
    image: microsoft/azure-vote-front:redis-v1
    container_name: azure-vote-front
    environment:
      REDIS: azure-vote-back
    ports:
        - "80:80"
```

Voer de volgende opdracht uit om de Azure Vote-service te maken.

```bash
docker-compose up -d
```

Uitvoer:

```bash
Creating network "user_default" with the default driver
Pulling azure-vote-front (microsoft/azure-vote-front:redis-v1)...
swarm-agent-EE873B23000005: Pulling microsoft/azure-vote-front:redis-v1...
swarm-agent-EE873B23000004: Pulling microsoft/azure-vote-front:redis-v1... : downloaded
Pulling azure-vote-back (redis:latest)...
swarm-agent-EE873B23000004: Pulling redis:latest... : downloaded
Creating azure-vote-front ... 
Creating azure-vote-back ... 
Creating azure-vote-front
Creating azure-vote-back ...
```

## <a name="test-the-application"></a>De toepassing testen

Browse naar het IP-adres van de Swarm-agentpool om de Azure Vote-toepassing te testen.

![Afbeelding van browsen naar Azure Vote](media/container-service-docker-swarm-mode-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a>Cluster verwijderen
U kunt de opdracht [az group delete](/cli/azure/group#delete) gebruiken om de resourcegroep, de containerservice en alle gerelateerde resources te verwijderen wanneer u het cluster niet meer nodig hebt.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-the-code"></a>Code ophalen

In deze snelstartgids zijn vooraf gemaakte containerinstallatiekopieën gebruikt om een Docker-service te maken. De gerelateerde toepassingscode, Dockerfile en het Opstellen-bestand zijn beschikbaar op GitHub.

[https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een Docker Swarm-cluster geïmplementeerd. Vervolgens hebt u een toepassing met meerdere containers op dit cluster geïmplementeerd.

Ga naar CI/CD met Docker Swarm en VSTS voor informatie over het integreren van Docker Swarm met Visual Studio Team Services.

> [!div class="nextstepaction"]
> [CI/CD met Docker Swarm en VSTS](./container-service-docker-swarm-setup-ci-cd.md)