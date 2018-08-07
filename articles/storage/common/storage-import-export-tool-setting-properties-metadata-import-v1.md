---
title: Eigenschappen en metagegevens met behulp van Azure Import/Export - v1 instellen | Microsoft Docs
description: Informatie over het opgeven van eigenschappen en metagegevens worden ingesteld op de doel-blobs bij het uitvoeren van de Azure Import/Export-hulpprogramma voor het voorbereiden van uw schijven. Dit verwijst naar de V1-versie van het hulpprogramma Import/Export.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 9a27bae49b9a626c46440392fee03f14c403fe76
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39520697"
---
# <a name="setting-properties-and-metadata-during-the-import-process"></a>Eigenschappen en metagegevens instellen tijdens het importproces
Wanneer u de Microsoft Azure Import/Export-hulpprogramma voor het voorbereiden van uw schijven uitvoert, kunt u eigenschappen en metagegevens zijn ingesteld voor de doel-blobs. Volg deze stappen:  
  
1.  Om in te stellen de blob-eigenschappen, maak een tekstbestand op uw lokale computer die Hiermee geeft u de namen van eigenschappen en waarden.  
  
2.  Om metagegevens van de blob, maak een tekstbestand op uw lokale computer die Hiermee geeft u de metagegevens van namen en waarden.  
  
3.  Het volledige pad doorgeven aan een of beide van deze bestanden naar de Azure Import/Export-hulpprogramma als onderdeel van de `PrepImport` bewerking.  
  
> [!NOTE]
>  Wanneer u een bestand eigenschappen of metagegevens als onderdeel van een kopieersessie opgeeft, worden deze eigenschappen of metagegevens zijn ingesteld voor elke blob die wordt geïmporteerd als onderdeel van die sessie kopiëren. Als u opgeven van een andere set eigenschappen en metagegevens voor een aantal van de blobs worden geïmporteerd wilt, moet u een afzonderlijk exemplaar om sessie te maken met verschillende eigenschappen of bestanden met metagegevens.  
  
## <a name="specify-blob-properties-in-a-text-file"></a>Blobeigenschappen opgeven in een tekstbestand  
Als u wilt opgeven van eigenschappen van de blob, maak een lokale tekstbestand en XML-bestand dat Hiermee geeft u de namen van eigenschappen als elementen en eigenschapswaarden als waarden bevatten. Hier volgt een voorbeeld waarin sommige eigenschapswaarden:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
Sla het bestand op een lokale locatie, zoals `C:\WAImportExport\ImportProperties.txt`.  
  
## <a name="specify-blob-metadata-in-a-text-file"></a>Metagegevens van de Blob opgeven in een tekstbestand  
Op dezelfde manier om op te geven de metagegevens van de blob, maakt u een lokale tekstbestand dat Hiermee geeft u de namen van metagegevens als elementen en metagegevenswaarden als waarden. Hier volgt een voorbeeld waarin enkele metagegevenswaarden:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
Sla het bestand op een lokale locatie, zoals `C:\WAImportExport\ImportMetadata.txt`.  
  
## <a name="create-a-copy-session-including-the-properties-or-metadata-files"></a>Een kopieersessie met inbegrip van de eigenschappen of bestanden met metagegevens maken  
Wanneer u het hulpprogramma Azure Import/Export om voor te bereiden van de import-taak uitvoert, geeft u het eigenschappenbestand op de opdrachtregel met behulp de `PropertyFile` parameter. Geef het bestand met metagegevens over het gebruik van de opdrachtregel de `/MetadataFile` parameter. Hier volgt een voorbeeld waarin beide bestanden:  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="next-steps"></a>Volgende stappen

* [Bestandsindeling van het metagegevens- en eigenschappenbestand van de Import/Export-service](../storage-import-export-file-format-metadata-and-properties.md)
