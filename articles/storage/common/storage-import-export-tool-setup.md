---
title: Instellen van de Azure-hulpprogramma voor importeren/exporteren | Microsoft Docs
description: Informatie over het instellen van de voorbereiding van het station en het hulpprogramma voor de Azure Import/Export-service voor het repareren.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: muralikk
ms.openlocfilehash: 6b5febd051d0b956c90cb14c260dda1881adac3f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="setting-up-the-azure-importexport-tool"></a>Instellen van de Azure-hulpprogramma voor importeren/exporteren

Het hulpprogramma voor importeren/exporteren van Microsoft Azure is het station voorbereiding hulpprogramma en herstel die u met de Microsoft Azure Import/Export-service gebruiken kunt. U kunt het hulpprogramma gebruiken voor de volgende functies:

* Voordat u een import-taak maakt, kunt u dit hulpprogramma gebruiken om gegevens te kopiëren naar de harde schijven die u wilt verzenden naar een Azure-Datacenter.
* Nadat een import-taak is voltooid, kunt u dit hulpprogramma kunt gebruiken om te herstellen van alle bestaande blobs zijn beschadigd, ontbreekt of het conflict met andere blobs.
* Nadat u de stations van een voltooide exporttaak ontvangt, kunt u dit hulpprogramma kunt gebruiken om te herstellen van bestanden die beschadigd of ontbreken op de stations zijn.

## <a name="prerequisites"></a>Vereisten

Als u **voorbereiden stations** voor een import-taak, de volgende vereisten moeten worden voldaan:

* U moet een actief Azure-abonnement hebben.
* Uw abonnement vergezeld gaan van een opslagaccount met voldoende beschikbare ruimte voor het opslaan van de bestanden die u gaat importeren.
* U moet ten minste één van de toegangssleutels voor opslag-account.
* U moet een computer (de ' kopie-machine') met Windows 7, Windows Server 2008 R2 of een nieuwere Windows-besturingssysteem geïnstalleerd.
* .NET Framework 4 moet worden geïnstalleerd op de computer kopiëren.
* BitLocker moet zijn ingeschakeld op de computer kopiëren.
* U moet een of meer 2,5-inch of 3.5-inch SATA of III of SSD harde schijven gekoppeld aan de machine kopiëren leeg.
* De bestanden die u van plan bent om te importeren moet toegankelijk zijn vanaf de computer kopiëren of ze zich op een netwerkshare of een lokale vaste schijf.

Als u te probeert **herstellen van een import** die is gedeeltelijk mislukt, moet u:

* De logboekbestanden kopiëren
* De sleutel van het opslagaccount

Als u te probeert **herstellen exporteren van een** die is gedeeltelijk mislukt, moet u:

* De logboekbestanden kopiëren
* De manifest-bestanden (optioneel)
* De sleutel van het opslagaccount

## <a name="installing-the-azure-importexport-tool"></a>Installeren van het hulpprogramma Azure Import/Export

