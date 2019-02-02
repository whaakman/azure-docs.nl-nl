---
title: De Azure CLI gebruiken met Azure Storage | Microsoft Docs
description: Informatie over het gebruik van de Azure-opdrachtregelinterface (Azure CLI) met Azure Storage te maken en beheren van storage-accounts en werken met Azure-blobs en bestanden.
services: storage
author: roygara
ms.service: storage
ms.devlang: azurecli
ms.topic: article
ms.date: 06/02/2017
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: d815bae2478d8853c63cd773cbfeaf5d3e90458c
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55659760"
---
# <a name="using-the-azure-cli-with-azure-storage"></a>De Azure CLI gebruiken met Azure Storage

De open-source, platformoverschrijdende Azure-CLI biedt een reeks opdrachten voor het werken met het Azure-platform. Het biedt veel van dezelfde functionaliteit gevonden in de [Azure-portal](https://portal.azure.com), met inbegrip van toegang tot uitgebreide gegevens.

In deze handleiding, laten we zien u hoe u de [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) verschillende taken werken met resources in uw Azure Storage-account uit te voeren. U wordt aangeraden dat u downloaden en installeren of naar de nieuwste versie van de CLI upgraden voordat u deze handleiding.

De voorbeelden in deze handleiding wordt ervan uitgegaan dat het gebruik van de Bash-shell op Ubuntu, maar andere platforms op dezelfde manier moeten uitvoeren. 

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="prerequisites"></a>Vereisten
Deze handleiding wordt ervan uitgegaan dat u inzicht in de basisbeginselen van Azure Storage. Ook wordt ervan uitgegaan dat u voldoet aan de vereisten voor het maken van account die hieronder zijn opgegeven voor Azure en de Storage-service.

### <a name="accounts"></a>Accounts
* **Azure-account**: Als u nog een Azure-abonnement hebt [maken van een gratis Azure-account](https://azure.microsoft.com/free/).
* **Storage-account**: Zie [een opslagaccount maken](storage-quickstart-create-account.md) in [over Azure storage-accounts](storage-create-storage-account.md).

### <a name="install-the-azure-cli"></a>Azure-CLI installeren

Download en installeer de Azure CLI door de instructies die worden beschreven in [Azure CLI installeren](/cli/azure/install-az-cli2).

> [!TIP]
> Als u problemen ondervindt met de installatie, bekijk de [installatieproblemen oplossen](/cli/azure/install-az-cli2) gedeelte van het artikel en de [installeren probleemoplossing](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) handleiding op GitHub.
>

## <a name="working-with-the-cli"></a>Werken met de CLI

Nadat u de CLI hebt geïnstalleerd, kunt u de `az` opdracht in de opdrachtregelinterface (Bash, Terminal, opdrachtprompt) voor toegang tot de Azure CLI-opdrachten. Type de `az` opdracht om te zien van een volledige lijst met de basisopdrachten (in de volgende voorbeelduitvoer is afgekapt):

```
     /\
    /  \    _____   _ _ __ ___
   / /\ \  |_  / | | | \'__/ _ \
  / ____ \  / /| |_| | | |  __/
 /_/    \_\/___|\__,_|_|  \___|


Welcome to the cool new Azure CLI!

Here are the base commands:

    account          : Manage subscriptions.
    acr              : Manage Azure container registries.
    acs              : Manage Azure Container Services.
    ad               : Synchronize on-premises directories and manage Azure Active Directory
                       resources.
    ...
```

Voer de opdracht in de opdrachtregelinterface `az storage --help` aan lijst met de `storage` opdracht subgroepen. De beschrijvingen van de subgroepen biedt een overzicht van de functionaliteit van die de Azure CLI biedt voor het werken met uw storage-resources.

```
Group
    az storage: Durable, highly available, and massively scalable cloud storage.

Subgroups:
    account  : Manage storage accounts.
    blob     : Object storage for unstructured data.
    container: Manage blob storage containers.
    cors     : Manage Storage service Cross-Origin Resource Sharing (CORS).
    directory: Manage file storage directories.
    entity   : Manage table storage entities.
    file     : File shares that use the standard SMB 3.0 protocol.
    logging  : Manage Storage service logging information.
    message  : Manage queue storage messages.
    metrics  : Manage Storage service metrics.
    queue    : Use queues to effectively scale applications according to traffic.
    share    : Manage file shares.
    table    : NoSQL key-value storage using semi-structured datasets.
```

## <a name="connect-the-cli-to-your-azure-subscription"></a>Koppel de Opdrachtregelinterface aan uw Azure-abonnement

Als u wilt werken met de resources in uw Azure-abonnement, u moet eerst aanmelden bij uw Azure-account met `az login`. Er zijn verschillende manieren die kunt u zich aanmeldt:

* **Interactieve aanmelding**: `az login`
* **Meld u aan met de gebruikersnaam en wachtwoord**: `az login -u johndoe@contoso.com -p VerySecret`
  * Dit werkt niet met Microsoft-accounts of accounts met meervoudige verificatie.
* **Meld u aan met een service-principal**: `az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`

## <a name="azure-cli-sample-script"></a>Azure CLI-voorbeeldscript

Vervolgens gaat we een kleine shellscript in dat problemen met een paar eenvoudige Azure CLI-opdrachten voor interactie met Azure Storage-resources gebruiken. Het script eerst maakt u een nieuwe container in uw opslagaccount, vervolgens wordt een bestaand bestand (als een blob) op die container geüpload. Deze worden vervolgens alle blobs in de container en ten slotte wordt het bestand gedownload naar een bestemming op uw lokale computer die u opgeeft.

```bash
#!/bin/bash
# A simple Azure Storage example script

export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_KEY=<storage_account_key>

export container_name=<container_name>
export blob_name=<blob_name>
export file_to_upload=<file_to_upload>
export destination_file=<destination_file>

echo "Creating the container..."
az storage container create --name $container_name

echo "Uploading the file..."
az storage blob upload --container-name $container_name --file $file_to_upload --name $blob_name

echo "Listing the blobs..."
az storage blob list --container-name $container_name --output table

echo "Downloading the file..."
az storage blob download --container-name $container_name --name $blob_name --file $destination_file --output table

echo "Done"
```

**Configureren en voer het script**

1. Open uw favoriete teksteditor en kopieer en plak het vorige script in de editor.

2. Werk vervolgens de variabelen van het script om uw configuratie-instellingen weer te geven. Vervang de volgende waarden zoals opgegeven:

   * **\<storage_account_name\>**  de naam van uw storage-account.
   * **\<storage_account_key\>**  de primaire of secundaire toegangssleutel voor uw storage-account.
   * **\<container_name\>**  een naam van de nieuwe container te maken, zoals "azure-cli-voorbeeld-container".
   * **\<blob_name\>**  een naam op voor de bestemmings-blob in de container.
   * **\<file_to_upload\>**  het pad naar kleine bestanden op uw lokale computer, zoals ' ~ / images/HelloWorld.png '.
   * **\<destination_file\>**  de bestemming het pad, zoals ' ~ / downloadedImage.png '.

3. Nadat u de benodigde variabelen hebt bijgewerkt, sla het script op en sluit de editor. De volgende stappen wordt ervan uitgegaan dat u hebt de naam van uw script **my_storage_sample.sh**.

4. Markeer het script als uitvoerbare bestanden, indien nodig: `chmod +x my_storage_sample.sh`

5. Voer het script. Bijvoorbeeld, in Bash: `./my_storage_sample.sh`

Ziet u uitvoer die vergelijkbaar is met het volgende, en de **\<destination_file\>** u hebt opgegeven in het script moet worden weergegeven op uw lokale computer.

```
Creating the container...
{
  "created": true
}
Uploading the file...
Percent complete: %100.0
Listing the blobs...
Name       Blob Type      Length  Content Type              Last Modified
---------  -----------  --------  ------------------------  -------------------------
README.md  BlockBlob        6700  application/octet-stream  2017-05-12T20:54:59+00:00
Downloading the file...
Name
---------
README.md
Done
```

> [!TIP]
> De bovenstaande uitvoer **tabel** indeling. U kunt opgeven welke indeling moet worden gebruikt door op te geven de uitvoer de `--output` argument in uw CLI-opdrachten, of stel deze wereldwijd met `az configure`.
>

## <a name="manage-storage-accounts"></a>Opslagaccounts beheren

### <a name="create-a-new-storage-account"></a>Een nieuw opslagaccount maken
U hebt een opslagaccount nodig om Azure Storage te gebruiken. U kunt een nieuw Azure-opslagaccount maken nadat u uw computer hebt geconfigureerd [verbinding maken met uw abonnement](#connect-to-your-azure-subscription).

```azurecli
az storage account create \
    --location <location> \
    --name <account_name> \
    --resource-group <resource_group> \
    --sku <account_sku>
```

* `--location` (Vereist): De locatie. Bijvoorbeeld 'VS-West'.
* `--name` (Vereist): De naam van het opslagaccount. De naam moet 3 tot 24 tekens bestaan en gebruik alleen kleine letters alfanumerieke tekens.
* `--resource-group` (Vereist): De naam van de resourcegroep.
* `--sku` (Vereist): Het opslagaccount SKU. Toegestane waarden:
  * `Premium_LRS`
  * `Standard_GRS`
  * `Standard_LRS`
  * `Standard_RAGRS`
  * `Standard_ZRS`

### <a name="set-default-azure-storage-account-environment-variables"></a>Azure storage-standaardaccount omgevingsvariabelen instellen

U kunt meerdere opslagaccounts in uw Azure-abonnement hebben. Als u wilt een van deze moet worden gebruikt voor alle daaropvolgende opslagopdrachten selecteert, kunt u deze omgevingsvariabelen instellen:

Geef eerst uw opslagaccountsleutels weer met behulp van de opdracht [az storage account keys list](/cli/azure/storage/account/keys):

```azurecli-interactive
az storage account keys list \
    --account-name <account_name> \
    --resource-group <resource_group> \
    --output table
```

Nu dat u de sleutel hebt, kunt u deze en de naam van het als omgevingsvariabelen definiëren:

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_KEY=<key>
```

Er is een andere manier om in te stellen van een standaardopslagaccount met behulp van een verbindingsreeks. Haal eerst de verbindingsreeks met de `show-connection-string` opdracht:

```azurecli
az storage account show-connection-string \
    --name <account_name> \
    --resource-group <resource_group>
```

Vervolgens kopieert u de verbindingsreeks van de uitvoer en stelt u de `AZURE_STORAGE_CONNECTION_STRING` omgevingsvariabele (mogelijk moet u de verbindingsreeks plaats tussen aanhalingstekens):

```azurecli
export AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
```

> [!NOTE]
> Alle voorbeelden in de volgende secties van dit artikel wordt ervan uitgegaan dat u hebt ingesteld de `AZURE_STORAGE_ACCOUNT` en `AZURE_STORAGE_KEY` omgevingsvariabelen.

## <a name="create-and-manage-blobs"></a>Maken en blobs beheren
Azure Blob-opslag is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens, die kunnen worden benaderd vanaf elke locatie ter wereld via HTTP of HTTPS. In deze sectie wordt ervan uitgegaan dat u al bekend met concepten van Azure Blob storage bent. Zie voor gedetailleerde informatie [aan de slag met Azure Blob storage met .NET](../blobs/storage-dotnet-how-to-use-blobs.md) en [concepten van Blob Service](/rest/api/storageservices/blob-service-concepts).

### <a name="create-a-container"></a>Een container maken
Elke blob in Azure storage moet zich in een container. U kunt een container maken met behulp van de `az storage container create` opdracht:

```azurecli
az storage container create --name <container_name>
```

U kunt een van drie niveaus van leestoegang voor een nieuwe container instellen door op te geven de optionele `--public-access` argument:

* `off` (standaard): Containergegevens is gebonden aan de eigenaar van het account.
* `blob`: Openbare leestoegang voor blobs.
* `container`: Openbare lees- en toegang tot de volledige container.

Zie [Anonieme leestoegang tot containers en blobs beheren](../blobs/storage-manage-access-to-resources.md) voor meer informatie.

### <a name="upload-a-blob-to-a-container"></a>Een blob uploaden naar een container
Azure Blob storage ondersteunt blok, toevoegen en pagina-blobs. Blobs uploaden naar een container met behulp van de `blob upload` opdracht:

```azurecli
az storage blob upload \
    --file <local_file_path> \
    --container-name <container_name> \
    --name <blob_name>
```

Als u uploaden rechtstreeks naar een map in de container in uw opslagaccount wilt, vervangt u `--name <blob_name>` met `--name <folder/blob_name>`.

 Standaard de `blob upload` opdracht *.vhd bestanden uploadt naar de pagina-blobs of anderszins blok-blobs. Als u wilt een ander type opgeven wanneer u een blob uploadt, kunt u de `--type` argument--toegestane waarden zijn `append`, `block`, en `page`.

 Zie voor meer informatie over de verschillende blobtypen [blok-Blobs, toevoeg-Blobs en pagina-Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).


### <a name="download-a-blob-from-a-container"></a>Een blob downloaden uit een container
In dit voorbeeld ziet u hoe u een blob downloaden uit een container:

```azurecli
az storage blob download \
    --container-name mycontainer \
    --name myblob.png \
    --file ~/mydownloadedblob.png
```

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Lijst van de blobs in een container met de [az storage blob list](/cli/azure/storage/blob#az_storage_blob_list) opdracht.

```azurecli
az storage blob list \
    --container-name mycontainer \
    --output table
```

### <a name="copy-blobs"></a>Blobs kopiëren
U kunt blobs binnen of tussen opslagaccounts en regio's asynchroon kopiëren.

In het volgende voorbeeld ziet u hoe u blobs van één opslagaccount naar een ander kopieert. Eerst wordt er een container in het bronopslagaccount gemaakt en openbare leestoegang opgegeven voor de blobs. Daarna wordt er een bestand naar de container geüpload en wordt de blob vanuit die container naar een container in het doelopslagaccount gekopieerd.

```azurecli
# Create container in source account
az storage container create \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --name sourcecontainer \
    --public-access blob

# Upload blob to container in source account
az storage blob upload \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --container-name sourcecontainer \
    --file ~/Pictures/sourcefile.png \
    --name sourcefile.png

# Copy blob from source account to destination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

In het bovenstaande voorbeeld moet de doelcontainer al bestaan in de doel-opslagaccount voor de kopieerbewerking te voltooien. Bovendien moet de bron-blob die in het argument `--source-uri` is opgegeven, een SAS-token (Shared Access Signature) bevatten of openbaar toegankelijk zijn, zoals in dit voorbeeld.

### <a name="delete-a-blob"></a>Een blob verwijderen
Als u wilt verwijderen van een blob, gebruikt u de `blob delete` opdracht:

```azurecli
az storage blob delete --container-name <container_name> --name <blob_name>
```

## <a name="create-and-manage-file-shares"></a>Maken en beheren van bestandsshares
Azure Files biedt gedeelde opslag voor toepassingen die gebruikmaken van het protocol Server Message Block (SMB). Microsoft Azure virtuele machines en cloudservices, evenals on-premises toepassingen, kunnen bestandsgegevens via gekoppelde shares delen. U kunt bestandsshares en gegevens uit een bestand via de Azure CLI beheren. Zie voor meer informatie over Azure Files [Inleiding tot Azure Files](../files/storage-files-introduction.md).

### <a name="create-a-file-share"></a>Een bestandsshare maken
Een Azure-bestandsshare is een SMB-bestandsshare in Azure. Alle mappen en bestanden moeten worden gemaakt in een bestandsshare. Een account kan een onbeperkt aantal shares bevatten en een share kan een onbeperkt aantal bestanden, tot de capaciteitslimiet van het opslagaccount. Het volgende voorbeeld wordt een bestandsshare met de naam **myshare**.

```azurecli
az storage share create --name myshare
```

### <a name="create-a-directory"></a>Een map maken
Een directory biedt een hiërarchische structuur in een Azure-bestandsshare. Het volgende voorbeeld wordt een map met de naam **mijnmap** in de bestandsshare.

```azurecli
az storage directory create --name myDir --share-name myshare
```

Pad naar een map kan bijvoorbeeld meerdere niveaus bevatten **dir1/dir2**. Echter, moet u ervoor zorgen dat alle bovenliggende mappen bestaan voordat u een submap maakt. Bijvoorbeeld, voor het pad **dir1/dir2**, moet u eerst directory maken **dir1**, maakt u directory **dir2**.

### <a name="upload-a-local-file-to-a-share"></a>Een lokaal bestand te uploaden naar een share
Het volgende voorbeeld wordt een bestand geüpload **~/temp/samplefile.txt** op basis van de **myshare** -bestandsshare. De `--source` argument specificeert het bestaande lokale bestand te uploaden.

```azurecli
az storage file upload --share-name myshare --source ~/temp/samplefile.txt
```

Als met het maken van een directory, kunt u een pad in de share naar het bestand uploaden naar een bestaande map in de share opgeven:

```azurecli
az storage file upload --share-name myshare/myDir --source ~/temp/samplefile.txt
```

Een bestand in de share kan maximaal 1 TB groot zijn.

### <a name="list-the-files-in-a-share"></a>De bestanden in een share
U kunt bestanden en mappen in een share weergeven met behulp van de `az storage file list` opdracht:

```azurecli
# List the files in the root of a share
az storage file list --share-name myshare --output table

# List the files in a directory within a share
az storage file list --share-name myshare/myDir --output table

# List the files in a path within a share
az storage file list --share-name myshare --path myDir/mySubDir/MySubDir2 --output table
```

### <a name="copy-files"></a>Bestanden kopiëren      
U kunt een bestand kopiëren naar een ander bestand, een bestand naar een blob of een blob naar een bestand. Als u bijvoorbeeld een bestand kopiëren naar een map in een andere share:        
        
```azurecli
az storage file copy start \
--source-share share1 --source-path dir1/file.txt \
--destination-share share2 --destination-path dir2/file.txt     
```

## <a name="create-share-snapshot"></a>Share-momentopname maken
U kunt een momentopname van een share maken met behulp van de `az storage share snapshot` opdracht:

```cli
az storage share snapshot -n <share name>
```

Voorbeeld van uitvoer
```json
{
  "metadata": {},
  "name": "<share name>",
  "properties": {
    "etag": "\"0x8D50B7F9A8D7F30\"",
    "lastModified": "2017-10-04T23:28:22+00:00",
    "quota": null
  },
  "snapshot": "2017-10-04T23:28:35.0000000Z"
}
```

### <a name="list-share-snapshots"></a>Momentopnamen van shares opvragen

U kunt momentopnamen van shares van een bepaalde bestandsshare met aanbieden `az storage share list --include-snapshots`

```cli
az storage share list --include-snapshots
```

**Voorbeeld van uitvoer**
```json
[
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:44:13.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:45:18.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": null
  }
]
```

### <a name="browse-share-snapshots"></a>Bladeren door momentopnamen van shares
U mogelijk ook bladeren naar een bepaalde bestandsshare als u wilt weergeven van de inhoud met behulp van de momentopname `az storage file list`. Een heeft om op te geven van de sharenaam `--share-name <snare name>` en de tijdstempel `--snapshot '2017-10-04T19:45:18.0000000Z'`

```azurecli-interactive
az storage file list --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z' -otable
```

**Voorbeeld van uitvoer**
```
Name            Content Length    Type    Last Modified
--------------  ----------------  ------  ---------------
HelloWorldDir/                    dir
IMG_0966.JPG    533568            file
IMG_1105.JPG    717711            file
IMG_1341.JPG    608459            file
IMG_1405.JPG    652156            file
IMG_1611.JPG    442671            file
IMG_1634.JPG    1495999           file
IMG_1635.JPG    974058            file

```
### <a name="restore-from-share-snapshots"></a>Terugzetten van momentopnamen van shares

U kunt een bestand herstellen door te kopiëren of een bestand wordt gedownload van een share die momentopname over met `az storage file download` opdracht

```azurecli-interactive
az storage file download --path IMG_0966.JPG --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z'
```
**Voorbeeld van uitvoer**
```
{
  "content": null,
  "metadata": {},
  "name": "IMG_0966.JPG",
  "properties": {
    "contentLength": 533568,
    "contentRange": "bytes 0-533567/533568",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentType": "application/octet-stream"
    },
    "copy": {
      "completionTime": null,
      "id": null,
      "progress": null,
      "source": null,
      "status": null,
      "statusDescription": null
    },
    "etag": "\"0x8D50B5F49F7ACDF\"",
    "lastModified": "2017-10-04T19:37:03+00:00",
    "serverEncrypted": true
  }
}
```
## <a name="delete-share-snapshot"></a>Share-momentopname verwijderen
U kunt een momentopname van een share verwijderen met behulp van de `az storage share delete` opdracht door te geven `--snapshot` parameter met de tijdstempel van share-momentopname:

```cli
az storage share delete -n <share name> --snapshot '2017-10-04T23:28:35.0000000Z' 
```

Voorbeeld van uitvoer
```json
{
  "deleted": true
}
```

## <a name="next-steps"></a>Volgende stappen
Hier volgen enkele aanvullende resources voor meer informatie over het werken met de Azure CLI. 

* [Aan de slag met Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Overzicht van Azure CLI-opdrachten](/cli/azure)
* [Azure CLI op GitHub](https://github.com/Azure/azure-cli)
