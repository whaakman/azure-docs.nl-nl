---
title: Azure Data Box-schijf oplossen van problemen met gegevens uploaden problemen met behulp van Logboeken | Microsoft Docs
description: Hierin wordt beschreven hoe u de logboeken en oplossen van problemen tijdens het uploaden van gegevens naar Azure Data Box-schijf.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/17/2019
ms.author: alkohli
ms.openlocfilehash: deaa9a220ee4d765650779b40742225e300ffdb7
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807505"
---
# <a name="understand-logs-to-troubleshoot-data-upload-issues-in-azure-data-box-disk"></a>Inzicht in de logboeken voor het oplossen van problemen met het uploaden van gegevens in Azure Data Box-schijf

In dit artikel is van toepassing op Microsoft Azure Data Box-schijf en worden de problemen beschreven die u ziet wanneer u gegevens naar Azure uploadt.

## <a name="about-upload-logs"></a>Over het uploaden van Logboeken

Wanneer de gegevens wordt geüpload naar Azure in het datacenter `_error.xml` en `_verbose.xml` bestanden worden gegenereerd voor elke storage-account. Deze logboeken zijn geüpload naar de storage-account dat is gebruikt om gegevens te uploaden. 

Zowel de logboeken zich in dezelfde indeling en bevatten beschrijvingen van de XML van de gebeurtenissen die zijn opgetreden tijdens het kopiëren van de gegevens van de schijf met de Azure Storage-account.

Het uitgebreide logboek bevat informatie over de status van de kopieerbewerking voor elke blob of file, terwijl het foutenlogboek bevat alleen de gegevens voor blobs of bestanden die zijn fouten tijdens het uploaden aangetroffen.

Het foutenlogboek heeft dezelfde structuur als de uitgebreide, maar uitgefilterd voltooide bewerkingen.

## <a name="download-logs"></a>Logboeken downloaden

De volgende stappen om te vinden van de logboeken uploaden.

1. Als er fouten optreden tijdens het uploaden van de gegevens naar Azure, wordt een pad naar de map waar de diagnostics-logboeken zich bevinden in de portal weergegeven.

    ![Koppeling naar Logboeken in de portal](./media/data-box-disk-troubleshoot-upload/data-box-disk-portal-logs.png)

2. Ga naar **waies**.

    ![fout- en uitgebreide Logboeken](./media/data-box-disk-troubleshoot-upload/data-box-disk-portal-logs-1.png)

In elk geval ziet u de foutenlogboeken van de en de uitgebreide Logboeken. Selecteer elk logboek en downloaden van een lokale kopie.

## <a name="sample-upload-logs"></a>Logboeken van voorbeeldgegevens uploaden

Een voorbeeld van de `_verbose.xml` wordt hieronder weergegeven. In dit geval wordt is de volgorde voltooid zonder fouten.

```xml

<?xml version="1.0" encoding="utf-8"?>
<DriveLog Version="2018-10-01">
  <DriveId>184020D95632</DriveId>
  <Blob Status="Completed">
    <BlobPath>$root/botetapageblob.vhd</BlobPath>
    <FilePath>\PageBlob\botetapageblob.vhd</FilePath>
    <Length>1073742336</Length>
    <ImportDisposition Status="Created">rename</ImportDisposition>
    <PageRangeList>
      <PageRange Offset="0" Length="4194304" Status="Completed" />
      <PageRange Offset="4194304" Length="4194304" Status="Completed" />
      <PageRange Offset="8388608" Length="4194304" Status="Completed" />
      --------CUT-------------------------------------------------------
      <PageRange Offset="1061158912" Length="4194304" Status="Completed" />
      <PageRange Offset="1065353216" Length="4194304" Status="Completed" />
      <PageRange Offset="1069547520" Length="4194304" Status="Completed" />
      <PageRange Offset="1073741824" Length="512" Status="Completed" />
    </PageRangeList>
  </Blob>
  <Blob Status="Completed">
    <BlobPath>$root/botetablockblob.txt</BlobPath>
    <FilePath>\BlockBlob\botetablockblob.txt</FilePath>
    <Length>19</Length>
    <ImportDisposition Status="Created">rename</ImportDisposition>
    <BlockList>
      <Block Offset="0" Length="19" Status="Completed" />
    </BlockList>
  </Blob>
  <File Status="Completed">
    <FileStoragePath>botetaazurefilesfolder/botetaazurefiles.txt</FileStoragePath>
    <FilePath>\AzureFile\botetaazurefilesfolder\botetaazurefiles.txt</FilePath>
    <Length>20</Length>
    <ImportDisposition Status="Created">rename</ImportDisposition>
    <FileRangeList>
      <FileRange Offset="0" Length="20" Status="Completed" />
    </FileRangeList>
  </File>
  <Status>Completed</Status>
</DriveLog>
```

Voor dezelfde volgorde, een voorbeeld van de `_error.xml` wordt hieronder weergegeven.

```xml

<?xml version="1.0" encoding="utf-8"?>
<DriveLog Version="2018-10-01">
  <DriveId>184020D95632</DriveId>
  <Summary>
    <ValidationErrors>
      <None Count="3" />
    </ValidationErrors>
    <CopyErrors>
      <None Count="3" Description="No errors encountered" />
    </CopyErrors>
  </Summary>
  <Status>Completed</Status>
</DriveLog>
```

Een voorbeeld van de `_error.xml` hieronder waar u de volgorde is voltooid met fouten. 

Het foutenbestand in dit geval heeft een `Summary` en een andere sectie waarin alle bestand niveau fouten. 

