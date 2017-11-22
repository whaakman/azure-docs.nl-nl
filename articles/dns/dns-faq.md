---
title: Veelgestelde vragen over Azure DNS | Microsoft Docs
description: Veelgestelde vragen over Azure DNS
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2017
ms.author: kumud
ms.openlocfilehash: a8cff450730d05970398989ceb3e8585aefd6771
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2017
---
# <a name="azure-dns-faq"></a>Veelgestelde vragen over Azure DNS

## <a name="about-azure-dns"></a>Over Azure DNS

### <a name="what-is-azure-dns"></a>Wat is Azure DNS?

Het Domain Name System en DNS, is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een website of -service het IP-adres. Azure DNS is een hosting service voor DNS-domeinen omzetten van namen met behulp van Microsoft Azure-infrastructuur. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services.

DNS-domeinen in Azure DNS worden gehost op Azure wereldwijd netwerk van DNS-naamservers. We gebruiken Anycast networking zodat elke DNS-query wordt beantwoord door de dichtstbijzijnde beschikbare DNS-server. Dit biedt zowel snelle prestaties en hoge beschikbaarheid voor uw domein.

De Azure DNS-service is gebaseerd op Azure Resource Manager. Als zodanig voordelen van Resource Manager-functies zoals rollen gebaseerd toegangsbeheer controlelogboeken en resource vergrendelen. Uw domeinen en records kunnen worden beheerd via de Azure-portal, Azure PowerShell-cmdlets en de platformoverschrijdende Azure CLI. Toepassingen waarbij automatisch DNS-beheer kunnen integreren met de service via de REST-API en SDK's.

### <a name="how-much-does-azure-dns-cost"></a>Wat kost Azure DNS?

Het facturering model van Azure DNS is gebaseerd op het aantal DNS-zones die worden gehost in Azure DNS en het aantal DNS-query's die ze ontvangen. Kortingen zijn beschikbaar op basis van gebruik.

