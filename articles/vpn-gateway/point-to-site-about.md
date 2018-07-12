---
title: Over Azure Point-to-Site VPN-verbindingen | Microsoft Docs
description: Dit artikel helpt u inzicht in de punt-naar-Site-verbindingen en kunt u bepalen welk verificatietype voor P2S-VPN-gateway te gebruiken.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/06/2018
ms.author: cherylmc
ms.openlocfilehash: 2668d92b5b933f7ccf8ebcccbe7ea77ea6ea1e86
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38721425"
---
# <a name="about-point-to-site-vpn"></a>Over punt-naar-Site-VPN

Met een point-to-site-VPN-gatewayverbinding (P2S) kunt u vanaf een afzonderlijke clientcomputer een beveiligde verbinding maken met uw virtuele netwerk. Een P2S-verbinding wordt tot stand gebracht door deze te starten vanaf de clientcomputer. Deze oplossing is handig voor telewerkers die verbinding willen maken met een Azure VNet vanaf een externe locatie, zoals vanaf thuis of een conferentie. P2S-VPN is ook een uitstekende oplossing in plaats van een S2S-VPN wanneer u maar een paar clients hebt die verbinding moeten maken met een VNet. Dit artikel is van toepassing op het Resource Manager-implementatiemodel.

## <a name="protocol"></a>Welk protocol maakt gebruik van P2S?

Punt-naar-site VPN kunt gebruiken dat een van de volgende protocollen:

* Secure Socket Tunneling Protocol (SSTP), een eigen VPN op basis van een SSL-protocol. Een SSL VPN-oplossing kan passeren, firewalls, omdat de meeste firewalls open TCP-poort 443, dat gebruikmaakt van SSL. SSTP wordt alleen ondersteund op Windows-apparaten. Azure ondersteunt alle versies van Windows die SSTP (Windows 7 en hoger) hebben.

* IKEv2 VPN, een op standaarden gebaseerde IPsec VPN-oplossing. IKEv2 VPN kan worden gebruikt om verbinding te maken vanaf Mac-apparaten (OSX-versie 10.11 en hoger).

Als u een gemengde clientomgeving met Windows en Mac-apparaten hebt, configureert u zowel SSTP als IKEv2.

>[!NOTE]
>IKEv2 voor P2S is beschikbaar voor het Resource Manager-implementatiemodel alleen. Het is niet beschikbaar voor het klassieke implementatiemodel.
>

## <a name="authentication"></a>Hoe worden de P2S-VPN-clients geverifieerd?

Voordat u Azure accepteert een P2S-VPN-verbinding, is de gebruiker moet eerst worden geverifieerd. Er zijn twee methoden die Azure biedt om te verifiëren van een gebruiker verbinding maakt.

### <a name="authenticate-using-native-azure-certificate-authentication"></a>Verifiëren met behulp van systeemeigen Azure-certificaatverificatie

Wanneer u de systeemeigen Azure certificaatverificatie gebruikt, wordt een clientcertificaat dat aanwezig is op het apparaat wordt gebruikt voor verificatie van de gebruiker die verbinding maakt. Clientcertificaten worden gegenereerd op basis van een vertrouwd basiscertificaat en geïnstalleerd op elke clientcomputer. U kunt een basiscertificaat dat is gegenereerd met behulp van een bedrijfsoplossing gebruiken of u kunt een zelfondertekend certificaat genereren.

De validatie van het clientcertificaat wordt uitgevoerd door de VPN-gateway en er gebeurt tijdens het tot stand brengen van de P2S-VPN-verbinding. Het basiscertificaat voor de validatie is vereist en moet worden geüpload naar Azure.

### <a name="authenticate-using-active-directory-ad-domain-server"></a>Verifiëren met behulp van Active Directory (AD) Domain-Server

