---
title: Over VPN-gateway voor Azure-Stack | Microsoft Docs
description: Meer informatie over en configureren van VPN-gateways die u met Azure-Stack gebruiken.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 0e30522f-20d6-4da7-87d3-28ca3567a890
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/01/2017
ms.author: brenduns
ms.openlocfilehash: ba9642d8c51f57623aded44b84d7127334806bc1
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
---
# <a name="about-vpn-gateway-for-azure-stack"></a>Over VPN-gateway voor Azure-Stack
*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*


Voordat u netwerkverkeer tussen uw virtuele Azure-netwerk en uw on-premises site verzenden kunt, moet u een virtuele netwerkgateway maken voor het virtuele netwerk.

Een VPN-gateway is een soort gateway voor virtuele netwerken die versleuteld verkeer verzendt via een openbare verbinding. U kunt VPN-gateways gebruiken om verkeer veilig tussen een virtueel netwerk in Azure-Stack en een virtueel netwerk in Azure of tussen een virtueel netwerk en een ander netwerk dat is verbonden met een VPN-apparaat te verzenden.

Wanneer u een virtuele netwerkgateway maakt, geeft u het gatewaytype aan dat u wilt maken. Azure Stack ondersteunt één type van de virtuele netwerkgateway: het type 'VPN-'.

Elk virtueel netwerk kan twee virtuele netwerkgateways hebben, maar slechts één van elk type. Afhankelijk van de instellingen die u kiest, kunt u meerdere verbindingen maken voor één VPN-gateway. Een voorbeeld hiervan is een configuratie met meerdere siteverbindingen.

> [!NOTE]
> In Azure, moet de doorvoer bandbreedte voor VPN-Gateway-SKU gewenst worden verdeeld over alle verbindingen die zijn verbonden.  De waarde voor de bandbreedte voor de VPN-Gateway-SKU wordt in Azure-Stack toegepast op elke verbindingsbron die is verbonden met het.     

> Bijvoorbeeld, In Azure, de Basic VPN-Gateway-SKU aankan ongeveer 100 Mbps van geaggregeerde doorvoer.  Als u twee verbindingen met deze VPN-Gateway maken en één verbinding met behulp van 50 Mbps bandbreedte, vervolgens 50 Mbps is beschikbaar voor de andere verbinding.   

> In Azure-Stack *elke* verbinding met de Basic VPN-Gateway-SKU 100 Mbps van doorvoer wordt toegewezen.

## <a name="configuring-a-vpn-gateway"></a>Een VPN-gateway configureren
Een VPN-gatewayverbinding is afhankelijk van meerdere resources die zijn geconfigureerd met specifieke instellingen. De meeste resources kunnen afzonderlijk worden geconfigureerd, hoewel ze in sommige gevallen in een bepaalde volgorde moeten worden geconfigureerd.

### <a name="settings"></a>Instellingen
De instellingen die u voor elke resource hebt gekozen, zijn essentieel om een geslaagde verbinding te maken. Zie voor meer informatie over afzonderlijke bronnen en instellingen voor VPN-Gateway [over VPN-gateway-instellingen voor Azure-Stack](azure-stack-vpn-gateway-settings.md). Hier vindt u informatie om te begrijpen gatewaytypen, VPN-typen, verbindingstypen, gateway-subnetten, lokale netwerkgateways en diverse andere resource-instellingen kunt u overwegen.

### <a name="deployment-tools"></a>Implementatiehulpmiddelen
U kunt beginnen met het maken en configureren van resources met een configuratiehulpprogramma, zoals Azure Portal. U kunt later alsnog besluiten over te schakelen naar een ander hulpprogramma, zoals PowerShell, om aanvullende resources te configureren, of om desgewenst bestaande bronnen te wijzigen. Op dit moment is het niet mogelijk om elke resource en resource-instelling in Azure Portal te configureren. De instructies in de artikelen voor elke verbindingstopologie geven aan of een specifiek confihuratiehulpprogramma nodig is.

## <a name="connection-topology-diagrams"></a>Topologie gatewayverbindingsdiagrammen
Het is belangrijk te weten dat er verschillende configuraties beschikbaar zijn voor VPN-gatewayverbindingen. U moet bepalen welke configuratie het beste aansluit bij uw behoeften. In de gedeelten hieronder kunt u informatie en topologiediagrammen over de volgende VPN-gatewayverbindingen bekijken. In de tabellen worden de volgende zaken weergegeven:

