---
title: Azure Import/Export-logboekbestandindeling | Microsoft Docs
description: Meer informatie over de indeling van de logboekbestanden gemaakt wanneer stappen worden uitgevoerd voor een taak Import/Export-service.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 2ae44edf0d9356000f64ab72fd609f1921cf095c
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316577"
---
# <a name="azure-importexport-service-log-file-format"></a>Logboekbestandindeling van Azure Import/Export-service
Wanneer de Microsoft Azure Import/Export-service een actie op een station dat als onderdeel van een import-taak of een exporttaak bekijken uitvoert, worden de logboeken geschreven naar het blok-blobs in de storage-account dat is gekoppeld aan die taak.  
  
Er zijn twee logboeken die door de Import/Export-service kunnen worden geschreven:  
  
-   Het foutenlogboek wordt altijd in het geval van een fout gegenereerd.  
  
-   Het uitgebreide logboek is standaard niet ingeschakeld, maar kan worden ingeschakeld door in te stellen de `EnableVerboseLog` eigenschap op een [taak plaatsen](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) of [Update taakeigenschappen](/rest/api/storageimportexport/jobs#Jobs_Update) bewerking.  
  
## <a name="log-file-location"></a>De locatie van logboekbestand  
De logboeken worden geschreven naar het blok-blobs in de container of de virtuele map die is opgegeven door de `ImportExportStatesPath` instelling, die u kunt instellen voor een `Put Job` bewerking. De locatie waar de logboeken worden geschreven is afhankelijk van hoe verificatie wordt opgegeven voor de taak, samen met de opgegeven waarde voor `ImportExportStatesPath`. Verificatie voor de taak kan worden opgegeven via een opslagaccountsleutel of een container SAS (shared access signature).  
  
De naam van de container of de virtuele map kan ofwel de standaardnaam van `waimportexport`, of een andere container of de naam van de virtuele map die u opgeeft.  
  
De onderstaande tabel ziet u de mogelijke opties:  
  
|Verificatiemethode|De waarde van `ImportExportStatesPath`Element|Locatie van logboekbestand Blobs|  
|---------------------------|----------------------------------------------|---------------------------|  
|Opslagaccountsleutel|Standaardwaarde|Een container met de naam `waimportexport`, dit is de standaard-container. Bijvoorbeeld:<br /><br /> `https://myaccount.blob.core.windows.net/waimportexport`|  
|Opslagaccountsleutel|Door gebruiker opgegeven waarde|Een container met de naam van de gebruiker. Bijvoorbeeld:<br /><br /> `https://myaccount.blob.core.windows.net/mylogcontainer`|  
|Container SAS|Standaardwaarde|Een virtuele map met de naam `waimportexport`, dit is de standaardnaam, onder de container die is opgegeven in de SAS.<br /><br /> Bijvoorbeeld, als de SAS die is opgegeven voor de taak is `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, bedragen de Logboeklocatie `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`|  
|Container SAS|Door gebruiker opgegeven waarde|Een virtuele map met de naam van de gebruiker, onder de container die is opgegeven in de SAS.<br /><br /> Bijvoorbeeld, als de SAS die is opgegeven voor de taak is `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, en de opgegeven virtuele map heet `mylogblobs`, bedragen de Logboeklocatie `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.|  
  
U kunt de URL voor de fout en uitgebreide logboeken ophalen door het aanroepen van de [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) bewerking. De logboeken zijn beschikbaar nadat de verwerking van het station is voltooid.  
  
## <a name="log-file-format"></a>Logboekbestandindeling  
De indeling voor beide logboeken is hetzelfde: een blob met XML-beschrijvingen van de gebeurtenissen die zijn opgetreden tijdens het kopiëren van BLOB's tussen de harde schijf en het account van de klant.  
  
Het uitgebreide logboek vindt u alle informatie over de status van de kopieerbewerking voor elke blob (voor een importtaak) of het bestand (voor een exporttaak), terwijl het foutenlogboek bevat alleen de gegevens voor blobs of bestanden die zijn fouten tijdens het importeren aangetroffen of taak voor het exporteren.  
  
Hieronder ziet u de uitgebreide logboekindeling. Het foutenlogboek van dezelfde structuur heeft, maar filtert voltooide bewerkingen.  

```xml
<DriveLog Version="2014-11-01">  
  <DriveId>drive-id</DriveId>  
  [<Blob Status="blob-status">  
   <BlobPath>blob-path</BlobPath>  
   <FilePath>file-path</FilePath>  
   [<Snapshot>snapshot</Snapshot>]  
   <Length>length</Length>  
   [<LastModified>last-modified</LastModified>]  
   [<ImportDisposition Status="import-disposition-status">import-disposition</ImportDisposition>]  
   [page-range-list-or-block-list]  
   [metadata-status]  
   [properties-status]  
  </Blob>]  
  [<Blob>  
    . . .  
  </Blob>]  
  <Status>drive-status</Status>  
</DriveLog>  
  
page-range-list-or-block-list ::= 
  page-range-list | block-list  
  
page-range-list ::=   
<PageRangeList>  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
</PageRangeList>  
  
block-list ::=  
<BlockList>  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]  
       [Hash="md5-hash"] Status="block-status"/>]  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]   
       [Hash="md5-hash"] Status="block-status"/>]  
