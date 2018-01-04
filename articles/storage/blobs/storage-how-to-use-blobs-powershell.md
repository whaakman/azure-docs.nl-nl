---
title: Bewerkingen uitvoeren op Azure Blob storage (objectopslag) met PowerShell | Microsoft Docs
description: Zelfstudie - bewerkingen uitvoeren op Azure Blob storage (objectopslag) met PowerShell
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: robinsh
ms.openlocfilehash: 170c3091efc90f640792682377ed10e2eab0cab3
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="perform-azure-blob-storage-operations-with-azure-powershell"></a>Azure Blob storage bewerkingen uitvoeren met Azure PowerShell

Azure Blob Storage is een service voor het opslaan van grote hoeveelheden ongestructureerde objectgegevens, zoals tekst of binaire gegevens, die via HTTP of HTTPS overal vandaan kunnen worden opgevraagd. In dit artikel bevat informatie over basisbewerkingen in Azure Blob-opslag zoals uploaden, downloaden en verwijderen van blobs. Procedures voor:

> [!div class="checklist"]
> * Een container maken 
> * Uploaden van BLOB 's
> * De blobs in een container in een lijst weergeven 
> * Blobs downloaden
> * Blobs kopiëren
> * Blobs verwijderen
> * Weergeven en eigenschappen en metagegevens een blob instellen
> * Beveiliging op basis van handtekeningen voor gedeelde toegang beheren

Voor deze zelfstudie is moduleversie 3,6 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

[!INCLUDE [storage-quickstart-tutorial-intro-include-powershell](../../../includes/storage-quickstart-tutorial-intro-include-powershell.md)]

## <a name="create-a-container"></a>Een container maken

Blobs worden altijd naar een container geüpload. Containers zijn vergelijkbaar met mappen op uw computer, zodat u kunt het ordenen van groepen blobs in containers zoals ordenen van uw bestanden in mappen op uw computer. Een opslagaccount kan een onbeperkt aantal containers; hebben Dit wordt alleen beperkt door de hoeveelheid schijfruimte die in het opslagaccount in beslag wordt genomen (maximaal 500TB). 

Wanneer u een container maakt, kunt u het toegangsniveau die helpt bepalen wie toegang heeft tot de blobs in de container kunt instellen. Ze kunnen bijvoorbeeld persoonlijke (toegangsniveau = `Off`), wat betekent dat niemand ze toegankelijk zijn zonder een shared access signature of de toegangssleutels voor het opslagaccount. Als u geen het toegangsniveau opgeeft wanneer u de container maakt, wordt standaard persoonlijke.

U kunt de afbeeldingen in de container openbaar toegankelijk zijn. Bijvoorbeeld, als u wilt afbeeldingen moet worden weergegeven op uw website op te slaan, wilt u dat ze als public. In dit geval u de container access instellen op `blob`, en iedereen met een URL die verwijst naar een blob in de container toegang hebt tot de blob.

Stel de containernaam in voor het maken van de container en de container, het instellen van machtigingen naar de blob' maken. Namen van containers moeten beginnen met een letter of cijfer en mag alleen letters, cijfers en het koppelteken (-) bevatten. Zie voor meer regels over de naamgeving van blobs en containers [Naming en verwijzen naar Containers, Blobs en metagegevens](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

Maak een nieuwe container met [nieuw AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer). Het toegangsniveau ingesteld op openbaar. De containernaam van de in dit voorbeeld is *howtoblobs*.

