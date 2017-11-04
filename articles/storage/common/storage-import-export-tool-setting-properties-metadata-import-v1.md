---
title: Instellen van eigenschappen en metagegevens met behulp van Azure Import/Export - v1 | Microsoft Docs
description: Informatie over het opgeven van de eigenschappen en metagegevens worden ingesteld op de bestemming blobs bij het uitvoeren van de Azure-hulpprogramma voor importeren/exporteren voor het voorbereiden van uw schijven. Dit verwijst naar v1 van het hulpprogramma voor importeren/exporteren.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: e8541695-bcfb-4bf0-84d9-72c9de32a0a8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 77bdaa5559de86cd1de9f30e70656e47fd5719e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="setting-properties-and-metadata-during-the-import-process"></a>Eigenschappen en metagegevens instellen tijdens het importproces
Wanneer u het hulpprogramma Microsoft Azure Import/Export voorbereiden van uw schijven uitvoert, kunt u eigenschappen en metagegevens worden ingesteld op de doel-blobs. Volg deze stappen:  
  
1.  Maak een tekstbestand op uw lokale computer waarmee u de namen en waarden blob eigenschappen ingesteld.  
  
2.  Maak een tekstbestand op uw lokale computer waarmee metagegevens namen en waarden blobmetagegevens stelt.  
  
3.  Het volledige pad doorgeven aan een of beide van deze bestanden naar de Azure-hulpprogramma voor importeren/exporteren als onderdeel van de `PrepImport` bewerking.  
  
> [!NOTE]
>  Wanneer u een bestand eigenschappen of metagegevens als onderdeel van een sessie exemplaar opgeeft, worden deze eigenschappen of metagegevens zijn ingesteld voor elke blob die is geïmporteerd als onderdeel van die sessie kopiëren. Als u opgeven van een andere set eigenschappen of metagegevens voor enkele van de blobs wordt geïmporteerd wilt, moet u een afzonderlijk exemplaar om sessie te maken met andere eigenschappen of bestanden met metagegevens.  
  
## <a name="specify-blob-properties-in-a-text-file"></a>Blob-eigenschappen opgeven in een tekstbestand  
Blob als eigenschappen wilt opgeven, een lokaal tekstbestand maken en XML-bestand dat wordt opgegeven namen van eigenschappen als elementen en eigenschapswaarden als waarden bevatten. Hier volgt een voorbeeld waarin sommige eigenschapswaarden:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
Sla het bestand op een lokale locatie zoals `C:\WAImportExport\ImportProperties.txt`.  
  
## <a name="specify-blob-metadata-in-a-text-file"></a>Blobmetagegevens opgeven in een tekstbestand  
Op dezelfde manier als wilt opgeven blobmetagegevens, maakt u een lokaal tekstbestand met namen van metagegevens als elementen en metagegevenswaarden als waarden. Hier volgt een voorbeeld waarin enkele metagegevenswaarden:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
Sla het bestand op een lokale locatie zoals `C:\WAImportExport\ImportMetadata.txt`.  
  
## <a name="create-a-copy-session-including-the-properties-or-metadata-files"></a>Een kopieersessie met inbegrip van de eigenschappen of bestanden met metagegevens maken  
Wanneer u het hulpprogramma Azure Import/Export voorbereiden van de import-taak uitvoert, geeft u het eigenschappenbestand op de opdrachtregel met behulp van de `PropertyFile` parameter. Geef het metagegevensbestand op de opdrachtregel met behulp van de `/MetadataFile` parameter. Hier volgt een voorbeeld waarin beide bestanden:  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="next-steps"></a>Volgende stappen

* [Bestandsindeling van het metagegevens- en eigenschappenbestand van de Import/Export-service](../storage-import-export-file-format-metadata-and-properties.md)
