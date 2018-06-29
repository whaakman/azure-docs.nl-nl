---
title: Installeren en configureren van Ansible voor gebruik met virtuele machines in Azure | Microsoft Docs
description: Meer informatie over het installeren en configureren van Ansible voor het beheren van Azure-resources op Ubuntu en CentOS SLES
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/04/2018
ms.author: iainfou
ms.openlocfilehash: 262c72968ddb5985e0217bced1b83af6fdb7c03c
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052603"
---
# <a name="install-and-configure-ansible-to-manage-virtual-machines-in-azure"></a>Installeren en configureren van Ansible voor het beheren van virtuele machines in Azure

Ansible kunt u de implementatie en configuratie van resources in uw omgeving automatiseren. U kunt Ansible gebruiken voor het beheren van uw virtuele machines (VM's) in Azure, net als elke andere bron dezelfde. Dit artikel wordt uitgelegd hoe u Ansible en de vereiste modules voor de Azure Python SDK installeert voor enkele van de meest voorkomende Linux-distributies. U kunt Ansible installeren op andere distributies door de geïnstalleerde pakketten aanpassen aan uw bepaald platform aan te passen. Voor het maken van Azure-resources op een veilige manier, u ook informatie over het maken en definiëren van referenties voor Ansible te gebruiken.

Zie voor meer informatie over opties voor de installatie en stappen voor extra platforms de [Ansible installatiehandleiding](https://docs.ansible.com/ansible/intro_installation.html).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in dit artikel is vereist dat u de Azure CLI versie 2.0.30 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="install-ansible"></a>Ansible installeren

Een van de eenvoudigste manieren om Ansible met Azure is met de Azure-Cloud-Shell, een browser gebaseerde shell-ervaring te beheren en ontwikkelen van Azure-resources. Ansible vooraf is geïnstalleerd in de Cloud-Shell, zodat u verder instructies over het installeren van Ansible en gaat u naar [maken Azure-referenties](#create-azure-credentials). Zie voor een lijst van extra hulpprogramma's ook beschikbaar in de Cloud-Shell [functies en hulpprogramma's voor Bash in de Azure-Cloud-Shell](../../cloud-shell/features.md#tools).

De volgende instructies hoe u een Linux-VM voor verschillende Distributies maken en installeer vervolgens Ansible. Als u niet nodig hebt voor het maken van een Linux-VM, slaat u deze eerste stap voor het maken van een Azure-resourcegroep. Als u een virtuele machine maken hoeft, maakt u eerst een resourcegroep met [az groep maken](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Nu, selecteer een van de volgende Distributies voor stapsgewijze instructies voor het maken van een virtuele machine, indien nodig, en installeert u Ansible:

- [7.4 centOS](#centos-74)
- [Ubuntu 16.04 TNS](#ubuntu1604-lts)
- [SLES 12 SP2](#sles-12-sp2)

### <a name="centos-74"></a>7.4 centOS

Maak indien nodig met een virtuele machine [az vm maken](/cli/azure/vm#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image OpenLogic:CentOS:7.4:latest \
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

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

Maak indien nodig met een virtuele machine [az vm maken](/cli/azure/vm#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image Canonical:UbuntuServer:16.04-LTS:latest \
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
sudo pip install ansible[azure]
```

Nu wordt verplaatst naar [maken Azure-referenties](#create-azure-credentials).

### <a name="sles-12-sp2"></a>SLES 12 SP2

Maak indien nodig met een virtuele machine [az vm maken](/cli/azure/vm#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image SUSE:SLES:12-SP2:latest \
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

Op de hostcomputer of in de Azure-Cloud-Shell, maakt u een service principal met [az ad sp maken-voor-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). De referenties die Ansible moet worden uitgevoerd op het scherm:

```azurecli-interactive
az ad sp create-for-rbac --query '{"client_id": appId, "secret": password, "tenant": tenant}'
```

Een voorbeeld van de uitvoer van de bovenstaande opdrachten is als volgt:

```json
{
  "client_id": "eec5624a-90f8-4386-8a87-02730b5410d5",
  "secret": "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

Om te verifiëren naar Azure, moet u ook verkrijgen van uw Azure-abonnement-ID met [az account weergeven](/cli/azure/account#az-account-show):

```azurecli-interactive
az account show --query "{ subscription_id: id }"
```

U gebruikt de uitvoer van deze twee opdrachten in de volgende stap.

## <a name="create-ansible-credentials-file"></a>Ansible referentiebestand maken

Om referenties te verstrekken aan Ansible, omgevingsvariabelen definiëren of maak een bestand lokale referenties. Zie voor meer informatie over het definiëren van Ansible referenties [referenties bieden voor Azure Modules](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).

Voor een ontwikkelingsomgeving, maakt u een *referenties* -bestand voor Ansible op de VM-host. Maak een referentiebestand op de virtuele machine waarop u Ansible in de vorige stap hebt geïnstalleerd:

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

Sla het bestand op en sluit het.

## <a name="use-ansible-environment-variables"></a>Omgevingsvariabelen Ansible gebruiken

Als u gebruikmaken van hulpprogramma's zoals Ansible Tower of Jenkins wilt, moet u voor het definiëren van omgevingsvariabelen. Deze stap worden overgeslagen als u alleen gaat de client Ansible gebruiken en het bestand Azure-referenties die zijn gemaakt in de vorige stap. Omgevingsvariabelen definiëren dezelfde informatie als het bestand Azure-referenties:

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=eec5624a-90f8-4386-8a87-02730b5410d5
export AZURE_SECRET=531dcffa-3aff-4488-99bb-4816c395ea3f
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a>Volgende stappen

U hebt nu Ansible en de vereiste Azure Python SDK modules geïnstalleerd en referenties die zijn gedefinieerd voor Ansible te gebruiken. Meer informatie over hoe [een virtuele machine maken met Ansible](ansible-create-vm.md). U kunt ook meer te weten hoe [een volledige Azure-virtuele machine maken en de ondersteunende resources met Ansible](ansible-create-complete-vm.md).
