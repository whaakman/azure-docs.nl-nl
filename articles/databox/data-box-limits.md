---
title: Azure Data Box beperkt | Microsoft Docs
description: Beschrijving van systeemlimieten en aanbevolen grootten voor de Microsoft Azure Data Box-onderdelen en verbindingen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 10/10/2018
ms.author: alkohli
ms.openlocfilehash: de47cae219aa457343df292bb91b6af06c4b1186
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091926"
---
# <a name="azure-data-box-limits"></a>Azure Data Box-limieten

Houd rekening met deze limieten bij het implementeren en uitvoeren van uw Microsoft Azure Data Box. De volgende tabel beschrijft deze limieten voor de Data Box.


## <a name="data-box-service-limits"></a>Data Box-service-limieten

 - Data Box-service is alleen beschikbaar in VS helemaal de [Azure-regio's voor openbare Azure-cloud](https://azure.microsoft.com/regions/).
 - Als u meerdere opslagaccounts met Data Box-service gebruikt, moeten alle opslagaccounts behoren tot dezelfde Azure-regio alleen.
 - U wordt aangeraden dat u niet meer dan drie opslagaccounts. Met behulp van meer storage-accounts kan mogelijk gevolgen hebben voor de prestaties.

## <a name="data-box-limits"></a>Data Box-limieten

- Data Box kan maximaal 500 miljoen bestanden opslaan.

## <a name="azure-storage-limits"></a>Limieten voor Azure-opslag

In deze sectie beschrijft de limieten voor Azure Storage-service en de vereiste naamgevingsregels voor Azure Files, Azure blok-blobs en Azure-pagina-blobs, zoals van toepassing op de Data Box-service. De opslaglimieten zorgvuldig te controleren en volgt u alle aanbevelingen.

Voor de meest recente informatie over limieten voor Azure storage-service en aanbevolen procedures voor de naamgeving van shares, containers en -bestanden, gaat u naar:

- [Naamgeving van en verwijzen naar containers](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Naamgeving van en verwijzing naar shares](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Blok-blobs en pagina-blob-conventies](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Als er bestanden of mappen die groter zijn dan de grenzen van de service Azure Storage of niet voldoen aan de naamgevingsconventies Azure bestanden/Blob, worden klikt u vervolgens deze bestanden of mappen niet opgenomen in de Azure Storage via de Data Box-service.

## <a name="data-upload-caveats"></a>Onder voorbehoud het uploaden van gegevens

- Gegevens rechtstreeks onder een van de precreated shares niet kopiëren. U moet een map in de bestandsshare maken en kopieert u gegevens naar die map.
- Een map onder de *StorageAccount_BlockBlob* en *StorageAccount_PageBlob* is een container. Bijvoorbeeld, containers worden gemaakt als *StorageAccount_BlockBlob/container* en *StorageAccount_PageBlob/container*.
- Elke gemaakt direct onder de map *StorageAccount_AzureFiles* wordt omgezet in een Azure-bestandsshare.
- Hebt u een bestaande Azure-object (zoals een blob of een bestand) in de cloud met dezelfde naam als het object dat wordt gekopieerd, overschrijft de gegevens in het bestand in de cloud.
- Alle bestanden die worden weggeschreven naar *StorageAccount_BlockBlob* en *StorageAccount_PageBlob* shares respectievelijk als een blok-blobs en pagina-blob is geüpload.
- Azure blob-opslag biedt geen ondersteuning voor mappen. Als u een map onder de *StorageAccount_BlockBlob* map, en vervolgens virtuele mappen worden gemaakt in de blob-naam. Voor Azure Files, wordt de werkelijke mapstructuur behouden.
- Een directory-hiërarchie (zonder bestanden) gemaakt op basis van lege *StorageAccount_BlockBlob* en *StorageAccount_PageBlob* mappen is niet geüpload.
- Als er fouten optreden tijdens het uploaden van gegevens naar Azure, wordt een foutenlogboek gemaakt in het doelopslagaccount. Het pad naar dit foutenlogboek is beschikbaar wanneer het uploaden voltooid is en kunt u corrigerende maatregelen nemen in het logboek bekijken. Verwijder geen gegevens van de bron zonder de geüploade gegevens te verifiëren.

## <a name="azure-storage-account-size-limits"></a>Maximale grootte van Azure storage-account

Hier zijn de limieten voor de grootte van de gegevens die worden gekopieerd naar storage-account. Zorg ervoor dat de gegevens die u uploadt, aan deze limieten voldoet. Voor de meest actuele informatie over deze limieten, gaat u naar [prestatiedoelen voor Azure blob storage schaal](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) en [Azure Files schalen doelen](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Grootte van gegevens die zijn gekopieerd naar Azure storage-account                      | Standaardlimiet          |
|---------------------------------------------------------------------|------------------------|
| Blok-blobs en pagina-blobs                                            | 500 TiB per opslagaccount. <br> Dit omvat gegevens uit alle bronnen met inbegrip van Data Box.|
| Azure-bestand                                                          | 5 TiB per share.<br> Alle mappen onder *StorageAccount_AzureFiles* deze limiet moet volgen.       |

## <a name="azure-object-size-limits"></a>De maximale grootte Azure-object

Hier vindt u de grootte van de Azure-objecten die kunnen worden geschreven. Zorg ervoor dat alle bestanden die worden geüpload, aan deze limieten voldoen.

| Azure-objecttype | Standaardlimiet                                             |
|-------------------|-----------------------------------------------------------|
| Blok-blob        | ~ 4.75 TiB                                                 |
| Pagina-blobs         | 8 TiB <br> Elk bestand dat is geüpload in de indeling van pagina-blob moet zijn uitgelijnd 512 bytes (een integraal meerdere), anders het uploaden is mislukt. <br> VHD- en VHDX zijn dan 512 bytes uitgelijnd. |
| Azure-bestand        | 1 TiB                                                      |

## <a name="azure-block-blob-page-blob-and-file-naming-conventions"></a>Azure blok-blobs, pagina-blob en bestand naamgevingsregels

| Entiteit                                       | Conventies                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Namen van containers voor blok-blobs en pagina-blobs | Moet een geldige DNS-naam die tussen de 3 en 63 tekens lang zijn. <br>  Moet beginnen met een letter of cijfer. <br> Mag alleen kleine letters, cijfers en afbreekstreepjes (-). <br> Elk koppelteken (-) moet direct worden voorafgegaan en gevolgd door een letter of cijfer. <br> In namen worden geen opeenvolgende koppeltekens toegestaan. |
| De namen van shares voor Azure files                  | Hetzelfde als hierboven                                                                                                                                                                                                                                                                                                             |
| Directory- en bestandsnamen voor Azure files     |<li> Aanvraag te behouden, niet-hoofdlettergevoelige en mag niet groter zijn dan 255 tekens. </li><li> Mag niet eindigen met de schuine streep (/). </li><li>Indien opgegeven, worden deze automatisch verwijderd. </li><li> Volgende tekens zijn niet toegestaan: ' "\ /: | in combinatie *? "</li><li> Gereserveerde tekens voor URL's moeten op de juiste wijze van een escape-teken zijn voorzien. </li><li> Ongeldige tekens voor URL-pad zijn niet toegestaan. Codepunten zoals \uE000 zijn geen geldige Unicode-tekens. Sommige ASCII of Unicode-tekens, zoals stuurcodes (0x00-0x1F \u0081, enzovoort), zijn ook niet toegestaan. Voor regels voor Unicode-tekenreeksen in HTTP/1.1 Zie RFC 2616, sectie 2.2: eenvoudige regels en RFC 3987. </li><li> De volgende bestand namen zijn niet toegestaan: LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, PRN, AUX, NUL, CON, CLOCK$, punt-teken (.), en twee punten tekens (.).</li>|
| Blobnamen voor blok-blob en pagina-blob      | </li><li>Blobnamen zijn hoofdlettergevoelig en kunnen elke combinatie van tekens bevatten. </li><li>Een blobnaam moet 1 tot 1024 tekens bevatten. </li><li>Gereserveerde tekens voor URL's moeten op de juiste wijze van een escape-teken zijn voorzien. </li><li>Het aantal padsegmenten dat de blobnaam omvat, mag niet meer dan 254 zijn. Een padsegment is de tekenreeks tussen opeenvolgende scheidingstekens (bijvoorbeeld de slash '/') die overeenkomt met de naam van een virtuele map.</li> |
