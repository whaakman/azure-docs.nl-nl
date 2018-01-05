---
title: Pakket opnamen met Azure-netwerk-Watcher - Azure CLI 2.0 beheren | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u voor het beheren van de functie voor het vastleggen van pakket van netwerk-Watcher met Azure CLI 2.0
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: cb0c1d10-f7f2-4c34-b08c-f73452430be8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 2c0cb9b72d23f46e60c96efe96a9ad32ba6fc746
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-20"></a>Pakket opnamen beheren met Azure met Azure CLI 2.0 netwerk-Watcher

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST-API](network-watcher-packet-capture-manage-rest.md)

Netwerk-Watcher pakketopname kunt u om sessies vastleggen om bij te houden van verkeer van en naar een virtuele machine te maken. Filters zijn beschikbaar voor de opnamesessie om te controleren of dat u alleen het verkeer die u wilt vastleggen. Pakketopname helpt op te sporen netwerk afwijkingen reactief en proactief. Andere toepassingen zijn onder andere het verzamelen van netwerkstatistieken, krijgt informatie over het netwerk beveiligingsrisico's voor foutopsporing van client-servercommunicaties en nog veel meer. Door op afstand activeren pakket opnamen, deze mogelijkheid kan vergemakkelijken de last van een pakketopname handmatig en op de gewenste machine, die kostbare tijd bespaart worden uitgevoerd.

Dit artikel wordt de volgende generatie CLI gebruikt voor de resource management-implementatiemodel, Azure CLI 2.0, die beschikbaar is voor Windows, Mac en Linux.

Als u wilt de stappen in dit artikel uitvoert, moet u [installeren van de Azure-opdrachtregelinterface voor Mac, Linux en Windows (Azure CLI)](https://docs.microsoft.com/cli/azure/install-az-cli2).

In dit artikel leert u de verschillende beheertaken die momenteel beschikbaar voor pakketopname zijn.

- [**Start een pakketopname**](#start-a-packet-capture)
- [**Stop de pakketopname van een**](#stop-a-packet-capture)
- [**Het vastleggen van een pakket verwijderen**](#delete-a-packet-capture)
- [**Een pakketopname downloaden**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u hebt de volgende bronnen:

- Een exemplaar van netwerk-Watcher in de regio die u wilt maken van een pakketopname
- Een virtuele machine met de pakket-capture-extensie is ingeschakeld.

> [!IMPORTANT]
> Pakketopname vereist een agent te worden uitgevoerd op de virtuele machine. De Agent wordt geïnstalleerd als een uitbreiding. Voor instructies voor het VM-extensies, gaat u naar [uitbreidingen van de virtuele Machine en functies](../virtual-machines/windows/extensions-features.md).

## <a name="install-vm-extension"></a>VM-extensie installeren

### <a name="step-1"></a>Stap 1

Voer de `az vm extension set` cmdlet het pakket capture-agent installeren op de virtuele gastmachine.

Voor Windows virtuele machines:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

Voor virtuele Linux-machines:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux--version 1.4
````

### <a name="step-2"></a>Stap 2

Om ervoor te zorgen dat de agent is geïnstalleerd, voer de `vm extension show` cmdlet en geef dit door de naam van de resource en de virtuele machine. Controleer de resulterende lijst om te controleren of dat de agent is geïnstalleerd.

```azurecli
az vm extension show --resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

Het volgende voorbeeld is een voorbeeld van het antwoord worden uitgevoerd`az vm extension show`

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

## <a name="start-a-packet-capture"></a>Start een pakketopname

Als de voorgaande stappen voltooid zijn, wordt het pakket vastleggen-agent geïnstalleerd op de virtuele machine.

### <a name="step-1"></a>Stap 1

De volgende stap is het exemplaar van de netwerk-Watcher ophalen. Tkan-naam van de netwerk-Watcher wordt doorgegeven aan de `az network watcher show` cmdlet in stap 4.

```azurecli
az network watcher show --resource-group resourceGroup --name networkWatcherName
```

### <a name="step-2"></a>Stap 2

Een opslagaccount ophalen. Dit opslagaccount wordt gebruikt voor het opslaan van het pakket capture-bestand.

```azurecli
azure storage account list
```

### <a name="step-3"></a>Stap 3

Filters kunnen worden gebruikt om te beperken van de gegevens die door het vastleggen van het pakket wordt opgeslagen. Het volgende voorbeeld stelt een pakketopname met verschillende filters.  De eerste drie filters voor het verzamelen van uitgaande TCP-verkeer alleen van het lokale IP 10.0.0.3 voor doelpoorten 20, 80 en 443.  Het laatste filter verzamelt UDP-verkeer.

```azurecli
az network watcher packet-capture create --resource-group {resoureceurceGroupName} --vm {vmName} --name packetCaptureName --storage-account gwteststorage123abc --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

Het volgende voorbeeld is de verwachte uitvoer uitvoeren van de `az network watcher packet-capture create` cmdlet.

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

## <a name="get-a-packet-capture"></a>Ophalen van een pakketopname

Met de `az network watcher packet-capture show` -cmdlet haalt de status van een pakketopname momenteel wordt uitgevoerd of voltooid is.

```azurecli
az network watcher packet-capture show --name packetCaptureName --location westcentralus
```

Het volgende voorbeeld wordt de uitvoer van de `az network watcher packet-capture show` cmdlet. Het volgende voorbeeld is nadat het vastleggen voltooid is. De waarde PacketCaptureStatus is gestopt met een StopReason TimeExceeded. Deze waarde geeft aan dat het vastleggen van het pakket voltooid is en de tijd wordt uitgevoerd.

```
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
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "provisioningState": "Succeeded",
  "resourceGroup": "NetworkWatcherRG",
  "storageLocation": {
    "filePath": null,
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/gwteststorage123abc",
    "storagePath": "https://gwteststorage123abc.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/{resourceGroupName}/providers/microsoft.compute/virtualmachines/{vmName}/2017/05/25/packetcapt
ure_16_22_34_630.cap"
  },
  "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}",
  "timeLimitInSeconds": 18000,
  "totalBytesPerSession": 1073741824
}
```

## <a name="stop-a-packet-capture"></a>Stop de pakketopname van een

Door het uitvoeren van de `az network watcher packet-capture stop` als een opnamesessie Bezig het is-cmdlet is gestopt.

```azurecli
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> De cmdlet retourneert geen antwoord wanneer uitgevoerd op een actieve opnamesessie of een bestaande sessie die al is gestopt.

## <a name="delete-a-packet-capture"></a>Het vastleggen van een pakket verwijderen

```azurecli
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
```

> [!NOTE]
> Als u het vastleggen van een pakket verwijdert, verwijdert niet het bestand in het opslagaccount.

## <a name="download-a-packet-capture"></a>Een pakketopname downloaden

Zodra de sessie van uw pakket vastleggen is voltooid, kan het bestand vastleggen kan worden geüpload naar de blob-opslag of naar een lokaal bestand op de virtuele machine. De opslaglocatie van de pakketopname is gedefinieerd bij het maken van de sessie. Een handig hulpmiddel voor toegang tot deze capture-bestanden opgeslagen in een opslagaccount is Microsoft Azure Storage Explorer, die u kunt hier downloaden: http://storageexplorer.com/

Als een opslagaccount is opgegeven, worden bestanden voor pakket worden opgeslagen in een opslagaccount op de volgende locatie:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het automatiseren van pakket opnamen met waarschuwingen van de virtuele machine met weer te geven [maken van een waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md)

Als bepaalde verkeer is toegestaan in of buiten uw virtuele machine in via vinden [controleren IP-stroom controleren](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
