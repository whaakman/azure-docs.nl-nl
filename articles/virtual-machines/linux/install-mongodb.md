---
title: MongoDB op een virtuele Linux-machine met de Azure CLI installeren | Microsoft Docs
description: Meer informatie over het installeren en configureren van MongoDB op een Linux-virtuele machine isvg gebruiken de Azure CLI
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: cc786b97c38c115fb288a5b9a0f73f566777e7df
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55734416"
---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm"></a>Installeren en configureren van MongoDB op een Linux VM

[MongoDB](http://www.mongodb.org) is een populaire open-source, hoogwaardige NoSQL-database. Dit artikel leest u hoe het installeren en configureren van MongoDB op een Linux-VM met de Azure CLI. Voorbeelden hoe die gegevens worden weergegeven aan:

* [Handmatig installeren en configureren van een eenvoudige MongoDB-exemplaar](#manually-install-and-configure-mongodb-on-a-vm)
* [Een eenvoudige MongoDB-exemplaar met behulp van Resource Manager-sjabloon maken](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Een complexe MongoDB shard cluster met de replica wordt ingesteld met behulp van Resource Manager-sjabloon maken](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Handmatig installeren en configureren van MongoDB op een virtuele machine
MongoDB [installatie-instructies bieden](https://docs.mongodb.com/manual/administration/install-on-linux/) voor Linux-distributies zoals Red Hat / CentOS, SUSE, Ubuntu en Debian. Het volgende voorbeeld wordt een *CentOS* VM. Voor het maken van deze omgeving, moet u de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index).

Maak een resourcegroep maken met [az group create](/cli/azure/group). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Maak een VM met [az vm create](/cli/azure/vm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met een gebruiker met de naam *azureuser* met behulp van SSH-verificatie voor openbare sleutel

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH naar de virtuele machine met behulp van uw eigen gebruikersnaam en het `publicIpAddress` die worden vermeld in de uitvoer van de vorige stap:

```bash
ssh azureuser@<publicIpAddress>
```

Als u wilt toevoegen de installatiebronnen voor MongoDB, maakt u een **yum** opslagplaatsbestand als volgt te werk:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.6.repo
```

Open het bestand van de MongoDB-opslagplaats om te bewerken, zoals met `vi` of `nano`. Voeg de volgende regels:

```sh
[mongodb-org-3.6]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.6/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc
```

Installeren met behulp van MongoDB **yum** als volgt:

```bash
sudo yum install -y mongodb-org
```

SELinux wordt standaard afgedwongen op CentOS-installatiekopieën die voorkomt dat u toegang tot MongoDB. Hulpprogramma's voor Groepsbeleidsbeheer installeren en configureren van SELinux om toe te staan van MongoDB naar de standaard-TCP-poort 27017 als volgt uitgevoerd:

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Start de MongoDB-service als volgt:

```bash
sudo service mongod start
```

De MongoDB-installatie controleren door verbinding te maken met behulp van de lokale `mongo` client:

```bash
mongo
```

Test nu de MongoDB-exemplaar dat door sommige gegevens toe te voegen en vervolgens te zoeken:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Indien gewenst, configureert u MongoDB op automatisch starten tijdens het systeem opnieuw is opgestart:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Basic MongoDB-exemplaar op CentOS met behulp van een sjabloon maken
U kunt een eenvoudige MongoDB-exemplaar maken op een enkele CentOS-virtuele machine met behulp van de volgende Azure-snelstartsjabloon van GitHub. Deze sjabloon maakt gebruik van de aangepaste scriptextensie voor Linux om toe te voegen een **yum** opslagplaats naar uw nieuwe virtuele machine en CentOS en installeer MongoDB.

* [Basic MongoDB-exemplaar op CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Voor het maken van deze omgeving, moet u de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index). Maak eerst een resourcegroep met [az group create](/cli/azure/group). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vervolgens implementeert u de sjabloon MongoDB met [az group deployment maken](/cli/azure/group/deployment). Wanneer u wordt gevraagd, voert u uw eigen unieke waarden voor *newStorageAccountName*, *dnsNameForPublicIP*, en gebruikersnaam van beheerder en het wachtwoord:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

Meld u op de virtuele machine met behulp van de openbare DNS-adres van uw virtuele machine. U vindt het openbare DNS-adres met [az vm show](/cli/azure/vm):

```azurecli
az vm show -g myResourceGroup -n myLinuxVM -d --query [fqdns] -o tsv
```

SSH naar uw virtuele machine via uw eigen gebruikersnaam en openbare DNS-adres:

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

De MongoDB-installatie controleren door verbinding te maken met behulp van de lokale `mongo` client als volgt te werk:

```bash
mongo
```

Test nu de instantie door sommige gegevens toe te voegen en te zoeken als volgt:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Een complexe Shard MongoDB-Cluster maken in CentOS met behulp van een sjabloon
U kunt een complexe MongoDB shard cluster met behulp van de volgende Azure-snelstartsjabloon van GitHub kunt maken. Deze sjabloon volgt de [aanbevolen procedures voor MongoDB-shard-cluster](https://docs.mongodb.com/manual/core/sharded-cluster-components/) om redundantie en hoge beschikbaarheid te bieden. De sjabloon maakt u twee shards met drie knooppunten in elke replicaset. Een config replica van de server instellen met drie knooppunten ook wordt gemaakt, plus twee **mongos** router servers voor het bieden van consistentie in toepassingen over de shards.

* [MongoDB Sharding-Cluster in CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Implementeren van deze complexe shard MongoDB-cluster moet meer dan 20 kernen, dit is doorgaans het standaardaantal core per regio voor een abonnement. Open een Azure-ondersteuningsaanvraag voor het verhogen van het aantal kernen.

Voor het maken van deze omgeving, moet u de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index). Maak eerst een resourcegroep met [az group create](/cli/azure/group). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vervolgens implementeert u de sjabloon MongoDB met [az group deployment maken](/cli/azure/group/deployment). Definieer uw eigen resource computernamen en waar nodig deze als voor de grootte *mongoAdminUsername*, *sizeOfDataDiskInGB*, en *configNodeVmSize*:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "mongoAdminUsername": {"value": "mongoadmin"},
    "mongoAdminPassword": {"value": "P@ssw0rd!"},
    "dnsNamePrefix": {"value": "mypublicdns"},
    "environment": {"value": "AzureCloud"},
    "numDataDisks": {"value": "4"},
    "sizeOfDataDiskInGB": {"value": 20},
    "centOsVersion": {"value": "7.0"},
    "routerNodeVmSize": {"value": "Standard_DS3_v2"},
    "configNodeVmSize": {"value": "Standard_DS3_v2"},
    "replicaNodeVmSize": {"value": "Standard_DS3_v2"},
    "zabbixServerIPAddress": {"value": "Null"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json \
  --name myMongoDBCluster \
  --no-wait
```

Deze implementatie kan overnemen van een uur om te implementeren en configureren van alle VM-exemplaren. De `--no-wait` vlag aan het einde van de voorgaande opdracht wordt gebruikt om terug te keren besturingselement naar de opdrachtprompt zodra de sjabloonimplementatie is geaccepteerd door de Azure-platform. Vervolgens kunt u de status van de implementatie met bekijken [az group deployment show](/cli/azure/group/deployment). Het volgende voorbeeld bekijkt de status van de *myMongoDBCluster* implementatie in de *myResourceGroup* resourcegroep:

```azurecli
az group deployment show \
    --resource-group myResourceGroup \
    --name myMongoDBCluster \
    --query [properties.provisioningState] \
    --output tsv
```

## <a name="next-steps"></a>Volgende stappen
In deze voorbeelden verbinding u met de MongoDB-exemplaar dat lokaal van de virtuele machine. Als u verbinding maken met de MongoDB-exemplaar dat vanaf een andere virtuele machine of het netwerk wilt, controleert u of de juiste [netwerkbeveiligingsgroepsregels worden gemaakt](nsg-quickstart.md).

Deze voorbeelden implementeren de core MongoDB-omgeving voor ontwikkelingsdoeleinden. De vereiste configuratie-opties voor uw omgeving van toepassing. Zie voor meer informatie de [MongoDB security docs](https://docs.mongodb.com/manual/security/).

Zie voor meer informatie over het maken van met behulp van sjablonen, de [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

De Azure Resource Manager-sjablonen gebruiken de aangepaste Scriptextensie om te downloaden en uitvoeren van scripts op uw virtuele machines. Zie voor meer informatie, [met behulp van de aangepaste Scriptextensie van Azure met virtuele Linux-Machines](extensions-customscript.md).

