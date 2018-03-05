---
title: 'Een virtueel netwerk koppelen aan een ExpressRoute-circuit: Azure portal | Microsoft Docs'
description: Verbinding maken met een VNet een Azure ExpressRoute-Circuit. Stappen.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f5cb5441-2fba-46d9-99a5-d1d586e7bda4
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2018
ms.author: cherylmc
ms.openlocfilehash: 95b732229f151b8f27dce1dcc3825d9aa2e1d1ed
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-the-portal"></a>Een virtueel netwerk verbinden met een ExpressRoute-circuit met de portal
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure-CLI](howto-linkvnet-cli.md)
> * [Video - Azure-portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-linkvnet-classic.md)
> 

In dit artikel helpt u bij het maken van een verbinding voor een virtueel netwerk koppelen aan een Azure ExpressRoute-circuit met de Azure portal. De virtuele netwerken die u verbinding met uw Azure ExpressRoute-circuit maken kunnen in hetzelfde abonnement, of ze deel kunnen uitmaken van een ander abonnement.

## <a name="before-you-begin"></a>Voordat u begint

* Controleer de [vereisten](expressroute-prerequisites.md), [routeringsvereisten](expressroute-routing.md), en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.
* U moet een actief ExpressRoute-circuit hebben.

  * Volg de instructies voor [maken van een ExpressRoute-circuit](expressroute-howto-circuit-portal-resource-manager.md) en laat het circuit inschakelen door de connectiviteitsprovider.
  * Zorg ervoor dat u persoonlijke Azure-peering voor uw circuit is geconfigureerd. Zie de [Configure routing](expressroute-howto-routing-portal-resource-manager.md) artikel voor routering instructies.
  * Zorg ervoor dat de persoonlijke Azure-peering is geconfigureerd en van de BGP-peering tussen uw netwerk en Microsoft is, zodat u kunt end-to-end-connectiviteit inschakelen.
  * Zorg ervoor dat u hebt een virtueel netwerk en een virtuele netwerkgateway gemaakt en volledig is ingericht. Volg de instructies voor [maken van een virtuele netwerkgateway voor ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). Het GatewayType 'ExpressRoute' niet VPN maakt gebruik van een virtuele netwerkgateway voor ExpressRoute.

* U kunt maximaal 10 virtuele netwerken koppelen aan een standaard ExpressRoute-circuit. Alle virtuele netwerken moet in dezelfde geopolitieke regio bij gebruik van een standaard ExpressRoute-circuit. 
* U kunt een virtueel netwerk buiten de geopolitieke regio van het ExpressRoute-circuit koppelen of verbinding maken met een groter aantal virtuele netwerken aan uw ExpressRoute-circuit als u de invoegtoepassing ExpressRoute premium ingeschakeld. Controleer de [Veelgestelde vragen over](expressroute-faqs.md) voor meer informatie over de premium-invoegtoepassing.
* U kunt [Bekijk een video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) voordat u begint met de stappen beter te begrijpen.

## <a name="connect-a-vnet-to-a-circuit---same-subscription"></a>Verbinding maken van een VNet met een circuit - hetzelfde abonnement

> [!NOTE]
> BGP-configuratie-informatie wordt niet weergegeven als de layer 3-provider uw peerings geconfigureerd. Als uw circuit ingericht status heeft is, moet u mogelijk zijn om verbindingen te maken.
>

### <a name="to-create-a-connection"></a>Een verbinding te maken

