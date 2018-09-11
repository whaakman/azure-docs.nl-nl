---
title: Pakketopname beheren met Azure Network Watcher - PowerShell | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u voor het beheren van de functie voor het vastleggen van pakketten van Network Watcher met behulp van PowerShell
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 04d82085-c9ea-4ea1-b050-a3dd4960f3aa
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: d768367b8e35c30b1e8cb1646e6cbfceb6151dec
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348290"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-powershell"></a>Pakketopname beheren met Azure Network Watcher met behulp van PowerShell

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [Azure-CLI](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST-API](network-watcher-packet-capture-manage-rest.md)

Network Watcher packet-capture kunt u capture-sessies voor het volgen van verkeer van en naar een virtuele machine maken. Filters zijn beschikbaar voor de opnamesessie om te controleren of dat u alleen het verkeer die u wilt vastleggen. Pakketopname helpt bij het opsporen van afwijkingen netwerk reactief en proactief. Andere toepassingen zijn onder andere de netwerkstatistieken, het verkrijgen van informatie over het netwerk indringers, fouten opsporen in client-servercommunicatie en nog veel meer verzamelen. Door vereenvoudigt deze mogelijkheid voor het extern activeren pakketopnamen, de belasting van het uitvoeren van een pakketopname handmatig en op de gewenste computer, wat kostbare tijd bespaart.

In dit artikel doorloopt u de verschillende beheertaken die momenteel beschikbaar voor de pakketopname zijn.

- [**Een pakketopname starten**](#start-a-packet-capture)
- [**Een pakketopname stoppen**](#stop-a-packet-capture)
- [**Een pakketopname verwijderen**](#delete-a-packet-capture)
- [**Een pakketopname downloaden**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u hebt de volgende bronnen:

* Een exemplaar van Network Watcher in de regio die u wilt maken van een pakketopname

* Een virtuele machine met de packet capture-uitbreiding is ingeschakeld.

> [!IMPORTANT]
> Pakketopname is vereist voor een VM-extensie `AzureNetworkWatcherExtension`. Ga voor het installeren van de extensie op een Windows-VM naar [Azure Network Watcher-Agent de extensie van de virtuele machine voor Windows](../virtual-machines/windows/extensions-nwa.md) en voor Linux-VM naar [Azure Network Watcher-Agent de extensie van de virtuele machine voor Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="install-vm-extension"></a>VM-extensie installeren

### <a name="step-1"></a>Stap 1

```powershell
$VM = Get-AzureRmVM -ResourceGroupName testrg -Name VM1
```

### <a name="step-2"></a>Stap 2

Het volgende voorbeeld wordt de extensie-informatie die nodig is om uit te voeren de `Set-AzureRmVMExtension` cmdlet. Deze cmdlet wordt de packet capture-agent op de virtuele gastmachine.

> [!NOTE]
> De `Set-AzureRmVMExtension` cmdlet kan enkele minuten duren.

Voor Windows virtual machines:

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentWindows -Version 1.4.585.2
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

Voor virtuele Linux-machines:

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentLinux -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
````

Het volgende voorbeeld is een geslaagde respons nadat de `Set-AzureRmVMExtension` cmdlet.

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
```

### <a name="step-3"></a>Stap 3

Uitvoeren om ervoor te zorgen dat de agent is geïnstalleerd, de `Get-AzureRmVMExtension` cmdlet en geeft die door de naam van de virtuele machine en de naam van de uitbreiding.

```powershell
Get-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -VMName $VM.Name -Name $ExtensionName
```

Het volgende voorbeeld wordt een voorbeeld van het antwoord niet worden uitgevoerd `Get-AzureRmVMExtension`

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

## <a name="start-a-packet-capture"></a>Een pakketopname starten

Als de voorgaande stappen voltooid zijn, worden de packet capture-agent is geïnstalleerd op de virtuele machine.

### <a name="step-1"></a>Stap 1

De volgende stap is om op te halen van de Network Watcher-exemplaar. Deze variabele wordt doorgegeven aan de `New-AzureRmNetworkWatcherPacketCapture` cmdlet in stap 4.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName  
```

### <a name="step-2"></a>Stap 2

Ophalen van een storage-account. Dit opslagaccount wordt gebruikt voor het opslaan van het pakket vastleggen-bestand.

```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName testrg -Name testrgsa123
```

### <a name="step-3"></a>Stap 3

Filters kunnen worden gebruikt om de gegevens die zijn opgeslagen door de pakketopname te beperken. Hiermee stelt u het volgende voorbeeld van twee filters.  Één filter verzamelt de uitgaande TCP-verkeer alleen vanuit het lokale IP 10.0.0.3 naar doelpoorten 20, 80 en 443.  Het tweede filter verzamelt alleen UDP-verkeer.

```powershell
$filter1 = New-AzureRmPacketCaptureFilterConfig -Protocol TCP -RemoteIPAddress "1.1.1.1-255.255.255.255" -LocalIPAddress "10.0.0.3" -LocalPort "1-65535" -RemotePort "20;80;443"
$filter2 = New-AzureRmPacketCaptureFilterConfig -Protocol UDP
```

> [!NOTE]
> Meerdere filters kunnen worden gedefinieerd voor een pakketopname.

### <a name="step-4"></a>Stap 4

Voer de `New-AzureRmNetworkWatcherPacketCapture` cmdlet voor het starten van het pakket vastleggen, de vereiste waarden doorgeven in de voorgaande stappen hebt opgehaald.
```powershell

New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $vm.Id -PacketCaptureName "PacketCaptureTest" -StorageAccountId $storageAccount.id -TimeLimitInSeconds 60 -Filter $filter1, $filter2
```

Het volgende voorbeeld is de verwachte uitvoer wordt uitgevoerd de `New-AzureRmNetworkWatcherPacketCapture` cmdlet.

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

## <a name="get-a-packet-capture"></a>Een pakketopname ophalen

Met de `Get-AzureRmNetworkWatcherPacketCapture` cmdlet, wordt de status van een pakketopname die momenteel wordt uitgevoerd of voltooid.

```powershell
Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

Het volgende voorbeeld wordt de uitvoer van de `Get-AzureRmNetworkWatcherPacketCapture` cmdlet. Het volgende voorbeeld wordt na het vastleggen voltooid is. De waarde PacketCaptureStatus is gestopt, met een StopReason TimeExceeded. Deze waarde geeft aan dat de pakketopname voltooid is en de tijd hebt uitgevoerd.
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

## <a name="stop-a-packet-capture"></a>Een pakketopname stoppen

Door het uitvoeren van de `Stop-AzureRmNetworkWatcherPacketCapture` cmdlet, als een opnamesessie bezig is is gestopt.

```powershell
Stop-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> De cmdlet retourneert geen reactie wanneer die worden uitgevoerd op een actieve opnamesessie of een bestaande sessie die al is gestopt.

## <a name="delete-a-packet-capture"></a>Een pakketopname verwijderen

```powershell
Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
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

Zoeken of bepaalde verkeer is toegestaan in orr buiten uw virtuele machine door naar de pagina [controleren IP-stroom controleren](diagnose-vm-network-traffic-filtering-problem.md)

<!-- Image references -->














