---
title: Gebruik Docker Compose op een virtuele Linux-machine in Azure | Microsoft Docs
description: Het gebruik van Docker en opstellen op Linux virtuele machines met de Azure CLI
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 02ab8cf9-318d-4a28-9d0c-4a31dccc2a84
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: iainfou
ms.openlocfilehash: 387f3095e8eebce3fa6c2f47ffc87995e65bfe2b
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29848403"
---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a>Aan de slag met Docker en opstellen om te definiëren en een toepassing met meerdere container in Azure uitvoeren
Met [opstellen](http://github.com/docker/compose), u een eenvoudige tekstbestand gebruiken voor het definiëren van een toepassing die bestaat uit meerdere Docker-containers. U vervolgens spin van uw toepassing in één opdracht dat alles voor het implementeren van uw omgeving gedefinieerde werkt. Een voorbeeld: in dit artikel leest u hoe snel een WordPress-blog met een back-end MariaDB SQL-database op een VM Ubuntu instellen. U kunt ook Compose gebruiken voor het instellen van complexe toepassingen.


## <a name="set-up-a-linux-vm-as-a-docker-host"></a>Instellen van een Linux-VM als Docker-host
U kunt verschillende Azure procedures en de beschikbare installatiekopieën of de Resource Manager-sjablonen in Azure Marketplace gebruiken voor het maken van een Linux-VM en ingesteld als een Docker-host. Zie bijvoorbeeld [gebruik van de Docker-VM-extensie voor het implementeren van uw omgeving](dockerextension.md) snel een Ubuntu VM maken met de virtuele machine in Azure Docker-extensie met behulp van een [snelstartsjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Wanneer u de Docker-VM-extensie, uw virtuele machine is automatisch geconfigureerd als een Docker-host en Compose is al geïnstalleerd.


### <a name="create-docker-host-with-azure-cli-20"></a>Docker-host te maken met Azure CLI 2.0
Installeer de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) en meld u aan op een Azure-account met [az aanmelding](/cli/azure/reference-index#az_login).

Maak eerst een resourcegroep voor uw omgeving Docker met [az groep maken](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vervolgens implementeert u een virtuele machine met [az implementatie maken](/cli/azure/group/deployment#az_group_deployment_create) waarin de Azure Docker VM-extensie van [deze Azure Resource Manager-sjabloon op GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Geef desgevraagd uw eigen unieke waarden voor *newStorageAccountName*, *adminUsername*, *adminPassword*, en *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Het duurt enkele minuten duren voordat de implementatie is voltooid.


## <a name="verify-that-compose-is-installed"></a>Controleren of Compose is geïnstalleerd
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

Als u wilt controleren of Compose is geïnstalleerd op de virtuele machine, voer de volgende opdracht:

```bash
docker-compose --version
```

Ziet u uitvoer ziet er als *docker compose 1.6.2, 4d 72027 bouwen*.

> [!TIP]
> Als u een andere methode gebruikt voor het maken van een Docker-host en moet installeren zelf samenstellen, raadpleegt u de [documentatie opstellen](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="create-a-docker-composeyml-configuration-file"></a>Maak een docker-compose.yml-configuratiebestand
Vervolgens maakt u een `docker-compose.yml` bestand, dat gewoon een tekst configuratiebestand is voor het definiëren van de Docker-containers worden uitgevoerd op de virtuele machine. Het bestand geeft de afbeelding die moet worden uitgevoerd op elke container (of het mogelijk een build van een Dockerfile) nodig omgevingsvariabelen en afhankelijkheden, poorten en de koppelingen tussen containers. Zie voor meer informatie over yml file-syntaxis, [opstellen bestandsverwijzing](https://docs.docker.com/compose/compose-file/).

Maak een *docker-compose.yml* bestand. Gebruik uw favoriete teksteditor sommige gegevens toevoegen aan het bestand. Het volgende voorbeeld wordt het bestand met een prompt voor `sensible-editor` kiest u een editor die u wilt gebruiken:

```bash
sensible-editor docker-compose.yml
```

Plak het volgende voorbeeld in uw bestand Docker Compose. Deze configuratie maakt gebruik van installatiekopieën van de [DockerHub register](https://registry.hub.docker.com/_/wordpress/) WordPress (het open-source weblog en content management system) en een gekoppelde back-end MariaDB SQL-database installeren. Voer uw eigen *MYSQL_ROOT_PASSWORD* als volgt:

```sh
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="start-the-containers-with-compose"></a>Starten van de containers met opstellen
In dezelfde map als uw *docker-compose.yml* -bestand, de volgende opdracht uitvoeren (afhankelijk van uw omgeving, moet u mogelijk uitvoeren `docker-compose` met `sudo`):

```bash
docker-compose up -d
```

Deze opdracht start de Docker-containers opgegeven in *docker-compose.yml*. Het duurt enkele minuten duren voor deze stap te voltooien. Ziet u uitvoer die vergelijkbaar is met het volgende voorbeeld:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> Zorg ervoor dat u de **-d** optie op opstarten, zodat de containers continu op de achtergrond uitgevoerd.


Typ om te bevestigen dat de containers zijn, `docker-compose ps`. U ziet ongeveer als volgt:

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

U kunt nu verbinding maken met WordPress rechtstreeks op de virtuele machine op poort 80. Open een webbrowser en voer de DNS-naam van uw virtuele machine (zoals `http://mypublicdns.eastus.cloudapp.azure.com`). U ziet nu de WordPress startscherm waarin u kunt de installatie voltooien en aan de slag met de toepassing.

![WordPress startscherm][wordpress_start]

## <a name="next-steps"></a>Volgende stappen
* Ga naar de [Docker VM-extensie-gebruikershandleiding](https://github.com/Azure/azure-docker-extension/blob/master/README.md) voor meer opties voor het configureren van Docker en opstellen in uw Docker-virtuele machine. Bijvoorbeeld, is een optie om het opstellen yml-bestand (geconverteerd naar JSON) rechtstreeks in de configuratie van de Docker-VM-extensie.
* Bekijk de [opstellen Naslaggids](http://docs.docker.com/compose/reference/) en [gebruikershandleiding](http://docs.docker.com/compose/) voor meer voorbeelden van het maken en implementeren van meerdere container apps.
* Een Azure Resource Manager-sjabloon gebruiken uw eigen of een bijgedragen de [community](https://azure.microsoft.com/documentation/templates/), voor het implementeren van een Azure-VM met Docker en een toepassing die is ingesteld met opstellen. Bijvoorbeeld, de [implementeren van een WordPress-blog met Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) sjabloon worden gebruikt voor Docker en opstellen WordPress snel implementeren met een MySQL-back-end op een Ubuntu VM.
* Probeer de integratie van Docker Compose met een Docker Swarm-cluster. Zie [met behulp van opstellen met Swarm](https://docs.docker.com/compose/swarm/) voor scenario's.

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
