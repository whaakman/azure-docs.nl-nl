---
title: Azure-bestandsshares beheren met Azure CLI
description: Leer hoe u Azure Files beheert met behulp van Azure CLI.
services: storage
documentationcenter: na
author: wmgries
manager: aungoo
editor: tamram
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/26/2018
ms.author: wgries
ms.openlocfilehash: 00fd984a6bed8691712df0d4c335d2b9d4fd3ffa
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029387"
---
# <a name="manage-azure-file-shares-using-azure-cli"></a>Azure-bestandsshares beheren met Azure CLI
[Azure Files](storage-files-introduction.md) is het gebruiksvriendelijke cloudbestandssysteem van Microsoft. Azure-bestandsshares kunnen worden gekoppeld in Windows, Linux en macOS. In dit artikel worden de basisbeginselen besproken van het werken met Azure-bestandsshares met behulp van Azure CLI. Leer hoe u het volgende doet: 

> [!div class="checklist"]
> * Een resourcegroep en een opslagaccount maken
> * Een Azure-bestandsshare maken 
> * Een map maken
> * Bestand uploaden 
> * Bestand downloaden
> * Een momentopname van een share maken en gebruiken

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om Azure CLI lokaal te installeren en te gebruiken, moet u voor de stappen in dit artikel Azure CLI-versie 2.0.4 of hoger uitvoeren. Voer **az --version** uit om uw Azure CLI-versie te vinden. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

