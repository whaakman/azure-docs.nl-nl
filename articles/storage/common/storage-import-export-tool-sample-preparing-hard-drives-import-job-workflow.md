---
title: De voorbeeldwerkstroom harde schijven voor de import-taak van een Azure Import/Export voorbereiden | Microsoft Docs
description: Zie een scenario voor het volledige proces van het voorbereiden van stations voor een import-taak in de Azure Import/Export-service.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: muralikk
ms.openlocfilehash: 60139ff36b66432620591ceaf201e046ad30217f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a>Voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak

Dit artikel begeleidt u bij het volledige proces van het voorbereiden van stations voor een import-taak.

## <a name="sample-data"></a>Voorbeeldgegevens

In dit voorbeeld importeert de volgende gegevens in Azure storage-account met de naam `mystorageaccount`:

|Locatie|Beschrijving|Gegevensgrootte|
|--------------|-----------------|-----|
|H:\Video\ |Een verzameling van video 's|12 TB|
|H:\Photo\ |Een verzameling van foto 's|30 GB|
|K:\Temp\FavoriteMovie.ISO|De schijfimage A Blu-ray™|25 GB|
|\\\bigshare\john\music\ |Een verzameling van muziekbestanden op een netwerkshare|10 GB|

## <a name="storage-account-destinations"></a>Storage-account bestemmingen

De import-taak worden de gegevens importeren in de volgende bestemmingen in de storage-account:

|Bron|Doel-virtuele map of blob|
|------------|-------------------------------------------|
|H:\Video\ |video /|
|H:\Photo\ |Photo /|
|K:\Temp\FavoriteMovie.ISO|favorite/FavoriteMovies.ISO|
|\\\bigshare\john\music\ |Music|

Met deze toewijzing, het bestand `H:\Video\Drama\GreatMovie.mov` wordt geïmporteerd naar de blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.

## <a name="determine-hard-drive-requirements"></a>Harde schijf vereisten bepalen

Compute vervolgens om te bepalen hoeveel harde schijven nodig zijn, de grootte van de gegevens:

`12TB + 30GB + 25GB + 10GB = 12TB + 65GB`

Twee harde schijven van 8TB moet voldoende zijn voor dit voorbeeld. Echter, omdat de bronmap `H:\Video` heeft 12TB aan gegevens en capaciteit van uw één harde schijf is alleen 8TB, kun je dit opgeven in de volgende manier de **driveset.csv** bestand:

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
X,Format,SilentMode,Encrypt,
Y,Format,SilentMode,Encrypt,
```
Het hulpprogramma wordt gegevens verdelen over twee harde schijven op een geoptimaliseerde manier.

## <a name="attach-drives-and-configure-the-job"></a>Koppelen van stations en configureren van de taak
U koppelt beide schijven aan de machine en volumes maken. Vervolgens auteur **dataset.csv** bestand:
```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

Bovendien kunt u de volgende metagegevens voor alle bestanden instellen:

* **UploadMethod:** Windows Azure Import/Export-service
* **DataSetName:** SampleData
* **CreationDate:** 1/10/2013

Om in te stellen metagegevens voor de geïmporteerde bestanden, maak een tekstbestand, `c:\WAImportExport\SampleMetadata.txt`, met de volgende inhoud:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

U kunt ook bepaalde eigenschappen instellen voor de `FavoriteMovie.ISO` blob:

* **Content-Type:** application/octet-stream
* **Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==
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

## <a name="run-the-azure-importexport-tool-waimportexportexe"></a>Voer het Azure Import/Export-hulpprogramma (WAImportExport.exe)

U bent nu klaar om uit te voeren van het Azure Import/Export-hulpprogramma voor het voorbereiden van de twee harde schijven.

**Voor de eerste sessie:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

Als meer gegevens worden toegevoegd moet, maakt u een andere dataset-bestand (dezelfde indeling als Initialdataset).

**Voor de tweede sessie:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

Nadat de kopie-sessies hebt voltooid, kunt u de twee schijven verbreken van de computer kopiëren en naar de juiste Azure-datacenter worden verzonden. U gaat de twee journaal-bestanden uploaden `<FirstDriveSerialNumber>.xml` en `<SecondDriveSerialNumber>.xml`, wanneer u de import-taak in de Azure-portal maken.

## <a name="next-steps"></a>Volgende stappen

* [Harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import.md)
* [Naslaggids voor veelgebruikte opdrachten](../storage-import-export-tool-quick-reference.md)
