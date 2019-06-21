---
title: Azure Data Box-schijf oplossen van problemen met gegevens uploaden problemen met behulp van Logboeken | Microsoft Docs
description: Hierin wordt beschreven hoe u de logboeken en oplossen van problemen tijdens het uploaden van gegevens naar Azure Data Box-schijf.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/14/2019
ms.author: alkohli
ms.openlocfilehash: 6f3ac38c3eac968bd2f7ec2aada435466d3ff279
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148307"
---
# <a name="understand-logs-to-troubleshoot-data-upload-issues-in-azure-data-box-disk"></a>Inzicht in de logboeken voor het oplossen van problemen met het uploaden van gegevens in Azure Data Box-schijf

In dit artikel is van toepassing op Microsoft Azure Data Box-schijf en worden de problemen beschreven die u ziet wanneer u gegevens naar Azure uploadt.

## <a name="data-upload-logs"></a>Logboeken voor het uploaden van gegevens

Wanneer de gegevens wordt geüpload naar Azure in het datacenter `_error.xml` en `_verbose.xml` bestanden worden gegenereerd. Deze logboeken zijn geüpload naar de storage-account dat is gebruikt om gegevens te uploaden. Een voorbeeld van de `_error.xml` wordt hieronder weergegeven.
    
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

## <a name="download-logs"></a>Logboeken downloaden

Er zijn twee manieren om te zoeken en downloaden van de logboeken met diagnostische gegevens.

- Als er fouten optreden tijdens het uploaden van de gegevens naar Azure, wordt een pad naar de map waar de diagnostics-logboeken zich bevinden in de portal weergegeven.

    ![Koppeling naar Logboeken in de portal](./media/data-box-disk-troubleshoot-upload/data-box-disk-portal-logs.png)

- Ga naar het opslagaccount dat is gekoppeld aan uw Data Box-order. Ga naar **Blob-service > Blobs verkennen** en zoek naar de blob die overeenkomt met het opslagaccount. Ga naar **waies**.

    ![Logboeken kopiëren 2](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs2.png)

In elk geval ziet u de foutenlogboeken van de en de uitgebreide Logboeken. Selecteer elk logboek en downloaden van een lokale kopie.


## <a name="data-upload-errors"></a>Fouten bij het uploaden van gegevens

De fouten die zijn gegenereerd tijdens het uploaden van de gegevens naar Azure worden samengevat in de volgende tabel.

| Foutcode | Description                        |
|-------------|------------------------------|
|`None` |  Is voltooid.           |
|`Renamed` | De naam van de blob is gewijzigd.  |                                                            |
|`CompletedWithErrors` | Uploaden is voltooid met fouten. De details van de bestanden in de fout zijn opgenomen in het logboekbestand.  |
|`Corrupted`|CRC berekend gedurende de gegevensopname komt niet overeen met de CRC die is berekend tijdens het uploaden.  |  
|`StorageRequestFailed` | Azure storage-aanvraag is mislukt.   |     |
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
