---
title: Schijven op een Linux-VM in Azure versleutelen | Microsoft Docs
description: Versleutelen van virtuele schijven op een Linux-VM voor verbeterde beveiliging met de Azure CLI
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/30/2018
ms.author: cynthn
ms.openlocfilehash: 2a057c311a8b080b695492664df0eaabc5609b35
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856788"
---
# <a name="how-to-encrypt-a-linux-virtual-machine-in-azure"></a>Versleutelen van een virtuele Linux-machine in Azure

Voor uitgebreide virtuele machine (VM) beveiliging en naleving, kunnen virtuele schijven en de virtuele machine zelf worden versleuteld. VM's zijn versleuteld met behulp van cryptografische sleutels die worden beveiligd in een Azure Key Vault. U bepaalt deze cryptografische sleutels en het gebruik ervan kunt controleren. Dit artikel wordt uitgelegd hoe u voor het versleutelen van virtuele schijven op een Linux-VM met de Azure CLI. 

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell starten

Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. 

Als u Cloud Shell wilt openen, selecteert u **Proberen** in de rechterbovenhoek van een codeblok. U kunt Cloud Shell ook openen in een afzonderlijk browsertabblad door naar [https://shell.azure.com/bash](https://shell.azure.com/bash) te gaan. Klik op **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en druk vervolgens op Enter om de code uit te voeren.

Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, in dit artikel is vereist dat u de Azure CLI versie 2.0.30 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="overview-of-disk-encryption"></a>Overzicht van schijfversleuteling
Virtuele schijven op Linux-VM's zijn versleuteld met behulp van rest [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Er zijn geen kosten voor het versleutelen van virtuele schijven in Azure. Cryptografische sleutels worden opgeslagen in Azure Key Vault met behulp van software-beveiliging, of u kunt importeren of genereer uw sleutels in Hardware Security Modules (HSM's) gecertificeerd voor FIPS 140-2 level 2 standaarden. U behoudt de controle van deze cryptografische sleutels en het gebruik ervan kunt controleren. Deze cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven die zijn gekoppeld aan uw virtuele machine. 

Het proces voor het versleutelen van een virtuele machine is als volgt:

1. Maak een cryptografische sleutel in een Azure Key Vault.
1. Configureer de cryptografische sleutel om te worden gebruikt voor het versleutelen van schijven.
1. Schijfversleuteling voor de virtuele schijven inschakelen.
1. De vereiste cryptografische sleutels worden aangevraagd vanuit Azure Key Vault.
1. De virtuele schijven worden versleuteld met behulp van de cryptografiesleutel van de opgegeven.


## <a name="requirements-and-limitations"></a>Vereisten en beperkingen
Ondersteunde scenario's en vereisten voor versleuteling van schijf:

* De volgende Linux-server SKU's - Ubuntu, CentOS, SUSE en SUSE Linux Enterprise Server (SLES) en Red Hat Enterprise Linux.
* Alle resources (zoals Key Vault, een Storage-account en een virtuele machine) moeten zich in dezelfde Azure-regio en -abonnement.
* Standard A, D, DS, G, GS, enz., serie VM's.
* Bijwerken van de cryptografische sleutels op een Linux VM die al is versleuteld.

Schijfversleuteling wordt momenteel niet ondersteund in de volgende scenario's:

* Basic-laag virtuele machines.
* Virtuele machines die zijn gemaakt met het klassieke implementatiemodel.
* Het uitschakelen van OS-schijfversleuteling voor de virtuele Linux-machines.
* Het gebruik van aangepaste Linux-installatiekopieën.

Zie voor meer informatie over ondersteunde scenario's en beperkingen [Azure Disk Encryption voor IaaS-VM's](../../security/azure-security-disk-encryption.md)


## <a name="create-an-azure-key-vault-and-keys"></a>Een Azure Key Vault en sleutels maken
In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld-parameternamen bevatten *myResourceGroup*, *myKey*, en *myVM*.

De eerste stap is het maken van een Azure Key Vault voor het opslaan van uw cryptografische sleutels. Azure Key Vault kunt opslaan, sleutels, geheimen of wachtwoorden waarmee u kunt ze veilig in uw toepassingen en services te implementeren. Voor de versleuteling van de virtuele schijf, kunt u Key Vault gebruiken voor het opslaan van een cryptografische sleutel die wordt gebruikt om te versleutelen of ontsleutelen van de virtuele schijven.