```powershell
$containerName = "howtoblobs"
New-AzureStorageContainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-into-a-container"></a>BLOB's uploaden naar een container

Azure Blob Storage ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs.  VHD-bestanden die worden gebruikt voor IaaS-VM's zijn pagina-blobs. Toevoeg-blobs worden gebruikt voor logboekregistratie, bijvoorbeeld wanneer u wilt schrijven naar een bestand en vervolgens meer gegevens wilt blijven toevoegen. De meeste bestanden die zijn opgeslagen in Blob-opslag, zijn blok-blobs. 

Als u een bestand wilt uploaden naar een blok-blob, haalt u een containerverwijzing op en haalt u vervolgens een verwijzing op naar de blok-blob in deze container. Zodra u de blobverwijzing hebt, kunt u er gegevens naar uploaden met behulp van [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent). Deze bewerking wordt de blob gemaakt als deze niet bestaat, of deze wordt overschreven als deze al bestaat.

Hieronder ziet u hoe u een blob uploadt naar een container. Stel eerst de variabelen die naar de map op de lokale computer waar de bestanden zich bevinden en instellen van een variabele voor de naam van het bestand verwijzen te uploaden. Dit is handig als u wilt meerdere keren dezelfde bewerking uitvoeren. Een aantal bestanden uploaden zodat u meerdere vermeldingen zien kunt wanneer de blobs in de container.

De volgende voorbeelden Image001.jpg en Image002.png laden vanaf D:\\_TestImages op de lokale schijf aan de container die u zojuist hebt gemaakt.

```powershell
$localFileDirectory = "D:\_TestImages\"

$blobName = "Image001.jpg"
$localFile = $localFileDirectory + $blobName
Set-AzureStorageBlobContent -File $localFile `
  -Container $containerName `
  -Blob $blobName `
  -Context $ctx 
```

Upload een ander bestand. 

```powershell
$blobName = "Image002.png"
$localFile = $localFileDirectory + $blobName 
Set-AzureStorageBlobContent -File $localFile `
  -Container $containerName `
  -Blob $blobName `
  -Context $ctx
```

Upload zoveel bestanden als u nodig hebt, voordat u doorgaat.

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Een lijst met blobs in de container met behulp [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) en het selecteren van de blob-naam moet worden weergegeven.

```powershell
Get-AzureStorageBlob -Container $ContainerName -Context $ctx | select Name 
```

## <a name="download-blobs"></a>Blobs downloaden

Download de blobs naar de lokale vaste schijf. Stel eerst een variabele die verwijst naar de lokale map waarnaar u wilt de blobs downloaden. Stel voor elke blob worden gedownload, de naam en de aanroep [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) voor het downloaden van de blob.

Dit voorbeeld kopieert de blobs naar D:\\_TestImages\Downloads op de lokale schijf. 

```powershell
# local directory to which to download the files
# example "D:\_TestImages\Downloads\"
$localTargetDirectory = "D:\_TestImages\Downloads\"

# download the first blob
$blobName = "Image001.jpg"
Get-AzureStorageBlobContent -Blob $blobName `
  -Container $containerName `
  -Destination $localTargetDirectory `
  -Context $ctx 

# download another blob
$blobName = "Image002.png"
Get-AzureStorageBlobContent -Blob $blobName `
  -Container $containerName `
  -Destination $localTargetDirectory `
  -Context $ctx 
```

## <a name="copy-blobs"></a>Blobs kopiëren

Er zijn enkele aandachtspunten bij het kopiëren van BLOB's. U kan worden alleen kopiëren van de blob naar een nieuwe naam in dezelfde locatie. U kan worden kopiëren van de blob naar een afzonderlijke opslagaccount. Kan dat er een grote blob (zoals een VHD-bestand) worden gekopieerd naar een ander opslagaccount. Deze elk uitgevoerd anders.

### <a name="simple-blob-copy"></a>Eenvoudige blob kopiëren

U kunt een kopie van een van de blobs in een container maken door deze te kopiëren naar een nieuwe blob. In dit voorbeeld kopieert deze naar de container met een andere naam, maar u kunt net zo gemakkelijk maken van een andere container en in plaats daarvan er kopiëren. Dit voorbeeld ziet u hoe u [Start AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) kopiëren van de blob. U moet de bron en bestemming blob-naam- en containernaam opgeven.

```powershell
$blobName = "Image002.png"
$newBlobName = "CopyOf_" + $blobName 

Start-AzureStorageBlobCopy -SrcBlob $blobName `
  -SrcContainer $containerName `
  -DestContainer $containerName `
  -DestBlob $newBlobName `
  -Context $ctx 

