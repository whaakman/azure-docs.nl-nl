---
title: 'Maken van een op route gebaseerde VPN-gateway: Azure portal | Microsoft Docs'
description: Maak snel een op route gebaseerde VPN-Gateway met behulp van de Azure portal
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2018
ms.author: cherylmc
ms.openlocfilehash: fe05ab36f971105cf72342b8df5e2a82de7fc2b8
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391790"
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Een op route gebaseerde VPN-gateway maken met behulp van de Azure portal

Dit artikel helpt u snel een op route gebaseerde Azure VPN-gateway met behulp van de Azure portal kunt maken.  Een VPN-gateway wordt gebruikt bij het maken van een VPN-verbinding met uw on-premises netwerk. U kunt ook een VPN-gateway gebruiken om VNets te verbinden. 

De stappen in dit artikel maakt een VNet, een subnet, een gatewaysubnet en een op route gebaseerde VPN-gateway (virtuele netwerkgateway). Als het maken van de gateway is voltooid, kunt u vervolgens verbindingen maken. Deze stappen hebt u een Azure-abonnement nodig. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="vnet"></a>Een virtueel netwerk maken

1. Open een browser, ga naar [Azure Portal](http://portal.azure.com) en meld u aan met uw Azure-account.
2. Klik op **Een resource maken**. In het veld **Marketplace doorzoeken** typt u 'virtueel netwerk'. Zoek **Virtual Network** in de lijst met resultaten en klik erop om de pagina **Virtual Network** te openen.
3. Kies onder aan de pagina Virtueelnetwerk in de **een implementatiemodel selecteren** lijst, Controleer **Resource Manager** is geselecteerd in de vervolgkeuzelijst en klik vervolgens op **maken**. Hiermee opent u de **virtueel netwerk maken** pagina.
4. Configureer de VNet-instellingen op de pagina **Virtueel netwerk maken**. Wanneer u de velden invult, verandert het rode uitroepteken in een groen vinkje als de tekens die zijn ingevoerd in het veld, geldig zijn. Gebruik de volgende waarden:

  - **Naam**: TestVNet1
  - **Adresruimte**: 10.1.0.0/16
  - **Abonnement**: Controleer of het weergegeven abonnement is die u wilt gebruiken. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.
  - **Resourcegroep**: TestRG1
  - **Locatie**: VS-Oost
  - **Subnet**: Frontend
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
  - **Gateway van het type**: VPN 
  - **VPN-type**: op Route gebaseerd
  - **SKU**: VpnGw1
  - **Locatie**: VS-Oost
  - **Virtueel netwerk**: klik op **virtuele-netwerk/een virtueel netwerk kiezen** openen de **een virtueel netwerk kiezen** pagina. Selecteer **VNet1**.

  ![Gateway-instellingen configureren](./media/create-routebased-vpn-gateway-portal/configure-gateway.png "gateway-instellingen configureren")

## <a name="pip"></a>Een openbaar IP-adres maken

Een VPN-gateway moet een dynamisch toegewezen openbare IP-adres hebben. Wanneer u een verbinding met een VPN-gateway maakt, is dit het IP-adres waarmee uw on-premises apparaat verbinding maakt.

1. Selecteer **IP-configuratie voor eerste IP-configuratie maken gateway** om aan te vragen van een openbaar IP-adres.

  ![Eerste IP-configuratie](./media/create-routebased-vpn-gateway-portal/add-public-ip-address.png "eerste IP-configuratie")
2. Op de **het openbare IP-pagina kiezen**, klikt u op **+ nieuwe maken** openen de **openbare IP-adres maken** pagina.
3. Configureer de instellingen met de volgende waarden:

  - **Naam**: **VNet1GWIP**
  - **SKU**: **Basic**

  ![Openbaar IP maken](./media/create-routebased-vpn-gateway-portal/public-ip-address-name.png "PIP maken")
4. Klik op **OK** onderaan deze pagina uw wijzigingen op te slaan.

## <a name="creategw"></a>De VPN-gateway maken

1. Controleer de instellingen op de **virtuele netwerkgateway maken** pagina. Waarden aanpassen indien nodig.

  ![VPN-gateway maken](./media/create-routebased-vpn-gateway-portal/create-vpn-gateway.png "maken VPN-gateway")
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
