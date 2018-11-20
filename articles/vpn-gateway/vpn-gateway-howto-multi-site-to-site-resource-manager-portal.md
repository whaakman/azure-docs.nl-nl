---
title: 'Meerdere VPN-gateway-Site-naar-Site-verbindingen toevoegen aan een VNet: Azure Portal: Resource Manager | Microsoft Docs'
description: S2S-verbindingen van meerdere sites toevoegen aan een VPN-gateway met een bestaande verbinding
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.assetid: f3e8b165-f20a-42ab-afbb-bf60974bb4b1
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: cherylmc
ms.openlocfilehash: a0074cf703c5d30dca3c8353f4821e71684a190c
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52164554"
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Een Site-naar-Site-verbinding toevoegen aan een VNet met een bestaande VPN-gatewayverbinding

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klassiek)](vpn-gateway-multi-site.md)
>
> 

Dit artikel helpt u Site-naar-Site (S2S)-verbindingen kunt toevoegen aan een VPN-gateway met een bestaande verbinding met behulp van de Azure-portal. Dit type verbinding wordt vaak aangeduid als 'multi-site'-configuratie. U kunt een S2S-verbinding toevoegen aan een VNet met een S2S-verbinding, punt-naar-Site-verbinding of VNet-naar-VNet-verbinding. Er zijn enkele beperkingen bij het toevoegen van verbindingen. Controleer de [voordat u begint met](#before) sectie in dit artikel om te controleren voordat u begint met uw configuratie. 

In dit artikel is van toepassing op Resource Manager VNets met een RouteBased VPN-gateway. Deze stappen niet van toepassing op ExpressRoute/Site-naar-Site-configuraties voor naast elkaar bestaande verbinding. Zie [ExpressRoute/S2S-verbindingen naast elkaar](../expressroute/expressroute-howto-coexist-resource-manager.md) voor informatie over gelijktijdige verbindingen.

### <a name="deployment-models-and-methods"></a>Implementatiemodellen en -methoden
[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Deze tabel wordt bijgewerkt naarmate nieuwe artikelen en aanvullende hulpprogramma's voor deze configuratie beschikbaar komen. Wanneer een artikel beschikbaar is, koppelen we dit rechtstreeks aan uit deze tabel.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="before"></a>Voordat u begint
Controleer of de volgende items:

* Niet maakt u een gelijktijdige ExpressRoute/S2S-verbinding.
* U hebt een virtueel netwerk dat is gemaakt met het implementatiemodel van Resource Manager met een bestaande verbinding.
* De virtuele netwerkgateway voor uw VNet is een op route gebaseerd. Als u een PolicyBased VPN-gateway hebt, moet u de virtuele netwerkgateway verwijderen en een nieuwe VPN-gateway als RouteBased maken.
* Geen van de adresbereiken overlappen voor het gebruik van de VNets waarmee dit VNet verbinding maakt.
* U hebt compatibel VPN-apparaat en iemand die dit kan configureren. Zie [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Over VPN-apparaten). Als u niet weet hoe u uw VPN-apparaat moet configureren of de IP-adresbereiken in uw on-premises netwerkconfiguratie niet kent, moet u contact opnemen met iemand die u hierbij kan helpen en de benodigde gegevens kan verstrekken.
* U hebt een extern gericht openbaar IP-adres voor uw VPN-apparaat. Dit IP-adres kan zich niet achter een NAT bevinden.

## <a name="part1"></a>Deel 1: een verbinding configureren
1. Navigeer via een browser naar de [Azure Portal](http://portal.azure.com) en log, indien nodig, in met uw Azure-account.
2. Klik op **alle resources** en zoek uw **virtuele netwerkgateway** uit de lijst met resources en klik erop.
3. Op de **gateway van virtueel netwerk** pagina, klikt u op **verbindingen**.
   
    ![Pagina Verbindingen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Pagina Verbindingen")<br>
4. Op de **verbindingen** pagina, klikt u op **+ toevoegen**.
   
    ![De knop verbinding toevoegen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "knop van de verbinding toevoegen")<br>
5. Op de **verbinding toevoegen** pagina, vul de volgende velden:
   
   * **Naam:** de naam die u wilt geven tot de site maakt u de verbinding aan.
   * **Verbindingstype:** Selecteer **Site-naar-site (IPsec)**.
     
     ![Pagina van de verbinding toevoegen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "pagina van de verbinding toevoegen")<br>

## <a name="part2"></a>Deel 2: een lokale netwerkgateway toevoegen
1. Klik op **lokale netwerkgateway** ***een lokale netwerkgateway kiezen***. Hiermee opent u de **lokale netwerkgateway kiezen** pagina.
   
    ![De lokale netwerkgateway kiezen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "lokale netwerkgateway kiezen")<br>
2. Klik op **nieuw** openen de **lokale netwerkgateway maken** pagina.
   
    ![Pagina lokale netwerkgateway maken](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "lokale netwerkgateway maken")<br>
3. Op de **lokale netwerkgateway maken** pagina, vul de volgende velden:
   
   * **Naam:** de naam die u wilt geven tot een resource van de lokale netwerkgateway.
   * **IP-adres:** het openbare IP-adres van de VPN-apparaat op de site die u wilt verbinden.
   * **Adresruimte:** de adresruimte die u wilt worden doorgestuurd naar de nieuwe lokale netwerksite.
4. Klik op **OK** op de **lokale netwerkgateway maken** pagina de wijzigingen op te slaan.

## <a name="part3"></a>Deel 3: de gedeelde sleutel toevoegen en maak de verbinding
1. Op de **verbinding toevoegen** pagina, voegt u de gedeelde sleutel die u wilt gebruiken om uw verbinding te maken. U kunt de gedeelde sleutel ophalen uit uw VPN-apparaat, of één hier en configureer vervolgens het VPN-apparaat voor het gebruik van dezelfde gedeelde sleutel. Het belangrijkste is dat de sleutels exact hetzelfde zijn.
   
    ![Gedeelde sleutel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Gedeelde sleutel")<br>
2. Aan de onderkant van de pagina, klikt u op **OK** om de verbinding te maken.

## <a name="part4"></a>Deel 4: de VPN-verbinding controleren


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a>Volgende stappen

Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie het [leertraject](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) voor virtuele machines voor meer informatie.