# get list of blobs to verify the new one has been created
Get-AzureStorageBlob -Container $containerName -Context $ctx | select Name 
```

### <a name="copy-a-blob-to-a-different-storage-account"></a>Een blob kopiëren naar een ander opslagaccount 

U kunt een blob kopiëren naar een afzonderlijke opslagaccount. Een voorbeeld om dit te doen is een VHD-bestand dat een back-up een van uw virtuele machines naar een ander opslagaccount om het back-up te kopiëren. 

Een tweede opslagaccount instellen, ophalen van de context en instellen van een container in dit opslagaccount kopieert. Dit gedeelte van het script is bijna identiek aan het script hierboven, met uitzondering van de tweede opslagaccount gebruik in plaats van de eerste.

```powershell
#create new storage account, get context 
$storageAccount2Name = "blobstutorialtestcopy"
$storageAccount2 = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccount2Name `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob

$ctx2 = $storageAccount2.Context

#create a container in the second storage account 
$containerName2 = "tutorialblobscopied"
New-AzureStorageContainer -Name $containerName2 `
  -Context $ctx2 `
  -Permission blob

# copy one of the blobs from the first storage account to the second one 
# specify the source and destination container, blob name, and context
Start-AzureStorageBlobCopy -SrcBlob $blobName `
  -SrcContainer $containerName `
  -DestContainer $containerName2 `
  -DestBlob $blobName `
  -SrcContext $ctx `
  -DestContext $ctx2 

# list the blobs in the container in the second storage account
# to verify that the newly copied blob is there
Get-AzureStorageBlob -Container $containerName2 -Context $ctx2 | select Name 
```

### <a name="copying-very-large-blobs-asynchronously"></a>Zeer grote blobs kopiëren asynchroon

Als u een zeer grote blob die meerdere GB kopieert, kunt u profiteren van de asynchrone kopie door starten, en vervolgens terugkomen en het controleren van de status totdat deze voltooid. Op deze manier u zijn niet gekoppeld terwijl het de kopieerbewerking uitvoert.

Als u binnen een opslagaccount kopieert, wordt de kopie is zeer snel. Als u tussen twee storage-accounts in dezelfde regio kopiëren wilt, wordt de kopie vrij snel is. Als de bron- en doelserver zich in afzonderlijke regio's, kan het enkele uren in beslag de kopie, afhankelijk van de afstand en de grootte van het bestand krijgen. 

Dit script maakt gebruik van de dezelfde variabelen die zijn gedefinieerd in de laatste kopie-voorbeeld. Het verschil is dat deze de status van het exemplaar worden vastgelegd en wordt opgevraagd het herhaaldelijk elke 10 seconden totdat deze is voltooid. U moet een zeer grote blob uploaden naar uw storage-account om dit nemen meer dan één iteratie voltooien te zien.

Gebruik [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/get-azurestorageblobcopystate) query uitvoeren op de status van de kopie. 

```powershell
# start the blob copy, and assign the result to $blobResult
$blobResult = Start-AzureStorageBlobCopy -SrcBlob $blobName `
  -SrcContainer $containerName `
  -DestContainer $containerName2 `
  -DestBlob $blobName `
  -Context $ctx `
  -DestContext $ctx2

# get the status of the blob copy
$status = $blobResult | Get-AzureStorageBlobCopyState 
# show the value of the status
$status 

# loop until it finishes copying, pausing for 10 seconds after each iteration
# it is finished when the status is no longer 'Pending'
while ($status.Status -eq "Pending") {
    $status = $blobResult | Get-AzureStorageBlobCopyState 
    $status
    Start-Sleep 10
}

# get the list of blobs to see the new file in the second storage account 
Get-AzureStorageBlob -Container $containerName2 -Context $ctx2 | select Name 
```

## <a name="delete-blobs"></a>Blobs verwijderen

U blobs verwijderen van een opslagaccount, [verwijderen AzureStorageBlob](/powershell/module/azure.storage/Remove-AzureStorageBlob). 

```powershell
$blobName = "Image001.jpg"
Remove-AzureStorageBlob -Blob $blobName -Container $containerName -Context $ctx

# get list of blobs to see the one you deleted is gone
Get-AzureStorageBlob -Container $containerName -Context $ctx | select Name 
```

