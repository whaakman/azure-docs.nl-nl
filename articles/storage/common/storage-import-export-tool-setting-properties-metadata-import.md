---
title: Eigenschappen en metagegevens met behulp van Azure Import/Export instellen | Microsoft Docs
description: Informatie over het opgeven van eigenschappen en metagegevens worden ingesteld op de doel-blobs bij het uitvoeren van de Azure Import/Export-hulpprogramma voor het voorbereiden van uw schijven.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 3e2796d943fbc4a4ce99576de74b2fc5c77bcca9
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55459448"
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

## <a name="specify-blob-metadata-in-a-text-file"></a>Metagegevens van de blob opgeven in een tekstbestand

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

## <a name="add-the-path-to-properties-and-metadata-files-in-datasetcsv"></a>Het pad naar eigenschappen en van metagegevensbestanden in dataset.csv toevoegen

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,https://mystorageaccount.blob.core.windows.net/video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,https://mystorageaccount.blob.core.windows.net/photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,https://mystorageaccount.blob.core.windows.net/music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

## <a name="next-steps"></a>Volgende stappen

* [Bestandsindeling van het metagegevens- en eigenschappenbestand van de Import/Export-service](../storage-import-export-file-format-metadata-and-properties.md)
