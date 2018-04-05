---
title: Azure-bestandsshares beheren met Azure CLI
description: Leer hoe u Azure Files beheert met behulp van Azure CLI.
services: storage
documentationcenter: na
author: wmgries
manager: jeconnoc
editor: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/26/2018
ms.author: wgries
ms.openlocfilehash: 066a43b553be18a5a0bc889fff441824df301b98
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
---
# <a name="managing-azure-file-shares-using-the-azure-cli"></a>Azure-bestandsshares beheren met Azure CLI
[Azure Files ](storage-files-introduction.md) is het eenvoudig te gebruiken cloudbestandssysteem van Microsoft. Azure-bestandsshares kunnen worden gekoppeld in Windows, Linux en macOS. In deze handleiding worden de basisbeginselen besproken van het werken met Azure-bestandsshares met behulp van Azure CLI. Leer hoe u het volgende doet: 

> [!div class="checklist"]
> * Een resourcegroep en een opslagaccount maken
> * Een Azure-bestandsshare maken 
> * Een map maken
> * Bestand uploaden 
> * Bestand downloaden
> * Een momentopname van een share maken en gebruiken

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om Azure CLI lokaal te installeren en te gebruiken, moet u voor deze handleiding Azure CLI 2.0.4 of nieuwer hebben. Voer **az --version** uit om de versie op te vragen. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

