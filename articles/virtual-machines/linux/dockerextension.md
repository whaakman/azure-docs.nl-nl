---
title: Gebruik de Azure Docker VM-extensie | Microsoft Docs
description: Informatie over het gebruik van de Docker VM-extensie snel en veilig implementeren van een Docker-omgeving in Azure met behulp van Resource Manager-sjablonen en Azure CLI
services: virtual-machines-linux
documentationcenter: ''
author: zr-msft
manager: jeconnoc
editor: ''
ms.assetid: 936d67d7-6921-4275-bf11-1e0115e66b7f
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/18/2017
ms.author: zarhoads
ms.openlocfilehash: 75959225d6fcc5487466ed26a21ba2d26c55cde9
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49465930"
---
# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Een Docker-omgeving maken in Azure met behulp van de Docker VM-extensie

Docker is een populaire containerbeheer en beeldbewerking platform waarmee u snel werken met containers in Linux. In Azure zijn er verschillende manieren waarop u kunt Docker implementeren op basis van uw behoeften. In dit artikel richt zich op het gebruik van de Docker VM-extensie en Azure Resource Manager-sjablonen met de Azure CLI. 

> [!WARNING]
> De Azure Docker VM-extensie voor Linux is afgeschaft en wordt in November 2018 beëindigd.
> De extensie installeert alleen Docker, dus alternatieven zoals cloud-init of de aangepaste Scriptextensie zijn een betere manier om de Docker-versie van keuze te installeren. Zie voor meer informatie over het gebruik van cloud-init [aanpassen van een Linux-VM met cloud-init](tutorial-automate-vm-deployment.md).

## <a name="azure-docker-vm-extension-overview"></a>Overzicht van Azure Docker VM-extensie
De Azure Docker VM-extensie installeert en configureert de Docker-daemon, Docker-client en Docker Compose in uw Linux-machine (VM). Met behulp van de Azure Docker VM-extensie, hebt u meer controle en functies dan gewoon met behulp van Docker Machine of het maken van de Docker-host zelf. Deze extra functies, zoals [Docker Compose](https://docs.docker.com/compose/overview/), Controleer de Azure Docker VM-extensie is geschikt voor krachtiger developer- of productieomgevingen.

Zie de volgende artikelen voor meer informatie over de verschillende implementatiemethoden, met behulp van Docker Machine en Azure Container Services, waaronder:

* Op snel prototypen maken een app, kunt u een enkele Docker-host met behulp van [Docker Machine](docker-machine.md).
* Voor het bouwen van gereed is voor productie, schaalbare omgevingen die aanvullende tools voor planning en beheer bieden, kunt u implementeren een [Kubernetes](../../container-service/kubernetes/index.yml) of [Docker Swarm](../../container-service/dcos-swarm/index.yml) -cluster in Azure Container Services.


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Implementeren van een sjabloon met de Azure Docker VM-extensie
We gaan een bestaande quickstart-sjabloon gebruiken om een Ubuntu-VM die gebruikmaakt van de Azure Docker VM-extensie installeren en configureren van de Docker-host te maken. U kunt hier de sjabloon weergeven: [eenvoudige implementatie van een Ubuntu-VM met Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). U moet de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index#az_login).

Maak eerst een resourcegroep met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vervolgens implementeert u een virtuele machine met [az group deployment maken](/cli/azure/group/deployment#az_group_deployment_create) waarin de Azure Docker VM-extensie van [deze Azure Resource Manager-sjabloon op GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Geef desgevraagd uw eigen unieke waarden voor *newStorageAccountName*, *adminUsername*, *adminPassword*, en *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Het duurt een paar minuten voor de implementatie is voltooid.


## <a name="deploy-your-first-nginx-container"></a>Uw eerste NGINX-container implementeren
Als u wilt weergeven van details van uw virtuele machine, met inbegrip van de DNS-naam gebruiken [az vm show](/cli/azure/vm#az_vm_show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --show-details \
    --query [fqdns] \
    --output tsv
```

SSH naar uw nieuwe Docker-host. Geef uw eigen gebruikersnaam en het DNS-naam van de voorgaande stappen:

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

Zodra u aangemeld bij de Docker-host, moet u gaan we een NGINX-container uitvoeren:

```bash
sudo docker run -d -p 80:80 nginx
```

De uitvoer is vergelijkbaar met het volgende voorbeeld als de NGINX-installatiekopie is gedownload en een container gestart:

```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Controleer de status van de containers die worden uitgevoerd op uw Docker-host als volgt:

```bash
sudo docker ps
```

De uitvoer is vergelijkbaar met het volgende voorbeeld, waarin wordt getoond dat de NGINX-container actief is en TCP-poorten 80 en 443 en worden doorgestuurd:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Als u wilt uw container in actie zien, opent u een webbrowser en voert de DNS-naam van uw Docker-host:

![Ngnix-container die wordt uitgevoerd](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Azure Docker VM-extensie-sjabloonverwijzing
Het vorige voorbeeld maakt gebruik van een bestaande quickstart-sjabloon. U kunt ook de Azure Docker VM-extensie implementeren met uw eigen Resource Manager-sjablonen. Om dit te doen, voegt u het volgende toe aan uw Resource Manager-sjablonen definiëren de `vmName` van uw virtuele machine op de juiste wijze:

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.*",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

U vindt meer gedetailleerde stapsgewijze instructies over het gebruik van Resource Manager-sjablonen door te lezen [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

## <a name="next-steps"></a>Volgende stappen
U kunt desgewenst [configureren van de Docker-daemon TCP-poort](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), begrijpen [Docker-beveiliging](https://docs.docker.com/engine/security/security/), of implementeer containers met [Docker Compose](https://docs.docker.com/compose/overview/). Zie voor meer informatie over de Azure Docker VM-extensie zichzelf, het [GitHub-project](https://github.com/Azure/azure-docker-extension/).

Lees meer informatie over de aanvullende opties van de implementatie van Docker in Azure:

* [Docker Machine gebruiken met de Azure-stuurprogramma](docker-machine.md)  
* [Aan de slag met Docker en Compose om te definiëren en een toepassing met meerdere containers worden uitgevoerd op een Azure-machine](docker-compose-quickstart.md).
* [Een Azure Container Service-cluster implementeren](../../container-service/dcos-swarm/container-service-deployment.md)

