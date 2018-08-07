---
title: Azure Import/Export-eigenschappen en metagegevens bestandsindeling | Microsoft Docs
description: Leer hoe u metagegevens en eigenschappen voor een of meer blobs die deel uitmaken van een importeren of exporteren van de taak op te geven.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 5a886244b43ad006a95e9be0350d9c69fd987ad9
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526229"
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a>Azure Import/Export-service eigenschappen en metagegevens bestandsindeling
U kunt de eigenschappen voor een of meer blobs en metagegevens opgeven als onderdeel van een import-taak of een exporttaak bekijken. Om in te stellen metagegevens of eigenschappen voor blobs als onderdeel van een import-taak wordt gemaakt, bieden u een bestand met metagegevens of eigenschappen op de harde schijf met de gegevens die kunnen worden geïmporteerd. Voor een exporttaak bekijken, worden eigenschappen en metagegevens geschreven naar een bestand metagegevens of eigenschappen die is opgenomen op de harde schijf die aan u worden geretourneerd.  
  
## <a name="metadata-file-format"></a>Bestandsindeling van metagegevens  
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
|`Metadata`|Root-element|Het hoofdelement van het bestand met metagegevens.|  
|`metadata-name`|Reeks|Optioneel. Het XML-element Hiermee geeft u de naam van de metagegevens voor de blob en de waarde bevat de waarde van de instelling van de metagegevens.|  
  
## <a name="properties-file-format"></a>Bestandsindeling van eigenschappen  
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
|`Properties`|Root-element|Het hoofdelement van het eigenschappenbestand.|  
|`Last-Modified`|Reeks|Optioneel. Het tijdstip laatst gewijzigd voor de blob. Voor alleen voor export-taken.|  
|`Etag`|Reeks|Optioneel. De ETag-waarde van de blob. Voor alleen voor export-taken.|  
|`Content-Length`|Reeks|Optioneel. De grootte van de blob in bytes. Voor alleen voor export-taken.|  
|`Content-Type`|Reeks|Optioneel. Het inhoudstype van de blob.|  
|`Content-MD5`|Reeks|Optioneel. MD5-hash van de blob.|  
|`Content-Encoding`|Reeks|Optioneel. De inhoud van de blob-codering.|  
|`Content-Language`|Reeks|Optioneel. Inhoud van de blob-taal.|  
|`Cache-Control`|Reeks|Optioneel. De cache-control-tekenreeks voor de blob.|  

## <a name="next-steps"></a>Volgende stappen

Zie [blob-eigenschappen instellen](/rest/api/storageservices/set-blob-properties), [Blobmetagegevens ingesteld](/rest/api/storageservices/set-blob-metadata), en [instelling en bij het ophalen van de eigenschappen en metagegevens voor blob-bronnen](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) voor gedetailleerde regels over de metagegevens van de instelling voor blob en eigenschappen.
