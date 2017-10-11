---
title: Herstellen van een Azure Import/Export-import-taak - v1 | Microsoft Docs
description: Informatie over het herstellen van een import-taak die is gemaakt en uitgevoerd met behulp van de Azure Import/Export-service.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 38cc16bd-ad55-4625-9a85-e1726c35fd1b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: c837713fd9e7d03287ae5a3644fd6bb47714c9d4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="repairing-an-import-job"></a>Een importtaak herstellen
De Microsoft Azure Import/Export-service kan niet worden enkele van uw bestanden of onderdelen van een bestand kopiëren naar de Windows Azure Blob-service. Redenen voor fouten zijn onder andere:  
  
-   Beschadigde bestanden  
  
-   Beschadigde schijven  
  
-   De opslagaccountsleutel is gewijzigd terwijl het bestand is wordt overgebracht.  
  
Kunt u het hulpprogramma voor importeren/exporteren van Microsoft Azure met het importeren van taak kopiëren logboekbestanden uitvoeren en het hulpprogramma voor de ontbrekende bestanden (of delen van een bestand) wordt geüpload naar uw Windows Azure storage-account om de importtaak te voltooien.  
  
## <a name="repairimport-parameters"></a>RepairImport parameters

U kunt de volgende parameters opgeven met **RepairImport**: 
  
|||  
|-|-|  
|**/ r:**< RepairFile\>|**Vereist.** Pad naar het bestand herstellen, waarin de voortgang van het herstel en kunt u de reparatie van een onderbroken hervatten. Elk station moet slechts één repair-bestand hebben. Wanneer u een reparatie voor een bepaald station start, worden aan een reparatie-bestand, dat nog niet bestaat in het pad doorgeven. Om de reparatie van een onderbroken hervatten, moet u doorgeven in een bestaand bestand voor herstel. Het herstel bestand overeenkomt met het doelstation moet altijd worden opgegeven.|  
|**schakeloptie/LOGDIR op:**< LogDirectory\>|**Optioneel.** De logboekmap. Uitgebreide logboekbestanden worden geschreven naar deze map. Als er geen logboekmap is opgegeven, wordt de huidige map gebruikt als de logboekmap.|  
|**/ d:**< TargetDirectories\>|**Vereist.** Een of meer door puntkomma's gescheiden mappen met de oorspronkelijke bestanden die zijn geïmporteerd. Het station importeren kan ook worden gebruikt, maar is niet vereist als alternatieve locaties van de originele bestanden beschikbaar zijn.|  
|**/BK:**< BitLockerKey\>|**Optioneel.** Als u wilt dat het hulpprogramma voor het ontgrendelen van een versleuteld station waar de originele bestanden beschikbaar zijn, moet u de BitLocker-sleutel opgeven.|  
|**/sn:**< StorageAccountName\>|**Vereist.** De naam van het opslagaccount voor de import-taak.|  
|**/SK:**< StorageAccountKey\>|**Vereist** als een container SAS is niet opgegeven. De accountsleutel voor het opslagaccount voor de import-taak.|  
|**/csas:**< ContainerSas\>|**Vereist** als de sleutel van het opslagaccount is niet opgegeven. De container SAS voor toegang tot de blobs die zijn gekoppeld aan de import-taak.|  
|**/ CopyLogFile:**< DriveCopyLogFile\>|**Vereist.** Pad naar het station kopiëren-logboekbestand (uitgebreid logboek of fout bij het aanmelden). Het bestand is gegenereerd door de Windows Azure Import/Export-service en kan worden gedownload van de blob-opslag die is gekoppeld aan de taak. Het logboekbestand kopiëren bevat informatie over mislukte blobs of bestanden die moeten worden hersteld.|  
|**/ PathMapFile:**< DrivePathMapFile\>|**Optioneel.** Pad naar een tekstbestand dat kan worden gebruikt voor het oplossen van dubbelzinnigheden als er meerdere bestanden met dezelfde naam die u in dezelfde taak zijn importeren. De eerste keer dat het hulpprogramma wordt uitgevoerd, kan dit bestand met alle niet-eenduidige namen vullen. Dit bestand latere uitvoeringen van het hulpprogramma gebruiken om op te lossen de dubbelzinnigheden.|  
  
