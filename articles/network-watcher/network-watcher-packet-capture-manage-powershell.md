---
title: Pakket opnamen met Azure-netwerk-Watcher - PowerShell beheren | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u de functie voor het vastleggen van pakket van netwerk-Watcher met behulp van PowerShell beheren
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 04d82085-c9ea-4ea1-b050-a3dd4960f3aa
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: b27e0684b0914764f22b59e050e75c7be3a82cc6
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-powershell"></a>Pakket opnamen beheren met Azure met behulp van PowerShell netwerk-Watcher

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST-API](network-watcher-packet-capture-manage-rest.md)

Netwerk-Watcher pakketopname kunt u om sessies vastleggen om bij te houden van verkeer van en naar een virtuele machine te maken. Filters zijn beschikbaar voor de opnamesessie om te controleren of dat u alleen het verkeer die u wilt vastleggen. Pakketopname helpt op te sporen netwerk afwijkingen reactief en proactief. Andere toepassingen zijn onder andere het verzamelen van netwerkstatistieken, krijgt informatie over het netwerk beveiligingsrisico's voor foutopsporing van client-servercommunicaties en nog veel meer. Door op afstand activeren pakket opnamen, deze mogelijkheid kan vergemakkelijken de last van een pakketopname handmatig en op de gewenste machine, die kostbare tijd bespaart worden uitgevoerd.

In dit artikel leert u de verschillende beheertaken die momenteel beschikbaar voor pakketopname zijn.

