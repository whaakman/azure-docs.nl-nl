---
title: 'Toevoegen van een virtuele netwerkgateway met een VNet voor ExpressRoute: Portal: Azure | Microsoft Docs'
description: Dit artikel begeleidt u bij het toevoegen van een gateway van virtueel netwerk met een bestaande Resource Manager VNet voor ExpressRoute.
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2017
ms.author: cherylmc
ms.openlocfilehash: 6198c8723ccbb225a4cc60526e66f4651390b26c
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51255690"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-the-azure-portal"></a>Een virtuele netwerkgateway configureren voor ExpressRoute met behulp van de Azure-portal
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klassiek - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

In dit artikel begeleidt u bij de stappen voor het toevoegen van een virtuele netwerkgateway voor een bestaande VNet. In dit artikel begeleidt u bij de stappen voor het toevoegen en verwijderen van de gateway van een virtueel netwerk (VNet) voor een bestaande VNet vergroten of verkleinen. De stappen voor deze configuratie zijn specifiek voor VNets die zijn gemaakt met het implementatiemodel van Resource Manager die worden gebruikt voor een ExpressRoute-configuratie. Zie voor meer informatie over virtuele netwerkgateways en configuratie-instellingen voor ExpressRoute gateway [virtuele netwerkgateways voor ExpressRoute](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Voordat u begint

De stappen voor deze taak gebruik van een VNet op basis van de waarden in de volgende lijst voor de configuratie-verwijzing. Deze lijst gebruiken we in onze voorbeelden van stappen. U kunt de lijst om te gebruiken als uitgangspunt, vervang de waarden door uw eigen kopiëren.

**Lijst van configuratie-verwijzing**

* Virtuele-netwerknaam 'TestVNet' =
* Virtuele netwerkadresruimte = 192.168.0.0/16
* Subnet Name = 'FrontEnd' 
    * Subnet een adresruimte = "192.168.1.0/24"
* Resourcegroep = "TestRG"
* Locatie = 'VS-Oost'
* Naam Gatewaysubnet: 'GatewaySubnet' u moet altijd een gatewaysubnet de naam *GatewaySubnet*.
    * Gateway Subnet een adresruimte = "192.168.200.0/26"
* Gatewaynaam = "ERGW"
* De naam van de gateway-IP = "MyERGWVIP"
* Gatewaytype 'ExpressRoute' = dit type is vereist voor een ExpressRoute-configuratie.
* Gatewaynaam van het openbare IP-adres = "MyERGWVIP"

U vindt een [Video](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network) van deze stappen voordat u begint met uw configuratie.

## <a name="create-the-gateway-subnet"></a>Her gatewaysubnet maken

1. Navigeer in de [portal](http://portal.azure.com) naar het virtuele netwerk van Resource Manager waarvoor u een gateway wilt maken.
2. Klik in de sectie **Instellingen** van de blade VNet op **Subnetten** om de blade Subnetten uit te vouwen.
3. Klik op de blade **Subnetten** op **+Gatewaysubnet** om de blade **Subnet toevoegen** te openen. 
   
    ![Het gatewaysubnet toevoegen](./media/expressroute-howto-add-gateway-portal-resource-manager/addgwsubnet.png "Het gatewaysubnet toevoegen")


4. Als **naam** voor het subnet wordt automatisch de waarde GatewaySubnet ingevuld. Deze waarde is vereist. Zonder deze waarde wordt het subnet niet in Azure als het gatewaysubnet herkend. Pas de automatisch ingevulde waarden voor de **Adressenreeks** aan overeenkomstig uw configuratievereisten. We raden aan een gatewaysubnet maken met een/27 of groter (/ 26/25 enz.). Klik vervolgens op **OK** opslaan van de waarden en het gatewaysubnet maken.

    ![Het subnet toevoegen](./media/expressroute-howto-add-gateway-portal-resource-manager/addsubnetgw.png "Het subnet toevoegen")

## <a name="create-the-virtual-network-gateway"></a>De gateway van het virtuele netwerk maken

1. Klik in de portal aan de linkerkant op **+** en typ 'Gateway van virtueel netwerk' in het zoekvak. Klik op het zoekresultaat **Gateway van het virtuele netwerk**. Klik onder aan de blade **Gateway van het virtuele netwerk** op **Maken**. Hiermee opent u de blade **Gateway van het virtuele netwerk maken**.
2. Vul op de blade **Gateway van het virtuele netwerk maken** de waarden in voor de gateway van het virtuele netwerk.

    ![Velden van de blade Gateway van het virtuele netwerk maken](./media/expressroute-howto-add-gateway-portal-resource-manager/gw.png "Velden van de blade Gateway van het virtuele netwerk maken")
3. **Naam**: naam van uw gateway. Deze mag niet gelijk zijn aan de naam van het gateway-subnet. Het is de naam van het gateway-object dat u maakt.
4. **Gateway van het type**: Selecteer **ExpressRoute**.
5. **SKU**: selecteer de gateway-SKU in de vervolgkeuzelijst.
6. **Locatie**: wijzig de **locatie** om naar de locatie van het virtuele netwerk te verwijzen. Als de locatie niet verwijst naar de regio waarin het virtuele netwerk zich bevindt, wordt het virtuele netwerk niet weergegeven in de vervolgkeuzelijst Een virtueel netwerk kiezen.
7. Kies het virtuele netwerk waaraan u deze gateway wilt toevoegen. Klik op **Virtueel netwerk** om de blade **Een virtueel netwerk kiezen** te openen. Selecteer het VNet. Als u uw VNet niet ziet, moet u controleren of het veld **Locatie** verwijst naar de regio waarin het virtuele netwerk zich bevindt.
9. Kies een openbaar IP-adres. Klik op **Openbaar IP-adres** om de blade **Openbaar IP-adres kiezen** te openen. Klik op **+Nieuwe maken** om de blade **Openbaar IP-adres maken** te openen. Geef een naam op voor uw openbare IP-adres. Met deze blade maakt u een openbaar IP-adresobject waaraan een openbaar IP-adres dynamisch wordt toegewezen. Klik op **OK** om uw wijzigingen in deze blade op te slaan.
10. **Abonnement**: controleer of het juiste abonnement is geselecteerd.
11. **Resourcegroep**: deze instelling wordt bepaald door het virtuele netwerk dat u selecteert.
12. Wijzig de **locatie** niet nadat u de voorgaande instellingen hebt opgegeven.
13. Controleer de instellingen. Als u wilt dat uw gateway op het dashboard wordt weergegeven, kunt u onderaan op de blade **Vastmaken aan dashboard** selecteren.
14. Klik op **Aanmaken** om de gateway aan te maken. De instellingen worden gevalideerd en de gateway wordt geïmplementeerd. Het maken van de gateway van virtueel netwerk kan tot 45 minuten duren om uit te voeren.

## <a name="next-steps"></a>Volgende stappen
Nadat u de VNet-gateway hebt gemaakt, kunt u uw VNet koppelen aan een ExpressRoute-circuit. Zie [een Virtueelnetwerk koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-portal-resource-manager.md).