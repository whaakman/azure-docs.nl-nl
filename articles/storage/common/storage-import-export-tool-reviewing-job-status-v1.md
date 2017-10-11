---
title: Taakstatus Azure Import/Export - v1 controleren | Microsoft Docs
description: Informatie over het gebruik van de logboekbestanden gemaakt wanneer de taak importeren of exporteren werd uitgevoerd voor de status van de taak voor importeren/exporteren.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: c69d1d69-6403-4eee-9949-0185faeecfa1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: muralikk
ms.openlocfilehash: bdb30bc28c36ab9e969efc8be3b87b97e4027b39
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>De status van de Azure Import/Export-taak controleren met logboekbestanden kopiëren
Wanneer de Microsoft Azure Import/Export-service stations die zijn gekoppeld aan een taak importeren of exporteren verwerkt, schrijft kopie logboekbestanden naar het opslagaccount naar of van waaruit u importeert of exporteert blobs. Het logboekbestand bevat een gedetailleerde status van elk bestand dat is geïmporteerd of geëxporteerd. De URL naar het logboekbestand van elke kopie wordt geretourneerd wanneer u een query uitvoeren op de status van een voltooide taak; Zie [Get Job](/rest/api/storageservices/Get-Job3) voor meer informatie.  

## <a name="example-urls"></a>Voorbeeld-URL 's

Het volgende zijn voorbeeld-URL's voor kopiëren-logboekbestanden voor een import-taak met twee schijven:  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 Zie [Import/Export-service indeling van logboekbestand](../storage-import-export-file-format-log.md) voor de indeling van de logboeken van kopiëren en een volledige lijst met statuscodes.  
  
## <a name="next-steps"></a>Volgende stappen
 
 * [Instellen van het hulpprogramma Azure Import/Export](storage-import-export-tool-setup-v1.md)   
 * [Harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Een importtaak herstellen](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Een exporttaak herstellen](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [Problemen met het hulpprogramma Azure Import/Export oplossen](storage-import-export-tool-troubleshooting-v1.md)
