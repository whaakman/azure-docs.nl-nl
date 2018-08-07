---
title: Taakstatus Azure Import/Export - v1 controleren | Microsoft Docs
description: Informatie over het gebruik van de logboekbestanden gemaakt wanneer de taak importeren of exporteren om te zien van de status van de Import/Export-taak werd uitgevoerd.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/26/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 51a73a4466f3460beead0ce03ddbf011a0b9cc3b
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39527658"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>De status van de Azure Import/Export-taak controleren met logboekbestanden kopiëren
Wanneer de Microsoft Azure Import/Export-service stations die zijn gekoppeld aan een taak importeren of exporteren verwerkt, schrijft deze kopie logboekbestanden naar het storage-account of waaruit u importeren of exporteren blobs. Het logboekbestand bevat gedetailleerde status van elk bestand dat is geïmporteerd of geëxporteerd. De URL naar het logboekbestand van elk exemplaar wordt geretourneerd wanneer u de status van een voltooide taak; opvragen Zie [Get Job](/rest/api/storageservices/Get-Job3) voor meer informatie.  

## <a name="example-urls"></a>Voorbeeld-URL 's

Het volgende zijn voorbeeld-URL's voor de kopie-logboekbestanden voor een importtaak met twee schijven:  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 Zie [Import/Export-service Logboekbestandindeling](../storage-import-export-file-format-log.md) voor de indeling van Logboeken kopiëren en de volledige lijst met waarden.  
  
## <a name="next-steps"></a>Volgende stappen
 
 * [Het hulpprogramma Azure Import/Export instellen](storage-import-export-tool-setup-v1.md)   
 * [Harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Een importtaak herstellen](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Een exporttaak herstellen](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [Problemen met het hulpprogramma Azure Import/Export oplossen](storage-import-export-tool-troubleshooting-v1.md)
