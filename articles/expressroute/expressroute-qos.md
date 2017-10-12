---
title: QoS-vereisten voor ExpressRoute | Microsoft Docs
description: Deze pagina bevat gedetailleerde vereisten voor het configureren en beheren van de QoS voor ExpressRoute-circuits.
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: db1c1447-0283-4a09-907b-ae481adc40c7
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: cherylmc
ms.openlocfilehash: c097a9ccba91f59b323215d42d37e6d85e0981ce
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="expressroute-qos-requirements"></a>QoS-vereisten voor ExpressRoute
Skype voor Bedrijven heeft verschillende workloads die allemaal een andere QoS-behandeling vereisen. Als u via ExpressRoute voice-services wilt gaan gebruiken, moet u voldoen aan de vereisten die hieronder worden beschreven.

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> QoS-vereisten zijn alleen van toepassing op de Microsoft-peering. De DSCP-waarden in uw netwerkverkeer dat is ontvangen op Openbare Azure-peering en Persoonlijke Azure-peering wordt ingesteld op 0. 
> 
> 

De volgende tabel bevat een lijst van DSCP-markeringen die worden gebruikt door Skype voor Bedrijven. Raadpleeg [Managing QoS for Skype for Business](https://technet.microsoft.com/library/gg405409.aspx) (QoS voor Skype voor Bedrijven beheren) voor meer informatie.

| **Verkeersklasse** | **Behandeling (DSCP-markering)** | **Workloads voor Skype voor Bedrijven** |
| --- | --- | --- |
| **Spraak** |EF (46) |Skype / Lync voice |
| **Interactief** |AF41 (34) |Video, VBSS |
| AF21 (18) |Apps delen | |
| **Standaard** |AF11 (10) |Bestandsoverdracht |
| CS0 (0) |Overige | |

* U moet de workloads classificeren en de juiste DSCP-waarden markeren. Volg [deze](https://technet.microsoft.com/library/gg405409.aspx) richtlijnen over het instellen van DSCP-markeringen in uw netwerk.
* U moet meerdere QoS-wachtrijen in uw netwerk configureren en ondersteunen. Voice moet een zelfstandige klasse zijn en de EF-behandeling ontvangen die is opgegeven in RFC 3246. 
* U kunt het wachtrijmechanisme, het congestiedetectiebeleid en de bandbreedtetoewijzing per verkeersklasse bepalen. De DSCP-markering voor workloads voor Skype voor Bedrijven moet echter behouden blijven. Als u DSCP-markeringen gebruikt die niet worden weergegeven, bijvoorbeeld AF31 (26), moet u deze DSCP-waarde wijzigen in 0 voordat u het pakket naar Microsoft verzendt. Microsoft verzendt alleen pakketten die zijn gemarkeerd met de DSCP-waarde die wordt weergegeven in de bovenstaande tabel. 

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg de vereisten voor [Routering](expressroute-routing.md) en [NAT](expressroute-nat.md).
* Klik op de volgende koppelingen als u uw ExpressRoute-verbinding wilt configureren.
  
  * [Een ExpressRoute-circuit maken](expressroute-howto-circuit-classic.md)
  * [Routering configureren](expressroute-howto-routing-classic.md)
  * [Een VNet koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-classic.md)

