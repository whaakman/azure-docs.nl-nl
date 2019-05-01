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
ms.openlocfilehash: 8ea98d6493b824bfa232ef8193388e93b97c506b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/26/2019
ms.locfileid: "64576998"
---
# <a name="azure-network-connection-overview"></a>Verbinding met Azure-netwerk-overzicht

Wanneer u een service CloudSimple in een regio en maakt het:

* Azure ExpressRoute-circuit gemaakt en gekoppeld aan de service in deze regio
* Uw regio CloudSimple netwerk verbindt met uw Azure-netwerk of uw on-premises netwerk met behulp van Azure ExpressRoute
* Biedt toegang tot services die worden uitgevoerd in uw Azure-abonnement of uw on-premises-netwerk van uw persoonlijke cloudomgeving

Deze verbinding is een hoge bandbreedte met lage latentie.

## <a name="benefits"></a>Voordelen

Azure-netwerkverbinding kunt u:

* Gebruik Azure als het doel van een back-up voor virtuele machines in uw Privécloud.
* KMS-servers in uw Azure-abonnement voor het versleutelen van uw virtueel SAN Private Cloud datastore implementeren.
* Hybride toepassingen waar de weblaag van de toepassing wordt uitgevoerd in de openbare cloud terwijl de toepassing en databaselagen uitvoeren in uw Privécloud gebruiken.

## <a name="expressroute-connection-to-on-premises-network"></a>ExpressRoute-verbinding met on-premises netwerk

U kunt uw bestaande Azure ExpressRoute-circuit voor uw regio CloudSimple verbinden. ExpressRoute globaal bereik functie wordt gebruikt om de twee circuits met elkaar verbinding te maken.  Een verbinding tot stand is gebracht tussen on-premises en CloudSimple ExpressRoute-circuits.

Deze methode maakt een verbinding tussen de twee omgevingen die is:

* Beveiligen
* Privé
* Hoge bandbreedte
* Lage latentie

Het maken van een ExpressRoute-verbinding met een on-premises netwerk [contact op met ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Volgende stappen

* [Instellen van virtuele netwerkverbinding](https://docs.azure.cloudsimple.com/virtual-network-connection)