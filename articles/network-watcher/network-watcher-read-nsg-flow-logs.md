---
title: Stroomlogboeken lezen | Microsoft Docs
description: In dit artikel ziet u hoe NSG-stroomlogboeken parseren
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: kumud
ms.openlocfilehash: becae0f085fcaf4b0d0c7b29e102aaa3186fb85e
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67653748"
---
# <a name="read-nsg-flow-logs"></a>NSG-stroomlogboeken lezen

Informatie over het lezen van NSG-stroom logboeken vermeldingen met PowerShell.

NSG-stroomlogboeken zijn opgeslagen in een opslagaccount in [blok-blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs). Blok-blobs bestaan uit kleinere blokken. Elk logboek is een afzonderlijke blok-blob die elk uur wordt gegenereerd. Nieuwe logboeken worden gegenereerd per uur, de logboeken worden bijgewerkt met nieuwe gegevens om de paar minuten met de meest recente gegevens. In dit artikel leert u hoe u onderdelen van de logboeken van de stroom lezen.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario"></a>Scenario

In het volgende scenario hebt u een voorbeeld van de flow-logboek die zijn opgeslagen in een storage-account. Leert u hoe u de meest recente gebeurtenissen in NSG-stroomlogboeken selectief kan lezen. In dit artikel gebruikt u PowerShell, maar de concepten die in het artikel worden besproken, zijn niet beperkt tot de programmeertaal en zijn van toepassing op alle talen ondersteund door de Azure Storage-API's.

## <a name="setup"></a>Instellen

Voordat u begint, kunt u Network Security Group Flow-logboekregistratie is ingeschakeld op een of meer Netwerkbeveiligingsgroepen in uw account moet hebben. Zie voor instructies over het inschakelen van stroomlogboeken voor netwerkbeveiliging, het volgende artikel: [Inleiding tot stroomlogboeken voor Netwerkbeveiligingsgroepen](network-watcher-nsg-flow-logging-overview.md).

## <a name="retrieve-the-block-list"></a>De lijst met geblokkeerde websites ophalen

De volgende PowerShell stelt u de variabelen die nodig zijn voor de NSG stroom log blob query en de blokken in vermelden de [CloudBlockBlob](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblockblob) blok-blob. Werk het script bevat de geldige waarden voor uw omgeving.

```powershell
function Get-NSGFlowLogCloudBlockBlob {
    [CmdletBinding()]
    param (
        [string] [Parameter(Mandatory=$true)] $subscriptionId,
        [string] [Parameter(Mandatory=$true)] $NSGResourceGroupName,
        [string] [Parameter(Mandatory=$true)] $NSGName,
        [string] [Parameter(Mandatory=$true)] $storageAccountName,
        [string] [Parameter(Mandatory=$true)] $storageAccountResourceGroup,
        [string] [Parameter(Mandatory=$true)] $macAddress,
        [datetime] [Parameter(Mandatory=$true)] $logTime
    )

    process {
        # Retrieve the primary storage account key to access the NSG logs
        $StorageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $storageAccountResourceGroup -Name $storageAccountName).Value[0]

        # Setup a new storage context to be used to query the logs
        $ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

        # Container name used by NSG flow logs
        $ContainerName = "insights-logs-networksecuritygroupflowevent"

        # Name of the blob that contains the NSG flow log
        $BlobName = "resourceId=/SUBSCRIPTIONS/${subscriptionId}/RESOURCEGROUPS/${NSGResourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/${NSGName}/y=$($logTime.Year)/m=$(($logTime).ToString("MM"))/d=$(($logTime).ToString("dd"))/h=$(($logTime).ToString("HH"))/m=00/macAddress=$($macAddress)/PT1H.json"

        # Gets the storage blog
        $Blob = Get-AzStorageBlob -Context $ctx -Container $ContainerName -Blob $BlobName

        # Gets the block blog of type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from the storage blob
        $CloudBlockBlob = [Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob] $Blob.ICloudBlob

        #Return the Cloud Block Blob
        $CloudBlockBlob
    }
}

function Get-NSGFlowLogBlockList  {
    [CmdletBinding()]
    param (
        [Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob] [Parameter(Mandatory=$true)] $CloudBlockBlob
    )
    process {
        # Stores the block list in a variable from the block blob.
        $blockList = $CloudBlockBlob.DownloadBlockList()

        # Return the Block List
        $blockList
    }
}


$CloudBlockBlob = Get-NSGFlowLogCloudBlockBlob -subscriptionId "yourSubscriptionId" -NSGResourceGroupName "FLOWLOGSVALIDATIONWESTCENTRALUS" -NSGName "V2VALIDATIONVM-NSG" -storageAccountName "yourStorageAccountName" -storageAccountResourceGroup "ml-rg" -macAddress "000D3AF87856" -logTime "11/11/2018 03:00" 

$blockList = Get-NSGFlowLogBlockList -CloudBlockBlob $CloudBlockBlob
```

