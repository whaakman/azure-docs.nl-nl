---
title: Bewerkingen uitvoeren op Azure Blob-opslag (objectopslag) met Azure CLI | Microsoft Docs
description: Informatie over het uploaden en downloaden van blobs in Azure Blob-opslag, en het maken van een Shared Access Signature (SAS) om toegang tot een blob in uw opslagaccount te beheren.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/15/2017
ms.author: tamram
ms.openlocfilehash: bd96cf7eb1c0c7f51b110da848a8df7914ad85c7
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="perform-blob-storage-operations-with-azure-cli"></a>Blob-opslagbewerkingen uitvoeren met Azure CLI

Azure Blob Storage is een service voor het opslaan van grote hoeveelheden ongestructureerde objectgegevens, zoals tekst of binaire gegevens, die via HTTP of HTTPS overal vandaan kunnen worden opgevraagd. In deze zelfstudie worden de basisbewerkingen in Azure Blob-opslag behandeld, zoals het uploaden, downloaden en verwijderen van blobs. Procedures voor:

> [!div class="checklist"]
> * Een container maken
> * Een bestand (blob) uploaden naar een container
> * De blobs in een container in een lijst weergeven
> * Een blob downloaden uit een container
> * Een blob kopiëren tussen opslagaccounts
> * Een blob verwijderen
> * Blob-eigenschappen en metagegevens weergeven en wijzigen
> * Beveiliging beheren met een Shared Access Signature (SAS)

Voor deze zelfstudie is versie 2.0.4 of hoger van Azure CLI vereist. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Een container maken

Containers zijn vergelijkbaar met mappen op uw computer. In een container kunt u groepen blobs organiseren zoals u bestanden in een map organiseert. Een opslagaccount kan een onbeperkt aantal containers bevatten. U kunt maximaal 500 TB aan blob-gegevens opslaan in een container. Dit is de maximale hoeveelheid gegevens in een opslagaccount.

