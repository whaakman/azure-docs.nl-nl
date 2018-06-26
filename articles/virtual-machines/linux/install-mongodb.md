---
title: MongoDB installeren op een Linux-VM met de Azure CLI | Microsoft Docs
description: Meer informatie over het installeren en configureren van MongoDB op een Linux-virtuele machine iusing de Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: a47c0e2f655f51444dc586f696c26caa63ab6cac
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937579"
---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm"></a>Het installeren en configureren van MongoDB op een Linux-VM
[MongoDB](http://www.mongodb.org) is een populaire open-source, hoogwaardige NoSQL-database. In dit artikel leest u hoe installeren en configureren van MongoDB op een Linux-VM met de Azure CLI 2.0. Voorbeelden worden weergegeven dat detail hoe naar:

* [Handmatig installeren en configureren van een eenvoudige MongoDB-exemplaar](#manually-install-and-configure-mongodb-on-a-vm)
* [Een basic MongoDB-exemplaar met een Resource Manager-sjabloon maken](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Een complexe MongoDB shard-cluster met de replica wordt ingesteld met een Resource Manager-sjabloon maken](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Handmatig installeren en configureren van MongoDB op een virtuele machine
MongoDB [installatie-instructies](https://docs.mongodb.com/manual/administration/install-on-linux/) voor Linux-distributies met inbegrip van Red Hat / CentOS, SUSE, Ubuntu en Debian. Het volgende voorbeeld wordt een *CentOS* VM. Voor het maken van deze omgeving, moet u de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/reference-index#az_login).

Maak een resourcegroep maken met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Maak een VM met [az vm create](/cli/azure/vm#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* aan een gebruiker met de naam *azureuser* met behulp van SSH-verificatie voor openbare sleutel

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH naar de virtuele machine met behulp van uw eigen gebruikersnaam en het `publicIpAddress` vermeld in de uitvoer van de vorige stap:

```bash
ssh azureuser@<publicIpAddress>
```

Voor het toevoegen van de installatiebronnen voor MongoDB, maken een **yum** opslagplaatsbestand als volgt:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.6.repo
```

Open het bestand van de opslagplaats MongoDB om te bewerken, zoals met `vi` of `nano`. Voeg de volgende regels:

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

SELinux wordt standaard afgedwongen voor installatiekopieën van CentOS die voorkomen dat u toegang tot MongoDB. Beheerhulpprogramma's voor installeren en configureren van SELinux zodat MongoDB bewerkingen uitvoeren op de standaard TCP-poort 27017 als volgt:

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

Test de MongoDB-exemplaar nu door sommige gegevens toe te voegen en vervolgens te zoeken:

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


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Basic MongoDB-exemplaar op CentOS met een sjabloon maken
U kunt een basic MongoDB-exemplaar maken op één CentOS VM van de volgende Azure quickstart-sjabloon vanuit GitHub. Deze sjabloon wordt de aangepaste scriptextensie voor Linux toe te voegen een **yum** opslagplaats naar de zojuist gemaakte CentOS VM en installeer MongoDB.

* [Basic MongoDB-exemplaar op CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Voor het maken van deze omgeving, moet u de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/reference-index#az_login). Maak eerst een resourcegroep met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vervolgens implementeert u de sjabloon MongoDB met [az implementatie maken](/cli/azure/group/deployment#az_group_deployment_create). Wanneer u wordt gevraagd, typt u uw eigen unieke waarden voor *newStorageAccountName*, *dnsNameForPublicIP*, en gebruikersnaam van de beheerder en het wachtwoord:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

Meld u bij de virtuele machine met behulp van de openbare DNS-adres van uw virtuele machine. U kunt de openbare DNS-serveradres met weergeven [az vm weergeven](/cli/azure/vm#az_vm_show):

```azurecli
az vm show -g myResourceGroup -n myLinuxVM -d --query [fqdns] -o tsv
```

SSH met uw virtuele machine met uw eigen gebruikersnaam en een openbare DNS-adres:

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

De MongoDB-installatie controleren door verbinding te maken met behulp van de lokale `mongo` client als volgt:

```bash
mongo
```

Test nu het exemplaar door sommige gegevens toe te voegen en te zoeken als volgt:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Een complexe MongoDB Shard-Cluster op CentOS met een sjabloon maken
U kunt een complexe MongoDB shard cluster met behulp van de volgende Azure quickstart-sjabloon vanuit GitHub maken. Deze sjabloon volgt de [MongoDB shard cluster aanbevolen procedures](https://docs.mongodb.com/manual/core/sharded-cluster-components/) voor redundantie en hoge beschikbaarheid. De sjabloon maakt twee shards met drie knooppunten replicaset. Een config server replicaset met drie knooppunten ook wordt gemaakt, plus twee **mongos** router servers om te voorzien in consistentie van toepassingen uit de shards.

* [MongoDB Sharding-Cluster op CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Dit complexe MongoDB shard-cluster moet de implementatie meer dan 20 kernen, dit is doorgaans de standaardinstelling core-telling per regio voor een abonnement. Open een aanvraag voor ondersteuning van Azure te verhogen van het aantal kernen.

Voor het maken van deze omgeving, moet u de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/reference-index#az_login). Maak eerst een resourcegroep met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vervolgens implementeert u de sjabloon MongoDB met [az implementatie maken](/cli/azure/group/deployment#az_group_deployment_create). Definieer uw eigen resource namen en waar nodig zoals als voor groottes *mongoAdminUsername*, *sizeOfDataDiskInGB*, en *configNodeVmSize*:

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

Deze implementatie kan ruim een uur implementeren en configureren van alle VM-instanties duren. De `--no-wait` vlag besturingselement terugkeren naar de opdrachtprompt wanneer de sjabloonimplementatie is geaccepteerd door de Azure-platform aan het einde van de voorgaande opdracht wordt gebruikt. Vervolgens kunt u de status van de implementatie met bekijken [az groep implementatie weergeven](/cli/azure/group/deployment#az_group_deployment_show). Het volgende voorbeeld bekijkt de status van de *myMongoDBCluster* -implementatie in de *myResourceGroup* resourcegroep:

```azurecli
az group deployment show \
    --resource-group myResourceGroup \
    --name myMongoDBCluster \
    --query [properties.provisioningState] \
    --output tsv
```

## <a name="next-steps"></a>Volgende stappen
In deze voorbeelden verbinding u met de MongoDB-exemplaar lokaal van de virtuele machine. Als u wilt verbinding maken met de MongoDB-exemplaar van een andere virtuele machine of een netwerk, zorg ervoor dat de juiste [Netwerkbeveiligingsgroep regels zijn gemaakt](nsg-quickstart.md).

Deze voorbeelden implementeren de core MongoDB-omgeving voor ontwikkelingsdoeleinden. De vereiste configuratie-opties voor uw omgeving van toepassing. Zie voor meer informatie de [MongoDB beveiliging docs](https://docs.mongodb.com/manual/security/).

Zie voor meer informatie over het maken van sjablonen gebruiken de [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

De Azure Resource Manager-sjablonen gebruiken de aangepaste Scriptextensie om te downloaden en uitvoeren van scripts op uw virtuele machines. Zie voor meer informatie [met behulp van de Azure-extensie voor aangepaste scripts met virtuele Linux-Machines](extensions-customscript.md).

