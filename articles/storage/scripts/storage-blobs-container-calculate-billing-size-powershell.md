---
title: Azure PowerShell-voorbeeldscript - de totale grootte van een blobcontainer berekenen voor facturering | Microsoft Docs
description: De totale grootte van een container in Azure Blob-opslag berekenen voor factureringsdoeleinden.
services: storage
documentationcenter: na
author: WenJason
manager: digimobile
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: sample
origin.date: 11/07/2017
ms.date: 04/22/2019
ms.author: v-jay
ms.openlocfilehash: 02b4cfcc6d88430701f653665269532a4eb7092f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60460636"
---
# <a name="calculate-the-total-billing-size-of-a-blob-container"></a>De totale grootte van een blobcontainer berekenen voor facturering

Met dit script wordt de grootte van een container in Azure Blob-opslag berekend voor het schatten van factureringskosten. Met het script wordt de totale grootte van de blobs in de container opgeteld.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!NOTE]
> Met het PowerShell-script wordt de grootte van een container berekend voor factureringsdoeleinden. Als u de containergrootte voor andere doeleinden wilt berekenen, raadpleegt u [Calculate the total size of a Blob storage container](../scripts/storage-blobs-container-calculate-size-powershell.md) (De totale grootte van een Blob Storage-container berekenen) voor een eenvoudiger script dat een schatting biedt.

## <a name="determine-the-size-of-the-blob-container"></a>De grootte van de blobcontainer bepalen

De totale grootte van de blobcontainer omvat de grootte van de container zelf en de grootte van alle blobs onder de container.

In de volgende secties wordt beschreven hoe de opslagcapaciteit voor blobcontainers en blobs wordt berekend. In de volgende sectie betekent Len(X) het aantal tekens in de tekenreeks.

### <a name="blob-containers"></a>Blobcontainers

De volgende berekening laat zien hoe u de hoeveelheid opslag kunt schatten die per blobcontainer wordt verbruikt:

```
48 bytes + Len(ContainerName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
For-Each Signed Identifier[512 bytes]
```

Hieronder ziet u de uitsplitsing:
* 48 bytes overhead voor elke container, inclusief Tijdstip laatst gewijzigd, Machtigingen, Openbare instellingen en enkele systeemmetagegevens.

* De containernaam wordt opgeslagen in Unicode. Vermenigvuldig dus het aantal tekens met twee.

* Voor elke blok met blobcontainermetagegevens dat is opgeslagen, wordt de lengte van de naam (ASCII) opgeslagen, plus de lengte van de tekenreekswaarde.

* De 512 bytes per Ondertekende id is inclusief ondertekende id-naam, begintijd, verlooptijd en machtigingen.

### <a name="blobs"></a>Blobs

De volgende berekeningen laten zien hoe u de hoeveelheid opslag kunt schatten die per blob wordt verbruikt.

* Blok-blob (basis-blob of momentopname):

   ```
   124 bytes + Len(BlobName) * 2 bytes +
   For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
   8 bytes + number of committed and uncommitted blocks * Block ID Size in bytes +
   SizeInBytes(data in unique committed data blocks stored) +
   SizeInBytes(data in uncommitted data blocks)
   ```

* Pagina-blob (basis-blob of momentopname):

   ```
   124 bytes + Len(BlobName) * 2 bytes +
   For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
   number of nonconsecutive page ranges with data * 12 bytes +
   SizeInBytes(data in unique pages stored)
   ```

Hieronder ziet u de uitsplitsing:

* 124 bytes overhead voor blob, inclusief:
    - Tijdstip laatst gewijzigd
    - Grootte
    - Cache-Control
    - Content-Type
    - Content-Language
    - Content-Encoding
    - Content-MD5
    - Machtigingen
    - Informatie over momentopname
    - Lease
    - Enkele systeemmetagegevens

* De blobnaam wordt opgeslagen in Unicode. Vermenigvuldig dus het aantal tekens met twee.