Gebruik de opdracht [az storage container create](/cli/azure/storage/container#create) om een container te maken voor het opslaan van blobs.

```azurecli-interactive
az storage container create --name mystoragecontainer
```

Containernamen moeten beginnen met een letter of cijfer en mogen alleen letters, cijfers en het koppelteken (-) bevatten. Zie [Naming and referencing containers, blobs, and metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Containers, blobs en metagegevens een naam geven en hiernaar verwijzen) voor meer regels voor de naamgeving van blobs en containers.

## <a name="enable-public-read-access-for-a-container"></a>Openbare leestoegang voor een container inschakelen

Een nieuwe container is standaard persoonlijk. Dat wil zeggen dat niemand toegang heeft tot de container zonder een [Shared Access Signature](#create-a-shared-access-signature-sas) of de toegangssleutels voor het opslagaccount. Met Azure CLI kunt u dit gedrag wijzigen door containermachtigingen in te stellen op een van de drie niveaus:

| | |
|---|---|
| `--public-access off` | (Standaard) Geen openbare leestoegang |
| `--public-access blob` | Openbare leestoegang alleen voor blobs |
| `--public-access container` | Met openbare leestoegang voor blobs kunnen blobs in een container worden weergegeven |

Als u openbare toegang instelt op `blob` of `container`, schakelt u alleen-lezen toegang in voor iedereen op internet. Zo moet u openbare leestoegang inschakelen als u afbeeldingen wilt weergeven die als blobs op uw website zijn opgeslagen. Als u lees-/schrijftoegang wilt inschakelen, moet u in plaats daarvan een [Shared Access Signature (SAS)](#create-a-shared-access-signature-sas) gebruiken.

Gebruik de opdracht [az storage container set-permission](/cli/azure/storage/container#create) om openbare leestoegang voor de container in te schakelen.

```azurecli-interactive
az storage container set-permission \
    --name mystoragecontainer \
    --public-access container
```

## <a name="upload-a-blob-to-a-container"></a>Een blob uploaden naar een container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. Blok-blobs zijn het meest voorkomende type blobs dat in Azure Storage wordt opgeslagen. Toevoeg-blobs worden gebruikt wanneer gegevens moeten worden toegevoegd aan een bestaande blob zonder de bestaande inhoud te wijzigen, zoals in het geval van logboekregistratie. Pagina-blobs ondersteunen de VHD-bestanden van virtuele IaaS-machines.

In dit voorbeeld wordt met de opdracht [az storage blob upload](/cli/azure/storage/blob#upload) een blob geüpload naar de container die we in de laatste stap hebben gemaakt.

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Met deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of overschreven als dat wel het geval is. Upload een aantal bestanden, zodat u meerdere vermeldingen ziet wanneer u de blobs in de volgende stap weergeeft.

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Gebruik de opdracht [az storage blob list](/cli/azure/storage/blob#list) om de blobs in de container weer te geven.

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

Voorbeelduitvoer:

```
Name            Blob Type      Length  Content Type              Last Modified
--------------  -----------  --------  ------------------------  -------------------------
ISSUE_TEMPLATE  BlockBlob         761  application/octet-stream  2017-04-21T18:29:50+00:00
LICENSE         BlockBlob       18653  application/octet-stream  2017-04-21T18:28:50+00:00
LICENSE-CODE    BlockBlob        1090  application/octet-stream  2017-04-21T18:29:02+00:00
README.md       BlockBlob        6700  application/octet-stream  2017-04-21T18:27:33+00:00
dir1/file1.txt  BlockBlob        6700  application/octet-stream  2017-04-21T18:32:51+00:00
```

## <a name="download-a-blob"></a>Een blob downloaden

Gebruik de opdracht [az storage blob download](/cli/azure/storage/blob#download) om de blob te downloaden die u in een eerdere stap hebt geüpload.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="copy-a-blob-between-storage-accounts"></a>Een blob kopiëren tussen opslagaccounts

U kunt blobs binnen of tussen opslagaccounts en regio's asynchroon kopiëren.

In het volgende voorbeeld ziet u hoe u blobs van één opslagaccount naar een ander kopieert. Eerst wordt er een container in het bronopslagaccount gemaakt en openbare leestoegang opgegeven voor de blobs. Daarna wordt er een bestand naar de container geüpload en wordt de blob vanuit die container naar een container in het doelopslagaccount gekopieerd.

In dit voorbeeld slaagt de kopieerbewerking alleen als de doelcontainer al in het opslagaccount van de bestemming bestaat. Bovendien moet de bron-blob die in het argument `--source-uri` is opgegeven, een SAS-token (Shared Access Signature) bevatten of openbaar toegankelijk zijn, zoals in dit voorbeeld.

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
    --file ~/path/to/sourcefile \
    --name sourcefile

# Copy blob from source account to destination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

## <a name="delete-a-blob"></a>Een blob verwijderen

U verwijdert een blob uit een container met de opdracht [az storage blob delete](/cli/azure/storage/blob#delete).

```azurecli-interactive
az storage blob delete \
    --container-name mystoragecontainer \
    --name blobName
```

## <a name="set-the-content-type"></a>Type bestandsinhoud instellen

Het type inhoud, ook wel het MIME-type genoemd, geeft de indeling aan van de gegevens in de blob. Browsers en andere software maken gebruik van het type inhoud om te bepalen hoe de gegevens verwerkt moeten worden. In het volgende voorbeeld wordt het type inhoud op `image/png` ingesteld.

```azurecli-interactive
# Set the content type
az storage blob update
    --container-name mystoragecontainer 
    --name blobName 
    --content-type image/png
```

## <a name="display-and-modify-blob-properties-and-metadata"></a>Blob-eigenschappen en metagegevens weergeven en wijzigen

Elke blob heeft een aantal door de service gedefinieerde eigenschappen die u kunt weergeven met de opdracht [az storage blob show](/cli/azure/storage/blob#show), waaronder de naam, het type, de lengte enzovoort. Met de opdracht [az storage blob metadata update](/cli/azure/storage/blob/metadata#update) kunt u ook een blob met uw eigen eigenschappen en waarden configureren.

In dit voorbeeld geven we eerst de door de service gedefinieerde eigenschappen van een blob weer, waarna we de blob bijwerken met twee van onze eigen metagegevenseigenschappen. Tot slot geven we de metagegevenseigenschappen van de blob en de waarden ervan weer met de opdracht [az storage blob metadata show](/cli/azure/storage/blob/metadata#show).

```azurecli-interactive
# Show properties of a blob
az storage blob show \
    --container-name mystoragecontainer \
    --name blobName \
    --output table

# Set metadata properties of a blob (replaces all existing metadata)
az storage blob metadata update \
    --container-name mystoragecontainer \
    --name blobName \
    --metadata "key1=value1" "key2=value2"

# Show metadata properties of a blob
az storage blob metadata show \
    --container-name mystoragecontainer \
    --name blobName
```

## <a name="create-a-shared-access-signature-sas"></a>Een Shared Access Signature (SAS) maken

Shared Access Signatures (SAS) vormen een manier om beperkte toegang te verlenen tot objecten in uw opslagaccount zonder de toegangssleutels van uw opslagaccount bekend te maken. Als u een URI wilt genereren die toegang toestaat tot een persoonlijke resource, maakt u een SAS-token met de gewenste machtigingen en geldigheidsperiode (de effectieve duur) en voegt u deze als een queryreeks toe aan de URL van een resource. Zo wordt de volledige SAS-URI gevormd. Iedereen die over deze SAS-URI (de bron-URL plus het SAS-token) beschikt, heeft gedurende de geldigheidsperiode van het SAS-token toegang tot de resource. Zo zou u iemand twee minuten leestoegang kunnen toestaan tot een persoonlijke blob, om deze te bekijken.

In de volgende stappen schakelt u openbare toegang tot de container uit, test u de 'alleen persoonlijke' toegang en genereert en test u een SAS-URI.

### <a name="disable-container-public-access"></a>Openbare toegang tot de container uitschakelen

Stel eerst het toegangsniveau van de container in op `off`. Hiermee geeft u aan dat de container en blobs alleen toegankelijk zijn met een Shared Access Signature of een toegangssleutel voor het opslagaccount.

```azurecli-interactive
az storage container set-permission \
    --name mystoragecontainer \
    --public-access off
```

### <a name="verify-private-access"></a>Persoonlijke toegang controleren

Als u wilt controleren of er geen openbare leestoegang is tot de blobs in die container, gebruikt u de opdracht [az storage blob url](/cli/azure/storage/blob#url) om de URL voor een van de blobs te verkrijgen.

```azurecli-interactive
az storage blob url \
    --container-name mystoragecontainer \
    --name blobName \
    --output tsv
```

Navigeer in een persoonlijk browservenster naar de URL van de blob. Als het goed is, ziet u een `ResourceNotFound`-fout omdat de blob persoonlijk is en u geen Shared Access Signature hebt opgenomen.

### <a name="create-a-sas-uri"></a>Een SAS-URI maken

Nu gaat u een SAS-URI maken die toegang geeft tot de blob. In het volgende voorbeeld wordt eerst de opdracht [az storage blob url](/cli/azure/storage/blob#url) gegeven om een variabele te vullen met de URL voor de blob. Vervolgens wordt een andere variabele gevuld met een SAS-token dat is gegenereerd met de opdracht [az storage blob generate-sas](/cli/azure/storage/blob#generate-sas). Tot slot wordt de volledige SAS-URI voor de blob gegenereerd door de twee samen te voegen, gescheiden door het scheidingsteken voor query-tekenreeksen `?`.

```azurecli-interactive
# Get UTC datetimes for SAS start and expiry (Example: 1994-11-05T13:15:30Z)
sas_start=`date -u +'%Y-%m-%dT%H:%M:%SZ'`
sas_expiry=`date -u +'%Y-%m-%dT%H:%M:%SZ' -d '+2 minute'`

# Get the URL for the blob
blob_url=`az storage blob url \
    --container-name mystoragecontainer \
    --name blobName \
    --output tsv`

# Obtain a SAS token granting read (r) access between the
# SAS start and expiry times
sas_token=`az storage blob generate-sas \
    --container-name mystoragecontainer \
    --name blobName \
    --start $sas_start \
    --expiry $sas_expiry \
    --permissions r \
    --output tsv`

# Display the full SAS URI for the blob
echo $blob_url?$sas_token
```

### <a name="test-the-sas-uri"></a>De SAS-URI testen

Navigeer in een persoonlijk browservenster naar de volledige SAS-URI, die u in het bovenstaande codefragment hebt weergegeven met de opdracht `echo`. Nu wordt er geen fout weergegeven en kunt u de blob weergeven of downloaden.

Wacht tot de URL is verlopen (twee minuten in dit voorbeeld) en navigeer vervolgens in een ander persoonlijk browservenster naar de URI. Nu wordt de fout `AuthenticationFailed` weergegeven omdat het SAS-token is verlopen.

## <a name="clean-up-resources"></a>Resources opschonen

Als u de resources van de resourcegroep niet meer nodig hebt, met inbegrip van het opslagaccount dat u hebt gemaakt en alle bestaande blobs die u in deze zelfstudie hebt geüpload, verwijdert u de resourcegroep met de opdracht [az group delete](/cli/azure/group#delete).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u de basisbeginselen geleerd van het werken met blobs in Azure Storage:

> [!div class="checklist"]
> * Een container maken
> * Een bestand (blob) uploaden naar een container
> * De blobs in een container in een lijst weergeven
> * Een blob downloaden uit een container
> * Een blob kopiëren tussen opslagaccounts
> * Een blob verwijderen
> * Blob-eigenschappen en metagegevens weergeven en wijzigen
> * Beveiliging beheren met een Shared Access Signature (SAS)

De volgende bronnen bevatten aanvullende informatie over het werken met Azure CLI en het werken met de resources in uw opslagaccount.

* Azure-CLI
  * [Aanmelden bij Azure CLI 2.0](/cli/azure/authenticate-azure-cli): meer informatie over de verschillende methoden voor verificatie met de CLI, met inbegrip van niet-interactieve aanmelding via [service-principal](/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal) voor het uitvoeren van Azure CLI-scripts zonder toezicht.
  * [Azure CLI 2.0-opdrachtenreferentie](/cli/azure/)
* Microsoft Azure Storage Explorer
  * [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.
