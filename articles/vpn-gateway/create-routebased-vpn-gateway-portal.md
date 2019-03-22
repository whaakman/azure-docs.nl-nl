---
title: 'Maak een route gebaseerde VPN-gateway: Azure-portal | Microsoft Docs'
description: Een op route gebaseerde VPN-Gateway maken met behulp van de Azure portal
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 10/18/2018
ms.author: cherylmc
ms.openlocfilehash: ddc42023bae3403e7778327a40316462c85222c0
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58005522"
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Een op route gebaseerde VPN-gateway maken met behulp van de Azure portal

Dit artikel helpt u snel een op route gebaseerde Azure VPN-gateway met behulp van de Azure portal kunt maken.  Een VPN-gateway wordt gebruikt bij het maken van een VPN-verbinding met uw on-premises netwerk. U kunt ook een VPN-gateway gebruiken om VNets te verbinden. 

De stappen in dit artikel maakt een VNet, een subnet, een gatewaysubnet en een op route gebaseerde VPN-gateway (virtuele netwerkgateway). Als het maken van de gateway is voltooid, kunt u vervolgens verbindingen maken. Deze stappen hebt u een Azure-abonnement nodig. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="vnet"></a>Een virtueel netwerk maken

1. Open een browser, ga naar [Azure Portal](https://portal.azure.com) en meld u aan met uw Azure-account.
2. Klik op **Een resource maken**. In het veld **Marketplace doorzoeken** typt u 'virtueel netwerk'. Zoek **Virtual Network** in de lijst met resultaten en klik erop om de pagina **Virtual Network** te openen.
3. Kies onder aan de pagina Virtueelnetwerk in de **een implementatiemodel selecteren** lijst, Controleer **Resource Manager** is geselecteerd in de vervolgkeuzelijst en klik vervolgens op **maken**. Hiermee opent u de **virtueel netwerk maken** pagina.
4. Configureer de VNet-instellingen op de pagina **Virtueel netwerk maken**. Wanneer u de velden invult, verandert het rode uitroepteken in een groen vinkje als de tekens die zijn ingevoerd in het veld, geldig zijn. Gebruik de volgende waarden:

   - **Naam**: TestVNet1
   - **Adresruimte**: 10.1.0.0/16
   - **Abonnement**: Controleer of het weergegeven abonnement die u wilt gebruiken. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.
   - **Resourcegroep**: TestRG1
   - **Locatie**: US - oost
   - **Subnet**: Front-end
   - **Adresbereik**: 10.1.0.0/24

   ![Pagina Virtueel netwerk maken](./media/create-routebased-vpn-gateway-portal/create-virtual-network.png "Pagina Virtueel netwerk maken")
5. Na het invoeren van de waarden, selecteer **vastmaken aan dashboard** om gemakkelijk te vinden van uw VNet op het dashboard en klik vervolgens op **maken**. Nadat u hebt geklikt **maken**, ziet u een tegel op uw dashboard die de voortgang van uw VNet weergeeft. De tegel wordt gewijzigd wanneer het VNet wordt gemaakt.

## <a name="gwsubnet"></a>Een gatewaysubnet toevoegen

Het gatewaysubnet bevat de gereserveerde IP-adressen die de virtuele-netwerkgatewayservices gebruik. Een gatewaysubnet maken.

1. Navigeer in de portal naar het virtuele netwerk waarvoor u een virtuele netwerkgateway wilt maken.
2. Klik op de pagina van uw virtuele netwerk **subnetten** om uit te breiden **VNet1 - subnetten** pagina.
3. Klik op **+ gatewaysubnet** aan de bovenkant om te openen de **subnet toevoegen** pagina.

   ![Het gatewaysubnet toevoegen](./media/create-routebased-vpn-gateway-portal/gateway-subnet.png "Het gatewaysubnet toevoegen")
4. De **naam** voor uw subnet wordt automatisch gevuld met de vereiste waarde 'GatewaySubnet'. Pas de automatisch ingevulde **adresbereik** waarden zodat deze overeenkomen met de volgende waarden:

   **Adresbereik (CIDR-blok)**: 10.1.255.0/27

   ![Het gatewaysubnet toevoegen](./media/create-routebased-vpn-gateway-portal/add-gateway-subnet.png "Het gatewaysubnet toevoegen")
5. Klik op om het gatewaysubnet **OK** aan de onderkant van de pagina.

## <a name="gwvalues"></a>Gateway-instellingen configureren

1. Aan de linkerkant van de portal-pagina, klikt u op **+ een resource maken** en typ 'Virtuele netwerkgateway' in het zoekvak, klikt u vervolgens drukt u op **Enter**. Zoek in **Resultaten** naar **Virtuele netwerkgateway** en klik hierop.
2. Klik onderaan de pagina 'Virtuele netwerkgateway' op **maken** openen de **virtuele netwerkgateway maken** pagina.
3. Vul op de pagina **Virtuele netwerkgateway maken** de waarden in voor de gateway van het virtuele netwerk.

   - **Naam**: Vnet1GW
   - **Gatewaytype**: VPN 
   - **VPN-type**: Op route gebaseerd
   - **SKU**: VpnGw1
   - **Locatie**: US - oost
   - **Virtueel netwerk**: Klik op **virtuele-netwerk/een virtueel netwerk kiezen** openen de **een virtueel netwerk kiezen** pagina. Selecteer **VNet1**.
   - **Openbaar IP-adres**: Deze instelling bepaalt u het openbare IP-adres-object, dat gekoppeld aan de VPN-gateway wordt. Het openbare IP-adres wordt dynamisch toegewezen aan dit object wanneer de VPN-gateway wordt gemaakt. VPN Gateway ondersteunt momenteel alleen *dynamische* toewijzing van openbare IP-adressen. Dit betekent echter niet dat het IP-adres wordt gewijzigd nadat het aan uw VPN Gateway is toegewezen. Het openbare IP-adres verandert alleen wanneer de gateway wordt verwijderd en opnieuw wordt gemaakt. Het verandert niet wanneer de grootte van uw VPN Gateway verandert, wanneer deze gateway opnieuw wordt ingesteld of wanneer andere interne onderhoudswerkzaamheden of upgrades worden uitgevoerd.

     - Laat **Nieuwe maken** geselecteerd.
     - Typ in het tekstvak een **naam** voor uw openbare IP-adres. Voor deze oefening gebruikt u **VNet1GWIP**.<br>

     ![Gateway-instellingen configureren](./media/create-routebased-vpn-gateway-portal/gw.png "gateway-instellingen configureren")

## <a name="creategw"></a>De VPN-gateway maken

1. Controleer de instellingen op de **virtuele netwerkgateway maken** pagina. Waarden aanpassen indien nodig.
2. Klik op **maken** aan de onderkant van de pagina.

   Nadat u op **maken**, de instellingen worden gevalideerd en de **implementeren van virtuele netwerkgateway** tegel wordt weergegeven op het dashboard. Een VPN-gateway kan tot 45 minuten duren. U moet mogelijk uw portal-pagina vernieuwen om de voltooide status te kunnen zien.

## <a name="viewgw"></a>De VPN-gateway weergeven

1. Nadat de gateway is gemaakt, gaat u naar VNet1 in de portal. De VPN-gateway wordt weergegeven op de pagina overzicht als een verbonden apparaat.

   ![Verbonden apparaten](./media/create-routebased-vpn-gateway-portal/view-connected-devices.png "verbonden apparaten")

2. Klik in de lijst met apparaten op **VNet1GW** voor meer informatie.

   ![VPN-gateway weergeven](./media/create-routebased-vpn-gateway-portal/view-gateway.png "weergave VPN-gateway")

## <a name="next-steps"></a>Volgende stappen

Zodra de gateway heeft gemaakt, kunt u een verbinding maken tussen uw virtuele netwerk en een ander VNet. Of maak een verbinding tussen uw virtuele netwerk en een on-premises locatie.

> [!div class="nextstepaction"]
> [Maak een site-naar-site-verbinding](vpn-gateway-howto-site-to-site-resource-manager-portal.md)<br><br>
> [Een punt-naar-site-verbinding maken](vpn-gateway-howto-point-to-site-resource-manager-portal.md)<br><br>
> [Maak een verbinding met een ander VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
