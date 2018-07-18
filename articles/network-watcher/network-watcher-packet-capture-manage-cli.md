---
title: Pakketopname beheren met Azure Network Watcher - Azure CLI | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u voor het beheren van de functie voor het vastleggen van pakketten van Network Watcher met de Azure CLI
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: cb0c1d10-f7f2-4c34-b08c-f73452430be8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 9b40a85cf3c4edd26f2fc15045f3d6862d4ac1ff
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090483"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-azure-cli"></a>Pakketopname beheren met Azure Network Watcher met de Azure CLI

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [Azure-CLI](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST-API](network-watcher-packet-capture-manage-rest.md)

Network Watcher packet-capture kunt u capture-sessies voor het volgen van verkeer van en naar een virtuele machine maken. Filters zijn beschikbaar voor de opnamesessie om te controleren of dat u alleen het verkeer die u wilt vastleggen. Pakketopname helpt bij het opsporen van afwijkingen netwerk reactief en proactief. Andere toepassingen zijn onder andere de netwerkstatistieken, het verkrijgen van informatie over het netwerk indringers, fouten opsporen in client-servercommunicatie en nog veel meer verzamelen. Door vereenvoudigt deze mogelijkheid voor het extern activeren pakketopnamen, de belasting van het uitvoeren van een pakketopname handmatig en op de gewenste computer, wat kostbare tijd bespaart.

In dit artikel wordt onze CLI van de volgende generatie voor het implementatiemodel resource management, Azure CLI 2.0 die beschikbaar is voor Windows, Mac en Linux.

