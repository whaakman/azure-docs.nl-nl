---
title: Azure Import/Export-logboekindeling | Microsoft Docs
description: Meer informatie over de indeling van de logboekbestanden gemaakt wanneer de stappen worden uitgevoerd voor een taak van Import/Export-service.
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
ms.openlocfilehash: 16234ccaf13ce1d85cfd207ed4734e683070faa6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-importexport-service-log-file-format"></a>Azure Import/Export-service indeling van logboekbestand
Wanneer een actie in de Microsoft Azure Import/Export-service op een station dat als onderdeel van een import-taak of een taak voor het exporteren wordt uitgevoerd, worden logboeken geschreven naar blok-blobs in de storage-account dat is gekoppeld met deze taak.  
  
Er zijn twee logboekbestanden die kunnen worden geschreven door de Import/Export-service:  
  
-   Het foutenlogboek wordt altijd in het geval van een fout gegenereerd.  
  
-   Het uitgebreide logboek is standaard niet ingeschakeld, maar kan worden ingeschakeld door in te stellen de `EnableVerboseLog` -eigenschap op een [taak plaatsen](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) of [Update taakeigenschappen](/rest/api/storageimportexport/jobs#Jobs_Update) bewerking.  
  
## <a name="log-file-location"></a>De locatie van logboekbestand  
De logboeken worden geschreven naar het blok-blobs in de container of de virtuele map die is opgegeven door de `ImportExportStatesPath` instelling, die u kunt instellen op een `Put Job` bewerking. De locatie waarop de logboeken geschreven is afhankelijk van hoe de verificatie is opgegeven voor de taak, samen met de opgegeven waarde voor `ImportExportStatesPath`. Verificatie voor de taak kan worden opgegeven via de sleutel van een opslagaccount of een container SAS (shared access signature).  
  
De naam van de container of de virtuele map kan ofwel worden de standaardnaam van `waimportexport`, of een andere container of de naam van de virtuele map die u opgeeft.  
  
De onderstaande tabel ziet u de mogelijke opties:  
  
|Verificatiemethode|Waarde van `ImportExportStatesPath`Element|Locatie van het logboek Blobs|  
|---------------------------|----------------------------------------------|---------------------------|  
|De sleutel van opslagaccount|Standaardwaarde|Een container met de naam `waimportexport`, dit is de standaard-container. Bijvoorbeeld:<br /><br /> `https://myaccount.blob.core.windows.net/waimportexport`|  
|De sleutel van opslagaccount|Gebruiker opgegeven waarde|Een container met de naam door de gebruiker. Bijvoorbeeld:<br /><br /> `https://myaccount.blob.core.windows.net/mylogcontainer`|  
|Container SAS|Standaardwaarde|Een virtuele map met de naam `waimportexport`, dit is de naam van de standaard onder de container die is opgegeven in de SAS.<br /><br /> Bijvoorbeeld, als de SA's worden opgegeven voor de taak is `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, en vervolgens de locatie van het logboek zijn`https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`|  
|Container SAS|Gebruiker opgegeven waarde|Een virtuele map met de naam door de gebruiker, onder de container die is opgegeven in de SAS.<br /><br /> Bijvoorbeeld, als de SA's worden opgegeven voor de taak is `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, en de naam van de opgegeven virtuele map `mylogblobs`, wordt de locatie van het logboekbestand zou `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.|  
  
U kunt de URL voor de fout en de uitgebreide logboeken ophalen door het aanroepen van de [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) bewerking. De logboeken zijn beschikbaar nadat de verwerking van het station is voltooid.  
  
## <a name="log-file-format"></a>Indeling van logboekbestand  
De indeling voor beide logboeken is hetzelfde: een blob met XML-beschrijvingen van de gebeurtenissen die zijn opgetreden tijdens het kopiëren van BLOB's tussen de vaste schijf en het account van de klant.  
  
Het uitgebreide logboek bevat informatie over de status van de kopieerbewerking voor elke blob (voor een import-taak) of het bestand (voor een exporttaak) terwijl het foutenlogboek bevat alleen de informatie voor blobs of bestanden die fouten tijdens het importeren aangetroffen of taak voor het exporteren.  
  
De uitgebreide logboekindeling worden hieronder weergegeven. Het foutenlogboek van dezelfde structuur heeft, maar geslaagde bewerkingen, worden uitgefilterd.  

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
  
|XML-Element|Type|Beschrijving|  
|-----------------|----------|-----------------|  
|`DriveLog`|XML-Element|Hiermee geeft u een station voor logboekbestanden.|  
|`Version`|Kenmerk, tekenreeks|De versie van de indeling voor logboekbestanden.|  
|`DriveId`|Tekenreeks|Serienummer van het station hardware.|  
|`Status`|Tekenreeks|Status van de verwerking van het station. Zie de `Drive Status Codes` tabel hieronder voor meer informatie.|  
|`Blob`|Geneste XML-element|Hiermee geeft u een blob.|  
|`Blob/BlobPath`|Tekenreeks|De URI van de blob.|  
|`Blob/FilePath`|Tekenreeks|Het relatieve pad naar het bestand op het station.|  
|`Blob/Snapshot`|Datum/tijd|De versie van de momentopname van de blob, voor een exporttaak.|  
|`Blob/Length`|Geheel getal|De totale lengte van de blob in bytes.|  
|`Blob/LastModified`|Datum/tijd|De datum/tijd die de blob het laatst is gewijzigd voor een exporttaak.|  
|`Blob/ImportDisposition`|Tekenreeks|De bestemming importeren van de blob, voor een import-taak alleen.|  
|`Blob/ImportDisposition/@Status`|Kenmerk, tekenreeks|De status van de toestand importeren.|  
|`PageRangeList`|Geneste XML-element|Hiermee geeft u een lijst met paginabereiken voor een pagina-blob.|  
|`PageRange`|XML-element|Hiermee geeft u een paginabereik.|  
|`PageRange/@Offset`|Kenmerk, geheel getal|Verschuiving van het paginabereik in de blob wordt gestart.|  
|`PageRange/@Length`|Kenmerk, geheel getal|De lengte in bytes van het paginabereik.|  
|`PageRange/@Hash`|Kenmerk, tekenreeks|Base16 gecodeerd MD5-hash van het paginabereik.|  
|`PageRange/@Status`|Kenmerk, tekenreeks|De status van de verwerking van het paginabereik.|  
|`BlockList`|Geneste XML-element|Hiermee geeft u een lijst met bouwstenen voor een blok-blob.|  
|`Block`|XML-element|Hiermee geeft u een blok.|  
|`Block/@Offset`|Kenmerk, geheel getal|Verschuiving van het blok in de blob wordt gestart.|  
|`Block/@Length`|Kenmerk, geheel getal|De lengte in bytes van het blok.|  
|`Block/@Id`|Kenmerk, tekenreeks|De blok-ID.|  
|`Block/@Hash`|Kenmerk, tekenreeks|Base16 gecodeerd MD5-hash van het blok.|  
|`Block/@Status`|Kenmerk, tekenreeks|De status van de verwerking van het blok.|  
|`Metadata`|Geneste XML-element|Hiermee geeft u de metagegevens van de blob.|  
|`Metadata/@Status`|Kenmerk, tekenreeks|De status van de verwerking van de blobmetagegevens.|  
|`Metadata/GlobalPath`|Tekenreeks|Relatief pad naar het bestand algemene metagegevens.|  
|`Metadata/GlobalPath/@Hash`|Kenmerk, tekenreeks|Base16 gecodeerd MD5-hash van het bestand algemene metagegevens.|  
|`Metadata/Path`|Tekenreeks|Relatief pad naar het bestand met metagegevens.|  
|`Metadata/Path/@Hash`|Kenmerk, tekenreeks|Base16 gecodeerd MD5-hash van het bestand met metagegevens.|  
|`Properties`|Geneste XML-element|Geeft de blob-eigenschappen.|  
|`Properties/@Status`|Kenmerk, tekenreeks|Status van de verwerking van de blob-eigenschappen, bijvoorbeeld bestand niet gevonden, voltooid.|  
|`Properties/GlobalPath`|Tekenreeks|Relatief pad naar het bestand algemene eigenschappen.|  
|`Properties/GlobalPath/@Hash`|Kenmerk, tekenreeks|Base16 gecodeerd MD5-hash van het bestand algemene eigenschappen.|  
|`Properties/Path`|Tekenreeks|Relatief pad naar het eigenschappenbestand.|  
|`Properties/Path/@Hash`|Kenmerk, tekenreeks|Base16 gecodeerd MD5-hash van het eigenschappenbestand.|  
|`Blob/Status`|Tekenreeks|De status van de verwerking van de blob.|  
  
# <a name="drive-status-codes"></a>Statuscodes van station  
De volgende tabel bevat de statuscodes voor het verwerken van een station.  
  
|Statuscode|Beschrijving|  
|-----------------|-----------------|  
|`Completed`|Het station is verwerking zonder fouten voltooid.|  
|`CompletedWithWarnings`|Het station zijn met waarschuwingen in een of meer blobs per de import dispositions opgegeven voor de blobs verwerkt.|  
|`CompletedWithErrors`|Het station is voltooid met fouten in een of meer blobs of segmenten.|  
|`DiskNotFound`|Er is geen schijf gevonden op de schijf.|  
|`VolumeNotNtfs`|De eerste gegevensvolume op de schijf is niet in een NTFS-indeling.|  
|`DiskOperationFailed`|Er is een onbekende fout opgetreden bij het uitvoeren van bewerkingen op het station.|  
|`BitLockerVolumeNotFound`|Er is geen BitLocker encryptable volume gevonden.|  
|`BitLockerNotActivated`|BitLocker is niet ingeschakeld op het volume.|  
|`BitLockerProtectorNotFound`|De numerieke wachtwoordsleutelbeveiliging bestaat niet op het volume.|  
|`BitLockerKeyInvalid`|Het opgegeven numerieke wachtwoord kan niet het volume ontgrendelen.|  
|`BitLockerUnlockVolumeFailed`|Is een onbekende fout opgetreden bij het ontgrendelen van het volume.|  
|`BitLockerFailed`|Een onbekende fout opgetreden tijdens het uitvoeren van BitLocker-bewerkingen.|  
|`ManifestNameInvalid`|De naam van het manifestbestand is ongeldig.|  
|`ManifestNameTooLong`|De naam van het manifestbestand is te lang.|  
|`ManifestNotFound`|Het manifestbestand is niet gevonden.|  
|`ManifestAccessDenied`|Toegang tot het manifestbestand is geweigerd.|  
|`ManifestCorrupted`|Het manifestbestand is beschadigd (de inhoud komt niet overeen met de hash).|  
|`ManifestFormatInvalid`|Het manifest inhoud voldoet niet aan de vereiste indeling.|  
|`ManifestDriveIdMismatch`|De schijf-ID in het manifestbestand komt niet overeen met het één lezen van het station.|  
|`ReadManifestFailed`|Er is een schijf-i/o-fout opgetreden tijdens het lezen van het manifest.|  
|`BlobListFormatInvalid`|De uitvoer-blob lijst blob voldoet niet aan de vereiste indeling.|  
|`BlobRequestForbidden`|Toegang tot de blobs in de storage-account is niet toegestaan. Dit wordt mogelijk veroorzaakt door ongeldige opslagaccountsleutel of SAS-container.|  
|`InternalError`|En er is een interne fout opgetreden tijdens het verwerken van het station.|  
  
## <a name="blob-status-codes"></a>Statuscodes BLOB  
De volgende tabel bevat de statuscodes voor het verwerken van een blob.  
  
|Statuscode|Beschrijving|  
|-----------------|-----------------|  
|`Completed`|De blob is verwerking zonder fouten voltooid.|  
|`CompletedWithErrors`|De blob zijn met fouten in een of meer paginabereiken of blokken, metagegevens of eigenschappen verwerkt.|  
|`FileNameInvalid`|De bestandsnaam is ongeldig.|  
|`FileNameTooLong`|De bestandsnaam is te lang.|  
|`FileNotFound`|Het bestand is niet gevonden.|  
|`FileAccessDenied`|Toegang tot het bestand is geweigerd.|  
|`BlobRequestFailed`|De Blob-service-aanvraag voor toegang tot de blob is mislukt.|  
|`BlobRequestForbidden`|De Blob-service-aanvraag voor toegang tot de blob is verboden. Dit wordt mogelijk veroorzaakt door ongeldige opslagaccountsleutel of SAS-container.|  
|`RenameFailed`|Wijzigen van de blob (voor een import-taak) of het bestand (voor een taak voor het exporteren) is mislukt.|  
|`BlobUnexpectedChange`|Onverwachte wijziging is opgetreden met de blob (voor een taak voor het exporteren).|  
|`LeasePresent`|Er is een lease aanwezig is op de blob.|  
|`IOFailed`|Er is een schijf of het netwerk-i/o-fout opgetreden tijdens het verwerken van de blob.|  
|`Failed`|Er is een onbekende fout opgetreden tijdens het verwerken van de blob.|  
  
## <a name="import-disposition-status-codes"></a>Statuscodes disposition importeren  
De volgende tabel bevat de statuscodes voor het oplossen van een toestand importeren.  
  
|Statuscode|Beschrijving|  
|-----------------|-----------------|  
|`Created`|De blob is gemaakt.|  
|`Renamed`|De blob heeft per rename importeren bestemming gekregen. De `Blob/BlobPath` element bevat de URI voor de nieuwe naam blob.|  
|`Skipped`|De blob is overgeslagen `no-overwrite` toestand importeren.|  
|`Overwritten`|De blob heeft een bestaande blob per overschreven `overwrite` toestand importeren.|  
|`Cancelled`|Een eerdere fout is gestopt verdere verwerking van de toestand importeren.|  
  
## <a name="page-rangeblock-status-codes"></a>Statuscodes voor pagina-bereik/blok  
De volgende tabel bevat de statuscodes voor het verwerken van een paginabereik of een blok.  
  
|Statuscode|Beschrijving|  
|-----------------|-----------------|  
|`Completed`|De paginabereik of blok is verwerking zonder fouten voltooid.|  
|`Committed`|Het blok is doorgevoerd, maar niet in het volledige blok lijst omdat andere blokken is mislukt, of de volledige lijst met geblokkeerde websites zelf plaatsen is mislukt.|  
|`Uncommitted`|Het blok is geüpload, maar niet zijn doorgevoerd.|  
|`Corrupted`|De paginabereik of blok is beschadigd (de inhoud komt niet overeen met de hash).|  
|`FileUnexpectedEnd`|Een onverwacht bestandseinde er is opgetreden.|  
|`BlobUnexpectedEnd`|Een onverwacht einde van de blob is opgetreden.|  
|`BlobRequestFailed`|De Blob-service-aanvraag voor toegang tot het bereik of blok is mislukt.|  
|`IOFailed`|Er is een schijf of het netwerk-i/o-fout opgetreden tijdens het verwerken van de paginabereik of blok.|  
|`Failed`|Een onbekende fout opgetreden tijdens het verwerken van de paginabereik of blok.|  
|`Cancelled`|Er is een eerdere fout gestopt verdere verwerking van de paginabereik of blok.|  
  
## <a name="metadata-status-codes"></a>Statuscodes van metagegevens  
De volgende tabel bevat de statuscodes voor het verwerken van blobmetagegevens.  
  
|Statuscode|Beschrijving|  
|-----------------|-----------------|  
|`Completed`|De metagegevens is verwerking zonder fouten voltooid.|  
|`FileNameInvalid`|De bestandsnaam van metagegevens is ongeldig.|  
|`FileNameTooLong`|De metagegevens-bestandsnaam is te lang.|  
|`FileNotFound`|Het bestand met metagegevens is niet gevonden.|  
|`FileAccessDenied`|Toegang tot het bestand met metagegevens is geweigerd.|  
|`Corrupted`|Het metagegevensbestand is beschadigd (de inhoud komt niet overeen met de hash).|  
|`XmlReadFailed`|De metagegevens van inhoud voldoet niet aan de vereiste indeling.|  
|`XmlWriteFailed`|Schrijven van de metagegevens van de is XML mislukt.|  
|`BlobRequestFailed`|De Blob-service-aanvraag voor toegang tot de metagegevens is mislukt.|  
|`IOFailed`|Een schijf of het netwerk-i/o-fout opgetreden tijdens het verwerken van de metagegevens.|  
|`Failed`|Een onbekende fout opgetreden tijdens het verwerken van de metagegevens.|  
|`Cancelled`|Een eerdere fout is gestopt verdere verwerking van de metagegevens.|  
  
## <a name="properties-status-codes"></a>Statuscodes van eigenschappen  
De volgende tabel bevat de statuscodes voor het verwerken van blob-eigenschappen.  
  
|Statuscode|Beschrijving|  
|-----------------|-----------------|  
|`Completed`|De eigenschappen verwerken zonder fouten is voltooid.|  
|`FileNameInvalid`|De bestandsnaam van de eigenschappen is ongeldig.|  
|`FileNameTooLong`|De eigenschappen-bestandsnaam is te lang.|  
|`FileNotFound`|Het eigenschappenbestand is niet gevonden.|  
|`FileAccessDenied`|Toegang tot het eigenschappenbestand is geweigerd.|  
|`Corrupted`|De eigenschappenbestand is beschadigd (de inhoud komt niet overeen met de hash).|  
|`XmlReadFailed`|De eigenschappen van inhoud voldoet niet aan de vereiste indeling.|  
|`XmlWriteFailed`|Schrijven van de eigenschappen is XML mislukt.|  
|`BlobRequestFailed`|De aanvraag voor toegang tot de eigenschappen voor de Blob is mislukt.|  
|`IOFailed`|Er is een schijf of het netwerk-i/o-fout opgetreden tijdens het verwerken van de eigenschappen.|  
|`Failed`|Er is een onbekende fout opgetreden tijdens het verwerken van de eigenschappen.|  
|`Cancelled`|Er is een eerdere fout gestopt verdere verwerking van de eigenschappen.|  
  
## <a name="sample-logs"></a>Voorbeeld-Logboeken  
Hier volgt een voorbeeld van uitgebreide logboek.  
  
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
  
Het foutenlogboek van het bijbehorende worden hieronder weergegeven.  
  
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

 Het foutenlogboek volgen voor een import-taak bevat een fout over een bestand niet gevonden op de schijf importeren. Denk eraan dat de status van de volgende onderdelen is `Cancelled`.  
  
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

Het volgende foutenlogboek voor een taak voor het exporteren geeft aan dat de blob-inhoud is geschreven naar de schijf, maar is een fout opgetreden tijdens het exporteren van de blob-eigenschappen.  
  
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
