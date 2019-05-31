---
title: Het overbrengen van gegevens naar of van Azure Files met behulp van AzCopy v10 | Microsoft Docs
description: Gegevens overdragen met AzCopy en file storage.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 5d11d5c0da350b9abf2e2b06a11a1690cf3f6922
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247107"
---
# <a name="transfer-data-with-azcopy-and-file-storage"></a>Gegevens overdragen met AzCopy en bestandsopslag 

AzCopy is een opdrachtregelprogramma waarmee u kunt blobs of bestanden kopiëren naar of van een storage-account. In dit artikel bevat een van de voorbeeldopdrachten die met Azure Files werken.

Voordat u begint, Zie de [aan de slag met AzCopy](storage-use-azcopy-v10.md) artikel AzCopy downloaden en te raken met het hulpprogramma.

## <a name="create-file-shares"></a>Bestandsshares maken

U kunt de AzCopy gebruiken `make` opdracht om een bestandsshare te maken. Het voorbeeld in deze sectie maakt u een bestandsshare met de naam `myfileshare`.

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy make "https://<storage-account-name>.file.core.windows.net/<file-share-name>?<SAS-token>"` |
| **Voorbeeld** | `azcopy make "https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

## <a name="upload-files"></a>Bestanden uploaden

U kunt de AzCopy gebruiken `copy` opdracht voor het uploaden van bestanden en mappen op uw lokale computer.

Deze sectie bevat de volgende voorbeelden:

> [!div class="checklist"]
> * Bestand uploaden
> * Map uploaden
> * Bestanden uploaden met jokertekens

> [!NOTE]
> AzCopy wordt niet automatisch berekenen en opslaan van het bestand md5-hash-code. Als u AzCopy om dat te doen wilt, voeg de `--put-md5` markeren voor elke kopieeropdracht. Op die manier kunnen wanneer het bestand wordt gedownload, AzCopy berekent een MD5-hash voor gegevens gedownloade en verifieert u dat de MD5-hash wordt opgeslagen in van het bestand `Content-md5` eigenschap komt overeen met de berekende hash.

### <a name="upload-a-file"></a>Bestand uploaden

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy cp "<local-file-path>" "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-name>?<SAS-token>"` |
| **Voorbeeld** | `azcopy copy "C:\myFolder\myTextFile.txt" "https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

### <a name="upload-a-folder"></a>Map uploaden

In dit voorbeeld wordt een map (en alle bestanden in die map) gekopieerd naar een bestandsshare. Het resultaat is een map in de bestandsshare met dezelfde naam.

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy copy "<local-folder-path>" "https://<storage-account-name>.file.core.windows.net/<file-share-name>?<SAS-token>" --recursive` |
| **Voorbeeld** | `azcopy copy "C:\myFolder" "https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" --recursive` |

Als u wilt kopiëren naar een map in de bestandsshare, alleen de naam van de map in uw opdrachttekenreeks opgeven.

|    |     |
|--------|-----------|
| **Voorbeeld** | `azcopy copy "C:\myFolder" "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareFolder?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" --recursive` |

Als u de naam van een map die niet bestaat in de bestandsshare opgeeft, wordt een nieuwe map door AzCopy gemaakt met die naam.

### <a name="upload-the-contents-of-a-folder"></a>De inhoud van een map uploaden

U kunt de inhoud van een map uploaden zonder te kopiëren die voor de map zelf met behulp van het jokerteken (*).

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy copy "<local-folder-path>/*" "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<folder-path>?<SAS-token>` |
| **Voorbeeld** | `azcopy copy "C:\myFolder\*" "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareFolder?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

> [!NOTE]
> Toevoeg-de `--recursive` vlag voor het uploaden van bestanden in alle submappen.

## <a name="download-files"></a>Bestanden downloaden

U kunt de AzCopy gebruiken `copy` opdracht voor het downloaden van bestanden, mappen en bestand deelt met uw lokale computer.

Deze sectie bevat de volgende voorbeelden:

> [!div class="checklist"]
> * Bestand downloaden
> * Een map downloaden
> * Bestanden downloaden met behulp van jokertekens

> [!NOTE]
> Als de `Content-md5` waarde van de eigenschap van een bestand bevat een hash, AzCopy berekent een MD5-hash voor gedownloade gegevens en verifieert u dat de MD5-hash wordt opgeslagen in van het bestand `Content-md5` eigenschap komt overeen met de berekende hash. Als deze waarden niet overeenkomen, het downloaden is mislukt, tenzij u dit gedrag door toe te voegen negeren `--check-md5=NoCheck` of `--check-md5=LogOnly` aan de kopieeropdracht.

### <a name="download-a-file"></a>Bestand downloaden

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-path>?<SAS-token>" "<local-file-path>"` |
| **Voorbeeld** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myFolder\myTextFile.txt"` |

### <a name="download-a-folder"></a>Een map downloaden

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<folder-path>?<SAS-token>" "<local-folder-path>" --recursive` |
| **Voorbeeld** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareFolder?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myFolder"  --recursive` |

In dit voorbeeld resulteert in een map met de naam `C:\myFolder\myFileShareFolder` die alle gedownloade bestanden bevat.

### <a name="download-the-contents-of-a-folder"></a>De inhoud van een map downloaden

U kunt de inhoud van een map downloaden zonder te kopiëren die voor de map zelf met behulp van het jokerteken (*).

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/*?<SAS-token>" "<local-folder-path>/"` |
| **Voorbeeld** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareFolder/*?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myFolder"` |

> [!NOTE]
> Toevoeg-de `--recursive` vlag voor het downloaden van bestanden in alle submappen.

## <a name="next-steps"></a>Volgende stappen

Meer voorbeelden niet vinden in een van deze artikelen:

- [Aan de slag met AzCopy](storage-use-azcopy-v10.md)

- [Gegevens overdragen met AzCopy en blob-opslag](storage-use-azcopy-blobs.md)

- [Gegevens overdragen met AzCopy en Amazon S3 buckets](storage-use-azcopy-s3.md)

- [Configureer, optimaliseren en oplossen van AzCopy](storage-use-azcopy-configure.md)