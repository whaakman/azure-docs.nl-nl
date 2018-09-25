---
title: Over Azure Point-to-Site-routering | Microsoft Docs
description: In dit artikel helpt u begrijpen hoe punt-naar-Site VPN-routering werkt.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: ''
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/16/2018
ms.author: anzaman
ms.openlocfilehash: 620a2bf9221bdb7c46dc36a2b3ed23d853faff35
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031708"
---
# <a name="about-point-to-site-vpn-routing"></a>Over point-to-site-VPN-routering

In dit artikel helpt u begrijpen hoe Azure Point-to-Site VPN-routering werkt. De werking van P2S VPN-routering is afhankelijk van het clientbesturingssysteem, het protocol dat wordt gebruikt voor de VPN-verbinding en de manier waarop de virtuele netwerken (VNets) aan elkaar zijn verbonden.

Azure ondersteunt momenteel twee protocollen voor externe toegang, IKEv2 en SSTP. IKEv2 wordt ondersteund op veel client-besturingssystemen, met inbegrip van Windows, Linux, MacOS, Android en iOS. SSTP wordt alleen ondersteund op Windows. Als u een wijziging in de topologie van uw netwerk aanbrengt en Windows VPN-clients hebt, moet het VPN-clientpakket voor Windows-clients worden gedownload en geïnstalleerd opnieuw zodat de wijzigingen worden toegepast op de client.

> [!NOTE]
> In dit artikel geldt alleen voor IKEv2.
>

## <a name="diagrams"></a>De diagrammen

Er zijn een aantal verschillende diagrammen in dit artikel. Elke sectie ziet u een andere topologie of configuratie. Voor de doeleinden van dit artikel, Site-naar-Site (S2S) en VNet-naar-VNet-verbindingen werken de dezelfde manier als beide IPsec-tunnels zijn. Alle VPN-gateways in dit artikel zijn op basis van route.

## <a name="isolatedvnet"></a>Een VNet (geïsoleerd)

De punt-naar-Site VPN-gatewayverbinding in dit voorbeeld is voor een VNet dat niet is verbonden of die zijn gekoppeld aan een ander virtueel netwerk (VNet1). In dit voorbeeld, kunnen clients met behulp van SSTP of IKEv2 toegang tot VNet1.

![geïsoleerd VNet routering](./media/vpn-gateway-about-point-to-site-routing/1.jpg "VNet routering (geïsoleerd)")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes die zijn toegevoegd aan Windows-clients: 10.1.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows-clients hebben toegang tot VNet1

* Niet-Windows-clients hebben toegang tot VNet1

## <a name="multipeered"></a>Meerdere gekoppelde VNets

In dit voorbeeld is de punt-naar-Site VPN-gatewayverbinding voor VNet1. VNet1 is gekoppeld aan VNet2. VNet-2 is gekoppeld aan VNet3. VNet1 is gekoppeld aan en VNet4. Er is geen directe voor peering tussen VNet1 en VNet3. VNet1 is 'Gatewayoverdracht toestaan' en VNet2 is ' Externe gateways gebruiken' ingeschakeld.

Clients met behulp van Windows rechtstreeks gekoppelde VNets kunnen gebruiken, maar de VPN-client opnieuw moet worden gedownload als er wijzigingen zijn aangebracht aan VNet-peering of de netwerktopologie. Niet-Windows-clients hebben toegang tot rechtstreeks gekoppelde VNets. Toegang is niet transitief en is beperkt tot alleen rechtstreeks gekoppelde VNets.

![meerdere VNets gekoppeld](./media/vpn-gateway-about-point-to-site-routing/2.jpg "meerdere VNets gekoppeld")

### <a name="address-space"></a>Adresruimte:

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* En VNet4: 10.4.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes die zijn toegevoegd aan Windows-clients: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows-clients hebben toegang tot VNet1, VNet2 en en VNet4, maar de VPN-client opnieuw moet worden gedownload voor elke Topologiewijzigingen worden doorgevoerd.

* Niet-Windows-clients hebben toegang tot VNet1, VNet2 en en VNet4

## <a name="multis2s"></a>Meerdere VNets die zijn verbonden met een S2S-VPN

In dit voorbeeld is de punt-naar-Site VPN-gatewayverbinding voor VNet1. VNet1 naar VNet2 is verbonden met behulp van een Site-naar-Site VPN-verbinding. VNet2 is verbonden met VNet3 met behulp van een Site-naar-Site VPN-verbinding. Er is geen directe peering of Site-naar-Site VPN-verbinding tussen VNet1 en VNet3. Alle Site-naar-Site-verbindingen worden niet uitgevoerd voor BGP voor routering.

Clients met Windows of een ander ondersteund besturingssysteem, hebben alleen toegang tot VNet1. Voor toegang tot extra VNets, moet BGP worden gebruikt.

![meerdere VNets en S2S](./media/vpn-gateway-about-point-to-site-routing/3.jpg "meerdere VNets en S2S")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes die zijn toegevoegd aan Windows-clients: 10.1.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 10.2.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows-clients hebben alleen toegang tot VNet1

* Niet-Windows-clients alleen toegang kunnen krijgen tot VNet1

## <a name="multis2sbgp"></a>Meerdere VNets die zijn verbonden via een S2S-VPN (BGP)

In dit voorbeeld is de punt-naar-Site VPN-gatewayverbinding voor VNet1. VNet1 naar VNet2 is verbonden met behulp van een Site-naar-Site VPN-verbinding. VNet2 is verbonden met VNet3 met behulp van een Site-naar-Site VPN-verbinding. Er is geen directe peering of Site-naar-Site VPN-verbinding tussen VNet1 en VNet3. Alle Site-naar-Site-verbindingen uitvoert voor routering BGP.

