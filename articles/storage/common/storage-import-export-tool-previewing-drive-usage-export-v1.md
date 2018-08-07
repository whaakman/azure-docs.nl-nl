---
title: Op voorhand het schijfgebruik voor een exporttaak Azure Import/Export - v1 | Microsoft Docs
description: Informatie over het bekijken van de lijst met blobs die u hebt geselecteerd voor een exporttaak bekijken in de Azure Import/Export-service.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 21c0fd9b258100e769172332713769024fb12969
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39520568"
---
# <a name="previewing-drive-usage-for-an-export-job"></a>Op voorhand het schijfgebruik voor een exporttaak bekijken
Voordat u een exporttaak maken, moet u kiezen van een reeks blobs worden geëxporteerd. De Microsoft Azure Import/Export-service kunt u voor het gebruik van een lijst met blob-paden of blob-voorvoegsels voor de blobs die u hebt geselecteerd.  
  
Vervolgens moet u bepalen hoeveel stations die u wilt verzenden. Het hulpprogramma Import/Export biedt de `PreviewExport` opdracht om een voorbeeld van schijfgebruik voor de blobs die u hebt geselecteerd, op basis van de grootte van de stations die u wilt gebruiken.

## <a name="command-line-parameters"></a>Opdrachtregelparameters

U kunt de volgende parameters gebruiken bij het gebruik van de `PreviewExport` opdracht van het hulpprogramma Import/Export.

|Opdrachtregelparameter|Beschrijving|  
|--------------------------|-----------------|  
|**schakeloptie/LOGDIR op:**< LogDirectory\>|Optioneel. De logboekmap. Uitgebreide logboeken worden geschreven naar deze map. Als er geen logboekmap is opgegeven, wordt de huidige map worden gebruikt als de logboekmap.|  
|**/sn:**< StorageAccountName\>|Vereist. De naam van het opslagaccount voor de taak voor het exporteren.|  
|**/SK:**< StorageAccountKey\>|Vereist als een container SAS is niet opgegeven. De accountsleutel voor het opslagaccount voor de taak voor het exporteren.|  
|**/csas:**< ContainerSas\>|Vereist als een sleutel van het opslagaccount is niet opgegeven. De container SAS voor het weergeven van de blobs in de taak voor het exporteren worden geëxporteerd.|  
|**/ ExportBlobListFile:**< ExportBlobListFile\>|Vereist. Pad naar het XML-bestand opslaan met lijst met blob-paden of blob-voorvoegsels voor pad voor de blobs worden geëxporteerd. De bestandsindeling die wordt gebruikt de `BlobListBlobPath` -element in de [plaatsen taak](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) werking van de Import/Export-service REST-API.|  
|**/ DriveSize:**< DriveSize\>|Vereist. De grootte van schijven te gebruiken voor een exporttaak *bijvoorbeeld*, 500 GB, 1,5 TB.|  

## <a name="command-line-example"></a>Voorbeeld van opdrachtregel

Het volgende voorbeeld ziet u de `PreviewExport` opdracht:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
Het exportbestand voor de blob-lijst kan bevatten blob-namen en blob-voorvoegsels, zoals hier wordt weergegeven:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

De Azure Import/Export-hulpprogramma geeft een lijst van alle blobs worden geëxporteerd en berekent het pack ze in de stations van de opgegeven grootte, waarbij rekening wordt gehouden met eventuele vereiste overhead en maakt een schatting van het aantal stations die nodig zijn voor het opslaan van blobs en informatie over het gebruik van de schijf.  
  
Hier volgt een voorbeeld van de uitvoer, met informatieve logboeken weggelaten:  
  
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

* [Naslaginformatie over de Azure Import/Export-hulpprogramma](../storage-import-export-tool-how-to-v1.md)
