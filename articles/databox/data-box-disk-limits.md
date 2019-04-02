---
title: Azure Data Box-schijf beperkt | Microsoft Docs
description: Beschrijving van systeemlimieten en aanbevolen grootten voor de Microsoft Azure Data Box-schijf.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 32445e3f6859a6161eb2fae20233c598234f18a0
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58791640"
---
# <a name="azure-data-box-disk-limits"></a>Limieten voor Azure Data Box-schijf


Houd rekening met deze limieten bij het implementeren en uitvoeren van uw oplossing Microsoft Azure Data Box-schijf.

## <a name="data-box-service-limits"></a>Data Box-service-limieten

 - Data Box-service is beschikbaar in de Azure-regio's die worden vermeld in [beschikbaarheid in regio](data-box-disk-overview.md#region-availability).
 - Een enkel opslagaccount wordt ondersteund met de Data Box-schijf.

## <a name="data-box-disk-performance"></a>Prestaties van de Data Box-schijf

Bij een test met schijven die verbonden waren via USB 3.0 was de prestatie van de schijven maximaal 430 MB/s. De werkelijke waarden variëren, afhankelijk van de gebruikte bestandsgrootte. Voor kleinere bestanden zijn de prestaties lager.

## <a name="azure-storage-limits"></a>Limieten voor Azure-opslag

In deze sectie beschrijft de limieten voor Azure Storage-service en de vereiste naamgevingsregels voor Azure Files, Azure blok-blobs en Azure-pagina-blobs, zoals van toepassing op de Data Box-service. De opslaglimieten zorgvuldig te controleren en volgt u alle aanbevelingen.

Voor de meest recente informatie over limieten voor Azure storage-service en aanbevolen procedures voor de naamgeving van shares, containers en -bestanden, gaat u naar:

