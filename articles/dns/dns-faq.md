---
title: Veelgestelde vragen over Azure DNS | Microsoft Docs
description: Veelgestelde vragen over Azure DNS
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2017
ms.author: kumud
ms.openlocfilehash: e0eb39ced1d88d2e0b6128493304f112f9c685fa
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="azure-dns-faq"></a>Veelgestelde vragen over Azure DNS

## <a name="about-azure-dns"></a>Over Azure DNS

### <a name="what-is-azure-dns"></a>Wat is Azure DNS?

Het Domain Name System en DNS, is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een website of -service het IP-adres. Azure DNS is een hosting service voor DNS-domeinen omzetten van namen met behulp van Microsoft Azure-infrastructuur. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services.

DNS-domeinen in Azure DNS worden gehost op Azure wereldwijd netwerk van DNS-naamservers. Dit maakt gebruik van Anycast networking zodat elke DNS-query wordt beantwoord door de dichtstbijzijnde beschikbare DNS-server. Azure DNS biedt snelle prestaties en maximale beschikbaarheid voor uw domein.

De Azure DNS-service is gebaseerd op Azure Resource Manager. Als zodanig voordelen van Resource Manager-functies zoals rollen gebaseerd toegangsbeheer controlelogboeken en resource vergrendelen. Uw domeinen en records kunnen worden beheerd via de Azure-portal, Azure PowerShell-cmdlets en de platformoverschrijdende Azure CLI. Toepassingen waarbij automatisch DNS-beheer kunnen integreren met de service via de REST-API en SDK's.

### <a name="how-much-does-azure-dns-cost"></a>Wat kost Azure DNS?

Het facturering model van Azure DNS is gebaseerd op het aantal DNS-zones die worden gehost in Azure DNS en het aantal DNS-query's die ze ontvangen. Kortingen zijn beschikbaar op basis van gebruik.

