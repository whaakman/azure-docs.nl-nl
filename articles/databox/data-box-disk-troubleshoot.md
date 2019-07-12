---
title: Problemen in Azure Data Box Disk oplossen | Microsoft Docs
description: Hierin wordt beschreven hoe u problemen in Azure Data Box Disk oplost.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/14/2019
ms.author: alkohli
ms.openlocfilehash: f8116ec0836623adf803991017950ddc7f960923
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67805713"
---
# <a name="use-logs-to-troubleshoot-validation-issues-in-azure-data-box-disk"></a>Logboeken gebruiken om op te lossen validatieproblemen in Azure Data Box-schijf

In dit artikel is van toepassing op Microsoft Azure Data Box-schijf. Het artikel wordt beschreven hoe u de Logboeken gebruiken om op te lossen de validatieproblemen die u zien kunt wanneer u deze oplossing implementeert.

## <a name="validation-tool-log-files"></a>Validatie hulpprogramma-logboekbestanden

Wanneer het valideren van de gegevens op de schijven die met behulp van de [hulpprogramma voor het valideren](data-box-disk-deploy-copy-data.md#validate-data), een *error.xml* voor logboekregistratie van fouten wordt gegenereerd. Het logboekbestand bevindt zich in de `Drive:\DataBoxDiskImport\logs` map van het station. Een koppeling naar het foutenlogboek wordt verstrekt wanneer u validatie uitvoert.

<!--![Validation tool with link to error log](media/data-box-disk-troubleshoot/validation-tool-link-error-log.png)-->

Als u meerdere sessies voor validatie uitvoert, wordt een foutenlogboek gegenereerd per sessie.

- Hier volgt een voorbeeld van het foutenlogboek wanneer de gegevens geladen in de `PageBlob` map is niet uitgelijnd 512-bytes. Geen gegevens geüpload naar PageBlob moet 512-bytes zijn uitgelijnd, bijvoorbeeld een VHD of VHDX. De fouten in dit bestand zich in de `<Errors>` en waarschuwingen in `<Warnings>`.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <ErrorLog Version="2018-10-01">
            <SessionId>session#1</SessionId>
            <ItemType>PageBlob</ItemType>
            <SourceDirectory>D:\Dataset\TestDirectory</SourceDirectory>
            <Errors>
                <Error Code="Not512Aligned">
                    <Description>The file is not 512 bytes aligned.</Description>
                    <List>
                        <File Path="\Practice\myScript.ps1" />
                    </List>
                    <Count>1</Count>
                </Error>
            </Errors>
            <Warnings />
        </ErrorLog>
    ```

- Hier volgt een voorbeeld van het foutlogboek doorgeven als de containernaam van de niet geldig is. De map die u maakt onder `BlockBlob`, `PageBlob`, of `AzureFile` mappen op de schijf wordt een container in uw Azure Storage-account. De naam van de container moet voldoen aan de [Azure naamgevingsregels](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions).

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <ErrorLog Version="2018-10-01">
          <SessionId>bbsession</SessionId>
          <ItemType>BlockBlob</ItemType>
          <SourceDirectory>E:\BlockBlob</SourceDirectory>
          <Errors>
            <Error Code="InvalidShareContainerFormat">
              <List>
                <Container Name="Azu-reFile" />
                <Container Name="bbcont ainer1" />
              </List>
              <Count>2</Count>
            </Error>
          </Errors>
          <Warnings />
    </ErrorLog>
    ```

## <a name="validation-tool-errors"></a>Validatiefouten hulpprogramma

De fouten die zijn opgenomen in de *error.xml* met de bijbehorende aanbevolen acties worden samengevat in de volgende tabel.

| Foutcode| Description                       | Aanbevolen acties               |
|------------|--------------------------|-----------------------------------|
| `None` | De gegevens is gevalideerd. | Geen actie vereist. |
| `InvalidXmlCharsInPath` |Kan een manifestbestand niet maken omdat het bestandspad bevat tekens die ongeldig zijn. | Verwijder deze tekens om door te gaan.  |
| `OpenFileForReadFailed`| Kan het bestand niet verwerken. Dit kan zijn vanwege een toegang probleem of bestand system is beschadigd.|Kan het bestand vanwege een fout niet lezen. Details van de fout zich in de uitzondering. |
| `Not512Aligned` | Dit bestand is geen geldige indeling voor PageBlob-map.| Alleen uploadgegevens die 512 bytes is afgestemd op `PageBlob` map. Het bestand uit de map PageBlob verwijderen of verplaatsen naar de map BlockBlob. De validatie opnieuw.|
| `InvalidBlobPath` | Bestandspad toewijzen niet aan een geldige blob-pad in de cloud aan de hand van de Azure-Blob naamconventies.|Volg de richtlijnen voor de Azure naamgeving om de naam van het bestandspad te. |
| `EnumerationError` | Kan het bestand voor de validatie niet opsommen. |Er zijn mogelijk meerdere redenen voor deze fout. Een meest waarschijnlijke reden hiervoor is toegang tot het bestand. |
| `ShareSizeExceeded` | Dit bestand veroorzaakt de grootte van de Azure bestandsshare naar de Azure-limiet van 5 TB.|Reduceer de grootte van de gegevens in de share, zodat het voldoet aan de [Omvangslimieten voor Azure-object](data-box-disk-limits.md#azure-object-size-limits). De validatie opnieuw. |
| `AzureFileSizeExceeded` | De bestandsgrootte overschrijdt de grenzen van de grootte van Azure-bestand.| De grootte van het bestand of de gegevens verkleinen zodat het voldoet aan de [Omvangslimieten voor Azure-object](data-box-disk-limits.md#azure-object-size-limits). De validatie opnieuw.|
| `BlockBlobSizeExceeded` | De bestandsgrootte overschrijdt de grenzen van de grootte van Azure blok-Blob. | De grootte van het bestand of de gegevens verkleinen zodat het voldoet aan de [Omvangslimieten voor Azure-object](data-box-disk-limits.md#azure-object-size-limits). De validatie opnieuw. |
| `ManagedDiskSizeExceeded` | De bestandsgrootte overschrijdt de grenzen van de grootte van door Azure beheerde schijf. | De grootte van het bestand of de gegevens verkleinen zodat het voldoet aan de [Omvangslimieten voor Azure-object](data-box-disk-limits.md#azure-object-size-limits). De validatie opnieuw. |
| `PageBlobSizeExceeded` | De bestandsgrootte overschrijdt de grenzen van de grootte van door Azure beheerde schijf. | De grootte van het bestand of de gegevens verkleinen zodat het voldoet aan de [Omvangslimieten voor Azure-object](data-box-disk-limits.md#azure-object-size-limits). De validatie opnieuw. |
| `InvalidShareContainerFormat`  |De namen van mappen komen niet overeen met Azure voor naamgeving van containers of shares.         |De eerste map die is gemaakt op basis van de vooraf bestaande mappen op de schijf wordt een container in uw storage-account. De naam van deze share of container voldoet niet aan de naamgevingsconventies voor Azure. Naam van het bestand zodat het voldoet aan [Azure naamgevingsregels](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions). De validatie opnieuw.   |
| `InvalidBlobNameFormat` | Bestandspad toewijzen niet aan een geldige blob-pad in de cloud aan de hand van de Azure-Blob naamconventies.|Naam van het bestand zodat het voldoet aan [Azure naamgevingsregels](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions). De validatie opnieuw. |
| `InvalidFileNameFormat` | Bestandspad toewijzen niet aan een geldig pad in de cloud aan de hand van de Azure File naamconventies. |Naam van het bestand zodat het voldoet aan [Azure naamgevingsregels](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions). De validatie opnieuw. |
| `InvalidDiskNameFormat` | Bestandspad toewijzen niet aan de naam van een geldige schijf in de cloud aan de hand van de naamgevingsconventies voor Azure Managed Disk. |Naam van het bestand zodat het voldoet aan [Azure naamgevingsregels](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions). De validatie opnieuw.       |
| `NotPartOfFileShare` | Het uploadpad van bestanden is niet geldig. De bestanden uploaden naar een map in Azure Files.   | Verwijder de bestanden in de fout en deze bestanden uploaden naar een precreated map. De validatie opnieuw. |
| `NonVhdFileNotSupportedForManagedDisk` | Een niet-VHD-bestand kan niet worden geüpload als een beheerde schijf. |Verwijder de niet-VHD-bestanden van `ManagedDisk` map als deze worden niet ondersteund of verplaatsen van deze bestanden op een `PageBlob` map. De validatie opnieuw. |


## <a name="next-steps"></a>Volgende stappen

- Problemen oplossen [gegevens uploadfouten](data-box-disk-troubleshoot-upload.md).
