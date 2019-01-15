---
title: Hulpprogramma's voor Azure Stack-opslag | Microsoft Docs
description: Meer informatie over Azure Stack-opslaggegevens hulpprogramma's voor overdracht
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/03/2018
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: b907098f9d6471a7df5ca43f76cd9109ee4dea8e
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54263947"
---
# <a name="use-data-transfer-tools-for-azure-stack-storage"></a>Gebruik van hulpmiddelen voor het overbrengen van gegevens voor Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Microsoft Azure Stack biedt een set met de storage-services voor schijven, blobs, tabellen, wachtrijen en account-beheerfuncties. U kunt een set hulpprogramma's van Azure storage gebruiken als u wilt beheren of gegevens te verplaatsen naar of van Azure Stack-opslag. Dit artikel bevat een overzicht van de beschikbare hulpprogramma's.

Uw vereisten te bepalen welke van de volgende hulpprogramma's het beste voor u werkt:

* [AzCopy](#azcopy)

    Een opslag-specifieke, opdrachtregel-hulpprogramma die u downloaden kunt om gegevens te kopiëren van het ene object naar een ander object binnen uw opslagaccount of tussen opslagaccounts.

* [Azure PowerShell](#azure-powershell)

    Een taak, opdrachtregel-shell en scripttaal die is ontworpen voor systeembeheer.

* [Azure-CLI](#azure-cli)

    Een open-source, platformoverschrijdende hulpprogramma waarmee u een reeks opdrachten voor het werken met de Azure en Azure Stack-platforms.

* [Microsoft Opslagverkenner](#microsoft-azure-storage-explorer)

    Een app voor zelfstandige eenvoudig te gebruiken met een gebruikersinterface.

* [Blobfuse ](#blobfuse)

    Een virtuele stuurprogramma van het bestandssysteem voor Azure Blob Storage, waarmee u uw bestaande blok-blobgegevens in uw Storage-account via het bestandssysteem van Linux. 

Als gevolg van de storage-services verschillen tussen Azure en Azure Stack, is het mogelijk dat er een enkele specifieke vereisten voor elk hulpprogramma dat wordt beschreven in de volgende secties. Zie voor een vergelijking van Azure Stack storage en Azure storage, [Azure Stack-opslag: Verschillen en overwegingen](azure-stack-acs-differences.md).

## <a name="azcopy"></a>AzCopy

AzCopy is een opdrachtregelprogramma dat is ontworpen om gegevens te kopiëren naar en van Microsoft Azure-blob en table storage met behulp van eenvoudige opdrachten met optimale prestaties. U kunt gegevens kopiëren van het ene object naar een andere binnen uw opslagaccount of tussen opslagaccounts.

### <a name="download-and-install-azcopy"></a>Download en installeer AzCopy

Er zijn twee versies van het hulpprogramma AzCopy: AzCopy in Windows en AzCopy in Linux.

 - **AzCopy in Windows**
    - Download de ondersteunde versie van AzCopy voor Azure Stack. U kunt installeren en u kunt AzCopy gebruiken in Azure Stack op dezelfde manier als Azure. Zie voor meer informatie, [AzCopy in Windows](../../storage/common/storage-use-azcopy.md).
        - Voor de 1811 update, of nieuwere versies, [AzCopy 7.3.0 downloaden](https://aka.ms/azcopyforazurestack20171109).
        - Voor eerdere versies (1802-1809 update), [AzCopy 7.1.0 downloaden](https://aka.ms/azcopyforazurestack20170417).

 - **AzCopy in Linux**

    - U kunt installeren en u kunt AzCopy gebruiken in Azure Stack op dezelfde manier als Azure. Zie voor meer informatie, [AzCopy in Linux](../../storage/common/storage-use-azcopy-linux.md).
    - Zie voor vorige versies (1802-1809 updates), de [installatie van de stappen voor AzCopy 7.1 en eerdere versies](../../storage/common/storage-use-azcopy-linux.md#installation-steps-for-azcopy-71-and-earlier-versions).

### <a name="azcopy-command-examples-for-data-transfer"></a>Voorbeelden van AzCopy-opdracht voor gegevensoverdracht

De volgende voorbeelden volgen typische scenario's voor het kopiëren van gegevens naar en van Azure Stack-blobs. Zie voor meer informatie, [AzCopy in Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux) en [AzCopy in Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux).

### <a name="download-all-blobs-to-a-local-disk"></a>Download alle blobs naar een lokale schijf

**Windows**

```shell
AzCopy.exe /source:https://myaccount.blob.local.azurestack.external/mycontainer /dest:C:\myfolder /sourcekey:<key> /S
```

**Linux**

```bash
azcopy \
    --source https://myaccount.blob.local.azurestack.external/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

### <a name="upload-single-file-to-virtual-directory"></a>Eén bestand uploaden naar de virtuele map

**Windows**

```shell
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.local.azurestack.external/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

**Linux**

```bash
azcopy \
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.blob.local.azurestack.external/mycontainer/vd/abc.txt \
    --dest-key <key>
```

### <a name="move-data-between-azure-and-azure-stack-storage"></a>Gegevens verplaatsen tussen Azure en Azure Stack-opslag

Asynchrone gegevensoverdracht tussen Azure storage en Azure Stack wordt niet ondersteund. U moet opgeven van de overdracht met de **/SyncCopy** of **--gesynchroniseerde kopie** optie.

**Windows**

```shell
Azcopy /Source:https://myaccount.blob.local.azurestack.external/mycontainer /Dest:https://myaccount2.blob.core.windows.net/mycontainer2 /SourceKey:AzSKey /DestKey:Azurekey /S /SyncCopy
```

**Linux**

```bash
azcopy \
    --source https://myaccount1.blob.local.azurestack.external/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --sync-copy
```

### <a name="azcopy-known-issues"></a>Bekende problemen met Azcopy

 - Elke bewerking AzCopy in een bestandsarchief is niet beschikbaar omdat de opslag van bestanden is nog niet beschikbaar in Azure Stack.
 - Asynchrone gegevensoverdracht tussen Azure storage en Azure Stack wordt niet ondersteund. U kunt opgeven dat de overdracht met de **/SyncCopy** optie om de gegevens te kopiëren.
 - De Linux-versie van Azcopy ondersteunt alleen 1802 update of latere versies. En het biedt geen ondersteuning voor Table-service.

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell is een module die cmdlets biedt voor het beheren van services op Azure en Azure Stack. Het is een taakgebaseerde, opdrachtregel-shell en scripttaal die is ontworpen voor systeembeheer.

### <a name="install-and-configure-powershell-for-azure-stack"></a>PowerShell installeren en configureren voor Azure Stack

Azure Stack compatibele Azure PowerShell-modules zijn vereist voor het werken met Azure Stack. Zie voor meer informatie, [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md) en [PowerShell-omgeving van de Azure Stack-gebruiker configureren](azure-stack-powershell-configure-user.md) voor meer informatie.

### <a name="powershell-sample-script-for-azure-stack"></a>PowerShell-voorbeeldscript voor Azure Stack 

In dit voorbeeld wordt ervan uitgegaan dat u hebt [geïnstalleerd PowerShell voor Azure Stack](azure-stack-powershell-install.md). Dit script kunt u de configuratie te voltooien en vraagt u uw Azure Stack-tenant referenties voor uw account toevoegen aan de lokale PowerShell-omgeving. Vervolgens wordt het script de standaard Azure-abonnement instellen, een nieuw opslagaccount maken in Azure, een nieuwe container maken in deze nieuwe storage-account en een bestaand installatiekopiebestand (blob) uploaden naar deze container. Nadat het script geeft een lijst van alle blobs in die container, wordt een nieuwe bestemming-map maken op uw lokale computer en downloaden van het afbeeldingsbestand.

1. Installeer [Azure Stack-compatibel is met Azure PowerShell-modules](azure-stack-powershell-install.md).
2. In het [Azure Stack development Kit, worden de blobEndpoint](azure-stack-powershell-download.md) .
3. Open **Windows PowerShell ISE** en **als Administrator uitvoeren**, klikt u op **bestand** > **nieuw** om een nieuwe scriptbestand te maken.
4. Het onderstaande script kopiëren en plakken naar het nieuwe scriptbestand.
5. De scriptvariabelen op basis van uw configuratie-instellingen bijwerken.
   > [!NOTE]
   > Met dit script moet worden uitgevoerd op de hoofdmap voor **AzureStack_Tools**.

```PowerShell  
# begin

$ARMEvnName = "AzureStackUser" # set AzureStackUser as your Azure Stack environment name
$ARMEndPoint = "https://management.local.azurestack.external" 
$GraphAudience = "https://graph.windows.net/" 
$AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com" 

$SubscriptionName = "basic" # Update with the name of your subscription.
$ResourceGroupName = "myTestRG" # Give a name to your new resource group.
$StorageAccountName = "azsblobcontainer" # Give a name to your new storage account. It must be lowercase.
$Location = "Local" # Choose "Local" as an example.
$ContainerName = "photo" # Give a name to your new container.
$ImageToUpload = "C:\temp\Hello.jpg" # Prepare an image file and a source directory in your local computer.
$DestinationFolder = "C:\temp\download" # A destination directory in your local computer.

# Import the Connect PowerShell module"
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
Import-Module .\Connect\AzureStack.Connect.psm1

# Configure the PowerShell environment
# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRmEnvironment -Name $ARMEvnName -ARMEndpoint $ARMEndPoint 

# Set the GraphEndpointResourceId value
Set-AzureRmEnvironment -Name $ARMEvnName -GraphEndpoint $GraphAudience

# Login
$TenantID = Get-AzsDirectoryTenantId -AADTenantName $AADTenantName -EnvironmentName $ARMEvnName
Add-AzureRmAccount -EnvironmentName $ARMEvnName -TenantId $TenantID 

# Set a default Azure subscription.
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Create a new Resource Group 
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Create a new storage account.
New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS

# Set a default storage account.
Set-AzureRmCurrentStorageAccount -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName 

# Create a new container.
New-AzureStorageContainer -Name $ContainerName -Permission Off

# Upload a blob into a container.
Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

# List all blobs in a container.
Get-AzureStorageBlob -Container $ContainerName

# Download blobs from the container:
# Get a reference to a list of all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName

# Create the destination directory.
New-Item -Path $DestinationFolder -ItemType Directory -Force  

# Download blobs into the local destination directory.
$blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder

# end
```

### <a name="powershell-known-issues"></a>Bekende problemen met PowerShell

Met de huidige compatibele Azure PowerShell-moduleversie voor Azure Stack is 1.2.11 voor de gebruikersbewerkingen. Dit wijkt af van de meest recente versie van Azure PowerShell. Dit verschil heeft gevolgen voor opslagbewerking services:

De indeling van de geretourneerde waarde van `Get-AzureRmStorageAccountKey` in versie 1.2.11 heeft twee eigenschappen: `Key1` en `Key2`, terwijl de huidige versie van de Azure retourneert een matrix met alle accountsleutels.

```powershell
# This command gets a specific key for a storage account, 
# and works for Azure PowerShell version 1.4, and later versions.
(Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
-AccountName "MyStorageAccount").Value[0]

# This command gets a specific key for a storage account, 
# and works for Azure PowerShell version 1.3.2, and previous versions.
(Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
-AccountName "MyStorageAccount").Key1
```

Zie voor meer informatie, [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/Get-AzureRmStorageAccountKey).

## <a name="azure-cli"></a>Azure-CLI

De Azure-CLI is de opdrachtregelervaring van Azure voor het beheren van Azure-resources. U kunt installeren op macOS, Linux en Windows en uitvoeren vanaf de opdrachtregel.

Azure CLI is geoptimaliseerd voor het beheren en beheer van Azure-resources vanaf de opdrachtregel en voor het bouwen van automatiseringsscripts die op basis van de Azure Resource Manager werken. Het biedt veel dezelfde functies gevonden in de Azure Stack-portal, met inbegrip van toegang tot uitgebreide gegevens.

Azure Stack gebruikmaken van Azure CLI versie 2.0 of hoger. Zie voor meer informatie over het installeren en configureren van Azure CLI met Azure Stack [installeren en configureren van Azure Stack CLI](azure-stack-version-profiles-azurecli2.md). Zie voor meer informatie over het gebruik van de Azure CLI om uit te voeren van verschillende taken die werken met resources in uw Azure Stack-opslagaccount [de Azure CLI gebruiken met Azure storage](../../storage/storage-azure-cli.md)

### <a name="azure-cli-sample-script-for-azure-stack"></a>Azure CLI-voorbeeldscript voor Azure Stack

Nadat u de CLI-installatie en configuratie is voltooid, kunt u de volgende stappen uit om te werken met een kleine shell-voorbeeldscript om te communiceren met Azure Stack-opslagresources. Het script is voltooid de volgende acties:

* Hiermee maakt een nieuwe container in uw storage-account.
* Een bestaand bestand (als een blob) naar de container geüpload.
* Een lijst met alle blobs in de container.
* Het bestand wordt gedownload naar een bestemming op uw lokale computer die u opgeeft.

Voordat u dit script uitvoert, zorg ervoor dat u kunt met succes verbinding met maken en meld u aan met de doel-Azure Stack.

1. Open uw favoriete teksteditor en kopieer en plak het vorige script in de editor.
2. Update van het script variabelen om uw configuratie-instellingen weer te geven.
3. Nadat u de benodigde variabelen hebt bijgewerkt, sla het script op en sluit de editor af. De volgende stappen wordt ervan uitgegaan dat u hebt de naam van uw script **my_storage_sample.sh**.
4. Markeer het script als uitvoerbare bestanden, indien nodig: `chmod +x my_storage_sample.sh`
5. Voer het script. Bijvoorbeeld, in Bash: `./my_storage_sample.sh`

```azurecli
#!/bin/bash
# A simple Azure Stack storage example script

export AZURESTACK_RESOURCE_GROUP=<resource_group_name>
export AZURESTACK_RG_LOCATION="local"
export AZURESTACK_STORAGE_ACCOUNT_NAME=<storage_account_name>
export AZURESTACK_STORAGE_CONTAINER_NAME=<container_name>
export AZURESTACK_STORAGE_BLOB_NAME=<blob_name>
export FILE_TO_UPLOAD=<file_to_upload>
export DESTINATION_FILE=<destination_file>

echo "Creating the resource group..."
az group create --name $AZURESTACK_RESOURCE_GROUP --location $AZURESTACK_RG_LOCATION

echo "Creating the storage account..."
az storage account create --name $AZURESTACK_STORAGE_ACCOUNT_NAME --resource-group $AZURESTACK_RESOURCE_GROUP --account-type Standard_LRS

echo "Creating the blob container..."
az storage container create --name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME

echo "Uploading the file..."
az storage blob upload --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --file $FILE_TO_UPLOAD --name $AZURESTACK_STORAGE_BLOB_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME

echo "Listing the blobs..."
az storage blob list --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME --output table

echo "Downloading the file..."
az storage blob download --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME --name $AZURESTACK_STORAGE_BLOB_NAME --file $DESTINATION_FILE --output table

echo "Done"
```

## <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure storage explorer

Microsoft Azure storage explorer is een zelfstandige app van Microsoft. Hiermee kunt u eenvoudig kunt werken met Azure storage en Azure Stack-opslag gegevens via Windows, macOS en Linux-computers. Als u een eenvoudige manier voor het beheren van de gegevens van uw Azure Stack-opslag wilt, klikt u vervolgens kunt u overwegen Microsoft Azure storage explorer.

* Zie voor meer informatie over het configureren van Azure storage explorer om te werken met Azure Stack, [Opslagverkenner verbinding maken met een Azure Stack-abonnement](azure-stack-storage-connect-se.md).
* Zie voor meer informatie over Microsoft Azure storage explorer [aan de slag met Opslagverkenner](../../vs-azure-tools-storage-manage-with-storage-explorer.md)

## <a name="blobfuse"></a>Blobfuse 

[Blobfuse](https://github.com/Azure/azure-storage-fuse) is een stuurprogramma van het virtuele bestandssysteem voor Azure Blob Storage, waarmee u uw bestaande blok-blobgegevens in uw Storage-account via het bestandssysteem van Linux. Azure Blob-opslag is een object storage-service en daarom geen een hiërarchische naamruimte. Blobfuse biedt deze naamruimte met behulp van de virtuele directory-schema met behulp van een zone voor forward slash `/` als scheidingsteken. Blobfuse werkt op Azure en Azure Stack. 

Zie voor meer informatie over het koppelen van Blob-opslag als een bestandssysteem met Blobfuse op Linux, [hoe u Blob-opslag koppelen als een bestandssysteem met Blobfuse](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux). 

Voor Azure Stack, **blobEndpoint** moet worden opgegeven naast accountName, accountKey/sasToken, containerName, tijdens het configureren van de referenties van het Opslagaccount in de stap van de voorbereiding voor koppelen. 

In het Azure Stack development Kit, worden de blobEndpoint `myaccount.blob.local.azurestack.external`. In geïntegreerde Azure Stack-systeem, contact op met uw cloudbeheerder als u niet zeker weet over uw eindpunt. 

Houd er rekening mee dat accountKey en sasToken mag alleen bestaan uit geconfigureerde één bewerking tegelijk. Wanneer de sleutel van het opslagaccount is opgegeven, is het configuratiebestand van de referenties in de volgende indeling: 

```
accountName myaccount 
accountKey myaccesskey== 
containerName mycontainer 
blobEndpoint myaccount.blob.local.azurestack.external
```

Wanneer het token voor gedeelde toegang is opgegeven, is het configuratiebestand van de referenties in de volgende indeling:

```  
accountName myaccount 
sasToken ?mysastoken 
containerName mycontainer 
blobEndpoint myaccount.blob.local.azurestack.external
```

## <a name="next-steps"></a>Volgende stappen

* [Storage explorer verbinden met een Azure Stack-abonnement](azure-stack-storage-connect-se.md)
* [Aan de slag met Opslagverkenner](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Azure consistente: verschillen en overwegingen](azure-stack-acs-differences.md)
* [Inleiding tot Microsoft Azure storage](../../storage/common/storage-introduction.md)
