---
title: Gegevens overbrengen naar Azure Storage vanuit Amazon S3 buckets met behulp van AzCopy v10 | Microsoft Docs
description: Gegevens overdragen met AzCopy en Amazon S3 buckets
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 04/23/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 414573a90e5a7b7de845b65e6e88715a26024f10
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67125792"
---
# <a name="copy-data-from-amazon-s3-buckets-by-using-azcopy"></a>Gegevens kopiëren van Amazon S3 buckets met behulp van AzCopy

AzCopy is een opdrachtregelprogramma waarmee u kunt blobs of bestanden kopiëren naar of van een storage-account. Dit artikel helpt u objecten, mappen en buckets van Amazon Web Services (AWS) S3 naar Azure blob storage kopiëren met behulp van AzCopy.

## <a name="choose-how-youll-provide-authorization-credentials"></a>Kies hoe u autorisatiereferenties bieden

* Als u wilt toestaan dat met de Azure Storage, Azure Active Directory (AD) of een Shared Access Signature (SAS)-token te gebruiken.

* Als u wilt autoriseren met AWS S3, gebruikt u een AWS-toegangssleutel en een geheime toegangssleutel.

### <a name="authorize-with-azure-storage"></a>Autoriseren met Azure Storage

Zie de [aan de slag met AzCopy](storage-use-azcopy-v10.md) artikel als u wilt downloaden, AzCopy en kies hoe u autorisatiereferenties om de storage-service te bieden.

> [!NOTE]
> De voorbeelden in dit artikel wordt ervan uitgegaan dat u uw identiteit hebt geverifieerd met behulp van de `AzCopy login` opdracht. AzCopy gebruikt vervolgens de Azure AD-account om toegang tot gegevens in Blob-opslag te verlenen.
>
> Als u liever een SAS-token toegang verlenen aan blob-gegevens, kunt u dit token toevoegen aan de bron-URL in de AzCopy-opdracht.
>
> Bijvoorbeeld: `https://mystorageaccount.blob.core.windows.net/mycontainer?<SAS-token>`.

### <a name="authorize-with-aws-s3"></a>Autoriseren met AWS S3

Verzamelen van uw AWS-toegangssleutel en de geheime toegangssleutel en stelt u deze omgevingsvariabelen:

| Besturingssysteem | Opdracht  |
|--------|-----------|
| **Windows** | `set AWS_ACCESS_KEY_ID=<access-key>`<br>`set AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **Linux** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **MacOS** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>`|

## <a name="copy-objects-directories-and-buckets"></a>Kopiëren van objecten, mappen en buckets

AzCopy gebruikt de [blok plaatsen van URL](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) API, zodat gegevens rechtstreeks tussen AWS S3 en de opslagserver is gekopieerd. Deze kopieerbewerkingen gebruik niet de bandbreedte van het netwerk van uw computer.

> [!IMPORTANT]
> Deze functie is momenteel beschikbaar als preview-product. Als u besluit om gegevens te verwijderen uit de buckets S3 na een kopieerbewerking, moet u om te controleren dat de gegevens juist zijn gekopieerd naar uw opslagaccount voordat u de gegevens worden verwijderd.

