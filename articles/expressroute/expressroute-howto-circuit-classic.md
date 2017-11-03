---
title: 'Maken en een ExpressRoute-circuit wijzigen: PowerShell: klassieke Azure Portal | Microsoft Docs'
description: Dit artikel begeleidt u bij de stappen voor het maken en een ExpressRoute-circuit inrichten. Dit artikel ziet u ook het controleren van de status, update of delete en inrichting ervan ongedaan maakt het circuit.
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0134d242-6459-4dec-a2f1-4657c3bc8b23
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 93ddc2975db34053c6a776d1c3b931536f3f8ec7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a>Maken en een ExpressRoute-circuit met behulp van PowerShell (klassiek) wijzigen
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure-portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-circuit-classic.md)
>

Dit artikel begeleidt u bij de stappen voor het maken van een Azure ExpressRoute-circuit met behulp van PowerShell-cmdlets en het klassieke implementatiemodel. Dit artikel ziet u ook het controleren van de status, update of delete en een ExpressRoute-circuit inrichting ervan ongedaan maakt.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


**Over Azure-implementatiemodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Voordat u begint
### <a name="step-1-review-the-prerequisites-and-workflow-articles"></a>Step 1. Raadpleeg de vereisten en werkstroom artikelen
Zorg ervoor dat u hebt bekeken de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.  

### <a name="step-2-install-the-latest-versions-of-the-azure-service-management-sm-powershell-modules"></a>Stap 2. De nieuwste versies van de Azure Service Management (SM) PowerShell-modules installeren
Volg de instructies in [aan de slag met Azure PowerShell-cmdlets](/powershell/azure/overview) voor stapsgewijze instructies voor het configureren van uw computer voor het gebruik van de Azure PowerShell-modules.

### <a name="step-3-log-in-to-your-azure-account-and-select-a-subscription"></a>Stap 3. Aanmelden bij uw Azure-account en een abonnement selecteren
1. Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

        Login-AzureRmAccount

2. Controleer de abonnementen voor het account.

        Get-AzureRmSubscription

3. Als u meerdere abonnementen hebt, selecteert u het abonnement dat u wilt gebruiken.

        Select-AzureSubscription -SubscriptionId "Replace_with_your_subscription_id"

4. Controleer als de geselecteerde abonnements-id is ingesteld als standaard.

        Get-AzureSubscription -default

## <a name="create-and-provision-an-expressroute-circuit"></a>Maken en een ExpressRoute-circuit inrichten
### <a name="step-1-import-the-powershell-modules-for-expressroute"></a>Step 1. Importeer de PowerShell-modules voor ExpressRoute
 Als u dit nog niet hebt gedaan, moet u de Azure en een ExpressRoute-modules importeren in de PowerShell-sessie om te beginnen met behulp van de ExpressRoute-cmdlets. U kunt de modules importeren vanaf de locatie waar ze zijn geïnstalleerd op uw lokale computer. Afhankelijk van de methode die u gebruikt voor het installeren van de modules, de locatie mogelijk anders dan het volgende voorbeeld wordt getoond. In het voorbeeld desgewenst wijzigen.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>Stap 2. De lijst met ondersteunde providers, locaties en bandbreedten ophalen
Voordat u een ExpressRoute-circuit maken, moet u de lijst met ondersteunde connectiviteitsproviders, locaties en bandbreedte-opties.

De PowerShell-cmdlet `Get-AzureDedicatedCircuitServiceProvider` retourneert deze informatie, die u in latere stappen:

    Get-AzureDedicatedCircuitServiceProvider

Controleer of uw connectiviteitsprovider er wordt weergegeven. Noteer de volgende informatie omdat u hebt deze later nodig bij het maken van een circuit:

* Naam
* PeeringLocations
* BandwidthsOffered

U kunt nu gereed voor het maken van een ExpressRoute-circuit.         

### <a name="step-3-create-an-expressroute-circuit"></a>Stap 3. Een ExpressRoute-circuit maken
Het volgende voorbeeld ziet hoe u een 200-Mbps ExpressRoute-circuit via Equinix in Silicon Valley maakt. Als u een andere provider en andere instellingen gebruikt, vervangt u die informatie door wanneer u uw aanvraag.

> [!IMPORTANT]
> Uw ExpressRoute-circuit worden gefactureerd vanaf het moment dat de sleutel van een service wordt verleend. Zorg ervoor dat u deze bewerking niet uitvoeren wanneer de connectiviteitsprovider gereed is voor het inrichten van het circuit.
> 
> 

