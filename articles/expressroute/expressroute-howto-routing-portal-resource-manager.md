---
title: 'Het configureren van routering (peering) voor een ExpressRoute-circuit: Resource Manager: Azure | Microsoft Docs'
description: Dit artikel begeleidt u stapsgewijs door de procedure voor het maken en inrichten van de persoonlijke, openbare en Microsoft-peering van een ExpressRoute-circuit. In dit artikel leest u hoe u de status controleert en peerings voor uw circuit bijwerkt of verwijdert.
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c2a7ed2-ae5c-4e49-81f6-77cf9f2b2ac9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2017
ms.author: cherylmc
ms.openlocfilehash: be25e9ffab4fee79b8d9cc6c88c6ffb3e852af0d
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a>Maken en wijzigen van de peering voor een ExpressRoute-circuit

In dit artikel helpt u bij het maken en beheren van routeringsconfiguratie voor een ExpressRoute-circuit in het Resource Manager-implementatiemodel met de Azure portal. U kunt ook controleren van de status, update of delete en inrichting ervan ongedaan peerings voor een ExpressRoute-circuit. Als u een andere methode gebruiken om te werken met uw circuit wilt, selecteert u een artikel uit de volgende lijst:


## <a name="configuration-prerequisites"></a>Configuratievereisten

* Zorg dat u de pagina met [vereisten](expressroute-prerequisites.md), de pagina over [routeringsvereisten](expressroute-routing.md) en de pagina over [werkstromen](expressroute-workflows.md) hebt gelezen voordat u begint met de configuratie.
* U moet een actief ExpressRoute-circuit hebben. Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-portal-resource-manager.md) en laat het circuit inschakelen door de connectiviteitsprovider voordat u verder gaat. Het ExpressRoute-circuit moet zich in een status ingericht en zijn ingeschakeld om te kunnen uitvoeren van de cmdlets in de volgende secties.
* Als u van plan bent te gebruiken van een gedeelde sleutel/MD5-hash, moet u dit gebruikt op beide zijden van de tunnel en beperkt het aantal tekens tot maximaal 25.

Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die services met Laag-2-connectiviteit aanbieden. Als u gebruikmaakt van een serviceprovider die beheerde laag-3-services (meestal een IPVPN, zoals MPLS), uw connectiviteitsprovider configureert en beheert routering voor u. 

> [!IMPORTANT]
> Op dit moment bieden we nog geen peerings aan die door serviceproviders worden geconfigureerd via de beheerportal van de service. Deze mogelijkheid zal binnenkort worden ingeschakeld. Neem contact op met uw serviceprovider voordat u BGP-peerings configureert.
> 
> 

U kunt een, twee of alle drie de peerings (Azure privé, Azure openbaar en Microsoft) voor een ExpressRoute-circuit configureren. U kunt peerings configureren in elke gewenste volgorde. U moet er echter wel voor zorgen dat u de configuratie van elke peering een voor een voltooit. Zie voor meer informatie over routering domeinen en peerings [ExpressRoute-Routeringsdomeinen](expressroute-circuit-peerings.md).

## <a name="msft"></a>Microsoft-peering

Deze sectie helpt u bij het maken, verkrijgen, bijwerken en verwijderen van de configuratie voor de Microsoft-peering voor een ExpressRoute-circuit.

