---
title: Over Azure punt-naar-Site VPN-verbindingen | Microsoft Docs
description: Dit artikel vindt u informatie over punt-naar-Site-verbindingen en kunt u bepalen welk verificatietype voor P2S-VPN-gateway te gebruiken.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 010/19/2017
ms.author: cherylmc
ms.openlocfilehash: 0e31d58de113f737a48b6d3091650226f04ec69a
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2017
---
# <a name="about-point-to-site-vpn"></a>Over punt-naar-Site VPN-

Met een point-to-site-VPN-gatewayverbinding (P2S) kunt u vanaf een afzonderlijke clientcomputer een beveiligde verbinding maken met uw virtuele netwerk. Een P2S-verbinding wordt tot stand gebracht door deze te starten vanaf de clientcomputer. Deze oplossing is handig voor telewerkers die verbinding willen maken met een Azure VNet vanaf een externe locatie, zoals vanaf thuis of een conferentie. P2S-VPN is ook een uitstekende oplossing in plaats van een S2S-VPN wanneer u maar een paar clients hebt die verbinding moeten maken met een VNet. Dit artikel is van toepassing op het Resource Manager-implementatiemodel.

## <a name="protocol"></a>Welk protocol maakt gebruik van P2S?

Punt-naar-site VPN kunt gebruiken, een van de volgende protocollen:

* Secure Socket Tunneling Protocol (SSTP), een eigen VPN op basis van een SSL-protocol. Een SSL VPN-oplossing kan firewalls, doordringen omdat de meeste firewalls open TCP-poort 443, dat gebruikmaakt van SSL. SSTP wordt alleen ondersteund op Windows-apparaten. Azure biedt ondersteuning voor alle versies van Windows die SSTP (Windows 7 en hoger) hebben.

* IKEv2 VPN, een op standaarden gebaseerde IPsec VPN-oplossing. IKEv2 VPN kan worden gebruikt om verbinding te maken vanaf Mac-apparaten (OSX-versie 10.11 en hoger).

Als u een gemengde client-omgeving die bestaan uit Windows en Mac-apparaten hebt, SSTP en IKEv2 configureren.

>[!NOTE]
>IKEv2 voor P2S is momenteel in Preview en is beschikbaar voor het Resource Manager-implementatiemodel.
>

## <a name="authentication"></a>Hoe kan ik P2S-VPN-clients geverifieerd?

Voordat u Azure accepteert een P2S-VPN-verbinding, heeft de gebruiker moet eerst worden geverifieerd. Er zijn twee methoden die Azure biedt om te verifiëren van een gebruiker verbinding maakt.

### <a name="authenticate-using-native-azure-certificate-authentication"></a>Verifiëren met behulp van systeemeigen Azure certificaatverificatie

Wanneer u de systeemeigen Azure certificaatverificatie gebruikt, wordt een clientcertificaat dat aanwezig is op het apparaat wordt gebruikt voor verificatie van de gebruiker verbinding maakt. Clientcertificaten worden gegenereerd op basis van een vertrouwd basiscertificaat en vervolgens worden geïnstalleerd op elke clientcomputer. U kunt een basiscertificaat dat is gegenereerd met een bedrijfsoplossing of kunt u een zelfondertekend certificaat genereren.

De validatie van het clientcertificaat door de VPN-gateway wordt uitgevoerd en er gebeurt tijdens het tot stand brengen van de P2S-VPN-verbinding. Het basiscertificaat voor de validatie is vereist en moet worden geüpload naar Azure. 

### <a name="authenticate-using-active-directory-ad-domain-server"></a>Verifiëren met behulp van Active Directory (AD) domeinserver

AD-domein verificatie kunnen gebruikers verbinding maken met Azure met de domeinreferenties van hun organisatie. Vereist een RADIUS-server die in combinatie met de server van AD. Organisaties kunnen gebruikmaken van hun bestaande RADIUS-implementatie.   
 De RADIUS-server kan nu geïmplementeerde on-premises of in uw Azure VNET. Tijdens de verificatie, wordt de Azure VPN-Gateway fungeert als een pass through- en verificatieberichten forwards heen en weer tussen de RADIUS-server en het verbindende apparaat. Gateway bereikbaarheid RADIUS-server is dus belangrijk. Als de RADIUS-server aanwezig op locatie, is wordt een S2S VPN-verbinding van Azure naar de lokale site vereist voor bereikbaarheid is.  
 De RADIUS-server kan ook worden geïntegreerd met AD-certificaat-services. Hiermee kunt u de RADIUS-server en de implementatie van uw enterprise-certificaat voor verificatie via certificaat P2S gebruiken als alternatief voor de verificatie van Azure. Het voordeel is dat u hoeft niet te basiscertificaten en ingetrokken certificaten uploaden naar Azure.

Een RADIUS-server kan ook worden geïntegreerd met andere systemen externe identiteit. Hiermee opent u tal van opties voor P2S-VPN, inclusief opties voor multi-factor authentication.

>[!NOTE]
>RADIUS-verificatie voor P2S is momenteel in Preview.
>

![punt-naar-site]](./media/point-to-site-about/p2s.png "Punt-naar-Site")

### <a name="configuration-requirements-for-client-devices"></a>Configuratievereisten voor clientapparaten

Gebruikers gebruiken de systeemeigen VPN-clients op Windows en Mac-apparaten voor P2S. Azure biedt een VPN-client configuration zip-bestand met instellingen die vereist zijn door deze systeemeigen clients verbinding maken met Azure.

  * Voor Windows-apparaten bestaat de configuratie van de VPN-client uit een installatiepakket dat gebruikers op hun apparaten installeren.
  * Voor Mac-apparaten bestaat uit het bestand mobileconfig die gebruikers op hun apparaten installeren.

Het zip-bestand bevat ook de waarden van een aantal belangrijke instellingen op de Azure-zijde die u gebruiken kunt om uw eigen profiel voor deze apparaten te maken. De waarden onder andere het VPN-gateway-adres, geconfigureerde tunneltypen, routes en het basiscertificaat voor de validatie van de gateway.

### <a name="which-gateway-skus-support-p2s-vpn"></a>Welke Gateway-SKU's ondersteuning P2S VPN?

[!INCLUDE [p2s-skus](../../includes/vpn-gateway-table-point-to-site-skus-include.md)]

* De Benchmark cumulatieve doorvoer is gebaseerd op de metingen van meerdere tunnels die via één gateway worden gecombineerd. Het is niet een gegarandeerde doorvoer vanwege internetverkeer en het gedrag van uw toepassingen.
* Prijsinformatie kunt u vinden op de pagina met prijzen 
* Gegevens SLA (Service Level Agreement) vindt u op de pagina SLA.

## <a name="faqcert"></a>Veelgestelde vragen over de systeemeigen Azure certificaatverificatie

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="faqradius"></a>Veelgestelde vragen over de RADIUS-verificatie

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Volgende stappen

[P2S-verbindingen - RADIUS-verificatie configureren](point-to-site-how-to-radius-ps.md)

[P2S-verbindingen - Azure systeemeigen certificaatverificatie configureren](vpn-gateway-howto-point-to-site-rm-ps.md)