Eerst [downloaden van de Azure-hulpprogramma voor importeren/exporteren](https://www.microsoft.com/download/details.aspx?id=55280) en pak het bestand naar een map op uw computer, bijvoorbeeld `c:\WAImportExport`.

De Azure-hulpprogramma voor importeren/exporteren bestaat uit de volgende bestanden:

* DataSet.csv
* driveset.csv
* hddid.dll
* Microsoft.Data.Services.Client.dll
* Microsoft.WindowsAzure.Storage.dll
* Microsoft.WindowsAzure.Storage.pdb
* Microsoft.WindowsAzure.Storage.xml
* WAImportExport.exe
* WAImportExport.exe.config
* WAImportExport.pdb
* WAImportExportCore.dll
* WAImportExportCore.pdb
* WAImportExportRepair.dll
* WAImportExportRepair.pdb

Open een opdrachtpromptvenster in vervolgens **beheerdersmodus**, en in de map waarin de uitgepakte bestanden te wijzigen.

Help uitvoer voor de opdracht, voert u het hulpprogramma (`WAImportExport.exe`) zonder parameters:

```
WAImportExport, a client tool for Windows Azure Import/Export Service. Microsoft (c) 2013


Copy directories and/or files with a new copy session:
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>]
        [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>]
        DataSet:<dataset.csv>

Add more drives:
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>

Abort an interrupted copy session:
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AbortSession

Resume an interrupted copy session:
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /ResumeSession

List drives:
    WAImportExport.exe PrepImport /j:<JournalFile> /ListDrives

List copy sessions:
    WAImportExport.exe PrepImport /j:<JournalFile> /ListCopySessions

Repair a Drive:
    WAImportExport.exe RepairImport | RepairExport
        /r:<RepairFile> [/logdir:<LogDirectory>]
        [/d:<TargetDirectories>] [/bk:<BitLockerKey>]
        /sn:<StorageAccountName> /sk:<StorageAccountKey>
        [/CopyLogFile:<DriveCopyLogFile>] [/ManifestFile:<DriveManifestFile>]
        [/PathMapFile:<DrivePathMapFile>]

Preview an Export Job:
    WAImportExport.exe PreviewExport
        [/logdir:<LogDirectory>]
        /sn:<StorageAccountName> /sk:<StorageAccountKey>
        /ExportBlobListFile:<ExportBlobListFile> /DriveSize:<DriveSize>

Parameters:

    /j:<JournalFile>
        - Required. Path to the journal file. A journal file tracks a set of drives and
          records the progress in preparing these drives. The journal file must always
          be specified.
    /logdir:<LogDirectory>
        - Optional. The log directory. Verbose log files as well as some temporary
          files will be written to this directory. If not specified, current directory
          will be used as the log directory. The log directory can be specified only
          once for the same journal file.
    /id:<SessionId>
        - Optional. The session Id is used to identify a copy session. It is used to
          ensure accurate recovery of an interrupted copy session.
    /ResumeSession
        - Optional. If the last copy session was terminated abnormally, this parameter
          can be specified to resume the session.
    /AbortSession
        - Optional. If the last copy session was terminated abnormally, this parameter
          can be specified to abort the session.
    /sn:<StorageAccountName>
        - Required. Only applicable for RepairImport and RepairExport. The name of
          the storage account.
    /sk:<StorageAccountKey>
        - Required. The key of the storage account.
    /InitialDriveSet:<driveset.csv>
        - Required. A .csv file that contains a list of drives to prepare.
    /AdditionalDriveSet:<driveset.csv>
        - Required. A .csv file that contains a list of additional drives to be added.
    /r:<RepairFile>
        - Required. Only applicable for RepairImport and RepairExport.
          Path to the file for tracking repair progress. Each drive must have one
          and only one repair file.
    /d:<TargetDirectories>
        - Required. Only applicable for RepairImport and RepairExport.
          For RepairImport, one or more semicolon-separated directories to repair;
          For RepairExport, one directory to repair, e.g. root directory of the drive.
    /CopyLogFile:<DriveCopyLogFile>
        - Required. Only applicable for RepairImport and RepairExport. Path to the
          drive copy log file (verbose or error).
    /ManifestFile:<DriveManifestFile>
        - Required. Only applicable for RepairExport. Path to the drive manifest file.
    /PathMapFile:<DrivePathMapFile>
        - Optional. Only applicable for RepairImport. Path to the file containing
          mappings of file paths relative to the drive root to locations of actual files
          (tab-delimited). When first specified, it will be populated with file paths
          with empty targets, which means either they are not found in TargetDirectories,
          access denied, with invalid name, or they exist in multiple directories. The
          path map file can be manually edited to include the correct target paths and
          specified again for the tool to resolve the file paths correctly.
    /ExportBlobListFile:<ExportBlobListFile>
        - Required. Path to the XML file containing list of blob paths or blob path
          prefixes for the blobs to be exported. The file format is the same as the
          blob list blob format in the Put Job operation of the Import/Export Service
          REST API.
    /DriveSize:<DriveSize>
        - Required. Size of drives to be used for export. For example, 500GB, 1.5TB.
          Note: 1 GB = 1,000,000,000 bytes
                1 TB = 1,000,000,000,000 bytes
    /DataSet:<dataset.csv>
        - Required. A .csv file that contains a list of directories and/or a list files
          to be copied to target drives.

    /silentmode
        - Optional. If not specified, it will remind you the requirement of drives and
          need your confirmation to continue.

Examples:

    Copy a data set to a drive:
    WAImportExport.exe PrepImport
        /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GEL
        xmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /InitialDriveSet:driveset1.csv
        /DataSet:data.csv

    Copy another dataset to the same drive following the above command:
    WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#2 /DataSet:dataset2.csv

    Preview how many 1.5 TB drives are needed for an export job:
    WAImportExport.exe PreviewExport
        /sn:mytestaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7K
        ysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\temp\myexportbloblist.xml
        /DriveSize:1.5TB

    Repair an finished import job:
    WAImportExport.exe RepairImport
        /r:9WM35C2V.rep /d:X:\ /bk:442926-020713-108086-436744-137335-435358-242242-2795
        98 /sn:mytestaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94
        f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\temp\9WM35C2V_error.log
```

## <a name="next-steps"></a>Volgende stappen

* [Harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import.md)
* [Op voorhand het schijfgebruik voor een exporttaak bekijken](../storage-import-export-tool-previewing-drive-usage-export-v1.md)
* [De taakstatus controleren met kopielogboekbestanden](../storage-import-export-tool-reviewing-job-status-v1.md)
* [Een importtaak herstellen](../storage-import-export-tool-repairing-an-import-job-v1.md)
* [Een exporttaak herstellen](../storage-import-export-tool-repairing-an-export-job-v1.md)
* [Problemen met het hulpprogramma Azure Import/Export oplossen](storage-import-export-tool-troubleshooting-v1.md)