### <a name="copy-an-object"></a>Een object kopiëren

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy cp "https://s3.amazonaws.com/<bucket-name>/<object-name>" "https://<storage-account-name>.blob.core.windows.net/<container-name>/<blob-name>"` |
| **Voorbeeld** | `azcopy cp "https://s3.amazonaws.com/mybucket/myobject" "https://mystorageaccount.blob.core.windows.net/mycontainer/myblob"` |

> [!NOTE]
> Voorbeelden in dit artikel gebruiken pad-stijl-URL's voor AWS S3 duurbuckets (bijvoorbeeld: `http://s3.amazonaws.com/<bucket-name>`). 
>
> U kunt ook virtuele host-stijl-URL's ook gebruiken (bijvoorbeeld: `http://bucket.s3.amazonaws.com`). 
>
> Zie voor meer informatie over het hosten van virtuele van buckets [virtuele die als host fungeert van de Buckets]] (https://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html).

### <a name="copy-a-directory"></a>Een map kopiëren

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy cp "https://s3.amazonaws.com/<bucket-name>/<directory-name>" "https://<storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>" --recursive=true` |
| **Voorbeeld** | `azcopy cp "https://s3.amazonaws.com/mybucket/mydirectory" "https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory" --recursive=true` |

### <a name="copy-a-bucket"></a>Een bucket kopiëren

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy cp "https://s3.amazonaws.com/<bucket-name>" "https://<storage-account-name>.blob.core.windows.net/<container-name>" --recursive=true` |
| **Voorbeeld** | `azcopy cp "https://s3.amazonaws.com/mybucket" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive=true` |

### <a name="copy-all-buckets-in-all-regions"></a>Kopieer alle buckets in alle regio 's

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy cp "https://s3.amazonaws.com/" "https://<storage-account-name>.blob.core.windows.net" --recursive=true` |
| **Voorbeeld** | `azcopy cp "https://s3.amazonaws.com" "https://mystorageaccount.blob.core.windows.net" --recursive=true` |

### <a name="copy-all-buckets-in-a-specific-s3-region"></a>Kopieer alle buckets in een specifieke S3-regio

|    |     |
|--------|-----------|
| **Syntaxis** | `azcopy cp "https://s3-<region-name>.amazonaws.com/" "https://<storage-account-name>.blob.core.windows.net" --recursive=true` |
| **Voorbeeld** | `azcopy cp "https://s3-rds.eu-north-1.amazonaws.com" "https://mystorageaccount.blob.core.windows.net" --recursive=true` |

## <a name="handle-differences-in-object-naming-rules"></a>Verschillen in object naamgevingsregels verwerken

AWS S3 heeft een andere set naamgevingsregels voor bucket namen in vergelijking met Azure blob-containers. U kunt meer informatie over deze [hier](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules). Als u ervoor kiest om te kopiëren van een groep buckets naar Azure storage-account, wordt de kopieerbewerking mislukken vanwege de naamgeving van verschillen.

AzCopy verwerkt twee van de meest voorkomende problemen die zich voordoen kunnen; buckets die punten en buckets met opeenvolgende afbreekstreepjes bevatten. Namen van AWS S3-bucket kunnen punten en opeenvolgende afbreekstreepjes bevatten, maar niet van een container in Azure. AzCopy perioden vervangt door afbreekstreepjes en opeenvolgende afbreekstreepjes bevatten met een getal dat het aantal opeenvolgende afbreekstreepjes bevatten (bijvoorbeeld: een bucket met de naam `my----bucket` wordt `my-4-bucket`. 

Ook als AzCopy via bestanden kopieert, het gecontroleerd voor het benoemen van conflicten en probeert op te lossen. Bijvoorbeeld, als er buckets met de naam van de `bucket-name` en `bucket.name`, AzCopy een bucket met de naam wordt omgezet `bucket.name` eerste naar `bucket-name` en van daaruit naar `bucket-name-2`.

## <a name="handle-differences-in-object-metadata"></a>Verschillen in de metagegevens van een object worden verwerkt

AWS S3 en Azure kunt verschillende sets tekens in de namen van de objectsleutels. U kunt meer informatie over de tekens dat gebruikmaakt van AWS S3 [hier](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html#object-keys). Aan de Azure blob-objectsleutels voldoen aan de naamgevingsregels voor [ C# id's](https://docs.microsoft.com/dotnet/csharp/language-reference/).

Als onderdeel van een AzCopy `copy` opdracht, u kunt een waarde opgeven voor optionele de `s2s-invalid-metadata-handle` vlag die aangeeft hoe u wilt bestanden waar de metagegevens van het bestand niet-compatibele sleutelnamen bevat verwerkt. De volgende tabel beschrijft elke waarde voor markering.

| Waarde voor markering | Description  |
|--------|-----------|
| **ExcludeIfInvalid** | (Standaardoptie) De metagegevens niet is opgenomen in de overgebrachte object. AzCopy registreert een waarschuwing. |
| **FailIfInvalid** | Objecten zijn niet gekopieerd. AzCopy een fout wordt vastgelegd en is voorzien van die fout in het aantal mislukte items die wordt weergegeven in de samenvatting van de overdracht.  |
| **RenameIfInvalid**  | AzCopy oplossing voor de sleutel ongeldige metagegevens en het object worden gekopieerd naar Azure met behulp van de sleutel-waardepaar opgelost metagegevens. Zie voor meer precies welke stappen AzCopy duurt om te wijzigen, objectsleutels, de [hoe AzCopy wijzigt de naam van objectsleutels](#rename-logic) onderstaande sectie. Als AzCopy kan de naam van de sleutel is, klikt u vervolgens het object worden niet gekopieerd. |

<a id="rename-logic" />

### <a name="how-azcopy-renames-object-keys"></a>Hoe AzCopy wijzigt de naam van objectsleutels

AzCopy voert deze stappen uit:

1. Ongeldige tekens vervangt door '_'.

2. De tekenreeks wordt toegevoegd `rename_` aan het begin van een nieuwe geldige sleutel.

   Deze sleutel wordt gebruikt voor het opslaan van de metagegevens van de oorspronkelijke **waarde**.

3. De tekenreeks wordt toegevoegd `rename_key_` aan het begin van een nieuwe geldige sleutel.
   Deze sleutel wordt gebruikt voor het opslaan van de oorspronkelijke metagegevens ongeldig **sleutel**.
   U kunt deze sleutel gebruiken om te proberen te herstellen van de metagegevens in Azure-kant omdat de metagegevenssleutel wordt behouden als een waarde van de Blob storage-service.

## <a name="next-steps"></a>Volgende stappen

Meer voorbeelden niet vinden in een van deze artikelen:

- [Aan de slag met AzCopy](storage-use-azcopy-v10.md)

- [Gegevens overdragen met AzCopy en blob-opslag](storage-use-azcopy-blobs.md)

- [Gegevens overdragen met AzCopy en bestandsopslag](storage-use-azcopy-files.md)

- [Configureer, optimaliseren en oplossen van AzCopy](storage-use-azcopy-configure.md)