---
title: Linux Azure VM-installatiekopieën maken met Packer | Microsoft Docs
description: Informatie over het maken van installatiekopieën van virtuele Linux-machines in Azure met Packer
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/03/2018
ms.author: cynthn
ms.openlocfilehash: 3a7ac2e7a86a135f20f46b03be2c38af330a5367
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55730336"
---
# <a name="how-to-use-packer-to-create-linux-virtual-machine-images-in-azure"></a>Hoe u Linux-installatiekopieën voor virtuele machines maken in Azure met Packer
Elke virtuele machine (VM) in Azure is gemaakt op basis van een installatiekopie die u de Linux-distributie en de versie van het besturingssysteem definieert. Installatiekopieën kunnen bevatten vooraf geïnstalleerde toepassingen en configuraties. De Azure Marketplace bevat vele installatiekopieën die eerste en derde partij voor de meest voorkomende distributies en omgevingen met toepassingen, of kunt u uw eigen aangepaste installatiekopieën die zijn afgestemd op uw behoeften. Dit artikel wordt uitgelegd hoe u met het open-source-hulpprogramma [Packer](https://www.packer.io/) om te definiëren en maken van aangepaste installatiekopieën in Azure.


## <a name="create-azure-resource-group"></a>Azure-resourcegroep maken
Tijdens het bouwproces maakt Packer tijdelijke Azure-resources zoals het samenstellen van de bron-VM. Om vast te leggen die bron-VM voor gebruik als een installatiekopie, moet u een resourcegroep definiëren. De uitvoer van het bouwproces Packer wordt opgeslagen in deze resourcegroep.

Maak een resourcegroep maken met [az group create](/cli/azure/group). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create -n myResourceGroup -l eastus
```


## <a name="create-azure-credentials"></a>Azure-referenties maken
Packer verifieert met Azure met behulp van een service-principal. Een Azure service-principal is een beveiligings-id die u met apps, services en automatiseringsprogramma's, zoals Packer gebruiken kunt. U bepaalt en definiëren van de machtigingen over welke bewerkingen die de service-principal in Azure uitvoeren kunt.

Een service-principal met maken [az ad sp create-for-rbac](/cli/azure/ad/sp) en de uitvoer van de referenties die Packer moet:

```azurecli
az ad sp create-for-rbac --query "{ client_id: appId, client_secret: password, tenant_id: tenant }"
```

Een voorbeeld van de uitvoer in de voorgaande opdrachten is als volgt:

```azurecli
{
    "client_id": "f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
    "client_secret": "0e760437-bf34-4aad-9f8d-870be799c55d",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

Om te worden geverifieerd bij Azure, moet u ook uw Azure-abonnement-id met [az account show](/cli/azure/account):

```azurecli
az account show --query "{ subscription_id: id }"
```

U gebruikt de uitvoer van deze twee opdrachten in de volgende stap.


## <a name="define-packer-template"></a>Packer sjabloon definiëren
Als u wilt maken van installatiekopieën, kunt u een sjabloon maken als een JSON-bestand. In de sjabloon definieert u builders en provisioners die het werkelijke bouwproces worden uitgevoerd. Packer heeft een [provisioner voor Azure](https://www.packer.io/docs/builders/azure.html) die kunt u definiëren van Azure-resources, zoals de service principal-referenties die zijn gemaakt in de voorgaande stap.

Maak een bestand met de naam *ubuntu.json* en plak de volgende inhoud. Voer uw eigen waarden voor het volgende:

| Parameter                           | Waar u kunt verkrijgen |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | Eerste regel van de uitvoer van `az ad sp` maakopdracht - *appId* |
| *client_secret*                     | Line-of uitvoer van tweede `az ad sp` maakopdracht - *wachtwoord* |
| *tenant_id*                         | Derde regel van uitvoer van `az ad sp` maakopdracht - *tenant* |
| *subscription_id*                   | Uitvoer van `az account show` opdracht |
| *managed_image_resource_group_name* | Naam van de resourcegroep die u hebt gemaakt in de eerste stap |
| *managed_image_name*                | Naam voor de installatiekopie van het beheerde schijf die is gemaakt |


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

Deze sjabloon maakt een installatiekopie van een Ubuntu 16.04 LTS, NGINX wordt geïnstalleerd en deprovisions van de virtuele machine.

> [!NOTE]
> Als u wilt op de sjabloon de gebruikersreferenties inrichten uitbreiden, aanpassen van de opdracht provisioner die de Azure-agent te lezen deprovisions `-deprovision` in plaats van `deprovision+user`.
> De `+user` vlag verwijdert alle gebruikersaccounts uit de bron-VM.


## <a name="build-packer-image"></a>Packer-installatiekopie bouwen
Als u nog niet geïnstalleerd op uw lokale computer, Packer hebt [volgt u de installatie-instructies Packer](https://www.packer.io/docs/install/index.html).

Bouw de installatiekopie door op te geven uw Packer sjabloonbestand als volgt:

```bash
./packer build ubuntu.json
```

Een voorbeeld van de uitvoer in de voorgaande opdrachten is als volgt:

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

Het duurt een paar minuten voor Packer bouwen van de virtuele machine, het uitvoeren van de provisioners en het opschonen van de implementatie.


## <a name="create-vm-from-azure-image"></a>Virtuele machine maken van installatiekopieën van Azure
U kunt nu een virtuele machine maken door uw installatiekopie met [az vm maken](/cli/azure/vm). Geef de afbeelding die u hebt gemaakt met de `--image` parameter. Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* van *myPackerImage* en worden SSH-sleutels gegenereerd als deze nog niet bestaan:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image myPackerImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

Als u maken van virtuele machines in een andere resourcegroep of een andere regio dan uw Packer-installatiekopie wilt, geeft u de afbeeldings-ID in plaats van de naam van installatiekopie. U vindt de afbeeldings-ID met [az image show](/cli/azure/image#az-image-show).

Het duurt een paar minuten om de VM te maken. Nadat de virtuele machine is gemaakt, noteer de `publicIpAddress` weergegeven door de Azure CLI. Dit adres wordt gebruikt voor toegang tot de NGINX-site via een webbrowser.

Open poort 80 via internet met [az vm open-port](/cli/azure/vm) zodat beveiligd webverkeer uw virtuele machine kan bereiken:

```azurecli
az vm open-port \
    --resource-group myResourceGroup \
    --name myVM \
    --port 80
```

## <a name="test-vm-and-nginx"></a>Testen van de virtuele machine en NGINX
Nu kunt u een webbrowser openen en `http://publicIpAddress` in de adresbalk invoeren. Geef uw eigen openbare IP-adres op uit het creatieproces van de virtuele machine proces. De standaard-NGINX-pagina wordt weergegeven zoals in het volgende voorbeeld:

![Standaardsite van NGINX](./media/build-image-with-packer/nginx.png) 


## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld gebruikt u Packer om u te maken van een installatiekopie van een virtuele machine met NGINX is al geïnstalleerd. U kunt deze VM-installatiekopie samen met bestaande werkstromen voor de implementatie, zoals uw app implementeren in virtuele machines die zijn gemaakt op basis van de installatiekopie met Ansible, Chef en Puppet.

Zie voor aanvullende Packer voorbeeldsjablonen voor andere Linux-distributies, [deze GitHub-opslagplaats](https://github.com/hashicorp/packer/tree/master/examples/azure).
