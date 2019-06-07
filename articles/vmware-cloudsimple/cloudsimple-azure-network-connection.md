---
title: VMware-oplossing door CloudSimple - verbindingen van Azure-netwerk
description: Meer informatie over uw Azure-netwerk verbinden met het netwerk van de regio CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f2ab82b6c1b4b373c186019eaf96f9864861b9d9
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66497639"
---
# <a name="azure-network-connections-overview"></a>Overzicht van de verbindingen van Azure-netwerk

Wanneer u een service CloudSimple in een regio en maakt het:

* Hiermee maakt u een Azure ExpressRoute-circuit en gekoppeld aan de service in deze regio
* Kan de verbinding van uw regio CloudSimple netwerk naar uw Azure-netwerk of uw on-premises netwerk met behulp van Azure ExpressRoute
* Biedt toegang tot services die worden uitgevoerd in uw Azure-abonnement of uw on-premises-netwerk van uw omgeving Private Cloud

De verbinding is:

* Beveiligen
* Privé
* Hoge bandbreedte
* Lage latentie

## <a name="benefits"></a>Voordelen

Azure-netwerkverbinding kunt u:

* Gebruik Azure als het doel van een back-up voor virtuele machines in uw Privécloud.
* KMS-servers in uw Azure-abonnement voor het versleutelen van uw virtueel SAN Private Cloud datastore implementeren.
* Hybride toepassingen waar de weblaag van de toepassing wordt uitgevoerd in de openbare cloud terwijl de toepassing en databaselagen uitvoeren in uw Privécloud gebruiken.

## <a name="azure-virtual-network-connection"></a>Azure virtuele netwerkverbinding

Privéclouds kunnen worden verbonden met uw Azure-resources met behulp van ExpressRoute.  U kunt deze verbinding gebruiken voor toegang tot andere resources in uw Azure-abonnement van uw Privécloud uitgevoerd.  Deze verbinding kunt u uitbreiden u Private Cloud-netwerk met uw Azure-netwerk.

![Azure ExpressRoute-verbinding met virtual network](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>ExpressRoute-verbinding met on-premises netwerk

U kunt uw bestaande Azure ExpressRoute-circuit voor uw regio CloudSimple verbinden. ExpressRoute globaal bereik functie wordt gebruikt om de twee circuits met elkaar verbinding te maken.  Een verbinding tot stand is gebracht tussen de on-premises en CloudSimple ExpressRoute-circuits.  Deze verbinding kunt u uw on-premises netwerken met Private Cloud netwerk uitbreiden.

![On-premises ExpressRoute-verbinding - wereldwijd bereik](media/cloudsimple-global-reach-connection.png)

## <a name="next-steps"></a>Volgende stappen

* [Peering informatie voor Azure-netwerk op CloudSimple verbinding verkrijgen](https://docs.azure.cloudsimple.com/virtual-network-connection)
* [Verbinding maken tussen on-premises en CloudSimple met behulp van ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection)
