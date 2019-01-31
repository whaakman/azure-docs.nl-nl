---
title: Azure Import/Export-eigenschappen en metagegevens bestandsindeling | Microsoft Docs
description: Leer hoe u metagegevens en eigenschappen voor een of meer blobs die deel uitmaken van een importeren of exporteren van de taak op te geven.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 2066d4a2ed6db97285d92d15e14dbd21629dbdfa
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55457000"
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
  
|XML-Element|Type|Description|  
|-----------------|----------|-----------------|  
|`Metadata`|Root-element|Het hoofdelement van het bestand met metagegevens.|  
|`metadata-name`|String|Optioneel. Het XML-element Hiermee geeft u de naam van de metagegevens voor de blob en de waarde bevat de waarde van de instelling van de metagegevens.|  
  
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
  
|XML-Element|Type|Description|  
|-----------------|----------|-----------------|  
|`Properties`|Root-element|Het hoofdelement van het eigenschappenbestand.|  
|`Last-Modified`|String|Optioneel. Het tijdstip laatst gewijzigd voor de blob. Voor alleen voor export-taken.|  
|`Etag`|String|Optioneel. De ETag-waarde van de blob. Voor alleen voor export-taken.|  
|`Content-Length`|String|Optioneel. De grootte van de blob in bytes. Voor alleen voor export-taken.|  
|`Content-Type`|String|Optioneel. Het inhoudstype van de blob.|  
|`Content-MD5`|String|Optioneel. MD5-hash van de blob.|  
|`Content-Encoding`|String|Optioneel. De inhoud van de blob-codering.|  
|`Content-Language`|String|Optioneel. Inhoud van de blob-taal.|  
|`Cache-Control`|String|Optioneel. De cache-control-tekenreeks voor de blob.|  

## <a name="next-steps"></a>Volgende stappen

Zie [blob-eigenschappen instellen](/rest/api/storageservices/set-blob-properties), [Blobmetagegevens ingesteld](/rest/api/storageservices/set-blob-metadata), en [instelling en bij het ophalen van de eigenschappen en metagegevens voor blob-bronnen](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) voor gedetailleerde regels over de metagegevens van de instelling voor blob en eigenschappen.
