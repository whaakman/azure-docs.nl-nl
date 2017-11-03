---
title: Een voorbeeldweergave schijfgebruik voor een taak van de export Azure Import/Export - v1 | Microsoft Docs
description: Informatie over het bekijken van de lijst met blobs die u hebt geselecteerd voor een exporttaak in de Azure Import/Export-service.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 7707d744-7ec7-4de8-ac9b-93a18608dc9a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 6ec74ae0b0931f3fed99a43f4f7e58f9d425b138
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="previewing-drive-usage-for-an-export-job"></a>Op voorhand het schijfgebruik voor een exporttaak bekijken
Voordat u een exporttaak maakt, moet u kiezen van een reeks blobs worden geëxporteerd. De Microsoft Azure Import/Export-service kunt u voor het gebruik van een lijst met blob-paden of blob-voorvoegsels te vertegenwoordigen de blobs die u hebt geselecteerd.  
  
Vervolgens moet u bepalen hoeveel stations die u wilt verzenden. Het hulpprogramma voor importeren/exporteren geeft de `PreviewExport` opdracht voor de preview schijfgebruik voor de blobs die u hebt geselecteerd, op basis van de grootte van de stations die u gaat gebruiken.

## <a name="command-line-parameters"></a>Opdrachtregelparameters

U kunt de volgende parameters gebruiken wanneer u de `PreviewExport` opdracht van het hulpprogramma voor importeren/exporteren.

|Opdrachtregelparameter|Beschrijving|  
|--------------------------|-----------------|  
|**schakeloptie/LOGDIR op:**< LogDirectory\>|Optioneel. De logboekmap. Uitgebreide logboekbestanden worden geschreven naar deze map. Als er geen logboekmap is opgegeven, wordt de huidige map gebruikt als de logboekmap.|  
|**/sn:**< StorageAccountName\>|Vereist. De naam van het opslagaccount voor de taak voor het exporteren.|  
|**/SK:**< StorageAccountKey\>|Vereist als een container SAS is niet opgegeven. De accountsleutel voor het opslagaccount voor de taak voor exporteren.|  
|**/csas:**< ContainerSas\>|Vereist als de sleutel van een opslagaccount is niet opgegeven. De container SAS voor het weergeven van de blobs in de taak voor het exporteren worden geëxporteerd.|  
|**/ ExportBlobListFile:**< ExportBlobListFile\>|Vereist. Pad naar het XML-bestand met lijst met blob-paden bestand of blob-pad voorvoegsels voor de blobs worden geëxporteerd. De bestandsindeling die wordt gebruikt in de `BlobListBlobPath` -element in de [taak plaatsen](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) bewerking van de Import/Export-service REST-API.|  
|**/ DriveSize:**< DriveSize\>|Vereist. De grootte van stations voor een exporttaak *bijvoorbeeld*, 500 GB, 1,5 TB.|  

## <a name="command-line-example"></a>Voorbeeld van de opdrachtregel

Het volgende voorbeeld toont de `PreviewExport` opdracht:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
Het exportbestand voor de lijst van blob kan blobnamen bevatten en blob-voorvoegsels, zoals hier wordt weergegeven:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

De Azure-hulpprogramma voor importeren/exporteren geeft een lijst van alle blobs worden geëxporteerd en berekent hoe ze in de stations van de opgegeven grootte pack rekening wordt gehouden met eventuele benodigde overhead en maakt een schatting van het aantal stations die nodig zijn voor het opslaan van blobs en informatie over het gebruik van de schijf.  
  
Hier volgt een voorbeeld van uitvoer van de met informatief logboeken die worden weggelaten:  
  
```  
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  
  
Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB    
```  
  
## <a name="next-steps"></a>Volgende stappen

* [Naslaginformatie over Azure hulpprogramma voor importeren/exporteren](../storage-import-export-tool-how-to-v1.md)
