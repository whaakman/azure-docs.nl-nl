---
title: 'Uw on-premises netwerk verbinden met een virtueel Azure-netwerk: site-naar-site-VPN: klassieke portal | Microsoft Docs'
description: Stappen voor het maken van een IPSec-verbinding van uw on-premises netwerk met een virtueel Azure-netwerk via het openbare internet. Deze stappen helpen u een cross-premises site-naar-site-VPN-gatewayverbinding te maken met de klassieke portal en het klassieke implementatiemodel.
services: vpn-gateway
documentationcenter: 
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 024ecb29-64de-4ff1-84f1-1a45a8595f0b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/24/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 63d0972f31c2b26b391d5ba019f3e415d8053511
ms.contentlocale: nl-nl
ms.lasthandoff: 06/17/2017


---
# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-classic-portal-classic"></a>Een VNet met een site-naar-site-verbinding maken met de klassieke portal (klassiek)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

In dit artikel leest u hoe u de klassieke portal gebruikt om een site-naar-site-VPN-gatewayverbinding te maken vanuit uw lokale netwerk naar het VNet. De stappen in dit artikel zijn van toepassing op het klassieke implementatiemodel. U kunt deze configuratie ook maken met een ander implementatiehulpprogramma of een ander implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure Portal (klassiek)](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klassieke portal (klassiek)](vpn-gateway-site-to-site-create.md)
> 
>

![Diagram: cross-premises site-naar-site-VPN-gatewayverbinding](./media/vpn-gateway-site-to-site-create/site-to-site-connection-diagram.png)


Een site-naar-site-VPN-gatewayverbinding wordt gebruikt om een on-premises netwerk via een IPsec-/IKE-VPN-tunnel (IKEv1 of IKEv2) te verbinden met een virtueel Azure-netwerk. Voor dit type verbinding moet er on-premises een VPN-apparaat aanwezig zijn waaraan een extern openbaar IP-adres is toegewezen. Zie [Overzicht van VPN Gateway](vpn-gateway-about-vpngateways.md) voor meer informatie over VPN-gateways.

#### <a name="additional-configurations"></a>Aanvullende configuraties

Raadpleeg [Configure a VNet-to-VNet connection for the classic deployment model (Een VNet-naar-VNet verbinding voor het klassieke implementatiemodel configureren)](virtual-networks-configure-vnet-to-vnet-connection.md), als u VNets met elkaar wilt verbinden. Zie [Een S2S-verbinding toevoegen aan een VNet met een bestaande VPN-gatewayverbinding](vpn-gateway-multi-site.md) als u een site-naar-site-verbinding wilt toevoegen aan een VNet die al een verbinding heeft.
## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Controleer of u beschikt over de volgende items voordat u begint met de configuratie:

* Een compatibel VPN-apparaat en iemand die dit kan configureren. Zie [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Over VPN-apparaten). Als u niet weet hoe u uw VPN-apparaat moet configureren of de IP-adresbereiken in uw on-premises netwerkconfiguratie niet kent, moet u contact opnemen met iemand die u hierbij kan helpen en de benodigde gegevens kan verstrekken.
* Een extern gericht openbaar IP-adres voor het VPN-apparaat. Dit IP-adres kan zich niet achter een NAT bevinden.
* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial).