- [Naamgeving van en verwijzen naar containers](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Naamgeving van en verwijzing naar shares](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Blok-blobs en pagina-blob-conventies](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Als er bestanden of mappen die groter zijn dan de grenzen van de service Azure Storage of niet voldoen aan de naamgevingsconventies Azure bestanden/Blob, worden klikt u vervolgens deze bestanden of mappen niet opgenomen in de Azure Storage via de Data Box-service.

## <a name="data-upload-caveats"></a>Onder voorbehoud het uploaden van gegevens

- Kopieert geen gegevens rechtstreeks in de schijven. Gegevens kopiëren naar vooraf gemaakte *BlockBlob*,*PageBlob*, en *AzureFile* mappen.
- Een map onder de *BlockBlob* en *PageBlob* is een container. Bijvoorbeeld, containers worden gemaakt als *BlockBlob/container* en *PageBlob/container*.
- Hebt u een bestaande Azure-object (zoals een blob) in de cloud met dezelfde naam als het object dat wordt gekopieerd, wordt Data Box-schijf naam van het bestand als file(1) in de cloud.
- Alle bestanden die worden weggeschreven naar *BlockBlob* en *PageBlob* shares respectievelijk als een blok-blobs en pagina-blob is geüpload.
- Een directory-hiërarchie (zonder bestanden) gemaakt op basis van lege *BlockBlob* en *PageBlob* mappen is niet geüpload.
- Als er fouten optreden tijdens het uploaden van gegevens naar Azure, wordt een foutenlogboek gemaakt in het doelopslagaccount. Het pad naar dit foutenlogboek is beschikbaar in de portal wanneer het uploaden voltooid is en u corrigerende maatregelen nemen in het logboek kunt bekijken. Verwijder geen gegevens van de bron zonder de geüploade gegevens te verifiëren.
- Als u beheerde schijven in de volgorde opgegeven, controleert u de volgende aanvullende factoren:

    - U kunt slechts één beheerde schijf met een specifieke naam hebben in een resourcegroep, tussen de precreated mappen en alle de Data Box-schijf. Dit betekent dat de VHD's geüpload naar de precreated mappen moeten een unieke naam hebben. Zorg ervoor dat de opgegeven naam komt niet overeen met een al bestaande beheerde schijf in een resourcegroep. Als u VHD's dezelfde namen hebben, wordt slechts één VHD geconverteerd naar beheerde schijven met die naam. De andere VHD's worden naar het tijdelijke opslagaccount geüpload als pagina-blobs.
    - Kopieer de VHD's altijd op een van de precreated mappen. Als u de VHD's buiten deze mappen of in een map die u hebt gemaakt kopieert, wordt de VHD's worden geüpload naar Azure Storage-account als pagina-blobs en schijven niet worden beheerd.
    - De vaste VHD's kunnen worden geüpload voor het maken van beheerde schijven. Dynamische VHD's, differentiërende VHD's of VHDX-bestanden worden niet ondersteund.

## <a name="azure-storage-account-size-limits"></a>Maximale grootte van Azure storage-account

Hier zijn de limieten voor de grootte van de gegevens die worden gekopieerd naar storage-account. Zorg ervoor dat de gegevens die u uploadt, aan deze limieten voldoet. Voor de meest actuele informatie over deze limieten, gaat u naar [prestatiedoelen voor Azure blob storage schaal](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) en [Azure Files schalen doelen](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Grootte van gegevens die zijn gekopieerd naar Azure storage-account                      | Standaardlimiet          |
|---------------------------------------------------------------------|------------------------|
| Blok-blobs en pagina-blob                                            | 500 TB per opslagaccount. <br> Dit omvat gegevens uit alle bronnen met inbegrip van Data Box-schijf.|


## <a name="azure-object-size-limits"></a>De maximale grootte Azure-object

Hier vindt u de grootte van de Azure-objecten die kunnen worden geschreven. Zorg ervoor dat alle bestanden die worden geüpload, aan deze limieten voldoen.

| Azure-objecttype | Standaardlimiet                                             |
|-------------------|-----------------------------------------------------------|
| Blok-blob        | ~ 4.75 TiB                                                 |
| Pagina-blob         | 8 TiB <br> (Elk bestand dat is geüpload in de indeling van de pagina-Blob moet 512 bytes uitgelijnd, anders het uploaden is mislukt. <br> De VHD- en VHDX zijn 512 bytes uitgelijnd.) |
|Azure Files        | 1 TiB <br> Met maximaal de grootte van de share is 5 TiB     |
| Managed Disks     |4 TiB <br> Zie voor meer informatie over de grootte en beperkingen: <li>[Schaalbaarheidsdoelen voor beheerde schijven](../virtual-machines/windows/disk-scalability-targets.md#managed-virtual-machine-disks)</li>|


## <a name="azure-block-blob-page-blob-and-file-naming-conventions"></a>Azure blok-blobs, pagina-blob en bestand naamgevingsregels

| Entiteit                                       | Conventies                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Namen van containers voor blok-blobs en pagina-blobs <br> FileShare-namen voor Azure Files | Moet een geldige DNS-naam die tussen de 3 en 63 tekens lang zijn. <br>  Moet beginnen met een letter of cijfer. <br> Mag alleen kleine letters, cijfers en afbreekstreepjes (-). <br> Elk koppelteken (-) moet direct worden voorafgegaan en gevolgd door een letter of cijfer. <br> In namen worden geen opeenvolgende koppeltekens toegestaan. |
| Directory- en bestandsnamen voor Azure files     |<li> Aanvraag te behouden, niet-hoofdlettergevoelige en mag niet groter zijn dan 255 tekens. </li><li> Mag niet eindigen met de schuine streep (/). </li><li>Indien opgegeven, worden deze automatisch verwijderd. </li><li> Volgende tekens zijn niet toegestaan: <code>" \\ / : \| < > * ?</code></li><li> Gereserveerde tekens voor URL's moeten op de juiste wijze van een escape-teken zijn voorzien. </li><li> Ongeldige tekens voor URL-pad zijn niet toegestaan. Code-punten zoals \\uE000 zijn geen geldige Unicode-tekens. Sommige ASCII of Unicode-tekens, zoals stuurcodes (0x00-0x1F \\u0081, enzovoort), zijn ook niet toegestaan. Tekenreeksen in HTTP/1.1 Zie RFC 2616, sectie 2.2 voor regels voor Unicode: Eenvoudige regels en RFC 3987. </li><li> Volgende bestandsnamen zijn niet toegestaan: LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, PRN, AUX, NUL, CON, CLOCK$, punt-teken (.), en twee punten tekens (.).</li>|
| Blobnamen voor blok-blob en pagina-blob      | Blobnamen zijn hoofdlettergevoelig en kunnen elke combinatie van tekens bevatten. <br> Een blobnaam moet 1 tot 1024 tekens bevatten. <br> Gereserveerde tekens voor URL's moeten op de juiste wijze van een escape-teken zijn voorzien. <br>Het aantal padsegmenten dat de blobnaam omvat, mag niet meer dan 254 zijn. Een padsegment is de tekenreeks tussen opeenvolgende scheidingstekens (bijvoorbeeld de slash '/') die overeenkomt met de naam van een virtuele map. |

## <a name="managed-disk-naming-conventions"></a>Beheerde schijf naamgevingsregels

| Entiteit | Conventies                                             |
|-------------------|-----------------------------------------------------------|
| Beheerde schijf-namen       | <li> De naam moet 1 tot en met 80 tekens lang zijn. </li><li> De naam moet beginnen met een letter of cijfer, eindigen met een letter, cijfer of onderstrepingsteken. </li><li> De naam mag alleen letters, cijfers, onderstrepingstekens, punten of afbreekstreepjes bevatten. </li><li>   De naam mag geen spaties of `/`.                                              |

## <a name="next-steps"></a>Volgende stappen

- Beoordeling [systeemvereisten Data Box-schijf](data-box-disk-system-requirements.md)