</BlockList>  
  
metadata-status ::=  
<Metadata Status="metadata-status">  
   [<GlobalPath Hash="md5-hash">global-metadata-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">metadata-file-path</Path>]  
</Metadata>  
  
properties-status ::=  
<Properties Status="properties-status">  
   [<GlobalPath Hash="md5-hash">global-properties-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">properties-file-path</Path>]  
</Properties>  
```

De volgende tabel beschrijft de elementen van het logboekbestand.  
  
|XML-Element|Type|Description|  
|-----------------|----------|-----------------|  
|`DriveLog`|XML-Element|Hiermee geeft u een logboek station.|  
|`Version`|Tekenreeks-kenmerk|De versie van de indeling voor logboekbestanden.|  
|`DriveId`|Reeks|Serienummer van het station hardware.|  
|`Status`|Reeks|De status van de verwerking van het station. Zie de `Drive Status Codes` tabel hieronder voor meer informatie.|  
|`Blob`|Geneste XML-element|Hiermee geeft u een blob.|  
|`Blob/BlobPath`|Reeks|De URI van de blob.|  
|`Blob/FilePath`|Reeks|Het relatieve pad naar het bestand op het station.|  
|`Blob/Snapshot`|DateTime|De versie van de momentopname van de blob, voor een exporttaak bekijken.|  
|`Blob/Length`|Geheel getal|De totale lengte van de blob in bytes.|  
|`Blob/LastModified`|DateTime|De datum/tijd die de blob het laatst is gewijzigd, voor een exporttaak bekijken.|  
|`Blob/ImportDisposition`|Reeks|De toestand van de invoer van de blob, voor een importtaak alleen.|  
|`Blob/ImportDisposition/@Status`|Tekenreeks-kenmerk|De status van de toestand importeren.|  
|`PageRangeList`|Geneste XML-element|Hiermee geeft u een lijst met paginabereiken voor pagina-blob.|  
|`PageRange`|XML-element|Hiermee geeft u een bereik.|  
|`PageRange/@Offset`|Kenmerk, geheel getal zijn|Begin-offset van het paginabereik in de blob.|  
|`PageRange/@Length`|Kenmerk, geheel getal zijn|De lengte in bytes van het paginabereik.|  
|`PageRange/@Hash`|Tekenreeks-kenmerk|Base16 gecodeerde MD5-hash van het paginabereik.|  
|`PageRange/@Status`|Tekenreeks-kenmerk|De status van de verwerking van het paginabereik.|  
|`BlockList`|Geneste XML-element|Hiermee geeft u een lijst met bouwstenen voor een blok-blob.|  
|`Block`|XML-element|Hiermee geeft u een blok.|  
|`Block/@Offset`|Kenmerk, geheel getal zijn|Begin-offset van het blok in de blob.|  
|`Block/@Length`|Kenmerk, geheel getal zijn|De lengte in bytes van het blok.|  
|`Block/@Id`|Tekenreeks-kenmerk|De blok-ID.|  
|`Block/@Hash`|Tekenreeks-kenmerk|Base16 gecodeerde MD5-hash van het blok.|  
|`Block/@Status`|Tekenreeks-kenmerk|De status van de verwerking van het blok.|  
|`Metadata`|Geneste XML-element|Hiermee geeft u de metagegevens van de blob.|  
|`Metadata/@Status`|Tekenreeks-kenmerk|De status van de verwerking van de blobmetagegevens.|  
|`Metadata/GlobalPath`|Reeks|Relatief pad naar het bestand met globale metagegevens.|  
|`Metadata/GlobalPath/@Hash`|Tekenreeks-kenmerk|Base16 gecodeerde MD5-hash van het globale metagegevensbestand.|  
|`Metadata/Path`|Reeks|Relatief pad naar het bestand met metagegevens.|  
|`Metadata/Path/@Hash`|Tekenreeks-kenmerk|Base16 gecodeerde MD5-hash van het bestand met metagegevens.|  
|`Properties`|Geneste XML-element|Hiermee geeft u de blob-eigenschappen.|  
|`Properties/@Status`|Tekenreeks-kenmerk|Status van de verwerking van de blob-eigenschappen, bijvoorbeeld bestand niet wordt gevonden, is voltooid.|  
|`Properties/GlobalPath`|Reeks|Relatief pad naar het bestand algemene eigenschappen.|  
|`Properties/GlobalPath/@Hash`|Tekenreeks-kenmerk|Base16 gecodeerde MD5-hash van het bestand algemene eigenschappen.|  
|`Properties/Path`|Reeks|Relatief pad naar het eigenschappenbestand.|  
|`Properties/Path/@Hash`|Tekenreeks-kenmerk|Base16 gecodeerde MD5-hash van het eigenschappenbestand.|  
|`Blob/Status`|Reeks|De status van de verwerking van de blob.|  
  
## <a name="drive-status-codes"></a>Statuscodes van station  
De volgende tabel bevat de statuscodes voor het verwerken van een station.  
  
|Statuscode|Description|  
|-----------------|-----------------|  
|`Completed`|Het station is verwerking zonder fouten voltooid.|  
|`CompletedWithWarnings`|Het station is verwerkt met waarschuwingen in een of meer blobs per de import-dispositions opgegeven voor de blobs.|  
|`CompletedWithErrors`|Het station is voltooid met fouten in een of meer blobs of segmenten.|  
|`DiskNotFound`|Er is geen schijf bevindt zich op het station.|  
|`VolumeNotNtfs`|De eerste gegevensvolume op de schijf is niet in de NTFS-indeling.|  
|`DiskOperationFailed`|Er is een onbekende fout opgetreden bij het uitvoeren van bewerkingen op het station.|  
|`BitLockerVolumeNotFound`|Er is geen BitLocker encryptable volume is gevonden.|  
|`BitLockerNotActivated`|BitLocker is niet ingeschakeld op het volume.|  
|`BitLockerProtectorNotFound`|De numerieke wachtwoordsleutelbeveiliging bestaat niet op het volume.|  
|`BitLockerKeyInvalid`|Het opgegeven numerieke wachtwoord kan het volume niet ontgrendelen.|  
|`BitLockerUnlockVolumeFailed`|Onbekende fout opgetreden bij het ontgrendelen van het volume.|  
|`BitLockerFailed`|Er is een onbekende fout opgetreden tijdens het BitLocker-bewerkingen worden uitgevoerd.|  
|`ManifestNameInvalid`|De naam van het manifestbestand is ongeldig.|  
|`ManifestNameTooLong`|De naam van de manifest-bestand is te lang.|  
|`ManifestNotFound`|Het manifestbestand is niet gevonden.|  
|`ManifestAccessDenied`|Toegang tot het manifestbestand is geweigerd.|  
|`ManifestCorrupted`|Het manifestbestand is beschadigd (de inhoud komt niet overeen met de hash).|  
|`ManifestFormatInvalid`|De manifest-inhoud voldoet niet aan de vereiste indeling.|  
|`ManifestDriveIdMismatch`|De stations-ID in het manifestbestand komt niet overeen met één lezen uit het station.|  
|`ReadManifestFailed`|Er is een schijf-i/o-fout opgetreden tijdens het lezen van het manifest.|  
|`BlobListFormatInvalid`|De uitvoer-blob blob vermelden voldoet niet aan de vereiste indeling.|  
|`BlobRequestForbidden`|Toegang tot de blobs in de storage-account is niet toegestaan. Dit kan worden veroorzaakt door ongeldige opslagaccountsleutel of container SAS.|  
|`InternalError`|En er is een interne fout opgetreden tijdens het verwerken van het station.|  
  
## <a name="blob-status-codes"></a>BLOB-statuscodes  
De volgende tabel bevat de statuscodes voor het verwerken van een blob.  
  
|Statuscode|Description|  
|-----------------|-----------------|  
|`Completed`|De blob is verwerking zonder fouten voltooid.|  
|`CompletedWithErrors`|De blob is verwerkt met fouten in een of meer page-bereiken of blokken, metagegevens of eigenschappen.|  
|`FileNameInvalid`|De bestandsnaam is ongeldig.|  
|`FileNameTooLong`|De bestandsnaam is te lang.|  
|`FileNotFound`|Het bestand is niet gevonden.|  
|`FileAccessDenied`|Toegang tot het bestand is geweigerd.|  
|`BlobRequestFailed`|De Blob service-aanvraag voor toegang tot de blob is mislukt.|  
|`BlobRequestForbidden`|De Blob service-aanvraag voor toegang tot de blob is niet toegestaan. Dit kan worden veroorzaakt door ongeldige opslagaccountsleutel of container SAS.|  
|`RenameFailed`|Wijzig de naam van de blob (voor een importtaak) of het bestand (voor een exporttaak) is mislukt.|  
|`BlobUnexpectedChange`|Onverwachte wijziging is opgetreden met de blob (voor een exporttaak bekijken).|  
|`LeasePresent`|Er is een aanwezig zijn op de blob-lease.|  
|`IOFailed`|Er is een schijf- of i/o-fout opgetreden tijdens het verwerken van de blob.|  
|`Failed`|Er is een onbekende fout opgetreden tijdens het verwerken van de blob.|  
  
## <a name="import-disposition-status-codes"></a>Statuscodes toestand importeren  
De volgende tabel bevat de statuscodes voor het oplossen van een toestand importeren.  
  
|Statuscode|Description|  
|-----------------|-----------------|  
|`Created`|De blob is gemaakt.|  
|`Renamed`|De blob is per naam importeren toestand gewijzigd. De `Blob/BlobPath` element bevat de URI voor de nieuwe naam van de blob.|  
|`Skipped`|De blob is overgeslagen `no-overwrite` toestand importeren.|  
|`Overwritten`|De blob heeft een bestaande blob per overschreven `overwrite` toestand importeren.|  
|`Cancelled`|Een eerdere fout is gestopt verdere verwerking van de toestand importeren.|  
  
## <a name="page-rangeblock-status-codes"></a>Pagina-bereik/blok statuscodes  
De volgende tabel bevat de statuscodes voor het verwerken van een bereik of blok.  
  
|Statuscode|Description|  
|-----------------|-----------------|  
|`Completed`|De paginabereik of blok is verwerking zonder fouten voltooid.|  
|`Committed`|Het blok is doorgevoerd, maar niet in het volledige blok lijst omdat andere blokken is mislukt, of volledige lijst met geblokkeerde websites zelf plaatsen is mislukt.|  
|`Uncommitted`|Het blok is geüpload, maar niet zijn doorgevoerd.|  
|`Corrupted`|De paginabereik of blok is beschadigd (de inhoud komt niet overeen met de hash).|  
|`FileUnexpectedEnd`|Een onverwacht bestandseinde er is opgetreden.|  
|`BlobUnexpectedEnd`|Een onverwacht einde van de blob is opgetreden.|  
|`BlobRequestFailed`|De Blob service-aanvraag voor toegang tot het bereik of blok is mislukt.|  
|`IOFailed`|Er is een schijf- of i/o-fout opgetreden tijdens het verwerken van het paginabereik of het blok.|  
|`Failed`|Er is een onbekende fout opgetreden tijdens het verwerken van het paginabereik of het blok.|  
|`Cancelled`|Een eerdere fout is gestopt verdere verwerking van het paginabereik of het blok.|  
  
## <a name="metadata-status-codes"></a>Statuscodes van metagegevens  
De volgende tabel bevat de statuscodes voor het verwerken van metagegevens van de blob.  
  
|Statuscode|Description|  
|-----------------|-----------------|  
|`Completed`|De metagegevens is verwerking zonder fouten voltooid.|  
|`FileNameInvalid`|De naam van het metagegevens-bestand is ongeldig.|  
|`FileNameTooLong`|De naam van het metagegevens-bestand is te lang.|  
|`FileNotFound`|Het bestand met metagegevens is niet gevonden.|  
|`FileAccessDenied`|Toegang tot het bestand met metagegevens is geweigerd.|  
|`Corrupted`|Het bestand met metagegevens is beschadigd (de inhoud komt niet overeen met de hash).|  
|`XmlReadFailed`|De metagegevens-inhoud voldoet niet aan de vereiste indeling.|  
|`XmlWriteFailed`|Schrijven van de metagegevens is mislukt.|  
|`BlobRequestFailed`|De Blob service-aanvraag voor toegang tot de metagegevens is mislukt.|  
|`IOFailed`|Er is een schijf- of i/o-fout opgetreden tijdens het verwerken van de metagegevens.|  
|`Failed`|Er is een onbekende fout opgetreden tijdens het verwerken van de metagegevens.|  
|`Cancelled`|Een eerdere fout is gestopt verdere verwerking van de metagegevens.|  
  
## <a name="properties-status-codes"></a>Eigenschappen van statuscodes  
De volgende tabel bevat de statuscodes voor het verwerken van blob-eigenschappen.  
  
|Statuscode|Description|  
|-----------------|-----------------|  
|`Completed`|De eigenschappen verwerken zonder fouten is voltooid.|  
|`FileNameInvalid`|De naam van de eigenschappen is ongeldig.|  
|`FileNameTooLong`|De naam van de eigenschappen is te lang.|  
|`FileNotFound`|Het eigenschappenbestand is niet gevonden.|  
|`FileAccessDenied`|Toegang tot het eigenschappenbestand is geweigerd.|  
|`Corrupted`|De van het eigenschappenbestand is beschadigd (de inhoud komt niet overeen met de hash).|  
|`XmlReadFailed`|De eigenschappen van inhoud voldoet niet aan de vereiste indeling.|  
|`XmlWriteFailed`|Schrijven van de eigenschappen is mislukt.|  
|`BlobRequestFailed`|De Blob service-aanvraag voor toegang tot de eigenschappen is mislukt.|  
|`IOFailed`|Er is een schijf- of i/o-fout opgetreden tijdens het verwerken van de eigenschappen.|  
|`Failed`|Er is een onbekende fout opgetreden tijdens het verwerken van de eigenschappen.|  
|`Cancelled`|Een eerdere fout is gestopt verdere verwerking van de eigenschappen.|  
  
## <a name="sample-logs"></a>Voobeeldlogboeken  
Hier volgt een voorbeeld van het uitgebreide logboek.  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV123456</DriveId>  
    <Blob Status="Completed">  
       <BlobPath>pictures/bob/wild/desert.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\wild\desert.jpg</FilePath>  
       <Length>98304</Length>  
       <ImportDisposition Status="Created">overwrite</ImportDisposition>  
       <BlockList>  
          <Block Offset="0" Length="65536" Id="AAAAAA==" Hash=" 9C8AE14A55241F98533C4D80D85CDC68" Status="Completed"/>  
          <Block Offset="65536" Length="32768" Id="AQAAAA==" Hash=" DF54C531C9B3CA2570FDDDB3BCD0E27D" Status="Completed"/>  
       </BlockList>  
       <Metadata Status="Completed">  
          <GlobalPath Hash=" E34F54B7086BCF4EC1601D056F4C7E37">\Users\bob\Pictures\wild\metadata.xml</GlobalPath>  
       </Metadata>  
    </Blob>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="0" Length="65536" Hash="19701B8877418393CB3CB567F53EE225" Status="Completed"/>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
          <PageRange Offset="131072" Length="4096" Hash="9BA552E1C3EEAFFC91B42B979900A996" Status="Completed"/>  
       </PageRangeList>  
       <Properties Status="Completed">  
          <Path Hash="38D7AE80653F47F63C0222FEE90EC4E7">\Users\bob\Pictures\animals\koala.jpg.properties</Path>  
       </Properties>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
Hieronder ziet u het foutenlogboek van het bijbehorende.  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV6965824</DriveId>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
       </PageRangeList>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

 Het foutenlogboek volgen voor een importtaak bevat een fout over een bestand niet gevonden op het station importeren. Merk op dat de status van de volgende onderdelen `Cancelled`.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="FileNotFound">  
    <BlobPath>pictures/animals/koala.jpg</BlobPath>  
    <FilePath>\animals\koala.jpg</FilePath>  
    <Length>30310</Length>  
    <ImportDisposition Status="Cancelled">rename</ImportDisposition>  
    <BlockList>  
      <Block Offset="0" Length="6062" Id="MD5/cAzn4h7VVSWXf696qp5Uaw==" Hash="700CE7E21ED55525977FAF7AAA9E546B" Status="Cancelled" />  
      <Block Offset="6062" Length="6062" Id="MD5/PEnGwYOI8LPLNYdfKr7kAg==" Hash="3C49C6C18388F0B3CB35875F2ABEE402" Status="Cancelled" />  
      <Block Offset="12124" Length="6062" Id="MD5/FG4WxqfZKuUWZ2nGTU2qVA==" Hash="146E16C6A7D92AE5166769C64D4DAA54" Status="Cancelled" />  
      <Block Offset="18186" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
      <Block Offset="24248" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

Het volgende foutenlogboek voor een exporttaak geeft aan dat de inhoud van de blob is geschreven naar het station, maar dat is een fout opgetreden tijdens het exporteren van de blob-eigenschappen.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C3U</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
    <FilePath>\pictures\wild\canyon.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList />  
    <Properties Status="Failed" />  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
## <a name="next-steps"></a>Volgende stappen
 
* [REST-API van Storage Import/Export](/rest/api/storageimportexport/)