De `$blockList` variabele retourneert een lijst van de blokken in de blob. Elk blok-blob bevat ten minste twee blokken.  Het eerste vereiste blok heeft een lengte van `12` bytes, dit blok bevat de haakjes openen van het json-logboek. Het andere blok is de haakjes en heeft een lengte van `2` bytes.  Zoals u het volgende voorbeeld-logboek ziet bevat zeven vermeldingen, elk een afzonderlijke vermelding wordt. Alle nieuwe vermeldingen in het logboek worden toegevoegd aan het einde na de laatste blok.

```
Name                                         Length Committed
----                                         ------ ---------
ZDk5MTk5N2FkNGE0MmY5MTk5ZWViYjA0YmZhODRhYzY=     12      True
NzQxNDA5MTRhNDUzMGI2M2Y1MDMyOWZlN2QwNDZiYzQ=   2685      True
ODdjM2UyMWY3NzFhZTU3MmVlMmU5MDNlOWEwNWE3YWY=   2586      True
ZDU2MjA3OGQ2ZDU3MjczMWQ4MTRmYWNhYjAzOGJkMTg=   2688      True
ZmM3ZWJjMGQ0ZDA1ODJlOWMyODhlOWE3MDI1MGJhMTc=   2775      True
ZGVkYTc4MzQzNjEyMzlmZWE5MmRiNjc1OWE5OTc0OTQ=   2676      True
ZmY2MjUzYTIwYWIyOGU1OTA2ZDY1OWYzNmY2NmU4ZTY=   2777      True
Mzk1YzQwM2U0ZWY1ZDRhOWFlMTNhYjQ3OGVhYmUzNjk=   2675      True
ZjAyZTliYWE3OTI1YWZmYjFmMWI0MjJhNzMxZTI4MDM=      2      True
```

## <a name="read-the-block-blob"></a>Lees de blok-blob

Vervolgens moet u om te lezen de `$blocklist` variabele om de gegevens te halen. In dit voorbeeld die wordt de blokkeringslijst hebt doorlopen, het aantal bytes lezen uit elk blok en ze in een matrix van artikel. Gebruik de [DownloadRangeToByteArray](/dotnet/api/microsoft.azure.storage.blob.cloudblob.downloadrangetobytearray) methode voor het ophalen van de gegevens.

```powershell
function Get-NSGFlowLogReadBlock  {
    [CmdletBinding()]
    param (
        [System.Array] [Parameter(Mandatory=$true)] $blockList,
        [Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob] [Parameter(Mandatory=$true)] $CloudBlockBlob

    )
    # Set the size of the byte array to the largest block
    $maxvalue = ($blocklist | measure Length -Maximum).Maximum

    # Create an array to store values in
    $valuearray = @()

    # Define the starting index to track the current block being read
    $index = 0

    # Loop through each block in the block list
    for($i=0; $i -lt $blocklist.count; $i++)
    {
        # Create a byte array object to story the bytes from the block
        $downloadArray = New-Object -TypeName byte[] -ArgumentList $maxvalue

        # Download the data into the ByteArray, starting with the current index, for the number of bytes in the current block. Index is increased by 3 when reading to remove preceding comma.
        $CloudBlockBlob.DownloadRangeToByteArray($downloadArray,0,$index, $($blockList[$i].Length-1)) | Out-Null

        # Increment the index by adding the current block length to the previous index
        $index = $index + $blockList[$i].Length

        # Retrieve the string from the byte array

        $value = [System.Text.Encoding]::ASCII.GetString($downloadArray)

        # Add the log entry to the value array
        $valuearray += $value
    }
    #Return the Array
    $valuearray
}
$valuearray = Get-NSGFlowLogReadBlock -blockList $blockList -CloudBlockBlob $CloudBlockBlob
```

Nu de `$valuearray` matrix de tekenreekswaarde bevat van elk blok. Als u wilt controleren of de vermelding, krijgen de tweede op de laatste waarde van de matrix door te voeren `$valuearray[$valuearray.Length-2]`. U wilt niet de laatste waarde, omdat het een haakje sluiten.

De resultaten van deze waarde worden weergegeven in het volgende voorbeeld:

```json
        {
             "time": "2017-06-16T20:59:43.7340000Z",
             "systemId": "5f4d02d3-a7d0-4ed4-9ce8-c0ae9377951c",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/CONTOSORG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/CONTOSONSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_AllowInternetOutBound","flows":[{"mac":"000D3A18077E","flowTuples":["1497646722,10.0.0.4,168.62.32.14,44904,443,T,O,A","1497646722,10.0.0.4,52.240.48.24,45218,443,T,O,A","1497646725,10.
0.0.4,168.62.32.14,44910,443,T,O,A","1497646725,10.0.0.4,52.240.48.24,45224,443,T,O,A","1497646728,10.0.0.4,168.62.32.14,44916,443,T,O,A","1497646728,10.0.0.4,52.240.48.24,45230,443,T,O,A","1497646732,10.0.0.4,168.62.32.14,44922,443,T,O,A","14976
46732,10.0.0.4,52.240.48.24,45236,443,T,O,A","1497646735,10.0.0.4,168.62.32.14,44928,443,T,O,A","1497646735,10.0.0.4,52.240.48.24,45242,443,T,O,A","1497646738,10.0.0.4,168.62.32.14,44934,443,T,O,A","1497646738,10.0.0.4,52.240.48.24,45248,443,T,O,
A","1497646742,10.0.0.4,168.62.32.14,44942,443,T,O,A","1497646742,10.0.0.4,52.240.48.24,45256,443,T,O,A","1497646745,10.0.0.4,168.62.32.14,44948,443,T,O,A","1497646745,10.0.0.4,52.240.48.24,45262,443,T,O,A","1497646749,10.0.0.4,168.62.32.14,44954
,443,T,O,A","1497646749,10.0.0.4,52.240.48.24,45268,443,T,O,A","1497646753,10.0.0.4,168.62.32.14,44960,443,T,O,A","1497646753,10.0.0.4,52.240.48.24,45274,443,T,O,A","1497646756,10.0.0.4,168.62.32.14,44966,443,T,O,A","1497646756,10.0.0.4,52.240.48
.24,45280,443,T,O,A","1497646759,10.0.0.4,168.62.32.14,44972,443,T,O,A","1497646759,10.0.0.4,52.240.48.24,45286,443,T,O,A","1497646763,10.0.0.4,168.62.32.14,44978,443,T,O,A","1497646763,10.0.0.4,52.240.48.24,45292,443,T,O,A","1497646766,10.0.0.4,
168.62.32.14,44984,443,T,O,A","1497646766,10.0.0.4,52.240.48.24,45298,443,T,O,A","1497646769,10.0.0.4,168.62.32.14,44990,443,T,O,A","1497646769,10.0.0.4,52.240.48.24,45304,443,T,O,A","1497646773,10.0.0.4,168.62.32.14,44996,443,T,O,A","1497646773,
10.0.0.4,52.240.48.24,45310,443,T,O,A","1497646776,10.0.0.4,168.62.32.14,45002,443,T,O,A","1497646776,10.0.0.4,52.240.48.24,45316,443,T,O,A","1497646779,10.0.0.4,168.62.32.14,45008,443,T,O,A","1497646779,10.0.0.4,52.240.48.24,45322,443,T,O,A"]}]}
,{"rule":"DefaultRule_DenyAllInBound","flows":[]},{"rule":"UserRule_ssh-rule","flows":[]},{"rule":"UserRule_web-rule","flows":[{"mac":"000D3A18077E","flowTuples":["1497646738,13.82.225.93,10.0.0.4,1180,80,T,I,A","1497646750,13.82.225.93,10.0.0.4,
1184,80,T,I,A","1497646768,13.82.225.93,10.0.0.4,1181,80,T,I,A","1497646780,13.82.225.93,10.0.0.4,1336,80,T,I,A"]}]}]}
        }
```

Dit scenario is een voorbeeld van vermeldingen in de NSG-stroomlogboeken lezen zonder dat het hele logboek parseren. U kunt nieuwe vermeldingen in het logboek kunt lezen, zoals ze zijn geschreven met behulp van de blok-ID of door bij te houden van de lengte van blokken die zijn opgeslagen in de blok-blob. Hiermee kunt u alleen de nieuwe gegevens worden gelezen.

## <a name="next-steps"></a>Volgende stappen

Ga naar [Elastic Stack gebruiken](network-watcher-visualize-nsg-flow-logs-open-source-tools.md), [gebruik Grafana](network-watcher-nsg-grafana.md), en [gebruik Graylog](network-watcher-analyze-nsg-flow-logs-graylog.md) voor meer informatie over manieren om NSG-stroomlogboeken weer te geven. Een functie van de Azure Open Source-benadering voor de blobs rechtstreeks verbruikt en verzenden naar verschillende log analytics-klanten kan hier vinden: [Azure Network Watcher NSG Stroomlogboeken Connector](https://github.com/Microsoft/AzureNetworkWatcherNSGFlowLogsConnector).

Ga voor meer informatie over de storage-blobs naar: [Azure Functions Blob storage-bindingen](../azure-functions/functions-bindings-storage-blob.md)
