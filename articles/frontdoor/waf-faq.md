---
title: Azure-web application firewall - Veelgestelde vragen
description: Deze pagina vindt u antwoorden op veelgestelde vragen over Azure voordeur Service
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2019
ms.author: kumud;tyao
ms.openlocfilehash: ef021bc9a4f3cb8b49ab3b455e00dfa8dc27dc76
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58909998"
---
# <a name="frequently-asked-questions-for-azure-web-application-firewall"></a>Veelgestelde vragen over Azure-web application firewall

In dit artikel vindt u antwoorden op veelgestelde vragen over Azure-web application firewall (WAF)-functies en functionaliteit. 

## <a name="what-is-azure-waf"></a>Wat is Azure WAF?

Azure WAF is een firewall voor webtoepassingen die uw webtoepassingen tegen algemene bedreigingen, zoals SQL-injectie, cross-site scripting en andere webaanvallen beschermt. U kunt een WAF-beleid die bestaan uit een combinatie van regels voor aangepaste en beheerde toegang tot uw webtoepassingen beheren definiëren.

Een Azure-WAF-beleid kan worden toegepast voor webtoepassingen die worden gehost op Application Gateway of de voordeur Azure-services.

## <a name="what-is-waf-for-azure-front-door-service"></a>Wat is de WAF voor Azure voordeur Service? 

Azure voordeur is een zeer schaalbare, wereldwijd gedistribueerde toepassing en het netwerk voor contentlevering. Azure WAF, wanneer geïntegreerd met de voordeur, denial-of-service wordt gestopt en gerichte aanvallen van de toepassing aan de rand van de Azure-netwerk, dicht bij de bronnen van de aanval voordat uw virtuele netwerk binnenkomt biedt beveiliging zonder verlies van prestaties.

## <a name="how-will-i-be-charged-for-azure-waf-for-front-door"></a>Hoe moet ik betalen voor Azure WAF voor voordeur?
Tijdens de openbare preview WAF gebruik aan voordeur gratis zijn. Houd er rekening mee dat de voordeur kosten in rekening gebracht extra is. Zie de voordeur Service prijzen [hier](https://azure.microsoft.com/pricing/details/frontdoor/).

## <a name="does-azure-waf-support-https"></a>Biedt ondersteuning voor Azure WAF HTTPS?

Voordeur Service biedt SSL-offloading. WAF is geïntegreerd met de voordeur en een aanvraag kunt inspecteren nadat deze is ontsleuteld.

## <a name="does-azure-waf-support-ipv6"></a>Azure WAF biedt ondersteuning voor IPv6?

Ja. U kunt configureren dat IP-beperking voor IPv4 en IPv6.

## <a name="how-up-to-date-are-the-managed-rule-sets"></a>Hoe up-to-date zijn de beheerde regelsets?

We doen ons best om te blijven met het wijzigen van bedreigingen. Nadat een nieuwe regel wordt bijgewerkt, wordt deze toegevoegd aan de standaard-regel is ingesteld met een nieuwe versienummer.

## <a name="what-is-the-propagation-time-if-i-make-a-change-to-my-waf-policy"></a>Wat is de tijd doorgeven als ik een wijziging in de WAF-beleid aanbrengt?

Een WAF-beleid implementeren wereldwijd gewoonlijk duurt circa 5 minuten en vaak eerder is voltooid.

## <a name="can-waf-policies-be-different-for-different-regions"></a>Kunnen het beleid voor WAF zijn verschillen voor verschillende regio's?

Wanneer is geïntegreerd met de voordeur Service, is de WAF een globale bron. Dezelfde configuratie is van toepassing op alle voordeur locaties.
 
## <a name="how-do-i-limit-access-to-my-back-end-to-be-from-front-door-only"></a>Hoe ik de toegang beperken tot mijn back-end-to-alleen afkomstig zijn uit de voordeur?

U kunt IP-ACL (toegangsbeheerlijst) configureren in uw back-end-to-voor alleen voordeur uitgaande IP-adresbereiken toestaan en weigeren van een directe toegang vanaf Internet. Service-tags worden ondersteund voor gebruik in het virtuele netwerk. Bovendien kunt u controleren dat het veld met X-doorgestuurd-Host HTTP-header geldig voor uw webtoepassing is.




## <a name="which-azure-waf-options-should-i-choose"></a>Welke opties voor Azure WAF moet ik kiezen?

Er zijn twee opties bij het toepassen van beleid voor WAF in Azure. WAF voordeur met Azure is een wereldwijd gedistribueerde edge security-oplossing. WAF met Application Gateway is een oplossing regionale, toegewezen. Het is raadzaam om u een oplossing op basis van de vereisten van uw algehele prestaties en beveiliging. Zie voor meer informatie, [taakverdeling met de Azure-toepassing levering suite](https://docs.microsoft.com/azure/frontdoor/front-door-lb-with-azure-app-delivery-suite).


## <a name="do-you-support-same-waf-features-in-all-integrated-platforms"></a>Ondersteund dezelfde WAF-functies in alle geïntegreerde platformen?

ModSec CRS 2.2.9 en CRS 3.0 regels worden op dit moment alleen ondersteund met WAF bij Application Gateway. Frequentiebeperkende, geo-filteren en Azure beheerde regelset standaard regels worden alleen met WAF voordeur op Azure ondersteund.

## <a name="is-ddos-protection-integrated-with-front-door"></a>Is DDoS-bescherming geïntegreerd met voordeur? 

Wereldwijd worden gedistribueerd op randen van de Azure-netwerk, kan Azure voordeur absorberen en geografisch isoleren groot aanvallen. U kunt aangepaste WAF-beleid te automatisch blokkeren en de snelheid limiet HTTP (s) aanvallen die had handtekeningen maken. Meer informatie, kunt u DDoS Protection standaard op de VNet waar uw back-ends worden geïmplementeerd. Azure DDoS Protection Standard-klanten krijgen extra voordelen, waaronder kostenbescherming, SLA-garantie en toegang tot experts van DDoS Rapid Response-Team voor directe ondersteuning tijdens een aanval. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [ Azure web application firewall](waf-overview.md).
- Meer informatie over [Azure voordeur](front-door-overview.md).
