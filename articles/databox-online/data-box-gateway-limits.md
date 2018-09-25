---
title: Azure Data Box-Gateway beperkt | Microsoft Docs
description: Beschrijving van systeemlimieten en aanbevolen grootten voor de Microsoft Azure Data Box-Gateway.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/24/2018
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: edb4995b626055be830a7accb74d99f1db3ef8d0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46962179"
---
# <a name="azure-data-box-gateway-limits-preview"></a>Azure Data Box-Gateway-limieten (Preview)


Houd rekening met deze limieten bij het implementeren en uw Microsoft Azure Data Box-Gateway-oplossing werken. 

> [!IMPORTANT] 
> Data Box-Gateway is in Preview. Controleer de [gebruiksvoorwaarden voor de Preview-versie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert. 


## <a name="data-box-gateway-service-limits"></a>Gegevens in het Gateway-service-limieten

- In deze release is service alleen beschikbaar in bepaalde regio's in de VS, EU en Azië en Stille Oceaan. Ga voor meer informatie naar [beschikbaarheid in regio](#data-box-gateway-overview#region-availability). Het opslagaccount moet zich fysiek die het dichtst bij de regio waar het apparaat is geïmplementeerd (kan afwijken van de service geografische regio).
- Een Data Box-Gateway-resource verplaatsen naar een ander abonnement of de resource-groep wordt niet ondersteund. Voor meer informatie gaat u naar [resources verplaatsen naar een nieuwe resourcegroep of abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).

## <a name="data-box-gateway-device-limits"></a>Gegevens in het Gateway-apparaat-limieten

De volgende tabel beschrijft de limieten voor de gegevens in het Gateway-apparaat.

| Beschrijving | Waarde |
|---|---|
|Nee. van bestanden per apparaat |100 miljoen <br> Limiet is ongeveer 25 miljoen bestanden voor elke 2 TB aan schijfruimte met maximale limiet op 100 miljoen |
|Nee. van shares per apparaat |24 |
|Maximale bestandsgrootte die zijn geschreven naar een share|5 TB |

## <a name="azure-storage-limits"></a>Limieten voor Azure-opslag

In deze sectie beschrijft de limieten voor Azure Storage-service en de vereiste naamgevingsregels voor Azure Files, Azure blok-blobs en Azure-pagina-blobs, zoals van toepassing op de gegevens in het/de Data Gateway in het Edge-service. De opslaglimieten zorgvuldig te controleren en volgt u alle aanbevelingen.

Voor de meest recente informatie over limieten voor Azure storage-service en aanbevolen procedures voor de naamgeving van shares, containers en -bestanden, gaat u naar:

- [Naamgeving van en verwijzen naar containers](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Naamgeving van en verwijzen naar bestandsshares](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Blok-blobs en pagina-blob-conventies](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Als er bestanden of mappen die groter zijn dan de grenzen van de service Azure Storage of niet voldoen aan de naamgevingsconventies Azure bestanden/Blob, worden klikt u vervolgens deze bestanden of mappen niet opgenomen in de Azure Storage via de gegevens in het/de Data Gateway in het Edge-service.

## <a name="data-upload-caveats"></a>Onder voorbehoud het uploaden van gegevens

Volgende voorbehoud toepassen op gegevens zoals deze is verplaatst naar Azure.

- Het is raadzaam dat meer dan één apparaat niet tot dezelfde container moet schrijven.
- Hebt u een bestaande Azure-object (zoals een blob of een bestand) in de cloud met dezelfde naam als het object dat wordt gekopieerd, wordt het bestand in de cloud door apparaat overschreven. 
- Een lege map voor de hiërarchie (zonder bestanden) gemaakt op basis van gedeelde mappen is niet geüpload naar de blob-containers.


## <a name="azure-storage-account-size-and-object-size-limits"></a>Azure-account grootte en object maximale grootte opslag

Hier zijn de limieten voor de grootte van de gegevens die worden gekopieerd naar storage-account. Zorg ervoor dat de gegevens die u uploadt, aan deze limieten voldoet. Voor de meest actuele informatie over deze limieten, gaat u naar [prestatiedoelen voor Azure blob storage schaal](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) en [Azure Files schalen doelen](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Grootte van gegevens die zijn gekopieerd naar Azure storage-account                      | Standaardlimiet          |
|---------------------------------------------------------------------|------------------------|
| Blok-blobs en pagina-blob                                            | 500 TB per opslagaccount|


## <a name="azure-object-size-limits"></a>De maximale grootte Azure-object

Hier vindt u de grootte van de Azure-objecten die kunnen worden geschreven. Zorg ervoor dat alle bestanden die worden geüpload, aan deze limieten voldoen.

| Azure-objecttype | Standaardlimiet                                             |
|-------------------|-----------------------------------------------------------|
| Blok-blob        | ~ 8 TB                                                 |
| Pagina-blob         | 1 TB <br> Elk bestand dat is geüpload in de indeling van de pagina-Blob moet zijn uitgelijnd 512 bytes (een integraal meerdere), anders het uploaden is mislukt. <br> De VHD en VHDX zijn dan 512 bytes uitgelijnd. |


## <a name="next-steps"></a>Volgende stappen

- [Voorbereidingen voor het implementeren van Azure Data Box-Gateway](data-box-gateway-deploy-prep.md)
