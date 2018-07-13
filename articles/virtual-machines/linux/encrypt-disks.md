---
title: Schijven op een Linux-VM in Azure versleutelen | Microsoft Docs
description: Versleutelen van virtuele schijven op een Linux-VM voor verbeterde beveiliging met behulp van de Azure CLI 2.0
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
ms.date: 05/31/2018
ms.author: cynthn
ms.openlocfilehash: 75ec087536d6f833a9a2106b1fdf4ed1fd73ef8e
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38634617"
---
# <a name="how-to-encrypt-a-linux-virtual-machine-in-azure"></a>Versleutelen van een virtuele Linux-machine in Azure
Voor uitgebreide virtuele machine (VM) beveiliging en naleving, kunnen virtuele schijven en de virtuele machine zelf worden versleuteld. VM's zijn versleuteld met behulp van cryptografische sleutels die worden beveiligd in een Azure Key Vault. U bepaalt deze cryptografische sleutels en het gebruik ervan kunt controleren. Dit artikel wordt uitgelegd hoe u voor het versleutelen van virtuele schijven op een Linux-VM met de Azure CLI 2.0. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, in dit artikel is vereist dat u de Azure CLI versie 2.0.30 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="overview-of-disk-encryption"></a>Overzicht van schijfversleuteling
Virtuele schijven op Linux-VM's zijn versleuteld met behulp van rest [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Er zijn geen kosten voor het versleutelen van virtuele schijven in Azure. Cryptografische sleutels worden opgeslagen in Azure Key Vault met behulp van software-beveiliging, of u kunt importeren of genereer uw sleutels in Hardware Security Modules (HSM's) gecertificeerd voor FIPS 140-2 level 2 standaarden. U behoudt de controle van deze cryptografische sleutels en het gebruik ervan kunt controleren. Deze cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven die zijn gekoppeld aan uw virtuele machine. Een Azure Active Directory service-principal biedt een veilige mechanisme voor het uitgeven van deze cryptografische sleutels als VM's worden aangestuurd in of uit.

Het proces voor het versleutelen van een virtuele machine is als volgt:

1. Maak een cryptografische sleutel in een Azure Key Vault.
2. Configureer de cryptografische sleutel om te worden gebruikt voor het versleutelen van schijven.
3. Als u wilt lezen de cryptografische sleutel uit de Azure Key Vault, een Azure Active Directory-service-principal maken met de juiste machtigingen.
4. De opdracht voor het versleutelen van de virtuele schijven, het opgeven van de Azure Active Directory service principal en de juiste cryptografische sleutel moet worden gebruikt.
5. De service-principal voor Azure Active Directory vraagt de cryptografiesleutel van de vereiste van Azure Key Vault.
6. De virtuele schijven worden versleuteld met behulp van de cryptografiesleutel van de opgegeven.

## <a name="encryption-process"></a>Versleutelingsproces
Schijfversleuteling is afhankelijk van de volgende extra onderdelen:

* **Azure Key Vault** : wordt gebruikt om cryptografische sleutels en geheimen die worden gebruikt voor het proces van de versleuteling/ontsleuteling schijf te beveiligen.
  * Als er een bestaat, kunt u een bestaande Azure Key Vault. U hoeft niet te reserveren van een Key Vault voor het versleutelen van schijven.
  * Afzonderlijke administratieve grenzen en de zichtbaarheid van de sleutel, kunt u een toegewezen Sleutelkluis maken.
* **Azure Active Directory** -het veilig uitwisselen van vereiste cryptografische sleutels en de verificatiegegevens voor de aangevraagde acties worden verwerkt.
  * Doorgaans kunt u een bestaand Azure Active Directory-exemplaar waarin uw toepassing.
  * De service-principal biedt een veilige mechanisme voor het aanvragen en de juiste cryptografische sleutels worden uitgegeven. U niet een werkelijke toepassing ontwikkelt die kan worden geïntegreerd met Azure Active Directory.

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
az group create --name myResourceGroup --location eastus
```

De Azure Key Vault met de cryptografische sleutels en de bijbehorende rekenresources, zoals opslag en de virtuele machine zelf moet zich in dezelfde regio bevinden. Maak een Azure Key Vault met [az keyvault maken](/cli/azure/keyvault#az-keyvault-create) en de Key Vault voor gebruik met schijfversleuteling in te schakelen. Geef een unieke naam van de Key Vault voor *keyvault_name* als volgt:

```azurecli-interactive
keyvault_name=myuniquekeyvaultname
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

U kunt de cryptografische sleutels met behulp van software of Hardware Security Model (HSM) beveiliging opslaan. Met behulp van een HSM, is een premium Key Vault vereist. Er is een extra kosten voor het maken van een premium Key Vault in plaats van standard Key Vault waarin software beschermde sleutels. Voor het maken van een premium Key Vault, in de vorige stap toevoegen `--sku Premium` aan de opdracht. Het volgende voorbeeld wordt met software beschermde sleutels nadat u een standard-Sleutelkluis hebt gemaakt.

