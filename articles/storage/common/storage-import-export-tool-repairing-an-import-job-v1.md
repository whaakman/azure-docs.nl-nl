---
title: Een importtaak Azure Import/Export - v1 herstellen | Microsoft Docs
description: Informatie over het herstellen van een import-taak die is gemaakt en uitgevoerd met behulp van de Azure Import/Export-service.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: fda1d3d626c91ba984f08b96c79ab6a2fd2ec74b
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55471729"
---
# <a name="repairing-an-import-job"></a>Een importtaak herstellen
De Microsoft Azure Import/Export-service kan niet worden enkele van uw bestanden of delen van een bestand kopiëren naar de Windows Azure Blob-service. Enkele redenen voor fouten zijn onder andere:  
  
-   Beschadigde bestanden  
  
-   Beschadigde stations  
  
-   De opslagaccountsleutel gewijzigd terwijl het bestand is overgebracht.  
  
U kunt uitvoeren de Microsoft Azure Import/Export-hulpprogramma met het importeren van taak kopiëren-logboekbestanden en het hulpprogramma uploadt de ontbrekende bestanden (of delen van een bestand) naar uw Windows Azure storage-account om de importtaak te voltooien.  
  
## <a name="repairimport-parameters"></a>RepairImport parameters

Kunnen de volgende parameters worden opgegeven met **RepairImport**: 
  
|||  
|-|-|  
|**r:**< RepairFile\>|**Vereist.** Pad naar het bestand herstellen, die de voortgang van het herstel, en kunt u doorgaan met de reparatie van een onderbroken. Elk station moet slechts één herstel-bestand hebben. Wanneer u een herstel voor een bepaald station start, worden in het pad doorgeven naar een bestand herstellen, die nog niet bestaat. Als u wilt de reparatie van een onderbroken hervatten, moet u de naam van een bestaand herstel-bestand doorgeven. De herstel-bestand dat overeenkomt met de doel-station moet altijd worden opgegeven.|  
|**/logdir:**<LogDirectory\>|**Optioneel.** De logboekmap. Uitgebreid logboek van de bestanden worden naar deze map geschreven. Als er geen logboekmap is opgegeven, wordt de huidige map wordt gebruikt als de logboekmap.|  
|**/d:**<TargetDirectories\>|**Vereist.** Een of meer gescheiden door puntkomma's mappen met de oorspronkelijke bestanden die zijn geïmporteerd. De import-schijf kan ook worden gebruikt, maar is niet vereist als alternatieve locaties van de oorspronkelijke bestanden beschikbaar zijn.|  
|**/bk:**<BitLockerKey\>|**Optioneel.** Als u wilt dat het hulpprogramma voor het ontgrendelen van een versleuteld station waar de oorspronkelijke bestanden beschikbaar zijn, moet u de BitLocker-sleutel opgeven.|  
|**/sn:**<StorageAccountName\>|**Vereist.** De naam van het opslagaccount voor de import-taak.|  
|**/sk:**<StorageAccountKey\>|**Vereiste** als en alleen als een container SAS is niet opgegeven. De accountsleutel voor het opslagaccount voor de import-taak.|  
|**/csas:**<ContainerSas\>|**Vereiste** alleen als de toegangssleutel van het opslagaccount is niet opgegeven. De container SAS voor toegang tot de blobs die zijn gekoppeld aan de import-taak.|  
|**/CopyLogFile:**<DriveCopyLogFile\>|**Vereist.** Pad naar het station kopiëren-logboekbestand (uitgebreid logboek of de fout aanmelden). Het bestand is gegenereerd door de Windows Azure Import/Export-service en kan worden gedownload van de blob-opslag die is gekoppeld aan de taak. De kopie-logboekbestand bevat informatie over mislukte blobs of bestanden die moeten worden hersteld.|  
|**/PathMapFile:**<DrivePathMapFile\>|**Optioneel.** Pad naar een tekstbestand dat kan worden gebruikt voor het oplossen van dubbelzinnigheden hebt u meerdere bestanden met dezelfde naam die u in de dezelfde taak zijn importeren. De eerste keer dat het hulpprogramma wordt uitgevoerd, kunt dit bestand met alle namen van niet-eenduidige invullen. Volgende uitvoeringen van het hulpprogramma voor dit bestand gebruiken om op te lossen de dubbelzinnigheden.|  
  
