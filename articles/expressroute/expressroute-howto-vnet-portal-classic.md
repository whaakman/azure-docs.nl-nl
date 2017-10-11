---
title: Een virtueel netwerk en de Gateway in de klassieke portal configureren voor ExpressRoute | Microsoft Docs
description: Dit artikel begeleidt u bij het instellen van een virtueel netwerk voor ExpressRoute met behulp van het klassieke implementatiemodel en de klassieke portal.
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 688817d9-51c8-4372-9af8-33fa098c7c5a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2016
ms.author: cherylmc
ms.openlocfilehash: f62254b2a7df50aa55a2a49009702848a9aecebd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Een virtueel netwerk maken voor ExpressRoute in de klassieke portal
De stappen in dit artikel begeleidt u bij het configureren van een virtueel netwerk en een virtuele netwerkgateway voor gebruik met behulp van het klassieke implementatiemodel en de klassieke portal ExpressRoute.

Als u nog geen instructies voor het Resource Manager-implementatiemodel, kunt u de volgende artikelen: [een virtueel netwerk maken met behulp van PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) en [een VPN-Gateway toevoegen aan een Resource Manager VNet voor ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Over Azure-implementatiemodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="create-a-classic-vnet-and-gateway"></a>Een klassiek VNet en gateway maken
De volgende stappen uit maken een klassiek VNet en een virtuele netwerkgateway. Als u al een klassiek VNet hebt, raadpleegt u de [configureren van een bestaande klassieke VNet](#config) sectie in dit artikel.

1. Meld u aan bij de [klassieke Azure Portal](http://manage.windowsazure.com).
2. Klik in de linkerbenedenhoek van het scherm op **nieuw**. Klik in het navigatievenster op **Netwerkservices** en vervolgens op **Virtueel netwerk**. Klik op **Aangepast maken** om de configuratiewizard te starten.
3. Op de **Details van virtueel netwerk** pagina, voert u het volgende:
   
   * **Naam** – naam van het virtuele netwerk. U gebruikt deze virtuele-netwerknaam bij het implementeren van uw virtuele machines en PaaS-instanties, zodat u niet wilt maken van de naam is te ingewikkeld.
   * **Locatie** – de locatie is direct gerelateerd aan de fysieke locatie (regio) waar u uw resources (VM's) zich bevinden. Selecteer bijvoorbeeld 'VS - oost' als u wilt dat de virtuele machines die u in dit virtuele netwerk implementeert, zich fysiek bevinden in 'VS - oost'. U kunt de regio die is gekoppeld aan het virtuele netwerk nadat u deze hebt gemaakt niet wijzigen.
4. Op de pagina **DNS-servers en VPN-connectiviteit** voert u de volgende informatie in en klikt u op de pijl Volgende in de rechterbenedenhoek. 
   
   * **DNS-Servers** : Geef de DNS-servernaam en het IP-adres op of Selecteer een eerder geregistreerde DNS-server in het snelmenu. Met deze instelling wordt geen DNS-server gemaakt. U kunt hiermee de DNS-servers opgeven die u wilt gebruiken voor de naamomzetting voor dit virtuele netwerk.
   * **Site-naar-Site-connectiviteit** -Schakel het selectievakje voor **een site-naar-site-VPN configureren**.
   * **ExpressRoute** – Selecteer het selectievakje **gebruik ExpressRoute**. Deze optie wordt alleen weergegeven als u hebt geselecteerd **een Site-naar-Site-VPN configureren**.
   * **Lokale netwerk** -u moet een lokale netwerksite hebben voor ExpressRoute. In het geval van een ExpressRoute-verbinding, zal de adresvoorvoegsels opgegeven voor de lokale netwerksite worden genegeerd. In plaats daarvan wordt de adresvoorvoegsels geadverteerd aan Microsoft via het ExpressRoute-circuit gebruikt voor de routering.<BR>Als u al een lokaal netwerk gemaakt voor uw ExpressRoute-verbinding hebt, selecteert u deze uit de vervolgkeuzelijst. Zo niet, selecteer **opgeven van een nieuw lokaal netwerk**.
5. De **Site-naar-Site-connectiviteit** pagina wordt weergegeven als u een nieuw lokaal netwerk opgeven in de vorige stap hebt geselecteerd. Voer de volgende informatie voor het configureren van uw lokale netwerk, en klik op de pijl Volgende. 
   
   * **Naam** -de naam die u wilt (op locatie) van de lokale netwerksite.
   * **Adresruimte** - inclusief IP-beginadres en de CIDR (aantal adressen). Als deze niet met het adresbereik voor het virtuele netwerk overlappen, kunt u eventuele-adresbereik opgeven. Normaal gesproken dit geeft de adresbereiken voor uw on-premises netwerken, maar in het geval van ExpressRoute, worden deze instellingen niet gebruikt. Deze instelling is echter vereist om te kunnen maken van het lokale netwerk wanneer u de klassieke portal gebruikt.
   * **Adresruimte toevoegen** -deze instelling is niet relevant voor ExpressRoute.
6. Op de **adresruimten voor virtueel netwerk** pagina, voer de volgende informatie en klik op het vinkje in de rechterbenedenhoek om uw netwerk te configureren. 
   
   * **Adresruimte** - zoals begin-IP en los deze telling. Controleer of dat de adresruimten die u opgeeft niet overlappen met adresruimten die u op uw lokale netwerk hebt.
   * **Subnet toevoegen** - inclusief IP-beginadres en het aantal adressen. Er zijn geen aanvullende subnetten nodig.
   * **Gatewaysubnet toevoegen** -Klik hier om het gatewaysubnet. Het gatewaysubnet wordt alleen gebruikt voor de gateway van het virtuele netwerk. Het is vereist voor deze configuratie.<BR>Het gatewaysubnet CIDR (aantal adressen) voor ExpressRoute moet /28 of groter (/ 27, / 26 enzovoort.). Hierdoor voldoende IP-adressen in dat subnet waarmee de configuratie werkt. In de klassieke portal als u het selectievakje voor het gebruik van ExpressRoute, hebt geselecteerd geeft de portal een gatewaysubnet met /28.  U kunt het aantal van de CIDR-adres in de klassieke portal niet aanpassen. Het gatewaysubnet wordt weergegeven als **Gateway** in de klassieke portal, hoewel de werkelijke naam van het gatewaysubnet dat is gemaakt daadwerkelijk **GatewaySubnet**. U kunt deze naam weergeven met behulp van PowerShell of in de Azure-portal.
7. Wanneer u onder aan de pagina op het vinkje klikt, wordt het virtuele netwerk gemaakt. Wanneer deze is voltooid, ziet u **gemaakt** vermeld onder **Status** op de **netwerken** pagina in de klassieke portal.

## <a name="gw"></a>De gateway maken
1. Op de **netwerken** pagina, klikt u op het virtuele netwerk dat u zojuist hebt gemaakt en klik op **Dashboard** boven aan de pagina.
2. Aan de onderkant van de **Dashboard** pagina, klikt u op **Gateway maken** en selecteer **dynamische routering**. Klik op **Ja** om te bevestigen dat u wilt maken van een gateway.
3. Wanneer de gateway wordt gestart maken, ziet u een bericht waarin wordt aangegeven dat de gateway is gestart. Duurt 45 minuten duren voordat de gateway te maken.
4. Uw netwerk koppelen aan een circuit. Volg de instructies in het artikel [het VNets koppelen aan ExpressRoute-circuits](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Een bestaande klassieke VNet configureren voor ExpressRoute
Als u al een klassiek VNet hebt, kunt u deze verbinding maken met ExpressRoute in de klassieke portal configureren. De instellingen zijn hetzelfde als de bovenstaande secties Lees via deze gedeelten om vertrouwd te raken met de vereiste instellingen. Als u wilt een naast elkaar bestaande ExpressRoute/Site-naar-Site-verbinding te maken, Zie [in dit artikel](expressroute-howto-coexist-classic.md) voor de stappen. Ze zijn anders dan de stappen in dit artikel.

1. U moet het lokale netwerk maken voordat u de rest van uw VNet-instellingen bijwerken. Als u wilt maken van een nieuw lokaal netwerk dat vereist is bij het configureren van ExpressRoute via de klassieke portal, klikt u op **nieuw**  **>**  **netwerkservices**  **>**  **virtueel netwerk**  **>**  **toevoegen lokale netwerk**. Volg de stappen in de wizard voor het maken van het lokale netwerk.
2. Gebruik **configureren** bijwerken van de rest van de instellingen voor uw VNet en voor het koppelen van het VNet met het lokale netwerk.
3. Nadat u de instellingen configureren, gaat u naar de [maken van de gateway](#gw) sectie van dit artikel de gateway te maken.

## <a name="next-steps"></a>Volgende stappen
* Als u wilt dat virtuele machines toevoegen aan het virtuele netwerk, Zie [virtuele Machines leertrajecten](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
* Als u meer informatie over ExpressRoute wilt, raadpleegt u de [overzicht van ExpressRoute](expressroute-introduction.md).

