---
title: Een virtueel netwerk met een site-naar-site-VPN-gatewayverbinding maken met behulp van de klassieke Azure-portal | Microsoft Docs
description: Maak een VNet met een S2S-VPN-gatewayverbinding voor cross-premises en hybride configuraties met behulp van het klassieke implementatiemodel.
services: vpn-gateway
documentationcenter: 
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 024ecb29-64de-4ff1-84f1-1a45a8595f0b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/14/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: d269d9a76ff4ccd973eee70d2d5b54a7262383ef
ms.openlocfilehash: b45801d4e6deb25ca8c6af5c3b46de741d4993e0


---
# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-classic-portal"></a>Een VNet met een site-naar-site-verbinding maken met de klassieke Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Klassiek - Klassieke portal](vpn-gateway-site-to-site-create.md)
> 
> 

In dit artikel wordt stapsgewijs beschreven hoe u een virtueel netwerk en een site-naar-site-VPN-gatewayverbinding met uw on-premises netwerk maakt met behulp van het klassieke implementatiemodel en de klassieke portal. Site-naar-site-verbindingen kunnen worden gebruikt voor cross-premises en hybride configuraties.

![Site-naar-site-diagram](./media/vpn-gateway-site-to-site-create/site2site.png "site-to-site")

### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Implementatiemodellen en -methoden voor site-naar-site-verbindingen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

In de volgende tabel staan de momenteel beschikbare implementatiemodellen en -methoden voor site-naar-site-configuraties. Als er een artikel met configuratiestappen beschikbaar is, kunt u dit via een rechtstreekse koppeling in deze tabel raadplegen.

[!INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Aanvullende configuraties
Raadpleeg [Configure a VNet-to-VNet connection for the classic deployment model (Een VNet-naar-VNet verbinding voor het klassieke implementatiemodel configureren)](virtual-networks-configure-vnet-to-vnet-connection.md), als u VNets met elkaar wilt verbinden. Zie [Een S2S-verbinding toevoegen aan een VNet met een bestaande VPN-gatewayverbinding](vpn-gateway-multi-site.md) als u een site-naar-site-verbinding wilt toevoegen aan een VNet die al een verbinding heeft.

## <a name="before-you-begin"></a>Voordat u begint
Controleer of u beschikt over de volgende items voordat u begint met de configuratie.

* Een compatibel VPN-apparaat en iemand die dit kan configureren. Zie [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Over VPN-apparaten). Als u niet weet hoe u uw VPN-apparaat moet configureren of de IP-adresbereiken in uw on-premises netwerkconfiguratie niet kent, moet u contact opnemen met iemand die u hierbij kan helpen en de benodigde gegevens kan verstrekken.
* Een extern gericht openbaar IP-adres voor het VPN-apparaat. Dit IP-adres kan zich niet achter een NAT bevinden.
* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial).

