---
title: Systeemvereisten voor Microsoft Azure Data Box | Microsoft Docs
description: Meer informatie over de software- en netwerkvereisten voor de Azure Data Box
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 01/23/2019
ms.author: alkohli
ms.openlocfilehash: 7d52af9e3948f40936795efab5b6671c3f71007a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55206744"
---
# <a name="azure-data-box-system-requirements"></a>Azure Data Box-systeemvereisten

Dit artikel beschrijft de belangrijke systeemvereisten voor uw Microsoft Azure Data Box en de clients die verbinding maken met de Data Box. Het is raadzaam om dat u de informatie zorgvuldig controleren voordat u uw Data Box te implementeren, en vervolgens terug naar deze zo nodig tijdens de implementatie en het volgende gebruik verwijzen.

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

| **Protocols** | **Versies** | 
| --- | --- | 
| SMB |2.X en hoger |
| NFS | Alle versies tot en met 4.1|

### <a name="supported-storage-accounts"></a>Ondersteunde opslagaccounts

Hier volgt een lijst van de ondersteunde opslagaccounts en de opslagtypen voor de Data Box-apparaat. Zie voor een volledige lijst van alle verschillende soorten opslagaccounts en de bijbehorende volledige mogelijkheden [typen opslagaccounts](/azure/storage/common/storage-account-overview#types-of-storage-accounts).

| **Storage-account / opslagtypen ondersteund** | **Blok-blob** |**Pagina-blobs*** |**Azure Files** |**Opmerkingen**|
| --- | --- | -- | -- | -- |
| Klassieke Standard | J | J | J |
| Standaard voor algemeen gebruik v1  | J | J | J | Zowel warme als koude worden ondersteund.|
| Premium voor algemeen gebruik v1  |  | J| | |
| Algemeen gebruik v2 Standard  | J | J | J | Zowel warme als koude worden ondersteund.|
| Algemeen gebruik v2 Premium  |  |J | | |
| BLOB-opslag Standard |J | | |Zowel warme als koude worden ondersteund. |

\* *-Gegevens geüpload naar de pagina-blobs moet 512 bytes uitgelijnd, zoals VHD's.*

>[!NOTE]
> Azure Data Lake Storage Gen 2-accounts worden niet ondersteund.


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

Uw datacenter moet een netwerk met hoge snelheid hebben. Het wordt aangeraden dat u beschikt over minstens één 10-GbE-verbinding. Als een 10 GbE-verbinding niet beschikbaar is, kan de gegevenskoppeling van een 1 GbE-worden gebruikt om te kopiëren van gegevens, maar de kopie snelheden worden beïnvloed.

## <a name="next-step"></a>Volgende stap

* [Implementeren van uw Azure Data Box](data-box-deploy-ordered.md)

