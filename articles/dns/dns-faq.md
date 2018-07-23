---
title: Veelgestelde vragen over Azure DNS | Microsoft Docs
description: Veelgestelde vragen over Azure DNS
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
editor: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2017
ms.author: victorh
ms.openlocfilehash: 747b2e2499a9bafcf7a7b03bc2ce144828c55c75
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39172497"
---
# <a name="azure-dns-faq"></a>Veelgestelde vragen over Azure DNS

## <a name="about-azure-dns"></a>Over Azure DNS

### <a name="what-is-azure-dns"></a>Wat is Azure DNS?

De Domain Name System of DNS is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een website of -service naar het IP-adres. Azure DNS is een hostingservice voor DNS-domeinen die naamomzetting met behulp van Microsoft Azure-infrastructuur biedt. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services.

DNS-domeinen in Azure DNS worden gehost op de wereldwijde netwerk van Azure met DNS-naamservers. Dit maakt gebruik van Anycast-netwerken zodat elke DNS-query wordt beantwoord door de dichtstbijzijnde beschikbare DNS-server. Azure DNS biedt zowel snelle prestaties en hoge beschikbaarheid voor uw domein.

De Azure DNS-service is gebaseerd op Azure Resource Manager. Zo profiteert het bedrijf van Resource Manager-functies, zoals op rollen gebaseerd toegangsbeheer, logboeken voor controle en resource vergrendelen. Uw domeinen en -records kunnen worden beheerd via de Azure portal, Azure PowerShell-cmdlets en de platformoverschrijdende Azure CLI. Toepassingen waarbij automatisch DNS-beheer kunnen integreren met de service via de REST-API en SDK's.

### <a name="how-much-does-azure-dns-cost"></a>Wat kost Azure DNS?

Het factureringsmodel van Azure DNS is gebaseerd op het aantal DNS-zones die worden gehost in Azure DNS en het aantal DNS-query's die ze ontvangen. Kortingen zijn opgegeven op basis van gebruik.

