---
title: Werkstromen voor het configureren van een ExpressRoute-circuit | Microsoft Docs
description: Deze pagina wordt u begeleid bij de werkstromen voor het configureren van ExpressRoute-circuit en -peerings
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 55e0418c-e0bf-44a7-9aa1-720076df9297
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: cherylmc
ms.openlocfilehash: cba1b2cfee379e7d2b079bcb3089981ef1044d66
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a>ExpressRoute-werkstromen voor circuitinrichting en -statussen
Deze pagina doorloopt u de service-inrichting en configuratie werkstromen routeren op hoog niveau.

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

De volgende afbeelding en de bijbehorende stappen wordt de taken die u volgen moet om een ExpressRoute-circuit ingericht end-to-end weergeven. 

1. PowerShell gebruiken voor het configureren van een ExpressRoute-circuit. Volg de instructies in de [maken ExpressRoute-circuits](expressroute-howto-circuit-classic.md) artikel voor meer informatie.
2. De verbinding van de volgorde van de serviceprovider. Dit proces is. Neem contact op met uw connectiviteitsprovider voor meer informatie over het rangschikken van connectiviteit.
3. Zorg ervoor dat het circuit is ingericht met succes gecontroleerd of het ExpressRoute-circuit Inrichtingsstatus via PowerShell. 
4. Configureer Routeringsdomeinen. Als uw connectiviteitsprovider beheert de Layer 3 voor u, zal ze configureren voor uw circuit routering. Als uw connectiviteitsprovider alleen Layer 2-services biedt, moet u routering per richtlijnen die worden beschreven in de [routeringsvereisten](expressroute-routing.md) en [routeringsconfiguratie](expressroute-howto-routing-classic.md) pagina's.
   
   * Inschakelen van persoonlijke Azure-peering - moet u deze peering als u wilt verbinding maken met virtuele machines / cloudservices geïmplementeerd in virtuele netwerken inschakelen.
   * Inschakelen van openbare Azure-peering - moet u de openbare Azure-peering als u wilt verbinding maken met Azure-services die worden gehost op openbare IP-adressen inschakelen. Dit is een vereiste voor toegang tot Azure-resources als u hebt gekozen om in te schakelen standaardroutering voor persoonlijke Azure-peering.
   * Schakel Microsoft-peering - u moet dit inschakelt voor toegang tot Office 365 en Dynamics 365. 
     
     > [!IMPORTANT]
     > U moet ervoor zorgen dat u een afzonderlijke proxy gebruiken / edge verbinding maken met Microsoft dan de versie die u gebruikt voor het Internet. Met behulp van de rand voor ExpressRoute- en Internet veroorzaken asymmetrische Routering en leiden tot uitval connectiviteit voor uw netwerk.
     > 
     > 
     
     ![](./media/expressroute-workflows/routing-workflow.png)
5. Virtuele netwerken koppelen aan ExpressRoute-circuits - kunt u virtuele netwerken koppelen aan uw ExpressRoute-circuit. Volg de instructies [VNets koppelen](expressroute-howto-linkvnet-arm.md) aan uw circuit. Deze VNets in dezelfde Azure-abonnement als het ExpressRoute-circuit kan zijn of kunnen zich in een ander abonnement.

## <a name="expressroute-circuit-provisioning-states"></a>ExpressRoute-circuit inrichtingstatuswaarden
Elk ExpressRoute-circuit heeft twee statussen:

* Serviceprovider Inrichtingsstatus
* Status

Status vertegenwoordigt van Microsoft-Inrichtingsstatus. Deze eigenschap is ingesteld op ingeschakeld wanneer u een Expressroute-circuit maken

De provider-Inrichtingsstatus connectiviteit vertegenwoordigt de status van de connectiviteitsprovider zijde. Het kan zijn *NotProvisioned*, *inrichten*, of *ingericht*. Het ExpressRoute-circuit moet zijn ingericht voor u te kunnen gebruiken.

### <a name="possible-states-of-an-expressroute-circuit"></a>Mogelijke statussen van een ExpressRoute-circuit
Deze sectie vindt u uit de mogelijke statussen voor een ExpressRoute-circuit.

**Tijdens het maken**

U ziet het ExpressRoute-circuit in de volgende status zodra u de PowerShell-cmdlet voor het maken van het ExpressRoute-circuit worden uitgevoerd.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


**Wanneer de connectiviteitsprovider is bezig het inrichten van het circuit**

U ziet het ExpressRoute-circuit in de volgende status zodra u de servicesleutel aan de connectiviteitsprovider doorgeven en ze tijdens het inrichtingsproces hebt gestart.

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


**Wanneer de connectiviteitsprovider tijdens het inrichtingsproces is voltooid**

U ziet het ExpressRoute-circuit in de volgende status zodra de connectiviteitsprovider tijdens het inrichtingsproces is voltooid.

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

Ingericht en ingeschakeld is dat de enige staat het circuit kan zich in te kunnen gebruiken. Als u een Layer 2-provider gebruikt, kunt u configureren routering voor uw circuit alleen wanneer deze status heeft.

**Wanneer de connectiviteitsprovider het circuit is laagsjabloon**

Als u de serviceprovider voor het ExpressRoute-circuit inrichting ervan ongedaan hebt aangevraagd, ziet u het circuit is ingesteld op de volgende status nadat de serviceprovider de inrichting proces is voltooid.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


U kunt deze opnieuw inschakelen als nodig is of PowerShell-cmdlets voor het verwijderen van het circuit worden uitgevoerd.  

> [!IMPORTANT]
> Als u de PowerShell-cmdlet voor het verwijderen van het circuit bij het inrichten van de ServiceProviderProvisioningState of ingericht, de bewerking mislukt uitvoert. Neem contact op met uw connectiviteitsprovider om na te inrichting ervan ongedaan maakt het ExpressRoute-circuit eerst en verwijder vervolgens het circuit. Microsoft blijft het circuit factureren totdat u de PowerShell-cmdlet voor het verwijderen van het circuit uitvoert.
> 
> 

## <a name="routing-session-configuration-state"></a>Configuratie van routering sessiestatus
De Inrichtingsstatus BGP laat u weten als de BGP-sessie op de Microsoft edge is ingeschakeld. De status moet zijn ingeschakeld om te kunnen gebruiken de peering.

Het is belangrijk om te controleren van de status van de BGP-sessie met name voor Microsoft-peering. Naast het BGP Inrichtingsstatus, is een andere status aangeroepen *aangekondigde openbare voorvoegsels status*. De status van de aangekondigde openbare voorvoegsels moet *geconfigureerd* state, zowel voor de BGP-sessie worden omhoog als voor uw rondsturen werkt end-to-end. 

Als de status van de aangekondigde openbare voorvoegsel is ingesteld op een *validatie nodig* staat, de BGP-sessie niet is ingeschakeld, zoals de geadverteerde voorvoegsels komt niet overeen met het AS-nummer in een van de routeringsregisters. 

> [!IMPORTANT]
> Als de status aangekondigde openbare voorvoegsels *handmatige validatie* staat, moet u een ondersteuningsticket met openen [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) en bewijzen dat u eigenaar van de IP-adressen die zijn aangekondigd samen met de gekoppelde autonoom systeemnummer.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* Configureer uw ExpressRoute-verbinding.
  
  * [Een ExpressRoute-circuit maken](expressroute-howto-circuit-arm.md)
  * [Routering configureren](expressroute-howto-routing-arm.md)
  * [Een VNet koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-arm.md)

