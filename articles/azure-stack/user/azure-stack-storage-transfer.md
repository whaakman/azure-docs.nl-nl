---
title: Hulpprogramma's voor de opslag van Azure-Stack
description: Meer informatie over Azure-Stack opslaggegevens hulpmiddelen voor bestandsoverdracht
services: azure-stack
documentationcenter: 
author: xiaofmao
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: xiaofmao
ms.openlocfilehash: 9799498a11449a9ed496d0fdb40312603eda064e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="tools-for-azure-stack-storage"></a>Hulpprogramma's voor de opslag van Azure Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Microsoft Azure-Stack biedt een set van de storage-services voor schijven, blobs, tabellen, wachtrijen en functionaliteit voor Gebruikersaccountbeheer. U kunt een set hulpprogramma's van Azure Storage gebruiken als u wilt beheren of gegevens te verplaatsen naar of van de Azure-Storage-Stack. Dit artikel bevat een overzicht van de beschikbare hulpprogramma's.

Het hulpprogramma die het beste werkt, is afhankelijk van uw vereisten:
* [AzCopy](#azcopy)

    Een opslag-specifieke opdrachtregelhulpprogramma waarmee u downloaden kunt om gegevens te kopiëren van het ene object naar de andere binnen uw opslagaccount of tussen opslagaccounts.

* [Azure PowerShell](#azure-powershell)

    Een taakgebaseerde opdrachtregel-shell en scripttaal die speciaal is ontworpen voor systeembeheer.

* [Azure CLI](#azure-cli)

    Een open-source platformoverschrijdende hulpprogramma waarmee een reeks opdrachten voor het werken met de Azure en Azure Stack-platforms.

* [Microsoft Opslagverkenner (Preview)](#microsoft-azure-storage-explorer)

    Een eenvoudig te gebruiken zelfstandige app met een gebruikersinterface.

Vanwege de Storage-services verschillen tussen Azure en Azure-Stack, is er mogelijk een aantal specifieke vereisten voor elk hulpprogramma dat wordt beschreven in de volgende secties. Zie voor een vergelijking tussen de Stack van Azure storage en Azure storage [Azure Stack Storage: verschillen en overwegingen](azure-stack-acs-differences.md).


## <a name="azcopy"></a>AzCopy
AzCopy is een opdrachtregelprogramma dat is ontworpen om gegevens te kopiëren naar en van Microsoft Azure-Blob en Table storage met eenvoudige opdrachten met optimale prestaties. U kunt gegevens van het ene object naar de andere kopiëren binnen uw opslagaccount of tussen opslagaccounts. Er zijn twee versie van de AzCopy: AzCopy voor Windows en AzCopy op Linux. Azure-Stack ondersteunt alleen de Windows-versie. 
 
### <a name="download-and-install-azcopy"></a>Downloaden en installeren van AzCopy 
[Download](https://aka.ms/azcopyforazurestack) de ondersteunde Windows-versie van AzCopy voor Azure-Stack. U kunt installeren en gebruiken van AzCopy op Azure-Stack dezelfde manier als Azure. Zie voor meer informatie, [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../../storage/common/storage-use-azcopy.md). 

### <a name="azcopy-command-examples-for-data-transfer"></a>Voorbeelden van AzCopy-opdracht voor gegevensoverdracht
De volgende voorbeelden ziet enkele typische scenario's voor het kopiëren van gegevens naar en van de Stack Azure blobs. Zie voor meer informatie, [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../../storage/storage-use-azcopy.md). 
#### <a name="download-all-blobs-to-local-disk"></a>Alle blobs downloaden naar de lokale schijf
```azcopy
AzCopy.exe /source:https://myaccount.blob.local.azurestack.external/mycontainer /dest:C:\myfolder /sourcekey:<key> /S
```
#### <a name="upload-single-file-to-virtual-directory"></a>Bestand uploaden naar virtuele map 
```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.local.azurestack.external/mycontainer/vd /DestKey:key /Pattern:abc.txt
```
#### <a name="move-data-between-azure-and-azure-stack-storage"></a>Gegevens verplaatsen tussen Azure en Azure Stack Storage 
Asynchrone gegevensoverdracht tussen Azure Storage en Azure-Stack wordt niet ondersteund. u moet opgeven van de overdracht met de `/SyncCopy` optie. 
```azcopy 
Azcopy /Source:https://myaccount.blob.local.azurestack.external/mycontainer /Dest:https://myaccount2.blob.core.windows.net/mycontainer2 /SourceKey:AzSKey /DestKey:Azurekey /S /SyncCopy
```

### <a name="azcopy-known-issues"></a>Azcopy bekende problemen
* Eventuele AzCopy-bewerking op de opslag van bestanden is niet beschikbaar omdat de opslag van bestanden is nog niet beschikbaar in Azure-Stack.
* Asynchrone gegevensoverdracht tussen Azure Storage en Azure-Stack wordt niet ondersteund. U kunt opgeven dat de overdracht met de `/SyncCopy` optie om de gegevens te kopiëren.
* De Linux-versie van Azcopy wordt niet ondersteund voor de opslag van de Azure-Stack. 

## <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell is een module met cmdlets voor het beheren van services op de Azure- en Azure-Stack. Het is een op taken gebaseerde opdrachtregelshell en scripttaal die speciaal is ontworpen voor systeembeheer.

### <a name="install-and-configure-powershell-for-azure-stack"></a>PowerShell installeren en configureren voor Azure Stack
Azure Stack compatibele Azure PowerShell-modules zijn vereist voor het werken met Azure-Stack. Zie voor meer informatie [Installeer PowerShell voor Azure-Stack](azure-stack-powershell-install.md) en [configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](azure-stack-powershell-configure-user.md) voor meer informatie.

### <a name="powershell-sample-script-for-azure-stack"></a>PowerShell-voorbeeldscript voor Azure-Stack 
Dit voorbeeld wordt ervan uitgegaan dat u hebt met succes [Installeer PowerShell voor Azure-Stack](azure-stack-powershell-install.md). Dit script wordt helpen u de configuratie van de conplete en referenties voor uw account toevoegen aan de lokale PowerShell environemnt vraagt u uw Azure-Stack-tenant. Vervolgens wordt het script de Azure-abonnement instellen, een nieuw opslagaccount maken in Azure, een nieuwe container in deze nieuwe opslagaccount maken en uploaden van een bestaand installatiekopiebestand (blob) op die container. Nadat het script geeft een lijst van alle blobs in de container, worden een nieuwe bestemming-map maken in uw lokale computer en het installatiekopiebestand te downloaden.

1. Installeer [Azure Stack-compatibele Azure PowerShell-modules](azure-stack-powershell-install.md).  
2. Download de [hulpprogramma's voor het werken met Azure-Stack](azure-stack-powershell-download.md).  
3. Open **Windows PowerShell ISE** en **als Administrator uitvoeren**, klikt u op **bestand** > **nieuw** voor het maken van een script.
4. Het onderstaande script kopiëren en plakken naar het nieuwe scriptbestand.
5. De scriptvariabelen op basis van uw configuratie-instellingen worden bijgewerkt. 
6. Opmerking: dit script moet worden uitgevoerd onder de hoofdmap van gedownloade **AzureStack_Tools**. 

```PowerShell 
# begin

$ARMEvnName = "AzureStackUser" # set AzureStackUser as your Azure Stack environemnt name
$ARMEndPoint = "https://management.local.azurestack.external" 
$GraphAudiance = "https://graph.windows.net/" 
$AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com" 

$SubscriptionName = "basic" # Update with the name of your subscription.
$ResourceGroupName = "myTestRG" # Give a name to your new resource group.
$StorageAccountName = "azsblobcontainer" # Give a name to your new storage account. It must be lowercase!
$Location = "Local" # Choose "Local" as an example.
$ContainerName = "photo" # Give a name to your new container.
$ImageToUpload = "C:\temp\Hello.jpg" # Prepare an image file and a source directory in your local computer.
$DestinationFolder = "C:\temp\downlaod" # A destination directory in your local computer.

# Import the Connect PowerShell module"
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
Import-Module .\Connect\AzureStack.Connect.psm1

# Configure the PowerShell environment
# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRmEnvironment -Name $ARMEvnName -ARMEndpoint $ARMEndPoint 

# Set the GraphEndpointResourceId value
Set-AzureRmEnvironment -Name $ARMEvnName -GraphEndpoint $GraphAudiance

# Login
$TenantID = Get-AzsDirectoryTenantId -AADTenantName $AADTenantName -EnvironmentName $ARMEvnName
Login-AzureRmAccount -EnvironmentName $ARMEvnName -TenantId $TenantID 

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

### <a name="powershell-known-issues"></a>PowerShell bekende problemen 
Met de huidige compatibele Azure PowerShell-moduleversie voor Azure-Stack is 1.2.10. Dit wijkt af van de meest recente versie van Azure PowerShell. Dit verschil geldt voor opslagbewerking services:

* De indeling van de geretourneerde waarde van `Get-AzureRmStorageAccountKey` in versie 1.2.10 heeft twee eigenschappen: `Key1` en `Key2`, terwijl de huidige versie van de Azure retourneert een matrix met alle sleutels van het account.
   ```
   # This command gets a specific key for a Storage account, 
   # and works for Azure PowerShell version 1.4, and later versions.
   (Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
   -AccountName "MyStorageAccount").Value[0]

   # This command gets a specific key for a Storage account, 
   # and works for Azure PowerShell version 1.3.2, and previous versions.
   (Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
   -AccountName "MyStorageAccount").Key1

   ```
   Zie voor meer informatie [Get-AzureRmStorageAccountKey](https://docs.microsoft.com/powershell/module/azurerm.storage/Get-AzureRmStorageAccountKey?view=azurermps-4.1.0).

## <a name="azure-cli"></a>Azure CLI
De Azure CLI is vanaf de opdrachtregel van Azure-ervaring voor het beheren van Azure-resources. U kunt deze op Mac OS-, Linux- en Windows installeren en uitvoeren vanaf de opdrachtregel. 

Azure CLI is geoptimaliseerd voor het beheren en Azure-resources beheren vanaf de opdrachtregel en voor het bouwen van automatiseringsscripts die met Azure Resource Manager werken. Het biedt veel van dezelfde functies gevonden in de Stack Azure-portal, waaronder rich gegevenstoegang.

Azure Stack vereist Azure CLI versie 2.0. Zie voor meer informatie over het installeren en configureren van Azure CLI met Azure-Stack [installeren en configureren van Azure Stack CLI](azure-stack-connect-cli.md). Zie voor meer informatie over het gebruik van de Azure CLI 2.0 voor het uitvoeren van verschillende taken die werken met resources in uw Azure-Stack opslagaccount [met behulp van de CLI2.0 Azure met Azure Storage](../../storage/storage-azure-cli.md)

### <a name="azure-cli-sample-script-for-azure-stack"></a>Azure CLI-voorbeeldscript voor Azure-Stack 
Nadat u de CLI-installatie en configuratie hebt voltooid, kunt u de volgende stappen uit om te werken met een kleine shell-voorbeeldscript om te communiceren met de Azure-Stack opslagbronnen kunt proberen. Het script maakt eerst een nieuwe container in uw opslagaccount en vervolgens een bestaand bestand uploadt (als een blob) op die container, een lijst met alle blobs in de container en ten slotte wordt het bestand wordt gedownload naar een bestemming op uw lokale computer die u opgeeft. Voordat u dit script uitvoert, controleert u of u verbinding maken en meld u aan met de doel-Azure-Stack. 
1. Uw favoriete teksteditor openen en vervolgens kopieert en plakt u dit script in de editor.
2. Update van het script variabelen om uw configuratie-instellingen weer te geven. 
3. Nadat u de benodigde variabelen hebt bijgewerkt, sla het script op en sluit de editor af. De volgende stappen wordt ervan uitgegaan dat u hebt uw script my_storage_sample.sh genoemd.
4. Markeer het script als uitvoerbare, indien nodig:`chmod +x my_storage_sample.sh`
5. Voer het script. Bijvoorbeeld in Bash:`./my_storage_sample.sh`

```bash
#!/bin/bash
# A simple Azure Stack Storage example script

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

## <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer

Microsoft Azure Storage Explorer is een zelfstandige app van Microsoft. Hiermee kunt u eenvoudig werken met Azure Storage en Azure Stack Storage-gegevens op Windows, Mac OS- en Linux. Als u een eenvoudige manier voor het beheren van uw Azure-Stack Storage-gegevens wilt, klikt u vervolgens Overweeg het gebruik van Microsoft Azure Storage Explorer.

Zie voor meer informatie over het configureren van Azure Storage Explorer om te werken met Azure-Stack [Opslagverkenner verbinding maken met een Azure-Stack-abonnement](azure-stack-storage-connect-se.md).

Zie voor meer informatie over Microsoft Azure Storage Explorer [aan de slag met Opslagverkenner (Preview)](../../vs-azure-tools-storage-manage-with-storage-explorer.md)

## <a name="next-steps"></a>Volgende stappen
* [Storage Explorer verbinding met een Azure-Stack-abonnement](azure-stack-storage-connect-se.md)
* [Aan de slag met Opslagverkenner (Preview)](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Consistent zijn met Azure storage: verschillen en overwegingen](azure-stack-acs-differences.md)
* [Inleiding tot Microsoft Azure Storage](../../storage/common/storage-introduction.md)

