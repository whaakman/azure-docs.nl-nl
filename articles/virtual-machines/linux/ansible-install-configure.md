---
title: Installeren en configureren van Ansible voor gebruik met virtuele machines in Azure | Microsoft Docs
description: Meer informatie over het installeren en configureren van Ansible voor het beheren van Azure-resources op Ubuntu en CentOS SLES
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/18/2017
ms.author: iainfou
ms.openlocfilehash: 13b043f3d6154852647f6bb738d3717be6802fa9
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="install-and-configure-ansible-to-manage-virtual-machines-in-azure"></a>Installeren en configureren van Ansible voor het beheren van virtuele machines in Azure
Dit artikel wordt uitgelegd hoe u Ansible en de vereiste modules voor de Azure Python SDK installeert voor enkele van de meest voorkomende Linux-distributies. U kunt Ansible installeren op andere distributies door de geïnstalleerde pakketten aanpassen aan uw bepaald platform aan te passen. Voor het maken van Azure-resources op een veilige manier, u ook informatie over het maken en definiëren van referenties voor Ansible te gebruiken. 

Zie voor meer informatie over opties voor de installatie en stappen voor extra platforms de [Ansible installatiehandleiding](https://docs.ansible.com/ansible/intro_installation.html).


## <a name="install-ansible"></a>Ansible installeren
Maak eerst een resourcegroep met [az groep maken](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupAnsible* in de *eastus* locatie:

```azurecli
az group create --name myResourceGroupAnsible --location eastus
```

Nu een virtuele machine maken en installeer Ansible voor een van de volgende distributies van uw keuze:

- [Ubuntu 16.04 TNS](#ubuntu1604-lts)
- [7.3 centOS](#centos-73)
- [SLES 12 SP2](#sles-12-sp2)

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS
Maak een VM met [az vm create](/cli/azure/vm#create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH naar uw virtuele machine met de `publicIpAddress` vermeld in de uitvoer van de virtuele machine opnieuw te maken:

```bash
ssh azureuser@<publicIpAddress>
```

Op de virtuele machine, moet u de vereiste pakketten voor de Azure Python SDK-modules en Ansible als volgt installeren:

```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Ansible and Azure SDKs via pip
pip install ansible[azure]
```

Nu wordt verplaatst naar [maken Azure-referenties](#create-azure-credentials).


### <a name="centos-73"></a>7.3 centOS
Maak een VM met [az vm create](/cli/azure/vm#create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH naar uw virtuele machine met de `publicIpAddress` vermeld in de uitvoer van de virtuele machine opnieuw te maken:

```bash
ssh azureuser@<publicIpAddress>
```

Op de virtuele machine, moet u de vereiste pakketten voor de Azure Python SDK-modules en Ansible als volgt installeren:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Nu wordt verplaatst naar [maken Azure-referenties](#create-azure-credentials).


### <a name="sles-12-sp2"></a>SLES 12 SP2
Maak een VM met [az vm create](/cli/azure/vm#create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image SLES \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH naar uw virtuele machine met de `publicIpAddress` vermeld in de uitvoer van de virtuele machine opnieuw te maken:

```bash
ssh azureuser@<publicIpAddress>
```

Op de virtuele machine, moet u de vereiste pakketten voor de Azure Python SDK-modules en Ansible als volgt installeren:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 make \
    python-devel libopenssl-devel libtool python-pip python-setuptools

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]

# Remove conflicting Python cryptography package
sudo pip uninstall -y cryptography
```

Nu wordt verplaatst naar [maken Azure-referenties](#create-azure-credentials).


## <a name="create-azure-credentials"></a>Azure-referenties maken
Ansible communiceert met Azure met behulp van een gebruikersnaam en wachtwoord of een service-principal. Een Azure-service-principal is een beveiligings-id die u met apps, services en automatiseringsprogramma's zoals Ansible gebruiken kunt. U de machtigingen over welke bewerkingen die de service-principal in Azure uitvoeren kunt te definiëren en beheren. Voor betere beveiliging via gewoon een gebruikersnaam en wachtwoord voor het bieden, in dit voorbeeld wordt een basic service principal.

Een service-principal maken op de hostcomputer met [az ad sp maken-voor-rbac](/cli/azure/ad/sp#create-for-rbac) en de uitvoer van de referenties die Ansible moet:

```azurecli
az ad sp create-for-rbac --query [client_id: appId, secret: password, tenant: tenant]
```

Een voorbeeld van de uitvoer van de bovenstaande opdrachten is als volgt:

```json
{
  "client_id": "eec5624a-90f8-4386-8a87-02730b5410d5",
  "secret": "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

Om te verifiëren naar Azure, moet u ook uw Azure-abonnement-id met [az account weergeven](/cli/azure/account#show):

```azurecli
az account show --query "{ subscription_id: id }"
```

U gebruikt de uitvoer van deze twee opdrachten in de volgende stap.


## <a name="create-ansible-credentials-file"></a>Ansible referentiebestand maken
Om referenties te verstrekken aan Ansible, omgevingsvariabelen definiëren of maak een bestand lokale referenties. Zie voor meer informatie over het definiëren van Ansible referenties [referenties bieden voor Azure Modules](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules). 

Voor een ontwikkelingsomgeving, maakt u een *referenties* voor Ansible bestand op de VM-host als volgt:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

De *referenties* bestand zelf combineert de abonnements-ID met de uitvoer van een service-principal maken. De uitvoer van de vorige [az ad sp maken-voor-rbac](/cli/azure/ad/sp#create-for-rbac) opdracht is hetzelfde als die nodig zijn voor *client_id*, *geheim*, en *tenant*. Het volgende voorbeeld *referenties* bestand deze waarden die overeenkomen met de vorige uitvoer wordt weergegeven. Voer uw eigen waarden als volgt:

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=eec5624a-90f8-4386-8a87-02730b5410d5
secret=531dcffa-3aff-4488-99bb-4816c395ea3f
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```


## <a name="use-ansible-environment-variables"></a>Omgevingsvariabelen Ansible gebruiken
Als u gebruikmaken van hulpprogramma's zoals Ansible Tower of Jenkins wilt, kunt u als volgt omgevingsvariabelen definiëren. Deze variabelen worden de abonnements-ID gecombineerd met de uitvoer van een service-principal maken. De uitvoer van de vorige [az ad sp maken-voor-rbac](/cli/azure/ad/sp#create-for-rbac) opdracht wordt dezelfde volgorde als nodig is voor *AZURE_CLIENT_ID*, *AZURE_SECRET*, en *AZURE_TENANT* . 

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=eec5624a-90f8-4386-8a87-02730b5410d5
export AZURE_SECRET=531dcffa-3aff-4488-99bb-4816c395ea3f
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a>Volgende stappen
U hebt nu Ansible en de vereiste Azure Python SDK modules geïnstalleerd en referenties die zijn gedefinieerd voor Ansible te gebruiken. Meer informatie over hoe [een virtuele machine maken met Ansible](ansible-create-vm.md). U kunt ook meer te weten hoe [een volledige Azure-virtuele machine maken en de ondersteunende resources met Ansible](ansible-create-complete-vm.md).
