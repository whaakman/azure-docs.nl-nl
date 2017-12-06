---
title: Zelfstudie voor Azure Container Service - DC/OS beheren
description: Zelfstudie voor Azure Container Service - DC/OS beheren
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 07/17/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0c58bd764cf0fdacd55675f8343c6e7481a11823
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="azure-container-service-tutorial---manage-dcos"></a>Zelfstudie voor Azure Container Service - DC/OS beheren

DC/OS biedt een gedistribueerde platform voor lopende moderne en beperkte toepassingen. Met Azure Container Service is het inrichten van een productie gereed DC/OS-cluster snel en eenvoudig. Deze eenvoudige stappen die nodig zijn voor snel starten-details implementeert een DC/OS-cluster en voer basic werkbelasting.

> [!div class="checklist"]
> * Een ACS-DC/OS-cluster maken
> * Verbinding maken met het cluster
> * De DC/OS CLI installeren
> * Een toepassing implementeert op het cluster
> * Schalen van een toepassing op het cluster
> * De DC/OS clusterknooppunten schalen
> * Basic DC/OS-management
> * De DC/OS-cluster verwijderen

Voor deze zelfstudie is versie 2.0.4 of hoger van de Azure CLI vereist. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="create-dcos-cluster"></a>DC/OS-cluster maken

Maak eerst een resourcegroep met de [az groep maken](/cli/azure/group#create) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *Europa West*.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Maak vervolgens een DC/OS-cluster met de [az acs maken](/cli/azure/acs#create) opdracht.

Het volgende voorbeeld wordt een DC/OS-cluster met de naam *myDCOSCluster* en SSH-sleutels gemaakt als deze niet al bestaan. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

Na enkele minuten, de opdracht is voltooid en retourneert informatie over de implementatie.

## <a name="connect-to-dcos-cluster"></a>Verbinding maken met DC/OS-cluster

Zodra een DC/OS-cluster is gemaakt, kan het zijn toegang tot en met een SSH-tunnel. Voer de volgende opdracht om te retourneren van het openbare IP-adres van de DC/OS-master. Dit IP-adres is opgeslagen in een variabele en gebruikt in de volgende stap.

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

De SSH-tunnel maken, voert de volgende opdracht uit en volg de aanwijzingen op het scherm instructies. Als poort 80 al in gebruik is is, mislukt de opdracht. De via een tunnel poort naar een niet in gebruik, zoals bijwerken `85:localhost:80`. 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

## <a name="install-dcos-cli"></a>DC/OS CLI installeren

Installeer de DC/OS cli met de [az acs dcos install-cli](/azure/acs/dcos#install-cli) opdracht. Als u Azure CloudShell gebruikt, wordt de DC/OS CLI is al geïnstalleerd. Als u de Azure CLI voor Mac OS- of Linux uitvoert, moet u mogelijk de opdracht uitvoert met sudo.

```azurecli
az acs dcos install-cli
```

Voordat de CLI kan worden gebruikt met het cluster, moet deze worden geconfigureerd voor het gebruik van de SSH-tunnel. Voer de volgende opdracht, de poort aan te passen, indien nodig om dit te doen.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Een toepassing uitvoeren

De standaardwaarde planning mechanisme voor een ACS-DC/OS-cluster is Marathon. Marathon wordt gebruikt om een toepassing starten en beheren van de status van de toepassing op de DC/OS-cluster. Als u een toepassing via Marathon plannen, maakt u een bestand met de naam **marathon app.json**, en kopieer de volgende inhoud in de App. 

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

Voer de volgende opdracht om te plannen van de toepassing uit te voeren op de DC/OS-cluster.

```azurecli
dcos marathon app add marathon-app.json
```

Overzicht van de implementatiestatus voor de app, voer de volgende opdracht.

```azurecli
dcos marathon app list
```

Wanneer de **taken** kolomwaarde verandert van *0/1* naar *1/1*, implementatie van toepassing is voltooid.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a>Marathon-toepassing schalen

In het vorige voorbeeld is een toepassing met één exemplaar gemaakt. Voor het bijwerken van deze implementatie dat drie exemplaren van de toepassing beschikbaar zijn, opent u de **marathon app.json** bestand, en werk de exemplaareigenschap tot 3.

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 3,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

Bijwerken van de toepassing met behulp van de `dcos marathon app update` opdracht.

```azurecli
dcos marathon app update demo-app-private < marathon-app.json
```

Overzicht van de implementatiestatus voor de app, voer de volgende opdracht.

```azurecli
dcos marathon app list
```

Wanneer de **taken** kolomwaarde verandert van *1/3* naar *31/*, implementatie van toepassing is voltooid.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a>Internet toegankelijk app uitvoeren

De ACS-DC/OS-cluster bestaat uit twee sets van knooppunt, een openbare die toegankelijk is op het internet en een particuliere dat niet toegankelijk op het internet. De standaardset is de persoonlijke knooppunten, die is gebruikt in het vorige voorbeeld.

U kunt een toepassing toegankelijk op het internet, moet u deze aan de verzameling openbare implementeren. Om dit te doen, geven de `acceptedResourceRoles` een waarde van het object `slave_public`.

Maak een bestand met de naam **nginx public.json** en kopieer de volgende inhoud in de App.

```json
{
  "id": "demo-app",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  },
  "acceptedResourceRoles": [
    "slave_public"
  ]
}
```

Voer de volgende opdracht om te plannen van de toepassing uit te voeren op de DC/OS-cluster.

```azurecli 
dcos marathon app add nginx-public.json
```

Haal het openbare IP-adres van de DC/OS-cluster openbare agents.

```azurecli 
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Bladeren naar dit adres retourneert de standaard NGINX-site.

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a>Schaal DC/OS-cluster

In de voorgaande voorbeelden is een toepassing geschaald naar meerdere exemplaren. De DC/OS-infrastructuur kan ook worden uitgebreid om meer of minder rekencapaciteit. Dit wordt gedaan met de [az acs schalen]() opdracht. 

Als het huidige aantal DC/OS-agents weergeven, gebruikt de [az acs weergeven](/cli/azure/acs#show) opdracht.

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

Voor een verhoging van het aantal tot en met 5, gebruiken de [az acs schalen](/cli/azure/acs#scale) opdracht. 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a>DC/OS-cluster verwijderen

Wanneer deze niet langer nodig is, kunt u de [az groep verwijderen](/cli/azure/group#delete) opdracht voor het verwijderen van de resourcegroep, DC/OS-cluster, en alle bijbehorende resources.

```azurecli 
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over basic DC/OS-beheertaak, waaronder de volgende. 

> [!div class="checklist"]
> * Een ACS-DC/OS-cluster maken
> * Verbinding maken met het cluster
> * De DC/OS CLI installeren
> * Een toepassing implementeert op het cluster
> * Schalen van een toepassing op het cluster
> * De DC/OS clusterknooppunten schalen
> * De DC/OS-cluster verwijderen

Ga naar de volgende zelfstudie voor meer informatie over taakverdeling toepassing in DC/OS op Azure. 

> [!div class="nextstepaction"]
> [Load balancing gebruiken voor toepassingen](container-service-load-balancing.md)