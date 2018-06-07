---
title: Schijven op een Linux VM in Azure versleutelen | Microsoft Docs
description: Het coderen van virtuele schijven op een Linux-VM voor verbeterde beveiliging op basis van de Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2017
ms.author: iainfou
ms.openlocfilehash: c35cd220eab26300404a039467e1a0b35592f23d
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34824775"
---
# <a name="how-to-encrypt-virtual-disks-on-a-linux-vm"></a>Het coderen van virtuele schijven op een Linux-VM
Voor de uitgebreide virtuele machine (VM) beveiliging en naleving, kunnen virtuele schijven en de virtuele machine zelf worden versleuteld. Virtuele machines zijn versleuteld met behulp van de cryptografische sleutels die worden beveiligd in een Azure Sleutelkluis. U kunt het gebruik ervan controleren en beheren van deze cryptografische sleutels. Dit artikel wordt uitgelegd hoe u voor het versleutelen van virtuele schijven op een Linux-VM met de Azure CLI 2.0. U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> Zorg ervoor dat u het vfat stuurprogramma ingeschakeld in uw Linux-VM hebt. Enkele beveiligingsaspecten procedures zoals configuratie-items benchmarks vraag om het stuurprogramma vfat uitschakelen. Dat is vereist voor de versleuteling te werken nadat het proces is voltooid.