- [**Start een pakketopname**](#start-a-packet-capture)
- [**Stop de pakketopname van een**](#stop-a-packet-capture)
- [**Het vastleggen van een pakket verwijderen**](#delete-a-packet-capture)
- [**Een pakketopname downloaden**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u hebt de volgende bronnen:

* Een exemplaar van netwerk-Watcher in de regio die u wilt maken van een pakketopname

* Een virtuele machine met de pakket-capture-extensie is ingeschakeld.

> [!IMPORTANT]
> Pakketopname vereist dat de extensie van een virtuele machine `AzureNetworkWatcherExtension`. Voor het installeren van de extensie op een Windows-virtuele machine gaat u naar [extensie voor het virtuele machine voor Windows Azure-netwerk-Watcher Agent](../virtual-machines/windows/extensions-nwa.md) en voor Linux-VM naar [Azure-netwerk-Watcher Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="install-vm-extension"></a>VM-extensie installeren

### <a name="step-1"></a>Stap 1

```powershell
$VM = Get-AzureRmVM -ResourceGroupName testrg -Name VM1
```

### <a name="step-2"></a>Stap 2

Het volgende voorbeeld wordt de extensie-informatie die nodig zijn voor de `Set-AzureRmVMExtension` cmdlet. Deze cmdlet installeert de agent van pakket vastleggen op de virtuele gastmachine.

> [!NOTE]
> De `Set-AzureRmVMExtension` cmdlet kan enkele minuten duren.

Voor Windows virtuele machines:

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentWindows -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

Voor virtuele Linux-machines:

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentLinux -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
````

Het volgende voorbeeld is een geslaagde reactie nadat de `Set-AzureRmVMExtension` cmdlet.

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
```

### <a name="step-3"></a>Stap 3

Om ervoor te zorgen dat de agent is geïnstalleerd, voer de `Get-AzureRmVMExtension` cmdlet en geef dit door de naam van de virtuele machine en de extensienaam.

```powershell
Get-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -VMName $VM.Name -Name $ExtensionName
```

Het volgende voorbeeld is een voorbeeld van het antwoord worden uitgevoerd`Get-AzureRmVMExtension`

```
ResourceGroupName       : testrg
VMName                  : testvm1
Name                    : AzureNetworkWatcherExtension
Location                : westcentralus
Etag                    : null
Publisher               : Microsoft.Azure.NetworkWatcher
ExtensionType           : NetworkWatcherAgentWindows
TypeHandlerVersion      : 1.4
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1/
                          extensions/AzureNetworkWatcherExtension
PublicSettings          : 
ProtectedSettings       : 
ProvisioningState       : Succeeded
Statuses                : 
SubStatuses             : 
AutoUpgradeMinorVersion : True
ForceUpdateTag          : 
```

## <a name="start-a-packet-capture"></a>Start een pakketopname

Als de voorgaande stappen voltooid zijn, wordt het pakket vastleggen-agent geïnstalleerd op de virtuele machine.

### <a name="step-1"></a>Stap 1

De volgende stap is het exemplaar van de netwerk-Watcher ophalen. Deze variabele wordt doorgegeven aan de `New-AzureRmNetworkWatcherPacketCapture` cmdlet in stap 4.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName  
```

### <a name="step-2"></a>Stap 2

Een opslagaccount ophalen. Dit opslagaccount wordt gebruikt voor het opslaan van het pakket capture-bestand.

```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName testrg -Name testrgsa123
```

### <a name="step-3"></a>Stap 3

Filters kunnen worden gebruikt om te beperken van de gegevens die door het vastleggen van het pakket wordt opgeslagen. Het volgende voorbeeld heeft twee filters ingesteld.  Een filter verzamelt uitgaande TCP-verkeer alleen van het lokale IP 10.0.0.3 naar doelpoorten 20, 80 en 443.  Het tweede filter verzamelt UDP-verkeer.

```powershell
$filter1 = New-AzureRmPacketCaptureFilterConfig -Protocol TCP -RemoteIPAddress "1.1.1.1-255.255.255" -LocalIPAddress "10.0.0.3" -LocalPort "1-65535" -RemotePort "20;80;443"
$filter2 = New-AzureRmPacketCaptureFilterConfig -Protocol UDP
```

> [!NOTE]
> Meerdere filters kunnen worden gedefinieerd voor het vastleggen van een pakket.

### <a name="step-4"></a>Stap 4

Voer de `New-AzureRmNetworkWatcherPacketCapture` cmdlet starten van het pakket vastleggen, doorgeven van de vereiste waarden in de voorgaande stappen opgehaald.
```powershell

New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $vm.Id -PacketCaptureName "PacketCaptureTest" -StorageAccountId $storageAccount.id -TimeLimitInSeconds 60 -Filter $filter1, $filter2
```

Het volgende voorbeeld is de verwachte uitvoer uitvoeren van de `New-AzureRmNetworkWatcherPacketCapture` cmdlet.

```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"3bf27278-8251-4651-9546-c7f369855e4e"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]


```

## <a name="get-a-packet-capture"></a>Ophalen van een pakketopname

Met de `Get-AzureRmNetworkWatcherPacketCapture` -cmdlet haalt de status van een pakketopname momenteel wordt uitgevoerd of voltooid is.

```powershell
Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

Het volgende voorbeeld wordt de uitvoer van de `Get-AzureRmNetworkWatcherPacketCapture` cmdlet. Het volgende voorbeeld is nadat het vastleggen voltooid is. De waarde PacketCaptureStatus is gestopt met een StopReason TimeExceeded. Deze waarde geeft aan dat het vastleggen van het pakket voltooid is en de tijd wordt uitgevoerd.
```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"4b9a81ed-dc63-472e-869e-96d7166ccb9b"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]
CaptureStartTime        : 2/1/2017 10:43:01 PM
PacketCaptureStatus     : Stopped
StopReason              : TimeExceeded
PacketCaptureError      : []
```

## <a name="stop-a-packet-capture"></a>Stop de pakketopname van een

Door het uitvoeren van de `Stop-AzureRmNetworkWatcherPacketCapture` als een opnamesessie Bezig het is-cmdlet is gestopt.

```powershell
Stop-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> De cmdlet retourneert geen antwoord wanneer uitgevoerd op een actieve opnamesessie of een bestaande sessie die al is gestopt.

## <a name="delete-a-packet-capture"></a>Het vastleggen van een pakket verwijderen

```powershell
Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
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

Als bepaalde verkeer is toegestaan in orr buiten uw virtuele machine in via vinden [controleren IP-stroom controleren](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->














