---
title: Naslaginformatie over opdrachten in de taak voor het importeren van Azure Import/Export-hulpprogramma - v1 | Microsoft Docs
description: Naslaginformatie van Azure Import/Export-hulpprogramma voor importeren van veelgebruikte taak opdrachten. Dit verwijst naar de V1-versie van het hulpprogramma Import/Export.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 16e0fa6f7336b39f63b00564d37c1be308bebb16
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526280"
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Snelzoekgids voor veelgebruikte opdrachten voor importtaken
In deze sectie biedt dat een snelle referentie voor een aantal veelgebruikte opdrachten. Zie voor gedetailleerde informatie over gebruik, [harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import-v1.md).  

## <a name="prepare-a-hard-drive-when-data-has-already-been-copied-to-the-hard-drive"></a>Een harde schijf voorbereiden wanneer gegevens al is gekopieerd naar de harde schijf
 De volgende opdracht bereidt een harde schijf wanneer gegevens naar deze al is gekopieerd, maar nog niet is versleuteld met BitLocker:  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-to-a-hard-drive"></a>Kopiëren van één map naar een harde schijf  
 De volgende opdracht wordt één bronmap gekopieerd naar een harde schijf die nog niet is versleuteld met BitLocker:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-two-directories-to-a-hard-drive"></a>Twee mappen kopiëren naar een harde schijf  
 Als u wilt kopiëren twee directory's van bron naar een station, gebruik de volgende opdrachten:  
  
 De eerste opdracht geeft de logboekmap, de sleutel van het opslagaccount, de stationsletter van het doel, `format/encrypt` vereisten, en de algemene parameters:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 De tweede opdracht geeft u het logboekbestand, een nieuwe sessie-ID en de bron- en doellocaties:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-to-a-hard-drive-in-a-second-copy-session"></a>Een groot bestand kopiëren naar een harde schijf in een tweede kopieersessie  
 De volgende opdracht wordt een enkele grote bestand gekopieerd naar een harde schijf die in een eerdere kopieersessie is voorbereid:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a>Volgende stappen

* [Voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