## <a name="quick-commands"></a>Snelle opdrachten
Als u nodig hebt voor de taak, de volgende sectie details snel de base-opdrachten voor het versleutelen van virtuele schijven op de virtuele machine. Meer gedetailleerde informatie en context voor elke stap u de rest van het document vindt [vanaf hier](#overview-of-disk-encryption).

U moet de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/reference-index#az_login). In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. De namen van de voorbeeld-parameter *myResourceGroup*, *myKey*, en *myVM*.

De provider voor Azure Sleutelkluis in uw Azure-abonnement met eerst inschakelen [az provider registreren](/cli/azure/provider#az_provider_register) en maak een resourcegroep met [az groep maken](/cli/azure/group#az_group_create). Het volgende voorbeeld wordt een Resourcegroepnaam *myResourceGroup* in de *eastus* locatie:

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

Maken van een Azure Key Vault met [az keyvault maken](/cli/azure/keyvault#az_keyvault_create) en inschakelen van de Sleutelkluis voor gebruik met schijfversleuteling. Geef een unieke naam van de Sleutelkluis voor *keyvault_name* als volgt:

```azurecli
keyvault_name=myuniquekeyvaultname
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

Maken van een cryptografische sleutel in uw Sleutelkluis met [az keyvault-sleutel maken](/cli/azure/keyvault/key#az_keyvault_key_create). Het volgende voorbeeld wordt een sleutel met de naam *myKey*:

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```

Maken van een service-principal met Azure Active Directory met [az ad sp maken-voor-rbac](/cli/azure/ad/sp#az_ad_sp_create_for_rbac). De service-principal verwerkt de verificatie en de uitwisseling van de cryptografische sleutels uit Sleutelkluis. Het volgende voorbeeld wordt gelezen in de waarden voor de service-principal-ID en het wachtwoord voor gebruik in latere opdrachten:

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

Het wachtwoord wordt alleen uitgevoerd wanneer u de service-principal maken. Indien gewenst, weergeven en vastleggen van het wachtwoord (`echo $sp_password`). U kunt uw service-principals met lijst [az ad sp lijst](/cli/azure/ad/sp#az_ad_sp_list) en aanvullende informatie weergeven over een specifieke service-principal met [az ad sp weergeven](/cli/azure/ad/sp#az_ad_sp_show).

Machtigingen instellen voor uw Sleutelkluis met [az keyvault-beleid instellen](/cli/azure/keyvault#az_keyvault_set_policy). In het volgende voorbeeld wordt de service-principal-ID opgegeven in de voorgaande opdracht:

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
    --key-permissions wrapKey \
    --secret-permissions set
```

Maak een VM met [az vm maken](/cli/azure/vm#az_vm_create) en koppelt u een gegevensschijf 5 Gb. Alleen bepaalde marketplace-installatiekopieën ondersteuning bieden voor schijfversleuteling. Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met behulp van een *CentOS 7.2n* afbeelding:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH naar uw virtuele machine met de *publicIpAddress* wordt weergegeven in de uitvoer van de voorgaande opdracht. Maken van een partitie en het bestandssysteem en vervolgens de gegevensschijf koppelen. Zie voor meer informatie [verbinding maken met een Linux-VM te koppelen van de nieuwe schijf](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Sluit uw SSH-sessie.

Versleutelen van uw virtuele machine met [az vm versleuteling inschakelen](/cli/azure/vm/encryption#az_vm_encryption_enable). Het volgende voorbeeld wordt de *$sp_id* en *$sp_password* variabelen in de voorgaande `ad sp create-for-rbac` opdracht:

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

Het duurt enige tijd voor het versleutelingsproces schijf om te voltooien. De status van het proces met [az vm versleuteling weergeven](/cli/azure/vm/encryption#az_vm_encryption_show):

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

De status ziet **EncryptionInProgress**. Wacht totdat de status van het besturingssysteem schijf rapporten **VMRestartPending**, start u uw virtuele machine met [az vm opnieuw](/cli/azure/vm#az_vm_restart):

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

Het versleutelingsproces schijf tijdens het opstarten is voltooid, dus wacht een paar minuten voordat u controleert de status van versleuteling opnieuw met [az vm versleuteling weergeven](/cli/azure/vm/encryption#az_vm_encryption_show):

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

De status moet nu rapporteren de besturingssysteemschijf en de gegevensschijf als **versleutelde**.


## <a name="overview-of-disk-encryption"></a>Overzicht van schijfversleuteling
Virtuele schijven op virtuele Linux-machines zijn versleuteld met behulp van rest [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Er zijn geen kosten voor het versleutelen van virtuele schijven in Azure. Cryptografische sleutels worden opgeslagen in Azure Key Vault met software-beveiliging, of u kunt importeren of genereren van uw sleutels in Hardware Security Modules (HSM's) die is gecertificeerd voor FIPS 140-2 level 2-standaarden. U beheer behouden over deze cryptografische sleutels en het gebruik ervan kunt controleren. Deze cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven gekoppeld aan uw virtuele machine. Een Azure Active Directory service-principal biedt een veilige mechanisme voor het uitgeven van deze cryptografische sleutels als virtuele machines van stroom in- of uitschakelen voorzien worden.

Het proces voor het versleutelen van een virtuele machine is als volgt:

1. Maak een cryptografische sleutel in een Azure Sleutelkluis.
2. Configureer de cryptografische sleutel om te worden gebruikt voor het versleutelen van schijven.
3. De cryptografische sleutel om uit te lezen Azure Sleutelkluis, maak een Azure Active Directory service principal met de juiste machtigingen.
4. Geef de opdracht voor het versleutelen van uw virtuele schijven, opgeven van de Azure Active Directory service principal en de juiste cryptografische sleutel moet worden gebruikt.
5. De Azure Active Directory-service-principal vraagt de cryptografiesleutel van de vereiste van Azure Sleutelkluis.
6. De virtuele schijven worden versleuteld met behulp van de cryptografiesleutel van de opgegeven.

## <a name="encryption-process"></a>Versleutelingsproces
Schijfversleuteling is afhankelijk van de volgende extra onderdelen:

* **Azure Sleutelkluis** : wordt gebruikt ter bescherming van de cryptografische sleutels en geheimen gebruikt voor het proces van de codering/decodering schijf.
  * Als dit bestaat, kunt u een bestaande Azure Sleutelkluis. U hoeft niet te reserveren van een Sleutelkluis voor het versleutelen van schijven.
  * Afzonderlijke administratieve grenzen en sleutel zichtbaarheid, kunt u een speciale Sleutelkluis.
* **Azure Active Directory** -het veilig uitwisselen van vereiste cryptografische sleutels en verificatie voor de aangevraagde acties worden verwerkt.
  * Doorgaans kunt u een bestaand Azure Active Directory-exemplaar waarin de toepassing.
  * De service-principal biedt een veilige mechanisme om te vragen en de juiste cryptografische sleutels worden uitgegeven. U ontwikkelt een werkelijke toepassing die is geïntegreerd met Azure Active Directory niet.

## <a name="requirements-and-limitations"></a>Vereisten en beperkingen
Ondersteunde scenario's en vereisten voor de schijfversleuteling:

* De volgende Linux-server SKU's - Ubuntu, CentOS, SUSE en SUSE Linux Enterprise Server (SLES) en Red Hat Enterprise Linux.
* Alle resources (zoals Sleutelkluis, een opslagaccount en een VM) moeten zich in dezelfde Azure-regio en abonnement.
* Standard A, D, DS, G, GS, enz., reeks virtuele machines.
* Bijwerken van de cryptografische sleutels op een Linux-VM al is versleuteld.
* VFAT stuurprogramma is ingeschakeld in de Linux-VM.

Schijfversleuteling is momenteel niet ondersteund in de volgende scenario's:

* Basisstaffel virtuele machines.
* Virtuele machines gemaakt met behulp van het klassieke implementatiemodel.
* OS-schijfversleuteling op Linux VM's uitschakelen.
* Gebruik van aangepaste Linux-installatiekopieën.

Zie voor meer informatie over ondersteunde scenario's en beperkingen [Azure Disk Encryption voor IaaS VM's](../../security/azure-security-disk-encryption.md)


## <a name="create-azure-key-vault-and-keys"></a>Azure Sleutelkluis en de sleutels maken
U moet de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/reference-index#az_login). In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. De namen van de voorbeeld-parameter *myResourceGroup*, *myKey*, en *myVM*.

De eerste stap is het maken van een Azure Key Vault voor het opslaan van uw cryptografische sleutels. Azure Sleutelkluis kunt opslaan sleutels, geheimen of wachtwoorden waarmee u kunt ze veilig implementeert in uw toepassingen en services. Voor versleuteling van de virtuele schijf, kunt u Sleutelkluis gebruiken voor het opslaan van een cryptografische sleutel die wordt gebruikt voor het versleutelen of ontsleutelen van de virtuele schijven.

Inschakelen van de provider voor Azure Sleutelkluis in uw Azure-abonnement met [az provider registreren](/cli/azure/provider#az_provider_register) en maak een resourcegroep met [az groep maken](/cli/azure/group#az_group_create). Het volgende voorbeeld wordt een Resourcegroepnaam *myResourceGroup* in de `eastus` locatie:

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

De Azure Sleutelkluis die de cryptografische sleutels en de bijbehorende compute-bronnen zoals opslag en de virtuele machine zelf moet zich bevinden in dezelfde regio. Maken van een Azure Key Vault met [az keyvault maken](/cli/azure/keyvault#az_keyvault_create) en inschakelen van de Sleutelkluis voor gebruik met schijfversleuteling. Geef een unieke naam van de Sleutelkluis voor *keyvault_name* als volgt:

```azurecli
keyvault_name=myuniquekeyvaultname
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

U kunt met behulp van software of Hardware Security Model (HSM) protection cryptografiesleutels opslaan. Een HSM, moet een premium Sleutelkluis. Er is een extra kosten voor het maken van een premium Sleutelkluis in plaats van standaard Sleutelkluis waarmee softwarematige beveiligde sleutels worden opgeslagen. Voor het maken van een premium Sleutelkluis in de vorige stap toevoegen `--sku Premium` aan de opdracht. Het volgende voorbeeld wordt de softwarematige beveiligde sleutels nadat u een standaard Sleutelkluis hebt gemaakt.

Voor beide beveiligingsmodellen moet het Azure-platform toegang om aan te vragen van de cryptografische sleutels wanneer de virtuele machine wordt opgestart voor het ontsleutelen van de virtuele schijven worden verleend. Maken van een cryptografische sleutel in uw Sleutelkluis met [az keyvault-sleutel maken](/cli/azure/keyvault/key#az_keyvault_key_create). Het volgende voorbeeld wordt een sleutel met de naam *myKey*:

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-the-azure-active-directory-service-principal"></a>De Azure Active Directory-service-principal maken
Wanneer virtuele schijven worden versleuteld en ontsleuteld, kunt u een account voor het afhandelen van de verificatie- en uitwisselen van cryptografische sleutels uit Sleutelkluis opgeven. Dit account, een Azure Active Directory service-principal kunt de Azure-platform om aan te vragen van de juiste cryptografische sleutels namens de virtuele machine. Een standaardexemplaar van de Azure Active Directory is beschikbaar in uw abonnement, hoewel veel organisaties hebben speciale mappen van Azure Active Directory.

Maken van een service-principal met Azure Active Directory met [az ad sp maken-voor-rbac](/cli/azure/ad/sp#az_ad_sp_create_for_rbac). Het volgende voorbeeld wordt gelezen in de waarden voor de service-principal-Id en wachtwoord voor gebruik in latere opdrachten:

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

Het wachtwoord wordt alleen weergegeven wanneer u de service-principal maken. Indien gewenst, weergeven en vastleggen van het wachtwoord (`echo $sp_password`). U kunt uw service-principals met lijst [az ad sp lijst](/cli/azure/ad/sp#az_ad_sp_list) en aanvullende informatie weergeven over een specifieke service-principal met [az ad sp weergeven](/cli/azure/ad/sp#az_ad_sp_show).

Om deel te versleutelen of ontsleutelen van virtuele schijven, moeten de machtigingen voor de cryptografische sleutel die is opgeslagen in de Sleutelkluis toe te staan van de Azure Active Directory-service-principal te lezen van de sleutels worden ingesteld. Machtigingen instellen voor uw Sleutelkluis met [az keyvault-beleid instellen](/cli/azure/keyvault#az_keyvault_set_policy). In het volgende voorbeeld wordt de service-principal-ID opgegeven in de voorgaande opdracht:

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-virtual-machine"></a>Virtuele machine maken
Maak een VM voor het versleutelen met [az vm maken](/cli/azure/vm#az_vm_create) en koppelt u een gegevensschijf 5 Gb. Alleen bepaalde marketplace-installatiekopieën ondersteuning bieden voor schijfversleuteling. Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met behulp van een *CentOS 7.2n* afbeelding:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH naar uw virtuele machine met de *publicIpAddress* wordt weergegeven in de uitvoer van de voorgaande opdracht. Maken van een partitie en het bestandssysteem en vervolgens de gegevensschijf koppelen. Zie voor meer informatie [verbinding maken met een Linux-VM te koppelen van de nieuwe schijf](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Sluit uw SSH-sessie.


## <a name="encrypt-virtual-machine"></a>Virtuele machine versleutelen
Voor het versleutelen van de virtuele schijven, brengt u samen de eerdere onderdelen:

1. Geef de Azure Active Directory-service-principal en het wachtwoord.
2. Geef de Sleutelkluis voor het opslaan van de metagegevens van de versleutelde schijven.
3. Geef de cryptografische sleutels moet worden gebruikt voor de daadwerkelijke versleuteling en ontsleuteling.
4. Geef op of u wilt versleutelen schijf met het besturingssysteem, de gegevensschijven of alle.

Versleutelen van uw virtuele machine met [az vm versleuteling inschakelen](/cli/azure/vm/encryption#az_vm_encryption_enable). Het volgende voorbeeld wordt de *$sp_id* en *$sp_password* variabelen in de voorgaande [az ad sp maken-voor-rbac](/cli/azure/ad/sp#az_ad_sp_create_for_rbac) opdracht:

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

Het duurt enige tijd voor het versleutelingsproces schijf om te voltooien. De status van het proces met [az vm versleuteling weergeven](/cli/azure/vm/encryption#az_vm_encryption_show):

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

De uitvoer is vergelijkbaar met het volgende voorbeeld ingekorte:

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress",
]
```

Wacht totdat de status van het besturingssysteem schijf rapporten **VMRestartPending**, start u uw virtuele machine met [az vm opnieuw](/cli/azure/vm#az_vm_restart):

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

Het versleutelingsproces schijf tijdens het opstarten is voltooid, dus wacht een paar minuten voordat u controleert de status van versleuteling opnieuw met **az vm versleuteling weergeven**:

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

De status moet nu rapporteren de besturingssysteemschijf en de gegevensschijf als **versleutelde**.


## <a name="add-additional-data-disks"></a>Toevoegen van extra gegevensschijven
Nadat u uw gegevensschijven hebt versleuteld, kunt u later extra virtuele schijven toevoegen aan uw virtuele machine en ook versleutelen. U kunt bijvoorbeeld een tweede virtuele schijf toevoegen aan uw virtuele machine als volgt:

```azurecli
az vm disk attach-new --resource-group myResourceGroup --vm-name myVM --size-in-gb 5
```

Voer de opdracht voor het versleutelen van de virtuele schijven als volgt:

```azurecli
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
* Zie voor meer informatie over het beheren van Azure Sleutelkluis, met inbegrip van verwijderen van de cryptografische sleutels en kluizen, [Key Vault beheren met CLI](../../key-vault/key-vault-manage-with-cli2.md).
* Zie voor meer informatie over schijfversleuteling, zoals het voorbereiden van een versleutelde aangepaste VM uploaden naar Azure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
