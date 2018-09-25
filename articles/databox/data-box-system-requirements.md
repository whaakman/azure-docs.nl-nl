---
title: Systeemvereisten voor Microsoft Azure Data Box | Microsoft Docs
description: Meer informatie over de software en netwerkvereisten voor uw Azure Data Box
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
ms.workload: NA
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: f97c6174adf454a031e94942843075c457236575
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982970"
---
# <a name="azure-data-box-system-requirements"></a>Azure Data Box-systeemvereisten

Dit artikel beschrijft de belangrijke systeemvereisten voor uw Microsoft Azure Data Box en de clients die verbinding maken met de Data Box. Het is raadzaam dat u de informatie voordat u uw Data Box te implementeren, en vervolgens terug naar deze zo nodig tijdens de implementatie en het volgende gebruik verwijzen zorgvuldig te controleren.

De systeemvereisten zijn onder andere:

* **Softwarevereisten voor hosts die verbinding maken met Data Box** -beschrijving van de ondersteunde platforms, browsers voor de lokale web-UI, SMB-clients en eventuele bijkomende vereisten voor hosts die verbinding met de Data Box maken kunnen.
* **Netwerkvereisten voor de Data Box** -vindt u informatie over de vereisten voor de optimale werking van de Data Box.


## <a name="software-requirements"></a>Softwarevereisten

De vereisten voor bevatten de gegevens op de ondersteunde besturingssystemen, de ondersteunde browsers voor de lokale web-UI en de SMB-clients.

### <a name="supported-operating-systems-for-clients"></a>Ondersteunde besturingssystemen voor clients

Hier volgt een lijst van de ondersteunde besturingssystemen voor de bewerking voor het kopiëren van gegevens via de clients die zijn verbonden met de Data Box-apparaat.

| **Besturingssysteem** | **Versies** | 
| --- | --- | 
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 | 
| Windows |7, 8, 10 | 
|Linux    |         |

### <a name="supported-file-systems-for-linux-clients"></a>Ondersteunde bestandssystemen voor Linux-clients

| **Protocollen** | **Versies** | 
| --- | --- | 
| SMB |2.X en hoger |
| NFS | Alle versies tot en met 4.1|

### <a name="supported-storage-accounts"></a>Ondersteunde opslagaccounts

Hier volgt een lijst van de typen ondersteunde opslag voor de Data Box-apparaat.

| **Opslagaccount** | **Opmerkingen** |
| --- | --- |
| Klassiek | Standard |
| Algemeen doel  |Standaard; zowel V1 als V2 worden ondersteund. |
| Blob |Zowel warme als koude worden ondersteund. |


### <a name="supported-storage-types"></a>Ondersteunde opslagtypen

Hier volgt een lijst van de typen ondersteunde opslag voor de Data Box-apparaat.

| **Bestandsindeling** | **Opmerkingen** |
| --- | --- |
| Azure blok-blob | |
| Azure-pagina-blobs  | De gegevens moet 512 bytes uitgelijnd.|
| Azure Files | |


### <a name="supported-web-browsers"></a>Ondersteunde webbrowsers

Hier volgt een lijst van webbrowsers die worden ondersteund voor de lokale webgebruikersinterface.

| **Browser** | **Versies** | **Aanvullende vereisten/opmerkingen** |
| --- | --- | --- |
| Google Chrome |Meest recente versie |Getest met Chrome|
| Microsoft Edge |Meest recente versie | |
| FireFox | Meest recente versie | Getest met FireFox|
| Internet Explorer |Meest recente versie |Als u niet aanmelden, controleert u of cookies en Javascript zijn ingeschakeld. Zodat de toegang tot de gebruikersinterface toevoegen het IP-adres van het apparaat naar **privacyacties** zodat het apparaat toegang heeft tot cookies. |


## <a name="networking-requirements"></a>Netwerkvereisten

Uw datacenter moet dankzij het netwerk. Het wordt aangeraden dat u ten minste één hebt 10 GbE-verbinding. Als een 10 GbE-verbinding niet beschikbaar is, een 1 GbE-gegevens-koppeling kan worden gebruikt om gegevens te kopiëren, maar de kopie-snelheden worden beïnvloed.

## <a name="next-step"></a>Volgende stap

* [Implementeren van uw Azure Data Box](data-box-deploy-ordered.md)

