---
title: Load balance containers in Azure DC/OS-cluster
description: Verdelen over meerdere containers in een Azure Container Service DC/OS-cluster.
services: container-service
author: rgardler
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 06/02/2017
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 6f5467d0fbcc577a548f1100ed6e4d380fe38759
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a>Load balance containers in een Azure Container Service DC/OS-cluster

In dit artikel wordt besproken voor het maken van een interne load balancer in een DC/OS beheerd-Azure Containerservice met behulp van Marathon-Taakverdeling. Deze configuratie kunt u uw toepassingen horizontaal schalen. Ook kunt u profiteren van de openbare en persoonlijke agent clusters door het plaatsen van uw netwerktaakverdelers op het openbare als uw toepassingscontainers op het persoonlijke cluster. In deze zelfstudie hebt u:

> [!div class="checklist"]
> * Een Load Balancer van Marathon configureren
> * Implementeer een toepassing met behulp van de load balancer
> * Configureren en de Azure load balancer

U moet een ACS-DC/OS-cluster de stappen in deze zelfstudie. Indien nodig, [dit voorbeeldscript](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) kunt maken voor u.

Voor deze zelfstudie is versie 2.0.4 of hoger van de Azure CLI vereist. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a>Overzicht van netwerktaakverdeling

Er zijn twee lagen voor taakverdeling in een Azure Container Service DC/OS-cluster: 

**Azure Load Balancer** biedt openbare invoerpunten (de waarden die eindgebruikers toegang). Een Azure LB automatisch wordt geleverd met Azure Container Service en is standaard geconfigureerd om poort 80 en 443 van 8080 weer te geven.

**De Marathon-taakverdeling (marathon-taakverdeling)** routes binnenkomende aanvragen naar containerexemplaren die aanvragen voor deze service. De marathon-taakverdeling wordt dynamisch aangepast geschaald de containers bieden onze webservice. Deze load balancer is niet opgegeven in uw Containerservice standaard, maar het is eenvoudig te installeren.

## <a name="configure-marathon-load-balancer"></a>Marathon-taakverdeling configureren

Marathon Load Balancer herconfigureert zichzelf dynamisch op basis van de containers die u hebt geïmplementeerd. Het is ook aan het verlies van een container of agent - als dit het geval is, Apache Mesos elders in de container opnieuw is opgestart en marathon-taakverdeling past zich.

Voer de volgende opdracht voor het installeren van de marathon-taakverdeling op de openbare agent-cluster.

```azurecli-interactive
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a>Taakverdeling toepassing implementeren

Nu we het Marathon-LB-pakket hebben, kunnen we een toepassingscontainer implementeren waarvan we de taken willen verdelen. 

De FQDN-naam van de agents die openbaar blootgestelde eerst worden opgehaald.

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

Maak vervolgens een bestand met de naam *hello web.json* en kopieer de volgende inhoud. De `HAPROXY_0_VHOST` label moet worden bijgewerkt met de FQDN van de DC/OS-agents. 

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}
```

Gebruik de DC/OS CLI de toepassing uit te voeren. Standaard Marathon implementeert het de toepassing naar de persoonlijke cluster. Dit betekent dat de bovenstaande implementatie alleen toegankelijk via de load balancer is, dit is meestal het gewenste gedrag.

```azurecli-interactive
dcos marathon app add hello-web.json
```

Zodra de toepassing is geïmplementeerd, blader naar de FQDN-naam van het cluster agent taakverdeling toepassing weergeven.

![Afbeelding van taakverdeling toepassing](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a>Azure Load Balancer configureren

Standaard stelt de Azure Load Balancer poorten 80, 8080 en 443 open. Als u een van deze drie poorten gebruikt (zoals we dit in het bovenstaande voorbeeld doen), dan hoeft u niets te doen. U moet kunnen om de load balancer-uw agent FQDN en telkens wanneer die u vernieuwt u een van de drie webservers op een wijze round robin hebt bereikt. 

Als u een andere poort gebruikt, moet u een round-robinregel en een test toevoegen op de load balancer voor de poort die u gebruikt. Dit kan vanuit de [Azure CLI](../../azure-resource-manager/xplat-cli-azure-resource-manager.md), met de opdrachten `azure network lb rule create` en `azure network lb probe create`.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over taakverdeling in ACS aan het Marathon- en Azure load balancers met inbegrip van de volgende acties:

> [!div class="checklist"]
> * Een Load Balancer van Marathon configureren
> * Implementeer een toepassing met behulp van de load balancer
> * Configureren en de Azure load balancer

Ga naar de volgende zelfstudie voor meer informatie over de integratie van Azure-opslag met DC/OS in Azure.

> [!div class="nextstepaction"]
> [De bestandsshare koppelen Azure in DC/OS-cluster](container-service-dcos-fileshare.md)