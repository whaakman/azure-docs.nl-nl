---
title: Herstellen van een taak van de export Azure Import/Export - v1 | Microsoft Docs
description: Informatie over het herstellen van een taak voor het exporteren die zijn gemaakt en uitgevoerd met behulp van de Azure Import/Export-service.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 728e2a42-04ce-4be8-9375-e9e2bc6827a5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 57ab58fa1fd8371d0b6f019f94bb162bcc1e0e43
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="repairing-an-export-job"></a>Een exporttaak herstellen
Nadat een taak voor het exporteren is voltooid, kunt u de Microsoft Azure-hulpprogramma voor importeren/exporteren on-premises naar uitvoeren:  
  
1.  Download de bestanden die de Azure Import/Export-service kon niet exporteren.  
  
2.  Valideren of de bestanden op de schijf correct zijn geëxporteerd.  
  
U moet zijn verbonden met Azure Storage om deze functionaliteit te gebruiken.  
  
De opdracht voor het herstellen van een import-taak is **RepairExport**.

## <a name="repairexport-parameters"></a>RepairExport parameters

U kunt de volgende parameters opgeven met **RepairExport**:  
  
|Parameter|Beschrijving|  
|---------------|-----------------|  
|**/ r: < RepairFile\>**|Vereist. Pad naar het bestand herstellen, waarin de voortgang van het herstel en kunt u de reparatie van een onderbroken hervatten. Elk station moet slechts één repair-bestand hebben. Wanneer u een reparatie voor een bepaald station start, wordt u in het pad doorgeven aan een reparatie-bestand dat nog niet bestaat. Om de reparatie van een onderbroken hervatten, moet u doorgeven in een bestaand bestand voor herstel. Het herstel bestand overeenkomt met het doelstation moet altijd worden opgegeven.|  
|**schakeloptie/LOGDIR op: < LogDirectory\>**|Optioneel. De logboekmap. Uitgebreide logboekbestanden worden geschreven naar deze map. Als er geen logboekmap is opgegeven, wordt de huidige map gebruikt als de logboekmap.|  
|**/ d: < TargetDirectory\>**|Vereist. De map om te valideren en te herstellen. Dit is meestal de hoofdmap van het station exporteren, maar kan ook worden een netwerkbestandsshare die een kopie van de geëxporteerde bestanden.|  
|**/BK: < BitLockerKey\>**|Optioneel. Als u wilt dat het hulpprogramma voor het ontgrendelen van een gecodeerd waarin de geëxporteerde bestanden zijn opgeslagen, moet u de BitLocker-sleutel opgeven.|  
|**/sn: < StorageAccountName\>**|Vereist. De naam van het opslagaccount voor de taak voor het exporteren.|  
|**/SK: < StorageAccountKey\>**|**Vereist** als een container SAS is niet opgegeven. De accountsleutel voor het opslagaccount voor de taak voor exporteren.|  
|**/csas: < ContainerSas\>**|**Vereist** als de sleutel van het opslagaccount is niet opgegeven. De container SAS voor toegang tot de blobs die zijn gekoppeld aan de taak voor het exporteren.|  
|**/ CopyLogFile: < DriveCopyLogFile\>**|Vereist. Het pad naar het logboekbestand van de schijf kopiëren. Het bestand is gegenereerd door de Windows Azure Import/Export-service en kan worden gedownload van de blob-opslag die is gekoppeld aan de taak. Het logboekbestand kopiëren bevat informatie over mislukte blobs of bestanden die moeten worden hersteld.|  
|**/ ManifestFile: < DriveManifestFile\>**|Optioneel. Het pad naar het manifestbestand van de export-station. Dit bestand is gegenereerd door de Windows Azure Import/Export-service en opgeslagen op de schijf exporteren en optioneel in een blob in de storage-account die is gekoppeld aan de taak.<br /><br /> De inhoud van de bestanden op de schijf van de uitvoer wordt geverifieerd met de MD5-hashes die zijn opgenomen in dit bestand. Bestanden die worden bepaald beschadigd te zijn gedownload en herschreven naar de doel-mappen.|  
  
## <a name="using-repairexport-mode-to-correct-failed-exports"></a>RepairExport modus gebruiken om op te lossen mislukte uitvoer  
U kunt het Azure Import/Export-hulpprogramma gebruiken voor het downloaden van bestanden die exporteren is mislukt. Het logboekbestand kopie bevat een lijst met bestanden die exporteren is mislukt.  
  
De oorzaken van fouten van de uitvoer zijn onder andere de volgende mogelijkheden:  
  
-   Beschadigde schijven  
  
-   De opslagaccountsleutel die is gewijzigd tijdens de overdracht  
  
Het hulpprogramma uitvoeren **RepairExport** modus, moet u eerst verbinding maken met het station met de geëxporteerde bestanden op uw computer. Voer vervolgens het Azure Import/Export-hulpprogramma opgeven van het pad naar het station met de `/d` parameter. Ook moet u het pad opgeven naar de schijf kopiëren logboekbestand dat u hebt gedownload. De volgende opdrachtregel voorbeeld hieronder wordt uitgevoerd voor het hulpprogramma voor het herstellen van bestanden die exporteren is mislukt:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
Hier volgt een voorbeeld van een logbestand kopiëren die laat dat een blok in de blob zien is mislukt om te exporteren:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/desert.jpg</BlobPath>  
    <FilePath>\pictures\wild\desert.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList>  
      <Block Offset="65536" Length="65536" Id="AQAAAA==" Status="Failed" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