Zie voor meer informatie de [Azure DNS-pagina met prijzen](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Wat is de SLA voor Azure DNS?

Azure zorgt ervoor dat geldige DNS-aanvragen een reactie van ten minste één Azure DNS-naamserver ten minste 99,99% van de tijd ontvangen wordt.

Zie voor meer informatie de [Azure DNS-SLA-pagina](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>Wat is een DNS-zone? Is dat hetzelfde als een DNS-domein? 

Een domein is een unieke naam in het domain name system, bijvoorbeeld 'contoso.com'.


Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Het domein 'contoso.com' kan bijvoorbeeld verschillende DNS-records, zoals 'mail.contoso.com' (voor een e-mailserver) en 'www.contoso.com' (voor een website) bevatten. Deze records zou worden gehost in de DNS-zone 'contoso.com'.

De domeinnaam van een is *alleen een naam*, terwijl een DNS-zone is een Gegevensresource met de DNS-records voor de domeinnaam van een. Met Azure DNS kunt u een DNS-zone hosten en de DNS-records voor een domein in Azure beheren. Het biedt ook DNS-naamservers om te reageren op DNS-query's via Internet.

### <a name="do-i-need-to-purchase-a-dns-domain-name-to-use-azure-dns"></a>Moet ik een DNS-domeinnaam aanschaffen om Azure DNS te kunnen gebruiken? 

Dat hoeft niet.

U hoeft geen domein aan te schaffen om een DNS-zone in Azure DNS te hosten. U kunt altijd een DNS-zone maken zonder dat u eigenaar van een domeinnaam bent. DNS-query's voor deze zone wordt alleen opgelost als ze worden doorgestuurd naar de Azure DNS-naamservers toegewezen aan de zone.

U moet de domeinnaam aanschaffen als u wilt uw DNS-zone een koppeling naar de globale DNS-hiërarchie – met deze koppeling kunt DNS-query's vanaf elke locatie ter wereld te vinden van uw DNS-zone en met uw DNS-records worden beantwoord.

## <a name="azure-dns-features"></a>Azure DNS-functies

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Azure DNS biedt ondersteuning voor routering of eindpunt failover van verkeer op basis van DNS?

Routering en het eindpunt failover van verkeer op basis van DNS worden geleverd door Azure Traffic Manager. Azure Traffic Manager is een afzonderlijke Azure-service die kan worden gebruikt samen met Azure DNS. Zie voor meer informatie de [overzicht Traffic Manager](../traffic-manager/traffic-manager-overview.md).

Azure DNS ondersteunt alleen het hosten van 'statische' DNS-domeinen, waarbij elke DNS-query voor een opgegeven DNS-record altijd dezelfde DNS-antwoord is ontvangen.

### <a name="does-azure-dns-support-domain-name-registration"></a>Azure DNS biedt ondersteuning voor registratie van domeinnamen?

Nee. Azure DNS ondersteunt momenteel geen domeinnamen aanschaffen. Als u aanschaffen wilt, moet u het gebruik van een externe domeinnaamregistrar. De registrar kosten doorgaans jaarlijks een klein rekening. De domeinen kunnen vervolgens worden gehost in Azure DNS voor het beheren van DNS-records. Zie [een domein delegeren naar Azure DNS](dns-domain-delegation.md) voor meer informatie.

Domein kopen is een functie in Azure achterstand wordt bijgehouden. Kunt u de site voor gebruikersfeedback van naar [registreren van het ondersteuningsteam van het voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-dnssec"></a>Azure DNS biedt ondersteuning voor DNSSEC?

Nee. Azure DNS biedt momenteel geen ondersteuning voor DNSSEC.

DNSSEC is een functie in Azure DNS achterstand worden bijgehouden. Kunt u de site voor gebruikersfeedback van naar [registreren van het ondersteuningsteam van het voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Azure DNS biedt ondersteuning voor zoneoverdracht (AXFR/IXFR)?

Nee. Azure DNS biedt momenteel geen ondersteuning voor zoneoverdracht. DNS-zones kunnen worden [geïmporteerd in Azure DNS met behulp van de Azure CLI](dns-import-export.md). DNS-records kunnen vervolgens worden beheerd via de [Azure DNS-beheerportal](dns-operations-recordsets-portal.md), onze [REST-API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell-cmdlets](dns-operations-recordsets.md), of [CLI-hulpprogramma](dns-operations-recordsets-cli.md).

Zoneoverdracht is een functie in Azure DNS achterstand worden bijgehouden. Kunt u de site voor gebruikersfeedback van naar [registreren van het ondersteuningsteam van het voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Azure DNS biedt ondersteuning voor URL-omleidingen?

Nee. URL-omleiding services zijn niet daadwerkelijk een DNS-service - ze werken op het niveau van de HTTP-, in plaats van de DNS-niveau. Sommige providers DNS te bundelen van een URL omleiden service als onderdeel van hun algehele aanbieding. Dit wordt momenteel niet ondersteund door Azure DNS.

URL-omleiding functie wordt bijgehouden in Azure DNS achterstand. Kunt u de site voor gebruikersfeedback van naar [registreren van het ondersteuningsteam van het voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

### <a name="does-azure-dns-support-extended-ascii-encoding-8-bit-set-for-txt-recordset-"></a>Azure DNS biedt ondersteuning voor uitgebreide ASCII-codering (8-bits) instellen voor TXT-Recordset?

Ja. Azure DNS ondersteunt de uitgebreide ASCII-tekencodering set voor Recordsets, TXT, als u de nieuwste versie van de Azure REST API's, SDK's, PowerShell en CLI (versies ouder dan 10-01-2017 of SDK 2.1 kan geen ondersteuning voor de uitgebreide ASCII-verzameling). Bijvoorbeeld, als de gebruiker een tekenreeks als de waarde voor een TXT-record biedt heeft met de uitgebreide ASCII-teken \128 (bijvoorbeeld: "abcd\128efgh"), Azure DNS gebruikt de bytewaarde van dit teken (dit is 128) in de interne weergave. Op het moment van DNS-omzetting wordt ook deze bytewaarde geretourneerd in het antwoord. Let ook op dat "abc" en "\097\098\099" uitwisselbaar zijn wat betreft resolutie is. 

We volgen [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt) zone bestand master indeling escape regels voor TXT-records. Bijvoorbeeld, ' \' nu eigenlijk verlaat u alles per de RFC. Als u 'A\B' als de TXT-recordwaarde opgeeft, worden weergegeven en kunt oplossen door als alleen "AB". Als u wilt dat echt de TXT-record 'A\B' op het probleem zou moeten hebben, moet u als escape voor de '\" opnieuw, Internet Explorer Geef als ' A\\B '. 

Houd er rekening mee dat deze ondersteuning momenteel niet beschikbaar voor TXT-records gemaakt op basis van de Azure-Portal is. 

## <a name="using-azure-dns"></a>Met behulp van Azure DNS

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Kan ik mede host een domein met behulp van Azure DNS en een andere DNS-provider?

Ja. Azure DNS ondersteunt CO hosting-domeinen met een andere DNS-services.

Om dit te doen, moet u de NS-records voor het domein (die regelen welke providers DNS ontvangen een query uitgevoerd voor het domein) wijzigen om te verwijzen naar de naamservers van beide providers. U kunt deze NS-records op drie locaties: in Azure DNS, in de andere provider en in de bovenliggende zone (doorgaans geconfigureerd via de domeinnaamregistrar). Zie voor meer informatie over DNS-delegering [domein-DNS-delegering](dns-domain-delegation.md).

Bovendien moet u ervoor te zorgen dat de DNS-records voor het domein gesynchroniseerd tussen beide DNS-providers zijn. Azure DNS biedt momenteel geen ondersteuning voor DNS-zone-overdrachten. DNS-records moeten worden gesynchroniseerd met behulp van de [Azure DNS-beheerportal](dns-operations-recordsets-portal.md), [REST-API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell-cmdlets](dns-operations-recordsets.md), of [CLI-hulpprogramma](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-four-azure-dns-name-servers"></a>Heb ik mijn domein delegeren naar alle vier Azure DNS-naamservers?

Ja. Azure DNS wijst vier naamservers toe aan elke DNS-zone voor foutisolatie en verhoogde flexibiliteit. Als u wilt komen voor de SLA van Azure DNS, moet u uw domein delegeren naar alle vier de naamservers.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Wat zijn de limieten voor Azure DNS?

De volgende standaard beperkingen gelden bij het gebruik van Azure DNS:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Kan ik een Azure DNS-zone verplaatsen tussen resourcegroepen of tussen abonnementen?

Ja. DNS-zones kunnen worden verplaatst tussen resourcegroepen of abonnementen.

Er is geen invloed op de DNS-query's bij het verplaatsen van een DNS-zone. De naamservers die zijn toegewezen aan de zone blijven hetzelfde en DNS-query's worden verwerkt als normale in.

Zie voor meer informatie en instructies over het verplaatsen van de DNS-zones, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>Hoe lang duurt het voor DNS-wijzigingen worden doorgevoerd?

Nieuwe DNS-zones en DNS-records worden doorgaans weergegeven op de DNS-naamservers snel, binnen een paar seconden.

Wijzigingen in bestaande DNS-records iets langer kunnen duren, maar nog steeds binnen 60 seconden moeten worden weergegeven op de DNS-naamservers. In dit geval wordt DNS-caching buiten Azure DNS (door DNS-clients en DNS-recursieve resolvers) kan ook invloed op de tijd die nodig om een DNS-wijziging door te worden van kracht. De Cacheduur van deze kan worden beheerd met behulp van de eigenschap Time-To-Live (TTL) van elke recordset.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>Hoe kan ik mijn DNS-zones tegen onbedoeld beveiligen?

Azure DNS wordt beheerd met Azure Resource Manager en de voordelen van het toegangsbeheer-functies die Azure Resource Manager biedt. Op rollen gebaseerd toegangsbeheer kan worden gebruikt om te bepalen welke gebruikers hebt gelezen of schrijftoegang tot de DNS-zones en -recordsets. Resourcevergrendelingen kunnen worden toegepast om te voorkomen dat onbedoelde wijziging of verwijdering van DNS-zones en -recordsets.

Zie voor meer informatie, [Protecting DNS-Zones en Records](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Hoe stel ik SPF-records in Azure DNS?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-nameservers-resolve-over-ipv6"></a>Kan Azure DNS-naamservers oplossen via IPv6? 

Ja. Azure DNS-naamservers zijn dual stack (IPv4 en IPv6-adressen hebben). Als u wilt het IPv6-adres voor de Azure DNS-naamservers toegewezen aan de DNS-zone hebt gevonden, kunt u een hulpprogramma zoals nslookup (bijvoorbeeld `nslookup -q=aaaa <Azure DNS Nameserver>`).

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Hoe stel ik van een internationale domeinnaam (IDN) in Azure DNS?

Internationale domeinnamen (IDN) werken, doordat de codering van elke DNS-naam met behulp van '[punycode](https://en.wikipedia.org/wiki/Punycode)'. DNS-query's worden gemaakt met behulp van deze namen punycode-codering.

U kunt internationale domeinnamen (IDN's) configureren in Azure DNS door eerst de naam van de zone te converteren of RecordsetNaam naar punycode. Azure DNS biedt momenteel geen ondersteuning voor ingebouwde conversie naar/van punycode.

## <a name="private-dns"></a>Privé-DNS

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

### <a name="does-azure-dns-support-private-domains"></a>Azure DNS biedt ondersteuning voor 'persoonlijke' domeinen?
Ondersteuning voor 'persoonlijke' domeinen is geïmplementeerd met behulp van de functie voor Privézones.  Deze functie is momenteel beschikbaar in openbare preview.  Privézones worden beheerd met dezelfde hulpmiddelen als internetgerichte Azure DNS-zones, maar ze alleen binnen de opgegeven virtuele netwerken van oplosbaar zijn.  Zie de [overzicht](private-dns-overview.md) voor meer informatie.

Privézones worden op dit moment niet ondersteund in Azure portal. 

Zie voor informatie over andere interne DNS-opties in Azure, [naamomzetting voor VM's en Rolexemplaren](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="what-is-the-difference-between-registration-virtual-network-and-resolution-virtual-network-in-the-context-of-private-zones"></a>Wat is het verschil tussen Registration virtual network en virtuele resolutienetwerk in de context van privézones? 
U kunt virtuele netwerken koppelen aan een persoonlijke DNS-zone op twee manieren: als een Registration virtual network of als een resolutie van virtueel netwerk. In beide gevallen worden virtuele machines in het virtuele netwerk kunnen worden opgelost op basis van de records in de persoonlijke zone. Echter, als u een virtueel netwerk als een Registration virtual network opgeeft, Azure, wordt automatisch geregistreerd (dynamische registratie) DNS-records in de zone voor de virtuele machines in het virtuele netwerk. Bovendien, als een virtuele machine in de registratie van een virtueel netwerk wordt verwijderd, Azure worden ook automatisch verwijderd de bijbehorende DNS-record van de gekoppelde privézone. 

### <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>Azure DNS Private Zones werken in verschillende Azure-regio's?
Ja. Privézones wordt ondersteund voor DNS-omzetting tussen virtuele netwerken per Azure-regio, zelfs als u geen expliciet peering van de virtuele netwerken, zolang de virtuele netwerken zijn opgegeven als het probleem zou moeten virtuele netwerken voor de privézone. Klanten moet mogelijk de virtuele netwerken aan worden gekoppeld voor TCP/HTTP-verkeer van de ene regio naar een andere. 

### <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Verbinding met Internet is van virtuele netwerken die zijn vereist voor Privézones?
Nee. Private Zones worden gebruikt in combinatie met virtuele netwerken en laat klanten domeinen beheren voor virtuele Machines of andere bronnen binnen en tussen virtuele netwerken. Er is geen verbinding met internet is vereist voor naamomzetting. 

### <a name="can-the-same-private-zone-be-used-for-multiple-virtual-networks-for-resolution"></a>Kan de dezelfde Privézone worden gebruikt voor meerdere virtuele netwerken voor het omzetten van? 
Ja. Klanten kunnen maximaal 10 Resolution virtual networks koppelen aan een enkele privézone.

### <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-resolution-virtual-network-to-a-private-zone"></a>Een virtueel netwerk dat bij een ander abonnement hoort kan worden toegevoegd als een virtuele resolutienetwerk aan een Privézone? 
Ja, zolang de gebruiker heeft de bewerking schrijfmachtiging op zowel de virtuele netwerken als de privé-DNS-zone. Houd er rekening mee dat de machtiging schrijven kan worden toegewezen aan meerdere RBAC-rollen. De klassieke netwerk Inzender RBAC-rol heeft bijvoorbeeld schrijfmachtigingen voor de virtuele netwerken. Zie voor meer informatie over RBAC-rollen [rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md)

### <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-the-virtual-machines-are-deleted-by-the-customer"></a>De DNS-records automatisch worden geregistreerd virtuele machine in een privé-zone worden automatisch verwijderd wanneer de virtuele machines worden verwijderd door de klant?
Ja. Als u een virtuele machine binnen een virtueel netwerk voor inschrijving verwijdert, wordt de DNS-records die zijn geregistreerd in de zone omdat deze een virtueel netwerk van de registratie wordt automatisch verwijderd. 

### <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-registration-virtual-network-be-deleted-manually"></a>Kan een record automatisch worden geregistreerd virtuele machine in een privé-zone (van een virtueel netwerk van de registratie) handmatig worden verwijderd? 
Nee. Op dit moment zijn de virtuele machine DNS-records die automatisch worden geregistreerd bij een privézone vanuit een virtueel netwerk van registratie niet zichtbaar of bewerkt door klanten. U kunt echter vervangen (overschrijven) die automatisch worden geregistreerd DNS-records met een handmatig gemaakte DNS-record in de zone. Zie de volgende vragen en antwoorden die dit corrigeert.

### <a name="what-happens-when-we-attempt-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-registration-virtual-network"></a>Wat gebeurt er als u nu probeert een nieuwe DNS-record handmatig maken in een privé-zone met de dezelfde hostnaam als een bestaande virtuele-machine (automatisch worden geregistreerd) in een virtueel netwerk registratie? 
Als u probeert een nieuwe DNS-record handmatig maken in een privé-zone met de dezelfde hostnaam als een bestaande virtuele-machine (automatisch worden geregistreerd) in een virtueel netwerk van de registratie, kunnen we de nieuwe DNS-record te overschrijven de automatisch worden geregistreerd record van de virtuele machine. Bovendien, als u vervolgens probeert naar deze DNS-record handmatig worden gemaakt van de zone verwijdert, de verwijdering is wel mogelijk en de automatische registratie opnieuw wordt uitgevoerd (de DNS-record worden opnieuw gemaakt automatisch in de zone) zo lang als de virtuele machine nog steeds bestaat en een privé-IP heeft gekoppeld. 

### <a name="what-happens-when-we-unlink-a-registration-virtual-network-from-a-private-zone--would-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-as-well"></a>Wat gebeurt er wanneer we een virtueel netwerk van de registratie van een privézone ontkoppelen? Zou u de records automatisch worden geregistreerd virtuele machine van het virtuele netwerk verwijderd uit de zone ook?
Ja. Als u een virtueel netwerk (update de DNS-zone te verwijderen van het bijbehorende Registration virtual network) van registratie van een privézone ontkoppelt, worden alle records die automatisch worden geregistreerd virtuele machine in Azure verwijderen uit de zone. 

### <a name="what-happens-when-we-delete-a-registration-or-resolution-virtual-network-that-is-linked-to-a-private-zone--do-we-have-to-manually-update-the-private-zone-to-un-link-the-virtual-network-as-a-registration-or-resolution--virtual-network-from-the-zone"></a>Wat gebeurt er wanneer we een inschrijving (of het probleem zou moeten) virtueel netwerk dat is gekoppeld aan een privézone verwijderen? Hebben we de privézone handmatig bijwerken naar het virtuele netwerk van virtuele netwerk bevinden als een registratie (of het probleem zou moeten) ongedaan maken-koppeling van de zone?
Ja. Wanneer u een virtueel netwerk inschrijving (of het probleem zou moeten) verwijderen zonder het ontkoppelen van een private zone eerst, Azure kunt de verwijderingsbewerking te voltooien, maar het virtuele netwerk is niet automatisch niet-gekoppelde afkomstig van uw privé-zone indien van toepassing. U moet het virtuele netwerk van de persoonlijke zone handmatig verwijderen van de koppeling. Daarom is het raadzaam eerst ontkoppelen van het virtuele netwerk van uw persoonlijke zone voordat deze worden verwijderd.

### <a name="would-dns-resolution-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-contosolocal-is-linked-to-a-virtual-network"></a>DNS-omzetting met behulp van de FQDN-naam (internal.cloudapp.net) nog steeds werken, zelfs wanneer een Privézone wilt (bijvoorbeeld: contoso.local) is gekoppeld aan een virtueel netwerk? 
Ja. Functie voor Privézones is geen vervanging voor de standaard-DNS-oplossingen met behulp van de zone Azure verschafte internal.cloudapp.net en wordt aangeboden als een aanvullende mogelijkheden of uitbreiding. Voor beide gevallen (of op Azure verschafte internal.cloudapp.net of op uw eigen Privézone relying) adviseren u om de FQDN-naam van de zone die u wilt oplossen op basis van gebruik. 

### <a name="would-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>Zou het DNS-achtervoegsel op virtuele machines in een gekoppeld virtueel netwerk worden gewijzigd met die van de privé-Zone? 
Nee. Op dit moment is het DNS-achtervoegsel op de virtuele machines in het gekoppelde virtuele netwerk als het standaard-Azure verschafte achtervoegsel blijft ("*. internal.cloudapp.net '). U kunt dit DNS-achtervoegsel echter handmatig wijzigen op uw virtuele machines met die van de privézone. 

### <a name="are-there-any-limitations-for-private-zones-during-this-preview"></a>Zijn er beperkingen voor Privézones tijdens deze previewperiode?
Ja. Tijdens de openbare Preview bestaat de volgende beperkingen:
* 1 registration virtual networks per Privézone
* Resolutie van maximaal 10 virtuele netwerken per Privézone
* Een bepaald virtueel netwerk kan alleen worden gekoppeld aan een Privézone als een Registration virtual network
* Een bepaald virtueel netwerk kan worden gekoppeld aan maximaal 10 Privézones als een virtuele resolutienetwerk
* Als een virtueel netwerk van de registratie is opgegeven, de DNS-records voor de virtuele machines van dit virtuele netwerk die zijn geregistreerd bij de Privézone zich niet kan worden weergegeven of worden opgehaald uit de Powershell/CLI/API's, maar de records van de virtuele machine inderdaad worden geregistreerd en wordt opgelost is
* Reverse-DNS werkt alleen voor privé-IP-adresruimte in het Registration virtual network
* Reverse-DNS voor een privé-IP dat is niet geregistreerd in de Privézone (bijvoorbeeld: privé-IP voor een virtuele machine in een virtueel netwerk dat is gekoppeld als een virtuele resolutienetwerk aan een privézone) retourneert 'internal.cloudapp.net' als de DNS-achtervoegsel, maar Dit achtervoegsel wordt niet worden omgezet.   
* Virtueel netwerk moet leeg zijn (zoals) Er zijn geen virtuele machines met een NIC die is gekoppeld) wanneer in eerste instantie (zoals) voor de eerste keer) koppelen aan een Privézone als registratie- of Resolutienetwerken virtuele netwerk. Het virtuele netwerk kan echter vervolgens worden niet-lege voor toekomstige koppelen als een virtueel netwerk van registratie- of Resolutienetwerken naar andere private zones. 
* Op dit moment voorwaardelijk doorsturen niet wordt ondersteund, bijvoorbeeld voor het inschakelen van het probleem zou moeten tussen Azure en on-premises netwerken. Zie voor documentatie over hoe klanten van dit scenario via andere methoden profiteren kunnen, [naamomzetting voor virtuele machines en rollen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)

### <a name="are-there-any-quotas-or-limits-on-zones-or-records-for-private-zones"></a>Zijn er quota's of limieten voor Zones of Records voor Privézones?
Er zijn geen afzonderlijke limieten voor het aantal Zones toegestaan per abonnement of aantal recordsets per Zone voor Privézones. Zowel openbare en Private Zones tellen mee voor de algemene DNS-limieten beschreven [hier](../azure-subscription-service-limits.md#dns-limits)

### <a name="is-there-portal-support-for-private-zones"></a>Is er ondersteuning voor Privézones Portal?
Privézones die al zijn gemaakt via niet-Portal-mechanismen (API/PowerShell/CLI/SDK's) worden weergegeven op de Azure-portal, maar klanten wordt geen nieuwe private zones maken of beheren van koppelingen met virtuele netwerken. Bovendien voor virtuele netwerken die zijn gekoppeld als Registration virtual networks, zich records automatisch worden geregistreerd VM niet zichtbaar zijn vanaf de Portal. 

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over Azure DNS](dns-overview.md)
<br>
[Meer informatie over het gebruik van Azure DNS voor persoonlijke domeinen](private-dns-overview.md)
<br>
[Meer informatie over DNS-zones en records](dns-zones-records.md)
<br>
[Aan de slag met Azure DNS](dns-getstarted-portal.md)