* Voor elke blok met metagegevens dat is opgeslagen, wordt de lengte van de naam (opgeslagen als ASCII) toegevoegd, plus de lengte van de tekenreekswaarde.

* Voor de blok-blobs:
  * 8 bytes voor de blok-lijst.
  * Aantal blokken maal de grootte van de blok-id in bytes.
  * De grootte van de gegevens in alle toegezegde en niet-toegezegde blokken.

    >[!NOTE]
    >Wanneer momentopnamen worden gebruikt, is deze grootte alleen inclusief de unieke gegevens voor deze basis-blob of momentopname. Als de niet-toegezegde blokken na een week niet zijn gebruikt, worden ze definitief verwijderd. Hierna tellen ze niet meer mee in de facturering.

* Voor pagina-blobs:
  * Het aantal niet-aaneengesloten pagina’s met gegevens maal 12 bytes. Dit is het aantal unieke paginabereiken dat u ziet wanneer u de API **GetPageRanges** aanroept.

  * De grootte van de gegevens in bytes van alle opgeslagen pagina’s.

    >[!NOTE]
    >Wanneer momentopnamen worden gebruikt, is deze grootte alleen inclusief de unieke pagina’s voor de basis-blob of momentopname die wordt geteld.

## <a name="sample-script"></a>Voorbeeldscript

