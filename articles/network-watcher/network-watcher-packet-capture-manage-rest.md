---
title: Pakketopname beheren met Azure Network Watcher - REST-API | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u de functie voor het vastleggen van pakketten van Network Watcher met behulp van REST-API van Azure beheren
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 53fe0324-835f-4005-afc8-145eeb314aeb
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 48688c58115a002508f9df0b242ea3e2d2d94719
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57855432"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-rest-api"></a>Pakketopname beheren met Azure Network Watcher met behulp van REST-API van Azure

> [!div class="op_single_selector"]
> - [Azure-portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [Azure-CLI](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST API](network-watcher-packet-capture-manage-rest.md)

Network Watcher packet-capture kunt u capture-sessies voor het volgen van verkeer van en naar een virtuele machine maken. Filters zijn beschikbaar voor de opnamesessie om te controleren of dat u alleen het verkeer die u wilt vastleggen. Pakketopname helpt bij het opsporen van afwijkingen netwerk reactief en proactief. Andere toepassingen zijn onder andere de netwerkstatistieken, het verkrijgen van informatie over het netwerk indringers, fouten opsporen in client-servercommunicatie en nog veel meer verzamelen. Door vereenvoudigt deze mogelijkheid voor het extern activeren pakketopnamen, de belasting van het uitvoeren van een pakketopname handmatig en op de gewenste computer, wat kostbare tijd bespaart.

In dit artikel doorloopt u de verschillende beheertaken die momenteel beschikbaar voor de pakketopname zijn.

- [**Een pakketopname ophalen**](#get-a-packet-capture)
- [**Lijst van alle pakketopnamen**](#list-all-packet-captures)
- [**De status van een pakketopname opvragen**](#query-packet-capture-status)
- [**Een pakketopname starten**](#start-packet-capture)
- [**Een pakketopname stoppen**](#stop-packet-capture)
- [**Een pakketopname verwijderen**](#delete-packet-capture)

## <a name="before-you-begin"></a>Voordat u begint

In dit scenario, moet u de netwerk-Watcher Rest-API voor het uitvoeren van de IP-stroom controleren aanroepen. ARMclient wordt gebruikt voor het aanroepen van de REST-API met behulp van PowerShell. ARMClient is gevonden op chocolatey op [ARMClient op Chocolatey](https://chocolatey.org/packages/ARMClient)

In dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een Network Watcher](network-watcher-create.md) te maken van een Network Watcher.

> Pakketopname is vereist voor een VM-extensie `AzureNetworkWatcherExtension`. Ga voor het installeren van de extensie op een Windows-VM naar [Azure Network Watcher-Agent de extensie van de virtuele machine voor Windows](../virtual-machines/windows/extensions-nwa.md) en voor Linux-VM naar [Azure Network Watcher-Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="log-in-with-armclient"></a>Meld u aan met ARMClient

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Een virtuele machine ophalen

Voer het volgende script om te retourneren van een virtuele machine. Deze informatie is nodig voor het starten van een pakketopname.

De volgende code moet variabelen:

- **subscriptionId** -de abonnements-id kan ook worden opgehaald met de **Get-AzureRMSubscription** cmdlet.
- **resourceGroupName** -de naam van een resourcegroep met virtuele machines.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

De id van de virtuele machine wordt gebruikt in de volgende uitvoer in het volgende voorbeeld.

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```


## <a name="get-a-packet-capture"></a>Een pakketopname ophalen

Het volgende voorbeeld wordt de status van een enkele pakketopname

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/querystatus?api-version=2016-12-01"
```

De volgende antwoorden zijn voorbeelden van een typische antwoord geretourneerd bij het opvragen van de status van een pakketopname.

```json
{
  "name": "TestPacketCapture5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
  "captureStartTime": "2016-12-06T17:20:01.5671279Z",
  "packetCaptureStatus": "Running",
  "packetCaptureError": []
}
```

```json
{
  "name": "TestPacketCapture5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
  "captureStartTime": "2016-12-06T17:20:01.5671279Z",
  "packetCaptureStatus": "Stopped",
  "stopReason": "TimeExceeded",
  "packetCaptureError": []
}
```

## <a name="list-all-packet-captures"></a>Lijst van alle pakketopnamen

Het volgende voorbeeld haalt alle sessies van pakket vastleggen in een regio.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
armclient get "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures?api-version=2016-12-01"
```

Het volgende antwoord is een voorbeeld van een typische antwoord geretourneerd bij het ophalen van alle pakketten worden vastgelegd

```json
{
  "value": [
    {
      "name": "TestPacketCapture6",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
      "etag": "W/\"091762e1-c23f-448b-89d5-37cf56e4c045\"",
      "properties": {
        "provisioningState": "Succeeded",
        "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute/virtualMachines/ContosoVM",
        "bytesToCapturePerPacket": 0,
        "totalBytesPerSession": 1073741824,
        "timeLimitInSeconds": 60,
        "storageLocation": {
          "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Storage/storageAccounts/contosoexamplergdiag374",
          "storagePath": "https://contosoexamplergdiag374.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/contosoexamplerg/providers/microsoft.compute/virtualmachines/contosovm/2016/12/06/packetcap
ture_17_19_53_056.cap",
          "filePath": "c:\\temp\\packetcapture.cap"
        },
        "filters": [
          {
            "protocol": "Any",
            "localIPAddress": "",
            "localPort": "",
            "remoteIPAddress": "",
            "remotePort": ""
          }
        ]
      }
    },
    {
      "name": "TestPacketCapture7",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture7",
      "etag": "W/\"091762e1-c23f-448b-89d5-37cf56e4c045\"",
      "properties": {
        "provisioningState": "Failed",
        "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute/virtualMachines/ContosoVM",
        "bytesToCapturePerPacket": 0,
        "totalBytesPerSession": 1073741824,
        "timeLimitInSeconds": 60,
        "storageLocation": {
          "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Storage/storageAccounts/contosoexamplergdiag374",
          "storagePath": "https://contosoexamplergdiag374.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/contosoexamplerg/providers/microsoft.compute/virtualmachines/contosovm/2016/12/06/packetcap
ture_17_23_15_364.cap",
          "filePath": "c:\\temp\\packetcapture.cap"
        },
        "filters": [
          {
            "protocol": "Any",
            "localIPAddress": "",
            "localPort": "",
            "remoteIPAddress": "",
            "remotePort": ""
          }
        ]
      }
    }
  ]
}
```

## <a name="query-packet-capture-status"></a>Status van pakket vastleggen

Het volgende voorbeeld haalt alle sessies van pakket vastleggen in een regio.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
armclient get "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/querystatus?api-version=2016-12-01"
```

Het volgende antwoord is een voorbeeld van een typische antwoord geretourneerd bij het opvragen van de status van een pakketopname.

```json
{
    "name": "vm1PacketCapture",
    "id": "/subscriptions/{guid}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkWatchers/{networkWatcherName}/packetCaptures/{packetCaptureName}",
    "captureStartTime" : "9/7/2016 12:35:24PM",
    "packetCaptureStatus" : "Stopped",
    "stopReason" : "TimeExceeded",
    "packetCaptureError" : [ ]
}
```

## <a name="start-packet-capture"></a>Pakketopname starten

Het volgende voorbeeld wordt een pakketopname op een virtuele machine.  Het voorbeeld met parameters om toe te staan voor flexibiliteit bij het maken van een voorbeeld.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
$storageaccountname = "contosoexamplergdiag374"
$vmName = "ContosoVM"
$bytestoCaptureperPacket = "0"
$bytesPerSession = "1073741824"
$captureTimeinSeconds = "60"
$localIP = ""
$localPort = "" # Examples are: 80, or 80-120
$remoteIP = ""
$remotePort = "" # Examples are: 80, or 80-120
$protocol = "" # Valid values are TCP, UDP and Any.
$targetUri = "" # Example: /subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.compute/virtualMachine/$vmName
$storageId = "" # Example: "https://mytestaccountname.blob.core.windows.net/capture/vm1Capture.cap"
$storagePath = ""
$localFilePath = "c:\\temp\\packetcapture.cap" # Example: "d:\capture\vm1Capture.cap"

$requestBody = @"
{
    'properties':  {
                       'target':  '/${targetUri}',
                       'bytesToCapturePerPacket':  '${bytestoCaptureperPacket}',
                       'totalBytesPerSession':  '${bytesPerSession}',
                       'timeLimitinSeconds':  '${captureTimeinSeconds}',
                       'storageLocation':  {
                                               'storageId':  '${storageId}',
                                               'storagePath':  '${storagePath}',
                                               'filePath':  '${localFilePath}'
                                           },
                       'filters':  [
                                       {
                                           'protocol':  '${protocol}',
                                           'localIPAddress':  '${localIP}',
                                           'localPort':  '${localPort}',
                                           'remoteIPAddress':  '${remoteIP}',
                                           'remotePort':  '${remotePort}'
                                       }
                                   ]
                   }
}
"@

armclient PUT "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}?api-version=2016-07-01" $requestbody
```

## <a name="stop-packet-capture"></a>Pakketopname stoppen

Het volgende voorbeeld wordt een pakketopname op een virtuele machine gestopt.  Het voorbeeld met parameters om toe te staan voor flexibiliteit bij het maken van een voorbeeld.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/stop?api-version=2016-12-01"
```

## <a name="delete-packet-capture"></a>Pakketopname verwijderen

Het volgende voorbeeld wordt een pakketopname op een virtuele machine.  Het voorbeeld met parameters om toe te staan voor flexibiliteit bij het maken van een voorbeeld.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"

armclient delete "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}?api-version=2016-12-01"
```

> [!NOTE]
> Een pakketopname verwijderen verwijderen het bestand in het opslagaccount niet

## <a name="next-steps"></a>Volgende stappen

Zie voor instructies over het downloaden van bestanden vanuit azure storage-accounts, [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Een ander hulpmiddel dat kan worden gebruikt, is Storage Explorer. Meer informatie over Storage Explorer Hier vindt op de volgende koppeling: [Storage Explorer](https://storageexplorer.com/)

Meer informatie over het automatiseren van pakketopnamen met waarschuwingen van de virtuele machine via [maken van een waarschuwing geactiveerd pakketopname](network-watcher-alert-triggered-packet-capture.md)













