---
title: MongoDB installeren op een Linux-VM met behulp van de Azure CLI 1.0 | Microsoft Docs
description: Informatie over het installeren en configureren van MongoDB op een virtuele Linux-machine in Azure met behulp van het Resource Manager-implementatiemodel.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: c97ade0a3d95824f723aad55776de861fe49441f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm-using-the-azure-cli-10"></a>Het installeren en configureren van MongoDB op een Linux-VM met behulp van de Azure CLI 1.0
[MongoDB](http://www.mongodb.org) is een populaire open-source, hoogwaardige NoSQL-database. In dit artikel leest u hoe installeren en configureren van MongoDB op een Linux VM in Azure met het implementatiemodel van Resource Manager. Voorbeelden worden weergegeven dat detail hoe naar:

* [Handmatig installeren en configureren van een eenvoudige MongoDB-exemplaar](#manually-install-and-configure-mongodb-on-a-vm)
* [Een basic MongoDB-exemplaar met een Resource Manager-sjabloon maken](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Een complexe MongoDB shard-cluster met de replica wordt ingesteld met een Resource Manager-sjabloon maken](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- Azure CLI 1.0: onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel (dit artikel)
- [Azure CLI 2.0](create-cli-complete-nodejs.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Handmatig installeren en configureren van MongoDB op een virtuele machine
MongoDB [installatie-instructies](https://docs.mongodb.com/manual/administration/install-on-linux/) voor Linux-distributies met inbegrip van Red Hat / CentOS, SUSE, Ubuntu en Debian. Het volgende voorbeeld wordt een *CentOS* VM die gebruikmaakt van een SSH-sleutel die is opgeslagen op *~/.ssh/id_rsa.pub*. Beantwoord de aanwijzingen voor de opslagaccountnaam, DNS-naam en beheerdersreferenties:

```azurecli
azure vm quick-create \
    --image-urn CentOS \
    --ssh-publickey-file ~/.ssh/id_rsa.pub 
```

Aanmelden bij de virtuele machine met behulp van het openbare IP-adres weergegeven aan het einde van de vorige stap van de VM-maken:

```bash
ssh azureuser@40.78.23.145
```

Voor het toevoegen van de installatiebronnen voor MongoDB, maken een **yum** opslagplaatsbestand als volgt:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

Open het bestand van de opslagplaats MongoDB om te bewerken. Voeg de volgende regels:

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

Installeren met behulp van MongoDB **yum** als volgt:

```bash
sudo yum install -y mongodb-org
```

SELinux wordt standaard afgedwongen voor installatiekopieën van CentOS die voorkomen dat u toegang tot MongoDB. Beheerhulpprogramma's voor installeren en configureren van SELinux zodat MongoDB bewerkingen uitvoeren op de standaard TCP-poort 27017 als volgt. 

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
U kunt een basic MongoDB-exemplaar maken op één CentOS VM van de volgende Azure quickstart-sjabloon vanuit GitHub. Deze sjabloon wordt de aangepaste scriptextensie voor Linux toe te voegen een `yum` opslagplaats naar de zojuist gemaakte CentOS VM en installeer MongoDB.

* [Basic MongoDB-exemplaar op CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Het volgende voorbeeld maakt u een resourcegroep met de naam `myResourceGroup` in de `eastus` regio. Voer uw eigen waarden als volgt:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [!NOTE]
> De Azure CLI keert u terug naar een prompt binnen een paar seconden van de implementatie wordt gemaakt, maar de installatie en configuratie duurt een paar minuten. Controleer de status van de implementatie met de `azure group deployment show myResourceGroup`, de naam van de resourcegroep dienovereenkomstig in te voeren. Wacht totdat de **ProvisioningState** toont *geslaagd* voordat u probeert te SSH naar de virtuele machine.

Zodra de implementatie voltooid, SSH naar de virtuele machine is. Het IP-adres van uw virtuele machine met de `azure vm show` opdracht zoals in het volgende voorbeeld:

```azurecli
azure vm show --resource-group myResourceGroup --name myLinuxVM
```

Het openbare IP-adres wordt aan het einde van de uitvoer weergegeven. SSH met uw virtuele machine met het IP-adres van uw virtuele machine:

```bash
ssh azureuser@138.91.149.74
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

* [MongoDB Sharding-Cluster op CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Dit complexe MongoDB shard-cluster moet de implementatie meer dan 20 kernen, dit is doorgaans de standaardinstelling core-telling per regio voor een abonnement. Open een aanvraag voor ondersteuning van Azure te verhogen van het aantal kernen.

Het volgende voorbeeld maakt u een resourcegroep met de naam *myResourceGroup* in de *eastus* regio. Voer uw eigen waarden als volgt:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [!NOTE]
> De Azure CLI keert u terug naar een prompt binnen een paar seconden van de implementatie wordt gemaakt, maar de installatie en configuratie kunnen ruim een uur duren voltooien. Controleer de status van de implementatie met de `azure group deployment show myResourceGroup`, de naam van de resourcegroep dienovereenkomstig aanpassen. Wacht totdat de **ProvisioningState** toont *geslaagd* voordat u verbinding maakt met de VM's.


## <a name="next-steps"></a>Volgende stappen
In deze voorbeelden verbinding u met de MongoDB-exemplaar lokaal van de virtuele machine. Als u wilt verbinding maken met de MongoDB-exemplaar van een andere virtuele machine of een netwerk, zorg ervoor dat de juiste [Netwerkbeveiligingsgroep regels zijn gemaakt](nsg-quickstart.md).

Zie voor meer informatie over het maken van sjablonen gebruiken de [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

De Azure Resource Manager-sjablonen gebruiken de aangepaste Scriptextensie om te downloaden en uitvoeren van scripts op uw virtuele machines. Zie voor meer informatie [met behulp van de Azure-extensie voor aangepaste scripts met virtuele Linux-Machines](extensions-customscript.md).