De `Summary` bevat de `ValidationErrors` en de `CopyErrors`. In dit geval 8 bestanden of mappen zijn geüpload naar Azure en er zijn geen validatiefouten. Wanneer de gegevens zijn gekopieerd naar Azure Storage-account, 5-bestanden of mappen is geüpload. De resterende 3-bestanden of mappen die zijn gewijzigd volgens de naamgevingsregels van Azure container en vervolgens geüpload naar Azure.

De status voor het bestand zich in `BlobStatus` met de beschrijving van alle acties voor het uploaden van de blobs. Drie containers worden in dit geval worden gewijzigd omdat de mappen waarnaar de gegevens zijn gekopieerd niet aan de Azure naamgevingsregels voor containers voldoet. Voor de blobs die in deze containers zijn geüpload, zijn de nieuwe naam, pad van de blob in Azure, oorspronkelijke pad is ongeldig en de blobgrootte zijn opgenomen.
    
```xml
 <?xml version="1.0" encoding="utf-8"?>
    <DriveLog Version="2018-10-01">
      <DriveId>18041C582D7E</DriveId>
      <Summary>
     <!--Summary for validation and data copy to Azure -->
        <ValidationErrors>
          <None Count="8" />
        </ValidationErrors>
        <CopyErrors>
          <Completed Count="5" Description="No errors encountered" />
          <ContainerRenamed Count="3" Description="Renamed the container as the original container name does not follow Azure conventions." />
        </CopyErrors>
      </Summary>
    <!--List of renamed containers with the new names, new file path in Azure, original invalid file path, and size -->
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/private.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\private.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/resource.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\resource.vhd</OriginalFilePath>
        <SizeInBytes>71528448</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/role.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\role.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Status>CompletedWithErrors</Status>
    </DriveLog>
```

## <a name="data-upload-errors"></a>Fouten bij het uploaden van gegevens

De fouten die zijn gegenereerd tijdens het uploaden van de gegevens naar Azure worden samengevat in de volgende tabel.

| Foutcode | Description                   |
|-------------|------------------------------|
|`None` |  Is voltooid.           |
|`Renamed` | De naam van de blob is gewijzigd.   |
|`CompletedWithErrors` | Uploaden is voltooid met fouten. De details van de bestanden in de fout zijn opgenomen in het logboekbestand.  |
|`Corrupted`|CRC berekend gedurende de gegevensopname komt niet overeen met de CRC die is berekend tijdens het uploaden.  |  
|`StorageRequestFailed` | Azure storage-aanvraag is mislukt.   |     
|`LeasePresent` | Dit item is in lease gegeven en wordt gebruikt door een andere gebruiker. |
|`StorageRequestForbidden` |Kan niet uploaden vanwege problemen met de verificatie. |
|`ManagedDiskCreationTerminalFailure` | Kan niet uploaden als beheerde schijven. De bestanden zijn beschikbaar in het tijdelijke opslagaccount dat als pagina-blobs. U kunt handmatig pagina-blobs converteren naar beheerde schijven.  |
|`DiskConversionNotStartedTierInfoMissing` | Omdat het VHD-bestand werd gekopieerd buiten de mappen precreated laag, is niet een beheerde schijf gemaakt. Het bestand is geüpload als pagina-blob naar de staging storage-account opgegeven tijdens het maken van de order. U kunt dit handmatig converteren naar een beheerde schijf.|
|`InvalidWorkitem` | Kan de gegevens niet laden omdat deze niet met het Azure naamgeving overeen komt en conventies beperkt.|
|`InvalidPageBlobUploadAsBlockBlob` | Geüpload als blok-blobs in een container met het voorvoegsel `databoxdisk-invalid-pb-`.|
|`InvalidAzureFileUploadAsBlockBlob` | Geüpload als blok-blobs in een container met het voorvoegsel `databoxdisk-invalid-af`-.|
|`InvalidManagedDiskUploadAsBlockBlob` | Geüpload als blok-blobs in een container met het voorvoegsel `databoxdisk-invalid-md`-.|
|`InvalidManagedDiskUploadAsPageBlob` |Uploaden als pagina-blobs in een container met het voorvoegsel `databoxdisk-invalid-md-`. |
|`MovedToOverflowShare` |Geüploade bestanden naar een nieuwe share als de oorspronkelijke grootte van de bestandsshare is de maximale grootte van de Azure-limiet overschreden. De nieuwe naam van bestandsshare is de naam van de oorspronkelijke voorafgegaan door `-2`.   |
|`MovedToDefaultAzureShare` |Geüploade bestanden die niet onderdeel van een map naar een Standaardshare zijn. De sharenaam begint met `databox-`. |
|`ContainerRenamed` |De container voor deze bestanden niet voldoen aan de naamgevingsconventies voor Azure en is gewijzigd. De nieuwe naam begint met `databox-` en wordt voorafgegaan door de SHA1-hash van de oorspronkelijke naam |
|`ShareRenamed` |De share voor deze bestanden niet voldoen aan de naamgevingsconventies voor Azure en is gewijzigd. De nieuwe naam begint met `databox-` en wordt voorafgegaan door de SHA1-hash van de oorspronkelijke naam. |
|`BlobRenamed` |Deze bestanden niet voldoen aan de naamgevingsconventies voor Azure en zijn gewijzigd. Controleer de `BlobPath` veld voor de nieuwe naam. |
|`FileRenamed` |Deze bestanden niet voldoen aan de naamgevingsconventies voor Azure en zijn gewijzigd. Controleer de `FileStoragePath` veld voor de nieuwe naam. |
|`DiskRenamed` |Deze bestanden niet voldoen aan de naamgevingsconventies voor Azure en zijn gewijzigd. Controleer de `BlobPath` veld voor de nieuwe naam. |


## <a name="next-steps"></a>Volgende stappen

- [Open een ondersteuningsticket voor problemen met Data Box-schijf](data-box-disk-contact-microsoft-support.md).
