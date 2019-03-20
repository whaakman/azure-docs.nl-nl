---
title: Docker Compose gebruiken op een virtuele Linux-machine in Azure | Microsoft Docs
description: Over het installeren en gebruiken van Docker en Compose op virtuele Linux-machines met de Azure CLI
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 02ab8cf9-318d-4a28-9d0c-4a31dccc2a84
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2019
ms.author: cynthn
ms.openlocfilehash: 03501ea774cf58a4be88ed9155e5cfdfb99f0379
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58007190"
---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a>Aan de slag met Docker en Compose om te definiëren en uitvoeren van een toepassing met meerdere containers in Azure
Met [opstellen](https://github.com/docker/compose), u een eenvoudig tekstbestand gebruiken voor het definiëren van een toepassing die bestaat uit meerdere Docker-containers. U instellen uw toepassing in één opdracht dat alles is voor het implementeren van uw omgeving gedefinieerde vervolgens. Bijvoorbeeld: in dit artikel wordt beschreven hoe u snel een WordPress-blog met een back-end MariaDB SQL-database op een Ubuntu-VM instellen. U kunt ook opstellen voor het instellen van complexere toepassingen gebruiken.

In dit artikel is laatste over het gebruik van 2/14/2019 getest door de [Azure Cloud Shell](https://shell.azure.com/bash) en de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) versie 2.0.58.

## <a name="create-docker-host-with-azure-cli"></a>Docker-host maken met Azure CLI
Installeer de meest recente [Azure CLI](/cli/azure/install-az-cli2) en aan te melden bij een Azure-account met [az login](/cli/azure/reference-index).

Maak eerst een resourcegroep voor uw Docker-omgeving met [az-groep maken](/cli/azure/group). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive
az group create --name myDockerGroup --location eastus
```

Maak een bestand met de naam *cloud-init.txt* en plak de volgende configuratie. Voer `sensible-editor cloud-init.txt` in voor het maken van het bestand en om een overzicht van beschikbare editors te zien. 

```yaml
#include https://get.docker.com
```

Maak een virtuele machine met [az vm create](/cli/azure/vm#az-vm-create). Gebruik de `--custom-data`-parameter om door te geven in uw cloud-init-configuratiebestand. Geef het volledige pad naar *cloud-init.txt* op als u het bestand buiten uw huidige werkmap hebt opgeslagen. Het volgende voorbeeld wordt een virtuele machine met de naam *myDockerVM* en open poort 80 voor webverkeer.

```azurecli-interactive
az vm create \
    --resource-group myDockerGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
az vm open-port --port 80 \
    --resource-group myDockerGroup \
    --name myDockerVM
```

Het duurt enkele minuten voordat de virtuele machine wordt gemaakt, de pakketten worden geïnstalleerd en de app gestart. Er zijn achtergrondtaken die nog worden uitgevoerd nadat u door de Azure CLI bent teruggeleid naar de prompt. Wanneer de virtuele machine is gemaakt, let op de `publicIpAddress` weergegeven door de Azure CLI. 

                 

## <a name="install-compose"></a>Installeer opstellen


SSH naar uw nieuwe virtuele machine van de Docker-host. Geef uw eigen IP-adres.

```bash
ssh azureuser@10.10.111.11
```

Installeer opstellen op de virtuele machine.

```bash
sudo apt install docker-compose
```


## <a name="create-a-docker-composeyml-configuration-file"></a>Een docker-compose.yml-configuratiebestand maken
Maak een `docker-compose.yml` configuratiebestand voor het definiëren van de Docker-containers uit te voeren op de virtuele machine. Het bestand geeft u de installatiekopie uit te voeren op elke container, die nodig zijn omgevingsvariabelen en afhankelijkheden, poorten en de koppelingen tussen containers. Zie voor meer informatie over yml bestand syntaxis [Compose verwijzing naar een bestand](https://docs.docker.com/compose/compose-file/).

Maak een *docker-compose.yml* bestand. Gebruik uw favoriete teksteditor enkele gegevens toevoegen aan het bestand. Het volgende voorbeeld wordt het bestand met een prompt voor `sensible-editor` om op te halen van een editor die u wilt gebruiken.

```bash
sensible-editor docker-compose.yml
```

Plak het volgende voorbeeld in uw Docker Compose-bestand. Deze configuratie maakt gebruik van afbeeldingen uit de [DockerHub register](https://registry.hub.docker.com/_/wordpress/) WordPress (het open-source blogengines en inhoud management system) en een gekoppelde back-end MariaDB SQL-database installeren. Voer uw eigen *MYSQL_ROOT_PASSWORD*.

```yml
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

## <a name="start-the-containers-with-compose"></a>Start de containers met opstellen
In dezelfde map als uw *docker-compose.yml* -bestand, voert u de volgende opdracht uit (afhankelijk van uw omgeving, moet u mogelijk uitvoeren `docker-compose` met behulp van `sudo`):

```bash
sudo docker-compose up -d
```

Met deze opdracht start de Docker-containers die is opgegeven in *docker-compose.yml*. Het duurt een minuut of twee voor deze stap te voltooien. Hier ziet u uitvoer die vergelijkbaar is met het volgende:

```
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```


Typ om te controleren dat de containers zijn, `sudo docker-compose ps`. U ziet er ongeveer als:

```
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

U kunt nu verbinding maken met WordPress rechtstreeks op de virtuele machine op poort 80. Open een webbrowser en voer de naam van de IP-adres van uw virtuele machine. U ziet nu de WordPress startscherm, waar u kunt de installatie voltooien en aan de slag met de toepassing.

![WordPress-startscherm](./media/docker-compose-quickstart/wordpressstart.png)

## <a name="next-steps"></a>Volgende stappen
* Bekijk de [Compose opdrachtregelreferentiewebpagina](https://docs.docker.com/compose/reference/) en [gebruikershandleiding](https://docs.docker.com/compose/) voor meer voorbeelden van het bouwen en implementeren van apps met meerdere containers.
* Een Azure Resource Manager-sjabloon gebruiken uw eigen of één hebben bijgedragen uit de [community](https://azure.microsoft.com/documentation/templates/), voor het implementeren van een virtuele Azure-machine met Docker en een toepassing instellen met opstellen. Bijvoorbeeld, de [implementeren van een WordPress-blog met Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) sjabloon maakt gebruik van Docker en Compose WordPress snel implementeren met een MySQL-back-end op een Ubuntu-VM.
* Probeer het integreren van Docker Compose met een Docker Swarm-cluster. Zie [Compose gebruiken met Swarm](https://docs.docker.com/compose/swarm/) voor scenario's.

