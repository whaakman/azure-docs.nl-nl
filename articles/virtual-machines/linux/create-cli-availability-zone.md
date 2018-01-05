---
title: Maak een zoned Linux-VM met de Azure CLI | Microsoft Docs
description: Maak een Linux-VM in een zone beschikbaarheid met de Azure CLI
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: danlep
ms.custom: 
ms.openlocfilehash: 5e742187295d0bd6dbc0767ee164335fc0cf9f02
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="create-a-linux-virtual-machine-in-an-availability-zone-with-the-azure-cli"></a>Maken van een virtuele Linux-machine in een zone beschikbaarheid met de Azure CLI

In dit artikel wordt stapsgewijs via de Azure CLI gebruiken voor het maken van een Linux-VM in een zone Azure beschikbaarheid. Een [beschikbaarheidszone](../../availability-zones/az-overview.md) is een fysiek afgescheiden zone in een Azure-regio. Gebruik beschikbaarheidszones om uw apps en gegevens te beschermen tegen het onwaarschijnlijke risico van een storing of het verloren gaan van een heel datacenter.

[!INCLUDE [availability-zones-preview-statement.md](../../../includes/availability-zones-preview-statement.md)]

Zorg ervoor dat de meest recente geïnstalleerd [Azure CLI 2.0](/cli/azure/install-az-cli2) en aangemeld bij een Azure-account met [az aanmelding](/cli/azure/#login).


## <a name="check-vm-sku-availability"></a>Controleer de beschikbaarheid van de VM-SKU
De beschikbaarheid van VM-grootten of SKU's, kan verschillen per regio en de zone. Om te helpen plannen voor het gebruik van de Zones van de beschikbaarheid, kunt u de beschikbare VM-SKU's door de Azure-regio en de zone aanbieden. Deze mogelijkheid zorgt ervoor dat u een correcte VM-grootte kiest en ophalen van de gewenste tolerantie in zones. Zie voor meer informatie over de verschillende typen van de virtuele machine en grootten [overzicht van de VM-grootten](sizes.md).

Vindt u de beschikbare SKU van de VM's met de [az vm lijst-SKU's](/cli/azure/vm#az_vm_list_skus) opdracht. Het volgende voorbeeld worden de beschikbare VM-SKU's in de *eastus2* regio:

```azurecli
az vm list-skus --location eastus2 --output table
```

De uitvoer is vergelijkbaar met het volgende verkorte voorbeeld, waarin de beschikbaarheid van Zones waarin elke VM-grootte beschikbaar is:

```azurecli
ResourceType      Locations  Name               Tier       Size     Zones
----------------  ---------  -----------------  ---------  -------  -------
virtualMachines   eastus2    Standard_DS1_v2    Standard   DS1_v2   1,2,3
virtualMachines   eastus2    Standard_DS2_v2    Standard   DS2_v2   1,2,3
[...]
virtualMachines   eastus2    Standard_F1s       Standard   F1s      1,2,3
virtualMachines   eastus2    Standard_F2s       Standard   F2s      1,2,3
[...]
virtualMachines   eastus2    Standard_D2s_v3    Standard   D2_v3    1,2,3
virtualMachines   eastus2    Standard_D4s_v3    Standard   D4_v3    1,2,3
[...]
virtualMachines   eastus2    Standard_E2_v3     Standard   E2_v3    1,2,3
virtualMachines   eastus2    Standard_E4_v3     Standard   E4_v3    1,2,3
```


## <a name="create-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az_group_create).  

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Een resourcegroep moet worden gemaakt voordat een virtuele machine. In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroupVM* wordt gemaakt in de *eastus2* regio. VS Oost 2 is een van de Azure-regio's die ondersteuning biedt voor beschikbaarheidszones in preview.

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus2
```

De resourcegroep is opgegeven bij het maken of wijzigen van een virtuele machine die in dit artikel kan worden gezien.

## <a name="create-virtual-machine"></a>Virtuele machine maken

Maak een virtuele machine met de [az vm maken](/cli/azure/vm#az_vm_create) opdracht. 

Wanneer u een virtuele machine maakt, er zijn diverse opties beschikbaar zoals besturingssysteemkopie schijf sizing en administratieve referenties. In dit voorbeeld wordt een virtuele machine gemaakt met de naam *myVM* Ubuntu Server uitgevoerd. De virtuele machine is gemaakt in de zone beschikbaarheid *1*. Standaard wordt de virtuele machine gemaakt de *Standard_DS1_v2* grootte. Deze grootte wordt ondersteund in de beschikbaarheid van zones preview.

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys --zone 1
```

Het duurt enkele minuten voor het maken van de virtuele machine. Wanneer de virtuele machine is gemaakt, levert de Azure CLI informatie over de virtuele machine. Noteer de `zones` waarde waarmee wordt aangegeven van de beschikbaarheid zone waarin de virtuele machine wordt uitgevoerd. 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus2",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM",
  "zones": "1"
}
```

## <a name="zone-for-ip-address-and-managed-disk"></a>Zone voor IP-adres en beheerde schijf

Wanneer de virtuele machine wordt geïmplementeerd in een zone beschikbaarheid, zijn de IP-adres en de beheerde schijfbronnen geïmplementeerd in dezelfde regio bevindt beschikbaarheid. De volgende voorbeelden ophalen informatie over deze resources.

Voor het eerst gebruiken de [az vm lijst-ip-adressen](/cli/azure/vm#az_vm_list_ip_addresses) opdracht voor het retourneren van de naam van het openbare IP-adres resource in *myVM*. In dit voorbeeld wordt de naam van de opgeslagen in een variabele die wordt gebruikt in een latere stap.

```azurecli-interactive
ipaddressname=$(az vm list-ip-addresses -g myResourceGroupVM -n myVM --query "[].virtualMachine.network.publicIpAddresses[].name" -o tsv)
```

U kunt nu informatie over het IP-adres:

```azurecli-interactive
az network public-ip show --resource-group myResourceGroupVM --name $ipaddressname
```

De uitvoer wordt weergegeven dat het IP-adres in dezelfde regio bevindt beschikbaarheid als de virtuele machine:

```azurecli-interactive
{
  "dnsSettings": null,
  "etag": "W/\"b7ad25eb-3191-4c8f-9cec-c5e4a3a37d35\"",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/publicIPAddresses/myVMPublicIP",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "52.174.34.95",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM",
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

Op dezelfde manier, Controleer of de VM-beheerde schijven in de zone beschikbaarheid. Gebruik de [az vm weergeven](/cli/azure/vm#az_vm_show) opdracht voor het retourneren van de schijf-id. In dit voorbeeld wordt de schijf-id opgeslagen in een variabele die wordt gebruikt in een latere stap. 

```azurecli-interactive
osdiskname=$(az vm show -g myResourceGroupVM -n myVM --query "storageProfile.osDisk.name" -o tsv)
```
U kunt nu informatie over de beheerde schijf:

```azurecli-interactive
az disk show --resource-group myResourceGroupVM --name $osdiskname
```

In de uitvoer ziet u dat de beheerde schijf zich in dezelfde beschikbaarheidszone bevindt als de virtuele machine:

```azurecli-interactive
{
  "creationData": {
    "createOption": "FromImage",
    "imageReference": {
      "id": "/Subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/Providers/Microsoft.Compute/Locations/westeurope/Publishers/Canonical/ArtifactTypes/VMImage/Offers/UbuntuServer/Skus/16.04-LTS/Versions/latest",
      "lun": null
    },
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/disks/osdisk_761c570dab",
  "location": "eastus2",
  "managedBy": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "name": "osdisk_761c570dab",
  "osType": "Linux",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroupVM",
  "sku": {
    "name": "Premium_LRS",
    "tier": "Premium"
  },
  "tags": {},
  "timeCreated": "2017-09-05T22:16:06.892752+00:00",
  "type": "Microsoft.Compute/disks",
  "zones": [
    "1"
  ]
}
```
 


## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een virtuele machine maakt in een beschikbaarheidszone. Meer informatie over [regio's en beschikbaarheid](regions-and-availability.md) voor virtuele Azure-machines.