## <a name="read-and-write-a-blobs-properties-and-metadata"></a>De eigenschappen en metagegevens een blob lezen en schrijven

Voor toegang tot de set eigenschappen en methoden voor een geretourneerde **IListBlobItem**, moet u dit casten (of converteert u deze) naar een **CloudBlockBlob**, **CloudPageBlob**, of **CloudBlobDirectory** object. Als het type onbekend is, kunt u typecontrole gebruiken om te bepalen waarnaar het moet worden gecast. De volgende code laat zien hoe ophalen en de uitvoer van de eigenschappen van een van de blobs geüpload eerder met [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) en die het resultaat dat een CloudBlockBlob-object.

```powershell
# blob properties
# get a reference to the blob you uploaded -- this is an **IListBlobItem** -- then
# convert it to a CloudBlockBlob, giving you access to the properties 
# and methods of the blob 
$blobName = "Image001.jpg"
$blob = Get-AzureStorageBlob -Context $ctx `
   -Container $containerName `
   -Blob $blobName 

#convert $blob to a CloudBlockBlob object
$cloudBlockBlob = [Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob] $blob.ICloudBlob

# you can view the properties by typing in $cloudBlockBlob 
#   and hitting the period key; this brings up IntelliSense,
#   which shows you all of the available properties
Write-Host "blob type = " $cloudBlockBlob.BlobType
Write-Host "blob name = " $cloudBlockBlob.Name
Write-Host "blob uri = " $cloudBLockBlob.Uri
```

Vullen van de eigenschappen van het door het aanroepen van FetchAttributes en weergeven van deze eigenschappen. Sommige eigenschappen beschrijfbaar zijn en kunt u hun waarden wijzigen. Dit voorbeeld toont het wijzigen van het type inhoud, ook wel bekend als het MIME-type.

```powershell
# populate the system properties
$cloudBlockBlob.FetchAttributes()

# view some of the system properties
# contentType is commonly referred to as MIME type
Write-Host "content type = " $cloudBlockBlob.Properties.ContentType
Write-Host "size = " $cloudBlockBlob.Properties.Length 

# change the content type to image/png
$contentType = "image/jpg"
$cloudBlockBlob.Properties.ContentType = $contentType 
$cloudBlockBlob.SetProperties() 

# get the system properties again to show that the content type has changed
$cloudBlockBlob.FetchAttributes()
Write-Host "content type = " $cloudBlockBlob.Properties.ContentType
```

Elke blob heeft een eigen metagegevens die u kunt instellen. U kunt bijvoorbeeld de naam van de gebruiker die de blob of de datum/tijd die het eerst is geüpload geüpload opslaan. De metagegevens bestaat uit de sleutel/waarde-paren. Dit kan worden gewijzigd met behulp van PowerShell als volgt.

```powershell
# "filename" is the key, "original file name" is the value
$cloudBlockBlob.Metadata["filename"] = "original file name"

# "owner" is the key, "RobinS" is the value
$cloudBlockBlob.Metadata["owner"] = "RobinS"

# save the metadata and then display it
$cloudBlockBlob.SetMetadata()
$cloudBlockBlob.Metadata

# clear the metadata, save it, and show it
$cloudBlockBlob.Metadata.Clear()
$cloudBlockBlob.SetMetadata()
$cloudBlockBlob.Metadata
```

## <a name="managing-security-for-blobs"></a>Beveiliging beheren voor blobs 

Standaard beveiligt Azure Storage uw gegevens door het beperken van toegang tot de accounteigenaar, die in bezit is van de toegangssleutels voor opslag-account. Als u blobgegevens in uw opslagaccount wilt delen, moet u voorkomen dat u de beveiliging van de toegangssleutels van uw account in gevaar brengt. Om dit te doen, kunt u een Shared Access Signature-URL, dit is een URL naar de asset die queryparameters en een beveiligingstoken waarmee een specifiek niveau van de machtiging voor een specifieke hoeveelheid tijd bevat. U wilt bijvoorbeeld alleen toegang geven tot een blob met persoonlijke gedurende vijf minuten, zodat iemand deze kunt bekijken. 