Voor beide beveiligingsmodellen moet het Azure-platform worden gemachtigd om aan te vragen van de cryptografische sleutels wanneer de virtuele machine wordt opgestart voor het ontsleutelen van de virtuele schijven. Maken van een cryptografische sleutel in uw Key Vault met [az keyvault key maken](/cli/azure/keyvault/key#az-keyvault-key-create). Het volgende voorbeeld wordt een sleutel met de naam *myKey*:

```azurecli-interactive
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-an-azure-active-directory-service-principal"></a>Een service-principal voor Azure Active Directory maken
Wanneer virtuele schijven worden versleuteld of ontsleuteld, geeft u een account voor het afhandelen van de verificatie en uitwisselen van cryptografische sleutels uit Key Vault. Dit account, een service-principal van Azure Active Directory kunt het Azure-platform om aan te vragen van de juiste cryptografische sleutels namens de virtuele machine. Een standaardexemplaar van de Azure Active Directory is beschikbaar in uw abonnement, hoewel veel organisaties zijn speciaal bestemd voor Azure Active Directory-mappen.

Maken van een service-principal met behulp van Azure Active Directory met [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). Het volgende voorbeeld leest de waarden voor de service-principal en het wachtwoord voor gebruik in latere opdrachten:

```azurecli-interactive
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

Het wachtwoord wordt alleen weergegeven wanneer u de service-principal maken. Indien gewenst, weergeven en noteert u het wachtwoord (`echo $sp_password`). U kunt uw service-principals met een lijst [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list) en extra informatie weergeven over een specifieke service-principal met [az ad sp show](/cli/azure/ad/sp#az-ad-sp-show).

Als u wilt versleutelen of ontsleutelen van virtuele schijven, moeten machtigingen voor de cryptografische sleutel opgeslagen in Key Vault om toe te staan van de service-principal voor Azure Active Directory te lezen van de sleutels worden ingesteld. Machtigingen instellen voor uw Key Vault met [az keyvault-beleid instellen](/cli/azure/keyvault#az-keyvault-set-policy). In het volgende voorbeeld wordt de service-principal-ID opgegeven in de voorgaande opdracht:

```azurecli-interactive
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Maak een VM met [az vm maken](/cli/azure/vm#az-vm-create) en het koppelen van een schijf van 5 Gb gegevens. Alleen bepaalde marketplace-installatiekopieën ondersteuning voor schijfversleuteling. Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met behulp van een *Ubuntu 16.04 LTS* installatiekopie:

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH naar uw virtuele machine met de *publicIpAddress* wordt weergegeven in de uitvoer van de voorgaande opdracht. Maken van een partitie en bestandssysteem, en vervolgens de gegevensschijf koppelen. Zie voor meer informatie, [verbinding maken met een Linux-VM om te koppelen van de nieuwe schijf](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Sluit de SSH-sessie.


## <a name="encrypt-the-virtual-machine"></a>De virtuele machine versleutelen
Voor het versleutelen van de virtuele schijven, breng u samen de eerdere onderdelen:

1. De service-principal voor Azure Active Directory en het wachtwoord opgeven.
2. Geef de Key Vault voor het opslaan van de metagegevens van de versleutelde schijven.
3. Geef de cryptografische sleutels te gebruiken voor de daadwerkelijke versleuteling en ontsleuteling.
4. Geef op of u wilt voor het versleutelen van de besturingssysteemschijf, schijven of alle.

Versleutelen van uw virtuele machine met [az vm encryption inschakelen](/cli/azure/vm/encryption#az-vm-encryption-enable). Het volgende voorbeeld wordt de *$sp_id* en *$sp_password* variabelen in de voorgaande [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) opdracht:

```azurecli-interactive
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

Het duurt even voordat het versleutelingsproces schijf om te voltooien. De status van het proces met [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group myResourceGroup --name myVM
```

De uitvoer is vergelijkbaar met het volgende voorbeeld afgekapt:

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress"
]
```

Wacht totdat de status van het besturingssysteem schijf rapporten **VMRestartPending**, start de virtuele machine met [az vm restart](/cli/azure/vm#az-vm-restart):

```azurecli-interactive
az vm restart --resource-group myResourceGroup --name myVM
```

Het versleutelingsproces schijf tijdens het opstarten is voltooid, dus wacht een paar minuten voordat u controleert de status van versleuteling opnieuw met [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group myResourceGroup --name myVM
```

De status moet nu rapporteren de besturingssysteemschijf en de gegevensschijf als **versleutelde**.


## <a name="add-additional-data-disks"></a>Extra gegevensschijven toevoegen
Nadat u de gegevensschijven van uw zijn versleuteld, kunt u later aanvullende virtuele schijven toevoegen aan uw virtuele machine en ook versleuteld. U kunt bijvoorbeeld een tweede virtuele schijf toevoegen aan uw virtuele machine als volgt:

```azurecli-interactive
az vm disk attach \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --disk myDataDisk \
    --new \
    --size-gb 5
```

Voer de opdracht voor het versleutelen van de virtuele schijven als volgt:

```azurecli-interactive
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```


## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het beheren van Azure Key Vault, met inbegrip van verwijderen van cryptografische sleutels en kluizen, [Key Vault beheren met behulp van CLI](../../key-vault/key-vault-manage-with-cli2.md).
* Zie voor meer informatie over schijfversleuteling, zoals het voorbereiden van een versleutelde aangepaste VM uploaden naar Azure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
