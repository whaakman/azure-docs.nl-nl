---
title: Beperken van netwerktoegang tot PaaS-resources - Azure CLI | Microsoft Docs
description: In dit artikel leert u hoe u begrenzen en beperken van toegang tot het netwerk naar Azure-resources, zoals Azure Storage en Azure SQL Database, met virtual network-service-eindpunten met behulp van de Azure CLI.
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
ms.openlocfilehash: 87fcfd98065bcf1f0fea3a06029853f69d67842d
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55663805"
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-cli"></a>Netwerktoegang tot PaaS-resources beperken met service-eindpunten van een virtueel netwerk met de Azure CLI

Met service-eindpunten voor virtuele netwerken kunt u de netwerktoegang tot sommige Azure-servicebronnen beperken tot een subnet van een virtueel netwerk. U kunt ook internettoegang tot de resources verwijderen. Service-eindpunten zorgen voor een rechtstreekse verbinding van uw virtuele netwerk met ondersteunde Azure-services, zodat u de privéadresruimte van uw virtuele netwerk kunt gebruiken voor toegang tot de Azure-services. Verkeer dat bestemd is voor Azure-resources via de service-eindpunten blijft altijd op het Microsoft Azure-backbone-netwerk. In dit artikel leert u het volgende:

* Een virtueel netwerk maken met één subnet
* Een subnet toevoegen en een service-eindpunt inschakelen
* Een Azure-resource maken en alleen toegang ertoe toestaan vanaf een subnet
* Een virtuele machine (VM) implementeren op elk subnet
* Toegang tot een resource vanaf een subnet bevestigen
* Bevestigen dat toegang wordt geweigerd aan een resource vanaf een subnet en internet

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstart de Azure CLI versie 2.0.28 of later uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Voordat u een virtueel netwerk maakt, moet u maken van een resourcegroep voor het virtuele netwerk en alle andere resources in dit artikel hebt gemaakt. Maak een resourcegroep maken met [az group create](/cli/azure/group). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

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