> [!IMPORTANT]
> Microsoft-peering van ExpressRoute-circuits die zijn geconfigureerd, voordat u 1 augustus 2017 hebben alle service voorvoegsels die worden geadverteerd via de Microsoft-peering, zelfs als routefilters zijn niet gedefinieerd. Microsoft-peering van ExpressRoute-circuits die zijn geconfigureerd op of na 1 augustus 2017 geen geen voorvoegsels aangekondigd totdat een routefilter is gekoppeld aan het circuit. Zie voor meer informatie [configureren van een routefilter voor Microsoft-peering](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Microsoft-peering maken

[!INCLUDE [Premium](../../includes/expressroute-mspeering-premium-include.md)]

1. Configureer het ExpressRoute-circuit. Zorg dat het circuit volledig is ingericht door de connectiviteitsprovider voordat u verder gaat. Als uw connectiviteitsprovider beheerde laag-3-services biedt, kunt u uw connectiviteitsprovider om in te schakelen van Microsoft-peering voor u te vragen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routering voor u, na het maken van het circuit niet beheert blijven echter uw configuratie met behulp van de volgende stappen.

  ![Microsoft-peering weergeven](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Configureer Microsoft-peering voor het circuit. Zorg ervoor dat u over de volgende informatie beschikt voordat u verder gaat.

  * Een /30-subnet voor de primaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR.
  * Een /30-subnet voor de secundaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR.
  * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt.
  * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken.
  * Geadverteerde voorvoegsels: u moet een lijst verstrekken van alle voorvoegsels die u via de BGP-sessie wilt adverteren. Alleen openbare IP-adresvoorvoegsels worden geaccepteerd. Als u van plan bent een reeks voorvoegsels verzenden, kunt u een door komma's gescheiden lijst verzenden. Deze voorvoegsels moeten voor u zijn geregistreerd in een RIR/IRR.
  * **Optionele -** klant-ASN: als u voorvoegsels adverteert die niet zijn geregistreerd op de AS-nummer peering, kunt u het AS-nummer waaraan ze zijn geregistreerd.
  * Naam van routeringsregister: u kunt het RIR/IRR opgeven waarbij het AS-nummer en de voorvoegsels zijn geregistreerd.
  * **Optionele -** een MD5-hash, als u ervoor kiest een te gebruiken.
3. U kunt de peering die u configureren, wilt zoals wordt weergegeven in het volgende voorbeeld. Selecteer de rij voor Microsoft-peering.

  ![Selecteer de rij voor Microsoft-peering](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. Configureer Microsoft-peering. De volgende afbeelding toont een configuratievoorbeeld van een:

  ![Configureer Microsoft-peering](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. Sla de configuratie op wanneer u alle parameters hebt opgegeven.

  Als het circuit moet worden van een 'validatie nodig' status (zoals weergegeven in de afbeelding), moet u een ondersteuningsticket om aan te tonen dat u eigenaar bent van de voorvoegsels aan ons ondersteuningsteam aan openen.

  ![Microsoft-peeringconfiguratie opslaan](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

  U kunt een ondersteuningsticket rechtstreeks vanuit de portal openen, zoals wordt weergegeven in het volgende voorbeeld:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. Nadat de configuratie is geaccepteerd, ziet u iets soortgelijks als in de volgende afbeelding:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="getmsft"></a>Details van Microsoft-peering weergeven

U kunt de eigenschappen van openbare Azure-peering weergeven door de peering te selecteren.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="updatemsft"></a>Configuratie van Microsoft-peering bijwerken

U kunt de rij voor peering selecteren en de eigenschappen van de peering wijzigen.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="deletemsft"></a>Microsoft-peering verwijderen

U kunt de peeringconfiguratie verwijderen door het verwijderingspictogram, zoals wordt weergegeven in de volgende afbeelding:

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="private"></a>Persoonlijke Azure-peering

Deze sectie helpt u bij het maken, verkrijgen, bijwerken en verwijderen van de Azure persoonlijke peering configuratie voor een ExpressRoute-circuit.

### <a name="to-create-azure-private-peering"></a>Persoonlijke Azure-peering maken

1. Configureer het ExpressRoute-circuit. Zorg dat het circuit volledig is ingericht door de connectiviteitsprovider voordat u verder gaat. Als uw connectiviteitsprovider beheerde laag-3-services biedt, kunt u uw connectiviteitsprovider om in te schakelen persoonlijke Azure-peering voor u te vragen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routering voor u, na het maken van het circuit niet beheert blijven echter uw configuratie met behulp van de volgende stappen.

  ![lijst](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Configureer persoonlijke Azure-peering voor het circuit. Zorg ervoor dat u de volgende items hebt voordat u verdergaat met de volgende stappen:

  * Een /30-subnet voor de primaire koppeling. Het subnet moet deel uitmaken van een adresruimte gereserveerd voor virtuele netwerken niet.
  * Een /30-subnet voor de secundaire koppeling. Het subnet moet deel uitmaken van een adresruimte gereserveerd voor virtuele netwerken niet.
  * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt.
  * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken. U kunt een persoonlijk AS-nummer voor deze peering gebruiken. Zorg dat u niet 65515 gebruikt.
  * **Optionele -** een MD5-hash, als u ervoor kiest een te gebruiken.
3. Selecteer de rij van het Azure persoonlijke peering, zoals wordt weergegeven in het volgende voorbeeld:

  ![Persoonlijke](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. Configureer persoonlijke Azure-peering. De volgende afbeelding toont een configuratievoorbeeld van een:

  ![Configureer persoonlijke peering](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. Sla de configuratie op wanneer u alle parameters hebt opgegeven. Nadat de configuratie is geaccepteerd, ziet u iets soortgelijks als in het volgende voorbeeld:

  ![persoonlijke peering opslaan](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="getprivate"></a>Azure details van persoonlijke peering weergeven

U kunt de eigenschappen van persoonlijke Azure-peering weergeven door de peering te selecteren.

![persoonlijke peering weergeven](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="updateprivate"></a>Configuratie van Azure persoonlijke peering bijwerken

U kunt de rij voor peering selecteren en de eigenschappen van de peering wijzigen.

![persoonlijke peering bijwerken](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="deleteprivate"></a>Persoonlijke Azure-peering verwijderen

U kunt de peeringconfiguratie verwijderen door het verwijderingspictogram, zoals wordt weergegeven in de volgende afbeelding:

![persoonlijke peering verwijderen](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="public"></a>Openbare Azure-peering

Deze sectie helpt u bij het maken, verkrijgen, bijwerken en verwijderen van de Azure openbare peering configuratie voor een ExpressRoute-circuit.

### <a name="to-create-azure-public-peering"></a>Openbare Azure-peering maken

1. Configureer het ExpressRoute-circuit. Zorg dat het circuit volledig is ingericht door de connectiviteitsprovider voordat u verder gaat. Als uw connectiviteitsprovider beheerde laag-3-services biedt, kunt u uw connectiviteitsprovider om in te schakelen openbare Azure-peering voor u te vragen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routering voor u, na het maken van het circuit niet beheert blijven echter uw configuratie met behulp van de volgende stappen.

  ![openbare peering weergeven](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Configureer openbare Azure-peering voor het circuit. Zorg ervoor dat u de volgende items hebt voordat u verdergaat met de volgende stappen:

  * Een /30-subnet voor de primaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn.
  * Een /30-subnet voor de secundaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn.
  * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt.
  * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken.
  * **Optionele -** een MD5-hash, als u ervoor kiest een te gebruiken.
3. Selecteer de Azure rij openbare peering, zoals weergegeven in de volgende afbeelding:

  ![Selecteer de rij voor openbare peering](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. Configureer openbare Azure-peering. De volgende afbeelding toont een configuratievoorbeeld van een:

  ![Configureer openbare peering](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. Sla de configuratie op wanneer u alle parameters hebt opgegeven. Nadat de configuratie is geaccepteerd, ziet u iets soortgelijks als in het volgende voorbeeld:

  ![Configuratie van openbare peering opslaan](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="getpublic"></a>Details van een Azure openbare peering weergeven

U kunt de eigenschappen van openbare Azure-peering weergeven door de peering te selecteren.

![de eigenschappen van openbare peering weergeven](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="updatepublic"></a>Configuratie van een Azure openbare peering bijwerken

U kunt de rij voor peering selecteren en de eigenschappen van de peering wijzigen.

![Selecteer de rij voor openbare peering](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="deletepublic"></a>Openbare Azure-peering verwijderen

U kunt de peeringconfiguratie verwijderen door het verwijderingspictogram, zoals wordt weergegeven in het volgende voorbeeld:

![openbare peering verwijderen](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="next-steps"></a>Volgende stappen

Volgende stap, [een VNet koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-portal-resource-manager.md)
* Voor meer informatie over ExpressRoute-werkstromen raadpleegt u [ExpressRoute workflows](expressroute-workflows.md) (ExpressRoute-werkstromen).
* Voor meer informatie over circuitpeering raadpleegt u [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-circuits en -routeringsdomeinen).
* Bekijk het [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Virtual Network-overzicht) voor meer informatie over het gebruik van virtuele netwerken.