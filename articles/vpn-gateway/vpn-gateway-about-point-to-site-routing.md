---
title: Over Azure punt-naar-Site routering | Microsoft Docs
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
ms.openlocfilehash: d25709fb4abb1b8a35596c3dc246f7419a99419b
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="about-point-to-site-vpn-routing"></a>Informatie over punt-naar-Site VPN-routering

In dit artikel helpt u begrijpen hoe Azure punt-naar-Site VPN-routering werkt. P2S-VPN-routeringsgedrag is afhankelijk van de client-besturingssysteem, het protocol dat wordt gebruikt voor de VPN-verbinding en hoe de virtuele netwerken (vnet's) met elkaar zijn verbonden.

Azure ondersteunt momenteel twee protocollen voor externe toegang, IKEv2- en SSTP. IKEv2 wordt ondersteund op veel client-besturingssystemen, met inbegrip van Windows, Linux, Mac OS, Android en iOS. SSTP wordt alleen ondersteund in Windows. Als u een wijziging in de topologie van uw netwerk aanbrengt en de Windows VPN-clients hebt, moet het VPN-clientpakket voor Windows-clients worden gedownload en geïnstalleerd opnieuw om de wijzigingen worden toegepast op de client.

> [!NOTE]
> In dit artikel geldt alleen voor IKEv2.
>

## <a name="diagrams"></a>De diagrammen

Er zijn een aantal verschillende diagrammen in dit artikel. Elke sectie ziet u een andere topologie of de configuratie. Voor de doeleinden van dit artikel, Site-naar-Site (S2S) en VNet-naar-VNet-verbindingen werken dezelfde manier als beide IPsec-tunnels zijn. Alle VPN-gateways in dit artikel zijn op basis van route.

## <a name="isolatedvnet"></a>Een geïsoleerde VNet

De punt-naar-Site VPN-gateway-verbinding in dit voorbeeld is voor een VNet dat niet is verbonden of een ander virtueel netwerk (VNet1) gekoppeld. In dit voorbeeld is clients via SSTP of IKEv2 VNet1 toegankelijk.

![geïsoleerde VNet routering](./media/vpn-gateway-about-point-to-site-routing/1.jpg "geïsoleerd VNet routering")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes die worden toegevoegd aan Windows-clients: 10.1.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows-clients hebben toegang tot VNet1

* Niet-Windows-clients hebben toegang tot VNet1

## <a name="multipeered"></a>Meerdere brengen VNets

In dit voorbeeld is de punt-naar-Site VPN-gateway-verbinding voor VNet1. VNet1 is gekoppeld aan VNet2. VNet-2 is gekoppeld aan VNet3. VNet1 is gekoppeld aan VNet4. Er is geen directe peering tussen VNet1 en VNet3. VNet1 heeft 'Gateway onderweg toestaan' en VNet2 ' Externe gateways gebruiken' ingeschakeld.

Clients met behulp van Windows toegang tot VNets rechtstreeks peer is ingesteld, maar de VPN-client opnieuw moet worden gedownload als er wijzigingen zijn aangebracht aan VNet-peering of de netwerktopologie. Niet-Windows-clients hebben toegang tot de VNets rechtstreeks peer is ingesteld. Toegang is niet transitief en is beperkt tot alleen VNets rechtstreeks peer is ingesteld.

![meerdere VNets brengen](./media/vpn-gateway-about-point-to-site-routing/2.jpg "meerdere VNets brengen")

### <a name="address-space"></a>Adresruimte:

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* VNet4: 10.4.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes die worden toegevoegd aan Windows-clients: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows-clients hebben toegang tot VNet1 VNet2 en VNet4, maar de VPN-client opnieuw moet worden gedownload voor Topologiewijzigingen in werking te laten treden.

* Niet-Windows-clients hebben toegang tot VNet1 VNet2 en VNet4

## <a name="multis2s"></a>Meerdere VNets die zijn verbonden via een S2S-VPN

In dit voorbeeld is de punt-naar-Site VPN-gateway-verbinding voor VNet1. VNet1 is verbonden met VNet2 met behulp van een Site-naar-Site VPN-verbinding. VNet2 is verbonden met VNet3 met behulp van een Site-naar-Site VPN-verbinding. Er is geen directe peering of Site-naar-Site VPN-verbinding tussen VNet1 en VNet3. Alle Site-naar-Site-verbindingen worden niet uitgevoerd voor BGP voor routering.

Clients met Windows, of een ander ondersteund besturingssysteem, kunnen alleen toegang tot VNet1. Voor toegang tot extra VNets moet moet BGP worden gebruikt.

![meerdere VNets en S2S](./media/vpn-gateway-about-point-to-site-routing/3.jpg "meerdere VNets en S2S")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes die worden toegevoegd aan Windows-clients: 10.1.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 10.2.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows-clients alleen toegang tot VNet1

* Niet-Windows-clients kunnen alleen VNet1 benaderen

## <a name="multis2sbgp"></a>Meerdere VNets met elkaar verbonden via een S2S-VPN (BGP)

In dit voorbeeld is de punt-naar-Site VPN-gateway-verbinding voor VNet1. VNet1 is verbonden met VNet2 met behulp van een Site-naar-Site VPN-verbinding. VNet2 is verbonden met VNet3 met behulp van een Site-naar-Site VPN-verbinding. Er is geen directe peering of Site-naar-Site VPN-verbinding tussen VNet1 en VNet3. Alle Site-naar-Site-verbindingen uitvoert voor routering BGP.

Clients met Windows, of een ander ondersteund besturingssysteem, toegang tot alle VNets die zijn verbonden via een Site-naar-Site VPN-verbinding, maar de routes naar verbonden VNets moeten handmatig worden toegevoegd aan de Windows-clients.

![meerdere VNets en S2S (BGP)](./media/vpn-gateway-about-point-to-site-routing/4.jpg "meerdere VNets en S2S BGP")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes die worden toegevoegd aan Windows-clients: 10.1.0.0/16

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows-clients hebben toegang tot VNet1 VNet2 en VNet3, maar de routes naar VNet2 en VNet3 moeten handmatig worden toegevoegd.

* Niet-Windows-clients hebben toegang tot VNet1 VNet2 en VNet3

## <a name="vnetbranch"></a>Een VNet en een filiaal

In dit voorbeeld is de punt-naar-Site VPN-gateway-verbinding voor VNet1. VNet1 is niet verbonden / brengen met een ander virtueel netwerk, maar is verbonden met een on-premises site via een Site-naar-Site VPN-verbinding of BGP niet wordt uitgevoerd.

Windows-clients hebben toegang tot VNet1 en het filiaal (Site1), maar de routes naar Site1 moeten handmatig worden toegevoegd aan de client. Niet-Windows-clients hebben toegang tot VNet1, evenals de lokale Site1.

![Routering met een VNet en een filiaal](./media/vpn-gateway-about-point-to-site-routing/5.jpg "routering met een VNet en een filiaal")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes die worden toegevoegd aan Windows-clients: 10.1.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows-clients hebben toegang tot alleen VNet1

* Niet-Windows-clients kunnen alleen VNet1 benaderen

## <a name="vnetbranchbgp"></a>Een VNet en een filiaal (BGP)

In dit voorbeeld is de punt-naar-Site VPN-gateway-verbinding voor VNet1. VNet1 is niet aangesloten of brengen met een ander virtueel netwerk, maar is verbonden met een lokale site (Site1) via een Site-naar-Site VPN-verbinding met BGP.

Windows-clients hebben toegang tot het VNet en het filiaal (Site1), maar de routes naar Site1 moeten handmatig worden toegevoegd aan de client. Niet-Windows-clients hebben toegang tot het VNet, evenals het lokale filiaal.

![een VNet en een filiaal (BGP)](./media/vpn-gateway-about-point-to-site-routing/6.jpg "een VNet en een filiaal")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes die worden toegevoegd aan Windows-clients: 10.1.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Windows-clients hebben toegang tot VNet1 en Site1, maar de routes naar Site1 moeten handmatig worden toegevoegd.

* Niet-Windows-clients hebben toegang tot VNet1 en Site1.


## <a name="multivnets2sbranch"></a>Meerdere VNets met elkaar verbonden via S2S en een filiaal

In dit voorbeeld is de punt-naar-Site VPN-gateway-verbinding voor VNet1. VNet1 is verbonden met VNet2 met behulp van een Site-naar-Site VPN-verbinding. VNet2 is verbonden met VNet3 met behulp van een Site-naar-Site VPN-verbinding. Er is geen directe peering of Site-naar-Site VPN-tunnel tussen de VNet1 en VNet3 netwerken. VNet3 is verbonden met een filiaal (Site1) met behulp van een Site-naar-Site VPN-verbinding. Alle VPN-verbindingen BGP niet worden uitgevoerd.

Alle clients toegang tot VNet1 alleen.

![Multi-VNet S2S en branch office](./media/vpn-gateway-about-point-to-site-routing/7.jpg "multi-VNet S2S en branch office")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes toevoegen clients: 10.1.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* De Windows-clients alleen toegang kunnen krijgen tot VNet1

* Niet-Windows-clients kunnen alleen VNet1 benaderen

## <a name="multivnets2sbranchbgp"></a>Meerdere VNets met elkaar verbonden via S2S en een filiaal (BGP)

In dit voorbeeld is de punt-naar-Site VPN-gateway-verbinding voor VNet1. VNet1 is verbonden met VNet2 met behulp van een Site-naar-Site VPN-verbinding. VNet2 is verbonden met VNet3 met behulp van een Site-naar-Site VPN-verbinding. Er is geen directe peering of Site-naar-Site VPN-tunnel tussen de VNet1 en VNet3 netwerken. VNet3 is verbonden met een filiaal (Site1) met behulp van een Site-naar-Site VPN-verbinding. Alle VPN-verbindingen BGP niet worden uitgevoerd. Alle VPN-verbindingen worden BGP uitgevoerd.

Clients met behulp van Windows toegang heeft tot VNets en sites die zijn verbonden via een Site-naar-Site VPN-verbinding, maar de routes naar VNet2, VNet3 en Site1 moeten handmatig worden toegevoegd aan de client. Niet-Windows-clients hebben toegang tot VNets en sites die zijn verbonden via een Site-naar-Site VPN-verbinding zonder handmatige tussenkomst. De toegang is transitieve en clients hebben toegang tot bronnen in de verbinding van alle VNets en sites (op locatie).

![Multi-VNet S2S en branch office](./media/vpn-gateway-about-point-to-site-routing/8.jpg "multi-VNet S2S en branch office")

### <a name="address-space"></a>Adresruimte

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Routes die zijn toegevoegd

* Routes toevoegen clients: 10.1.0.0/16, 192.168.0.0/24

* Routes die zijn toegevoegd aan niet-Windows-clients: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* De Windows-clients hebben toegang tot VNet1, VNet2 VNet3 en Site1, maar de routes naar VNet2, VNet3 en Site1 moeten handmatig worden toegevoegd aan de client.

* Niet-Windows-clients hebben toegang tot VNet1, Vnet2 VNet3 en Site1.

## <a name="next-steps"></a>Volgende stappen

Zie [P2S-VPN met de Azure portal maken](vpn-gateway-howto-point-to-site-resource-manager-portal.md) om te beginnen met het maken van uw P2S-VPN.