## <a name="using-the-repairimport-command"></a>Met behulp van de opdracht RepairImport  
Als u wilt herstellen gegevens importeren door het streamen van de gegevens via het netwerk, moet u de mappen met de oorspronkelijke bestanden zijn u importeren met behulp van de `/d` parameter. Het logboekbestand kopiëren die u hebt gedownload van uw storage-account, moet u ook opgeven. Een typische opdrachtregel om te herstellen van een import-taak met gedeeltelijke fouten ziet eruit als:  
  
```  
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log  
```  
  
In het volgende voorbeeld van een logbestand kopiëren is beschadigd terwijl gegevenselementen 64 K van een bestand op de schijf die is geleverd voor de import-taak. Aangezien dit de enige fout die wordt aangegeven, wordt de rest van de blobs in de taak zijn geïmporteerd.  
  
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
  
Wanneer dit logboek kopie wordt doorgegeven aan de Azure-hulpprogramma voor importeren/exporteren, wordt het hulpprogramma probeert te importeren voor dit bestand met de ontbrekende inhoud via het netwerk te voltooien. Na het bovenstaande voorbeeld kunnen het hulpprogramma zoekt naar het oorspronkelijke bestand `\animals\koala.jpg` binnen de twee mappen `C:\Users\bob\Pictures` en `X:\BobBackup\photos`. Als het bestand `C:\Users\bob\Pictures\animals\koala.jpg` bestaat, de Azure-hulpprogramma voor importeren/exporteren wordt het ontbrekende bereik van gegevens gekopieerd naar de bijbehorende blob `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`.  
  
## <a name="resolving-conflicts-when-using-repairimport"></a>Het oplossen van conflicten bij gebruik van RepairImport  
In sommige gevallen is het hulpprogramma mogelijk niet vinden of openen van het vereiste bestand voor een van de volgende redenen: het bestand kan niet worden gevonden, het bestand is niet toegankelijk, de bestandsnaam is niet eenduidig of de inhoud van het bestand zijn niet langer juist.  
  
Een niet-eenduidige fout kan optreden als het hulpprogramma is bij het vinden van `\animals\koala.jpg` en er is een bestand met die naam onder beide `C:\Users\bob\pictures` en `X:\BobBackup\photos`. Dat wil zeggen, beide `C:\Users\bob\pictures\animals\koala.jpg` en `X:\BobBackup\photos\animals\koala.jpg` bestaat op de import-taak stations.  
  
De `/PathMapFile` optie kunt u deze fouten op te lossen. U kunt de naam van het bestand de lijst met bestanden die het hulpprogramma is niet in staat is bevat te herkennen, opgeven. De volgende opdrachtregel voorbeeld gevuld `9WM35C2V_pathmap.txt`:  
  
```
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log /PathMapFile:C:\WAImportExport\9WM35C2V_pathmap.txt  
```
  
Het hulpprogramma wordt vervolgens de problematisch bestandspaden op om te schrijven `9WM35C2V_pathmap.txt`, één op elke regel. Het bestand kan bijvoorbeeld de volgende vermeldingen bevatten na het uitvoeren van de opdracht:  
 
```
\animals\koala.jpg  
\animals\kangaroo.jpg  
```
  
 Voor elk bestand in de lijst, moet u proberen om te zoeken en open het bestand om te controleren of dat deze beschikbaar is om het hulpprogramma. Als u zien van het hulpprogramma expliciet waar vind ik een bestand wilt, kunt u het pad toewijzingsbestand wijzigen en het pad toevoegen aan elk bestand in dezelfde lijn, gescheiden door een tab-teken:  
  
```
\animals\koala.jpg           C:\Users\bob\Pictures\animals\koala.jpg  
\animals\kangaroo.jpg        X:\BobBackup\photos\animals\kangaroo.jpg  
```
  
Na het maken van de benodigde bestanden beschikbaar zijn voor het hulpprogramma of bijwerken van het toewijzingsbestand pad, kunt u het hulpprogramma voor het voltooien van het importproces opnieuw uit.  
  
## <a name="next-steps"></a>Volgende stappen
 
* [Instellen van het hulpprogramma Azure Import/Export](storage-import-export-tool-setup-v1.md)   
* [Harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [De taakstatus controleren met kopielogboekbestanden](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Een exporttaak herstellen](../storage-import-export-tool-repairing-an-export-job-v1.md)   
* [Problemen met het hulpprogramma Azure Import/Export oplossen](storage-import-export-tool-troubleshooting-v1.md)
