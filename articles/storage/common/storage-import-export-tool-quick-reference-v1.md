---
title: Naslaginformatie over opdrachten in de taak voor het importeren van Azure-hulpprogramma voor importeren/exporteren - v1 | Microsoft Docs
description: Hulpprogramma voor importeren/exporteren opdracht naslaginformatie over Azure voor opdrachten van veelgebruikte import-taak. Dit verwijst naar v1 van het hulpprogramma voor importeren/exporteren.
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
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 370cf6fae7ae106e8341f65086c8b8187d335746
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Snelzoekgids voor veelgebruikte opdrachten voor importtaken
Deze sectie bevat dat naslaginformatie over een aantal veelgebruikte opdrachten. Zie voor gedetailleerde informatie over het gebruik, [harde schijven voorbereiden voor een Import-taak](../storage-import-export-tool-preparing-hard-drives-import-v1.md).  

## <a name="prepare-a-hard-drive-when-data-has-already-been-copied-to-the-hard-drive"></a>Voorbereiding van een harde schijf als gegevens al is gekopieerd naar de harde schijf
 De volgende opdracht wordt een harde schijf voorbereid wanneer gegevens naar deze al is gekopieerd, maar nog niet is versleuteld met BitLocker:  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-to-a-hard-drive"></a>Een enkele map kopiëren naar een harde schijf  
 De volgende opdracht wordt één bronmap gekopieerd naar een harde schijf die nog niet is versleuteld met BitLocker:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-two-directories-to-a-hard-drive"></a>Twee mappen kopiëren naar een harde schijf  
 Om te kopiëren twee directory's van bron naar een station, gebruikt u de volgende opdrachten:  
  
 De eerste opdracht geeft de logboekmap, de opslagaccountsleutel, de stationsletter van het doel, `format/encrypt` vereisten en de algemene parameters:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 De tweede opdracht geeft het logboek-bestand, een nieuwe sessie-ID en de bron- en doellocaties:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-to-a-hard-drive-in-a-second-copy-session"></a>Een groot bestand kopiëren naar een harde schijf in een tweede kopieersessie  
 De volgende opdracht wordt één groot bestand gekopieerd naar een harde schijf die in een eerdere sessie. exemplaar is voorbereid:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a>Volgende stappen

* [Voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
