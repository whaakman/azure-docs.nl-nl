---
title: 'Een virtueel netwerk koppelen aan een ExpressRoute-circuit: PowerShell: klassieke: Azure | Microsoft Docs'
description: Dit document bevat een overzicht van hoe u virtuele netwerken (vnet's) koppelen aan ExpressRoute-circuits met behulp van het klassieke implementatiemodel en PowerShell.
services: expressroute
documentationcenter: na
author: ganesr
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 9b53fd72-9b6b-4844-80b9-4e1d54fd0c17
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2017
ms.author: ganesr
ms.openlocfilehash: 8df8a4c6ff0897c821e13248e0494b17e1a4992d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-powershell-classic"></a>Een virtueel netwerk verbinden met een ExpressRoute-circuit met behulp van PowerShell (klassiek)
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure-CLI](howto-linkvnet-cli.md)
> * [Video - Azure-portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-linkvnet-classic.md)
>

In dit artikel helpt u virtuele netwerken (vnet's) koppelen aan Azure ExpressRoute-circuits met behulp van het klassieke implementatiemodel en PowerShell. Virtuele netwerken in hetzelfde abonnement kunnen zijn of deel kunnen uitmaken van een ander abonnement.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Over Azure-implementatiemodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Configuratievereisten
1. U moet de meest recente versie van de Azure PowerShell-modules. U kunt de meest recente PowerShell-modules downloaden van de PowerShell-sectie van de [pagina Azure Downloads](https://azure.microsoft.com/downloads/). Volg de instructies in [installeren en configureren van Azure PowerShell](/powershell/azure/overview) voor stapsgewijze instructies voor het configureren van uw computer voor het gebruik van de Azure PowerShell-modules.
2. U moet controleren de [vereisten](expressroute-prerequisites.md), [routeringsvereisten](expressroute-routing.md), en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.
3. U moet een actief ExpressRoute-circuit hebben.
   * Volg de instructies voor [maken van een ExpressRoute-circuit](expressroute-howto-circuit-classic.md) en vraag uw connectiviteitsprovider het circuit inschakelen.
   * Zorg ervoor dat u persoonlijke Azure-peering voor uw circuit is geconfigureerd. Zie de [Configure routing](expressroute-howto-routing-classic.md) artikel voor routering instructies.
   * Zorg ervoor dat de persoonlijke Azure-peering is geconfigureerd en van de BGP-peering tussen uw netwerk en Microsoft is, zodat u kunt end-to-end-connectiviteit inschakelen.
   * U hebt een virtueel netwerk en een virtuele netwerkgateway gemaakt en volledig is ingericht. Volg de instructies voor [een virtueel netwerk configureren voor ExpressRoute](expressroute-howto-vnet-portal-classic.md).

U kunt maximaal 10 virtuele netwerken koppelen aan een ExpressRoute-circuit. Alle virtuele netwerken moeten zich in dezelfde geopolitieke regio. U kunt een groter aantal virtuele netwerken aan uw ExpressRoute-circuit of koppeling virtuele netwerken die zich in andere geopolitieke regio's als u de invoegtoepassing ExpressRoute premium ingeschakeld koppelen. Controleer de [Veelgestelde vragen over](expressroute-faqs.md) voor meer informatie over de premium-invoegtoepassing.

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Een virtueel netwerk in hetzelfde abonnement verbinden met een circuit
U kunt een virtueel netwerk koppelen aan een ExpressRoute-circuit met de volgende cmdlet. Zorg ervoor dat de virtuele netwerkgateway wordt gemaakt en gereed is voor het koppelen voordat u de cmdlet uitvoert.

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Een virtueel netwerk in een ander abonnement verbinden met een circuit
U kunt een ExpressRoute-circuit delen tussen meerdere abonnementen. De volgende afbeelding ziet een eenvoudige schematische van hoe delen werkt voor ExpressRoute-circuits voor meerdere abonnementen.

Elk van de kleinere clouds binnen de grote cloud wordt gebruikt voor abonnementen die bij verschillende afdelingen binnen een organisatie horen te vertegenwoordigen. Elk van de afdelingen binnen de organisatie kan hun eigen abonnement gebruiken voor het implementeren van hun services--, maar de afdelingen één ExpressRoute-circuit terugverbinding maken met uw on-premises netwerk kan delen. Één afdeling (in dit voorbeeld: IT) kunt eigenaar van het ExpressRoute-circuit. Andere abonnementen binnen de organisatie kunnen het ExpressRoute-circuit gebruiken.

> [!NOTE]
> Verbindingen en bandbreedte kosten voor het specifieke circuit wordt toegepast op de eigenaar van het ExpressRoute-circuit. Alle virtuele netwerken delen de dezelfde bandbreedte.
> 
> 

![Abonnementoverschrijdende connectiviteit](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Beheer
De *circuiteigenaar* is de beheerder/CO-beheerder van het abonnement waarin het ExpressRoute-circuit is gemaakt. De circuiteigenaar van het kan beheerders/coadministrators van andere abonnementen, aangeduid als machtigen *circuit gebruikers*, om de specifieke circuit waarvan ze eigenaar moet worden gebruikt. Circuit gebruikers die zijn gemachtigd voor het gebruik van de organisatie ExpressRoute-circuit kunnen het virtuele netwerk in het abonnement koppelen aan het ExpressRoute-circuit nadat ze zijn gemachtigd.

De circuiteigenaar van het bevoegd is om te wijzigen en machtigingen intrekt op elk gewenst moment. Intrekken van een vergunning leidt ertoe dat alle koppelingen worden verwijderd uit het abonnement waarvoor de toegang is ingetrokken.

### <a name="circuit-owner-operations"></a>Circuit eigenaar bewerkingen

**Maken van een vergunning**

De circuiteigenaar van het machtigt de beheerders van andere abonnementen te gebruiken van het opgegeven circuit. In het volgende voorbeeld kan de beheerder van het circuit (Contoso IT) de beheerder van een ander abonnement (Dev-Test) maximaal twee virtuele netwerken koppelen aan het circuit. De Contoso-IT-beheerder maakt dit door op te geven van de Microsoft Developer-Test-ID. De cmdlet verzenden geen e-mail naar de opgegeven id van Microsoft. De circuiteigenaar van het moet expliciet melden andere eigenaar van het abonnement dat de autorisatie voltooid is.

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

**Machtigingen controleren**

De circuiteigenaar van het Neem alle machtigingen die zijn uitgegeven voor een bepaalde circuit door de volgende cmdlet:

    Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"

    Description         : EngineeringTeam
    Limit               : 3
    LinkAuthorizationId : ####################################
    MicrosoftIds        : engadmin@contoso.com
    Used                : 1

    Description         : MarketingTeam
    Limit               : 1
    LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    MicrosoftIds        : marketingadmin@contoso.com
    Used                : 0

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : salesadmin@contoso.com
    Used                : 2


**Autorisaties bijwerken**

De circuiteigenaar van het kunt autorisaties wijzigen met behulp van de volgende cmdlet:

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


**Autorisaties verwijderen**

De circuiteigenaar van het kan intrekken/delete autorisaties voor de gebruiker door de volgende cmdlet:

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a>Bewerkingen voor circuit-gebruikers

**Machtigingen controleren**

De circuit-gebruiker kunt autorisaties bekijken met behulp van de volgende cmdlet:

    Get-AzureAuthorizedDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : ContosoIT
    Location                         : Washington DC
    MaximumAllowedLinks              : 2
    ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled
    UsedLinks                        : 0

**Wisselt autorisaties voor koppelingen**

De circuit-gebruiker kan de volgende cmdlet als u wilt gebruikmaken van een koppeling autorisatie uitvoeren:

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

Deze opdracht uitvoeren in het nieuwe gekoppelde abonnement voor het virtuele netwerk:

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).

