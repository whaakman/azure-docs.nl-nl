---
title: Toegang tot het netwerk beperken tot bronnen in PaaS - Azure CLI | Microsoft Docs
description: In dit artikel leert u hoe u en beperken van toegang tot het netwerk voor Azure-resources, zoals Azure Storage en Azure SQL Database met de service-eindpunten van een virtueel netwerk met de Azure CLI.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want only resources in a virtual network subnet to access an Azure PaaS resource, such as an Azure Storage account.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: f357861a7a44b249e06f091a8693b7f2d8dd5178
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
ms.locfileid: "30841976"
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-cli"></a>Toegang tot het netwerk beperken tot PaaS-resources met de service-eindpunten van een virtueel netwerk met de Azure CLI

Service-eindpunten voor virtueel netwerk kunnen u de netwerktoegang tot bepaalde bronnen Azure-service naar een virtueel netwerksubnet beperken. U kunt ook internettoegang tot de resources verwijderen. Service-eindpunten bieden rechtstreekse verbinding met uw virtuele netwerk met de ondersteunde Azure-services, zodat u kunt persoonlijke adresruimte van het virtuele netwerk gebruiken voor toegang tot de Azure-services. Verkeer dat is bestemd voor Azure-resources via de service-eindpunten altijd blijft op de Microsoft Azure-backbone-netwerk. In dit artikel leert u hoe:

* Een virtueel netwerk maken met één subnet
* Toevoegen van een subnet en een service-eindpunt inschakelen
* Een Azure-resource maken en toegang tot het netwerk laat van alleen een subnet
* Een virtuele machine (VM) implementeren voor elk subnet
* Toegang tot een resource van een subnet bevestigen
* Controleer de toegang is geweigerd tot een bron van een subnet en het internet

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, is deze snelstartgids vereist dat u de Azure CLI versie 2.0.28 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Voordat u een virtueel netwerk maakt, moet u maken van een resourcegroep voor het virtuele netwerk en andere resources die zijn gemaakt in dit artikel. Maak een resourcegroep maken met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive
az group create \
  --name myResourceGroup \
  --location eastus
```

Een virtueel netwerk maken met één subnet met [az network vnet maken](/cli/azure/network/vnet#az_network_vnet_create).

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

## <a name="enable-a-service-endpoint"></a>Een service-eindpunt inschakelen 

U kunt de service-eindpunten die alleen voor services die ondersteuning bieden voor service-eindpunten inschakelen. Service-eindpunt diensten beschikbaar weergeven in een Azure-locatie met [az network vnet lijst-eindpunt-services](/cli/azure/network/vnet#az_network_vnet_list_endpoint_services). Het volgende voorbeeld wordt een lijst met services-service-eindpunt-functionaliteit beschikbaar zijn in de *eastus* regio. Naar meer Azure-services service-eindpunt is ingeschakeld worden, wordt de lijst met services die worden geretourneerd gedurende een periode, uitgebreid.

```azurecli-interactive
az network vnet list-endpoint-services \
  --location eastus \
  --out table
``` 

Maken van een ander subnet in het virtuele netwerk met [az network vnet subnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). In dit voorbeeld wordt een service-eindpunt voor *Microsoft.Storage* voor het subnet wordt gemaakt: 

```azurecli-interactive
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24 \
  --service-endpoints Microsoft.Storage
```

## <a name="restrict-network-access-for-a-subnet"></a>Beperken van toegang tot het netwerk voor een subnet

Maken van een netwerkbeveiligingsgroep met [az netwerk nsg maken](/cli/azure/network/nsg#az_network_nsg_create). Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNsgPrivate*.

```azurecli-interactive
az network nsg create \
  --resource-group myResourceGroup \
  --name myNsgPrivate
```

Koppelen van de netwerkbeveiligingsgroep voor de *persoonlijke* subnet met [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update). Het volgende voorbeeld wordt de *myNsgPrivate* netwerkbeveiligingsgroep voor de *persoonlijke* subnet:

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Private \
  --resource-group myResourceGroup \
  --network-security-group myNsgPrivate
```