## <a name="a-namecreatevnetacreate-your-virtual-network"></a><a name="CreateVNet"></a>Het virtuele netwerk maken
1. Meld u aan bij de [klassieke Azure Portal](https://manage.windowsazure.com/).
2. Klik linksonder in het scherm op **Nieuw**. Klik in het navigatievenster op **Netwerkservices** en vervolgens op **Virtueel netwerk**. Klik op **Aangepast maken** om de configuratiewizard te starten.
3. Voer uw configuratie-instellingen op de volgende pagina's in om uw VNet te maken:

## <a name="a-namedetailsavirtual-network-details-page"></a><a name="Details"></a>Pagina met details over virtueel netwerk
Voer de volgende informatie in:

* **Naam**: geef het virtuele netwerk een naam. Bijvoorbeeld *EastUSVNet*. U gebruikt deze naam voor het virtuele netwerk wanneer u de virtuele machines en PaaS-instanties implementeert. Maak de naam dus niet te ingewikkeld.
* **Locatie**: de locatie is direct gerelateerd aan de fysieke locatie (regio) waar u wilt dat de resources (VM's) zich bevinden. Selecteer bijvoorbeeld VS - oost als u wilt dat de virtuele machines die u in dit virtuele netwerk implementeert zich fysiek bevinden in *VS - oost*. U kunt de regio die aan het virtuele netwerk is gekoppeld, niet meer wijzigen wanneer het netwerk is gemaakt.

## <a name="a-namednsadns-servers-and-vpn-connectivity-page"></a><a name="DNS"></a>Pagina DNS-servers en VPN-verbinding
Voer de volgende informatie in en klik rechtsonder op de pijl Volgende.

* **DNS-servers**: geef de naam en het IP-adres van de DNS-server op of selecteer een eerder geregistreerde DNS-server in het snelmenu. Met deze instelling wordt geen DNS-server gemaakt. U kunt hiermee de DNS-servers opgeven die u wilt gebruiken voor naamomzetting voor dit virtuele netwerk.
* **Site-naar-site-VPN configureren**: schakel het selectievakje **Site-naar-site-VPN configureren** in.
* **Lokaal netwerk**: een lokaal netwerk vertegenwoordigt uw fysieke on-premises locatie. U kunt een lokaal netwerk selecteren dat u eerder hebt gemaakt of u kunt een nieuw lokaal netwerk maken. Als u echter kiest voor het gebruik van een lokaal netwerk dat u eerder hebt gemaakt, gaat u naar de configuratiepagina **Lokale netwerken** en controleert u of het IP-adres van het VPN-apparaat (openbaar IPv4-adres) voor het VPN-apparaat juist is.

## <a name="a-nameconnectivityasite-to-site-connectivity-page"></a><a name="Connectivity"></a>Pagina Site-naar-site-connectiviteit
Als u een nieuw lokaal netwerk maakt, wordt de pagina **Site-naar-site-connectiviteit** geopend. Als u een lokaal netwerk wilt gebruiken dat u eerder hebt gemaakt, wordt deze pagina niet in de wizard weergegeven en kunt u naar de volgende sectie gaan.

Voer de volgende informatie in en klik op de pijl Volgende.

* **Naam**: de naam voor de lokale (on-premises) netwerksite.
* **IP-adres van het VPN-apparaat**: het openbare IPv4-adres van het on-premises VPN-apparaat dat u gebruikt om verbinding te maken met Azure. Het VPN-apparaat kan zich niet achter een NAT bevinden.
* **Adresruimte**: voeg het beginadres voor het IP-bereik en de CIDR (aantal adressen) toe. U geeft de adresbereiken op die u via de gateway van het virtuele netwerk naar uw lokale on-premises locatie wilt laten verzenden. Als een IP-doeladres binnen de bereiken valt die u hier opgeeft, wordt het verkeer via de gateway van het virtuele netwerk gerouteerd.
* **Adresruimte toevoegen**: als u meerdere-adresbereiken via de gateway van het virtuele netwerk wilt laten verzenden, geeft u de aanvullende adresbereiken op. U kunt later op de pagina **Lokaal netwerk** bereiken toevoegen of verwijderen.

## <a name="a-nameaddressavirtual-network-address-spaces-page"></a><a name="Address"></a>Pagina Adresruimten voor virtueel netwerk
Geef het adresbereik op dat u voor het virtuele netwerk wilt gebruiken. Dit zijn de dynamische IP-adressen (DIPS) die worden toegewezen aan de virtuele machines en andere rolinstanties die u in dit virtuele netwerk implementeert.

Het is vooral van belang dat u een bereik selecteert dat niet overlapt met een van de bereiken die voor uw on-premises netwerk worden gebruikt. Neem hiervoor contact op met uw netwerkbeheerder. De netwerkbeheerder moet mogelijk een bereik van IP-adressen in de adresruimte van uw on-premises netwerk reserveren voor het virtuele netwerk.

Voer de volgende informatie in en klik rechtsonder op het vinkje om het netwerk te configureren.

* **Adresruimte**: neem het IP-beginadres en het Aantal adressen op. Controleer of de adresruimten die u opgeeft niet overlappen met adresruimten in uw on-premises netwerk.
* **Subnet toevoegen**: neem het IP-beginadres en het Aantal adressen op. U hebt geen aanvullende subnetten nodig, maar misschien wilt u een apart subnet maken voor virtuele machines die statische DIPS krijgen. Of misschien wilt u uw virtuele machines in een subnet plaatsen dat is gescheiden van uw andere rolinstanties.
* **Gatewaysubnet toevoegen**: klik om het gatewaysubnet toe te voegen. Het gatewaysubnet wordt alleen gebruikt voor de gateway van het virtuele netwerk. Het is vereist voor deze configuratie.

Wanneer u onder aan de pagina op het vinkje klikt, wordt het virtuele netwerk gemaakt. Als dit is voltooid, wordt op de pagina **Netwerken** in de klassieke Azure Portal **Gemaakt** vermeld onder **Status**. Wanneer het VNet is gemaakt, kunt u de gateway van het virtuele netwerk configureren.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="a-namevnetgatewayaconfigure-your-virtual-network-gateway"></a><a name="VNetGateway"></a>De gateway van het virtuele netwerk configureren
Configureer de gateway van het virtuele netwerk om een beveiligde site-naar-site-verbinding te maken. Zie [Configure a virtual network gateway in the Azure classic portal](vpn-gateway-configure-vpn-gateway-mp.md) (De gateway van een virtueel netwerk configureren in de klassieke Azure Portal).

## <a name="next-steps"></a>Volgende stappen
 Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) voor meer informatie.




<!--HONumber=Nov16_HO4-->


