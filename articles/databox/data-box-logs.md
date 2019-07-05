---
title: Bijhouden en meld u Azure Data Box, Azure Data Box zware gebeurtenissen | Microsoft Docs
description: Beschrijft hoe u bij te houden en gebeurtenissen in de verschillende stadia van uw order voor Azure Data Box en Azure Data Box zware.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 06/03/2019
ms.author: alkohli
ms.openlocfilehash: ba08cd7fdecda99c04d5bb1007b3e5f61cd1bd5c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446769"
---
# <a name="tracking-and-event-logging-for-your-azure-data-box-and-azure-data-box-heavy"></a>Tracering en logboekregistratie voor uw Azure Data Box en Azure Data Box-zwaar

Een Data Box of gegevens in het zware order verloopt via de volgende stappen uit: bestellen, instellen van, gegevens kopiëren, retourneren, uploaden naar Azure en controleren, en de verwijdering van gegevens. Overeenkomt met elke stap in de volgorde, kunt u meerdere acties ondernemen voor het beheren van de toegang tot de volgorde, de controlegebeurtenissen, bijhouden van de volgorde en interpreteren van de verschillende logboeken die worden gegenereerd.

De volgende tabel bevat een overzicht van de volgorde Data Box of gegevens in het zware werk en de hulpprogramma's die beschikbaar zijn voor het volgen en controleren van de volgorde tijdens elke stap.