1. Zorg ervoor dat uw ExpressRoute-circuit en de persoonlijke Azure-peering correct is geconfigureerd. Volg de instructies in [maken van een ExpressRoute-circuit](expressroute-howto-circuit-arm.md) en [Configure routing](expressroute-howto-routing-arm.md). Uw ExpressRoute-circuit moet eruitzien als in de volgende afbeelding:

  ![ExpressRoute-circuit-schermafbeelding](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
2. U kunt nu starten voor het inrichten van een verbinding voor het koppelen van uw virtuele netwerkgateway aan uw ExpressRoute-circuit. Klik op **verbinding** > **toevoegen** openen de **verbinding toevoegen** pagina en configureer vervolgens de waarden.

  ![Schermafbeelding van de verbinding toevoegen](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)
3. Nadat de verbinding is geconfigureerd, kan uw verbindingsobject gegevens voor de verbinding wordt weergegeven.

  ![Schermafbeelding van de verbinding-object](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

## <a name="connect-a-vnet-to-a-circuit---different-subscription"></a>Verbinding maken van een VNet met een circuit - ander abonnement

U kunt een ExpressRoute-circuit delen tussen meerdere abonnementen. De onderstaande afbeelding ziet u een eenvoudige schematische van hoe delen werkt voor ExpressRoute-circuits voor meerdere abonnementen.

![Abonnementoverschrijdende connectiviteit](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- Elk van de kleinere clouds binnen de grote cloud wordt gebruikt voor abonnementen die bij verschillende afdelingen binnen een organisatie horen te vertegenwoordigen.
- Elk van de afdelingen binnen de organisatie kan hun eigen abonnement gebruiken voor het implementeren van hun services, maar één ExpressRoute-circuit terugverbinding maken met uw on-premises netwerk kunnen delen.
- Één afdeling (in dit voorbeeld: IT) kunt eigenaar van het ExpressRoute-circuit. Andere abonnementen binnen de organisatie kunnen gebruiken voor de ExpressRoute-circuit en de machtigingen die worden gekoppeld aan het circuit, met inbegrip van abonnementen die zijn gekoppeld aan andere Azure Active Directory-tenants en de Enterprise Agreement inschrijvingen.

  > [!NOTE]
  > Verbindingen en bandbreedte kosten voor het specifieke circuit wordt toegepast op de eigenaar van het ExpressRoute-circuit. Alle virtuele netwerken delen de dezelfde bandbreedte.
  >
  >

### <a name="administration---about-circuit-owners-and-circuit-users"></a>Beheer - over circuit eigenaars en circuit gebruikers

De circuiteigenaar is een geautoriseerde gebruiker Power van de bron van ExpressRoute-circuit. De circuiteigenaar van het kunt autorisaties die kunnen worden ingewisseld door 'circuit gebruikers' maken. Circuit gebruikers kunnen eigenaren van virtuele netwerkgateways die zich niet binnen hetzelfde abonnement als het ExpressRoute-circuit. Circuit gebruikers kunnen inwisselen autorisaties (één autorisatie per virtueel netwerk).

De circuiteigenaar van het bevoegd is om te wijzigen en machtigingen intrekt op elk gewenst moment. Intrekken van een vergunning resulteert in alle koppeling verbindingen wordt verwijderd uit het abonnement waarvoor de toegang is ingetrokken.

### <a name="circuit-owner-operations"></a>Circuit eigenaar bewerkingen

**Maken van een verbinding-autorisatieregels**

De circuiteigenaar van het maakt een autorisatie. Dit resulteert in het maken van een autorisatiesleutel die kan worden gebruikt door een gebruiker circuit verbinding maken hun virtuele netwerkgateways aan ExpressRoute-circuit. Een vergunning is geldig voor slechts één verbinding.

1. Klik op de pagina ExpressRoute **autorisaties** en typ vervolgens een **naam** voor autorisatie en klik op **opslaan**.

  ![Autorisaties](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)
2. Zodra de configuratie is opgeslagen, kopieert u de **Resource-ID** en de **Autorisatiesleutel**.

  ![Autorisatiesleutel](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

**De autorisatie van een verbinding verwijderen**

U kunt een verbinding verwijderen door het selecteren van de **verwijderen** pictogram op de pagina voor de verbinding.

### <a name="circuit-user-operations"></a>Bewerkingen voor circuit-gebruikers

De circuit-gebruiker moet de resource-ID en een autorisatiesleutel van de circuiteigenaar van het.

**Voor een verbindingsverificatie inwisselen**

1. Klik op de **+ nieuw** knop.

  ![Klik op nieuwe](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)
2. Zoeken naar **'Verbinding'** in de Marketplace te selecteren en op **maken**.

  ![Zoekactie voor verbinding](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)
3. Zorg ervoor dat de **verbindingstype** is ingesteld op 'ExpressRoute'.
4. Vul de details en klik vervolgens op **OK** op de pagina basisbeginselen.

  ![Basisprincipes van pagina](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)
5. In de **instellingen** pagina, selecteer de **virtuele netwerkgateway** en controleer de **inwisselen autorisatie** selectievakje.
6. Voer de **autorisatiesleutel** en de **circuit URI Peer** en geef een naam op voor de verbinding. Klik op **OK**.

  ![De pagina Instellingen](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)
7. Lees de informatie in de **samenvatting** pagina en klik op **OK**.

**Een verbindingsverificatie vrijgeven**

U kunt een vergunning vrijgeven door het verwijderen van de verbinding die is gekoppeld aan het virtuele netwerk in het ExpressRoute-circuit.

## <a name="delete-a-connection-to-unlink-a-vnet"></a>Verwijderen van een verbinding voor het ontkoppelen van een VNet

U kunt een verbinding verwijderen en uw VNet-naar-een ExpressRoute-circuit ontkoppelen door het selecteren van de **verwijderen** pictogram op de pagina voor de verbinding.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).