---
title: Azure Data Box-schijf beperkt | Microsoft Docs
description: Beschrijving van systeemlimieten en aanbevolen grootten voor de Microsoft Azure Data Box-schijf.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/12/2018
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: 4db70fa93914ba0544d9beb8e523241513a2e5ce
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009299"
---
# <a name="azure-data-box-disk-limits-preview"></a>Limieten voor Azure Data Box-schijf (Preview)


Houd rekening met deze limieten bij het implementeren en uitvoeren van uw oplossing Microsoft Azure Data Box-schijf. 

> [!IMPORTANT] 
> Azure Data Box-schijf is in Preview. Controleer de [gebruiksvoorwaarden voor de Preview-versie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert. 


## <a name="data-box-service-limits"></a>Data Box-service-limieten

 - Data Box-service is alleen beschikbaar in VS- en EU in alle Azure-regio's voor openbare Azure-cloud.
 - Een enkel opslagaccount wordt ondersteund met de Data Box-schijf.

## <a name="data-box-disk-performance"></a>Prestaties van de Data Box-schijf

Wanneer getest met schijven die zijn verbonden via USB 3.0, is de schijfprestaties tot 430 MB/s. De werkelijke getallen variëren al naar gelang de bestandsgrootte die wordt gebruikt. Voor kleinere bestanden ziet u mogelijk lagere prestaties.

## <a name="azure-storage-limits"></a>Limieten voor Azure-opslag

In deze sectie beschrijft de limieten voor Azure Storage-service en de vereiste naamgevingsregels voor Azure Files, Azure blok-blobs en Azure-pagina-blobs, zoals van toepassing op de Data Box-service. De opslaglimieten zorgvuldig te controleren en volgt u alle aanbevelingen.

Voor de meest recente informatie over limieten voor Azure storage-service en aanbevolen procedures voor de naamgeving van shares, containers en -bestanden, gaat u naar:

- [Naamgeving van en verwijzen naar containers](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Naamgeving van en verwijzen naar bestandsshares](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Blok-blobs en pagina-blob-conventies](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Als er bestanden of mappen die groter zijn dan de grenzen van de service Azure Storage of niet voldoen aan de naamgevingsconventies Azure bestanden/Blob, worden klikt u vervolgens deze bestanden of mappen niet opgenomen in de Azure Storage via de Data Box-service.

## <a name="data-upload-caveats"></a>Onder voorbehoud het uploaden van gegevens

- Kopieert geen gegevens rechtstreeks in de schijven. Gegevens kopiëren naar vooraf gemaakte *BlockBlob* en *PageBlob* mappen.
- Een map onder de *BlockBlob* en *PageBlob* is een container. Bijvoorbeeld, containers worden gemaakt als *BlockBlob/container* en *PageBlob/container*.
- Hebt u een bestaande Azure-object (zoals een blob) in de cloud met dezelfde naam als het object dat wordt gekopieerd, wordt het bestand in de cloud in Data Box-schijf overschreven.
- Alle bestanden die worden weggeschreven naar *BlockBlob* en *PageBlob* shares respectievelijk als een blok-blobs en pagina-blob is geüpload.
- Een directory-hiërarchie (zonder bestanden) gemaakt op basis van lege *BlockBlob* en *PageBlob* mappen is niet geüpload.
- Als er fouten optreden tijdens het uploaden van gegevens naar Azure, wordt een foutenlogboek gemaakt in het doelopslagaccount. Het pad naar dit foutenlogboek is beschikbaar in de portal wanneer het uploaden voltooid is en u corrigerende maatregelen nemen in het logboek kunt bekijken. Verwijder geen gegevens van de bron zonder de geüploade gegevens te verifiëren.

## <a name="azure-storage-account-size-limits"></a>Maximale grootte van Azure storage-account

Hier zijn de limieten voor de grootte van de gegevens die worden gekopieerd naar storage-account. Zorg ervoor dat de gegevens die u uploadt, aan deze limieten voldoet. Voor de meest actuele informatie over deze limieten, gaat u naar [prestatiedoelen voor Azure blob storage schaal](https://docs.microsoft.com/en-us/azure/storage/cstorage-scalability-targets#azure-blob-storage-scale-targets) en [Azure Files schalen doelen](https://docs.microsoft.com/en-us/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Grootte van gegevens die zijn gekopieerd naar Azure storage-account                      | Standaardlimiet          |
|---------------------------------------------------------------------|------------------------|
| Blok-blobs en pagina-blob                                            | 500 TB per opslagaccount. <br> Dit omvat gegevens uit alle bronnen met inbegrip van Data Box-schijf.|


## <a name="azure-object-size-limits"></a>De maximale grootte Azure-object

Hier vindt u de grootte van de Azure-objecten die kunnen worden geschreven. Zorg ervoor dat alle bestanden die worden geüpload, aan deze limieten voldoen.

| Azure-objecttype | Standaardlimiet                                             |
|-------------------|-----------------------------------------------------------|
| Blok-Blob        | ~ 8 TB                                                 |
| Pagina-blobs         | 1 TB <br> (Elk bestand dat is geüpload in de indeling van de pagina-Blob moet zijn uitgelijnd 512 bytes (een integraal meerdere), anders het uploaden is mislukt. <br> De VHD en VHDX zijn 512 bytes uitgelijnd.) |


## <a name="azure-block-blob-and-page-blob-naming-conventions"></a>Azure blok-blobs en pagina-blob naamconventies

| Entiteit                                       | Conventies                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Namen van containers voor blok-blobs en pagina-blobs | Moet een geldige DNS-naam die tussen de 3 en 63 tekens lang zijn. <br>  Moet beginnen met een letter of cijfer. <br> Mag alleen kleine letters, cijfers en afbreekstreepjes (-). <br> Elk streepje (-) moet direct worden voorafgegaan en gevolgd door een letter of cijfer. <br> Opeenvolgende streepjes zijn niet toegestaan in namen. |
| BLOB-namen voor blok-blobs en pagina-blobs      | BLOB-namen zijn hoofdlettergevoelig en kunnen elke combinatie van tekens bevatten. <br> Een blobnaam moet tussen 1 tot 1024 tekens lang zijn. <br> Gereserveerde URL-tekens moeten correct worden weergegeven. <br>Het aantal padsegmenten die bestaat uit de blob-naam niet langer zijn dan 254. Een padsegment is de waarde in tussen opeenvolgende scheidingstekens (bijvoorbeeld de slash '/') die overeenkomen met de naam van een virtuele map. |