- Beschikbaar implementatiemodel
- Beschikbare configuratiehulpprogramma's
- Rechtstreekse koppelingen naar een artikel, indien beschikbaar

De diagrammen en beschrijvingen in de volgende secties kunt u een verbinding-topologie voor uw vereisten selecteert. De diagrammen tonen de belangrijkste basistopologieën, maar het is mogelijk om met de diagrammen als richtlijn complexere configuraties te bouwen.

## <a name="site-to-site-and-multi-site-ipsecike-vpn-tunnel"></a>Site-naar-site en multi-site (IPsec-/IKE VPN-tunnel)
### <a name="site-to-site"></a>Site-naar-site
Een site-naar-site-VPN-gatewayverbinding (S2S) is een verbinding via een VPN-tunnel met IPsec/IKE (IKEv1 of IKEv2). Voor dit type verbinding moet er een VPN-apparaat on-premises aanwezig zijn waaraan een openbaar IP-adres is toegewezen en dat zich niet achter een NAT bevindt. S2S-verbindingen kunnen worden gebruikt voor cross-premises en hybride configuraties.    
![Configuratievoorbeeld voor site-naar-site VPN-verbinding](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="multi-site"></a>Multi-site
Dit type verbinding is een variatie op de site-naar-site-verbinding. U maakt meer dan één VPN-verbinding vanaf uw virtuele netwerkgateway, meestal met verschillende on-premises sites. Als u met meerdere verbindingen werkt, moet u een op een route gebaseerd VPN-type (ook bekend als een dynamische gateway voor klassieke VNets) gebruiken. Omdat elk virtueel netwerk maar één VPN-gateway kan hebben, delen alle verbindingen via de gateway de beschikbare bandbreedte. Dit wordt vaak een multi-site-verbinding genoemd.   
![Voorbeeld van een verbinding tussen meerdere locaties met Azure VPN Gateway](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-multisite-connection-diagram.png)



## <a name="gateway-skus"></a>Gateway-SKU's
Wanneer u een virtuele netwerkgateway voor Azure-Stack maakt, geeft u de gateway-SKU die u wilt gebruiken. De volgende VPN-gateway-SKU's worden ondersteund:
- Basic
- Standard
- HighPerformance

Wanneer u een hogere SKU, zoals Standard via Basic of HighPerformance-gateway via Standard- of Basic selecteert, worden meer CPU's en bandbreedte van het netwerk worden toegewezen aan de gateway. De gateway kan als gevolg hiervan hoger netwerkdoorvoer aan het virtuele netwerk te ondersteunen.

Azure-Stack biedt geen ondersteuning voor de gateway UltraPerformance SKU kan uitsluitend met Express Route gebruikt.

Houd rekening met het volgende als u een SKU selecteert:
- Azure-Stack biedt geen ondersteuning voor gateways op basis van beleid.
- Border Gateway Protocol (BGP) wordt niet ondersteund op de basis-SKU.
- ExpressRoute-VPN-Gateway worden gecombineerd configuraties worden niet ondersteund in Azure-Stack
- Actief-actief S2S VPN-gatewayverbindingen kunnen alleen worden geconfigureerd op de HighPerformance SKU.

## <a name="estimated-aggregate-throughput-by-sku"></a>Geschatte geaggregeerde doorvoer per SKU
In de volgende tabel ziet u de gatewaytypen en de geschatte geaggregeerde doorvoer per gateway-SKU.

|   | Doorvoer VPN-Gateway *(1)* |Max. IPsec-tunnels VPN-Gateway |
|-------|-------|-------|
|**Basis-SKU** ***(2)***    | 100 Mbps  | 10    |
|**Standaard SKU**       | 100 Mbps  | 10    |
|**Hoge prestaties SKU** | 200 Mbps    | 30    |
***(1)***  De VPN-doorvoer is niet een gegarandeerde doorvoer voor cross-premises verbindingen via Internet. Het is een meting van de maximaal mogelijke doorvoer.  
***(2)***  BGP wordt niet ondersteund voor de basis-SKU.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [instellingen voor VPN-gateways](azure-stack-vpn-gateway-settings.md) voor Azure-Stack.