Zie voor meer informatie de [Azure DNS pagina met prijzen](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Wat is de SLA voor Azure DNS?

Azure zorgt ervoor dat geldige DNS-aanvragen een reactie van ten minste één Azure DNS-naamserver ten minste 99,99% van de tijd ontvangen.

Zie voor meer informatie de [Azure DNS-SLA pagina](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>Wat is een DNS-zone? Is dat hetzelfde als een DNS-domein? 

Een domein is een unieke naam in het domain name system, bijvoorbeeld 'contoso.com'.


Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Het domein 'contoso.com' kan bijvoorbeeld meerdere DNS-records, zoals 'mail.contoso.com' (voor een e-mailserver) en 'www.contoso.com' (voor een website) bevatten. Deze records zou worden gehost in de DNS-zone 'contoso.com'.

Is een domeinnaam *alleen een naam*, terwijl een DNS-zone is een gegevensbron met de DNS-records voor een domeinnaam. Met Azure DNS kunt u een DNS-zone hosten en de DNS-records voor een domein in Azure beheren. Bovendien wordt DNS-naamservers om te reageren op DNS-query's van het Internet.

### <a name="do-i-need-to-purchase-a-dns-domain-name-to-use-azure-dns"></a>Moet ik een DNS-domeinnaam aanschaffen om Azure DNS te kunnen gebruiken? 

Dat hoeft niet.

U hoeft geen domein aan te schaffen om een DNS-zone in Azure DNS te hosten. U kunt altijd een DNS-zone maken zonder dat u eigenaar van een domeinnaam bent. DNS-query's voor deze zone wordt alleen omgezet als ze worden omgeleid naar de Azure DNS-naamservers toegewezen aan de zone.

U moet de domeinnaam aanschaffen als u wilt koppelen van uw DNS-zone in de globale DNS-hiërarchie – met deze koppeling kunt DNS-query's overal ter wereld te vinden van de DNS-zone en met de DNS-records worden beantwoord.

## <a name="azure-dns-features"></a>Azure DNS-functies

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Biedt ondersteuning voor Azure DNS DNS-verkeer routeren of eindpunt failover?

Routering en eindpunt failover van verkeer op basis van DNS worden geleverd door Azure Traffic Manager. Azure Traffic Manager is een afzonderlijke Azure-service die kan worden gebruikt samen met de Azure DNS. Zie voor meer informatie de [Traffic Manager-overzicht](../traffic-manager/traffic-manager-overview.md).

Azure DNS ondersteunt alleen 'statische' DNS-domeinen, waarbij elke DNS-query voor een bepaalde DNS-record altijd hetzelfde DNS-antwoord ontvangt hosten.

### <a name="does-azure-dns-support-domain-name-registration"></a>Azure DNS biedt ondersteuning voor registratie van domeinnamen?

Nee. Azure DNS ondersteunt momenteel geen kopen van domeinnamen. Als u aanschaffen domeinen wilt, moet u een domeinnaamregistrar van derden gebruiken. De registrar brengt doorgaans een kleine jaarlijkse rekening. De domeinen kunnen vervolgens worden gehost in Azure DNS voor het beheer van DNS-records. Zie [een domein aan Azure DNS overdragen](dns-domain-delegation.md) voor meer informatie.

Domein aankoop is een functie die wordt bijgehouden in Azure achterstand. U kunt de site feedback [registreren van de ondersteuning voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-dnssec"></a>Azure DNS biedt ondersteuning voor DNSSEC?

Nee. Azure DNS biedt momenteel geen ondersteuning voor DNSSEC.

DNSSEC is een functie die wordt bijgehouden in Azure DNS achterstand. U kunt de site feedback [registreren van de ondersteuning voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Biedt ondersteuning voor Azure DNS zoneoverdrachten (AXFR/IXFR)?

Nee. Azure DNS ondersteunt momenteel geen zoneoverdrachten. DNS-zones mag [geïmporteerd in Azure DNS met Azure CLI](dns-import-export.md). DNS-records kunnen vervolgens worden beheerd de [Azure DNS-beheerportal](dns-operations-recordsets-portal.md), onze [REST-API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell-cmdlets](dns-operations-recordsets.md), of [CLI-hulpprogramma](dns-operations-recordsets-cli.md).

Zoneoverdracht is een functie die wordt bijgehouden in Azure DNS achterstand. U kunt de site feedback [registreren van de ondersteuning voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Biedt ondersteuning voor Azure DNS URL omleidingen?

Nee. URL-omleiding services zijn niet daadwerkelijk een DNS-service - ze op het niveau van HTTP in plaats van het niveau van de DNS-werkt. Sommige providers DNS te bundelen van een URL omleiden service als onderdeel van hun algehele aanbieding. Dit wordt momenteel niet ondersteund door Azure DNS.

Functie Omleidings-URL wordt op Azure DNS achterstand bijgehouden. U kunt de site feedback [registreren van de ondersteuning voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

### <a name="does-azure-dns-support-extended-ascii-encoding-8-bit-set-for-txt-recordset-"></a>Ondersteunt Azure DNS uitgebreide ASCII codering (8-bits) voor TXT-Recordset

Ja. Azure DNS ondersteunt de uitgebreide ASCII-tekencodering set voor TXT-Recordsets als u de nieuwste versie van de REST-API's van Azure, SDK's, PowerShell en CLI (versies ouder zijn dan 10-01-2017 of SDK 2.1 komen geen ondersteuning voor de uitgebreide ASCII-set) gebruikt. Als de gebruiker een tekenreeks als waarde voor een TXT-record die heeft bijvoorbeeld de uitgebreide ASCII-teken \128 (bijvoorbeeld: 'abcd\128efgh'), Azure DNS gebruikt de bytewaarde van dit teken (dit is 128) in de interne weergave. Op het moment van DNS-omzetting ook deze bytewaarde geretourneerd in het antwoord. Let ook op dat "abc" en "\097\098\099" uitwisselbaar zijn wat betreft resolutie is. 

We houden [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt) zone bestand master indeling escape regels voor TXT-records. Bijvoorbeeld: ' \' nu daadwerkelijk verlaat u alles per de RFC. Als u 'A\B' als de TXT-recordwaarde opgeeft, worden weergegeven en los als NET "AB". Als u toch de TXT-record 'A\B' met een resolutie hebben, moet u als escape voor de '\" opnieuw eenledige Geef als ' A\\B '. 

Houd er rekening mee dat deze ondersteuning momenteel niet beschikbaar voor TXT-records die zijn gemaakt op basis van de Azure-Portal is. 

## <a name="using-azure-dns"></a>Met behulp van Azure DNS

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Kan ik dezelfde host een domein met behulp van Azure DNS en een andere DNS-provider?

Ja. Azure DNS ondersteunt mede hosting domeinen met een andere DNS-services.

Om dit te doen, moet u de NS-records voor het domein (die regelen welke providers DNS ontvangen query's voor het domein) wijzigen om te verwijzen naar de naamservers van beide providers. U kunt deze NS-records op drie locaties wijzigen: in Azure DNS, de andere provider en de bovenliggende zone (meestal geconfigureerd via de domeinnaamregistrar). Zie voor meer informatie over DNS-delegering [DNS-domeindelegering](dns-domain-delegation.md).

Bovendien moet u ervoor zorgen dat de DNS-records voor het domein gesynchroniseerd tussen beide DNS-providers zijn. Azure DNS ondersteunt momenteel geen DNS-zoneoverdracht. DNS-records moeten worden gesynchroniseerd met behulp van de [Azure DNS-beheerportal](dns-operations-recordsets-portal.md), [REST-API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell-cmdlets](dns-operations-recordsets.md), of [CLI-hulpprogramma](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-four-azure-dns-name-servers"></a>Heb ik mijn domein delegeren naar alle vier Azure DNS-naamservers?

Ja. Azure DNS naamservers vier aan elke DNS-zone voor fouttolerantie isolatie en verbeterde herstelmogelijkheden toegewezen. Om in aanmerking voor de SLA voor Azure DNS, moet u uw domein delegeren naar alle vier naamservers.

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

### <a name="do-azure-dns-nameservers-resolve-over-ipv6"></a>Kan Azure DNS-Nameservers via IPv6 oplossen? 

Ja. Azure DNS-Nameservers zijn dual stack (zowel IPv4 als IPv6-adressen zijn). Als u wilt zoeken naar het IPv6-adres voor de Azure DNS nameservers toegewezen aan de DNS-zone, kunt u een hulpprogramma zoals nslookup (bijvoorbeeld `nslookup -q=aaaa <Azure DNS Nameserver>`).

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Hoe stel ik van een internationale domeinnaam (IDN) in Azure DNS

Internationale domeinnamen (IDN's) werkt door codering elke DNS-naam gebruikt '[punycode](https://en.wikipedia.org/wiki/Punycode)'. DNS-query's worden gemaakt met deze namen punycode-codering.

U kunt internationale domeinnamen (IDN's) configureren in Azure DNS door eerst de naam van de zone te converteren of RecordsetNaam naar punycode. Azure DNS ondersteunt momenteel geen ingebouwde conversie van punycode.

## <a name="private-dns"></a>Persoonlijke DNS

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

### <a name="does-azure-dns-support-private-domains"></a>Ondersteunt Azure DNS-domeinen 'private'?
Ondersteuning voor 'persoonlijke' domeinen is geïmplementeerd met de functie voor privé-Zones.  Deze functie is momenteel beschikbaar in public preview.  Persoonlijke zones worden beheerd met dezelfde hulpmiddelen als internetgerichte Azure DNS-zones, maar ze zijn alleen omgezet vanuit binnen de opgegeven virtuele netwerken.  Zie de [overzicht](private-dns-overview.md) voor meer informatie.

Persoonlijke Zones worden op dit moment niet ondersteund in de Azure portal. 

Zie voor informatie over andere interne DNS-opties in Azure, [naamomzetting voor VM's en Rolexemplaren](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="what-is-the-difference-between-registration-virtual-network-and-resolution-virtual-network-in-the-context-of-private-zones"></a>Wat is het verschil tussen het virtuele netwerk registratie en resolutie virtueel netwerk in de context van persoonlijke zones? 
U kunt virtuele netwerken koppelen aan een persoonlijke DNS-zone op twee manieren - als een virtueel netwerk registratie of als een virtueel netwerk resolutie. In beide gevallen worden virtuele machines in het virtuele netwerk kunnen worden opgelost op basis van de records in de persoonlijke zone. Echter, als u een virtueel netwerk als een virtueel netwerk voor registratie opgeeft, Azure wordt automatisch geregistreerd (dynamische registratie) DNS-records in de zone voor de virtuele machines in het virtuele netwerk. Bovendien wanneer een virtuele machine in de registratie van een virtueel netwerk wordt verwijderd, wordt Azure ook automatisch verwijderd de bijbehorende DNS-record van de gekoppelde persoonlijke zone. 

### <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>Werkt Azure persoonlijke DNS-Zones in Azure-regio's?
Ja. Persoonlijke Zones wordt voor DNS-omzetting tussen virtuele netwerken in Azure-regio's, zelfs zonder expliciet peering van de virtuele netwerken, zolang de virtuele netwerken zijn opgegeven als virtuele netwerken van de oplossing voor de persoonlijke zone ondersteund. Klanten wellicht de peer worden ingesteld voor de TCP/HTTP-verkeer van de ene regio naar een andere virtuele netwerken. 

### <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Verbinding met Internet is afkomstig van virtuele netwerken die zijn vereist voor persoonlijke Zones?
Nee. Persoonlijke Zones worden gebruikt in combinatie met virtuele netwerken en laat klanten domeinen beheren voor virtuele Machines of andere bronnen binnen en tussen virtuele netwerken. Er is geen verbinding met internet is vereist voor naamomzetting. 

### <a name="can-the-same-private-zone-be-used-for-multiple-virtual-networks-for-resolution"></a>Kan dezelfde persoonlijke Zone worden gebruikt voor meerdere virtuele netwerken voor de omzetting van? 
Ja. Klanten kunnen maximaal 10 resolutie virtuele netwerken koppelen aan een enkele persoonlijke zone.

### <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-resolution-virtual-network-to-a-private-zone"></a>Een virtueel netwerk dat bij een ander abonnement hoort kan worden toegevoegd als een virtueel netwerk voor omzetting naar een privé-Zone? 
Ja, zolang de gebruiker heeft de machtiging voor Write-bewerking op zowel de virtuele netwerken als de persoonlijke DNS-zone. Houd er rekening mee dat de machtiging schrijven kan worden toegewezen aan meerdere RBAC-rollen. De klassieke netwerk Inzender RBAC-rol heeft bijvoorbeeld schrijfmachtigingen in voor virtuele netwerken. Zie voor meer informatie over RBAC-rollen [rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md)

### <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-the-virtual-machines-are-deleted-by-the-customer"></a>DNS-records automatisch worden geregistreerd virtuele machine in een persoonlijke zone worden automatisch verwijderd wanneer de virtuele machines worden verwijderd door de klant?
Ja. Als u een virtuele machine binnen een virtueel netwerk voor inschrijving verwijdert, worden er automatisch de DNS-records die zijn geregistreerd in de zone omdat deze een virtueel netwerk registratie ze verwijderd. 

### <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-registration-virtual-network-be-deleted-manually"></a>U kunt een record automatisch worden geregistreerd virtuele machine in een persoonlijke zone (van een virtueel netwerk van de registratie) handmatig verwijderen? 
Nee. Op dit moment is zijn de virtuele machine DNS-records die automatisch worden geregistreerd in een persoonlijke zone uit een virtueel netwerk van de registratie niet zichtbaar is of worden bewerkt door klanten. U kunt wel vervangen (overschrijven) dergelijke automatisch geregistreerde DNS-records met een handmatig gemaakte DNS-record in de zone. Zie de volgende vraag en antwoord die zijn gericht op dit.

### <a name="what-happens-when-we-attempt-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-registration-virtual-network"></a>Wat gebeurt er wanneer we proberen een nieuwe DNS-record handmatig maken in een persoonlijke zone met de dezelfde hostnaam als een (automatisch worden geregistreerd) bestaande virtuele machine in een virtueel netwerk van de registratie? 
Als u probeert een nieuwe DNS-record handmatig maken in een persoonlijke zone met de dezelfde hostnaam als een bestaande (automatisch worden geregistreerd) virtuele machine in een virtueel netwerk voor registratie, mag de nieuwe DNS-record automatisch geregistreerde overschrijven record van de virtuele machine. Bovendien, als u vervolgens probeert te verwijderen van dit DNS-record handmatig uit de zone zijn gemaakt, slaagt, het verwijderen en de automatische registratie gebeurt opnieuw (de DNS-record wordt niet opnieuw worden gemaakt automatisch in de zone) zo lang als de virtuele machine nog bestaat en een privé-IP is gekoppeld. 

### <a name="what-happens-when-we-unlink-a-registration-virtual-network-from-a-private-zone--would-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-as-well"></a>Wat gebeurt er wanneer er een virtueel netwerk van de registratie van een persoonlijke zone ontkoppelen? De records automatisch worden geregistreerd virtuele machine van het virtuele netwerk worden verwijderd uit de zone ook?
Ja. Als u een virtueel netwerk (update de DNS-zone verwijderen van de bijbehorende registratie virtueel netwerk) van de registratie van een persoonlijke zone ontkoppelt, wordt Azure records automatisch worden geregistreerd virtuele machine verwijderen uit de zone. 

### <a name="what-happens-when-we-delete-a-registration-or-resolution-virtual-network-that-is-linked-to-a-private-zone--do-we-have-to-manually-update-the-private-zone-to-un-link-the-virtual-network-as-a-registration-or-resolution--virtual-network-from-the-zone"></a>Wat gebeurt er wanneer we een registratie (of resolutie) virtueel netwerk dat is gekoppeld aan een persoonlijke zone verwijderen? Beschikken wij over de persoonlijke zone handmatig bijwerken naar het virtuele netwerk van virtuele netwerk als een registratie (of resolutie) un-koppeling uit de zone?
Ja. Wanneer u een virtueel netwerk registratie (of resolutie) verwijdert zone eerst zonder het ontkoppelen van een privé, Azure, kunnen de verwijderingsbewerking kan slagen, maar het virtuele netwerk is niet automatisch niet-gekoppelde uit uw persoonlijke zone indien van toepassing. U moet het virtuele netwerk van de persoonlijke zone handmatig te ontkoppelen. Daarom wordt u aangeraden eerst ontkoppelen van het virtuele netwerk van uw persoonlijke zone voordat u het verwijdert.

### <a name="would-dns-resolution-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-contosolocal-is-linked-to-a-virtual-network"></a>DNS-omzetting met behulp van de FQDN-naam (internal.cloudapp.net) nog steeds werken, zelfs wanneer een Zone persoonlijke zou (bijvoorbeeld: contoso.local) is gekoppeld aan een virtueel netwerk? 
Ja. Persoonlijke Zones onderdeel is geen vervanging voor de standaard DNS-oplossingen met behulp van de zone Azure verschafte internal.cloudapp.net en wordt aangeboden als een aanvullende mogelijkheid of uitbreiding. Voor beide gevallen (of op Azure verschafte internal.cloudapp.net of op uw eigen Zone persoonlijke relying) wordt u aangeraden de FQDN van de zone die u oplossen wilt tegen gebruiken. 

### <a name="would-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>De DNS-achtervoegsel op virtuele machines in een gekoppelde virtuele netwerk wordt gewijzigd met die van de privé-Zone? 
Nee. Op dit moment blijft de DNS-achtervoegsel op de virtuele machines in het gekoppelde virtuele netwerk als het standaard Azure verschafte achtervoegsel (' *. internal.cloudapp.net '). U kunt dit DNS-achtervoegsel echter handmatig wijzigen op uw virtuele machines met die van de persoonlijke zone. 

### <a name="are-there-any-limitations-for-private-zones-during-this-preview"></a>Zijn er beperkingen voor persoonlijke Zones tijdens deze preview?
Ja. Tijdens de openbare Preview bestaan de volgende beperkingen:
* 1 virtuele netwerken van de registratie per persoonlijke Zone
* Resolutie van maximaal 10 virtuele netwerken per persoonlijke Zone
* Een bepaald virtueel netwerk kan slechts worden gekoppeld aan een Zone voor persoonlijke als een virtueel netwerk voor registratie
* Een bepaald virtueel netwerk kan worden gekoppeld aan maximaal 10 persoonlijke Zones als een virtueel netwerk resolutie
* Als een virtueel netwerk van de registratie is opgegeven, de DNS-records voor de virtuele machines van dit virtuele netwerk die zijn geregistreerd op de Zone persoonlijke niet worden bekeken of worden opgehaald van de Powershell/CLI/API's, maar de VM-records inderdaad zijn geregistreerd en wordt opgelost is
* Omgekeerde DNS werkt alleen voor privé-IP-adresruimte in het virtuele netwerk voor registratie
* Omgekeerde DNS voor een particuliere IP die niet is geregistreerd in de privé-Zone (bijvoorbeeld: privé-IP voor een virtuele machine in een virtueel netwerk dat is gekoppeld als een virtueel netwerk omzetten naar een persoonlijke zone) retourneert 'internal.cloudapp.net' als de DNS-achtervoegsel echter Dit achtervoegsel kan niet worden omgezet.   
* Virtueel netwerk moet leeg zijn (eenledige Er is geen virtuele machines met een NIC die is gekoppeld) wanneer in eerste instantie (eenledige voor de eerste keer) koppelen aan een Zone persoonlijke als registratie of omzetting van virtueel netwerk. Echter, het virtuele netwerk niet-lege vervolgens kan worden voor toekomstige koppelen als een virtueel netwerk van registratie of omzetting naar andere persoonlijke zones. 
* Op dit moment voorwaardelijk doorsturen niet wordt ondersteund, bijvoorbeeld voor het inschakelen van de omzetting tussen Azure en OnPrem netwerken. Zie voor documentatie op hoe klanten in dit scenario via andere methoden kunnen realiseren [naamomzetting voor VM's en Rolinstanties](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)

### <a name="are-there-any-quotas-or-limits-on-zones-or-records-for-private-zones"></a>Zijn er quota's of limieten op de Zones of Records voor persoonlijke Zones?
Er zijn afzonderlijke een onbeperkt aantal Zones toegestaan per abonnement of het aantal recordsets per Zone voor privé-Zones. Zowel openbare als persoonlijke Zones meetellen voor de algehele DNS-limieten beschreven [hier](../azure-subscription-service-limits.md#dns-limits)

### <a name="is-there-portal-support-for-private-zones"></a>Is er ondersteuning voor privé-Zones Portal?
Privé-Zones die al zijn gemaakt via niet-Portal-mechanismen (API/PowerShell/CLI/SDK's) zijn zichtbaar in de Azure portal, maar klanten wordt niet meer nieuwe persoonlijke zones maken of koppelingen met virtuele netwerken beheren. Bovendien voor virtuele netwerken die zijn gekoppeld als registratie virtuele netwerken, automatisch worden geregistreerd VM records worden niet zichtbaar vanuit de Portal. 

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over Azure DNS](dns-overview.md)
<br>
[Meer informatie over het gebruik van Azure DNS voor persoonlijke domeinen](private-dns-overview.md)
<br>
[Meer informatie over DNS-zones en records](dns-zones-records.md)
<br>
[Aan de slag met Azure DNS](dns-getstarted-portal.md)