## <a name="CreateVNet"></a>Het virtuele netwerk maken
1. Meld u aan bij de [klassieke Azure Portal](https://manage.windowsazure.com/).
2. Klik linksonder in het scherm op **Nieuw**. Klik in het navigatievenster op **Netwerkservices** en vervolgens op **Virtueel netwerk**. Klik op **Aangepast maken** om de configuratiewizard te starten.
3. Voer uw configuratie-instellingen op de volgende pagina's in om uw VNet te maken:

## <a name="Details"></a>Pagina met details over virtueel netwerk
Voer de volgende informatie in:

* **Naam**: geef het virtuele netwerk een naam. Bijvoorbeeld *EastUSVNet*. U gebruikt deze virtuele-netwerknaam wanneer u de virtuele machines en PaaS-instanties implementeert. Maak de naam dus niet te ingewikkeld.
* **Locatie**: de locatie is direct gerelateerd aan de fysieke locatie (regio) waar u wilt dat de resources (VM's) zich bevinden. Selecteer bijvoorbeeld VS - oost als u wilt dat de virtuele machines die u in dit virtuele netwerk implementeert zich fysiek bevinden in *VS - oost*. U kunt de regio die aan het virtuele netwerk is gekoppeld, niet meer wijzigen wanneer het netwerk is gemaakt.

## <a name="DNS"></a>Pagina DNS-servers en VPN-verbinding
Voer de volgende informatie in en klik rechtsonder op de pijl Volgende.

* **DNS-servers**: geef de naam en het IP-adres van de DNS-server op of selecteer een eerder geregistreerde DNS-server in het snelmenu. Met deze instelling wordt geen DNS-server gemaakt. U kunt hiermee de DNS-servers opgeven die u wilt gebruiken voor naamomzetting voor dit virtuele netwerk.
* **Site-naar-site-VPN configureren**: schakel het selectievakje **Site-naar-site-VPN configureren** in.
* **Lokaal netwerk**: een lokaal netwerk vertegenwoordigt uw fysieke on-premises locatie. U kunt een lokaal netwerk selecteren dat u eerder hebt gemaakt of u kunt een nieuw lokaal netwerk maken. Als u echter kiest voor het gebruik van een lokaal netwerk dat u eerder hebt gemaakt, gaat u naar de configuratiepagina **Lokale netwerken** en controleert u of het IP-adres van het VPN-apparaat (openbaar IPv4-adres) voor het VPN-apparaat juist is.

## <a name="Connectivity"></a>Pagina Site-naar-site-connectiviteit
Als u een nieuw lokaal netwerk maakt, wordt de pagina **Site-naar-site-connectiviteit** geopend. Als u een lokaal netwerk wilt gebruiken dat u eerder hebt gemaakt, wordt deze pagina niet in de wizard weergegeven en kunt u naar de volgende sectie gaan.

Voer de volgende informatie in en klik op de pijl Volgende.

* **Naam**: de naam voor de lokale (on-premises) netwerksite.
* **IP-adres van het VPN-apparaat**: het openbare IPv4-adres van het on-premises VPN-apparaat dat u gebruikt om verbinding te maken met Azure. Het VPN-apparaat kan zich niet achter een NAT bevinden.
* **Adresruimte**: voeg het beginadres voor het IP-bereik en de CIDR (aantal adressen) toe. U geeft de adresbereiken op die u via de gateway van het virtuele netwerk naar uw lokale on-premises locatie wilt laten verzenden. Als een IP-doeladres binnen de bereiken valt die u hier opgeeft, wordt het verkeer via de gateway van het virtuele netwerk gerouteerd.
* **Adresruimte toevoegen**: als u meerdere-adresbereiken via de gateway van het virtuele netwerk wilt laten verzenden, geeft u de aanvullende adresbereiken op. U kunt later op de pagina **Lokaal netwerk** bereiken toevoegen of verwijderen.

## <a name="Address"></a>Pagina Adresruimten voor virtueel netwerk
Geef het adresbereik op dat u voor het virtuele netwerk wilt gebruiken. Dit zijn de dynamische IP-adressen (DIPS) die worden toegewezen aan de virtuele machines en andere rolinstanties die u in dit virtuele netwerk implementeert.

Het is vooral van belang dat u een bereik selecteert dat niet overlapt met een van de bereiken die voor uw on-premises netwerk worden gebruikt. Neem hiervoor contact op met uw netwerkbeheerder. De netwerkbeheerder moet mogelijk een bereik van IP-adressen in de adresruimte van uw on-premises netwerk reserveren voor het virtuele netwerk.

Voer de volgende informatie in en klik rechtsonder op het vinkje om het netwerk te configureren.

* **Adresruimte**: neem het IP-beginadres en het Aantal adressen op. Controleer of de adresruimten die u opgeeft niet overlappen met adresruimten in uw on-premises netwerk.
* **Subnet toevoegen**: neem het IP-beginadres en het Aantal adressen op. U hebt geen aanvullende subnetten nodig, maar misschien wilt u een apart subnet maken voor virtuele machines die statische DIPS krijgen. Of misschien wilt u uw virtuele machines in een subnet plaatsen dat is gescheiden van uw andere rolinstanties.
* **Gatewaysubnet toevoegen**: klik om het gatewaysubnet toe te voegen. Het gatewaysubnet wordt alleen gebruikt voor de gateway van het virtuele netwerk. Het is vereist voor deze configuratie.

Klik onder aan de pagina op het vinkje om het virtuele netwerk te maken. Als dit is voltooid, wordt op de pagina **Netwerken** van de klassieke Azure-portal de **Status** **Gemaakt** weergegeven. Wanneer het VNet is gemaakt, kunt u de gateway van het virtuele netwerk configureren.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="VNetGateway"></a>De gateway van het virtuele netwerk configureren
Configureer de gateway van het virtuele netwerk om een beveiligde site-naar-site-verbinding te maken. Zie [Configure a virtual network gateway in the Azure classic portal](vpn-gateway-configure-vpn-gateway-mp.md) (De gateway van een virtueel netwerk configureren in de klassieke Azure Portal).

## <a name="next-steps"></a>Volgende stappen
 Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie.