Clients met Windows of een ander ondersteund besturingssysteem, toegang tot alle vnet's die zijn verbonden via een Site-naar-Site VPN-verbinding, maar de routes naar verbonden VNets moeten handmatig worden toegevoegd aan de Windows-clients.

![meerdere VNets en S2S (BGP)](./media/vpn-gateway-about-point-to-site-routing/4.jpg "meerdere VNets en S2S BGP")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes die zijn toegevoegd aan Windows-clients: 10.1.0.0/16

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows-clients hebben toegang tot VNet1, VNet2 en VNet3, maar de routes naar VNet2 en VNet3 moet handmatig worden toegevoegd.

* Niet-Windows-clients hebben toegang tot VNet1, VNet2 en VNet3

## <a name="vnetbranch"></a>Een VNet en een filiaal

In dit voorbeeld is de punt-naar-Site VPN-gatewayverbinding voor VNet1. VNet1 is niet verbonden of gekoppeld aan andere virtuele netwerk, maar is verbonden met een on-premises site via een Site-naar-Site VPN-verbinding die wordt niet uitgevoerd voor BGP.

Windows- en niet-Windows-clients hebben alleen toegang tot VNet1.

![Routering met een VNet en een filiaal](./media/vpn-gateway-about-point-to-site-routing/5.jpg "routering met een VNet en een filiaal")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes die zijn toegevoegd aan Windows-clients: 10.1.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows-clients hebben toegang tot alleen VNet1

* Niet-Windows-clients alleen toegang kunnen krijgen tot VNet1

## <a name="vnetbranchbgp"></a>Een VNet en een filiaal (BGP)

In dit voorbeeld is de punt-naar-Site VPN-gatewayverbinding voor VNet1. VNet1 is niet verbonden of gekoppeld aan andere virtuele netwerk, maar is verbonden met een on-premises-site (Site1) via een Site-naar-Site-VPN-verbinding met BGP.

Windows-clients hebben toegang tot het VNet en het filiaal (Site1), maar de routes naar Site1 moeten handmatig worden toegevoegd aan de client. Niet-Windows-clients hebben toegang tot het VNet, evenals het filiaal on-premises.

![een VNet en een filiaal (BGP)](./media/vpn-gateway-about-point-to-site-routing/6.jpg "één VNet en een filiaal")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes die zijn toegevoegd aan Windows-clients: 10.1.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows-clients hebben toegang tot VNet1 en Site1, maar de routes naar Site1 moet handmatig worden toegevoegd.

* Niet-Windows-clients hebben toegang tot VNet1 en Site1.


## <a name="multivnets2sbranch"></a>Meerdere VNets die zijn verbonden via S2S en een filiaal

In dit voorbeeld is de punt-naar-Site VPN-gatewayverbinding voor VNet1. VNet1 naar VNet2 is verbonden met behulp van een Site-naar-Site VPN-verbinding. VNet2 is verbonden met VNet3 met behulp van een Site-naar-Site VPN-verbinding. Er is geen directe peering of Site-naar-Site VPN-tunnel tussen de netwerken VNet1 en VNet3. VNet3 is verbonden met een filiaal (Site1) met behulp van een Site-naar-Site VPN-verbinding. Alle VPN-verbindingen worden niet uitgevoerd voor BGP.

Alle clients kunnen alleen toegang krijgen tot VNet1.

![Multi-VNet S2S en de vertakking office](./media/vpn-gateway-about-point-to-site-routing/7.jpg "multi-VNet S2S en de vertakking office")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes toegevoegd clients: 10.1.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* De Windows-clients alleen toegang kunnen krijgen tot VNet1

* Niet-Windows-clients alleen toegang kunnen krijgen tot VNet1

## <a name="multivnets2sbranchbgp"></a>Meerdere VNets die zijn verbonden via S2S en een filiaal (BGP)

In dit voorbeeld is de punt-naar-Site VPN-gatewayverbinding voor VNet1. VNet1 naar VNet2 is verbonden met behulp van een Site-naar-Site VPN-verbinding. VNet2 is verbonden met VNet3 met behulp van een Site-naar-Site VPN-verbinding. Er is geen directe peering of Site-naar-Site VPN-tunnel tussen de netwerken VNet1 en VNet3. VNet3 is verbonden met een filiaal (Site1) met behulp van een Site-naar-Site VPN-verbinding. Alle VPN-verbindingen worden BGP uitgevoerd.

Clients met behulp van Windows toegang heeft tot VNets en sites die zijn verbonden met een Site-naar-Site VPN-verbinding, maar de routes naar VNet2, VNet3 en Site1 moeten handmatig worden toegevoegd aan de client. Niet-Windows-clients hebben toegang tot VNets en sites die zijn verbonden via een Site-naar-Site VPN-verbinding zonder handmatige tussenkomst. De toegang kan worden overgedragen en clients hebben toegang tot resources in alle verbonden VNets en sites (on-premises).

![Multi-VNet S2S en de vertakking office](./media/vpn-gateway-about-point-to-site-routing/8.jpg "multi-VNet S2S en de vertakking office")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes toegevoegd clients: 10.1.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* De Windows-clients hebben toegang tot VNet1, VNet2, VNet3 en Site1, maar de routes naar VNet2, VNet3 en Site1 moeten handmatig worden toegevoegd aan de client.

* Niet-Windows-clients hebben toegang tot VNet1, Vnet2, VNet3 en Site1.

## <a name="next-steps"></a>Volgende stappen

Zie [maken van een P2S-VPN met behulp van de Azure-portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md) om te beginnen met het maken van uw P2S-VPN.