Maken van de beveiligingsregels voor verbindingen met [az netwerk nsg regel maken](/cli/azure/network/nsg/rule#az_network_nsg_rule_create). De regel die volgt kunt uitgaande toegang tot het openbare IP-adressen toegewezen aan de Azure Storage-service: 

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Allow-Storage-All \
  --access Allow \
  --protocol "*" \
  --direction Outbound \
  --priority 100 \
  --source-address-prefix "VirtualNetwork" \
  --source-port-range "*" \
  --destination-address-prefix "Storage" \
  --destination-port-range "*"

Each network security group contains several [default security rules](security-overview.md#default-security-rules). The rule that follows overrides a default security rule that allows outbound access to all public IP addresses. The `destination-address-prefix "Internet"` option denies outbound access to all public IP addresses. The previous rule overrides this rule, due to its higher priority, which allows access to the public IP addresses of Azure Storage.

az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Deny-Internet-All \
  --access Deny \
  --protocol "*" \
  --direction Outbound \
  --priority 110 \
  --source-address-prefix "VirtualNetwork" \
  --source-port-range "*" \
  --destination-address-prefix "Internet" \
  --destination-port-range "*"

The following rule allows SSH traffic inbound to the subnet from anywhere. The rule overrides a default security rule that denies all inbound traffic from the internet. SSH is allowed to the subnet so that connectivity can be tested in a later step.

az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Allow-SSH-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 120 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "VirtualNetwork" \
  --destination-port-range "22"
```

## <a name="restrict-network-access-to-a-resource"></a>Beperken van toegang tot het netwerk aan een resource

Stapsgewijze instructies voor het beperken van toegang tot het netwerk naar bronnen die zijn gemaakt via de Azure-services ingeschakeld voor service-eindpunten varieert op services. Zie de documentatie voor afzonderlijke services voor specifieke stappen voor elke service. De rest van dit artikel bevat stappen voor het beperken van toegang tot het netwerk voor een Azure Storage-account als voorbeeld.

### <a name="create-a-storage-account"></a>Create a storage account

Maken van een Azure storage-account met [az storage-account maken](/cli/azure/storage/account#az_storage_account_create). Vervang `<replace-with-your-unique-storage-account-name>` met een naam die is uniek voor alle Azure locaties tussen 3 tot 24 tekens lang zijn, met behulp van alleen cijfers en kleine letters.

```azurecli-interactive
storageAcctName="<replace-with-your-unique-storage-account-name>"

az storage account create \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --sku Standard_LRS \
  --kind StorageV2
```

Nadat het opslagaccount is gemaakt, het ophalen van de verbindingsreeks voor de storage-account in een variabele met [az storage-account weergeven verbindingsreeks](/cli/azure/storage/account#az_storage_account_show_connection_string). De verbindingsreeks wordt gebruikt voor het maken van een bestandsshare in een latere stap.

```azurecli-interactive
saConnectionString=$(az storage account show-connection-string \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --query 'connectionString' \
  --out tsv)
```

<a name="account-key"></a>De inhoud van de variabele bekijken en noteer de waarde voor **AccountKey** geretourneerd in de uitvoer, omdat deze wordt gebruikt in een later stadium.

```azurecli-interactive
echo $saConnectionString
```

### <a name="create-a-file-share-in-the-storage-account"></a>Een bestandsshare in de storage-account maken

Een bestandsshare maken in het opslagaccount met [az storage-share maken](/cli/azure/storage/share#az_storage_share_create). Deze bestandsshare is een latere stap gekoppeld voor toegang tot het netwerk te bevestigen.

```azurecli-interactive
az storage share create \
  --name my-file-share \
  --quota 2048 \
  --connection-string $saConnectionString > /dev/null
```

### <a name="deny-all-network-access-to-a-storage-account"></a>Alle toegang tot het netwerk naar een opslagaccount weigeren

Storage-accounts accepteren standaard netwerkverbindingen van clients in een netwerk. Om te beperken van toegang tot geselecteerde netwerken, wijzigt u de standaardactie voor *weigeren* met [az storage account bijwerken](/cli/azure/storage/account#az_storage_account_update). Het opslagaccount is niet toegankelijk is vanaf een netwerk wanneer het netwerktoegang is geweigerd.

```azurecli-interactive
az storage account update \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --default-action Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Toegang tot het netwerk van een subnet inschakelen

Toestaan van toegang tot het netwerk naar de storage-account van de *persoonlijke* subnet met [az storage account netwerk-regel toevoegen](/cli/azure/storage/account/network-rule#az_storage_account_network_rule_add).

```azurecli-interactive
az storage account network-rule add \
  --resource-group myResourceGroup \
  --account-name $storageAcctName \
  --vnet-name myVirtualNetwork \
  --subnet Private
```
## <a name="create-virtual-machines"></a>Virtuele machines maken

Als u wilt testen toegang tot het netwerk naar een opslagaccount, moet u een virtuele machine implementeren met elk subnet.

### <a name="create-the-first-virtual-machine"></a>De eerste virtuele machine maken

Maak een VM in de *openbare* subnet met [az vm maken](/cli/azure/vm#az_vm_create). Als SSH-sleutels niet al bestaan op de standaardlocatie van de sleutel, wordt deze de opdracht maken. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPublic \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --generate-ssh-keys
```

De virtuele machine duurt een paar minuten maken. Nadat de virtuele machine is gemaakt, toont de Azure CLI informatie vergelijkbaar met het volgende voorbeeld: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmPublic",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Noteer de **publicIpAddress** in de resulterende uitvoer. Dit adres wordt gebruikt voor toegang tot de virtuele machine vanaf het internet in een later stadium.

### <a name="create-the-second-virtual-machine"></a>Het tweede virtuele machine maken

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPrivate \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --generate-ssh-keys
```

De virtuele machine duurt een paar minuten maken. Na het maken, dient u de **publicIpAddress** in de uitvoer die wordt geretourneerd. Dit adres wordt gebruikt voor toegang tot de virtuele machine vanaf het internet in een later stadium.

## <a name="confirm-access-to-storage-account"></a>Toegang tot de storage-account bevestigen

Voeg SSH toe aan de *myVmPrivate* VM. Vervang *<publicIpAddress>* met het openbare IP-adres van uw *myVmPrivate* VM.

```bash 
ssh <publicIpAddress>
```

Maak een map voor een koppelpunt:

```bash
sudo mkdir /mnt/MyAzureFileShare
```

Koppel de Azure-bestandsshare naar de map die u hebt gemaakt. Voordat u de volgende opdracht uitvoert, vervangt `<storage-account-name>` met de accountnaam en `<storage-account-key>` met de sleutel die u hebt opgehaald in [een opslagaccount maken](#create-a-storage-account).

```bash
sudo mount --types cifs //<storage-account-name>.file.core.windows.net/my-file-share /mnt/MyAzureFileShare --options vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

U ontvangt de `user@myVmPrivate:~$` prompt. De Azure-bestandsshare is gelukt aan */mnt/MyAzureFileShare*.

Bevestig dat de virtuele machine geen uitgaande verbinding heeft met andere openbare IP-adressen heeft:

```bash
ping bing.com -c 4
```

U ontvangt geen antwoorden, omdat de netwerkbeveiligingsgroep die is gekoppeld aan de *persoonlijke* subnet kan geen uitgaande toegang tot de openbare IP-adressen dan de adressen die zijn toegewezen aan de Azure Storage-service.

Afsluiten van de SSH-sessie op de *myVmPrivate* VM.

## <a name="confirm-access-is-denied-to-storage-account"></a>Toegang is geweigerd voor storage-account bevestigen

Gebruik de volgende opdracht voor het maken van een SSH-sessie met de *myVmPublic* VM. Vervang `<publicIpAddress>` met het openbare IP-adres van uw *myVmPublic* VM: 

```bash 
ssh <publicIpAddress>
```

Maak een map voor een koppelpunt:

```bash
sudo mkdir /mnt/MyAzureFileShare
```

Poging tot het koppelen van de Azure-bestandsshare naar de map die u hebt gemaakt. In dit artikel wordt ervan uitgegaan dat u de nieuwste versie van Ubuntu geïmplementeerd. Als u van eerdere versies van Ubuntu gebruikmaakt, Zie [koppelen op Linux](../storage/files/storage-how-to-use-files-linux.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor aanvullende instructies over het koppelen van bestandsshares. Voordat u de volgende opdracht uitvoert, vervangt `<storage-account-name>` met de accountnaam en `<storage-account-key>` met de sleutel die u hebt opgehaald in [een opslagaccount maken](#create-a-storage-account):

```bash
sudo mount --types cifs //storage-account-name>.file.core.windows.net/my-file-share /mnt/MyAzureFileShare --options vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

Toegang is geweigerd en u ontvangt een `mount error(13): Permission denied` fout, omdat de *myVmPublic* VM is geïmplementeerd in de *openbare* subnet. De *openbare* subnet beschikt niet over een service-eindpunt dat is ingeschakeld voor Azure Storage en het opslagaccount kunt u alleen toegang tot het netwerk van de *persoonlijke* subnet, niet de *openbare*subnet.

Afsluiten van de SSH-sessie op de *myVmPublic* VM.

Van uw computer wilt weergeven die de shares in uw opslagaccount met [az storage-share lijst](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list). Vervang `<account-name>` en `<account-key>` met de naam van het opslagaccount en de sleutel uit [een opslagaccount maken](#create-a-storage-account):

```azurecli-interactive
az storage share list \
  --account-name <account-name> \
  --account-key <account-key>
```

Toegang is geweigerd en u ontvangt een *deze aanvraag is niet geautoriseerd voor het uitvoeren van deze bewerking* fout, omdat de computer zich niet in de *persoonlijke* subnet van de *MyVirtualNetwork* virtueel netwerk.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, gebruik [az groep verwijderen](/cli/azure#az_group_delete) verwijderen van de resourcegroep en alle resources bevat.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een service-eindpunt voor een virtueel netwerksubnet ingeschakeld. Hebt u geleerd dat de service-eindpunten kunnen worden ingeschakeld voor resources met meerdere Azure-services worden geïmplementeerd. U hebt een Azure Storage-account en een beperkte netwerktoegang tot het opslagaccount alleen bronnen binnen een virtueel netwerksubnet gemaakt. Zie voor meer informatie over service-eindpunten, [overzicht van de Service-eindpunten](virtual-network-service-endpoints-overview.md) en [subnetten beheren](virtual-network-manage-subnet.md).

Als u meerdere virtuele netwerken in uw account hebt, kunt u twee virtuele netwerken met elkaar verbinden zodat de resources in elk virtueel netwerk met elkaar kunnen communiceren. Voor meer informatie Zie [verbinding maken met virtuele netwerken](tutorial-connect-virtual-networks-cli.md).