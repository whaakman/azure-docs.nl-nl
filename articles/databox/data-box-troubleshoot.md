---
title: Oplossen van problemen in uw Azure Data Box, Azure Data Box zware | Microsoft Docs
description: Beschrijft hoe u problemen in Azure Data Box en Azure Data Box zware gezien bij het kopiëren van gegevens op deze apparaten.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 06/24/2019
ms.author: alkohli
ms.openlocfilehash: bc0681a8ea15f736a7b253d6bd7ba2f7928d2a32
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439402"
---
# <a name="troubleshoot-issues-related-to-azure-data-box-and-azure-data-box-heavy"></a>Oplossen van problemen met betrekking tot Azure Data Box en Azure Data Box-zwaar

In dit artikel bevat gedetailleerde informatie over het oplossen van problemen die mogelijk ziet u wanneer u de Azure Data Box of Azure Data Box zware. Het artikel bevat de lijst met mogelijke fouten gezien als gegevens worden gekopieerd naar de Data Box, of wanneer gegevens van Data Box is geüpload.

## <a name="error-classes"></a>Foutklassen

De fouten in de Data Box en gegevens in het zware worden als volgt samengevat:

| Fout bij categorie *        | Description        | Aanbevolen actie    |
|----------------------------------------------|---------|--------------------------------------|
| Namen van container of bestandsshare | De namen van de container of bestandsshare Volg niet de naamgevingsregels voor Azure.  |Download de lijsten met fouten. <br> Wijzig de naam van de containers of shares. [Meer informatie](#container-or-share-name-errors).  |
| Container of bestandsshare maximale grootte | De totale hoeveelheid gegevens in containers of shares overschrijdt de limiet van Azure.   |Download de lijsten met fouten. <br> Verminder de algemene gegevens in de container of de share. [Meer informatie](#container-or-share-size-limit-errors).|
| Object of bestand maximale grootte | Het object of de bestanden in containers of shares overschrijdt de limiet van Azure.|Download de lijsten met fouten. <br> Verklein de bestandsgrootte in de container of de share. [Meer informatie](#object-or-file-size-limit-errors). |    
| Type gegevensopslag of file | De gegevensindeling of het bestandstype wordt niet ondersteund. |Download de lijsten met fouten. <br> Controleer of dat de gegevens is 512-bytes uitgelijnd en gekopieerd naar de vooraf gemaakte mappen voor pagina-blobs of beheerde schijven. [Meer informatie](#data-or-file-type-errors). |
| Niet-kritieke blob of file-fouten  | De namen van de blob of file Volg niet de naamgevingsregels van Azure of het bestandstype wordt niet ondersteund. | Deze blob of bestanden kunnen niet worden gekopieerd of de namen kunnen worden gewijzigd. [Informatie over het oplossen van deze fouten](#non-critical-blob-or-file-errors). |

\* De eerste vier foutcategorieën kritieke fouten zijn en moeten worden opgelost voordat u kunt doorgaan met de voorbereiding voor verzending.


## <a name="container-or-share-name-errors"></a>Fouten in de container of bestandsshare namen

Dit zijn fouten met betrekking tot de container en sharenamen.

### <a name="errorcontainerorsharenamelength"></a>ERROR_CONTAINER_OR_SHARE_NAME_LENGTH     

**Foutbeschrijving:** De naam van de container of share moet bestaan uit 3 tot 63 tekens. 

**Voorgestelde oplossing:** De map onder de Data Box of gegevens in het zware share(SMB/NFS) waarnaar u gegevens hebt gekopieerd, wordt een Azure-container in uw opslagaccount. 

- Op de **verbinding maken en kopiëren** pagina van de lokale webinterface van apparaat, downloaden en controleer de namen van de foutbestanden voor het identificeren van de map met problemen.
- Wijzig de naam van de map in de Data Box of gegevens in het zware bestandsshare om ervoor te zorgen dat:

    - De naam is tussen de 3 en 63 tekens.
    - De namen kunnen alleen letters, cijfers en afbreekstreepjes bevatten hebben.
    - De naam mag niet beginnen of eindigen met een afbreekstreepje.
    - De namen geen opeenvolgende afbreekstreepjes bevatten.
    - Voorbeelden van geldige namen: `my-folder-1`, `my-really-extra-long-folder-111`
    - Voorbeelden van namen die niet geldig: `my-folder_1`, `my`, `--myfolder`, `myfolder--`, `myfolder!`

    Zie voor meer informatie, de Azure naamgevingsregels voor [containernamen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#container-names) en [sharenamen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#share-names).


### <a name="errorcontainerorsharenamealphanumericdash"></a>ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH

**Foutbeschrijving:** De naam van de container of share mag alleen letters, cijfers of afbreekstreepjes bevatten.

**Voorgestelde oplossing:** De map onder de Data Box of gegevens in het zware share(SMB/NFS) waarnaar u gegevens hebt gekopieerd, wordt een Azure-container in uw opslagaccount. 

- Op de **verbinding maken en kopiëren** pagina van de lokale webinterface van apparaat, downloaden en controleer de namen van de foutbestanden voor het identificeren van de map met problemen.
- Wijzig de naam van de map in de Data Box of gegevens in het zware bestandsshare om ervoor te zorgen dat:

    - De naam is tussen de 3 en 63 tekens.
    - De namen kunnen alleen letters, cijfers en afbreekstreepjes bevatten hebben.
    - De naam mag niet beginnen of eindigen met een afbreekstreepje.
    - De namen geen opeenvolgende afbreekstreepjes bevatten.
    - Voorbeelden van geldige namen: `my-folder-1`, `my-really-extra-long-folder-111`
    - Voorbeelden van namen die niet geldig: `my-folder_1`, `my`, `--myfolder`, `myfolder--`, `myfolder!`

    Zie voor meer informatie, de Azure naamgevingsregels voor [containernamen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#container-names) en [sharenamen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#share-names).

### <a name="errorcontainerorsharenameimproperdash"></a>ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH

**Foutbeschrijving:** De namen van containers en de namen van shares mag niet beginnen of eindigen met een afbreekstreepje en geen opeenvolgende afbreekstreepjes bevatten.

**Voorgestelde oplossing:** De map onder de Data Box of gegevens in het zware share(SMB/NFS) waarnaar u gegevens hebt gekopieerd, wordt een Azure-container in uw opslagaccount. 

- Op de **verbinding maken en kopiëren** pagina van de lokale webinterface van apparaat, downloaden en controleer de namen van de foutbestanden voor het identificeren van de map met problemen.
- Wijzig de naam van de map in de Data Box of gegevens in het zware bestandsshare om ervoor te zorgen dat:

    - De naam is tussen de 3 en 63 tekens.
    - De namen kunnen alleen letters, cijfers en afbreekstreepjes bevatten hebben.
    - De naam mag niet beginnen of eindigen met een afbreekstreepje.
    - De namen geen opeenvolgende afbreekstreepjes bevatten.
    - Voorbeelden van geldige namen: `my-folder-1`, `my-really-extra-long-folder-111`
    - Voorbeelden van namen die niet geldig: `my-folder_1`, `my`, `--myfolder`, `myfolder--`, `myfolder!`

    Zie voor meer informatie, de Azure naamgevingsregels voor [containernamen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#container-names) en [sharenamen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#share-names).

## <a name="container-or-share-size-limit-errors"></a>Container of bestandsshare grootte limiet fouten

Dit zijn fouten met betrekking tot de gegevens van meer dan de hoeveelheid gegevens die zijn toegestaan in een container of een share.

### <a name="errorcontainerorsharecapacityexceeded"></a>ERROR_CONTAINER_OR_SHARE_CAPACITY_EXCEEDED

**Foutbeschrijving:** Azure-bestandsshare een share naar 5 TB aan gegevens wordt beperkt. Deze limiet is overschreden voor sommige bestandsshares.

**Voorgestelde oplossing:** Op de **verbinding maken en kopiëren** pagina van de lokale webgebruikersinterface downloaden, en controleert u de fout.

Identificeer de mappen die dit probleem van de foutenlogboeken en zorg ervoor dat de bestanden in die map onder 5 TB zijn.


## <a name="object-or-file-size-limit-errors"></a>Object of bestand grootte limiet fouten

Dit zijn fouten met betrekking tot de gegevens die overschrijden de maximale grootte van het object of het bestand dat is toegestaan in Azure. 

### <a name="errorbloborfilesizelimit"></a>ERROR_BLOB_OR_FILE_SIZE_LIMIT

**Foutbeschrijving:** De bestandsgrootte overschrijdt de maximale bestandsgrootte voor uploaden.

**Voorgestelde oplossing:** De blob of de grootte groter zijn dan de maximale limiet voor het uploaden is toegestaan.

- Op de **verbinding maken en kopiëren** pagina van de lokale webgebruikersinterface downloaden, en controleert u de fout.
- Zorg ervoor dat de grootte van de blob en bestand niet groter zijn dan de grenzen van de grootte van Azure-object.

## <a name="data-or-file-type-errors"></a>Gegevensopslag of file type fouten

Dit zijn fouten met betrekking tot niet-ondersteund bestandstype of gegevenstype gevonden in de container of de share. 

### <a name="errorbloborfilesizealignment"></a>ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT

**Foutbeschrijving:** De blob of het bestand is niet juist uitgelijnd.

**Voorgestelde oplossing:** De pagina-blob-share op Data Box of gegevens in het zware alleen ondersteunt de bestanden die 512 bytes zijn uitgelijnd (bijvoorbeeld VHD/VHDX). Geen gegevens gekopieerd naar de pagina-blob-share is geüpload naar Azure als pagina-blobs.

Verwijder alle niet-VHD/VHDX-gegevens van de pagina-blob-share. U kunt bestandsshares voor blok-blob of Azure-bestanden voor algemene gegevens.

Zie voor meer informatie, [overzicht van pagina-blobs](../storage/blobs/storage-blob-pageblob-overview.md).

### <a name="errorbloborfiletypeunsupported"></a>ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED

**Foutbeschrijving:** Een niet-ondersteund bestandstype is aanwezig in een beheerde schijf-share. Alleen vaste VHD's zijn toegestaan.

**Voorgestelde oplossing:**

- Zorg ervoor dat alleen de vaste VHD's voor het maken van beheerde schijven te uploaden.
- VHDX-bestanden of **dynamische** en **differentiërende** VHD's worden niet ondersteund.

### <a name="errordirectorydisallowedfortype"></a>ERROR_DIRECTORY_DISALLOWED_FOR_TYPE

**Foutbeschrijving:** Een map is niet toegestaan in een van de vooraf bestaande mappen voor de beheerde schijven. Alleen vaste VHD's zijn toegestaan in deze mappen.

**Voorgestelde oplossing:** De volgende drie mappen worden gemaakt die overeenkomen met containers in uw storage-account voor beheerde schijven in elke share: Premium SSD, standaard harde schijven en Standard-SSD. Deze mappen komen overeen met de prestatielaag voor de beheerde schijf.

- Zorg ervoor dat u uw pagina-blob-gegevens (VHD's) naar een van deze bestaande mappen kopiëren.
- Een map of directory is niet toegestaan in deze bestaande mappen. Verwijder de mappen die u hebt gemaakt in de bestaande mappen.

Zie voor meer informatie, [naar beheerde schijven kopiëren](data-box-deploy-copy-data-from-vhds.md#connect-to-data-box).

### <a name="reparsepointerror"></a>REPARSE_POINT_ERROR

**Foutbeschrijving:** Symbolische koppelingen zijn niet toegestaan in Linux. 

**Voorgestelde oplossing:** Symbolische koppelingen zijn meestal koppelingen, pipes en andere dergelijke bestanden. Verwijder de koppelingen of omzetten van de koppelingen en kopieer de gegevens.


## <a name="non-critical-blob-or-file-errors"></a>Niet-kritieke blob of file-fouten

De fouten die worden weergegeven tijdens het kopiëren van gegevens worden samengevat in de volgende secties.

### <a name="errorbloborfilenamecharactercontrol"></a>ERROR_BLOB_OR_FILE_NAME_CHARACTER_CONTROL

**Foutbeschrijving:** De namen van de blob of file bevatten besturingselement voor niet-ondersteunde tekens.

**Voorgestelde oplossing:** De blobs of de bestanden die u hebt gekopieerd bevatten namen met niet-ondersteunde tekens.

Op de **verbinding maken en kopiëren** pagina van de lokale webgebruikersinterface downloaden, en controleert u de fout.
Verwijder of wijzig de naam van de bestanden te verwijderen van niet-ondersteunde tekens.

Zie voor meer informatie, de Azure naamgevingsregels voor [blob-namen](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) en [bestandsnamen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names).

### <a name="errorbloborfilenamecharacterillegal"></a>ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL

**Foutbeschrijving:** De blob of file namen bevatten ongeldige tekens.

**Voorgestelde oplossing:** De blobs of de bestanden die u hebt gekopieerd bevatten namen met niet-ondersteunde tekens.

Op de **verbinding maken en kopiëren** pagina van de lokale webgebruikersinterface downloaden, en controleert u de fout.
Verwijder of wijzig de naam van de bestanden te verwijderen van niet-ondersteunde tekens.

Zie voor meer informatie, de Azure naamgevingsregels voor [blob-namen](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) en [bestandsnamen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names).


### <a name="errorbloborfilenameending"></a>ERROR_BLOB_OR_FILE_NAME_ENDING

**Foutbeschrijving:** De namen van de blob of file eindigen met ongeldige tekens.

**Voorgestelde oplossing:** De blobs of de bestanden die u hebt gekopieerd bevatten namen met niet-ondersteunde tekens.

Op de **verbinding maken en kopiëren** pagina van de lokale webgebruikersinterface downloaden, en controleert u de fout.
Verwijder of wijzig de naam van de bestanden te verwijderen van niet-ondersteunde tekens.

Zie voor meer informatie, de Azure naamgevingsregels voor [blob-namen](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) en [bestandsnamen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names).


### <a name="errorbloborfilenamesegmentcount"></a>ERROR_BLOB_OR_FILE_NAME_SEGMENT_COUNT

**Foutbeschrijving:** De naam van de blob of file bevat te veel padsegmenten.

**Voorgestelde oplossing:** De blobs of de bestanden die u hebt gekopieerd groter zijn dan het maximum aantal padsegmenten. Een padsegment is de waarde in tussen opeenvolgende scheidingstekens, bijvoorbeeld de slash /.

- Op de **verbinding maken en kopiëren** pagina van de lokale webgebruikersinterface downloaden, en controleert u de fout.
- Zorg ervoor dat de [blob-namen](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) en [bestandsnamen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names) voldoen aan de naamgevingsconventies voor Azure.

### <a name="errorbloborfilenameaggregatelength"></a>ERROR_BLOB_OR_FILE_NAME_AGGREGATE_LENGTH

**Foutbeschrijving:** De naam van de blob of het bestand is te lang.

**Voorgestelde oplossing:** De blob of namen van de bestanden groter zijn dan de maximale lengte.

- Op de **verbinding maken en kopiëren** pagina van de lokale webgebruikersinterface downloaden, en controleert u de fout.
- De blob-naam mag niet groter zijn dan 1024 tekens bestaan.
- Verwijder of wijzig de naam van de blob of bestanden zodat de namen niet langer zijn dan 1024 tekens.

Zie voor meer informatie, de Azure naamgevingsregels voor blob-namen en bestandsnamen.

### <a name="errorbloborfilenamecomponentlength"></a>ERROR_BLOB_OR_FILE_NAME_COMPONENT_LENGTH

**Foutbeschrijving:** Een van de segmenten in de blob- of bestandsnaam is te lang.

**Voorgestelde oplossing:** Een van de segmenten in de naam van de blob of file overschrijdt het maximum aantal tekens. Een padsegment is de waarde in tussen opeenvolgende scheidingstekens, bijvoorbeeld de slash /.

- Op de **verbinding maken en kopiëren** pagina van de lokale webgebruikersinterface downloaden, en controleert u de fout.
- Zorg ervoor dat de [blob-namen](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) en [bestandsnamen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names) voldoen aan de naamgevingsconventies voor Azure.


### <a name="errorcontainerorsharenamedisallowedfortype"></a>ERROR_CONTAINER_OR_SHARE_NAME_DISALLOWED_FOR_TYPE

**Foutbeschrijving:** Onjuiste containernamen zijn opgegeven voor beheerde schijfshares.

**Voorgestelde oplossing:** De volgende mappen worden gemaakt die overeenkomen met containers in uw storage-account voor beheerde schijven in elke share: Premium SSD, standaard harde schijven en Standard-SSD. Deze mappen komen overeen met de prestatielaag voor de beheerde schijf.

- Zorg ervoor dat u uw pagina-blob-gegevens (VHD's) naar een van deze bestaande mappen kopiëren. Alleen gegevens uit deze bestaande containers is geüpload naar Azure.
- Een andere map die is gemaakt op hetzelfde niveau als Premium-SSD-, Standard HDD- en Standard-SSD komt niet overeen met een geldige prestatielaag en kan niet worden gebruikt.
- Verwijder bestanden of mappen die buiten de prestatielagen zijn gemaakt.

Zie voor meer informatie, [naar beheerde schijven kopiëren](data-box-deploy-copy-data-from-vhds.md#connect-to-data-box).


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [gegevens in Blob storage-systeemvereisten](data-box-system-requirements-rest.md).
