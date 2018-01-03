---
title: 'Wijzigen van een ExpressRoute-circuit: PowerShell: klassieke Azure Portal | Microsoft Docs'
description: Dit artikel begeleidt u bij de stappen voor het controleren van de status, bijwerken of verwijderen en uw ExpressRoute-circuit classic deployment model inrichting ervan ongedaan.
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
ms.date: 11/08/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 457bb74fa15d31fecbf668038ac880cafb8a897d
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="modify-an-expressroute-circuit-using-powershell-classic"></a>Wijzigen van een ExpressRoute-circuit met behulp van PowerShell (klassiek)

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure-CLI](howto-circuit-cli.md)
> * [Video - Azure-portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-circuit-classic.md)
>

Dit artikel ziet u ook het controleren van de status, update of delete en een ExpressRoute-circuit inrichting ervan ongedaan maakt.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Over Azure-implementatiemodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Voordat u begint

Installeer de nieuwste versies van de Azure Service Management (SM) PowerShell-modules Volg de instructies in [aan de slag met Azure PowerShell-cmdlets](/powershell/azure/overview) voor stapsgewijze instructies voor het configureren van uw computer te gebruiken de Azure PowerShell-modules.

## <a name="get-the-status-of-a-circuit"></a>De status van een circuit ophalen

U kunt deze informatie op elk gewenst moment ophalen met behulp van de `Get-AzureCircuit` cmdlet. De oproep zonder parameters voor een lijst met alle circuits.

```powershell
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
```

U kunt de informatie op een specifieke ExpressRoute-circuit opvragen door de servicesleutel wordt doorgegeven als parameter voor de aanroep.

```powershell
Get-AzureDedicatedCircuit -ServiceKey "*********************************"

Bandwidth                        : 200
CircuitName                      : MyTestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Standard
Status                           : Enabled
```

U kunt gedetailleerde beschrijvingen van alle parameters krijgen door het uitvoeren van het volgende voorbeeld:

```powershell
get-help get-azurededicatedcircuit -detailed
```

## <a name="modify-a-circuit"></a>Een circuit wijzigen

U kunt bepaalde eigenschappen van een ExpressRoute-circuit wijzigen zonder enige impact op connectiviteit.

U kunt de volgende taken zonder uitvaltijd doen:

* In- of uitschakelen van een ExpressRoute premium-invoegtoepassing voor ExpressRoute-circuit.
* De bandbreedte van uw ExpressRoute-circuit vergroten, mits er capaciteit beschikbaar is op de poort. De bandbreedte van een circuit downgraden wordt niet ondersteund. 
* Wijzig het softwarelicentiecontrole plan van Datalimiet gegevens in onbeperkte gegevens. Het plan softwarelicentiecontrole wijzigen van onbeperkt naar Datalimiet gegevens wordt niet ondersteund.
* U kunt inschakelen en uitschakelen *klassieke bewerkingen toestaan*.

Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor meer informatie over limieten en beperkingen.

### <a name="enable-the-expressroute-premium-add-on"></a>De invoegtoepassing ExpressRoute premium inschakelen

U kunt de invoegtoepassing ExpressRoute premium inschakelen voor uw bestaande circuit met de volgende PowerShell-cmdlet:

```powershell
Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

Bandwidth                        : 1000
CircuitName                      : TestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Premium
Status                           : Enabled
```

Het circuit hebt nu de ExpressRoute premium-invoegtoepassing voor functies ingeschakeld. Zodra de opdracht is uitgevoerd, begint de facturering voor de mogelijkheid van premium-invoegtoepassing.

### <a name="disable-the-expressroute-premium-add-on"></a>De invoegtoepassing ExpressRoute premium uitschakelen

> [!IMPORTANT]
> Deze bewerking kan mislukken als u resources die groter zijn dan wat voor het standaard circuit is toegestaan.
> 
> 

#### <a name="considerations"></a>Overwegingen

