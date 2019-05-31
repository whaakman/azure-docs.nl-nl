---
title: Het overdragen van gegevens van en naar Azure Blob-opslag met behulp van AzCopy v10 | Microsoft Docs
description: In dit artikel bevat een verzameling van AzCopy voorbeeld van de opdrachten die u helpen bij containers te maken, kopiëren van bestanden en mappen tussen lokale bestandssystemen en containers synchroniseren.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 98e33f838ee9b6f506bf1dc01e1dd61ad587aa05
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299395"
---
# <a name="transfer-data-with-azcopy-and-blob-storage"></a>Gegevens overdragen met AzCopy en Blob-opslag

AzCopy is een opdrachtregelprogramma die u gebruiken kunt om gegevens te kopiëren naar, uit of tussen opslagaccounts. In dit artikel bevat een van de voorbeeldopdrachten die met Blob-opslag werken.

## <a name="get-started"></a>Aan de slag

Zie de [aan de slag met AzCopy](storage-use-azcopy-v10.md) artikel AzCopy downloaden en meer informatie over de manieren waarop u autorisatiereferenties voor de storage-service kunt opgeven.

> [!NOTE]
> De voorbeelden in dit artikel wordt ervan uitgegaan dat u uw identiteit hebt geverifieerd met behulp van de `AzCopy login` opdracht. AzCopy gebruikt vervolgens de Azure AD-account om toegang tot gegevens in Blob-opslag te verlenen.
>
> Als u liever een SAS-token toegang verlenen aan blob-gegevens, kunt u dit token toevoegen aan de bron-URL in de AzCopy-opdracht.
>
> Bijvoorbeeld: `https://<storage-account-name>.blob.core.windows.net/<container-name>?<SAS-token>"`.

## <a name="create-a-container"></a>Een container maken

U kunt de AzCopy gebruiken `make` opdracht om een container te maken. De voorbeelden in deze sectie maakt u een container met de naam `mycontainer`.

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy make "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>` |
| **Voorbeeld** | `azcopy make "https://mystorageaccount.blob.core.windows.net/mycontainer"` |
| **Voorbeeld** (hiërarchische naamruimte) | `azcopy make "https://mystorageaccount.dfs.core.windows.net/mycontainer"` |

## <a name="upload-files"></a>Bestanden uploaden

U kunt de AzCopy gebruiken `copy` opdracht voor het uploaden van bestanden en mappen op uw lokale computer.

Deze sectie bevat de volgende voorbeelden:

> [!div class="checklist"]
> * Bestand uploaden
> * Map uploaden
> * Bestanden uploaden met jokertekens

> [!NOTE]
> AzCopy wordt niet automatisch berekenen en opslaan van het bestand md5-hash-code. Als u AzCopy om dat te doen wilt, voeg de `--put-md5` markeren voor elke kopieeropdracht. Op die manier als de blob is gedownload, AzCopy berekent een MD5-hash voor gegevens gedownloade en verifieert u dat de MD5-hash wordt opgeslagen in van de blob `Content-md5` eigenschap komt overeen met de berekende hash.

### <a name="upload-a-file"></a>Bestand uploaden

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy cp "<local-file-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>"` |
| **Voorbeeld** | `azcopy copy "C:\myFolder\myTextFile.txt" "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |
| **Voorbeeld** (hiërarchische naamruimte) | `azcopy copy "C:\myFolder\myTextFile.txt" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt"` |

> [!NOTE]
> AzCopy standaard worden gegevens geüpload naar blok-blobs. Bestanden uploaden als toevoeg-Blobs en pagina-Blobs gebruikt u de vlag `--blob-type=[BlockBlob|PageBlob|AppendBlob]`.

### <a name="upload-a-folder"></a>Map uploaden

In dit voorbeeld kopieert een map (en alle bestanden in die map) naar een blob-container. Het resultaat is een map in de container met dezelfde naam.

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy copy "<local-folder-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Voorbeeld** | `azcopy copy "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |
| **Voorbeeld** (hiërarchische naamruimte) | `azcopy copy "C:\myFolder" "https://mystorageaccount.dfs.core.windows.net/mycontainer" --recursive` |

Als u wilt kopiëren naar een map in de container, moet u alleen de naam van de map opgeven in uw opdrachttekenreeks.

