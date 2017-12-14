---
title: De voorbeeldwerkstroom klaarmaken voor harde schijven voor een Azure Import/Export-import-taak - v1 | Microsoft Docs
description: Zie een scenario voor het volledige proces van het voorbereiden van stations voor een import-taak in de Azure Import/Export-service.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 6eb1b1b7-c69f-4365-b5ef-3cd5e05eb72a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 66e85bd3e9e43ae360d0507f5bdf3596abbeb7d1
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a>Voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak
In dit onderwerp leert u het volledige proces van het voorbereiden van stations voor een import-taak.  
  
In dit voorbeeld importeert de volgende gegevens in een venster Azure storage-account met de naam `mystorageaccount`:  
  
|Locatie|Beschrijving|  
|--------------|-----------------|  
|H:\Video|Een verzameling van video's, 5 TB in totaal.|  
|H:\Photo|Een verzameling van foto's, 30 GB in totaal.|  
|K:\Temp\FavoriteMovie.ISO|A Blu-ray™ schijfimage, 25 GB.|  
|\\\bigshare\john\music|Een verzameling van muziekbestanden op een netwerkshare, 10 GB in totaal.|  
  
De import-taak importeert deze gegevens in de volgende bestemmingen in de storage-account:  
  
|Bron|Doel-virtuele map of blob|  
|------------|-------------------------------------------|  
|H:\Video|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Photo|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|  
|K:\Temp\FavoriteMovie.ISO|https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|https://mystorageaccount.BLOB.Core.Windows.NET/Music|  
  
Met deze toewijzing, het bestand `H:\Video\Drama\GreatMovie.mov` is geïmporteerd naar de blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.  
  
Compute vervolgens om te bepalen hoeveel harde schijven nodig zijn, de grootte van de gegevens:  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
Twee 3 TB harde schijven moet voldoende zijn voor dit voorbeeld. Echter, omdat de bronmap `H:\Video` 5 TB aan gegevens en capaciteit van uw één harde schijf is alleen 3 TB hoeft te verbreken `H:\Video` in twee kleinere mappen: `H:\Video1` en `H:\Video2`, voordat u het hulpprogramma voor importeren/exporteren van Microsoft Azure. Deze stap levert de volgende bron-mappen:  
  
|Locatie|Grootte|Doel-virtuele map of blob|  
|--------------|----------|-------------------------------------------|  
|H:\Video1|2,5 TB|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Video2|2,5 TB|https://mystorageaccount.BLOB.Core.Windows.NET/video|  
|H:\Photo|30 GB|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|  
|K:\Temp\FavoriteMovies.ISO|25 GB|https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO|  
|\\\bigshare\john\music|10 GB|https://mystorageaccount.BLOB.Core.Windows.NET/Music|  
  
 Hoewel de `H:\Video`directory is gesplitst in twee mappen ze verwijzen naar dezelfde bestemming virtuele map in het opslagaccount. Op deze manier alle videobestanden worden beheerd via één `video` container in het opslagaccount.  
  
 De vorige bron mappen zijn daarna evenredig verdeeld over de twee harde schijven:  
  
||||  
|-|-|-|  
|Harde schijf|Bron-mappen|Totale grootte|  
|Eerste schijf|H:\Video1|2,5 TB + 30 GB|  
||H:\Photo||  
|Tweede station|H:\Video2|2,5 TB + 35 GB|  
||K:\Temp\BlueRay.ISO||  
||\\\bigshare\john\music||  
  
Bovendien kunt u de volgende metagegevens voor alle bestanden instellen:  
  
-   **UploadMethod:** Windows Azure Import/Export-service  
  
-   **DataSetName:** SampleData  
  
-   **CreationDate:** 1/10/2013  
  
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
  
-   **Content-Type:** application/octet-stream  
  
-   **Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==  
  
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
  
U bent nu klaar om uit te voeren van het Azure Import/Export-hulpprogramma voor het voorbereiden van de twee harde schijven. Opmerking:  
  
-   De eerste schijf is gekoppeld als station X.  
  
-   De tweede schijf is als station Y gekoppeld.  
  
-   De sleutel voor het opslagaccount `mystorageaccount` is `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a>Voorbereiden van schijf voor het importeren van gegevens vooraf is geladen
 
 Als de gegevens moeten worden geïmporteerd, al aanwezig op de schijf is, gebruikt u de vlag /skipwrite. De waarde van /t en /srcdir moet beide punt naar de schijf die wordt voorbereid voor importeren. Als alle gegevens worden geïmporteerd gaat niet op hetzelfde doel virtuele map of hoofdmap van het opslagaccount, de dezelfde opdracht uitvoeren voor elke doelmap afzonderlijk te houden de waarde van /id hetzelfde voor alle uitvoert.

>[!NOTE] 
>Geef geen/Format zoals deze wordt de gegevens op de schijf wissen. U kunt opgeven / versleutelen of /bk, afhankelijk van of de schijf al is versleuteld of niet. 
>

```
    When data is already present on the disk for each drive run the following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a>Sessies - kopiëren eerst station

Voer het hulpprogramma Azure Import/Export twee keer te kopiëren van de twee bron mappen voor de eerste schijf:  

**Eerste kopieersessie**
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

**Tweede kopieersessie**

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a>Kopieer sessies - tweede station
 
Voer de Azure-hulpprogramma voor importeren/exporteren voor het tweede station driemaal, eenmaal elk voor de bron-mappen, en eenmaal voor het installatiekopiebestand zelfstandige Blu-Ray™):  
  
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

## <a name="copy-session-completion"></a>Kopiëren van de sessie is voltooid

Nadat de kopie-sessies hebt voltooid, kunt u de twee schijven verbreken van de computer kopiëren en naar het juiste Windows Azure-datacenter worden verzonden. De twee journaal-bestanden uploaden `FirstDrive.jrn` en `SecondDrive.jrn`wanneer u de import-taak in de [Azure-portal](https://portal.azure.com).  
  
## <a name="next-steps"></a>Volgende stappen

* [Harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Naslaggids voor veelgebruikte opdrachten](../storage-import-export-tool-quick-reference-v1.md) 
