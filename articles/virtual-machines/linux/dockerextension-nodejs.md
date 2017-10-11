---
title: De virtuele machine in Azure Docker-uitbreiding gebruiken met de Azure CLI 1.0 | Microsoft Docs
description: Informatie over het gebruik van de Docker-VM-extensie voor het snel en veilig implementeren van een Docker-omgeving in Azure met behulp van Resource Manager-sjablonen.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: a3cbcf63533f4042dcd695e141655c5814bd7068
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension-with-the-azure-cli-10"></a>Maakt een Docker-omgeving in Azure met behulp van de Docker-VM-extensie met de Azure CLI 1.0
Docker is een populair containerbeheer en installatiekopieën platform waarmee u snel werken met containers op Linux (en ook Windows). In Azure zijn er verschillende manieren waarop u kunt Docker implementeren volgens uw behoeften. Dit artikel is gericht op het gebruik van de Docker-VM-extensie en Azure Resource Manager-sjablonen. 

Zie voor meer informatie over de verschillende implementatiemethoden, met behulp van Docker-Machine en Services van Azure-Container, inclusief de volgende artikelen:

* Snel prototype een app kunt u één Docker-host met behulp van [Docker-Machine](docker-machine.md).
* Voor grotere, stabiele omgevingen, kunt u de virtuele machine in Azure Docker-uitbreiding biedt ook ondersteuning voor [Docker Compose](https://docs.docker.com/compose/overview/) consistente containerimplementaties te genereren. Dit artikel gegevens met behulp van de virtuele machine in Azure Docker-extensie.
* Gereed voor productie, schaalbare omgevingen die aanvullende plannings- en hulpprogramma's bieden bouwen, kunt u implementeren een [Docker Swarm-cluster op Azure Container Services](../../container-service/dcos-swarm/container-service-deployment.md).

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#azure-docker-vm-extension-overview) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](dockerextension.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel 

## <a name="azure-docker-vm-extension-overview"></a>Overzicht van Azure Docker VM-extensie
De virtuele machine in Azure Docker-extensie installeert en configureert u de Docker-daemon, Docker-client en Docker Compose in uw virtuele Linux-machine (VM). U hebt meer controle en functies dan gewoon met behulp van Docker-Machine of maken van de Docker-host met behulp van de virtuele machine in Azure Docker-extensie. Deze aanvullende functies, zoals [Docker Compose](https://docs.docker.com/compose/overview/), zorg dat de virtuele machine in Azure Docker-extensie is geschikt voor krachtiger developer- of productieomgevingen.

Azure Resource Manager-sjablonen definiëren de volledige structuur van uw omgeving. Sjablonen kunt u maken en configureren van resources, zoals de Docker-host virtuele machines, opslag, op rollen gebaseerde toegangsbeheer (RBAC) en diagnostische gegevens. U kunt deze sjablonen voor het maken van aanvullende implementaties op een consistente manier hergebruiken. Zie voor meer informatie over Azure Resource Manager en sjablonen [overzicht van Resource Manager](../../azure-resource-manager/resource-group-overview.md). 

## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Een sjabloon met de extensie Azure Docker VM implementeren
We gebruiken een bestaande sjabloon voor de Quick Start voor het maken van een Ubuntu VM die gebruikmaakt van de virtuele machine in Azure Docker-uitbreiding installeren en configureren van de Docker-host. U kunt de sjabloon hier weergeven: [eenvoudige implementatie van een VM Ubuntu met Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

U moet de [nieuwste Azure CLI](../../cli-install-nodejs.md) geïnstalleerd en geregistreerd in de modus Resource Manager als volgt gebruiken:

```azurecli
azure config mode arm
```

De sjabloon met de Azure CLI, geven de URI van de sjabloon implementeert. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *westus*. Uw eigen Resourcegroepnaam en locatie als volgt gebruiken:

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Beantwoord de wordt u gevraagd uw storage-account een naam, een gebruikersnaam en wachtwoord, en bieden een DNS-naam. De uitvoer lijkt op die in het volgende voorbeeld:

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: azureuser
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicidns
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

De Azure CLI geeft u op de vraag nadat slechts enkele seconden, maar de Docker-host is wel wordt gemaakt en geconfigureerd voor de virtuele machine in Azure Docker-uitbreiding. Het duurt enkele minuten duren voordat de implementatie is voltooid. U kunt details over het gebruik van Docker host status zien de `azure vm show` opdracht.

Het volgende voorbeeld controleert de status van de virtuele machine met de naam *myDockerVM* (de standaardnaam van de sjabloon - geen deze naam niet wijzigen) in de resourcegroep met de naam *myResourceGroup*. Voer de naam van de resourcegroep die u in de vorige stap hebt gemaakt:

```azurecli
azure vm show --resource-group myResourceGroup --name myDockerVM
```

De uitvoer van de `azure vm show` opdracht is vergelijkbaar met het volgende voorbeeld:

```azurecli
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicdns.westus.cloudapp.azure.com
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

Aan de bovenkant van de uitvoer ziet u de **ProvisioningState** van de virtuele machine. Wanneer u ziet nu *geslaagd*, de implementatie is voltooid en kunt u SSH naar de virtuele machine.

Aan het einde van de uitvoer van de *FQDN* geeft de volledig gekwalificeerde domeinnaam van uw Docker-host. Deze FDQN-naam is die u gebruikt om SSH voor uw Docker-hosts in de resterende stappen.

## <a name="deploy-your-first-nginx-container"></a>Uw eerste nginx-container implementeren
Zodra de implementatie is voltooid, SSH naar uw nieuwe Docker-host van de lokale computer. Voer uw eigen gebruikersnaam en de FQDN-naam als volgt:

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com
```

Wanneer aangemeld bij de Docker-host, gaan we een nginx-container te uitvoeren:

```bash
sudo docker run -d -p 80:80 nginx
```

De uitvoer is vergelijkbaar met het volgende voorbeeld wordt de nginx-installatiekopie gedownload en een container gestart:

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

De uitvoer is vergelijkbaar met het volgende voorbeeld, waaruit blijkt dat de nginx-container actief is en TCP-poorten 80 en 443 en ernaar worden doorgestuurd:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

De container in actie zien, opent u een webbrowser en voer de FQDN-naam van uw Docker-host:

![Actieve ngnix container](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Azure Docker VM-sjabloon uitbreidingsverwijzing
Het vorige voorbeeld maakt gebruik van een bestaande Quick Start-sjabloon. U kunt ook de Azure Docker VM-extensie implementeren met uw eigen Resource Manager-sjablonen. Om dit te doen, kunt u het volgende toevoegen aan de Resource Manager-sjablonen, definiëren de *vmName* van uw virtuele machine op de juiste wijze:

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
    "typeHandlerVersion": "1.1",
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