|    |     |
|--------|-----------|
| **Voorbeeld** | `azcopy copy "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder" --recursive` |
| **Voorbeeld** (hiërarchische naamruimte) | `azcopy copy "C:\myFolder" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobFolder" --recursive` |

Als u de naam van een map die niet in de container bestaat opgeeft, wordt een nieuwe map door AzCopy gemaakt met die naam.

### <a name="upload-the-contents-of-a-folder"></a>De inhoud van een map uploaden

U kunt de inhoud van een map uploaden zonder te kopiëren die voor de map zelf met behulp van het jokerteken (*).

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy copy "<local-folder-path>\*" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<folder-path>` |
| **Voorbeeld** | `azcopy copy "C:\myFolder\*" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder"` |
| **Voorbeeld** (hiërarchische naamruimte) | `azcopy copy "C:\myFolder\*" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobFolder"` |

> [!NOTE]
> Toevoeg-de `--recursive` vlag voor het uploaden van bestanden in alle submappen.

## <a name="download-files"></a>Bestanden downloaden

U kunt de AzCopy gebruiken `copy` opdracht voor het downloaden van blobs, mappen en containers op uw lokale computer.

Deze sectie bevat de volgende voorbeelden:

> [!div class="checklist"]
> * Bestand downloaden
> * Een map downloaden
> * Bestanden downloaden met behulp van jokertekens

> [!NOTE]
> Als de `Content-md5` waarde van de eigenschap van een blob bevat een hash, AzCopy berekent een MD5-hash voor gedownloade gegevens en verifieert u dat de MD5-hash wordt opgeslagen in van de blob `Content-md5` eigenschap komt overeen met de berekende hash. Als deze waarden niet overeenkomen, het downloaden is mislukt, tenzij u dit gedrag door toe te voegen negeren `--check-md5=NoCheck` of `--check-md5=LogOnly` aan de kopieeropdracht.

### <a name="download-a-file"></a>Bestand downloaden

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>" "<local-file-path>"` |
| **Voorbeeld** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt" "C:\myFolder\myTextFile.txt"` |
| **Voorbeeld** (hiërarchische naamruimte) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt" "C:\myFolder\myTextFile.txt"` |

### <a name="download-a-folder"></a>Een map downloaden

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<folder-path>" "<local-folder-path>" --recursive` |
| **Voorbeeld** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder "C:\myFolder"  --recursive` |
| **Voorbeeld** (hiërarchische naamruimte) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobFolder "C:\myFolder"  --recursive` |

In dit voorbeeld resulteert in een map met de naam `C:\myFolder\myBlobFolder` die alle gedownloade bestanden bevat.

### <a name="download-the-contents-of-a-folder"></a>De inhoud van een map downloaden

U kunt de inhoud van een map downloaden zonder te kopiëren die voor de map zelf met behulp van het jokerteken (*).

> [!NOTE]
> Op dit moment wordt in dit scenario alleen ondersteund voor accounts waarvoor geen een hiërarchische naamruimte.

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy copy "https://<storage-account-name>.blob.core.windows.net/<container-name>/*" "<local-folder-path>/"` |
| **Voorbeeld** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder/*" "C:\myFolder"` |

> [!NOTE]
> Toevoeg-de `--recursive` vlag voor het downloaden van bestanden in alle submappen.

## <a name="copy-blobs-between-storage-accounts"></a>Blobs tussen opslagaccounts kopiëren

U kunt AzCopy gebruiken om te kopiëren van blobs naar andere storage-accounts. De kopieerbewerking is synchroon, zodat wanneer de opdracht retourneert, die aangeeft dat alle bestanden zijn gekopieerd.

> [!NOTE]
> Op dit moment wordt in dit scenario alleen ondersteund voor accounts waarvoor geen een hiërarchische naamruimte.

AzCopy gebruikt de [blok plaatsen van URL](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) API, zodat gegevens rechtstreeks tussen opslagservers worden gekopieerd. Deze kopieerbewerkingen gebruik niet de bandbreedte van het netwerk van uw computer.

Deze sectie bevat de volgende voorbeelden:

> [!div class="checklist"]
> * Een blob kopieert naar een ander opslagaccount
> * Een map te kopiëren naar een ander opslagaccount
> * Een containers kopiëren naar een ander opslagaccount
> * Alle containers, mappen en bestanden kopiëren naar een ander opslagaccount

