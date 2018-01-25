---
title: De Azure CLI 2.0 gebruiken met Azure Storage | Microsoft Docs
description: Informatie over het gebruik van de Azure-opdrachtregelinterface (Azure CLI) 2.0 met Azure Storage te maken en beheren van storage-accounts en werken met Azure BLOB's en bestanden. De Azure CLI 2.0 is een cross-platform-hulpprogramma dat is geschreven in Python.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 06/02/2017
ms.author: tamram
ms.openlocfilehash: 34780001afb309a2986cc21dae948d9d94f1a63f
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="using-the-azure-cli-20-with-azure-storage"></a>De Azure CLI 2.0 gebruiken met Azure Storage

De open source, platformoverschrijdende Azure CLI 2.0 biedt een reeks opdrachten voor het werken met de Azure-platform. Het biedt veel van dezelfde functionaliteit gevonden in de [Azure-portal](https://portal.azure.com), met inbegrip van toegang tot uitgebreide gegevens.

In deze handleiding wordt beschreven hoe u gebruikt de [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) verschillende taken die werken met resources in uw Azure Storage-account uit te voeren. U wordt aangeraden dat u downloaden en installeren of naar de nieuwste versie van de CLI 2.0 upgraden voordat u deze handleiding.

De voorbeelden in deze handleiding wordt ervan uitgegaan dat het gebruik van de Bash-shell op Ubuntu, maar andere platforms op dezelfde manier moeten uitvoeren. 

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="prerequisites"></a>Vereisten
Deze handleiding wordt ervan uitgegaan dat u de basisconcepten van Azure Storage begrijpt. Ook wordt ervan uitgegaan dat u kunnen voldoen aan de vereisten voor het maken van account die bent hieronder zijn opgegeven voor Azure en de Storage-service.

### <a name="accounts"></a>Accounts
* **Azure-account**: als u nog een Azure-abonnement hebt [maken van een gratis Azure-account](https://azure.microsoft.com/free/).
* **Opslagaccount**: zie [Een opslagaccount maken](storage-create-storage-account.md#create-a-storage-account) in [Over Azure-opslagaccounts](storage-create-storage-account.md).

### <a name="install-the-azure-cli-20"></a>Azure CLI 2.0 installeren

Download en installeer de Azure CLI 2.0 door de instructies die worden beschreven in [2.0 voor Azure CLI installeren](/cli/azure/install-az-cli2).

> [!TIP]
> Als u problemen met de installatie ondervindt, bekijk de [installatie probleemoplossing](/cli/azure/install-az-cli2#installation-troubleshooting) sectie van het artikel en de [installeren probleemoplossing](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) handleiding op GitHub.
>

## <a name="working-with-the-cli"></a>Werken met de CLI

Nadat u de CLI hebt geïnstalleerd, kunt u de `az` opdracht in de opdrachtregelinterface (Bash, Terminal, opdrachtprompt) voor toegang tot de Azure CLI-opdrachten. Typ de `az` opdracht voor een volledig overzicht van de basis-opdrachten (de volgende voorbeelduitvoer afgekapt):

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

Voer de opdracht uit in de opdrachtregelinterface `az storage --help` aan lijst met de `storage` opdracht subgroepen. De beschrijvingen van de subgroepen bevatten een overzicht van de functionaliteit van die de Azure CLI wordt verstrekt voor het werken met uw opslagresources.

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

## <a name="connect-the-cli-to-your-azure-subscription"></a>De CLI verbinding met uw Azure-abonnement

Om te werken met de resources in uw Azure-abonnement, u moet eerst aanmelden bij uw Azure-account met `az login`. Er zijn verschillende manieren die kunt u zich aanmeldt:

* **Interactieve aanmelding**:`az login`
* **Meld u aan met de gebruikersnaam en wachtwoord**:`az login -u johndoe@contoso.com -p VerySecret`
  * Dit werkt niet met Microsoft-accounts of -mailaccounts die gebruikmaken van multi-factor authentication-server.
* **Meld u aan met een service-principal**:`az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`

## <a name="azure-cli-20-sample-script"></a>Azure CLI 2.0-voorbeeldscript

Vervolgens werkt we met een kleine shellscript dat problemen van enkele elementaire 2.0 voor Azure CLI-opdrachten om te communiceren met een Azure Storage-resources. Het script eerst maakt een nieuwe container in uw opslagaccount en uploadt een bestaand bestand (als een blob) op die container. Deze vervolgens een lijst met alle blobs in de container en ten slotte wordt het bestand wordt gedownload naar een bestemming op uw lokale computer die u opgeeft.

```bash
#!/bin/bash
# A simple Azure Storage example script

export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

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

1. Uw favoriete teksteditor openen en vervolgens kopieert en plakt u dit script in de editor.

2. Werk vervolgens de scriptvariabelen om uw configuratie-instellingen weer te geven. Vervang de volgende waarden zoals opgegeven:

   * **\<storage_account_name\>**  de naam van uw opslagaccount.
   * **\<storage_account_key\>**  de primaire of secundaire toegangssleutel voor uw opslagaccount.
   * **\<container_name\>**  A Noem de nieuwe container te maken, zoals "azure-cli-voorbeeld-container".
   * **\<blob_name\>**  een naam voor de bestemmings-blob in de container.
   * **\<file_to_upload\>**  het pad naar een klein bestand op uw lokale computer, zoals ' ~ / images/HelloWorld.png '.
   * **\<destination_file\>**  het doelbestand pad, zoals ' ~ / downloadedImage.png '.

3. Nadat u de benodigde variabelen hebt bijgewerkt, sla het script op en sluit de editor af. De volgende stappen wordt ervan uitgegaan dat u hebt uw script genoemd **my_storage_sample.sh**.

4. Markeer het script als uitvoerbare, indien nodig:`chmod +x my_storage_sample.sh`

5. Voer het script. Bijvoorbeeld in Bash:`./my_storage_sample.sh`

Ziet u uitvoer die vergelijkbaar is met het volgende, en de  **\<destination_file\>**  u hebt opgegeven in het script moet worden weergegeven op de lokale computer.

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
> De voorgaande uitvoer is in **tabel** indeling. U kunt opgeven dat uitvoer indeling moet worden gebruikt door te geven de `--output` argument in de CLI-opdrachten of stel deze globaal met `az configure`.
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

* `--location`[Vereist]: locatie. Bijvoorbeeld "VS-West'.
* `--name`[Vereist]: naam van het opslagaccount. De naam moet 3 tot 24 tekens lang zijn, en gebruik alleen kleine letters alfanumerieke tekens.
* `--resource-group`[Vereist]: naam van resourcegroep.
* `--sku`[Vereist]: de SKU-opslagaccount. Toegestane waarden:
  * `Premium_LRS`
  * `Standard_GRS`
  * `Standard_LRS`
  * `Standard_RAGRS`
  * `Standard_ZRS`
```

### Set default Azure storage account environment variables
You can have multiple storage accounts in your Azure subscription. To select one of them to use for all subsequent storage commands, you can set these environment variables:

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Er is een andere manier om een standaardopslagaccount met behulp van een verbindingsreeks. Eerst ophalen van de verbindingsreeks met het `show-connection-string` opdracht:

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
> Alle voorbeelden in de volgende secties van dit artikel wordt ervan uitgegaan dat u hebt ingesteld de `AZURE_STORAGE_ACCOUNT` en `AZURE_STORAGE_ACCESS_KEY` omgevingsvariabelen.
>

## <a name="create-and-manage-blobs"></a>Maken en blobs beheren
Azure Blob storage is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens, die toegankelijk zijn vanuit overal ter wereld via HTTP of HTTPS. Deze sectie wordt ervan uitgegaan dat u al bekend met concepten voor Azure Blob-opslag bent. Zie voor gedetailleerde informatie [aan de slag met Azure Blob storage met .NET](../blobs/storage-dotnet-how-to-use-blobs.md) en [concepten van Blob-Service](/rest/api/storageservices/blob-service-concepts).

### <a name="create-a-container"></a>Een container maken
Elke blob in Azure-opslag moet zich in een container. U kunt een container maken met behulp van de `az storage container create` opdracht:

```azurecli
az storage container create --name <container_name>
```

U kunt een van drie niveaus van leestoegang voor een nieuwe container instellen door te geven de optionele `--public-access` argument:

* `off`(standaard): gegevens van de Container privé eigenaar van het account is.
* `blob`: Openbare leestoegang voor blobs.
* `container`: Openbare lees- en toegang tot de volledige container.

Zie voor meer informatie [anonieme leestoegang tot containers en blobs beheren](../blobs/storage-manage-access-to-resources.md).

### <a name="upload-a-blob-to-a-container"></a>Een blob uploaden naar een container
Azure Blob storage ondersteunt blok, toevoegen en pagina-blobs. BLOB's uploaden naar een container met behulp van de `blob upload` opdracht:

```azurecli
az storage blob upload \
    --file <local_file_path> \
    --container-name <container_name> \
    --name <blob_name>
```

 Standaard de `blob upload` opdracht *.vhd bestanden uploadt naar de pagina-blobs of anderszins blok-blobs. Als u wilt een ander type opgeven wanneer u een blob uploadt, kunt u de `--type` argument--toegestane waarden zijn `append`, `block`, en `page`.

 Zie voor meer informatie over de typen andere blob- [blok-Blobs, toevoeg-Blobs en pagina-Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).


### <a name="download-a-blob-from-a-container"></a>Een blob downloaden uit een container
In dit voorbeeld wordt getoond hoe een blob downloaden uit een container:

```azurecli
az storage blob download \
    --container-name mycontainer \
    --name myblob.png \
    --file ~/mydownloadedblob.png
```

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Lijst van de blobs in een container met de [lijst met blob storage az](/cli/azure/storage/blob#list) opdracht.

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

In het bovenstaande voorbeeld worden de doelcontainer moet al bestaan in de storage-account van de bestemming voor de kopieerbewerking is mislukt. Bovendien moet de bron-blob die in het argument `--source-uri` is opgegeven, een SAS-token (Shared Access Signature) bevatten of openbaar toegankelijk zijn, zoals in dit voorbeeld.

### <a name="delete-a-blob"></a>Een blob verwijderen
Als u wilt verwijderen van een blob, gebruiken de `blob delete` opdracht:

```azurecli
az storage blob delete --container-name <container_name> --name <blob_name>
```

## <a name="create-and-manage-file-shares"></a>Maken en beheren van bestandsshares
Azure Files biedt gedeelde opslag voor toepassingen die gebruikmaken van het protocol Server Message Block (SMB). Microsoft Azure virtuele machines en cloud-services, evenals de on-premises toepassingen kunnen bestandsgegevens via gekoppelde shares delen. U kunt bestandsshares en bestandsgegevens via de Azure CLI kunt beheren. Zie voor meer informatie over Azure-bestanden [Inleiding tot Azure-bestanden](../files/storage-files-introduction.md).

### <a name="create-a-file-share"></a>Een bestandsshare maken
Een Azure-bestandsshare is een SMB-bestandsshare in Azure. Alle mappen en bestanden moeten worden gemaakt in een bestandsshare. Een account kan een onbeperkt aantal shares bevatten en een bestandsshare kan een onbeperkt aantal bestanden, tot de capaciteitslimiet van het opslagaccount opslaan. Het volgende voorbeeld wordt een bestandsshare met de naam **mijnshare**.

```azurecli
az storage share create --name myshare
```

### <a name="create-a-directory"></a>Een map maken
Een directory biedt een hiërarchische structuur in een Azure-bestandsshare. Het volgende voorbeeld wordt een map met de naam **myDir** in de bestandsshare.

```azurecli
az storage directory create --name myDir --share-name myshare
```

Een pad kan bijvoorbeeld meerdere niveaus bevatten **dir1/dir2**. Echter, moet u ervoor zorgen dat alle bovenliggende mappen bestaan voordat u een submap maakt. Bijvoorbeeld: voor pad **dir1/dir2**, moet u eerst directory maken **dir1**, maak vervolgens de directory **dir2**.

### <a name="upload-a-local-file-to-a-share"></a>Een lokaal bestand uploaden naar een share
Het volgende voorbeeld wordt een bestand uit geüpload **~/temp/samplefile.txt** aan hoofdmap van de **mijnshare** bestandsshare. De `--source` argument Hiermee geeft u het bestaande lokale bestand te uploaden.

```azurecli
az storage file upload --share-name myshare --source ~/temp/samplefile.txt
```

Als met maken van de directory, kunt u een pad in de share het bestand te uploaden naar een bestaande map in de share opgeven:

```azurecli
az storage file upload --share-name myshare/myDir --source ~/temp/samplefile.txt
```

Een bestand in de share mag maximaal 1 TB groot zijn.

### <a name="list-the-files-in-a-share"></a>Lijst van de bestanden in een share
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
U kunt een bestand kopiëren naar een ander bestand, een bestand naar een blob of een blob naar een bestand. Als u bijvoorbeeld een bestand te kopiëren naar een map in een andere share:        
        
```azurecli
az storage file copy start \
--source-share share1 --source-path dir1/file.txt \
--destination-share share2 --destination-path dir2/file.txt     
```

## <a name="create-share-snapshot"></a>Share momentopname maken
U kunt een momentopname van een share maken met behulp van de `az storage share snapshot` opdracht:

```cli
az storage share snapshot -n <share name>
```

Voorbeelduitvoer
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

### <a name="list-share-snapshots"></a>Lijst share momentopnamen

Je kunt aanbieden share momentopnamen van het gebruik van een bepaalde bestandsshare`az storage share list --include-snapshots`

```cli
az storage share list --include-snapshots
```

**Voorbeelduitvoer**
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

### <a name="browse-share-snapshots"></a>Momentopnamen van de share bladeren
U kunt ook bekijken in een bepaalde bestandsshare om weer te geven van de inhoud met behulp van een momentopname `az storage file list`. Een heeft om op te geven van de sharenaam `--share-name <snare name>` en de timestamp`--snapshot '2017-10-04T19:45:18.0000000Z'`

```azurecli-interactive
az storage file list --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z' -otable
```

**Voorbeelduitvoer**
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
### <a name="restore-from-share-snapshots"></a>Terugzetten van momentopnamen van de share

U kunt een bestand herstellen door te kopiëren of downloaden van een bestand van een share die momentopname over met `az storage file download` opdracht

```azurecli-interactive
az storage file download --path IMG_0966.JPG --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z'
```
**Voorbeelduitvoer**
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
## <a name="delete-share-snapshot"></a>Share momentopname verwijderen
U kunt een momentopname van een share verwijderen met behulp van de `az storage share delete` opdracht doordat `--snapshot` parameter met de share momentopname tijdstempel:

```cli
az storage share delete -n <share name> --snapshot '2017-10-04T23:28:35.0000000Z' 
```

Voorbeelduitvoer
```json
{
  "deleted": true
}
```

## <a name="next-steps"></a>Volgende stappen
Hier volgen enkele aanvullende resources voor meer informatie over het werken met de Azure CLI 2.0.

* [Aan de slag met Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Azure CLI 2.0-opdrachtenreferentie](/cli/azure)
* [Azure CLI 2.0 op GitHub](https://github.com/Azure/azure-cli)