Het logboekbestand kopie geeft aan dat is een fout opgetreden terwijl de Windows Azure Import/Export-service is een van de blob-blokken gedownload naar het bestand op het station exporteren. De andere onderdelen van het bestand is gedownload, en de bestandslengte juist is ingesteld. Het hulpprogramma wordt in dit geval open het bestand op de schijf, downloaden van het blok van het opslagaccount en schrijven naar het bestand bereik vanaf verschuiving 65536 met lengte 65536.  
  
## <a name="using-repairexport-to-validate-drive-contents"></a>Met behulp van RepairExport station inhoud valideren  
U kunt ook Azure Import/Export met de **RepairExport** optie voor het valideren van de inhoud op het station juist zijn. Het manifestbestand op elk station export bevat MD5s voor de inhoud van het station.  
  
De Azure Import/Export-service kan ook de manifest-bestanden opslaan naar een opslagaccount tijdens het exporteren. De locatie van de manifest-bestanden is beschikbaar via de [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) bewerking als de taak is voltooid. Zie [Import/Export-service Manifest bestandsindeling](storage-import-export-file-format-metadata-and-properties.md) voor meer informatie over de indeling van het manifestbestand van een station.  
  
Het volgende voorbeeld ziet u het uitvoeren van het Azure Import/Export-hulpprogramma met de **/ManifestFile** en **/CopyLogFile** parameters:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log /ManifestFile:G:\9WM35C3U.manifest  
```  
  
Hier volgt een voorbeeld van een manifestbestand:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveManifest Version="2011-10-01">  
  <Drive>  
    <DriveId>9WM35C3U</DriveId>  
    <ClientCreator>Windows Azure Import/Export service</ClientCreator>  
    <BlobList>
      <Blob>  
        <BlobPath>pictures/city/redmond.jpg</BlobPath>  
        <FilePath>\pictures\city\redmond.jpg</FilePath>  
        <Length>15360</Length>  
        <PageRangeList>  
          <PageRange Offset="0" Length="3584" Hash="72FC55ED9AFDD40A0C8D5C4193208416" />  
          <PageRange Offset="3584" Length="3584" Hash="68B28A561B73D1DA769D4C24AA427DB8" />  
          <PageRange Offset="7168" Length="512" Hash="F521DF2F50C46BC5F9EA9FB787A23EED" />  
        </PageRangeList>  
        <PropertiesPath Hash="E72A22EA959566066AD89E3B49020C0A">\pictures\city\redmond.jpg.properties</PropertiesPath>  
      </Blob>  
      <Blob>  
        <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
        <FilePath>\pictures\wild\canyon.jpg</FilePath>  
        <Length>10884</Length>  
        <BlockList>  
          <Block Offset="0" Length="2721" Id="AAAAAA==" Hash="263DC9C4B99C2177769C5EBE04787037" />  
          <Block Offset="2721" Length="2721" Id="AQAAAA==" Hash="0C52BAE2CC20EFEC15CC1E3045517AA6" />  
          <Block Offset="5442" Length="2721" Id="AgAAAA==" Hash="73D1CB62CB426230C34C9F57B7148F10" />  
          <Block Offset="8163" Length="2721" Id="AwAAAA==" Hash="11210E665C5F8E7E4F136D053B243E6A" />  
        </BlockList>  
        <PropertiesPath Hash="81D7F81B2C29F10D6E123D386C3A4D5A">\pictures\wild\canyon.jpg.properties</PropertiesPath>  
      </Blob> 
    </BlobList>  
 </Drive>  
</DriveManifest>  
``` 
  
Nadat het herstelproces is voltooid, wordt het hulpprogramma Lees elk bestand waarnaar wordt verwezen in het manifestbestand en controleer of de integriteit van het bestand met de MD5-hashes. Voor het manifest hierboven gaat dit via de volgende onderdelen.  

```  
G:\pictures\city\redmond.jpg, offset 0, length 3584  
  
G:\pictures\city\redmond.jpg, offset 3584, length 3584  
  
G:\pictures\city\redmond.jpg, offset 7168, length 3584  
  
G:\pictures\city\redmond.jpg.properties  
  
G:\pictures\wild\canyon.jpg, offset 0, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 2721, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 5442, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 8163, length 2721  
  
G:\pictures\wild\canyon.jpg.properties  
```

De onderdelen die zijn mislukt de verificatie door het hulpprogramma wordt gedownload en herschreven naar hetzelfde bestand op de schijf.  
  
## <a name="next-steps"></a>Volgende stappen
 
* [Instellen van het hulpprogramma Azure Import/Export](storage-import-export-tool-setup-v1.md)   
* [Harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [De taakstatus controleren met kopielogboekbestanden](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Een importtaak herstellen](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Problemen met het hulpprogramma Azure Import/Export oplossen](storage-import-export-tool-troubleshooting-v1.md)