Zie voor meer informatie de [Azure DNS pagina met prijzen](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Wat is de SLA voor Azure DNS?

Wij garanderen dat geldige DNS-aanvragen ten minste 99,99% van de tijd een reactie zullen ontvangen van ten minste één Azure DNS-naamserver.

Zie voor meer informatie de [Azure DNS-SLA pagina](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>Wat is een DNS-zone? Is dat hetzelfde als een DNS-domein? 

Een domein is een unieke naam in het domain name system, bijvoorbeeld 'contoso.com'.

Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Het domein 'contoso.com' kan bijvoorbeeld meerdere DNS-records, zoals 'mail.contoso.com' (voor een e-mailserver) en 'www.contoso.com' (voor een website) bevatten. Deze zou worden gehost in de DNS-zone 'contoso.com'.

Is een domeinnaam *alleen een naam*, terwijl een DNS-zone is een gegevensbron met de DNS-records voor een domeinnaam. Met Azure DNS kunt u een DNS-zone hosten en de DNS-records voor een domein in Azure beheren. Bovendien wordt DNS-naamservers om te reageren op DNS-query's van het Internet.

### <a name="do-i-need-to-purchase-a-dns-domain-name-to-use-azure-dns"></a>Moet ik een DNS-domeinnaam aanschaffen om Azure DNS te kunnen gebruiken? 

Dat hoeft niet.

U hoeft geen domein aan te schaffen om een DNS-zone in Azure DNS te hosten. U kunt altijd een DNS-zone maken zonder dat u eigenaar van een domeinnaam bent. DNS-query's voor deze zone wordt alleen omgezet als ze worden omgeleid naar de Azure DNS-naamservers toegewezen aan de zone.

U moet de domeinnaam aanschaffen als u wilt koppelen van uw DNS-zone in de globale DNS-hiërarchie – Hierdoor kunnen DNS-query's vanaf elke locatie in de wereld te vinden van de DNS-zone en met de DNS-records worden beantwoord.

## <a name="azure-dns-features"></a>Azure DNS-functies

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Biedt ondersteuning voor Azure DNS DNS-verkeer routeren of eindpunt failover?

Routering en eindpunt failover van verkeer op basis van DNS worden geleverd door Azure Traffic Manager. Dit is een afzonderlijke Azure-service die kan worden gebruikt samen met de Azure DNS. Zie voor meer informatie de [Traffic Manager-overzicht](../traffic-manager/traffic-manager-overview.md).

Azure DNS ondersteunt alleen 'statische' DNS-domeinen, waarbij elke DNS-query voor een bepaalde DNS-record altijd hetzelfde DNS-antwoord ontvangt hosten.

### <a name="does-azure-dns-support-domain-name-registration"></a>Azure DNS biedt ondersteuning voor registratie van domeinnamen?

Nee. Azure DNS ondersteunt momenteel geen kopen van domeinnamen. Als u aanschaffen domeinen wilt, moet u een domeinnaamregistrar van derden gebruiken. De registrar brengt doorgaans een kleine jaarlijkse rekening. De domeinen kunnen vervolgens worden gehost in Azure DNS voor het beheer van DNS-records. Zie [een domein aan Azure DNS overdragen](dns-domain-delegation.md) voor meer informatie.

Dit is een functie die we op onze achterstand volgen. Kunt u ons feedback site [registreren van de ondersteuning voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-private-domains"></a>Ondersteunt Azure DNS-domeinen 'private'?
Ondersteuning voor 'persoonlijke' domeinen is geïmplementeerd met behulp van persoonlijke DNS-zones.  Deze functie is momenteel beschikbaar als een voorbeeld.  Persoonlijke DNS-zones worden beheerd met dezelfde hulpmiddelen als internetgerichte Azure DNS-zones, maar ze zijn alleen omgezet vanuit binnen de opgegeven virtuele netwerken.  Zie de [overzicht](private-dns-overview.md) voor meer informatie.

Zie voor informatie over andere interne DNS-opties in Azure, [naamomzetting voor VM's en Rolexemplaren](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="does-azure-dns-support-dnssec"></a>Azure DNS biedt ondersteuning voor DNSSEC?

Nee. Azure DNS biedt momenteel geen ondersteuning voor DNSSEC.

Dit is een functie die we op onze achterstand volgen. Kunt u ons feedback site [registreren van de ondersteuning voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Biedt ondersteuning voor Azure DNS zoneoverdrachten (AXFR/IXFR)?

Nee. Azure DNS ondersteunt momenteel geen zoneoverdrachten. DNS-zones mag [geïmporteerd in Azure DNS met Azure CLI](dns-import-export.md). DNS-records kunnen vervolgens worden beheerd de [Azure DNS-beheerportal](dns-operations-recordsets-portal.md), onze [REST-API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell-cmdlets](dns-operations-recordsets.md), of [CLI-hulpprogramma](dns-operations-recordsets-cli.md).

Dit is een functie die we op onze achterstand volgen. Kunt u ons feedback site [registreren van de ondersteuning voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Biedt ondersteuning voor Azure DNS URL omleidingen?

Nee. URL-omleiding services zijn niet daadwerkelijk een DNS-service - ze op het niveau van HTTP in plaats van het niveau van de DNS-werkt. Sommige providers DNS te bundelen van een URL omleiden service als onderdeel van hun algehele aanbieding. Dit wordt momenteel niet ondersteund door Azure DNS.

Deze functie wordt bijgehouden op onze achterstand. Kunt u ons feedback site [registreren van de ondersteuning voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

## <a name="using-azure-dns"></a>Met behulp van Azure DNS

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Kan ik dezelfde host een domein met behulp van Azure DNS en een andere DNS-provider?

Ja. Azure DNS ondersteunt mede hosting domeinen met een andere DNS-services.

Om dit te doen, moet u de NS-records voor het domein (die regelen welke providers DNS ontvangen query's voor het domein) wijzigen om te verwijzen naar de naamservers van beide providers. Deze NS-records moeten worden gewijzigd in 3 plaatsen: in Azure DNS, de andere provider en de bovenliggende zone (meestal geconfigureerd via de domeinnaamregistrar). Zie voor meer informatie over DNS-delegering [DNS-domeindelegering](dns-domain-delegation.md).

Bovendien moet u ervoor zorgen dat de DNS-records voor het domein gesynchroniseerd tussen beide DNS-providers zijn. Azure DNS ondersteunt momenteel geen DNS-zoneoverdracht. U moet synchroniseren van DNS-records met behulp van de [Azure DNS-beheerportal](dns-operations-recordsets-portal.md), onze [REST-API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell-cmdlets](dns-operations-recordsets.md), of [CLI-hulpprogramma](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-4-azure-dns-name-servers"></a>Heb ik mijn domein delegeren naar alle 4 Azure DNS-naamservers?

Ja. Azure DNS naamservers 4 aan elke DNS-zone voor fouttolerantie isolatie en verbeterde herstelmogelijkheden toegewezen. Als u wilt komen voor de SLA voor Azure DNS, moet u uw domein delegeren naar alle 4 naamservers.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Wat zijn de limieten voor Resourcegebruik voor Azure DNS?

De standaardlimiet van het volgende van toepassing wanneer u Azure DNS:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Kan ik een Azure DNS-zone verplaatsen tussen resourcegroepen of tussen abonnementen?

Ja. DNS-zones kunnen worden verplaatst tussen resourcegroepen of tussen abonnementen.

Er zijn geen gevolgen voor DNS-query's bij het verplaatsen van een DNS-zone. De naamservers die zijn toegewezen aan de zone blijven hetzelfde en DNS-query's worden verwerkt als normale in.

Zie voor meer informatie en instructies over het verplaatsen van DNS-zones, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>Hoe lang duurt het voor DNS-wijzigingen te laten doorvoeren?

Nieuwe DNS-zones en DNS-records zijn meestal worden deze weergegeven op de Azure DNS-naamservers snel binnen een paar seconden.

Wijzigingen in bestaande DNS-records iets langer kunnen duren, maar nog steeds moeten zijn doorgevoerd op de Azure DNS-naamservers binnen 60 seconden. In dit geval kan DNS-caching buiten Azure DNS (door DNS-clients en DNS-recursieve resolvers) ook gevolgen voor de duur van een DNS-wijziging van kracht. Deze Cacheduur kan worden beheerd met de eigenschap Time To Live (TTL) van elke recordset.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>Hoe kan ik mijn DNS-zones tegen het onbedoeld verwijderen beveiligen?

Azure DNS wordt beheerd met Azure Resource Manager en de voordelen van het toegangsbeheer-functies die Azure Resource Manager biedt. Toegangsbeheer op basis van rollen kan worden gebruikt om te bepalen welke gebruikers hebt gelezen of schrijftoegang tot de DNS-zones en -recordsets. Resource vergrendelingen kunnen ter voorkoming van onbedoelde wijziging en verwijdering van DNS-zones en -recordsets worden toegepast.

Zie voor meer informatie [Protecting DNS-Zones en Records](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Hoe stel ik SPF-records in Azure DNS

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-nameservers-resolve-over-ipv6-"></a>Kan Azure DNS-Nameservers via IPv6 oplossen? 

Ja. Azure DNS-Nameservers zijn dual stack (zowel IPv4 als IPv6-adressen zijn). Als u wilt zoeken naar het IPv6-adres voor de Azure DNS nameservers toegewezen aan de DNS-zone, kunt u een hulpprogramma zoals nslookup (bijvoorbeeld `nslookup -q=aaaa <Azure DNS Nameserver>`).

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Hoe stel ik van een internationale domeinnaam (IDN) in Azure DNS

Internationale domeinnamen (IDN's) werkt door codering elke DNS-naam gebruikt '[punycode](https://en.wikipedia.org/wiki/Punycode)'. DNS-query's worden gemaakt met deze namen punycode-codering.

U kunt internationale domeinnamen (IDN's) configureren in Azure DNS door eerst de naam van de zone te converteren of RecordsetNaam naar punycode. Azure DNS ondersteunt momenteel geen ingebouwde conversie van punycode.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over Azure DNS](dns-overview.md)
<br>
[Meer informatie over het gebruik van Azure DNS voor persoonlijke domeinen](private-dns-overview.md)
<br>
[Meer informatie over DNS-zones en records](dns-zones-records.md)
<br>
[Aan de slag met Azure DNS](dns-getstarted-portal.md)

