---
title: Voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak Azure Import/Export | Microsoft Docs
description: Zie een scenario voor het complete proces van schijven voorbereiden voor een importtaak in de Azure Import/Export-service.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 04/07/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 42da285fbb55df43959506996bcde9cf547c2a22
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57439255"
---
# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a>Voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak

In dit artikel begeleidt u bij het complete proces van schijven voorbereiden voor een importtaak.

## <a name="sample-data"></a>Voorbeeldgegevens

In dit voorbeeld de volgende gegevens worden geïmporteerd in Azure storage-account met de naam `mystorageaccount`:

|Locatie|Description|Gegevensgrootte|
|--------------|-----------------|-----|
|H:\Video\ |Een verzameling van video 's|12 TB|
|H:\Photo\ |Een verzameling van foto 's|30 GB|
|K:\Temp\FavoriteMovie.ISO|Installatiekopie van de schijf A Blu-ray™|25 GB|
|\\\bigshare\john\music\ |Een verzameling van muziekbestanden op een netwerkshare|10 GB|

## <a name="storage-account-destinations"></a>Storage-account bestemmingen

De import-taak worden de gegevens geïmporteerd in de volgende bestemmingen in de storage-account:

|Bron|De virtuele doelmap of de blob|
|------------|-------------------------------------------|
|H:\Video\ |video /|
|H:\Photo\ |foto's /|
|K:\Temp\FavoriteMovie.ISO|favorite/FavoriteMovies.ISO|
|\\\bigshare\john\music\ |Muziek|

Met deze toewijzing, het bestand `H:\Video\Drama\GreatMovie.mov` worden geïmporteerd in de blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.

## <a name="determine-hard-drive-requirements"></a>Bepaal de vereisten voor de harde schijf

Compute vervolgens om te bepalen hoeveel harde schijven nodig zijn, de grootte van de gegevens:

`12TB + 30GB + 25GB + 10GB = 12TB + 65GB`

In dit voorbeeld moet twee 8TB harde schijven voldoende. Echter, omdat de bronmap `H:\Video` heeft 12TB aan gegevens en de capaciteit van uw één harde schijf is alleen 8TB, kunt u zich voor het volgende in de volgende manier de **driveset.csv** bestand:

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
X,Format,SilentMode,Encrypt,
Y,Format,SilentMode,Encrypt,
```
Het hulpprogramma wordt gegevens verdelen over twee harde schijven in een geoptimaliseerde manier.

## <a name="attach-drives-and-configure-the-job"></a>Koppelen van stations en de taak configureren
U Sluit beide schijven aan op de machine en volumes maken. Vervolgens auteur **dataset.csv** bestand:
```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

U kunt bovendien de volgende metagegevens voor alle bestanden instellen:

* **UploadMethod:** Windows Azure Import/Export-service
* **DataSetName:** SampleData
* **CreationDate:** 10/1/2013

Om in te stellen metagegevens voor de geïmporteerde bestanden, maak een tekstbestand, `c:\WAImportExport\SampleMetadata.txt`, met de volgende inhoud:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

U kunt ook instellen sommige eigenschappen van de `FavoriteMovie.ISO` blob:

* **Content-Type:** application/octet-stream
* **Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==
* **Cache-Control:** no-cache

Deze eigenschappen instelt, maak een tekstbestand `c:\WAImportExport\SampleProperties.txt`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

## <a name="run-the-azure-importexport-tool-waimportexportexe"></a>De Azure Import/Export-hulpprogramma (WAImportExport.exe) uitvoeren

U bent nu klaar om uit te voeren van de Azure Import/Export-hulpprogramma voor het voorbereiden van de twee harde schijven.

**Voor de eerste sessie:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

Als meer gegevens worden toegevoegd moet, maakt u een andere gegevensset-bestand (dezelfde indeling als eerste gegevensset).

**Voor de tweede sessie:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

Nadat de kopie-sessies hebt voltooid, kunt u verbreken van de twee schijven van de computer kopiëren en ze verzenden naar het juiste Azure-datacentrum. U gaat de twee logboekbestanden uploaden `<FirstDriveSerialNumber>.xml` en `<SecondDriveSerialNumber>.xml`, wanneer u de import-taak in Azure portal maken.

## <a name="next-steps"></a>Volgende stappen

* [Harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import.md)
* [Naslaggids voor veelgebruikte opdrachten](../storage-import-export-tool-quick-reference.md)
