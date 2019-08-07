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
ms.openlocfilehash: a8e99da05f71cb01744111b41c301b11a0969057
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812709"
---
# <a name="azure-network-connections-overview"></a>Overzicht van Azure-netwerk verbindingen

Wanneer u een CloudSimple-service in een regio maakt, doet u het volgende:

* Hiermee wordt een Azure ExpressRoute-circuit gemaakt en gekoppeld aan de service in die regio
* Maakt verbinding vanuit uw CloudSimple Region-netwerk met uw virtuele Azure-netwerk of uw on-premises netwerk mogelijk met behulp van Azure ExpressRoute
* Geeft toegang tot services die worden uitgevoerd in uw Azure-abonnement, of uw on-premises netwerk, vanuit uw Privécloud

De verbinding is:

* Beveiligen
* Privé
* Hoge band breedte
* Lage latentie

## <a name="benefits"></a>Voordelen

Met Azure-netwerk verbinding kunt u:

* Gebruik Azure als back-updoel voor virtuele machines in uw Privécloud.
* Implementeer KMS-servers in uw Azure-abonnement om uw vSAN-gegevens opslag in de Privécloud te versleutelen.
* Gebruik hybride toepassingen waarbij de weblaag van de toepassing wordt uitgevoerd in de open bare Cloud terwijl de lagen van de toepassing en de data base in uw Privécloud worden uitgevoerd.

## <a name="azure-virtual-network-connection"></a>Virtuele Azure-netwerk verbinding

Persoonlijke Clouds kunnen worden verbonden met uw Azure-resources met behulp van ExpressRoute.  U kunt deze verbinding gebruiken voor toegang tot verschillende bronnen die worden uitgevoerd in uw Azure-abonnement vanuit uw Privécloud.  Met deze verbinding kunt u uw particuliere cloud netwerk uitbreiden naar uw virtuele Azure-netwerk.

![Azure ExpressRoute-verbinding met virtueel netwerk](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>ExpressRoute-verbinding met een on-premises netwerk

U kunt uw bestaande Azure ExpressRoute-circuit aansluiten op uw CloudSimple-regio. De functie ExpressRoute Global Reach wordt gebruikt om de twee circuits met elkaar te verbinden.  Er wordt een verbinding tot stand gebracht tussen de on-premises en CloudSimple ExpressRoute-circuits.  Deze verbinding stelt u in staat om uw on-premises netwerken uit te breiden naar een particulier Cloud netwerk.

![On-premises ExpressRoute-verbinding-Global Reach](media/cloudsimple-global-reach-connection.png)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over peering voor Azure Virtual Network verkrijgen voor CloudSimple-verbinding](https://docs.azure.cloudsimple.com/virtual-network-connection)
* [Verbinding maken tussen on-premises en CloudSimple met behulp van ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection)