Verificatie van AD-domeinen kan gebruikers verbinding maken met Azure met de domeinreferenties van hun organisatie. Hiervoor een RADIUS-server die is geïntegreerd met de AD-server. Organisaties kunnen ook gebruikmaken van hun bestaande RADIUS-implementatie.   
  De RADIUS-server kan worden on-premises geïmplementeerd of in uw Azure VNET. Tijdens de verificatie, wordt de Azure VPN-Gateway fungeert als een pass through- en stuurt verificatieberichten heen en weer tussen de RADIUS-server en het verbindende apparaat. Bereikbaarheid van de Gateway naar de RADIUS-server is dus belangrijk. Als de RADIUS-server kan de huidige on-premises, klikt u vervolgens is een S2S VPN-verbinding tussen Azure en de on-premises site vereist voor bereikbaarheid.  
  De RADIUS-server kan ook worden geïntegreerd met AD certificaatservices. Hiermee kunt u de RADIUS-server en de implementatie van uw enterprise-certificaat voor P2S-certificaatverificatie gebruiken als alternatief voor de Azure-certificaatverificatie. Het voordeel is dat u hoeft niet aan de ingetrokken certificaten en basiscertificaten uploaden naar Azure.

Een RADIUS-server kan ook worden geïntegreerd met andere systemen externe id. Hiermee opent u tal van verificatieopties voor P2S VPN, met inbegrip van opties voor meervoudige.

! [punt-naar-site]] (./media/point-to-site-about/p2s.png "Punt-naar-Site")

## <a name="what-are-the-client-configuration-requirements"></a>Wat zijn de vereisten van de configuratie van client?

>[!NOTE]
>Voor Windows-clients, moet u beheerdersrechten hebben op de client-apparaat om te starten van de VPN-verbinding van de client-apparaat naar Azure.
>

Gebruikers gebruiken de systeemeigen VPN-clients op Windows en Mac-apparaten voor P2S. Azure biedt een VPN-client configuration zip-bestand met instellingen die vereist zijn door deze systeemeigen clients verbinding maken met Azure.

* Voor Windows-apparaten, wordt de configuratie van de VPN-client bestaat uit een installer-pakket dat gebruikers op hun apparaten installeren.
* Voor Mac-apparaten bestaat uit het mobileconfig-bestand dat gebruikers op hun apparaten installeren.

Het zip-bestand bevat ook de waarden van een aantal belangrijke instellingen op de Azure-kant die u gebruiken kunt om uw eigen profiel voor deze apparaten te maken. Enkele van de waarden zijn het adres van de VPN-gateway, geconfigureerde tunneltypen, routes en het basiscertificaat voor validatie van de gateway.

>[!NOTE]
>[!INCLUDE [TLS version changes](../../includes/vpn-gateway-tls-change.md)]
>

## <a name="gwsku"></a>Welke Gateway-SKU's ondersteuning voor P2S VPN?

[!INCLUDE [p2s-skus](../../includes/vpn-gateway-table-point-to-site-skus-include.md)]

* De Benchmark cumulatieve doorvoer is gebaseerd op de metingen van meerdere tunnels die via één gateway worden gecombineerd. Het is niet een gegarandeerde doorvoer vanwege de omstandigheden van internetverkeer en het gedrag van uw toepassing.
* Informatie over prijzen vindt u op de pagina met prijzen 
* SLA (Service Level Agreement) informatie kan worden gevonden op de SLA-pagina.

>[!NOTE]
>De basis-SKU biedt geen ondersteuning voor IKEv2- of RADIUS-verificatie.
>

## <a name="configure"></a>Hoe configureer ik een P2S-verbinding?

Een P2S-configuratie is van een groot aantal specifieke stappen vereist. De volgende artikelen bevatten de stappen om u te helpen u bij de configuratie van P2S en koppelingen naar de VPN-client-apparaten configureren:

* [De verbinding van een P2S - RADIUS-verificatie configureren](point-to-site-how-to-radius-ps.md)

* [Een P2S-verbinding - Azure systeemeigen verificatie configureren](vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="faqcert"></a>Veelgestelde vragen over de systeemeigen Azure-certificaatverificatie

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="faqradius"></a>Veelgestelde vragen over de RADIUS-verificatie

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Volgende stappen

* [De verbinding van een P2S - RADIUS-verificatie configureren](point-to-site-how-to-radius-ps.md)

* [Een P2S-verbinding - Azure systeemeigen verificatie configureren](vpn-gateway-howto-point-to-site-rm-ps.md)