U kunt de service-eindpunten alleen voor services die ondersteuning bieden voor service-eindpunten inschakelen. Service-eindpunt ingeschakeld services beschikbaar weergeven in een Azure-locatie met [az network vnet--eindpuntservices](/cli/azure/network/vnet#az_network_vnet_list_endpoint_services). Het volgende voorbeeld retourneert een lijst van service-eindpunt ingeschakeld services beschikbaar in de *eastus* regio. De lijst met services die zijn geretourneerd zal na verloop van tijd toenemen naarmate er meer Azure-services worden service-eindpunt ingeschakeld.

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

## <a name="restrict-network-access-for-a-subnet"></a>Netwerktoegang voor een subnet beperken

Maak een netwerkbeveiligingsgroep met [az network nsg maken](/cli/azure/network/nsg#az_network_nsg_create). Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNsgPrivate*.

```azurecli-interactive
az network nsg create \
  --resource-group myResourceGroup \
  --name myNsgPrivate
```

Koppelen van de netwerkbeveiligingsgroep voor de *persoonlijke* subnet met de [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update). Het volgende voorbeeld wordt de *myNsgPrivate* netwerkbeveiligingsgroep naar de *persoonlijke* subnet:

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Private \
  --resource-group myResourceGroup \
  --network-security-group myNsgPrivate
```

Maken van de beveiligingsregels voor verbindingen met [az network nsg-regel maken](/cli/azure/network/nsg/rule#az_network_nsg_rule_create). De regel die volgt toestaat uitgaande toegang tot het openbare IP-adressen toegewezen aan de Azure Storage-service: 

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

## <a name="restrict-network-access-to-a-resource"></a>Netwerktoegang tot een resource beperken

De stappen die nodig zijn om netwerktoegang te beperken tot resources die zijn gemaakt met Azure-services waarvoor service-eindpunten zijn ingeschakeld, verschillen per service. Zie de documentatie voor afzonderlijke services voor specifieke stappen voor elke service. De rest van dit artikel bevat stappen voor het beperken van toegang tot het netwerk voor een Azure Storage-account, als voorbeeld.

### <a name="create-a-storage-account"></a>Create a storage account

Maak een Azure storage-account met [az storage-account maken](/cli/azure/storage/account#az_storage_account_create). Vervang `<replace-with-your-unique-storage-account-name>` met een naam die uniek is voor alle Azure locaties, tussen 3 en 24 tekens lang, met behulp van alleen cijfers en kleine letters.

```azurecli-interactive
storageAcctName="<replace-with-your-unique-storage-account-name>"

az storage account create \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --sku Standard_LRS \
  --kind StorageV2
```

Nadat het opslagaccount is gemaakt, de verbindingsreeks voor het opslagaccount ophalen in een variabele met [az storage account show-connection-string](/cli/azure/storage/account#az_storage_account_show_connection_string). De verbindingsreeks wordt gebruikt om een bestandsshare maken in een latere stap.

```azurecli-interactive
saConnectionString=$(az storage account show-connection-string \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --query 'connectionString' \
  --out tsv)
```

<a name="account-key"></a>De inhoud van de variabele bekijken en noteer de waarde voor **AccountKey** geretourneerd in de uitvoer, omdat deze wordt gebruikt in een latere stap.

```azurecli-interactive
echo $saConnectionString
```

### <a name="create-a-file-share-in-the-storage-account"></a>Een bestandsshare maken in het opslagaccount

Een bestandsshare maken in de storage-account met [az storage share maken](/cli/azure/storage/share). In een latere stap, is deze bestandsshare gekoppeld voor toegang tot het netwerk te bevestigen.

```azurecli-interactive
az storage share create \
  --name my-file-share \
  --quota 2048 \
  --connection-string $saConnectionString > /dev/null
```

### <a name="deny-all-network-access-to-a-storage-account"></a>Alle toegang tot het netwerk naar een opslagaccount weigeren

Standaard accepteren opslagaccounts netwerkverbindingen van clients in ieder netwerk. Om te beperken de toegang tot geselecteerde netwerken, wijzigt u de standaardactie voor *weigeren* met [az storage account update](/cli/azure/storage/account#az_storage_account_update). Zodra het netwerktoegang is geweigerd, is de storage-account niet toegankelijk is vanaf een netwerk.

```azurecli-interactive
az storage account update \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --default-action Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Netwerktoegang vanuit een subnet inschakelen

Toestaan van toegang tot het netwerk naar het opslagaccount van de *persoonlijke* subnet met de [az storage account network-regel toevoegen](/cli/azure/storage/account/network-rule#az_storage_account_network_rule_add).

```azurecli-interactive
az storage account network-rule add \
  --resource-group myResourceGroup \
  --account-name $storageAcctName \
  --vnet-name myVirtualNetwork \
  --subnet Private
```
## <a name="create-virtual-machines"></a>Virtuele machines maken

Implementeer een VM in elk subnet om de netwerktoegang tot een opslagaccount te testen.

### <a name="create-the-first-virtual-machine"></a>De eerste virtuele machine maken

Maken van een virtuele machine in de *openbare* subnet met de [az vm maken](/cli/azure/vm#az_vm_create). Als SSH-sleutels niet al bestaan op de standaardlocatie van de sleutel, worden ze met deze opdracht gemaakt. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPublic \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --generate-ssh-keys
```

Het maken van de virtuele machine duurt een paar minuten. Nadat de virtuele machine is gemaakt, toont de Azure CLI informatie die vergelijkbaar is met het volgende voorbeeld: 

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

Noteer de **publicIpAddress** in de resulterende uitvoer. Dit adres wordt gebruikt voor toegang tot de virtuele machine via internet in een latere stap.

### <a name="create-the-second-virtual-machine"></a>De tweede virtuele machine maken

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPrivate \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --generate-ssh-keys
```

Het maken van de virtuele machine duurt een paar minuten. Na het maken, noteer de **publicIpAddress** in de uitvoer die wordt geretourneerd. Dit adres wordt gebruikt voor toegang tot de virtuele machine via internet in een latere stap.

## <a name="confirm-access-to-storage-account"></a>Toegang tot opslagaccount bevestigen

Voeg SSH toe aan de *myVmPrivate* VM. Vervang *<publicIpAddress>* met het openbare IP-adres van uw *myVmPrivate* VM.

```bash 
ssh <publicIpAddress>
```

Maak een map voor een koppelpunt:

```bash
sudo mkdir /mnt/MyAzureFileShare
```

Koppel de Azure-bestandsshare naar de map die u hebt gemaakt. Voordat u de volgende opdracht uitvoert, Vervang `<storage-account-name>` met de accountnaam en `<storage-account-key>` met de sleutel die u hebt opgehaald in [een opslagaccount maken](#create-a-storage-account).

```bash
sudo mount --types cifs //<storage-account-name>.file.core.windows.net/my-file-share /mnt/MyAzureFileShare --options vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

U ontvangt de `user@myVmPrivate:~$` prompt. De Azure-bestandsshare is gekoppeld aan */mnt/MyAzureFileShare*.

Bevestig dat de virtuele machine geen uitgaande verbinding naar een andere openbare IP-adressen heeft:

```bash
ping bing.com -c 4
```

U krijgt geen antwoorden, omdat de netwerkbeveiligingsgroep die is gekoppeld aan het *Privé*-subnet geen uitgaande toegang toestaat aan andere openbare IP-adressen dan de adressen die zijn toegewezen aan de Azure Storage-service.

Sluit de SSH-sessie met de *myVmPrivate* VM.

## <a name="confirm-access-is-denied-to-storage-account"></a>Bevestigen dat toegang tot opslagaccount wordt geweigerd

Gebruik de volgende opdracht om te maken van een SSH-sessie met de *myVmPublic* VM. Vervang `<publicIpAddress>` met het openbare IP-adres van uw *myVmPublic* VM: 

```bash 
ssh <publicIpAddress>
```

Maak een map voor een koppelpunt:

```bash
sudo mkdir /mnt/MyAzureFileShare
```

Poging tot het koppelen van de Azure-bestandsshare naar de map die u hebt gemaakt. In dit artikel wordt ervan uitgegaan dat u de nieuwste versie van Ubuntu geïmplementeerd. Als u van eerdere versies van Ubuntu gebruikmaakt, Zie [koppelen in Linux](../storage/files/storage-how-to-use-files-linux.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor aanvullende instructies over het koppelen van bestandsshares. Voordat u de volgende opdracht uitvoert, Vervang `<storage-account-name>` met de accountnaam en `<storage-account-key>` met de sleutel die u hebt opgehaald in [een opslagaccount maken](#create-a-storage-account):

```bash
sudo mount --types cifs //storage-account-name>.file.core.windows.net/my-file-share /mnt/MyAzureFileShare --options vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

De toegang is geweigerd en u ontvangt een `mount error(13): Permission denied` fout, omdat de *myVmPublic* virtuele machine wordt geïmplementeerd binnen de *openbare* subnet. Het *Openbare* subnet heeft geen service-eindpunt ingeschakeld voor Azure Storage en het opslagaccount staat alleen netwerktoegang toe van het *Privé*-subnet, en niet van het *Openbare*subnet.

Sluit de SSH-sessie met de *myVmPublic* VM.

Vanaf uw computer, probeert de shares weergeven in uw opslagaccount met [az storage share list](/cli/azure/storage/share?view=azure-cli-latest). Vervang `<account-name>` en `<account-key>` met de naam van het opslagaccount en sleutel van [een opslagaccount maken](#create-a-storage-account):

```azurecli-interactive
az storage share list \
  --account-name <account-name> \
  --account-key <account-key>
```

De toegang is geweigerd en u ontvangt een *deze aanvraag is niet gemachtigd voor deze bewerking* fout, omdat de computer zich niet in de *persoonlijke* subnet van de *MyVirtualNetwork* virtueel netwerk.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, gebruikt u [az group delete](/cli/azure#az_group_delete) om de resourcegroep en alle resources die deze bevat te verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een service-eindpunt voor een subnet van een virtueel netwerk ingeschakeld. U hebt geleerd dat service-eindpunten kunnen worden ingeschakeld voor met meerdere Azure-services geïmplementeerde resources. U hebt een Azure Storage-account gemaakt en netwerktoegang tot het opslagaccount beperkt tot alleen resources binnen een subnet van een virtueel netwerk. Zie voor meer informatie over service-eindpunten [Overzicht voor service-eindpunten](virtual-network-service-endpoints-overview.md) en [Subnetten beheren](virtual-network-manage-subnet.md).

Als u meerdere virtuele netwerken in uw account hebt, kunt u twee virtuele netwerken met elkaar verbinden zodat de resources in beide virtuele netwerken met elkaar kunnen communiceren. Voor meer informatie Zie [virtuele netwerken verbinden](tutorial-connect-virtual-networks-cli.md).