| Data Box-order fase       | Hulpprogramma voor het volgen en controleren                                                                        |
|----------------------------|------------------------------------------------------------------------------------------------|
| Order maken               | [Instellen van toegangsbeheer voor de order via RBAC](#set-up-access-control-on-the-order)                                                    |
| Volgorde verwerkt            | [Bijhouden van de volgorde](#track-the-order) via <ul><li> Azure Portal </li><li> Verzending van de website van de provider </li><li>E-mailmeldingen</ul> |
| Apparaat instellen              | Apparaat referenties toegang aangemeld [activiteitenlogboeken](#query-activity-logs-during-setup)                                              |
| Gegevens kopiëren naar apparaat        | [Weergave *error.xml* bestanden](#view-error-log-during-data-copy) voor het kopiëren van gegevens                                                             |
| Voorbereiding voor verzending            | [De bestanden stuklijst controleren](#inspect-bom-during-prepare-to-ship) of de manifest-bestanden op het apparaat                                      |
| Het uploaden van gegevens naar Azure       | [Beoordeling *copylogs* ](#review-copy-log-during-upload-to-azure) voor fouten tijdens gegevens uploaden in Azure-datacenter                         |
| Gegevens wissen van apparaat   | [Reeks bewaring logboeken bekijken](#get-chain-of-custody-logs-after-data-erasure) met inbegrip van controlelogboeken en bestellen geschiedenis                |

Dit artikel wordt beschreven in de details van de verschillende hulpprogramma's beschikbaar om te volgen en controleren van de Data Box-of gegevens in het zware of mechanismen. De informatie in dit artikel geldt voor zowel, Data Box en gegevens in het zware. In de volgende secties worden uitgelegd, worden alle verwijzingen naar Data Box ook toepassen op gegevens in het zware.

## <a name="set-up-access-control-on-the-order"></a>Toegangsbeheer voor de volgorde instellen

U kunt bepalen wie toegang heeft tot uw order voor wanneer de order is gemaakt. Instellen van rollen gebaseerd toegangsbeheer (RBAC) functies in verschillende bereiken voor het beheren van de toegang tot de Data Box-order. Een RBAC-rol bepaalt het type toegang: lezen / schrijven, alleen-lezen, lezen / schrijven naar een subset van bewerkingen.

De twee rollen die kunnen worden gedefinieerd voor de Azure Data Box-service zijn:

- **In het gegevenslezer** -alleen-lezen toegang tot een order (s) hebt, zoals gedefinieerd door het bereik. Ze kunnen alleen gegevens van een order weergeven. Ze kunnen toegang krijgen tot andere details met betrekking tot de storage-accounts of de details van de order zoals adres enzovoort bewerken.
- **Inzender voor Data Box** -kan alleen een order gegevens overdraagt naar een bepaalde storage-account maken *als ze nog toegang voor schrijven naar een opslagaccount*. Zij kunnen niet ze geen toegang tot een opslagaccount, zelfs een Data Box-order om gegevens te kopiëren naar het account te maken. Deze rol definieert een Storage-account niet machtigingen noch verleent toegang tot die betrekking hebben op storage-accounts.  

Als u wilt toegang beperken tot een order, kunt u het volgende doen:

- Een rol op het niveau van een order toewijzen. De gebruiker heeft alleen de machtigingen zoals gedefinieerd door de rollen om te communiceren met die specifieke Data Box-order alleen en niets anders.
- Een rol op het niveau van de resource toe te wijzen, de gebruiker toegang heeft tot de Data Box-orders binnen een resourcegroep.

Zie voor meer informatie over voorgestelde RBAC gebruik [aanbevolen procedures voor RBAC](../role-based-access-control/overview.md#best-practice-for-using-rbac).

## <a name="track-the-order"></a>De bestelling volgen

U kunt uw bestelling via Azure portal en via de website van de verzending carrier bijhouden. De volgende mechanismen zijn voor het bijhouden van de Data Box-order op elk gewenst moment:

- Voor het volgen van de volgorde wanneer het apparaat zich in de Azure-datacenter of uw locatie, gaat u naar uw **Data Box-order > overzicht** in Azure portal.

    ![De status en wijzigingen bijhouden niet weergeven](media/data-box-logs/overview-view-status-1.png)

- Voor het volgen van de volgorde terwijl het apparaat onderweg zijn, gaat u naar de website van de regionale luchtvaartmaatschappij, bijvoorbeeld UPS website in VS. Geef het aantal dat is gekoppeld aan uw order.
- Data Box verzendt ook e-mailmeldingen telkens wanneer de status gewijzigd op basis van de e-mailberichten wanneer de order is gemaakt. Zie voor een lijst van alle statussen van de Data Box, [weer de status van](data-box-portal-admin.md#view-order-status). Zie het wijzigen van de instellingen voor meldingen die zijn gekoppeld aan de order [Meldingsdetails bewerken](data-box-portal-admin.md#edit-notification-details).

## <a name="query-activity-logs-during-setup"></a>Query-activiteitenlogboeken tijdens de installatie

- Uw Data Box wordt ontvangen op uw locatie in een vergrendelde status. U kunt referenties van het apparaat beschikbaar in de Azure portal gebruiken voor uw order.  

    Wanneer een Data Box is ingesteld, moet u mogelijk weten wie alle referenties van het apparaat heeft geopend. Om te achterhalen die toegang krijgen tot de **apparaatreferenties** blade kunt u de activiteitenlogboeken opvragen.  Een actie op die toegang tot omvat **Apparaatdetails > referenties** blade is aangemeld bij de activiteitenlogboeken als `ListCredentials` actie.

    ![Logboeken met queryactiviteit](media/data-box-logs/query-activity-log-1.png)

- Elke Meld u aan bij de Data Box is aangemeld realtime. Deze informatie is echter alleen beschikbaar in de [auditlogboeken](#audit-logs) nadat de order is voltooid.

## <a name="view-error-log-during-data-copy"></a>Foutenlogboek weergeven tijdens het kopiëren van gegevens

Tijdens het kopiëren van gegevens naar Data Box of gegevens in het zware, is een fout bij het bestand gegenereerd als er zich problemen voordoen met de gegevens worden gekopieerd.

### <a name="errorxml-file"></a>Error.XML bestand

Zorg ervoor dat de kopie-taken hebt voltooid zonder fouten. Als er fouten tijdens het kopiëren zijn, downloadt u de logboeken van de **verbinding maken en kopiëren** pagina.

- Als u een bestand dat niet 512 bytes uitgelijnd op een beheerde schijf-map op uw Data Box is gekopieerd, wordt het bestand is niet geüpload als pagina-blob naar de staging storage-account. Hier ziet u een fout in de logboeken. Verwijder het bestand en kopieer een bestand dat is 512 bytes uitgelijnd.
- Als u een VHDX- of een dynamische VHD- of een differentiërende VHD (deze bestanden worden niet ondersteund) hebt gekopieerd, ziet u een fout in de logboeken.

Hier volgt een voorbeeld van de *error.xml* voor verschillende fouten bij het kopiëren naar beheerde schijven.

```xml
<file error="ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED">\StandardHDD\testvhds\differencing-vhd-022019.vhd</file>
<file error="ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED">\StandardHDD\testvhds\dynamic-vhd-022019.vhd</file>
<file error="ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED">\StandardHDD\testvhds\insidefixedvhdx-022019.vhdx</file>
<file error="ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED">\StandardHDD\testvhds\insidediffvhd-022019.vhd</file>
```

Hier volgt een voorbeeld van de *error.xml* voor verschillende fouten bij het kopiëren naar de pagina-blobs.

```xml
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File100Bytes</file>
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File786Bytes</file>
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File513Bytes</file>
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File10Bytes</file>
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File500Bytes</file>
```


Hier volgt een voorbeeld van de *error.xml* voor verschillende fouten bij het kopiëren naar een blok-blobs.

```xml
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\ab</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\invalid dns name</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\morethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortesting</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\testdirectory-~!@#$%^&amp;()_+{}</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\test__doubleunderscore</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\-startingwith-hyphen</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\Starting with Capital</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\_startingwith_underscore</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\55555555--4444--3333--2222--111111111111</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\1</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\11111111-_2222-_3333-_4444-_555555555555</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\test--doublehyphen</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTI5Ni3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwMS3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwMy3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_CONTROL">\InvalidUnicodeFiles\Ã.txt</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwNS3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTI5OS3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwMi3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwNC3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTI5OC3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwMC3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTI5Ny3vv70=</file>
```

Hier volgt een voorbeeld van de *error.xml* voor verschillende fouten bij het kopiëren naar Azure Files.

```xml
<file error="ERROR_BLOB_OR_FILE_SIZE_LIMIT">\AzFileMorethan1TB\AzFile1.2TB</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\testdirectory-~!@#$%^&amp;()_+{}</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\55555555--4444--3333--2222--111111111111</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\-startingwith-hyphen</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\11111111-_2222-_3333-_4444-_555555555555</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\test--doublehyphen</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\ab</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\invalid dns name</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\test__doubleunderscore</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\morethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortesting</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\_startingwith_underscore</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\1</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\Starting with Capital</file>
```

In elk van de bovenstaande gevallen moet u de fouten oplossen voordat u met de volgende stap doorgaat. Voor meer informatie over de fouten die zijn ontvangen tijdens het kopiëren van gegevens naar Data Box via SMB- of NFS-protocol, gaat u naar [problemen oplossen van Data Box en gegevens in het zware](data-box-troubleshoot.md). Voor informatie over fouten die zijn ontvangen tijdens het kopiëren van gegevens naar Data Box via REST, Ga naar [Blob voor gegevens in het oplossen van opslagproblemen](data-box-troubleshoot-rest.md).

## <a name="inspect-bom-during-prepare-to-ship"></a>Stuklijst controleren tijdens de voorbereiding voor verzending

Tijdens de voorbereiding voor verzending, een lijst met bestanden die bekend staat als de factuur van of Materials (BOM) of een manifest-bestand wordt gemaakt.

- Dit bestand gebruiken om te controleren op basis van de werkelijke namen en het aantal bestanden die zijn gekopieerd naar de Data Box.
- Dit bestand gebruiken om te controleren op basis van de werkelijke grootte van de bestanden.
- Controleer de *crc64* overeenkomt met een niet-nul-tekenreeks. <!--A null value for crc64 indicates that there was a reparse point error)-->

Voor meer informatie over de fouten die zijn ontvangen tijdens het voorbereiden van het verzenden, gaat u naar [problemen oplossen van Data Box en gegevens in het zware](data-box-troubleshoot.md).

### <a name="bom-or-manifest-file"></a>Stuklijst of manifest-bestand

De stuklijst of de manifest-bestand bevat de lijst van alle bestanden die zijn gekopieerd naar de Data Box-apparaat. Het bestand stuklijst heeft bestandsnamen en de bijbehorende grootten, evenals de controlesom. Een afzonderlijke stuklijst-bestand wordt gemaakt voor de blok-blobs, pagina-blobs, Azure Files, voor kopiëren via de REST API's, en voor het kopiëren naar beheerde schijven op de Data Box. U kunt de bestanden stuklijst downloaden via de lokale webgebruikersinterface van het apparaat tijdens de voorbereiding voor verzending.

Deze bestanden bevinden zich op de Data Box-apparaat ook en worden geüpload naar het bijbehorende opslagaccount in de Azure-datacenter.

### <a name="bom-file-format"></a>Stuklijst-bestandsindeling

Stuklijst of manifest-bestand heeft de volgende algemene indeling:

`<file size = "file-size-in-bytes" crc64="cyclic-redundancy-check-string">\folder-path-on-data-box\name-of-file-copied.md</file>`

Hier volgt een voorbeeld van een manifest gegenereerd wanneer de gegevens zijn gekopieerd naar de blok-blob-share op de Data Box.

```
<file size="10923" crc64="0x51c78833c90e4e3f">\databox\media\data-box-deploy-copy-data\connect-shares-file-explorer1.png</file>
<file size="15308" crc64="0x091a8b2c7a3bcf0a">\databox\media\data-box-deploy-copy-data\get-share-credentials2.png</file>
<file size="53486" crc64="0x053da912fb45675f">\databox\media\data-box-deploy-copy-data\nfs-client-access.png</file>
<file size="6093" crc64="0xadb61d0d7c6d4deb">\databox\data-box-cable-options.md</file>
<file size="6499" crc64="0x080add29add367d9">\databox\data-box-deploy-copy-data-via-nfs.md</file>
<file size="11089" crc64="0xc3ce6b13a4fe3001">\databox\data-box-deploy-copy-data-via-rest.md</file>
<file size="7749" crc64="0xd2e346a4588e307a">\databox\data-box-deploy-ordered.md</file>
<file size="14275" crc64="0x260296e5d1b1608a">\databox\data-box-deploy-copy-data.md</file>
<file size="4077" crc64="0x2bb0a170225bceec">\databox\data-box-deploy-picked-up.md</file>
<file size="15447" crc64="0xcec0ca8527720b3c">\databox\data-box-portal-admin.md</file>
<file size="9126" crc64="0x820856b5a54321ad">\databox\data-box-overview.md</file>
<file size="10963" crc64="0x5e9a14f9f4784fd8">\databox\data-box-safety.md</file>
<file size="5941" crc64="0x8631d62fbc038760">\databox\data-box-security.md</file>
<file size="12536" crc64="0x8c8ff93e73d665ec">\databox\data-box-system-requirements-rest.md</file>
<file size="3220" crc64="0x7257a263c434839a">\databox\data-box-system-requirements.md</file>
```

De stuklijst of manifestbestanden zijn ook gekopieerd naar de Azure storage-account. U kunt de stuklijst gebruiken of manifest van de bestanden om te controleren dat bestanden die zijn geüpload naar Azure overeenkomen met de gegevens die zijn gekopieerd naar de Data Box.

## <a name="review-copy-log-during-upload-to-azure"></a>Bekijk kopie logboek tijdens het uploaden naar Azure

Tijdens het uploaden van gegevens naar Azure, een *copylog* wordt gemaakt.

### <a name="copylog"></a>Copylog

Voor elke order dat wordt verwerkt, maakt de Data Box-service *copylog* in het gekoppelde opslagaccount. De *copylog* is het totale aantal bestanden die zijn geüpload en het aantal bestanden dat met fouten uit tijdens de gegevens van Data Box kopiëren naar uw Azure storage-account.

De berekening van een cyclische redundantie controleren (CRC) wordt uitgevoerd tijdens het uploaden naar Azure. De CRC's van het kopiëren van gegevens en nadat het uploaden van gegevens met elkaar worden vergeleken. Een niet-overeenkomend CRC geeft aan dat de bijbehorende bestanden kunnen niet uploaden.

Standaard logboeken worden geschreven naar een container met de naam `copylog`. De logboeken worden opgeslagen met de volgende naamconventie gebruikt:

`storage-account-name/databoxcopylog/ordername_device-serial-number_CopyLog_guid.xml`.

Het pad copylog wordt ook weergegeven op de **overzicht** blade voor de portal.

![Pad naar copylog in de blade overzicht wanneer voltooid](media/data-box-logs/copy-log-path-1.png)

Het volgende voorbeeld wordt de algemene indeling van een bestand copylog beschreven voor het uploaden van een Data Box is voltooid:

```
<?xml version="1.0"?>
-<CopyLog Summary="Summary">
<Status>Succeeded</Status>
<TotalFiles>45</TotalFiles>
<FilesErrored>0</FilesErrored>
</CopyLog>
```

Uploaden naar Azure kan ook voltooid met fouten.

![Pad naar copylog in de blade overzicht wanneer dit is voltooid met fouten](media/data-box-logs/copy-log-path-2.png)

Hier volgt een voorbeeld van een copylog waar het uploaden is voltooid met fouten:

```xml
<ErroredEntity Path="iso\samsungssd.iso">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>409</ErrorCode>
  <ErrorMessage>The blob type is invalid for this operation.</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><ErroredEntity Path="iso\iSCSI_Software_Target_33.iso">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>409</ErrorCode>
  <ErrorMessage>The blob type is invalid for this operation.</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><CopyLog Summary="Summary">
  <Status>Failed</Status>
  <TotalFiles_Blobs>72</TotalFiles_Blobs>
  <FilesErrored>2</FilesErrored>
</CopyLog>
```
Hier volgt een voorbeeld van een `copylog` waar de containers die niet aan de naamgevingsconventies voor Azure voldoet zijn gewijzigd tijdens het uploaden van gegevens naar Azure.

De nieuwe unieke namen voor containers hebben de indeling `DataBox-GUID` en de gegevens voor de container in de nieuwe hernoemd container zijn geplaatst. De `copylog` Hiermee geeft u de oude en de nieuwe naam voor de container.

```xml
<ErroredEntity Path="New Folder">
   <Category>ContainerRenamed</Category>
   <ErrorCode>1</ErrorCode>
   <ErrorMessage>The original container/share/blob has been renamed to: DataBox-3fcd02de-bee6-471e-ac62-33d60317c576 :from: New Folder :because either the name has invalid character(s) or length is not supported</ErrorMessage>
  <Type>Container</Type>
</ErroredEntity>
```

Hier volgt een voorbeeld van een `copylog` waar de blobs of bestanden die niet aan de naamgevingsregels van Azure voldoet, zijn gewijzigd tijdens het uploaden van gegevens naar Azure. De nieuwe blob of bestandsnamen worden geconverteerd naar SHA256-samenvatting van het relatieve pad naar de container en worden geüpload naar het pad op basis van het doeltype. Het doel mag blok-blobs, pagina-blobs of Azure Files.

De `copylog` Hiermee geeft u de oude en nieuwe naam van de blob of file en het pad in Azure.

```xml
<ErroredEntity Path="TesDir028b4ba9-2426-4e50-9ed1-8e89bf30d285\Ã">
  <Category>BlobRenamed</Category>
  <ErrorCode>1</ErrorCode>
  <ErrorMessage>The original container/share/blob has been renamed to: PageBlob/DataBox-0xcdc5c61692e5d63af53a3cb5473e5200915e17b294683968a286c0228054f10e :from: Ã :because either name has invalid character(s) or length is not supported</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><ErroredEntity Path="TesDir9856b9ab-6acb-4bc3-8717-9a898bdb1f8c\Ã">
  <Category>BlobRenamed</Category>
  <ErrorCode>1</ErrorCode>
  <ErrorMessage>The original container/share/blob has been renamed to: AzureFile/DataBox-0xcdc5c61692e5d63af53a3cb5473e5200915e17b294683968a286c0228054f10e :from: Ã :because either name has invalid character(s) or length is not supported</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><ErroredEntity Path="TesDirf92f6ca4-3828-4338-840b-398b967d810b\Ã">
  <Category>BlobRenamed</Category>
  <ErrorCode>1</ErrorCode>
  <ErrorMessage>The original container/share/blob has been renamed to: BlockBlob/DataBox-0xcdc5c61692e5d63af53a3cb5473e5200915e17b294683968a286c0228054f10e :from: Ã :because either name has invalid character(s) or length is not supported</ErrorMessage>
  <Type>File</Type>
</ErroredEntity>
```

## <a name="get-chain-of-custody-logs-after-data-erasure"></a>Reeks bewaring Logboeken na het verwijderen van gegevens ophalen

Nadat de gegevens van de Data Box-schijven aan de hand van de richtlijnen van SP NIST 800-88 revisie 1 is gewist, wordt de keten van bewaring logboeken beschikbaar zijn. Deze logboeken bevatten de logboeken voor controle en de ordergeschiedenis van de. De stuklijst of manifestbestanden worden ook gekopieerd met de auditlogboeken.

### <a name="audit-logs"></a>Auditlogboeken

Auditlogboeken bevatten informatie over power-on en toegang tot de Data Box of de gegevens in het zware delen wanneer deze buiten Azure-datacenter. Deze logboeken bevinden zich op: `storage-account/azuredatabox-chainofcustodylogs`

Hier volgt een voorbeeld van het controlelogboek van een Data Box:

```
9/10/2018 8:23:01 PM : The operating system started at system time ‎2018‎-‎09‎-‎10T20:23:01.497758400Z.
9/10/2018 8:23:42 PM : An account was successfully logged on.
Subject:
    Security ID:        S-1-5-18
    Account Name:       WIN-DATABOXADMIN
    Account Domain: Workgroup
    Logon ID:       0x3E7
Logon Information:
    Logon Type:     3
    Restricted Admin Mode:  -
    Virtual Account:        No
    Elevated Token:     No
Impersonation Level:        Impersonation
New Logon:
    Security ID:        S-1-5-7
    Account Name:       ANONYMOUS LOGON
    Account Domain: NT AUTHORITY
    Logon ID:       0x775D5
    Linked Logon ID:    0x0
    Network Account Name:   -
    Network Account Domain: -
    Logon GUID:     {00000000-0000-0000-0000-000000000000}
Process Information:
    Process ID:     0x4
    Process Name:       
Network Information:
    Workstation Name:   -
    Source Network Address: -
    Source Port:        -
Detailed Authentication Information:
    Logon Process:      NfsSvr
    Authentication Package:MICROSOFT_AUTHENTICATION_PACKAGE_V1_0
    Transited Services: -
    Package Name (NTLM only):   -
    Key Length:     0
This event is generated when a logon session is created. It is generated on the computer that was accessed. 
The subject fields indicate the account on the local system which requested the logon. This is most commonly a service such as the Server service, or a local process such as Winlogon.exe or Services.exe. 
The logon type field indicates the kind of logon that occurred. The most common types are 2 (interactive) and 3 (network).
The New Logon fields indicate the account for whom the new logon was created, i.e. the account that was logged on.
The network fields indicate where a remote logon request originated. Workstation name is not always available and may be left blank in some cases.
The impersonation level field indicates the extent to which a process in the logon session can impersonate.
The authentication information fields provide detailed information about this specific logon request.
    - Logon GUID is a unique identifier that can be used to correlate this event with a KDC event.
    - Transited services indicate which intermediate services have participated in this logon request.
    - Package name indicates which sub-protocol was used among the NTLM protocols.
    - Key length indicates the length of the generated session key. This will be 0 if no session key was requested.
9/10/2018 8:25:58 PM : An account was successfully logged on.
```


## <a name="download-order-history"></a>Ordergeschiedenis downloaden

Ordergeschiedenis is beschikbaar in Azure portal. Als de volgorde voltooid is en de apparaten opschonen (verwijderen van gegevens van de schijven) voltooid is, gaat u naar uw order voor een apparaat en navigeer naar **detailgegevens Order**. ** Download ordergeschiedenis** optie beschikbaar is. Zie voor meer informatie, [downloaden ordergeschiedenis](data-box-portal-admin.md#download-order-history).

Als u de geschiedenis volgorde doorloopt, moet u het volgende zien:

- Mobiele provider het bijhouden van informatie voor uw apparaat.
- Gebeurtenissen met *SecureErase* activiteit. Deze gebeurtenissen komen overeen met de verwijdering van de gegevens op de schijf.
- Data Box log koppelingen. De paden voor de *auditlogboeken*, *copylogs*, en *stuklijst* bestanden worden weergegeven.

Hier volgt een voorbeeld van het logboek van de geschiedenis volgorde vanuit Azure-portal:

```
-------------------------------
Microsoft Data Box Order Report
-------------------------------
Name                                               : gus-poland                              
StartTime(UTC)                              : 9/19/2018 8:49:23 AM +00:00                       
DeviceType                                     : DataBox                                           
-------------------
Data Box Activities
-------------------
Time(UTC)                 | Activity                       | Status          | Description

9/19/2018 8:49:26 AM      | OrderCreated                   | Completed       |
10/2/2018 7:32:53 AM      | DevicePrepared                 | Completed       |
10/3/2018 1:36:43 PM      | ShippingToCustomer             | InProgress      | Shipment picked up. Local Time : 10/3/2018 1:36:43 PM at AMSTERDAM-NLD                                                                                
10/4/2018 8:23:30 PM      | ShippingToCustomer             | InProgress      | Processed at AMSTERDAM-NLD. Local Time : 10/4/2018 8:23:30 PM at AMSTERDAM-NLD                                                                        
10/4/2018 11:43:34 PM     | ShippingToCustomer             | InProgress      | Departed Facility in AMSTERDAM-NLD. Local Time : 10/4/2018 11:43:34 PM at AMSTERDAM-NLD
10/5/2018 8:13:49 AM      | ShippingToCustomer             | InProgress      | Arrived at Delivery Facility in BRIGHTON-GBR. Local Time : 10/5/2018 8:13:49 AM at LAMBETH-GBR                                                         
10/5/2018 9:13:24 AM      | ShippingToCustomer             | InProgress      | With delivery courier. Local Time : 10/5/2018 9:13:24 AM at BRIGHTON-GBR                                                                               
10/5/2018 12:03:04 PM     | ShippingToCustomer             | Completed       | Delivered - Signed for by. Local Time : 10/5/2018 12:03:04 PM at BRIGHTON-GBR                                                                          
1/25/2019 3:19:25 PM      | ShippingToDataCenter           | InProgress      | Shipment picked up. Local Time : 1/25/2019 3:19:25 PM at BRIGHTON-GBR                                                                                       
1/25/2019 8:03:55 PM      | ShippingToDataCenter           | InProgress      | Processed at BRIGHTON-GBR. Local Time : 1/25/2019 8:03:55 PM at LAMBETH-GBR                                                                            
1/25/2019 8:04:58 PM      | ShippingToDataCenter           | InProgress      | Departed Facility in BRIGHTON-GBR. Local Time : 1/25/2019 8:04:58 PM at BRIGHTON-GBR                                                                    
1/25/2019 9:06:09 PM      | ShippingToDataCenter           | InProgress      | Arrived at Sort Facility LONDON-HEATHROW-GBR. Local Time : 1/25/2019 9:06:09 PM at LONDON-HEATHROW-GBR                                                
1/25/2019 9:48:54 PM      | ShippingToDataCenter           | InProgress      | Processed at LONDON-HEATHROW-GBR. Local Time : 1/25/2019 9:48:54 PM at LONDON-HEATHROW-GBR                                                            
1/25/2019 10:30:20 PM     | ShippingToDataCenter           | InProgress      | Departed Facility in LONDON-HEATHROW-GBR. Local Time : 1/25/2019 10:30:20 PM at LONDON-HEATHROW-GBR
1/28/2019 7:11:35 AM      | ShippingToDataCenter           | InProgress      | Arrived at Delivery Facility in AMSTERDAM-NLD. Local Time : 1/28/2019 7:11:35 AM at AMSTERDAM-NLD                                                     
1/28/2019 9:07:57 AM      | ShippingToDataCenter           | InProgress      | With delivery courier. Local Time : 1/28/2019 9:07:57 AM at AMSTERDAM-NLD                                                                             
1/28/2019 1:35:56 PM      | ShippingToDataCenter           | InProgress      | Scheduled for delivery. Local Time : 1/28/2019 1:35:56 PM at AMSTERDAM-NLD                                                                            
1/28/2019 2:57:48 PM      | ShippingToDataCenter           | Completed       | Delivered - Signed for by. Local Time : 1/28/2019 2:57:48 PM at AMSTERDAM-NLD
1/29/2019 2:18:43 PM      | PhysicalVerification           | Completed       |
1/29/2019 3:49:50 PM      | DeviceBoot                     | Completed       | Appliance booted up successfully.
1/29/2019 3:49:51 PM      | AnomalyDetection               | Completed       | No anomaly detected.
2/12/2019 10:37:03 PM     | DataCopy                       | Resumed         |
2/13/2019 12:05:15 AM     | DataCopy                       | Resumed         |
2/15/2019 7:07:34 PM      | DataCopy                       | Completed       | Copy Completed.
2/15/2019 7:47:32 PM      | SecureErase                    | Started         |
2/15/2019 8:01:10 PM      | SecureErase                    | Completed       | Azure Data Box:<Device-serial-no> has been sanitized according to NIST 800-88 Rev 1.
------------------
Data Box Log Links
------------------
Account Name         : gusacct
Copy Logs Path       : databoxcopylog/gus-poland_<Device-serial-no>_CopyLog_<GUID>.xml
Audit Logs Path      : azuredatabox-chainofcustodylogs\<GUID>\<Device-serial-no>
BOM Files Path       : azuredatabox-chainofcustodylogs\<GUID>\<Device-serial-no>
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [oplossen van problemen in uw Data Box en de gegevens in het zware](data-box-troubleshoot.md).
