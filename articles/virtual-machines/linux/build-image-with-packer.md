---
title: "Het maken van Linux Azure VM-installatiekopieën met verpakker | Microsoft Docs"
description: "Informatie over het gebruik van verpakker installatiekopieën maken van virtuele Linux-machines in Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/13/2017
ms.author: iainfou
ms.openlocfilehash: d548d3df209df2a9ae8fa3f8ee684190bc140175
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-use-packer-to-create-linux-virtual-machine-images-in-azure"></a>Het gebruik van verpakker voor het maken van installatiekopieën van Linux virtuele machines in Azure
Elke virtuele machine (VM) in Azure wordt gemaakt van een installatiekopie die u de Linux-distributie en de versie van het besturingssysteem definieert. Voorbeelden van afbeeldingen zijn vooraf geïnstalleerde toepassingen en configuraties. Azure Marketplace bevat veel installatiekopieën van het eerste en derde partij voor het meest voorkomende distributies en omgevingen met toepassingen, of kunt u uw eigen aangepaste installatiekopieën die zijn afgestemd op uw behoeften. Dit artikel wordt uitgelegd hoe u de open-source hulpprogramma [verpakker](https://www.packer.io/) om te definiëren en te maken van aangepaste installatiekopieën in Azure.


## <a name="create-azure-resource-group"></a>Azure-resourcegroep maken
Tijdens het maken maakt verpakker tijdelijke Azure-resources als de bron-VM-builds. Als u wilt vastleggen die bron-VM voor gebruik als een installatiekopie, moet u een resourcegroep definiëren. De uitvoer van het buildproces verpakker wordt opgeslagen in deze resourcegroep.

Maak een resourcegroep maken met [az group create](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *eastus* locatie:

```azurecli
az group create -n myResourceGroup -l eastus
```


## <a name="create-azure-credentials"></a>Azure-referenties maken
Verpakker verifieert met Azure met behulp van een service-principal. Een Azure-service-principal is een beveiligings-id die u met apps, services en automatiseringsprogramma's zoals verpakker kunt gebruiken. U de machtigingen over welke bewerkingen die de service-principal in Azure uitvoeren kunt te definiëren en beheren.

Maken van een service principal met [az ad sp maken-voor-rbac](/cli/azure/ad/sp#create-for-rbac) en de uitvoer van de referenties die verpakker moet:

```azurecli
az ad sp create-for-rbac --query "{ client_id: appId, client_secret: password, tenant_id: tenant }"
```

Een voorbeeld van de uitvoer van de bovenstaande opdrachten is als volgt:

```azurecli
{
    "client_id": "f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
    "client_secret": "0e760437-bf34-4aad-9f8d-870be799c55d",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

Om te verifiëren naar Azure, moet u ook uw Azure-abonnement-id met [az account weergeven](/cli/azure/account#show):

```azurecli
az account show --query "{ subscription_id: id }"
```

U gebruikt de uitvoer van deze twee opdrachten in de volgende stap.


## <a name="define-packer-template"></a>Verpakker sjabloon definiëren
Als u wilt maken van installatiekopieën, kunt u een sjabloon maken als een JSON-bestand. In de sjabloon definieert u opbouwfuncties en provisioners die het werkelijke buildproces uitvoeren. Verpakker heeft een [provisioner voor Azure](https://www.packer.io/docs/builders/azure.html) die kunt u voor het definiëren van de Azure-resources, zoals de Servicereferenties principal gemaakt in de voorgaande stap.

Maak een bestand met de naam *ubuntu.json* en plak de volgende inhoud. Geef uw eigen waarden voor het volgende:

| Parameter                           | Waar u kunt verkrijgen |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | Eerste regel van de uitvoer van `az ad sp` maken command - *appId* |
| *client_secret*                     | Lijn van uitvoer van de tweede `az ad sp` maken command - *wachtwoord* |
| *tenant_id*                         | Derde regel van uitvoer van `az ad sp` maken command - *tenant* |
| *subscription_id*                   | De uitvoer van `az account show` opdracht |
| *managed_image_resource_group_name* | Naam van resourcegroep die u in de eerste stap hebt gemaakt |
| *managed_image_name*                | Naam voor de installatiekopie van de beheerde schijf die is gemaakt |


```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
    "client_secret": "0e760437-bf34-4aad-9f8d-870be799c55d",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Linux",
    "image_publisher": "Canonical",
    "image_offer": "UbuntuServer",
    "image_sku": "16.04-LTS",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "execute_command": "chmod +x {{ .Path }}; {{ .Vars }} sudo -E sh '{{ .Path }}'",
    "inline": [
      "apt-get update",
      "apt-get upgrade -y",
      "apt-get -y install nginx",

      "/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync"
    ],
    "inline_shebang": "/bin/sh -x",
    "type": "shell"
  }]
}
```

Deze sjabloon maakt een installatiekopie van een virtuele Ubuntu 16.04 TNS, NGINX installeert en vervolgens deprovisions van de virtuele machine.

> [!NOTE]
> Als u van deze sjabloon op inrichten gebruikersreferenties wilt uitbreiden, past u de provisioner-opdracht die de Azure-agent te lezen deprovisions `-deprovision` plaats `deprovision+user`.
> De `+user` vlag verwijdert alle gebruikersaccounts uit de bron-VM.


## <a name="build-packer-image"></a>Verpakker installatiekopie maken
Als u nog niet geïnstalleerd op uw lokale machine verpakker hebt [Volg de instructies van de installatie verpakker](https://www.packer.io/docs/install/index.html).

Maak de installatiekopie door op te geven van uw verpakker sjabloonbestand als volgt:

```bash
./packer build ubuntu.json
```

Een voorbeeld van de uitvoer van de bovenstaande opdrachten is als volgt:

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> dept : Engineering
==> azure-arm:  ->> task : Image deployment
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Getting the VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.218.147’
==> azure-arm: Waiting for SSH to become available...
==> azure-arm: Connected to SSH!
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-shell868574263
    azure-arm: WARNING! The waagent service will be stopped.
    azure-arm: WARNING! Cached DHCP leases will be deleted.
    azure-arm: WARNING! root password will be disabled. You will not be able to login as root.
    azure-arm: WARNING! /etc/resolvconf/resolv.conf.d/tail and /etc/resolvconf/resolv.conf.d/original will be deleted.
    azure-arm: WARNING! packer account and entire home directory will be deleted.
==> azure-arm: Querying the machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-swtxmqm7ly/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Compute Name              : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm: Deleting the temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. The artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```