### <a name="set-the-access-level-of-the-container-and-its-blobs-to-private"></a>Stel het toegangsniveau van de container en de blobs in een particuliere

Stel eerst het toegangsniveau van de container `Off`, waarmee wordt aangegeven dat u geen toegang zonder een shared access signature of de accountsleutel hebt. Gebruik de [Set AzureStorageContainerAcl](/powershell/module/azure.storage/Set-AzureStorageContainerAcl).

```powershell
Set-AzureStorageContainerAcl -Name $containerName -Context $ctx -Permission Off 
```

### <a name="test-private-access"></a>Persoonlijke toegang testen

De URL naar een van de blobs zonder een shared access signature maken om te bevestigen dat u geen toegang tot de blobs in de container hebt, en probeer het weergeven van de blob. Met behulp van het HTTPS-protocol, de URL niet in de volgende indeling:

    `https://storageaccountname.blob.core.windows.net/containername/blobname`

Dit laat zien hoe de blob-URL te maken.

```powershell
$blobUrl = "https://" `
  $storageAccountName ".blob.core.windows.net/" + `
  $containerName +  "/" $blobName

Write-Host "Blob URL = " $blobUrl 
```

Kopieer de URL van de blob en plak deze in een eigen browservenster--een Autorisatiefout wordt het weergegeven omdat de blob persoonlijk is en een shared access signature niet is opgenomen. 

### <a name="create-the-sas-uri"></a>Maken van de SAS-URI

Maken van een SAS-URI: dit is de koppeling naar de blob met inbegrip van de queryparameters en beveiligingstoken die gezamenlijk de SAS. Plak deze URI in een persoonlijke browservenster--de afbeelding wordt het weergegeven. 

Eerst maken de begindatum/tijd en verlooptijd datum/tijd om toegang te krijgen. Dit maakt gebruik van een venster 2 minuten. 

```powershell
# set the start time to the current date/time 
# set the end time to 2 minutes in the future
$StartTime = Get-Date
$EndTime = $StartTime.AddMinutes(2.0)
```

Maken van de SAS-URI met [nieuw AzureStorageBlobSASToken](/powershell/module/azure.storage/new-azurestorageblobsastoken). U moet de containernaam van de, blob-naam, de context van opslag, de machtigingen om te worden verleend (in dit geval is, lezen, schrijven en verwijderen), en de begin- en tijd om toegang te krijgen. 

```powershell
$SASURI = New-AzureStorageBlobSASToken -Container $containerName `
    -Blob $blobName `
    -Context $ctx `
    -Permission "rwd" `
    -StartTime $StartTime `
    -ExpiryTime $EndTime `
    -FullUri

Write-Host "URL with SAS = " $SASURI
```

Kopiëren van de SAS-URI en plaatsen in een persoonlijke browservenster; de afbeelding wordt het weergegeven.

Wachten op de URL om te verlopen (2 minuten in dit voorbeeld) en plak de URL in een andere InPrivate-browservenster lang genoeg. Deze tijd krijgt u een foutmelding autorisatie en wordt de afbeelding niet weergegeven omdat de SAS-URI is verlopen.

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder alle van de activa die u hebt gemaakt. Om dit te doen, kunt u de resourcegroep, ook alle resources binnen de groep te verwijderen. In dit geval verwijdert alle de storage-accounts en de resourcegroep zelf.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over basic Blob storage management zoals het:

> [!div class="checklist"]
> * Een container maken 
> * Uploaden van BLOB 's
> * De blobs in een container in een lijst weergeven 
> * Blobs downloaden
> * Blobs kopiëren
> * Blobs verwijderen
> * De metagegevens en de eigenschappen van een blob lezen en schrijven
> * Beveiliging op basis van handtekeningen voor gedeelde toegang beheren

### <a name="microsoft-azure-powershell-storage-cmdlets"></a>Opslag voor Microsoft Azure PowerShell-cmdlets
* [PowerShell Storage-cmdlets](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.
