---
title: Azure Import/Export metagegevens en eigenschappen bestandsindeling | Microsoft Docs
description: Informatie over het opgeven van metagegevens en eigenschappen voor een of meer blobs die deel uitmaken van een importeren of exporteren van de taak.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 840364c6-d9a8-4b43-a9f3-f7441c625069
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 3f728ad94cdcbd32092b677f11a737ae91376720
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a>Azure Import/Export-service metagegevens en eigenschappen bestandsindeling
U kunt de eigenschappen voor een of meer blobs en metagegevens als onderdeel van een import-taak of een taak voor exporteren opgeven. Metagegevens of eigenschappen voor BLOB's worden gemaakt als onderdeel van een import-taak, stelt opgeven u een bestand eigenschappen of metagegevens op de harde schijf met de gegevens moeten worden geïmporteerd. Voor een exporttaak worden metagegevens en eigenschappen geschreven naar een bestand eigenschappen of metagegevens die is opgenomen op de harde schijf die aan u worden geretourneerd.  
  
## <a name="metadata-file-format"></a>Bestandsindeling voor metagegevens  
De indeling van een bestand met metagegevens is als volgt:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|XML-Element|Type|Beschrijving|  
|-----------------|----------|-----------------|  
|`Metadata`|Hoofdelement|Het hoofdelement van het bestand met metagegevens.|  
|`metadata-name`|Tekenreeks|Optioneel. Het XML-element bevat de naam van de metagegevens voor de blob en de waarde geeft de waarde van de instelling voor metagegevens.|  
  
## <a name="properties-file-format"></a>Eigenschappen-bestandsindeling  
De indeling van een eigenschappenbestand is als volgt:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
[<Last-Modified>date-time-value</Last-Modified>]  
[<Etag>etag</Etag>]  
[<Content-Length>size-in-bytes<Content-Length>]  
[<Content-Type>content-type</Content-Type>]  
[<Content-MD5>content-md5</Content-MD5>]  
[<Content-Encoding>content-encoding</Content-Encoding>]  
[<Content-Language>content-language</Content-Language>]  
[<Cache-Control>cache-control</Cache-Control>]  
</Properties>  
```
  
|XML-Element|Type|Beschrijving|  
|-----------------|----------|-----------------|  
|`Properties`|Hoofdelement|Het hoofdelement van het eigenschappenbestand.|  
|`Last-Modified`|Tekenreeks|Optioneel. Tijd laatste wijziging voor de blob. Voor exporttaken.|  
|`Etag`|Tekenreeks|Optioneel. De ETag-waarde van de blob. Voor exporttaken.|  
|`Content-Length`|Tekenreeks|Optioneel. De grootte van de blob in bytes. Voor exporttaken.|  
|`Content-Type`|Tekenreeks|Optioneel. Het inhoudstype van de blob.|  
|`Content-MD5`|Tekenreeks|Optioneel. De blob-MD5-hash.|  
|`Content-Encoding`|Tekenreeks|Optioneel. De blob-inhoud codering.|  
|`Content-Language`|Tekenreeks|Optioneel. Inhoud van de blob-taal.|  
|`Cache-Control`|Tekenreeks|Optioneel. De cache-control-tekenreeks voor de blob.|  

## <a name="next-steps"></a>Volgende stappen

Zie [blob-eigenschappen instellen](/rest/api/storageservices/set-blob-properties), [Blobmetagegevens ingesteld](/rest/api/storageservices/set-blob-metadata), en [instelling en het bij het ophalen van eigenschappen en metagegevens voor blob-bronnen](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) voor gedetailleerde informatie over het instellen van eigenschappen en blobmetagegevens-regels.
