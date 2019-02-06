---
title: Docker Compose gebruiken op een virtuele Linux-machine in Azure | Microsoft Docs
description: Het gebruik van Docker en Compose op Linux-machines met de Azure CLI
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
ms.date: 12/18/2017
ms.author: cynthn
ms.openlocfilehash: 257083e1ae0c3c1cb3c5421882ffd0e06e2d1f5c
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55752140"
---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a>Aan de slag met Docker en Compose om te definiëren en uitvoeren van een toepassing met meerdere containers in Azure
Met [opstellen](http://github.com/docker/compose), u een eenvoudig tekstbestand gebruiken voor het definiëren van een toepassing die bestaat uit meerdere Docker-containers. U instellen uw toepassing in één opdracht dat alles is voor het implementeren van uw omgeving gedefinieerde vervolgens. Bijvoorbeeld: in dit artikel wordt beschreven hoe u snel een WordPress-blog met een back-end MariaDB SQL-database op een Ubuntu-VM instellen. U kunt ook opstellen voor het instellen van complexere toepassingen gebruiken.


## <a name="set-up-a-linux-vm-as-a-docker-host"></a>Instellen van een Linux-VM als een Docker-host
U kunt verschillende Azure-procedures en de beschikbare installatiekopieën of Resource Manager-sjablonen gebruiken in de Azure Marketplace te maken van een Linux-VM instellen als een Docker-host. Zie bijvoorbeeld [om uw omgeving te implementeren met behulp van de Docker VM-extensie](dockerextension.md) snel een Ubuntu-VM maken met de Azure Docker VM-extensie met behulp van een [quickstart-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Wanneer u de Docker VM-extensie gebruikt, wordt uw virtuele machine is automatisch ingesteld als een Docker-host en Compose al is geïnstalleerd.


### <a name="create-docker-host-with-azure-cli"></a>Docker-host maken met Azure CLI
Installeer de meest recente [Azure CLI](/cli/azure/install-az-cli2) en aan te melden bij een Azure-account met [az login](/cli/azure/reference-index).

Maak eerst een resourcegroep voor uw Docker-omgeving met [az-groep maken](/cli/azure/group). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vervolgens implementeert u een virtuele machine met [az group deployment maken](/cli/azure/group/deployment) waarin de Azure Docker VM-extensie van [deze Azure Resource Manager-sjabloon op GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Geef desgevraagd uw eigen unieke waarden voor *newStorageAccountName*, *adminUsername*, *adminPassword*, en *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Het duurt een paar minuten voor de implementatie is voltooid.


## <a name="verify-that-compose-is-installed"></a>Controleer of opstellen is geïnstalleerd
Als u wilt weergeven van details van uw virtuele machine, met inbegrip van de DNS-naam gebruiken [az vm show](/cli/azure/vm):

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

Als u wilt controleren of opstellen op de virtuele machine is geïnstalleerd, moet u de volgende opdracht uitvoeren:

```bash
docker-compose --version
```

Ziet u uitvoer die vergelijkbaar is met *1.6.2 docker-compose, het bouwen van 4d 72027*.

> [!TIP]
> Als u een andere methode gebruikt voor het maken van een Docker-host en moet installeren zelf samenstellen, raadpleegt u de [documentatie opstellen](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="create-a-docker-composeyml-configuration-file"></a>Een docker-compose.yml-configuratiebestand maken
Vervolgens maakt u een `docker-compose.yml` -bestand, dat slechts een configuratie tekstbestand is, voor het definiëren van de Docker-containers uit te voeren op de virtuele machine. Het bestand Hiermee geeft u de installatiekopie die moet worden uitgevoerd op elke container (of wordt een build van een docker-bestand) nodig omgevingsvariabelen en afhankelijkheden, poorten en de koppelingen tussen containers. Zie voor meer informatie over yml bestand syntaxis [Compose verwijzing naar een bestand](https://docs.docker.com/compose/compose-file/).

Maak een *docker-compose.yml* bestand. Gebruik uw favoriete teksteditor enkele gegevens toevoegen aan het bestand. Het volgende voorbeeld wordt het bestand met een prompt voor `sensible-editor` om op te halen van een editor die u wilt gebruiken:

```bash
sensible-editor docker-compose.yml
```

Plak het volgende voorbeeld in uw Docker Compose-bestand. Deze configuratie maakt gebruik van afbeeldingen uit de [DockerHub register](https://registry.hub.docker.com/_/wordpress/) WordPress (het open-source blogengines en inhoud management system) en een gekoppelde back-end MariaDB SQL-database installeren. Voer uw eigen *MYSQL_ROOT_PASSWORD* als volgt:

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

## <a name="start-the-containers-with-compose"></a>Start de containers met opstellen
In dezelfde map als uw *docker-compose.yml* -bestand, voert u de volgende opdracht uit (afhankelijk van uw omgeving, moet u mogelijk uitvoeren `docker-compose` met behulp van `sudo`):

```bash
docker-compose up -d
```

Met deze opdracht start de Docker-containers die is opgegeven in *docker-compose.yml*. Het duurt een minuut of twee voor deze stap te voltooien. U ziet de uitvoer is vergelijkbaar met het volgende voorbeeld:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> Zorg ervoor dat u de **-d** kiezen op opstarten, zodat de containers continu op de achtergrond worden uitgevoerd.


Typ om te controleren dat de containers zijn, `docker-compose ps`. U ziet er ongeveer als:

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

U kunt nu verbinding maken met WordPress rechtstreeks op de virtuele machine op poort 80. Open een webbrowser en voert de DNS-naam van uw virtuele machine (zoals `http://mypublicdns.eastus.cloudapp.azure.com`). U ziet nu de WordPress startscherm, waar u kunt de installatie voltooien en aan de slag met de toepassing.

![WordPress-startscherm][wordpress_start]

## <a name="next-steps"></a>Volgende stappen
* Ga naar de [gebruikershandleiding voor Docker-VM-extensie](https://github.com/Azure/azure-docker-extension/blob/master/README.md) voor meer opties voor het configureren van Docker en Compose in uw Docker-VM. Bijvoorbeeld, is een optie om het opstellen yml-bestand (geconverteerd naar JSON) rechtstreeks in de configuratie van de Docker VM-extensie.
* Bekijk de [Compose opdrachtregelreferentiewebpagina](http://docs.docker.com/compose/reference/) en [gebruikershandleiding](http://docs.docker.com/compose/) voor meer voorbeelden van het bouwen en implementeren van apps met meerdere containers.
* Een Azure Resource Manager-sjabloon gebruiken uw eigen of één hebben bijgedragen uit de [community](https://azure.microsoft.com/documentation/templates/), voor het implementeren van een virtuele Azure-machine met Docker en een toepassing instellen met opstellen. Bijvoorbeeld, de [implementeren van een WordPress-blog met Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) sjabloon maakt gebruik van Docker en Compose WordPress snel implementeren met een MySQL-back-end op een Ubuntu-VM.
* Probeer het integreren van Docker Compose met een Docker Swarm-cluster. Zie [Compose gebruiken met Swarm](https://docs.docker.com/compose/swarm/) voor scenario's.

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