* Zorg ervoor dat het aantal virtuele netwerken die zijn gekoppeld aan het circuit minder dan 10 is voordat u een van premium op standaard downgrade. Als u dit niet doet, de updateaanvraag mislukt en u wordt gefactureerd de premietarieven.
* U moet alle virtuele netwerken in andere geopolitieke regio's ontkoppelen. Als u dit niet, uw aanvraag bijwerken mislukt en u wordt gefactureerd de premietarieven.
* De routetabel moet minder dan 4000 routes voor persoonlijke peering. Als uw tabel route groter dan 4000 routes is, wordt de BGP-sessie zakt en pas het aantal geadverteerde voorvoegsels daaronder 4.000 won't worden ingeschakeld.

#### <a name="to-disable-the-premium-add-on"></a>De premium-invoegtoepassing uitschakelen

U kunt de invoegtoepassing ExpressRoute premium voor uw bestaande circuit uitschakelen met behulp van de volgende PowerShell-cmdlet:

```powershell

Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

Bandwidth                        : 1000
CircuitName                      : TestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Standard
Status                           : Enabled
```

### <a name="update-the-expressroute-circuit-bandwidth"></a>Bijwerken van de bandbreedte van het ExpressRoute-circuit

Controleer de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor ondersteunde bandbreedte-opties voor uw provider. U kunt de grootte die groter is dan de grootte van uw bestaande circuit, zolang de fysieke poort (waarop uw circuit is gemaakt) kunt verzamelen.

> [!IMPORTANT]
> U moet de ExpressRoute-circuit opnieuw maken als er onvoldoende capaciteit op de bestaande poort. U kunt het circuit niet upgraden als er geen extra capaciteit beschikbaar is op die locatie.
>
> U kunt de bandbreedte van een ExpressRoute-circuit zonder onderbreking niet reduceren. Downgraden bandbreedte, moet u het ExpressRoute-circuit inrichting ervan ongedaan en vervolgens opnieuw inrichten van nieuwe ExpressRoute-circuit.
> 
> 

#### <a name="resize-a-circuit"></a>Een circuit vergroten of verkleinen

Nadat u welke grootte die u nodig hebt besluit, kunt u de volgende opdracht om het formaat van uw circuit te gebruiken:

```powershell
Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

Bandwidth                        : 1000
CircuitName                      : TestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Standard
Status                           : Enabled
```

Zodra uw circuit, is in de aan de kant van Microsoft is aangepast, moet u contact op met uw connectiviteitsprovider om bij te werken-configuraties van hun kant overeenkomen met deze wijziging. Facturering begint op voor de optie bijgewerkte bandbreedte vanaf dit punt.

Als u de volgende fout ziet wanneer de bandbreedte van het circuit vergroten, betekent dit er is geen voldoende bandbreedte beschikbaar is op de fysieke poort waarop uw bestaande circuit is gemaakt. U moet dit circuit verwijderen en een nieuwe circuit van de grootte die u moet maken.

```powershell
Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
update operation
At line:1 char:1
+ Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
  + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
```

## <a name="deprovision-and-delete-a-circuit"></a>Inrichting ervan ongedaan maakt en een circuit verwijderen

### <a name="considerations"></a>Overwegingen

* U moet alle virtuele netwerken van het ExpressRoute-circuit voor deze bewerking niet ontkoppelen. Controleer of er geen virtuele netwerken die zijn gekoppeld aan het circuit als deze bewerking is mislukt.
* Als de ExpressRoute-circuit serviceprovider Inrichtingsstatus **inrichten** of **ingericht** moet u werken met uw serviceprovider voor inrichting ervan ongedaan maakt het circuit op hun kant. We blijven resources reserveren en u in rekening brengen totdat de serviceprovider is voltooid opheffen van inrichting het circuit en waarschuwt ons.
* Als de provider heeft het circuit deprovisioned (de serviceprovider Inrichtingsstatus is ingesteld op **niet ingericht**), kunt u het circuit verwijderen. Hierdoor wordt voorkomen dat facturering voor het circuit.

#### <a name="delete-a-circuit"></a>Een circuit verwijderen

U kunt uw ExpressRoute-circuit verwijderen door de volgende opdracht uit te voeren:

```powershell
Remove-AzureDedicatedCircuit -ServiceKey "*********************************"
```