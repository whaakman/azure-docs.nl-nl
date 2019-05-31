---
title: Systeemvereisten voor Microsoft Azure Data Box zware | Microsoft Docs
description: Meer informatie over de software en netwerkvereisten voor uw Azure Data Box zware
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: article
ms.date: 05/22/2019
ms.author: alkohli
ms.openlocfilehash: 8cd2f96954cde367eb99d89e89bcf672b53dd590
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247992"
---
# <a name="azure-data-box-heavy-system-requirements"></a>Azure Data Box zware systeemvereisten

Dit artikel beschrijft de belangrijke systeemvereisten voor uw Azure Data Box zware apparaat en de clients die verbinding maken met het apparaat. Het is raadzaam om dat u de informatie zorgvuldig controleren voordat u uw Data Box-zwaar implementeert, en vervolgens terug naar deze zo nodig tijdens de implementatie en het volgende gebruik verwijzen.

De systeemvereisten zijn onder andere:

* **Softwarevereisten voor hosts die verbinding maken met gegevens in het zware** -beschrijving van de ondersteunde platforms, browsers voor de lokale web-UI, SMB-clients en eventuele bijkomende vereisten voor hosts die verbinding met de Data Box maken kunnen.
* **Netwerkvereisten voor de Data Box-zwaar** -vindt u informatie over de vereisten voor de optimale werking van de gegevens in het zware apparaat.

## <a name="software-requirements"></a>Softwarevereisten

De vereisten voor bevatten de gegevens op de ondersteunde besturingssystemen, de ondersteunde browsers voor de lokale web-UI en de SMB-clients.

### <a name="supported-operating-systems-for-clients"></a>Ondersteunde besturingssystemen voor clients

[!INCLUDE [data-box-supported-os-clients](../../includes/data-box-supported-os-clients.md)]

### <a name="supported-file-systems-for-linux-clients"></a>Ondersteunde bestandssystemen voor Linux-clients

[!INCLUDE [data-box-supported-file-systems-clients](../../includes/data-box-supported-file-systems-clients.md)]

### <a name="supported-storage-accounts"></a>Ondersteunde opslagaccounts

[!INCLUDE [data-box-supported-storage-accounts](../../includes/data-box-supported-storage-accounts.md)]

### <a name="supported-storage-types"></a>Ondersteunde opslagtypen

[!INCLUDE [data-box-supported-storage-types](../../includes/data-box-supported-storage-types.md)]

### <a name="supported-web-browsers"></a>Ondersteunde webbrowsers

[!INCLUDE [data-box-supported-web-browsers](../../includes/data-box-supported-web-browsers.md)]

## <a name="networking-requirements"></a>Netwerkvereisten

Uw datacenter moet een netwerk met hoge snelheid hebben. Voor de snelste kopie snelheden worden bereikt, kunnen twee 40-GbE-verbindingen parallel (één per knooppunt) worden gebruikt. Als u geen 40-GbE beschikbaar hebt, wordt u aangeraden dat u ten minste twee 10 GbE-verbindingen (één per knooppunt hebt).

## <a name="next-steps"></a>Volgende stappen

* [Implementeren van uw Azure Data Box](data-box-deploy-ordered.md)