```powershell

# this script will show how to get the total size of the blobs in a container
# before running this, you need to create a storage account, create a container,
#    and upload some blobs into the container
# note: this retrieves all of the blobs in the container in one command.
#       connect Azure with Login-AzAccount -EnvironmentName AzureChinaCloud before you run the script.
#       requests sent as part of this tool will incur transactional costs. 
# command line usage: script.ps1 -ResourceGroup {YourResourceGroupName} -StorageAccountName {YourAccountName} -ContainerName {YourContainerName}
#


param(
    [Parameter(Mandatory=$true)]
    [string]$ResourceGroup,

    [Parameter(Mandatory=$true)]
    [string]$StorageAccountName,

    [Parameter(Mandatory=$true)]
    [string]$ContainerName
)

#Set-StrictMode will cause Get-AzureStorageBlob returns result in different data types when there is only one blob
#Set-StrictMode -Version 2

$VerbosePreference = "Continue"

if((Get-Module -ListAvailable Az.Storage) -eq $null)
{
    throw "Azure Powershell not found! Please install from https://docs.microsoft.com/en-us/powershell/azure/install-Az-ps"
}

# function Retry-OnRequest
function Retry-OnRequest
{
    param(
        [Parameter(Mandatory=$true)]
        $Action)
    
    # It could encounter various of temporary errors, like network errors, or storage server busy errors.
    # Should retry the request on transient errors

    # Retry on storage server timeout errors
    $clientTimeOut = New-TimeSpan -Minutes 15
    $retryPolicy = New-Object -TypeName Microsoft.WindowsAzure.Storage.RetryPolicies.ExponentialRetry -ArgumentList @($clientTimeOut, 10)        
    $requestOption = @{}
    $requestOption.RetryPolicy = $retryPolicy

    # Retry on temporary network errors
    $shouldRetryOnException = $false
    $maxRetryCountOnException = 3

    do
    {
        try
        {
            return $Action.Invoke($requestOption)
        }
        catch
        {
            if ($_.Exception.InnerException -ne $null -And $_.Exception.InnerException.GetType() -Eq [System.TimeoutException] -And $maxRetryCountOnException -gt 0)
            {
                $shouldRetryOnException = $true
                $maxRetryCountOnException--
            }
            else
            {
                $shouldRetryOnException = $false
                throw
            }
        }
    }
    while ($shouldRetryOnException)

}

# function Get-BlobBytes

function Get-BlobBytes
{
    param(
        [Parameter(Mandatory=$true)]
        $Blob,
        [Parameter(Mandatory=$false)]
        [bool]$IsPremiumAccount = $false)

    # Base + blobname
    $blobSizeInBytes = 124 + $Blob.Name.Length * 2

    # Get size of metadata
    $metadataEnumerator=$Blob.ICloudBlob.Metadata.GetEnumerator()
    while($metadataEnumerator.MoveNext())
    {
        $blobSizeInBytes += 3 + $metadataEnumerator.Current.Key.Length + $metadataEnumerator.Current.Value.Length
    }

    if (!$IsPremiumAccount)
    {
        if($Blob.BlobType -eq [Microsoft.WindowsAzure.Storage.Blob.BlobType]::BlockBlob)
        {
            $blobSizeInBytes += 8
            # Default is Microsoft.WindowsAzure.Storage.Blob.BlockListingFilter.Committed. Need All
            $action = { param($requestOption) return $Blob.ICloudBlob.DownloadBlockList([Microsoft.WindowsAzure.Storage.Blob.BlockListingFilter]::All, $null, $requestOption) }                

            $blocks=Retry-OnRequest $action      

            if ($null -eq $blocks)
            {
                $blobSizeInBytes += $Blob.ICloudBlob.Properties.Length
            }
            else
            {
                $blocks | ForEach-Object { $blobSizeInBytes += $_.Length + $_.Name.Length }
            }  
        }
        elseif($Blob.BlobType -eq [Microsoft.WindowsAzure.Storage.Blob.BlobType]::PageBlob)
        {
            # It could cause server time out issue when trying to get page ranges of highly fragmented page blob 
            # Get page ranges in segment can mitigate chance of meeting such kind of server time out issue
            # See https://blogs.msdn.microsoft.com/windowsazurestorage/2012/03/26/getting-the-page-ranges-of-a-large-page-blob-in-segments/ for details.
            $pageRangesSegSize = 148 * 1024 * 1024L
            $totalSize = $Blob.ICloudBlob.Properties.Length
            $pageRangeSegOffset = 0
        
            $pageRangesTemp = New-Object System.Collections.ArrayList
        
            while ($pageRangeSegOffset -lt $totalSize)
            {
                $action = {param($requestOption) return $Blob.ICloudBlob.GetPageRanges($pageRangeSegOffset, $pageRangesSegSize, $null, $requestOption) }

                Retry-OnRequest $action | ForEach-Object { $pageRangesTemp.Add($_) }  | Out-Null
                $pageRangeSegOffset += $pageRangesSegSize
            }

            $pageRanges = New-Object System.Collections.ArrayList

            foreach ($pageRange in $pageRangesTemp)
            {
                if($lastRange -eq $Null)
                {
                    $lastRange = New-Object PageRange
                    $lastRange.StartOffset = $pageRange.StartOffset
                    $lastRange.EndOffset =  $pageRange.EndOffset
                }
                else
                {
                    if (($lastRange.EndOffset + 1) -eq $pageRange.StartOffset)
                    {
                        $lastRange.EndOffset = $pageRange.EndOffset
                    }
                    else
                    {
                        $pageRanges.Add($lastRange)  | Out-Null
                        $lastRange = New-Object PageRange
                        $lastRange.StartOffset = $pageRange.StartOffset
                        $lastRange.EndOffset =  $pageRange.EndOffset
                    }
                }
            }

            $pageRanges.Add($lastRange) | Out-Null
            $pageRanges |  ForEach-Object { 
                    $blobSizeInBytes += 12 + $_.EndOffset - $_.StartOffset 
                }
        }
        else
        {
            $blobSizeInBytes += $Blob.ICloudBlob.Properties.Length
        }
        return $blobSizeInBytes
    }
    else
    {
        $blobSizeInBytes += $Blob.ICloudBlob.Properties.Length
    }
    return $blobSizeInBytes
}

# function Get-ContainerBytes

function Get-ContainerBytes
{
    param(
        [Parameter(Mandatory=$true)]
        [Microsoft.WindowsAzure.Storage.Blob.CloudBlobContainer]$Container,
        [Parameter(Mandatory=$false)]
        [bool]$IsPremiumAccount = $false)

    # Base + name of container
    $containerSizeInBytes = 48 + $Container.Name.Length*2

    # Get size of metadata
    $metadataEnumerator = $Container.Metadata.GetEnumerator()
    while($metadataEnumerator.MoveNext())
    {
        $containerSizeInBytes += 3 + $metadataEnumerator.Current.Key.Length + $metadataEnumerator.Current.Value.Length
    }

    # Get size for SharedAccessPolicies
    $containerSizeInBytes += $Container.GetPermissions().SharedAccessPolicies.Count * 512

    # Calculate size of all blobs.
    $blobCount = 0
    $Token = $Null
    $MaxReturn = 5000

    do {
        $Blobs = Get-AzStorageBlob -Context $storageContext -Container $Container.Name -MaxCount $MaxReturn -ContinuationToken $Token
        if($Blobs -eq $Null) { break }

        #Set-StrictMode will cause Get-AzureStorageBlob returns result in different data types when there is only one blob
        if($Blobs.GetType().Name -eq "AzureStorageBlob")
        {
            $Token = $Null
        }
        else
        {
            $Token = $Blobs[$Blobs.Count - 1].ContinuationToken;
        }

        $Blobs | ForEach-Object {
                $blobSize = Get-BlobBytes $_ $IsPremiumAccount
                $containerSizeInBytes += $blobSize
                $blobCount++

                if(($blobCount % 1000) -eq 0)
                {
                    Write-Verbose("Counting {0} Sizing {1} " -f $blobCount, $containerSizeInBytes)
                }
            }
    }
    While ($Token -ne $Null)

    return @{ "containerSize" = $containerSizeInBytes; "blobCount" = $blobCount }
}

#Login-AzAccount -EnvironmentName AzureChinaCloud

$storageAccount = Get-AzStorageAccount -ResourceGroupName $ResourceGroup -Name $StorageAccountName -ErrorAction SilentlyContinue
if($storageAccount -eq $null)
{
    throw "The storage account specified does not exist in this subscription."
}

$storageContext = $storageAccount.Context

if (-not ([System.Management.Automation.PSTypeName]'PageRange').Type)
{
    $Source = "
        public class PageRange
        {
            public long StartOffset;
            public long EndOffset;
        }"
    Add-Type -TypeDefinition $Source
}

$containers = New-Object System.Collections.ArrayList
if($ContainerName.Length -ne 0)
{
    $container = Get-AzStorageContainer -Context $storageContext -Name $ContainerName -ErrorAction SilentlyContinue |
        ForEach-Object { $containers.Add($_) } | Out-Null
}
else
{
    Get-AzStorageContainer -Context $storageContext | ForEach-Object { $containers.Add($_) } | Out-Null
}

$sizeInBytes = 0
$IsPremiumAccount = ($storageAccount.Sku.Tier -eq "Premium")

if($containers.Count -gt 0)
{
    $containers | ForEach-Object {
        Write-Output("Calculating container {0} ..." -f $_.CloudBlobContainer.Name)
        $result = Get-ContainerBytes $_.CloudBlobContainer $IsPremiumAccount
        $sizeInBytes += $result.containerSize

        Write-Output("Container '{0}' with {1} blobs has a sizeof {2:F2} MB." -f $_.CloudBlobContainer.Name,$result.blobCount,($result.containerSize/1MB))
    }
}
else
{
    Write-Warning "No containers found to process in storage account '$StorageAccountName'."
}
```

## <a name="next-steps"></a>Volgende stappen

- Zie [Calculate the total size of a Blob storage container](../scripts/storage-blobs-container-calculate-size-powershell.md) (De totale grootte van een Blob Storage-container berekenen) voor een eenvoudiger script dat een schatting biedt.

- Zie [Windows Azure Storage-facturering begrijpen](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/) voor meer informatie over Azure Storage-facturering.

- Raadpleeg de [documentatie van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) voor meer informatie over de Azure PowerShell-module.

- U vindt aanvullende Storage PowerShell-voorbeeldscripts in [PowerShell-voorbeelden voor Azure Storage](../blobs/storage-samples-blobs-powershell.md).