Het duurt enkele minuten duren voordat verpakker bouwen van de virtuele machine, het uitvoeren van de provisioners en het opschonen van de implementatie.


## <a name="create-vm-from-azure-image"></a>Virtuele machine van de afbeelding voor Azure maken
U kunt nu een virtuele machine maken van de installatiekopie met [az vm maken](/cli/azure/vm#create). Geef de afbeelding die u hebt gemaakt met de `--image` parameter. Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* van *myPackerImage* en SSH-sleutels genereert als deze niet al bestaan:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image myPackerImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

Het duurt enkele minuten voor het maken van de virtuele machine. Wanneer de virtuele machine is gemaakt, dient u de `publicIpAddress` weergegeven door de Azure CLI. Dit adres wordt gebruikt voor toegang tot de NGINX-site via een webbrowser.

Open poort 80 via Internet met zodat webverkeer bereiken van uw virtuele machine [az vm open poort](/cli/azure/vm#open-port):

```azurecli
az vm open-port \
    --resource-group myResourceGroup \
    --name myVM \
    --port 80
```

## <a name="test-vm-and-nginx"></a>Virtuele machine en NGINX testen
Nu kunt u een webbrowser openen en voer `http://publicIpAddress` in de adresbalk. Geef uw eigen openbare IP-adres van de virtuele machine proces maken. De standaard NGINX-pagina wordt weergegeven zoals in het volgende voorbeeld:

![Standaardsite van NGINX](./media/build-image-with-packer/nginx.png) 


## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld gebruikt u verpakker maken van een VM-installatiekopie met NGINX al is geïnstalleerd. U kunt deze VM-installatiekopie samen met bestaande werkstromen voor de implementatie, zoals uw app implementeren in virtuele machines die zijn gemaakt op basis van de installatiekopie met Ansible, Chef of Puppet.

Zie voor aanvullende voorbeeld verpakker sjablonen voor andere Linux-distributies, [deze GitHub-repo-](https://github.com/hashicorp/packer/tree/master/examples/azure).