## <a name="using-the-repairimport-command"></a>Gebruik de opdracht RepairImport  
Als u wilt herstellen gegevens importeren met het streamen van de gegevens via het netwerk, moet u de mappen met de oorspronkelijke bestanden zijn u importeren met behulp van de `/d` parameter. U moet ook het bestand kopiëren die u hebt gedownload van uw opslagaccount opgeven. Een typische vanaf de opdrachtregel om te herstellen van een import-taak met gedeeltelijke fouten ziet eruit zoals:  
  
```  
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log  
```  
  
In het volgende voorbeeld van een logbestand kopiëren is een 64-K-deel van een bestand op het station dat is verzonden voor de import-taak beschadigd. Aangezien dit de enige fout aangegeven, zijn de rest van de blobs in de taak zijn geïmporteerd.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
 <DriveId>9WM35C2V</DriveId>  
 <Blob Status="CompletedWithErrors">  
 <BlobPath>pictures/animals/koala.jpg</BlobPath>  
 <FilePath>\animals\koala.jpg</FilePath>  
 <Length>163840</Length>  
 <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
 <PageRangeList>  
  <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted" />  
 </PageRangeList>  
 </Blob>  
 <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
Wanneer dit logboek kopiëren wordt doorgegeven aan het hulpprogramma Azure Import/Export, wil het hulpprogramma voor het importeren van dit bestand voltooien door het kopiëren van de ontbrekende inhoud via het netwerk. Volgens het bovenstaande voorbeeld het hulpprogramma zoekt naar het oorspronkelijke bestand `\animals\koala.jpg` binnen de twee mappen `C:\Users\bob\Pictures` en `X:\BobBackup\photos`. Als het bestand `C:\Users\bob\Pictures\animals\koala.jpg` bestaat, het hulpprogramma Azure Import/Export kopieert het ontbrekende bereik van gegevens naar de bijbehorende blob `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`.  
  
## <a name="resolving-conflicts-when-using-repairimport"></a>Het oplossen van conflicten bij het gebruik van RepairImport  
In sommige gevallen, het hulpprogramma mogelijk niet vinden of openen van het vereiste bestand voor een van de volgende redenen: het bestand kan niet worden gevonden, het bestand is niet toegankelijk, de bestandsnaam is niet eenduidig of de inhoud van het bestand is niet meer correct.  
  
Een niet-eenduidige fout kan optreden als het hulpprogramma is bij het vinden van `\animals\koala.jpg` en er is een bestand met die naam onder beide `C:\Users\bob\pictures` en `X:\BobBackup\photos`. Dat wil zeggen, beide `C:\Users\bob\pictures\animals\koala.jpg` en `X:\BobBackup\photos\animals\koala.jpg` aanwezig zijn op de schijven van de taak importeren.  
  
De `/PathMapFile` optie kunt u deze oplossen. U kunt de naam van het bestand bevat de lijst met bestanden die het hulpprogramma kan niet correct kan worden geïdentificeerd. Voorbeeld van de volgende opdrachtregel worden gevuld `9WM35C2V_pathmap.txt`:  
  
```
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log /PathMapFile:C:\WAImportExport\9WM35C2V_pathmap.txt  
```
  
Het hulpprogramma schrijft vervolgens de problematische bestandspaden aan `9WM35C2V_pathmap.txt`, één op elke regel. Het bestand kan bijvoorbeeld de volgende vermeldingen bevatten na het uitvoeren van de opdracht:  
 
```
\animals\koala.jpg  
\animals\kangaroo.jpg  
```
  
 Voor elk bestand in de lijst, moet u proberen om te zoeken en open het bestand om te controleren of dat deze beschikbaar is om het hulpprogramma. Als u zien van het hulpprogramma expliciet waar vind ik een bestand wilt, kunt u het pad naar map bestand wijzigen en het pad toevoegen aan elk bestand in dezelfde regel, gescheiden door een tabteken:  
  
```
\animals\koala.jpg           C:\Users\bob\Pictures\animals\koala.jpg  
\animals\kangaroo.jpg        X:\BobBackup\photos\animals\kangaroo.jpg  
```
  
Na het maken van de benodigde bestanden beschikbaar zijn voor het hulpprogramma of bijwerken van toewijzingsbestand van het pad, kunt u het hulpprogramma voor het voltooien van het importproces opnieuw uitvoeren.  
  
## <a name="next-steps"></a>Volgende stappen
 
* [Het hulpprogramma Azure Import/Export instellen](storage-import-export-tool-setup-v1.md)   
* [Harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [De taakstatus controleren met kopielogboekbestanden](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Een exporttaak herstellen](../storage-import-export-tool-repairing-an-export-job-v1.md)   
* [Problemen met het hulpprogramma Azure Import/Export oplossen](storage-import-export-tool-troubleshooting-v1.md)