Hier volgt een voorbeeld van de aanvraag voor een nieuwe servicesleutel:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Of als u wilt maken van een ExpressRoute-circuit met de premium-invoegtoepassing, gebruikt u het volgende voorbeeld. Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor meer informatie over de premium-invoegtoepassing.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Het antwoord bevat de sleutel van de service. U kunt gedetailleerde beschrijvingen van alle parameters krijgen door het uitvoeren van het volgende:

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-the-expressroute-circuits"></a>Stap 4. Lijst van alle ExpressRoute-circuits
U kunt uitvoeren de `Get-AzureDedicatedCircuit` -opdracht voor een lijst met alle ExpressRoute-circuits die u hebt gemaakt:

    Get-AzureDedicatedCircuit

Het antwoord is iets soortgelijks als in het volgende voorbeeld:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

U kunt deze informatie op elk gewenst moment ophalen met behulp van de `Get-AzureDedicatedCircuit` cmdlet. De oproep zonder parameters voor een lijst met alle circuits. De sleutel van uw service wordt weergegeven in de *ServiceKey* veld.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

U kunt gedetailleerde beschrijvingen van alle parameters krijgen door het uitvoeren van het volgende:

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>Stap 5. De servicesleutel verzenden naar uw connectiviteitsprovider voor inrichting
*ServiceProviderProvisioningState* bevat informatie over de huidige status van inrichting aan de kant van de serviceprovider. *Status* biedt u de status aan de kant van Microsoft. Zie voor meer informatie over het circuit inrichtingstatuswaarden de [werkstromen](expressroute-workflows.md#expressroute-circuit-provisioning-states) artikel.

Wanneer u een nieuwe ExpressRoute-circuit maakt, wordt het circuit worden in de volgende status:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Het circuit gaat naar de volgende status wanneer de connectiviteitsprovider wordt deze voor u inschakelen:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Een ExpressRoute-circuit moet zich in de volgende status om te kunnen gebruiken:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>Stap 6. Controleer regelmatig de status en de status van de sleutel van het circuit
Dit laat u weten wanneer uw provider uw circuit is ingeschakeld. Na de configuratie van het circuit *ServiceProviderProvisioningState* weergegeven als *ingericht* zoals weergegeven in het volgende voorbeeld:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a>Stap 7. Maken van uw configuratie van de routering
Raadpleeg de [routeringsconfiguratie voor ExpressRoute-circuit (maken en wijzigen van circuit peerings)](expressroute-howto-routing-classic.md) artikel voor stapsgewijze instructies.

> [!IMPORTANT]
> Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die services op laag 2-connectiviteit aanbieden. Als u een serviceprovider die beheerde laag-3-services (meestal een IP VPN, zoals MPLS), uw connectiviteitsprovider configureren en beheren van routering voor u.
> 
> 

### <a name="step-8-link-a-virtual-network-to-an-expressroute-circuit"></a>Stap 8. Een virtueel netwerk koppelen aan een ExpressRoute-circuit
Vervolgens moet u een virtueel netwerk koppelen aan uw ExpressRoute-circuit. Raadpleeg [koppelen ExpressRoute-circuits aan virtuele netwerken](expressroute-howto-linkvnet-classic.md) voor stapsgewijze instructies. Als u moet een virtueel netwerk maken met het klassieke implementatiemodel voor ExpressRoute, Zie [een virtueel netwerk maken voor ExpressRoute](expressroute-howto-vnet-portal-classic.md).

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Opvragen van de status van een ExpressRoute-circuit
U kunt deze informatie op elk gewenst moment ophalen met behulp van de `Get-AzureCircuit` cmdlet. De oproep zonder parameters voor een lijst met alle circuits.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

U kunt de informatie op een specifieke ExpressRoute-circuit opvragen door de servicesleutel wordt doorgegeven als parameter voor de aanroep.

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


U kunt gedetailleerde beschrijvingen van alle parameters krijgen door het uitvoeren van het volgende voorbeeld:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Wijzigen van een ExpressRoute-circuit
U kunt bepaalde eigenschappen van een ExpressRoute-circuit wijzigen zonder enige impact op connectiviteit.

U kunt de volgende zonder uitvaltijd doen:

* In- of uitschakelen van een ExpressRoute premium-invoegtoepassing voor ExpressRoute-circuit.
* De bandbreedte van uw ExpressRoute-circuit vergroten, mits er capaciteit beschikbaar is op de poort. Houd er rekening mee dat de bandbreedte van een circuit downgraden wordt niet ondersteund. 
* Wijzig het softwarelicentiecontrole plan van Datalimiet gegevens in onbeperkte gegevens. Houd er rekening mee dat het plan softwarelicentiecontrole wijzigen van onbeperkt naar gemeten gegevens niet wordt ondersteund.
* U kunt inschakelen en uitschakelen *klassieke bewerkingen toestaan*.

Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor meer informatie over limieten en beperkingen.

### <a name="to-enable-the-expressroute-premium-add-on"></a>De invoegtoepassing ExpressRoute premium inschakelen
U kunt de invoegtoepassing ExpressRoute premium inschakelen voor uw bestaande circuit met de volgende PowerShell-cmdlet:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Het circuit hebt nu de ExpressRoute premium-invoegtoepassing voor functies ingeschakeld. Houd er rekening mee dat we facturering voor de premium-invoegtoepassing mogelijkheid start zodra de opdracht met succes is uitgevoerd.

### <a name="to-disable-the-expressroute-premium-add-on"></a>De invoegtoepassing ExpressRoute premium uitschakelen
> [!IMPORTANT]
> Deze bewerking kan mislukken als u resources die groter zijn dan wat voor het standaard circuit is toegestaan.
> 
> 

#### <a name="considerations"></a>Overwegingen

* U moet ervoor zorgen dat het aantal virtuele netwerken die zijn gekoppeld aan het circuit minder dan 10 is voordat u een van premium op standaard downgrade. Als u dit niet doet, worden uw aanvraag bijwerken mislukt en u de premietarieven in rekening gebracht.
* U moet alle virtuele netwerken in andere geopolitieke regio's ontkoppelen. Als u dit niet doet, worden uw aanvraag bijwerken mislukt en u de premietarieven in rekening gebracht.
* De routetabel moet minder dan 4000 routes voor persoonlijke peering. Als uw tabel route groter dan 4000 routes is, wordt de BGP-sessie wordt verwijderen en pas het aantal geadverteerde voorvoegsels daaronder 4.000 won't worden ingeschakeld.

#### <a name="disable-the-premium-add-on"></a>De premium-invoegtoepassing uitschakelen
U kunt de invoegtoepassing ExpressRoute premium voor uw bestaande circuit uitschakelen met behulp van de volgende PowerShell-cmdlet:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Bijwerken van de bandbreedte van het ExpressRoute-circuit
Controleer de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor ondersteunde bandbreedte-opties voor uw provider. U kunt de grootte die groter is dan de grootte van uw bestaande circuit, zolang de fysieke poort (waarop uw circuit is gemaakt) kunt verzamelen.

> [!IMPORTANT]
> U moet de ExpressRoute-circuit opnieuw maken als er onvoldoende capaciteit op de bestaande poort. U kunt het circuit niet upgraden als er geen extra capaciteit beschikbaar is op die locatie.
>
> U kunt de bandbreedte van een ExpressRoute-circuit zonder onderbreking niet reduceren. Downgraden bandbreedte, moet u het ExpressRoute-circuit inrichting ervan ongedaan en vervolgens opnieuw inrichten van nieuwe ExpressRoute-circuit.
> 
> 

#### <a name="resize-a-circuit"></a>Een circuit vergroten of verkleinen

Nadat u welke grootte die u nodig hebt besluit, kunt u de volgende opdracht om het formaat van uw circuit te gebruiken:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Uw circuit wordt is grootte van de Microsoft-zijde. U moet contact opnemen met uw connectiviteitsprovider om bij te werken-configuraties van hun kant overeenkomen met deze wijziging. Houd er rekening mee dat we u de optie bijgewerkte bandbreedte vanaf dit punt facturering start op.

Als u de volgende fout ziet wanneer de bandbreedte van het circuit vergroten, betekent dit er is geen voldoende bandbreedte beschikbaar is op de fysieke poort waarop uw bestaande circuit is gemaakt. U moet dit circuit verwijdert en maakt u een nieuw circuit van de grootte die u nodig hebt. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Opheffen van inrichting en een ExpressRoute-circuit verwijderen

### <a name="considerations"></a>Overwegingen

* U moet alle virtuele netwerken van het ExpressRoute-circuit voor deze bewerking niet ontkoppelen. Controleer of er geen virtuele netwerken die zijn gekoppeld aan het circuit als deze bewerking is mislukt.
* Als de ExpressRoute-circuit serviceprovider Inrichtingsstatus **inrichten** of **ingericht** moet u werken met uw serviceprovider voor inrichting ervan ongedaan maakt het circuit op hun kant. We blijven resources reserveren en u in rekening brengen totdat de serviceprovider is voltooid opheffen van inrichting het circuit en waarschuwt ons.
* Als de provider heeft het circuit deprovisioned (de serviceprovider Inrichtingsstatus is ingesteld op **niet ingericht**) kunt u het circuit verwijderen. Hiermee stopt u facturering voor het circuit.

#### <a name="delete-a-circuit"></a>Een circuit verwijderen

U kunt uw ExpressRoute-circuit verwijderen door de volgende opdracht uit te voeren:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Volgende stappen
Nadat u het circuit hebt gemaakt, zorg er dan voor dat u het volgende doen:

* [Maken en aanpassen van routering voor ExpressRoute-circuit](expressroute-howto-routing-classic.md)
* [Het virtuele netwerk koppelen aan uw ExpressRoute-circuit](expressroute-howto-linkvnet-classic.md)