### <a name="copy-a-blob-to-another-storage-account"></a>Een blob kopieert naar een ander opslagaccount

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>"` |
| **Voorbeeld** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt" "https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |

### <a name="copy-a-folder-to-another-storage-account"></a>Een map te kopiëren naar een ander opslagaccount

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<folder-path>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<folder-path>" --recursive` |
| **Voorbeeld** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobFolder" "https://mydestinationaccount.blob.core.windows.net/mycontainer/myBlobFolder" --recursive` |

### <a name="copy-a-containers-to-another-storage-account"></a>Een containers kopiëren naar een ander opslagaccount

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>" --recursive` |
| **Voorbeeld** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer" "https://mydestinationaccount.blob.core.windows.net/mycontainer" --recursive` |

### <a name="copy-all-containers-folders-and-files-to-another-storage-account"></a>Alle containers, mappen en bestanden kopiëren naar een ander opslagaccount

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/" "https://<destination-storage-account-name>.blob.core.windows.net/" --recursive"` |
| **Voorbeeld** | `azcopy cp "https://mysourceaccount.blob.core.windows.net" "https://mydestinationaccount.blob.core.windows.net" --recursive` |

## <a name="synchronize-files"></a>Synchroniseren van bestanden

U kunt de inhoud van een lokaal bestandssysteem voor een blob-container kunt synchroniseren. U kunt ook een blob-container naar een lokale bestandssysteem synchroniseren op uw computer. Synchronisatie kan niet worden teruggedraaid. Met andere woorden, kiest u welke van deze twee eindpunten de bron is en wat is de bestemming.

> [!NOTE]
> De huidige versie van AzCopy wordt niet gesynchroniseerd tussen de andere bronnen en bestemmingen (bijvoorbeeld: Amazon Web Services (AWS) S3 buckets of bestandsopslag).

De `sync` opdracht vergelijkt bestandsnamen en tijdstempels het laatst is gewijzigd. Stel de `--delete-destination` optionele vlag aan een waarde van `true` of `prompt` bestanden in de doelmap worden verwijderd als deze bestanden niet meer in de bronmap bestaat.

Als u de `--delete-destination` markering `true` AzCopy bestanden worden verwijderd zonder op te geven van de opdrachtprompt. Als u een prompt om te worden weergegeven voor het AzCopy-verwijdert een bestand wilt, stelt u de `--delete-destination` markering `prompt`.

> [!NOTE]
> Om te voorkomen dat onbedoelde verwijderingen, zorg ervoor dat u inschakelen de [voorlopig verwijderen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) functie voordat u de `--delete-destination=prompt|true` vlag.

### <a name="synchronize-a-container-to-a-local-file-system"></a>Synchroniseren van een container voor een lokaal bestandssysteem

In dit geval wordt het lokale bestandssysteem wordt de bron en de container is de bestemming.

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy sync "<local-folder-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Voorbeeld** | `azcopy sync "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |
| **Voorbeeld** (hiërarchische naamruimte) | `azcopy sync "C:\myFolder" "https://<storage-account-name>.dfs.core.windows.net/mycontainer" --recursive` |


### <a name="synchronize-a-local-file-system-to-a-container"></a>Synchroniseren van een lokaal bestandssysteem voor een container

In dit geval wordt de container wordt de bron en het lokale bestandssysteem is de bestemming.

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy sync "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" "C:\myFolder" --recursive` |
| **Voorbeeld** | `azcopy sync "https://mystorageaccount.blob.core.windows.net/mycontainer" "C:\myFolder" --recursive` |
| **Voorbeeld** (hiërarchische naamruimte) | `azcopy sync "https://mystorageaccount.dfs.core.windows.net/mycontainer" "C:\myFolder" --recursive` |

## <a name="next-steps"></a>Volgende stappen

Meer voorbeelden niet vinden in een van deze artikelen:

- [Aan de slag met AzCopy](storage-use-azcopy-v10.md)

- [Zelfstudie: Migreren van on-premises gegevens naar cloudopslag met behulp van AzCopy](storage-use-azcopy-migrate-on-premises-data.md)

- [Gegevens overdragen met AzCopy en bestandsopslag](storage-use-azcopy-files.md)

- [Gegevens overdragen met AzCopy en Amazon S3 buckets](storage-use-azcopy-s3.md)

- [Configureer, optimaliseren en oplossen van AzCopy](storage-use-azcopy-configure.md)