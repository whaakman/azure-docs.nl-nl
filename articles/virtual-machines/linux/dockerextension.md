---
title: Gebruik de Azure Docker VM-extensie | Microsoft Docs
description: Informatie over het gebruik van de Docker-VM-extensie voor het snel en veilig implementeren van een Docker-omgeving in Azure met behulp van Resource Manager-sjablonen en Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
ms.assetid: 936d67d7-6921-4275-bf11-1e0115e66b7f
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/18/2017
ms.author: iainfou
ms.openlocfilehash: 2fd7be23c4146051197c4b6d7db6deb06dfa416d
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Maakt een Docker-omgeving in Azure met behulp van de Docker-VM-extensie
Docker is een populair containerbeheer en installatiekopieën platform waarmee u snel werken met containers op Linux. In Azure zijn er verschillende manieren waarop u kunt Docker implementeren volgens uw behoeften. Dit artikel is gericht op het gebruik van de Docker-VM-extensie en Azure Resource Manager-sjablonen met de Azure CLI 2.0. U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](dockerextension-nodejs.md).

## <a name="azure-docker-vm-extension-overview"></a>Overzicht van Azure Docker VM-extensie
De virtuele machine in Azure Docker-extensie installeert en configureert u de Docker-daemon, Docker-client en Docker Compose in uw virtuele Linux-machine (VM). U hebt meer controle en functies dan gewoon met behulp van Docker-Machine of maken van de Docker-host met behulp van de virtuele machine in Azure Docker-extensie. Deze aanvullende functies, zoals [Docker Compose](https://docs.docker.com/compose/overview/), zorg dat de virtuele machine in Azure Docker-extensie is geschikt voor krachtiger developer- of productieomgevingen.

Zie voor meer informatie over de verschillende implementatiemethoden, met behulp van Docker-Machine en Services van Azure-Container, inclusief de volgende artikelen:

* Snel prototype een app kunt u één Docker-host met behulp van [Docker-Machine](docker-machine.md).
* Gereed voor productie, schaalbare omgevingen die aanvullende plannings- en hulpprogramma's bieden bouwen, kunt u implementeren een [Kubernetes](../../container-service/kubernetes/index.yml) of [Docker Swarm](../../container-service/dcos-swarm/index.yml) cluster op Azure Container Services.


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Een sjabloon met de extensie Azure Docker VM implementeren
We gebruiken een bestaande sjabloon voor de Quick Start voor het maken van een Ubuntu VM die gebruikmaakt van de virtuele machine in Azure Docker-uitbreiding installeren en configureren van de Docker-host. U kunt de sjabloon hier weergeven: [eenvoudige implementatie van een VM Ubuntu met Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). U moet de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/#az_login).

Maak eerst een resourcegroep met [az groep maken](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vervolgens implementeert u een virtuele machine met [az implementatie maken](/cli/azure/group/deployment#az_group_deployment_create) waarin de Azure Docker VM-extensie van [deze Azure Resource Manager-sjabloon op GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Geef desgevraagd uw eigen unieke waarden voor *newStorageAccountName*, *adminUsername*, *adminPassword*, en *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Het duurt enkele minuten duren voordat de implementatie is voltooid.


## <a name="deploy-your-first-nginx-container"></a>Uw eerste NGINX-container implementeren
Gebruiken om te bekijken van de VM, met inbegrip van de DNS-naam, [az vm weergeven](/cli/azure/vm#az_vm_show):

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

Wanneer aangemeld bij de Docker-host, gaan we een NGINX-container te uitvoeren:

```bash
sudo docker run -d -p 80:80 nginx
```

De uitvoer is vergelijkbaar met het volgende voorbeeld wordt de NGINX-installatiekopie gedownload en een container gestart:

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

Controleer de status van de containers die wordt uitgevoerd op uw host Docker als volgt:

```bash
sudo docker ps
```

De uitvoer is vergelijkbaar met het volgende voorbeeld, waaruit blijkt dat de NGINX-container actief is en TCP-poorten 80 en 443 en ernaar worden doorgestuurd:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

De container in actie zien, opent u een webbrowser en voer de DNS-naam van de Docker-host:

![Actieve ngnix container](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Azure Docker VM-sjabloon uitbreidingsverwijzing
Het vorige voorbeeld maakt gebruik van een bestaande Quick Start-sjabloon. U kunt ook de Azure Docker VM-extensie implementeren met uw eigen Resource Manager-sjablonen. Om dit te doen, kunt u het volgende toevoegen aan de Resource Manager-sjablonen, definiëren de `vmName` van uw virtuele machine op de juiste wijze:

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
U kunt desgewenst [de Docker-daemon TCP-poort configureren](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), begrijpen [Docker-beveiliging](https://docs.docker.com/engine/security/security/), of met behulp van containers implementeren [Docker Compose](https://docs.docker.com/compose/overview/). Zie voor meer informatie over de Azure Docker VM-extensie zichzelf, het [GitHub project](https://github.com/Azure/azure-docker-extension/).

Lees meer informatie over de extra Docker implementatie-opties in Azure:

* [Docker-Machine met het Azure-stuurprogramma gebruiken](docker-machine.md)  
* [Aan de slag met Docker en opstellen om te definiëren en een toepassing met meerdere container uitvoert op een virtuele machine van Azure](docker-compose-quickstart.md).
* [Een Azure Container Service-cluster implementeren](../../container-service/dcos-swarm/container-service-deployment.md)