De standaardinstelling is dat opdrachten van Azure CLI JSON (JavaScript Object Notation) retourneren, de gebruikelijke manier voor het verzenden en ontvangen van berichten van REST-API's. Om te kunnen werken met de JSON-antwoorden, wordt in enkele van de voorbeelden in deze handleiding de parameter 'query' gebruikt in de Azure CLI-opdrachten. Deze parameter gebruikt de [JMESPath-querytaal](http://jmespath.org/) voor het parseren van JSON. Volg de [zelfstudie over JMESPath](http://jmespath.org/tutorial.html) voor meer informatie over het bewerken van de resultaten van de Azure CLI-opdrachten.

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Als u nog geen Azure-resourcegroep hebt, kunt u een nieuwe groep maken met de opdracht [az group create](/cli/azure/group#create). 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *East US*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Een opslagaccount maken
Een opslagaccount is een gedeelde opslaggroep waarin u Azure-bestandsshares of andere opslagresources, zoals blobs of wachtrijen, kunt implementeren. Een opslagaccount kan een onbeperkt aantal bestandsshares bevatten en een share kan een onbeperkt aantal bestanden bevatten, totdat de capaciteit van het opslagaccount is bereikt.

In dit voorbeeld maakt u een opslagaccount met de naam `mystorageaccount<random number>` met behulp van de opdracht [az storage account create](/cli/azure/storage/account#create), waarna de naam van dat opslagaccount in de variabele `$STORAGEACCT` wordt geplaatst. Namen van opslagaccounts moeten uniek zijn. Gebruik `$RANDOM` om een nummer toe te voegen aan het einde van de naam van het opslagaccount om deze uniek te maken. 

```azurecli-interactive 
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

### <a name="get-the-storage-account-key"></a>Opslagaccountsleutel opvragen
Opslagaccountsleutels worden gebruikt om de toegang tot resources in een opslagaccount te regelen. Wanneer u een opslagaccount maakt, worden er automatisch sleutels voor het account gemaakt. U kunt de opslagaccountsleutels voor het opslagaccount opvragen met de opdracht [az storage account keys list](/cli/azure/storage/account/keys#list). 

```azurecli-interactive 
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken
U kunt nu uw eerste Azure-bestandsshare maken. U kunt bestandsshares maken met de opdracht [az storage share create](/cli/azure/storage/share#create). In dit voorbeeld wordt een Azure-bestandsshare gemaakt met de naam `myshare`. 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

> [!Important]  
> De namen van shares moeten bestaan uit kleine letters, cijfers en afbreekstreepjes, maar mogen niet beginnen met een afbreekstreepje. Zie [Naming and Referencing Shares, Directories, Files, and Metadata](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata) (Shares, mappen, bestanden en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de naamgeving van bestandsshares en bestanden.

## <a name="manipulating-the-contents-of-the-azure-file-share"></a>Inhoud van Azure-bestandsshare bewerken
U beschikt nu over een Azure-bestandsshare. De volgende stap is het koppelen van de bestandsshare met SMB op [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) of [macOS](storage-how-to-use-files-mac.md). U kunt uw Azure-bestandsshare ook bewerken met Azure CLI. Dit heeft de voorkeur boven het koppelen van de bestandsshare met SMB, omdat alle aanvragen met Azure CLI worden gedaan met de REST-API van File zodat u bestanden en mappen in de bestandsshare kunt maken, wijzigen en verwijderen vanuit:

- De Bash Cloud Shell (die geen ondersteuning biedt voor het koppelen van bestandsshares via SMB).
- Clients die geen SMB-shares kunnen koppelen, zoals on-premises clients waarvoor poort 445 is geblokkeerd.
- Scenario's zonder server, zoals in [Azure Functions](../../azure-functions/functions-overview.md). 

### <a name="create-directory"></a>Map maken
Als u in de hoofdmap van uw Azure-bestandsshare een nieuwe map wilt maken met de naam *myDirectory*, gebruikt u de opdracht [`az storage directory create`](/cli/azure/storage/directory#az_storage_directory_create).

```azurecli-interactive
az storage directory create \
   --account-name $STORAGEACCT \
   --account-key $STORAGEKEY \
   --share-name "myshare" \
   --name "myDirectory" 
```

### <a name="upload-a-file"></a>Bestand uploaden
Om te laten zien hoe u met de opdracht [`az storage file upload`](/cli/azure/storage/file#az_storage_file_upload) een bestand kunt uploaden, moeten we eerst een bestand op de scratch-schijf van uw Azure CLI Cloud Shell maken om te uploaden. In het volgende voorbeeld maken we het bestand waarna het wordt geüpload.

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

Nadat het bestand is geüpload, kunt u de opdracht [`az storage file list`](/cli/azure/storage/file#az_storage_file_list) gebruiken om te controleren of het bestand daadwerkelijk is geüpload naar uw Azure-bestandsshare.

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory" \
    --output table
```

### <a name="download-a-file"></a>Bestand downloaden
U kunt de opdracht [`az storage file download`](/cli/azure/storage/file#az_storage_file_download) gebruiken om een kopie te downloaden van het bestand dat u hebt geüpload naar de scratch-schijf van uw Cloud Shell.

```azurecli-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists because you've run this example before
rm -rf ~/clouddrive/SampleDownload.txt

az storage file download \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt" \
    --dest "~/clouddrive/SampleDownload.txt"
```

### <a name="copy-files"></a>Bestanden kopiëren
Een veelvoorkomende taak is het kopiëren van bestanden tussen bestandsshares of van of naar een opslagcontainer van Azure Blob. U kunt deze functionaliteit uitproberen door een nieuwe share te maken en het bestand dat u hebt geüpload, met de opdracht [az storage file copy](/cli/azure/storage/file/copy) naar deze nieuwe share te kopiëren. 

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

Als u nu de bestanden in de nieuwe share opvraagt, ziet u het gekopieerde bestand.

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --output table
```

Hoewel de opdracht `az storage file copy start` handig is voor het ad hoc verplaatsen van bestanden tussen Azure-bestandsshares en Azure Blob-opslagcontainers, is het beter om AzCopy te gebruiken voor grotere bewerkingen (voor wat betreft het aantal bestanden of de grootte van de verplaatste bestanden). U vindt hier meer informatie over [AzCopy voor Linux](../common/storage-use-azcopy-linux.md) en [AzCopy voor Windows](../common/storage-use-azcopy.md). AzCopy moet lokaal worden geïnstalleerd. De opdracht is niet beschikbaar in Cloud Shell. 

## <a name="create-and-modify-share-snapshots"></a>Momentopnamen van shares maken en wijzigen
Nog een andere handige taak die u kunt doen met een Azure-bestandsshare is het maken van een momentopname van de share. Een momentopname bevat voor een specifiek moment de actuele inhoud van een Azure-bestandsshare. Momentopnamen van een share zijn vergelijkbaar met technologieën van besturingssystemen die u mogelijk al kent, zoals:
- Momentopnamen van [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) voor Linux-systemen.
- Momentopnamen van [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) voor macOS. 
- [Volume Shadow Copy Service (VSS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee923636) voor Windows-bestandssystemen zoals NTFS en ReFS

U kunt een momentopname van een share maken met behulp van de opdracht [`az storage share snapshot`](/cli/azure/storage/share#az_storage_share_snapshot).

```azurecli-interactive
SNAPSHOT=$(az storage share snapshot \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --query "snapshot" | tr -d '"')
```

### <a name="browse-share-snapshot-contents"></a>Bladeren door inhoud van share-momentopnamen
U kunt door de inhoud van een momentopname van een share bladeren door het tijdstempel van de momentopname dat we hebben vastgelegd in de variabele `$SNAPSHOT`, door te geven aan de opdracht `az storage file list`.

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --snapshot $SNAPSHOT \
    --output table
```

### <a name="list-share-snapshots"></a>Momentopnamen van shares opvragen
Gebruik de volgende opdracht om een lijst op te vragen met de momentopnamen die u hebt gemaakt voor de share.

```azurecli-interactive
az storage share list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --include-snapshot \
    --query "[? name=='myshare' && snapshot!=null]" | tr -d '"'
```

### <a name="restore-from-a-share-snapshot"></a>Bestand herstellen vanuit een share-momentopname
U kunt een bestand herstellen met behulp van de opdracht `az storage file copy start` die we eerder hebben gebruikt. We verwijderen eerst het bestand `SampleUpload.txt` dat we hebben geüpload, zodat we het vanuit de momentopname kunnen herstellen.

```azurecli-interactive
# Delete SampleUpload.txt
az storage file delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt"

# Build the source URI for snapshot restore
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
U kunt een momentopname van een share verwijderen met behulp van de opdracht [`az storage share delete`](/cli/azure/storage/share#az_storage_share_delete), met de variabele met de verwijzing `$SNAPSHOT` naar de parameter `--snapshot`.

```azurecli-interactive
az storage share delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --snapshot $SNAPSHOT
```

## <a name="clean-up-resources"></a>Resources opschonen
Als u klaar bent, kunt u de opdracht [`az group delete`](/cli/azure/group#delete) gebruiken om de resourcegroep en alle bijbehorende resources te verwijderen. 

```azurecli-interactive 
az group delete --name "myResourceGroup"
```

U kunt er ook voor kiezen om resources één voor één te verwijderen:
- De Azure-bestandsshares verwijderen die we voor deze snelstartgids hebben gemaakt:

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

- Het opslagaccount zelf verwijderen (hierdoor worden impliciet de Azure-bestandsshares verwijderd die we hebben gemaakt, evenals andere opslagresources die u hebt gemaakt zoals een Azure Blob-opslagcontainer):

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