Als u wilt de stappen in dit artikel uitvoert, moet u [installeren van de Azure-opdrachtregelinterface voor Mac, Linux en Windows (Azure CLI)](https://docs.microsoft.com/cli/azure/install-az-cli2).

In dit artikel doorloopt u de verschillende beheertaken die momenteel beschikbaar voor de pakketopname zijn.

- [**Een pakketopname starten**](#start-a-packet-capture)
- [**Een pakketopname stoppen**](#stop-a-packet-capture)
- [**Een pakketopname verwijderen**](#delete-a-packet-capture)
- [**Een pakketopname downloaden**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u hebt de volgende bronnen:

- Een exemplaar van Network Watcher in de regio die u wilt maken van een pakketopname
- Een virtuele machine met de packet capture-uitbreiding is ingeschakeld.

> [!IMPORTANT]
> Pakketopname moet een agent worden uitgevoerd op de virtuele machine. De Agent is geïnstalleerd als een uitbreiding. Voor instructies voor het VM-extensies, gaat u naar [extensies voor virtuele machines en functies](../virtual-machines/windows/extensions-features.md).

## <a name="install-vm-extension"></a>VM-extensie installeren

### <a name="step-1"></a>Stap 1

Voer de `az vm extension set` cmdlet de packet capture-agent installeren op de virtuele gastmachine.

Voor Windows virtual machines:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

Voor virtuele Linux-machines:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux--version 1.4
````

### <a name="step-2"></a>Stap 2

Uitvoeren om ervoor te zorgen dat de agent is geïnstalleerd, de `vm extension show` cmdlet en geven deze de naam van de resource-groep en de virtuele machine. Controleer de resulterende lijst om te controleren of dat de agent is geïnstalleerd.

```azurecli
az vm extension show --resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

Het volgende voorbeeld wordt een voorbeeld van het antwoord niet worden uitgevoerd `az vm extension show`

```json
{
  "autoUpgradeMinorVersion": true,
  "forceUpdateTag": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/extensions/NetworkWatcherAgentWindows",
  "instanceView": null,
  "location": "westcentralus",
  "name": "NetworkWatcherAgentWindows",
  "protectedSettings": null,
  "provisioningState": "Succeeded",
  "publisher": "Microsoft.Azure.NetworkWatcher",
  "resourceGroup": "{resourceGroupName}",
  "settings": null,
  "tags": null,
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "typeHandlerVersion": "1.4",
  "virtualMachineExtensionType": "NetworkWatcherAgentWindows"
}
```

## <a name="start-a-packet-capture"></a>Een pakketopname starten

Als de voorgaande stappen voltooid zijn, worden de packet capture-agent is geïnstalleerd op de virtuele machine.

### <a name="step-1"></a>Stap 1

De volgende stap is om op te halen van de Network Watcher-exemplaar. Plaats de naam van de netwerk-Watcher wordt doorgegeven aan de `az network watcher show` cmdlet in stap 4.

```azurecli
az network watcher show --resource-group resourceGroup --name networkWatcherName
```

### <a name="step-2"></a>Stap 2

Ophalen van een storage-account. Dit opslagaccount wordt gebruikt voor het opslaan van het pakket vastleggen-bestand.

```azurecli
azure storage account list
```

### <a name="step-3"></a>Stap 3

Filters kunnen worden gebruikt om de gegevens die zijn opgeslagen door de pakketopname te beperken. Het volgende voorbeeld stelt u een pakketopname met verschillende filters.  De eerste drie filters voor het verzamelen van uitgaande TCP-verkeer alleen vanuit het lokale IP 10.0.0.3 voor doelpoorten 20, 80 en 443.  Het laatste filter verzamelt alleen UDP-verkeer.

```azurecli
az network watcher packet-capture create --resource-group {resourceGroupName} --vm {vmName} --name packetCaptureName --storage-account {storageAccountName} --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

Het volgende voorbeeld is de verwachte uitvoer wordt uitgevoerd de `az network watcher packet-capture create` cmdlet.

```json
{
  "bytesToCapturePerPacket": 0,
  "etag": "W/\"b8cf3528-2e14-45cb-a7f3-5712ffb687ac\"",
  "filters": [
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "20"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "80"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "443"
    },
    {
      "localIpAddress": "",
      "localPort": "",
      "protocol": "UDP",
      "remoteIpAddress": "",
      "remotePort": ""
    }
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/pa
cketCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "provisioningState": "Succeeded",
  "resourceGroup": "NetworkWatcherRG",
  "storageLocation": {
    "filePath": null,
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/gwteststorage123abc",
    "storagePath": "https://gwteststorage123abc.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/{resourceGroupName}/p
roviders/microsoft.compute/virtualmachines/{vmName}/2017/05/25/packetcapture_16_22_34_630.cap"
  },
  "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}",
  "timeLimitInSeconds": 18000,
  "totalBytesPerSession": 1073741824
}
```

## <a name="get-a-packet-capture"></a>Een pakketopname ophalen

Met de `az network watcher packet-capture show-status` cmdlet, wordt de status van een pakketopname die momenteel wordt uitgevoerd of voltooid.

```azurecli
az network watcher packet-capture show-status --name packetCaptureName --location {networkWatcherLocation}
```

Het volgende voorbeeld wordt de uitvoer van de `az network watcher packet-capture show-status` cmdlet. Het volgende voorbeeld is wanneer het vastleggen is gestopt, met een StopReason TimeExceeded. 

```
{
  "additionalProperties": {
    "status": "Succeeded"
  },
  "captureStartTime": "2016-12-06T17:20:01.5671279Z",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/pa
cketCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "packetCaptureError": [],
  "packetCaptureStatus": "Stopped",
  "stopReason": "TimeExceeded"
}
```

## <a name="stop-a-packet-capture"></a>Een pakketopname stoppen

Door het uitvoeren van de `az network watcher packet-capture stop` cmdlet, als een opnamesessie bezig is is gestopt.

```azurecli
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> De cmdlet retourneert geen reactie wanneer die worden uitgevoerd op een actieve opnamesessie of een bestaande sessie die al is gestopt.

## <a name="delete-a-packet-capture"></a>Een pakketopname verwijderen

```azurecli
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
```

> [!NOTE]
> Een pakketopname verwijderen, wordt het bestand in het opslagaccount niet verwijderen.

## <a name="download-a-packet-capture"></a>Een pakketopname downloaden

Zodra uw pakket opnamesessie is voltooid, kan de capture-bestand worden geüpload naar blob-opslag of naar een lokaal bestand op de virtuele machine. De opslaglocatie van de pakketopname is gedefinieerd tijdens het maken van de sessie. Een handig hulpmiddel voor toegang tot deze bestanden opgeslagen in een storage-account is Microsoft Azure Storage Explorer, die hier kan worden gedownload:  http://storageexplorer.com/

Als een storage-account is opgegeven, worden pakketten vastleggen van bestanden worden opgeslagen in een storage-account op de volgende locatie:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het automatiseren van pakketopnamen met waarschuwingen van de virtuele machine via [maken van een waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md)

Zoeken of bepaalde verkeer is toegestaan in of buiten uw virtuele machine door naar de pagina [controleren IP-stroom controleren](diagnose-vm-network-traffic-filtering-problem.md)

<!-- Image references -->