Azure CLI-opdrachten retourneren standaard JavaScript Object Notation (JSON). JSON is de standaardmethode voor het verzenden en ontvangen van berichten van REST API's. Om te kunnen werken met JSON-antwoorden, wordt in enkele van de voorbeelden in dit artikel de parameter *query* gebruikt in de Azure CLI-opdrachten. Deze parameter gebruikt de [JMESPath-querytaal](http://jmespath.org/) voor het parseren van JSON. Raadpleeg de [zelfstudie over JMESPath](http://jmespath.org/tutorial.html) voor meer informatie over hoe u de resultaten van Azure CLI-opdrachten kunt gebruiken door de JMESPath-querytaal te volgen.

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Als u nog geen Azure-resourcegroep hebt, kunt u met de opdracht [az group create](/cli/azure/group#create) een groep maken. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *East US*:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Create a storage account
Een opslagaccount is een gedeelde opslaggroep waarin u Azure-bestandsshares of andere opslagresources, zoals blobs of wachtrijen, kunt implementeren. Een opslagaccount kan een onbeperkt aantal bestandsshares bevatten. Een share kan een onbeperkt aantal bestanden opslaan, tot de capaciteitslimiet van het opslagaccount.

In het volgende voorbeeld wordt een opslagaccount gemaakt met de naam *mystorageaccount\<willekeurig nummer\>* met behulp van de opdracht [az storage account create](/cli/azure/storage/account#create), waarna de naam van dat opslagaccount in de variabele `$STORAGEACCT` wordt geplaatst. Namen van opslagaccounts moeten uniek zijn. Met behulp van `$RANDOM` wordt een nummer aan de naam van het opslagaccount toegevoegd om deze uniek te maken. 

```azurecli-interactive 
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

### <a name="get-the-storage-account-key"></a>Opslagaccountsleutel opvragen
Opslagaccountsleutels regelen de toegang tot resources in een opslagaccount. Wanneer u een opslagaccount maakt, worden de sleutels automatisch gemaakt. U kunt de opslagaccountsleutels voor uw opslagaccount opvragen met de opdracht [az storage account keys list](/cli/azure/storage/account/keys#list): 

```azurecli-interactive 
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken
U kunt nu uw eerste Azure-bestandsshare maken. U kunt bestandsshares maken met de opdracht [az storage share create](/cli/azure/storage/share#create). In dit voorbeeld wordt een Azure-bestandsshare gemaakt met de naam *myshare*: 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

> [!IMPORTANT]  
> De namen van shares mogen alleen kleine letters, cijfers en enkele afbreekstreepjes bevatten, maar mogen niet met een afbreekstreepje beginnen. Zie [Naming and referencing shares, directories, files, and metadata](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata) (Shares, mappen, bestanden en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de naamgeving van bestandsshares en bestanden.

## <a name="work-with-the-contents-of-an-azure-file-share"></a>Werken met de inhoud van een Azure-bestandsshare
U beschikt nu over een Azure-bestandsshare. De volgende stap is het koppelen van de bestandsshare met SMB op [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) of [macOS](storage-how-to-use-files-mac.md). U kunt ook Azure CLI gebruiken om met uw Azure-bestandsshare te werken. Het voordeel van het gebruik van Azure CLI ten opzichte van het koppelen van de bestandsshare met behulp van SMB is dat alle aanvragen die zijn ingediend via Azure CLI, worden gedaan met behulp van de REST API van het bestand. U kunt de REST API van het bestand gebruiken om bestanden en mappen te maken, te wijzigen en te verwijderen in uw bestandsshare vanaf deze locaties:

- De Bash Azure Cloud Shell (die geen ondersteuning biedt voor het koppelen van bestandsshares via SMB)
- Clients die geen SMB-shares kunnen koppelen, zoals on-premises clients waarvoor poort 445 is geblokkeerd
- Scenario's zonder server, zoals in [Azure Functions](../../azure-functions/functions-overview.md)

### <a name="create-a-directory"></a>Een map maken
Als u in de hoofdmap van uw Azure-bestandsshare een nieuwe map wilt maken met de naam *myDirectory*, gebruikt u de opdracht [`az storage directory create`](/cli/azure/storage/directory#az_storage_directory_create):

```azurecli-interactive
az storage directory create \
   --account-name $STORAGEACCT \
   --account-key $STORAGEKEY \
   --share-name "myshare" \
   --name "myDirectory" 
```

### <a name="upload-a-file"></a>Bestand uploaden
Om te laten zien hoe u een bestand uploadt met behulp van de opdracht [`az storage file upload`](/cli/azure/storage/file#az_storage_file_upload), maakt u eerst een bestand dat u naar de scratch-schijf van Cloud Shell kunt uploaden. In het volgende voorbeeld maakt u het bestand en uploadt u het bestand:

```azurecli-interactive
date > ~/clouddrive/SampleUpload.txt

az storage file upload \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --source "~/clouddrive/SampleUpload.txt" \
    --path "myDirectory/SampleUpload.txt"
```

Als u Azure CLI lokaal uitvoert, vervangt u `~/clouddrive` door een bestaand pad op uw computer.

Nadat het bestand is geüpload, kunt u de opdracht [`az storage file list`](/cli/azure/storage/file#az_storage_file_list) gebruiken om te controleren of het bestand daadwerkelijk is geüpload naar uw Azure-bestandsshare:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory" \
    --output table
```

### <a name="download-a-file"></a>Bestand downloaden
U kunt de opdracht [`az storage file download`](/cli/azure/storage/file#az_storage_file_download) gebruiken om een kopie te downloaden van het bestand dat u hebt geüpload naar de scratch-schijf van Cloud Shell:

```azurecli-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists, because you've run this example before
rm -rf ~/clouddrive/SampleDownload.txt

az storage file download \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt" \
    --dest "~/clouddrive/SampleDownload.txt"
```

### <a name="copy-files"></a>Bestanden kopiëren
Een veelvoorkomende taak is het kopiëren van bestanden tussen bestandsshares of van of naar een opslagcontainer van Azure Blob. Maak een nieuwe share, zodat u deze functionaliteit ziet. Kopieer het bestand dat u hebt geüpload naar deze nieuwe share met behulp van de opdracht [az storage file copy](/cli/azure/storage/file/copy): 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare2"

az storage directory create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --name "myDirectory2"

az storage file copy start \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --source-share "myshare" \
    --source-path "myDirectory/SampleUpload.txt" \
    --destination-share "myshare2" \
    --destination-path "myDirectory2/SampleCopy.txt"
```

Als u nu de bestanden in de nieuwe share opvraagt, ziet u het gekopieerde bestand:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --output table
```

Hoewel de opdracht `az storage file copy start` handig is voor het verplaatsen van bestanden tussen Azure-bestandsshares en Azure Blob-opslagcontainers, is het beter om AzCopy te gebruiken voor grotere bewerkingen. (Groter voor wat betreft het aantal bestanden of de grootte van de verplaatste bestanden.) U vindt hier meer informatie over [AzCopy voor Linux](../common/storage-use-azcopy-linux.md) en [AzCopy voor Windows](../common/storage-use-azcopy.md). AzCopy moet lokaal zijn geïnstalleerd. AzCopy is niet beschikbaar in Cloud Shell. 

## <a name="create-and-modify-share-snapshots"></a>Momentopnamen van shares maken en wijzigen
Een andere handige taak die u kunt doen met een Azure-bestandsshare is het maken van een momentopname van de share. Een momentopname bevat voor een specifiek moment de actuele inhoud van een Azure-bestandsshare. Momentopnamen van een share zijn vergelijkbaar met bepaalde technologieën van besturingssystemen die u mogelijk al kent, zoals:
- Momentopnamen van [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) voor Linux-systemen.
- Momentopnamen van [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) voor macOS
- [Volume Shadow Copy Service (VSS)](https://docs.microsoft.com/en-us/windows/desktop/VSS/volume-shadow-copy-service-portal) voor Windows-bestandssystemen, zoals NTFS en ReFS

U kunt een momentopname van een share maken met behulp van de opdracht [`az storage share snapshot`](/cli/azure/storage/share#az_storage_share_snapshot):

```azurecli-interactive
SNAPSHOT=$(az storage share snapshot \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --query "snapshot" | tr -d '"')
```

### <a name="browse-share-snapshot-contents"></a>Bladeren door inhoud van share-momentopnamen
U kunt door de inhoud van een momentopname van een share bladeren door het tijdstempel van de momentopname dat u hebt vastgelegd in de variabele `$SNAPSHOT`, door te geven aan de opdracht `az storage file list`:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --snapshot $SNAPSHOT \
    --output table
```

### <a name="list-share-snapshots"></a>Momentopnamen van shares opvragen
Gebruik de volgende opdracht om een lijst weer te geven met de momentopnamen die u hebt gemaakt voor de share:

```azurecli-interactive
az storage share list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --include-snapshot \
    --query "[? name=='myshare' && snapshot!=null]" | tr -d '"'
```

### <a name="restore-from-a-share-snapshot"></a>Bestand herstellen vanuit een share-momentopname
U kunt een bestand herstellen met behulp van de opdracht `az storage file copy start` die u eerder hebt gebruikt. Verwijder eerst het SampleUpload.txt-bestand dat u hebt geüpload, zodat u dit kunt herstellen vanuit de momentopname:

```azurecli-interactive
# Delete SampleUpload.txt
az storage file delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt"

# Build the source URI for a snapshot restore
URI=$(az storage account show \
    --resource-group "myResourceGroup" \
    --name $STORAGEACCT \
    --query "primaryEndpoints.file" | tr -d '"')

URI=$URI"myshare/myDirectory/SampleUpload.txt?sharesnapshot="$SNAPSHOT

# Restore SampleUpload.txt from the share snapshot
az storage file copy start \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --source-uri $URI \
    --destination-share "myshare" \
    --destination-path "myDirectory/SampleUpload.txt"
```

### <a name="delete-a-share-snapshot"></a>Een share-momentopname verwijderen
U kunt een momentopname van een share verwijderen met behulp van de opdracht [`az storage share delete`](/cli/azure/storage/share#az_storage_share_delete). Gebruik de variabele die de verwijzing `$SNAPSHOT` naar de `--snapshot`-parameter bevat:

```azurecli-interactive
az storage share delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --snapshot $SNAPSHOT
```

## <a name="clean-up-resources"></a>Resources opschonen
Als u klaar bent, kunt u de opdracht [`az group delete`](/cli/azure/group#delete) gebruiken om de resourcegroep en alle bijbehorende resources te verwijderen: 

```azurecli-interactive 
az group delete --name "myResourceGroup"
```

U kunt resources ook afzonderlijk verwijderen.
- De Azure-bestandsshares verwijderen die u voor dit artikel hebt gemaakt:

    ```azurecli-interactive
    az storage share delete \
        --account-name $STORAGEACCT \
        --account-key $STORAGEKEY \
        --name "myshare" \
        --delete-snapshots include

    az storage share delete \
        --account-name $STORAGEACCT \
        --account-key $STORAGEKEY \
        --name "myshare2" \
        --delete-snapshots include
    ```

- Het opslagaccount zelf verwijderen. (Dit verwijdert impliciet de Azure-bestandsshares en alle andere opslagresources, zoals een Azure Blob-opslagcontainer, die u hebt gemaakt.)

    ```azurecli-interactive
    az storage account delete \
        --resource-group "myResourceGroup" \
        --name $STORAGEACCT \
        --yes
    ```

## <a name="next-steps"></a>Volgende stappen
- [Bestandsshares beheren met Azure Portal](storage-how-to-use-files-portal.md)
- [Bestandsshares beheren met Azure PowerShell](storage-how-to-use-files-powershell.md)
- [Bestandsshares beheren met Storage Explorer](storage-how-to-use-files-storage-explorer.md)
- [Implementatie van Azure Files plannen](storage-files-planning.md)