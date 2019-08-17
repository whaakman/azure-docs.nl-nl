---
title: VMware-oplossing door CloudSimple-Azure-netwerk verbindingen
description: Meer informatie over het verbinden van uw virtuele Azure-netwerk met uw CloudSimple-regio netwerk
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: eca3e316d866814f6727dd8ef2c3fa490a551383
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69563158"
---
# <a name="azure-network-connections-overview"></a>Overzicht van Azure-netwerk verbindingen

Wanneer u een CloudSimple-service in een regio maakt, doet u het volgende:

* Hiermee maakt u een Azure ExpressRoute-circuit en koppelt u deze aan de service in die regio.
* Verbindt uw CloudSimple Region-netwerk met uw virtuele Azure-netwerk of uw on-premises netwerk met behulp van Azure ExpressRoute.
* Biedt toegang tot services die worden uitgevoerd in uw Azure-abonnement of uw on-premises netwerk vanuit uw Privécloud.

De ExpressRoute-verbinding is een hoge band breedte met een lage latentie.

## <a name="benefits"></a>Voordelen

Met Azure-netwerk verbinding kunt u:

* Gebruik Azure als back-updoel voor virtuele machines in uw Privécloud.
* Implementeer KMS-servers in uw Azure-abonnement om uw vSAN-gegevens opslag in de Privécloud te versleutelen.
* Gebruik hybride toepassingen waarbij de weblaag van de toepassing wordt uitgevoerd in de open bare Cloud terwijl de lagen van de toepassing en de data base in uw Privécloud worden uitgevoerd.

## <a name="azure-virtual-network-connection"></a>Virtuele Azure-netwerk verbinding

Persoonlijke Clouds kunnen worden verbonden met uw Azure-resources met behulp van ExpressRoute.  Met de ExpressRoute-verbinding kunt u vanuit uw Privécloud toegang krijgen tot resources die worden uitgevoerd in uw Azure-abonnement.  Met deze verbinding kunt u uw particuliere cloud netwerk uitbreiden naar uw virtuele Azure-netwerk.

[![Azure ExpressRoute-verbinding met virtueel netwerk](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>ExpressRoute-verbinding met een on-premises netwerk

U kunt uw bestaande Azure ExpressRoute-circuit aansluiten op uw CloudSimple-regio. De functie ExpressRoute Global Reach wordt gebruikt om de twee circuits met elkaar te verbinden.  Er wordt een verbinding tot stand gebracht tussen de on-premises en CloudSimple ExpressRoute-circuits.  Deze verbinding stelt u in staat om uw on-premises netwerken uit te breiden naar een particulier Cloud netwerk.

![On-premises ExpressRoute-verbinding-Global Reach](media/cloudsimple-global-reach-connection.png)

## <a name="next-steps"></a>Volgende stappen

* [Virtueel Azure-netwerk verbinden met CloudSimple met behulp van ExpressRoute](virtual-network-connection.md)
* [Verbinding maken tussen on-premises en CloudSimple met behulp van ExpressRoute](on-premises-connection.md)
