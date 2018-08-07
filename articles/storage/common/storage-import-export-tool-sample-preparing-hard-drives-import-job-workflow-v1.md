---
title: Voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak Azure Import/Export - v1 | Microsoft Docs
description: Zie een scenario voor het complete proces van schijven voorbereiden voor een importtaak in de Azure Import/Export-service.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: ae792df428d897277e15df9db3ff6f99a5b8859e
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39527284"
---
# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a>Voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak
In dit onderwerp begeleidt u bij het complete proces van schijven voorbereiden voor een importtaak.  
  
In dit voorbeeld de volgende gegevens worden geïmporteerd in een Windows Azure storage-account met de naam `mystorageaccount`:  
  
|Locatie|Beschrijving|  
|--------------|-----------------|  
|H:\Video|Een verzameling van video's, 5 TB in totaal.|  
|H:\Photo|Een verzameling van foto's, 30 GB in totaal.|  
|K:\Temp\FavoriteMovie.ISO|A Blu-ray™ schijfinstallatiekopie, 25 GB.|  
|\\\bigshare\john\music|Een verzameling van muziekbestanden op een netwerkshare, 10 GB in totaal.|  
  
De import-taak importeert deze gegevens in de volgende bestemmingen in de storage-account:  
  
|Bron|De virtuele doelmap of de blob|  
|------------|-------------------------------------------|  
|H:\Video|https://mystorageaccount.blob.core.windows.net/video|  
|H:\Photo|https://mystorageaccount.blob.core.windows.net/photo|  
|K:\Temp\FavoriteMovie.ISO|https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|https://mystorageaccount.blob.core.windows.net/music|  
  
Met deze toewijzing, het bestand `H:\Video\Drama\GreatMovie.mov` wordt geïmporteerd naar de blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.  
  
Compute vervolgens om te bepalen hoeveel harde schijven nodig zijn, de grootte van de gegevens:  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
In dit voorbeeld moet twee 3 TB harde schijven voldoende. Echter, omdat de bronmap `H:\Video` 5 TB aan gegevens heeft en de capaciteit van uw één harde schijf is alleen 3 TB, is het nodig zijn om te doorbreken `H:\Video` in twee kleinere mappen: `H:\Video1` en `H:\Video2`, voordat u de Microsoft Azure Import/Export-hulpprogramma. Deze stap levert de volgende bron-mappen:  
  
|Locatie|Grootte|De virtuele doelmap of de blob|  
|--------------|----------|-------------------------------------------|  
|H:\Video1|2,5 TB|https://mystorageaccount.blob.core.windows.net/video|  
|H:\Video2|2,5 TB|https://mystorageaccount.blob.core.windows.net/video|  
|H:\Photo|30 GB|https://mystorageaccount.blob.core.windows.net/photo|  
|K:\Temp\FavoriteMovies.ISO|25 GB|https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|10 GB|https://mystorageaccount.blob.core.windows.net/music|  
  
 Hoewel de `H:\Video`directory is gesplitst in twee directory's, ze verwijzen naar de dezelfde bestemming virtuele map in de storage-account. Op deze manier alle videobestanden worden bijgehouden in een enkel `video` container in het opslagaccount.  
  
 De vorige bron-mappen worden vervolgens gelijkmatig gedistribueerd naar de twee harde schijven:  
  
||||  
|-|-|-|  
|Harde schijf|Bron-mappen|Totale grootte|  
|Eerste station|H:\Video1|2,5 TB + 30 GB|  
||H:\Photo||  
|Tweede station|H:\Video2|2,5 TB + 35 GB|  
||K:\Temp\BlueRay.ISO||  
||\\\bigshare\john\music||  
  
U kunt bovendien de volgende metagegevens voor alle bestanden instellen:  
  
-   **UploadMethod:** Windows Azure Import/Export-service  
  
-   **DataSetName:** SampleData  
  
-   **CreationDate:** 10/1/2013  
  
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
  
-   **Content-Type:** application/octet-stream  
  
-   **Inhoud-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==  
  
-   **Cache-Control:** no-cache  
  
Deze eigenschappen instelt, maak een tekstbestand `c:\WAImportExport\SampleProperties.txt`:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
U bent nu klaar om uit te voeren van de Azure Import/Export-hulpprogramma voor het voorbereiden van de twee harde schijven. Houd rekening met het volgende:  
  
-   De eerste schijf is gekoppeld als station X.  
  
-   De tweede schijf is gekoppeld als Y-station.  
  
-   De sleutel voor het opslagaccount `mystorageaccount` is `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a>Schijf voorbereiden voor importeren wanneer gegevens worden vooraf geladen
 
 Als de gegevens worden geïmporteerd, al aanwezig op de schijf is, gebruikt u de vlag /skipwrite. De waarde van /t en /srcdir moet beide punt op de schijf wordt voorbereid voor het importeren. Als alle gegevens worden geïmporteerd is niet meer de dezelfde bestemming virtuele map of het basiscertificaat van de storage-account, de dezelfde opdracht uitvoeren voor elke doelmap afzonderlijk, de waarde van /id hetzelfde blijven voor alle runs.

>[!NOTE] 
>Geef geen/Format als deze de gegevens op de schijf worden gewist. U kunt opgeven / versleutelen of /bk, afhankelijk van of de schijf al is versleuteld of niet. 
>

```
    When data is already present on the disk for each drive run the following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a>Sessies - Kopieer eerst station

Voor de eerste schijf, het hulpprogramma Azure Import/Export twee keer als u wilt kopiëren van de bron van twee directory's worden uitgevoerd:  

**Eerste kopieersessie**
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

**Tweede kopieersessie**

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a>Kopieer sessies - tweede station
 
Voor de tweede schijf, voer het hulpprogramma Azure Import/Export drie keer, één keer voor de bron-mappen, en één keer voor het installatiekopiebestand zelfstandige Blu-Ray™):  
  
**Eerste kopieersessie** 

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Video2 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:y /format /encrypt /srcdir:H:\Video2 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```
  
**Tweede kopieersessie**

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Music /srcdir:\\bigshare\john\music /dstdir:music/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```  
  
**Derde kopieersessie**  

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```

## <a name="copy-session-completion"></a>Voltooiing van de sessie kopiëren

Nadat de kopie-sessies hebt voltooid, kunt u verbreken van de twee schijven van de computer kopiëren en ze verzenden naar de juiste Windows Azure-Datacenter. De twee logboekbestanden uploaden `FirstDrive.jrn` en `SecondDrive.jrn`, bij het maken van de import-taak in de [Azure-portal](https://portal.azure.com).  
  
## <a name="next-steps"></a>Volgende stappen

* [Harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Naslaggids voor veelgebruikte opdrachten](../storage-import-export-tool-quick-reference-v1.md) 
