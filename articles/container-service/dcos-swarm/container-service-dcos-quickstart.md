---
title: Azure Container Service Quick Start - DC/OS-Cluster implementeren | Microsoft Docs
description: Azure Container Service Quick Start - DC/OS-Cluster implementeren
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 8070d224fe6281e61f67483d4f1dd905a2ab99eb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-a-dcos-cluster"></a>Een DC/OS-cluster implementeren

DC/OS biedt een gedistribueerde platform voor lopende moderne en beperkte toepassingen. Met Azure Container Service is het inrichten van een productie gereed DC/OS-cluster snel en eenvoudig. Deze informatie snel starten de eenvoudige stappen die nodig zijn voor het implementeren van een DC/OS-cluster en basic werkbelasting uitvoeren.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor deze zelfstudie is versie 2.0.4 of hoger van de Azure CLI vereist. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-dcos-cluster"></a>DC/OS-cluster maken

Maken van een DC/OS-cluster met de [az acs maken](/cli/azure/acs#create) opdracht.

Het volgende voorbeeld wordt een DC/OS-cluster met de naam *myDCOSCluster* en SSH-sleutels gemaakt als deze niet al bestaan. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.  

```azurecli
az acs create --orchestrator-type dcos --resource-group myResourceGroup --name myDCOSCluster --generate-ssh-keys
```

In sommige gevallen, zoals met een beperkte proefversie, heeft een Azure-abonnement beperkte toegang tot Azure-resources. Als de implementatie mislukt vanwege beperkte beschikbare kernen, verminder dan het aantal standaardagenten door `--agent-count 1` toe te voegen aan de opdracht [az acs create](/cli/azure/acs#create). 

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

De SSH-tunnel kan worden getest door te bladeren naar `http://localhost`. Als een poort andere 80 is bereikt, de locatie moet worden gezocht aanpassen. 

Als de SSH-tunnel is gemaakt, wordt de DC/OS-portal geretourneerd.

![DCOS GEBRUIKERSINTERFACE](./media/container-service-dcos-quickstart/dcos-ui.png)

## <a name="install-dcos-cli"></a>DC/OS CLI installeren

De DC/OS-opdrachtregelinterface wordt gebruikt voor het beheren van een DC/OS-cluster vanaf de opdrachtregel. Installeer de DC/OS cli met de [az acs dcos install-cli](/azure/acs/dcos#install-cli) opdracht. Als u Azure CloudShell gebruikt, wordt de DC/OS CLI is al geïnstalleerd. 

Als u de Azure CLI voor Mac OS- of Linux uitvoert, moet u mogelijk de opdracht uitvoert met sudo.

```azurecli
az acs dcos install-cli
```

Voordat de CLI kan worden gebruikt met het cluster, moet deze worden geconfigureerd voor het gebruik van de SSH-tunnel. Voer de volgende opdracht, de poort aan te passen, indien nodig om dit te doen.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Een toepassing uitvoeren

De standaardwaarde planning mechanisme voor een ACS-DC/OS-cluster is Marathon. Marathon wordt gebruikt om een toepassing starten en beheren van de status van de toepassing op de DC/OS-cluster. Als u een toepassing via Marathon plannen, maakt u een bestand met de naam *marathon app.json*, en kopieer de volgende inhoud in de App. 

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
dcos marathon app add marathon-app.json
```

Overzicht van de implementatiestatus voor de app, voer de volgende opdracht.

```azurecli
dcos marathon app list
```

Wanneer de **WACHTEN** kolomwaarde verandert van *True* naar *False*, implementatie van toepassing is voltooid.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/1    ---       ---      False      DOCKER   None
```

Haal het openbare IP-adres van de agents DC/OS-cluster.

```azurecli
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Bladeren naar dit adres retourneert de standaard NGINX-site.

![NGINX](./media/container-service-dcos-quickstart/nginx.png)

## <a name="delete-dcos-cluster"></a>DC/OS-cluster verwijderen

Wanneer deze niet langer nodig is, kunt u de [az groep verwijderen](/cli/azure/group#delete) opdracht voor het verwijderen van de resourcegroep, DC/OS-cluster, en alle bijbehorende resources.

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Volgende stappen

In deze snel starten moet u een DC/OS-cluster hebt geïmplementeerd en een eenvoudige Docker-container hebt uitgevoerd op het cluster. Blijven de ACS-zelfstudies voor meer informatie over Azure Container Service.

> [!div class="nextstepaction"]
> [Een ACS-DC/OS-Cluster beheren](container-service-dcos-manage-tutorial.md)