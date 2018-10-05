---
title: Azure Virtual WAN gebruiken om ExpressRoute-verbindingen met Azure en on-premises omgevingen te maken | Microsoft Docs
description: In deze zelfstudie leert u hoe u Azure Virtual WAN gebruikt om ExpressRoute-verbindingen met Azure en on-premises omgevingen te maken.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 09/26/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my corporoate on-premises network(s) to my VNets using Virtual WAN and ExpressRoute.
ms.openlocfilehash: 0b8de4d04d9cca47423634164e458e8699154f30
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405305"
---
# <a name="tutorial-create-an-expressroute-association-using-azure-virtual-wan-preview"></a>Zelfstudie: Een ExpressRoute-koppeling maken met behulp van Azure Virtual WAN (preview)

In deze zelfstudie leert u hoe u Azure Virtual WAN gebruikt om uw resources in Azure te verbinden met behulp van een ExpressRoute-circuit en -koppeling. Zie voor meer informatie over Virtual WAN het [Overzicht van Virtual WAN](virtual-wan-about.md)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een vWAN maken
> * Een hub maken
> * Een circuit zoeken en aan de hub koppelen
> * Het circuit aan hub(s) koppelen
> * Een VNet verbinden met een hub
> * Uw virtuele WAN weergeven
> * Resourcestatus weergeven
> * Een verbinding bewaken

> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="register"></a>Deze functie registreren

Klik op **Proberen** om deze functie eenvoudig te registreren met Azure Cloud Shell.

>[!NOTE]
>Als u deze functie niet registreert, kunt u de functie niet gebruiken en wordt deze ook niet weergegeven in de portal.
>
>

Wanneer u op **Proberen** hebt geklikt om Azure Cloud Shell te openen, kopieert en plakt u de volgende opdrachten:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowCortexExpressRouteGateway
```
 
```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowCortexExpressRouteGateway
```

Zodra wordt weergegeven dat de functie is geregistreerd, registreert u het abonnement opnieuw bij de naamruimte Microsoft.Network.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="vnet"></a>1. Een virtueel netwerk maken

[!INCLUDE [Create a virtual network](../../includes/virtual-wan-tutorial-vnet-include.md)]

## <a name="openvwan"></a>2. Een virtueel WAN maken

Open een browser, ga naar de [Azure-portal (preview)](http://aka.ms/azurevirtualwanpreviewfeatures) en meld u aan met uw Azure-account.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

### <a name="getting-started-page"></a>Pagina Aan de slag

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-gettingstarted-include.md)]

## <a name="hub"></a>3. Een hub maken

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="hub"></a>4. Een circuit zoeken en aan de hub koppelen

1. Selecteer uw vWAN en selecteer onder **Virtual WAN-architectuur** de optie **ExpressRoute-circuits**
2. Als het ExpressRoute-circuit zich in hetzelfde abonnement bevindt als uw vWAN, klikt u op **ExpressRoute-circuit selecteren** vanuit uw abonnement(en) 
3. Selecteer met de vervolgkeuzelijst de ExpressRoute die u aan de hub wilt koppelen.
4. Als het ExpressRoute-circuit zich niet in hetzelfde abonnement bevindt of u [een autorisatiesleutel en peer-id](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md) hebt opgegeven, selecteert u **Een circuit zoeken waarvoor een autorisatiesleutel moet worden ingewisseld**
5. Voer de volgende details in:
* **Autorisatiesleutel**: gegenereerd door de circuiteigenaar zoals hierboven beschreven
* **URI van peercircuit**: circuit-URI die wordt geleverd door de circuiteigenaar en die de unieke id voor het circuit is
* **Routeringsgewicht** - [Routeringsgewicht](../expressroute/expressroute-optimize-routing.md) biedt u de mogelijkheid voorkeur te geven aan bepaalde paden wanneer meerdere circuits van verschillende peeringlocaties aan dezelfde hub zijn verbonden
6. Klik op **Circuit zoeken** en selecteer het circuit, indien gevonden
7. Selecteer een of meer hubs in de vervolgkeuzelijst en klik op **Opslaan**

## <a name="vnet"></a>5. Uw VNet verbinden met een hub

In deze stap brengt u de peering-verbinding tussen uw hub en een VNet tot stand. Herhaal deze stappen voor elk VNet dat u wilt verbinden.

1. Klik op de pagina voor uw virtuele WAN op **Virtuele netwerkverbinding**.
2. Klik op de pagina Virtuele netwerkverbinding op **+Verbinding toevoegen**.
3. Vul de volgende velden in op de pagina **Verbinding toevoegen**:

    * **Verbindingsnaam** - voer een naam in voor uw verbinding.
    * **Hubs** - selecteer de hub die u wilt koppelen aan deze verbinding.
    * **Abonnement** - controleer of het abonnement klopt.
    * **Virtueel netwerk** - selecteer het virtuele netwerk dat met deze hub wilt verbinden. Het virtuele netwerk mag geen bestaande virtuele netwerkgateway hebben.


## <a name="viewwan"></a>6. Uw virtuele WAN weergeven

1. Navigeer naar uw virtuele WAN.
2. Op de pagina Overzicht vertegenwoordigt elk punt op de kaart een hub. Houd de muisaanwijzer boven een punt om de statussamenvatting voor de hub weer te geven.
3. In de sectie Hubs en verbindingen vindt u informatie over de hubstatus, site, regio, VPN-verbindingsstatus en verzonden en ontvangen bytes.

## <a name="viewhealth"></a>7. Resourcestatus bekijken

1. Navigeer naar uw WAN.
2. Klik op de pagina van uw WAN, in de sectie **Ondersteuning en probleemoplossing**, op **Status** en bekijk de status van uw resource.

## <a name="connectmon"></a>8. Een verbinding bewaken

Maak een verbinding om de communicatie tussen een Azure-VM en een externe site te bewaken. Zie voor meer informatie over het instellen van een verbindingscontrole de pagina [Netwerkcommunicatie bewaken](~/articles/network-watcher/connection-monitor.md). Het bronveld is het IP-adres van de VM in Azure en het doel-IP-adres is het IP-adres voor de site.

## <a name="cleanup"></a>9. Resources opschonen

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen, wanneer u deze niet meer nodig hebt. Vervangen 'myResourceGroup' door de naam van uw resourcegroep en voer de volgende PowerShell-opdracht uit:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een vWAN maken
> * Een hub maken
> * Een circuit zoeken en aan de hub koppelen
> * Het circuit aan hub(s) koppelen
> * Een VNet verbinden met een hub
> * Uw virtuele WAN weergeven
> * Resourcestatus weergeven
> * Een verbinding bewaken

Zie voor meer informatie over Virtual WAN de pagina [Overzicht van Virtual WAN](virtual-wan-about.md).