Inschakelen van de provider Azure Key Vault binnen uw Azure-abonnement met [az provider register](/cli/azure/provider#az-provider-register) en maak een resourcegroep met [az-groep maken](/cli/azure/group#az-group-create). Het volgende voorbeeld wordt een Resourcegroepnaam *myResourceGroup* in de `eastus` locatie:

```azurecli-interactive
az provider register -n Microsoft.KeyVault
resourcegroup="myResourceGroup"
az group create --name $resourcegroup --location eastus
```

De Azure Key Vault met de cryptografische sleutels en de bijbehorende rekenresources, zoals opslag en de virtuele machine zelf moet zich in dezelfde regio bevinden. Maak een Azure Key Vault met [az keyvault maken](/cli/azure/keyvault#az-keyvault-create) en de Key Vault voor gebruik met schijfversleuteling in te schakelen. Geef een unieke naam van de Key Vault voor *keyvault_name* als volgt:

```azurecli-interactive
keyvault_name=myvaultname$RANDOM
az keyvault create \
    --name $keyvault_name \
    --resource-group $resourcegroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

U kunt de cryptografische sleutels met behulp van software of Hardware Security Model (HSM) beveiliging opslaan. Met behulp van een HSM, is een premium Key Vault vereist. Er is een extra kosten voor het maken van een premium Key Vault in plaats van standard Key Vault waarin software beschermde sleutels. Voor het maken van een premium Key Vault, in de vorige stap toevoegen `--sku Premium` aan de opdracht. Het volgende voorbeeld wordt met software beschermde sleutels nadat u een standard-Sleutelkluis hebt gemaakt.

Voor beide beveiligingsmodellen moet het Azure-platform worden gemachtigd om aan te vragen van de cryptografische sleutels wanneer de virtuele machine wordt opgestart voor het ontsleutelen van de virtuele schijven. Maken van een cryptografische sleutel in uw Key Vault met [az keyvault key maken](/cli/azure/keyvault/key#az-keyvault-key-create). Het volgende voorbeeld wordt een sleutel met de naam *myKey*:

```azurecli-interactive
az keyvault key create \
    --vault-name $keyvault_name \
    --name myKey \
    --protection software
```


## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Maak een VM met [az vm maken](/cli/azure/vm#az-vm-create) en het koppelen van een schijf van 5 Gb gegevens. Alleen bepaalde marketplace-installatiekopieën ondersteuning voor schijfversleuteling. Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met behulp van een *Ubuntu 16.04 LTS* installatiekopie:

```azurecli-interactive
az vm create \
    --resource-group $resourcegroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH naar uw virtuele machine met de *publicIpAddress* wordt weergegeven in de uitvoer van de voorgaande opdracht. Maken van een partitie en bestandssysteem, en vervolgens de gegevensschijf koppelen. Zie voor meer informatie, [verbinding maken met een Linux-VM om te koppelen van de nieuwe schijf](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Sluit de SSH-sessie.


## <a name="encrypt-the-virtual-machine"></a>De virtuele machine versleutelen


Versleutelen van uw virtuele machine met [az vm encryption inschakelen](/cli/azure/vm/encryption#az-vm-encryption-enable). Het volgende voorbeeld wordt de *$sp_id* en *$sp_password* variabelen in de voorgaande [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) opdracht:

```azurecli-interactive
az vm encryption enable \
    --resource-group $resourcegroup \
    --name myVM \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

Het duurt even voordat het versleutelingsproces schijf om te voltooien. De status van het proces met [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group $resourcegroup --name myVM --query 'status'
```

Als u klaar bent, is de uitvoer ziet er vergelijkbaar met het volgende voorbeeld:

```json
[
  {
    "code": "ProvisioningState/succeeded",
    "displayStatus": "Provisioning succeeded",
    "level": "Info",
    "message": "Encryption succeeded for all volumes",
    "time": null
  }
]
```


## <a name="add-additional-data-disks"></a>Extra gegevensschijven toevoegen
Nadat u de gegevensschijven van uw zijn versleuteld, kunt u extra virtuele schijven toevoegen aan uw virtuele machine en versleutelen. 

Zodra de gegevensschijf is toegevoegd aan de virtuele machine, moet u de opdracht voor het versleutelen van de virtuele schijven als volgt opnieuw uitvoeren:

```azurecli-interactive
az vm encryption enable \
    --resource-group $resourcegroup \
    --name myVM \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type data
```


## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het beheren van Azure Key Vault, met inbegrip van verwijderen van cryptografische sleutels en kluizen, [Key Vault beheren met behulp van CLI](../../key-vault/key-vault-manage-with-cli2.md).
* Zie voor meer informatie over schijfversleuteling, zoals het voorbereiden van een versleutelde aangepaste VM uploaden naar Azure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
