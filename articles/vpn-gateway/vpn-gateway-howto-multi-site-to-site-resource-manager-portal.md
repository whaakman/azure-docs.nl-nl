---
title: 'Meerdere VPN-gateway Site-naar-Site-verbindingen toevoegen aan een VNet: Azure-Portal: Resource Manager | Microsoft Docs'
description: S2S-verbindingen van meerdere sites toevoegen aan een VPN-gateway met een bestaande verbinding
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3e8b165-f20a-42ab-afbb-bf60974bb4b1
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: cherylmc
ms.openlocfilehash: 7ec57789ee76f4ec54e4f7b68ea75c19522f3d7c
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Een Site-naar-Site-verbinding toevoegen aan een VNet met een bestaande VPN-gateway-verbinding

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klassiek)](vpn-gateway-multi-site.md)
>
> 

Dit artikel begeleidt u bij de Site-naar-Site (S2S)-verbindingen toevoegen aan een VPN-gateway met een bestaande verbinding met behulp van de Azure-portal. Dit type verbinding is vaak een configuratie 'multi-site' genoemd. U kunt een S2S-verbinding toevoegen aan een VNet met een S2S-verbinding, een punt-naar-Site-verbinding of een VNet-naar-VNet-verbinding. Er zijn enkele beperkingen bij het toevoegen van verbindingen. Controleer de [voordat u begint met](#before) sectie in dit artikel om te controleren voordat u uw configuratie. 

In dit artikel is van toepassing op VNets gemaakt met het Resource Manager-implementatiemodel waarvoor een RouteBased VPN-gateway. Deze stappen zijn niet van toepassing op ExpressRoute/Site-naar-Site naast elkaar bestaande verbinding configuraties. Zie [ExpressRoute/S2S naast elkaar bestaande verbindingen](../expressroute/expressroute-howto-coexist-resource-manager.md) voor informatie over naast elkaar bestaande verbindingen.

### <a name="deployment-models-and-methods"></a>Implementatiemodellen en -methoden
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Deze tabel wordt bijgewerkt als er nieuwe artikelen en aanvullende hulpprogramma's beschikbaar zijn voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we dit rechtstreeks aan uit deze tabel.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="before"></a>Voordat u begint
Controleer of de volgende items:

* Geen maakt u een ExpressRoute/S2S naast elkaar bestaande verbinding.
* U hebt een virtueel netwerk dat is gemaakt met het implementatiemodel van Resource Manager met een bestaande verbinding.
* De virtuele netwerkgateway voor uw VNet is RouteBased. Als u een PolicyBased VPN-gateway hebt, moet u de virtuele netwerkgateway verwijderen en maak een nieuwe VPN-gateway als RouteBased.
* Geen van de adresbereiken voor een van de VNets die dit VNet is verbonden met niet overlappen.
* U hebt compatibel VPN-apparaat en iemand die dit kan configureren. Zie [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Over VPN-apparaten). Als u niet weet hoe u uw VPN-apparaat moet configureren of de IP-adresbereiken in uw on-premises netwerkconfiguratie niet kent, moet u contact opnemen met iemand die u hierbij kan helpen en de benodigde gegevens kan verstrekken.
* U hebt een extern gericht openbaar IP-adres voor uw VPN-apparaat. Dit IP-adres kan zich niet achter een NAT bevinden.

## <a name="part1"></a>Deel 1: een verbinding configureren
1. Navigeer via een browser naar de [Azure Portal](http://portal.azure.com) en log, indien nodig, in met uw Azure-account.
2. Klik op **alle resources** en zoek uw **virtuele netwerkgateway** uit de lijst met resources en klik erop.
3. Op de **virtuele netwerkgateway** blade, klikt u op **verbindingen**.
   
    ![Blade Verbindingen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Blade Verbindingen")<br>
4. Op de **verbindingen** blade, klikt u op **+ toevoegen**.
   
    ![De knop verbinding toevoegen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "de knop verbinding toevoegen")<br>
5. Op de **verbinding toevoegen** blade Vul de volgende velden:
   
   * **Naam:** de naam die u wilt geven tot de site maakt u de verbinding aan.
   * **Verbindingstype:** Selecteer **Site-naar-site (IPsec)**.
     
     ![Tabblad van de verbinding toevoegen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "verbinding-tabblad toevoegen")<br>

## <a name="part2"></a>Deel 2: een lokale netwerkgateway toevoegen
1. Klik op **lokale netwerkgateway** ***een lokale netwerkgateway kiezen***. Hiermee opent u de **de lokale netwerkgateway kiezen** blade.
   
    ![De lokale netwerkgateway kiezen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "de lokale netwerkgateway kiezen")<br>
2. Klik op **nieuw** openen de **de lokale netwerkgateway maken** blade.
   
    ![Blade lokale netwerkgateway maken](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "de lokale netwerkgateway maken")<br>
3. Op de **de lokale netwerkgateway maken** blade Vul de volgende velden:
   
   * **Naam:** de naam die u wilt geven tot de netwerkbron voor lokale gateway.
   * **IP-adres:** het openbare IP-adres van de VPN-apparaat op de site die u verbinding wilt maken.
   * **Adresruimte:** de adresruimte die u wilt worden doorgestuurd naar de lokale netwerksite.
4. Klik op **OK** op de **de lokale netwerkgateway maken** blade de wijzigingen wilt opslaan.

## <a name="part3"></a>Deel 3: de gedeelde sleutel toevoegen en de verbinding maken
1. Op de **verbinding toevoegen** blade toevoegen van de gedeelde sleutel die u wilt gebruiken om uw verbinding te maken. U kunt de gedeelde sleutel ophalen van uw VPN-apparaat, of een hier en configureer vervolgens het VPN-apparaat voor het gebruik van dezelfde gedeelde sleutel. Het belangrijkste is dat de sleutels exact hetzelfde zijn.
   
    ![Gedeelde sleutel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Gedeelde sleutel")<br>
2. Klik onderaan de blade **OK** om de verbinding te maken.

## <a name="part4"></a>Deel 4 – de VPN-verbinding controleren


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a>Volgende stappen

Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie het [leertraject](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) voor virtuele machines voor meer informatie.
