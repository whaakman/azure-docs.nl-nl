---
title: Maak een zoned Linux-VM met de Azure CLI | Microsoft Docs
description: Maak een Linux-VM in een zone beschikbaarheid met de Azure CLI
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: 512b6cde1a1de70f020a9af1254d2bc8e78f1b5f
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30905513"
---
# <a name="create-a-linux-virtual-machine-in-an-availability-zone-with-the-azure-cli"></a>Maken van een virtuele Linux-machine in een zone beschikbaarheid met de Azure CLI

In dit artikel wordt stapsgewijs via de Azure CLI gebruiken voor het maken van een Linux-VM in een zone Azure beschikbaarheid. Een [beschikbaarheidszone](../../availability-zones/az-overview.md) is een fysiek afgescheiden zone in een Azure-regio. Gebruik beschikbaarheidszones om uw apps en gegevens te beschermen tegen het onwaarschijnlijke risico van een storing of het verloren gaan van een heel datacenter.

Voor het gebruik van een zone beschikbaarheid, maakt u uw virtuele machine in een [ondersteunde Azure-regio](../../availability-zones/az-overview.md#regions-that-support-availability-zones).

Zorg ervoor dat de meest recente geïnstalleerd [Azure CLI 2.0](/cli/azure/install-az-cli2) en aangemeld bij een Azure-account met [az aanmelding](/cli/azure/reference-index#az_login).


## <a name="check-vm-sku-availability"></a>Beschikbaarheid van VM-SKU controleren
De beschikbaarheid van VM-grootten, of SKU's, kan verschillen per regio en zone. Als hulp bij de planning voor het gebruik van de beschikbaarheidszones kunt u een overzicht maken van de beschikbare VM-SKU's per Azure-regio en zone. Hiermee zorgt u ervoor dat u een correcte VM-grootte kiest en de gewenste tolerantie in zones verkrijgt. Voor meer informatie over de verschillende VM-typen en -grootten raadpleegt u het [Overzicht van de VM-grootten](sizes.md).

Vindt u de beschikbare SKU van de VM's met de [az vm lijst-SKU's](/cli/azure/vm#az_vm_list_skus) opdracht. Met het volgende voorbeeld wordt een lijst weergegeven van de beschikbare VM-SKU's in de regio *eastus2*:

```azurecli
az vm list-skus --location eastus2 --output table
```

De uitvoer is vergelijkbaar met het volgende verkorte voorbeeld, waarin de beschikbaarheidszones worden weergegeven waarin elke VM-grootte beschikbaar is:

```azurecli
ResourceType      Locations  Name               [...]    Tier       Size     Zones
----------------  ---------  -----------------           ---------  -------  -------
virtualMachines   eastus2    Standard_DS1_v2             Standard   DS1_v2   1,2,3
virtualMachines   eastus2    Standard_DS2_v2             Standard   DS2_v2   1,2,3
[...]
virtualMachines   eastus2    Standard_F1s                Standard   F1s      1,2,3
virtualMachines   eastus2    Standard_F2s                Standard   F2s      1,2,3
[...]
virtualMachines   eastus2    Standard_D2s_v3             Standard   D2_v3    1,2,3
virtualMachines   eastus2    Standard_D4s_v3             Standard   D4_v3    1,2,3
[...]
virtualMachines   eastus2    Standard_E2_v3              Standard   E2_v3    1,2,3
virtualMachines   eastus2    Standard_E4_v3              Standard   E4_v3    1,2,3
```


## <a name="create-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az_group_create).  

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Voordat een virtuele machine wordt gemaakt, moet een resourcegroep worden gemaakt. In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroupVM* wordt gemaakt in de *eastus2* regio. VS-Oost 2 is een van de Azure-regio's die ondersteuning biedt voor beschikbaarheid zones.

```azurecli 
az group create --name myResourceGroupVM --location eastus2
```

De resourcegroep is opgegeven bij het maken of wijzigen van een virtuele machine die in dit artikel kan worden gezien.

## <a name="create-virtual-machine"></a>Virtuele machine maken

Maak een virtuele machine met de opdracht [az vm create](/cli/azure/vm#az_vm_create). 

Wanneer u een virtuele machine maakt, zijn er diverse opties beschikbaar zoals besturingssysteeminstallatiekopie, schijfgrootte en beheerdersreferenties. In dit voorbeeld wordt een virtuele machine gemaakt met de naam *myVM* waarop Ubuntu-server wordt uitgevoerd. De virtuele machine is gemaakt in de zone beschikbaarheid *1*. Standaard wordt de virtuele machine gemaakt de *Standard_DS1_v2* grootte.

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --location eastus2 --image UbuntuLTS --generate-ssh-keys --zone 1
```

Het maken van de virtuele machine kan een paar minuten duren. Wanneer de virtuele machine is gemaakt, biedt de Azure CLI informatie over de virtuele machine. Noteer de `zones` waarde waarmee wordt aangegeven van de beschikbaarheid zone waarin de virtuele machine wordt uitgevoerd. 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus2",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM",
  "zones": "1"
}
```

## <a name="confirm-zone-for-managed-disk-and-ip-address"></a>Zone voor beheerde schijf- en IP-adres bevestigen

Wanneer de virtuele machine is geïmplementeerd in een zone beschikbaarheid, wordt een beheerde schijf voor de virtuele machine gemaakt in dezelfde regio bevindt beschikbaarheid. Een openbaar IP-adres wordt standaard ook gemaakt in de zone. De volgende voorbeelden ophalen informatie over deze resources.

Gebruiken om te controleren of de VM-beheerde schijven in de zone van de beschikbaarheid, de [az vm weergeven](/cli/azure/vm#az_vm_show) opdracht voor het retourneren van de schijf-id. In dit voorbeeld wordt de schijf-id opgeslagen in een variabele die wordt gebruikt in een latere stap. 

```azurecli-interactive
osdiskname=$(az vm show -g myResourceGroupVM -n myVM --query "storageProfile.osDisk.name" -o tsv)
```
U kunt nu informatie over de beheerde schijf:

```azurecli-interactive
az disk show --resource-group myResourceGroupVM --name $osdiskname
```

In de uitvoer ziet u dat de beheerde schijf zich in dezelfde beschikbaarheidszone bevindt als de virtuele machine:

```azurecli
{
  "creationData": {
    "createOption": "FromImage",
    "imageReference": {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westeurope/Publishers/Canonical/ArtifactTypes/VMImage/Offers/UbuntuServer/Skus/16.04-LTS/Versions/latest",
      "lun": null
    },
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/disks/osdisk_761c570dab",
  "location": "eastus2",
  "managedBy": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "name": "myVM_osdisk_761c570dab",
  "osType": "Linux",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroupVM",
  "sku": {
    "name": "Premium_LRS",
    "tier": "Premium"
  },
  "tags": {},
  "timeCreated": "2018-03-05T22:16:06.892752+00:00",
  "type": "Microsoft.Compute/disks",
  "zones": [
    "1"
  ]
}
```

Gebruik de [az vm lijst-ip-adressen](/cli/azure/vm#az_vm_list_ip_addresses) opdracht voor het retourneren van de naam van het openbare IP-adres resource in *myVM*. In dit voorbeeld wordt de naam van de opgeslagen in een variabele die wordt gebruikt in een latere stap.

```azurecli
ipaddressname=$(az vm list-ip-addresses -g myResourceGroupVM -n myVM --query "[].virtualMachine.network.publicIpAddresses[].name" -o tsv)
```

U kunt nu informatie over het IP-adres:

```azurecli
az network public-ip show --resource-group myResourceGroupVM --name $ipaddressname
```

De uitvoer wordt weergegeven dat het IP-adres in dezelfde regio bevindt beschikbaarheid als de virtuele machine:

```azurecli
{
  "dnsSettings": null,
  "etag": "W/\"b7ad25eb-3191-4c8f-9cec-c5e4a3a37d35\"",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/publicIPAddresses/myVMPublicIP",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "52.174.34.95",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "myResourceGroupVM",
    "subnet": null
  },
  "location": "eastUS2",
  "name": "myVMPublicIP",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "myResourceGroupVM",
  "resourceGuid": "8c70a073-09be-4504-0000-000000000000",
  "tags": {},
  "type": "Microsoft.Network/publicIPAddresses",
  "zones": [
    "1"
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een virtuele machine maken in een zone beschikbaarheid. Meer informatie over [regio's en beschikbaarheid](regions-and-availability.md) voor virtuele Azure-machines.




