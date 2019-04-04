---
title: Veelgestelde vragen over Azure DNS
description: Veelgestelde vragen over Azure DNS
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 3/21/2019
ms.author: victorh
ms.openlocfilehash: 4f0800dfd264059e1dc8aac32a54f216f777647f
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905712"
---
# <a name="azure-dns-faq"></a>Veelgestelde vragen over Azure DNS

## <a name="about-azure-dns"></a>Over Azure DNS

### <a name="what-is-azure-dns"></a>Wat is Azure DNS?

Domain Name System (DNS) worden omgezet of opgelost, de naam van een website of -service naar het IP-adres. Azure DNS is een hostingservice voor DNS-domeinen. Deze biedt naamomzetting met behulp van Microsoft Azure-infrastructuur. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services.

DNS-domeinen in Azure DNS worden gehost op het Azure wereldwijde netwerk van DNS-naamservers. Dit systeem maakt gebruik van Anycast-netwerken zodat elke DNS-query wordt beantwoord door de dichtstbijzijnde beschikbare DNS-server. Azure DNS biedt snelle prestaties en hoge beschikbaarheid voor uw domein.

Azure DNS is gebaseerd op Azure Resource Manager. Azure DNS-voordelen van Resource Manager-functies zoals op rollen gebaseerd toegangsbeheer, logboeken voor controle en resource vergrendelen. U kunt records via Azure portal, Azure PowerShell-cmdlets en de platformoverschrijdende Azure CLI en domeinen beheren. Toepassingen waarvoor automatische DNS-beheer kunnen integreren met de service via de REST-API en SDK's.

### <a name="how-much-does-azure-dns-cost"></a>Wat kost Azure DNS?

Het factureringsmodel van Azure DNS is gebaseerd op het aantal DNS-zones die worden gehost in Azure DNS. Het ook gebaseerd op het aantal DNS-query's die ze ontvangen. Kortingen zijn opgegeven op basis van gebruik.

Zie voor meer informatie de [Azure DNS-pagina met prijzen](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Wat is de SLA voor Azure DNS?

Azure zorgt ervoor dat geldige DNS-aanvragen een reactie van ten minste één Azure DNS-naamserver 100% van de tijd ontvangen.

Zie voor meer informatie de [Azure DNS-SLA-pagina](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>Wat is een DNS-zone? Is dat hetzelfde als een DNS-domein? 

Een domein is een unieke naam in het domain name system. Bijvoorbeeld: contoso.com.

Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Het domein contoso.com kan bijvoorbeeld verschillende DNS-records bevatten. De records advies inwinnen bij mail.contoso.com voor een e-mailserver en een www\.contoso.com voor een website. Deze records worden gehost in de DNS-zone contoso.com.

De domeinnaam van een is *alleen een naam*. Een DNS-zone is een Gegevensresource die de DNS-records voor een domeinnaam bevat. Met Azure DNS kunt u een DNS-zone hosten en de DNS-records voor een domein in Azure beheren. Het biedt ook DNS-naamservers om te reageren op DNS-query's via Internet.

### <a name="do-i-need-to-buy-a-dns-domain-name-to-use-azure-dns"></a>Moet ik een DNS-domeinnaam aanschaffen om Azure DNS te gebruiken? 

Dat hoeft niet.

U hoeft geen domein te kopen voor het hosten van een DNS-zone in Azure DNS. U kunt altijd een DNS-zone maken zonder dat u eigenaar van een domeinnaam bent. DNS-query's voor deze zone oplossen alleen als ze worden doorgestuurd naar de Azure DNS-naamservers toegewezen aan de zone.

Als u wilt uw DNS-zone een koppeling naar de globale DNS-hiërarchie, moet u de domeinnaam aanschaffen. Klik, DNS-query's vanaf elke locatie ter wereld uw DNS-zone en antwoord op met uw DNS-records vinden.

## <a name="azure-dns-features"></a>Azure DNS-functies

### <a name="are-there-any-restrictions-when-using-alias-records-for-a-domain-name-apex-with-traffic-manager"></a>Zijn er beperkingen bij het gebruik van de aliasrecords voor een domein naam apex met Traffic Manager?

Ja. Met Azure Traffic Manager, moet u statische openbare IP-adressen gebruiken. Configureer de **Extern eindpunt** doel met behulp van een statisch IP-adres. 

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Azure DNS biedt ondersteuning voor routering of eindpunt failover van verkeer op basis van DNS?

Routering en het eindpunt failover van verkeer op basis van DNS worden geleverd door Traffic Manager. Traffic Manager is een afzonderlijke Azure-service die kan worden gebruikt met Azure DNS. Zie voor meer informatie de [overzicht Traffic Manager](../traffic-manager/traffic-manager-overview.md).

Azure DNS ondersteunt alleen het hosten van statische DNS-domeinen, waarbij elke DNS-query voor een opgegeven DNS-record altijd dezelfde DNS-antwoord is ontvangen.

### <a name="does-azure-dns-support-domain-name-registration"></a>Azure DNS biedt ondersteuning voor registratie van domeinnamen?

Nee. Azure DNS ondersteunt op dit moment niet de optie voor het kopen van domeinnamen. Voor het kopen van domeinen, moet u een externe domeinnaamregistrar. De registrar kosten doorgaans jaarlijks een klein rekening. De domeinen die kunnen vervolgens worden gehost in Azure DNS voor het beheren van DNS-records. Zie [Delegate a domain to Azure DNS](dns-domain-delegation.md) (Een domein aan Azure DNS overdragen) voor meer informatie.

De functie voor het kopen van domeinnamen wordt bijgehouden in de achterstand van Azure. Gebruik van de site voor gebruikersfeedback van naar [registreren van het ondersteuningsteam van het voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-dnssec"></a>Azure DNS biedt ondersteuning voor DNSSEC?

Nee. Azure DNS ondersteunt momenteel geen Domain Name System Security Extensions (DNSSEC).

De functie DNSSEC wordt bijgehouden in de achterstand van Azure DNS. Gebruik van de site voor gebruikersfeedback van naar [registreren van het ondersteuningsteam van het voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Azure DNS biedt ondersteuning voor zoneoverdracht (AXFR/IXFR)?

Nee. Azure DNS ondersteunt momenteel geen zone-overdrachten. DNS-zones kunnen worden [geïmporteerd in Azure DNS met behulp van de Azure CLI](dns-import-export.md). DNS-records worden beheerd via de [Azure DNS-beheerportal](dns-operations-recordsets-portal.md), [REST-API](https://docs.microsoft.com/powershell/module/az.dns), [SDK](dns-sdk.md), [PowerShell-cmdlets](dns-operations-recordsets.md), of de [ CLI-hulpprogramma](dns-operations-recordsets-cli.md).

De functie zone-overdracht wordt bijgehouden in de achterstand van Azure DNS. Gebruik van de site voor gebruikersfeedback van naar [registreren van het ondersteuningsteam van het voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Azure DNS biedt ondersteuning voor URL-omleidingen?

Nee. URL-omleiding services zijn niet van een DNS-service. Ze werken op het niveau van HTTP in plaats van de DNS-niveau. Sommige providers DNS bundelen een service van de omleidings-URL als onderdeel van hun algehele aanbieding. Deze service wordt momenteel niet ondersteund door Azure DNS.

De functie van de omleidings-URL wordt bijgehouden in de achterstand van Azure DNS. Gebruik van de site voor gebruikersfeedback van naar [registreren van het ondersteuningsteam van het voor deze functie](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

### <a name="does-azure-dns-support-the-extended-ascii-encoding-8-bit-set-for-txt-record-sets"></a>Azure DNS biedt ondersteuning voor de uitgebreide ASCII (8-bits) instellen voor recordsets TXT-codering?

Ja. Azure DNS ondersteunt de uitgebreide ASCII codering voor TXT-recordsets. Maar u moet de nieuwste versie van de Azure REST API's, SDK's, PowerShell en de CLI. Versies die ouder zijn dan 1 oktober 2017, of SDK 2.1 bieden geen ondersteuning voor de uitgebreide ASCII-set. 

U kunt bijvoorbeeld een tekenreeks opgeven als de waarde voor een TXT-record met de uitgebreide ASCII-teken \128. Een voorbeeld is "abcd\128efgh." Azure DNS maakt gebruik van de bytewaarde van dit teken 128 is, in interne weergave. Op het moment van DNS-omzetting, wordt deze bytewaarde in het antwoord geretourneerd. Let ook op dat "abc" en "\097\098\099" uitwisselbaar zijn wat betreft resolutie is. 

We volgen [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt) zone bestand master indeling escape regels voor TXT-records. Bijvoorbeeld, `\` nu eigenlijk verlaat u alles per de RFC. Als u opgeeft `A\B` als de waarde van de TXT-record, is het weergegeven en opgelost als alleen `AB`. Als u echt wilt dat de TXT-record dat `A\B` resolutie, moet u als escape voor de `\` opnieuw. Geef een voorbeeld: `A\\B`.

Deze ondersteuning momenteel niet beschikbaar is voor TXT-records gemaakt op basis van de Azure-portal.

## <a name="alias-records"></a>Aliasrecords

### <a name="what-are-some-scenarios-where-alias-records-are-useful"></a>Wat zijn enkele scenario's waarbij aliasrecords nuttig zijn?

Zie de scenario's sectie in de [Azure overzicht DNS-aliassen records](dns-alias.md).

### <a name="what-record-types-are-supported-for-alias-record-sets"></a>Welke typen worden ondersteund voor alias recordsets?

Alias-recordsets worden ondersteund voor de volgende recordtypen in Azure DNS-zone:
 
- A 
- AAAA
- CNAME 

### <a name="what-resources-are-supported-as-targets-for-alias-record-sets"></a>Welke bronnen worden ondersteund als de doelen voor recordsets alias?

- **Wijs een openbaar IP-resource van een DNS-server A/AAAA-Recordset.** U kunt een A/AAAA-recordset maken en geef deze een alias recordset om te verwijzen naar een openbare IP-resource.
- **Verwijzen naar een Traffic Manager-profiel van een AAAA-DNS A/CNAME-Recordset.** U kunt verwijzen naar de CNAME van een Traffic Manager-profiel van een DNS CNAME-Recordset. Een voorbeeld is contoso.trafficmanager.net. Nu kunt u ook verwijzen naar een Traffic Manager-profiel met externe eindpunten van een A of AAAA-recordset in de DNS-zone.
- **Verwijzen naar een eindpunt Azure Content Delivery Network (CDN)**. Dit is handig als u statische website met Azure storage en Azure CDN te maken.
- **Verwijzen naar een andere DNS-recordset binnen dezelfde regio.** Aliasrecords kunnen verwijzen naar aan de andere recordsets van hetzelfde type. U kunt bijvoorbeeld een DNS CNAME-recordset hebben als alias voor een andere CNAME-recordset van hetzelfde type. Deze benadering is handig als u wilt dat aantal recordsets zijn aliassen en enkele niet-aliassen.

### <a name="can-i-create-and-update-alias-records-from-the-azure-portal"></a>Kan ik maken en de alias records uit de Azure-portal bijwerken?

Ja. U kunt maken of aliasrecords in de Azure-portal samen met de Azure REST API's, PowerShell, CLI en SDK's beheren.

### <a name="will-alias-records-help-to-make-sure-my-dns-record-set-is-deleted-when-the-underlying-public-ip-is-deleted"></a>Aliasrecords helpt om ervoor te zorgen dat mijn DNS-recordset wordt verwijderd wanneer het onderliggende openbare IP-adres wordt verwijderd?

Ja. Deze functie is een van de belangrijkste mogelijkheden van de aliasrecords. Deze vermijdt u mogelijke storingen voor gebruikers van uw toepassing.

### <a name="will-alias-records-help-to-make-sure-my-dns-record-set-is-updated-to-the-correct-ip-address-when-the-underlying-public-ip-address-changes"></a>Wordt de alias registreert help om te controleren of mijn DNS-recordset wordt bijgewerkt naar de juiste IP-adres wanneer de onderliggende openbare IP-adres verandert?

Ja. Deze functie is een van de belangrijkste mogelijkheden van de aliasrecords. Hiermee kunt u geen mogelijke storingen of beveiligingsrisico voor uw toepassing.

### <a name="are-there-any-restrictions-when-using-alias-record-sets-for-a-or-aaaa-records-to-point-to-traffic-manager"></a>Zijn er beperkingen wanneer u de alias-record voor A of AAAA-records om te verwijzen naar Traffic Manager?

Ja. Profiel moet alleen externe eindpunten gebruiken om te verwijzen naar een Traffic Manager-profiel als een alias van een A of AAAA-recordset moeten de Traffic Manager. Wanneer u de externe eindpunten in Traffic Manager maakt, bieden u de werkelijke IP-adressen van de eindpunten.

### <a name="is-there-an-additional-charge-to-use-alias-records"></a>Is er een extra kosten in rekening gebracht op aliasrecords gebruiken?

Aliasrecords zijn een kwalificatie op een geldige DNS-Recordset. Er is geen aanvullende facturering voor aliasrecords.

## <a name="use-azure-dns"></a>Azure DNS gebruiken

### <a name="can-i-co-host-a-domain-by-using-azure-dns-and-another-dns-provider"></a>Kan ik mede host een domein met behulp van Azure DNS en een andere DNS-provider?

Ja. Azure DNS ondersteunt CO hosting-domeinen met een andere DNS-services.

Als u CO hosting instelt, wijzig de NS-records voor het domein om te verwijzen naar de naamservers van beide providers. De naamserver (NS) vastgelegd besturingselement welke providers DNS-query's voor het domein ontvangen. U kunt deze NS-records in Azure DNS, in de andere provider en in de bovenliggende zone wijzigen. De bovenliggende zone is standaard geconfigureerd via de domeinnaamregistrar. Zie voor meer informatie over DNS-delegering [domein-DNS-delegering](dns-domain-delegation.md).

Zorg er ook voor zorgen dat de DNS-records voor het domein gesynchroniseerd tussen beide DNS-providers zijn. Azure DNS ondersteunt momenteel geen DNS-zone-overdrachten. DNS-records moeten worden gesynchroniseerd met behulp van de [Azure DNS-beheerportal](dns-operations-recordsets-portal.md), [REST-API](https://docs.microsoft.com/powershell/module/az.dns), [SDK](dns-sdk.md), [PowerShell-cmdlets](dns-operations-recordsets.md), of de [CLI-hulpprogramma](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-four-azure-dns-name-servers"></a>Heb ik mijn domein delegeren naar alle vier Azure DNS-naamservers?

Ja. Azure DNS wijst vier naamservers toe aan elke DNS-zone. Deze benadering is voor foutisolatie en verhoogde flexibiliteit. Als u wilt komen voor de SLA van Azure DNS, uw domein delegeren naar alle vier de naamservers.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Wat zijn de limieten voor Azure DNS?

De volgende standaardlimieten van toepassing wanneer u Azure DNS.

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Kan ik een Azure DNS-zone verplaatsen tussen resourcegroepen of tussen abonnementen?

Ja. DNS-zones kunnen worden verplaatst tussen resourcegroepen of abonnementen.

Er is geen invloed op de DNS-query's wanneer u een DNS-zone verplaatst. De naamservers die zijn toegewezen aan de zone blijven hetzelfde. DNS-query's worden verwerkt als normale in.

Zie voor meer informatie en instructies over het verplaatsen van de DNS-zones, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>Hoe lang duurt het voor DNS-wijzigingen worden doorgevoerd?

Nieuwe DNS-zones en DNS-records doorgaans worden weergegeven in de Azure DNS-naamservers snel. De timing is een paar seconden.

Wijzigingen in bestaande DNS-records kunnen iets langer duren. Ze doorgaans worden weergegeven in de Azure DNS-naamservers binnen 60 seconden. De DNS-cache door DNS-clients en de recursieve DNS-resolvers buiten Azure DNS ook kunnen invloed hebben op het tijdstip. Voor het beheren van deze Cacheduur van de, gebruikt u de eigenschap Time-To-Live (TTL) van elke recordset.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>Hoe kan ik mijn DNS-zones tegen onbedoeld beveiligen?

Azure DNS wordt beheerd met behulp van Azure Resource Manager. Azure DNS profiteren van de functies voor toegangsbeheer met Azure Resource Manager. Op rollen gebaseerd toegangsbeheer bepaalt welke gebruikers hebt gelezen of schrijftoegang tot de DNS-zones en -recordsets. Resourcevergrendelingen te voorkomen dat onbedoelde wijziging of verwijdering van DNS-zones en -recordsets.

Zie voor meer informatie, [beveiligen DNS-zones en records](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Hoe stel ik SPF-records in Azure DNS?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-name-servers-resolve-over-ipv6"></a>Kunnen de DNS-naamservers oplossen via IPv6? 

Ja. Azure DNS-naamservers zijn dual-stack. Dual-stack betekent dat ze beschikken over IPv4 en IPv6-adressen. U vindt het IPv6-adres voor de DNS-naamservers toegewezen aan de DNS-zone, een hulpprogramma zoals nslookup te gebruiken. Een voorbeeld is `nslookup -q=aaaa <Azure DNS Nameserver>`.

### <a name="how-do-i-set-up-an-idn-in-azure-dns"></a>Hoe stel ik een IDN in Azure DNS?

Geïnternationaliseerde domeinnamen (IDN) coderen elke DNS-naam met behulp van [punycode](https://en.wikipedia.org/wiki/Punycode). DNS-query's worden gemaakt met behulp van deze namen punycode-codering.

Als u wilt configureren IDN's in Azure DNS, moet u de naam van zone of recordnaam converteren naar punycode. Azure DNS ondersteunt momenteel geen ingebouwde conversie naar of van punycode.

## <a name="private-dns"></a>Privé-DNS

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

### <a name="does-azure-dns-support-private-domains"></a>Biedt ondersteuning voor Azure DNS private domeinen?

Ondersteuning voor persoonlijke domeinen is geïmplementeerd met behulp van de functie voor Privézones. Deze functie is momenteel beschikbaar in openbare preview. Privézones worden beheerd met behulp van dezelfde hulpprogramma's als internetgerichte Azure DNS-zones. Ze zijn omgezet alleen uit in de opgegeven virtuele netwerken. Zie voor meer informatie de [overzicht](private-dns-overview.md).

Op dit moment worden niet private zones ondersteund in Azure portal.

Zie voor informatie over andere interne DNS-opties in Azure, [naamomzetting voor VM's en rolexemplaren](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="whats-the-difference-between-registration-virtual-network-and-resolution-virtual-network-in-the-context-of-private-zones"></a>Wat is het verschil tussen Registration virtual network en virtuele resolutienetwerk in de context van privézones?

Als een Registration virtual network of als een resolutie van virtueel netwerk, kunt u virtuele netwerken koppelen aan een privé-DNS-zone. In beide gevallen gegevensrecord virtuele machines in het virtuele netwerk op basis van de records in de privézone. Met een virtueel netwerk van de registratie van worden DNS-records automatisch geregistreerd in de zone voor de virtuele machines in het virtuele netwerk. Wanneer een virtuele machine in de registratie van een virtueel netwerk wordt verwijderd, wordt de bijbehorende DNS-record van de gekoppelde privézone automatisch verwijderd. 

### <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>Azure DNS Private Zones werken in verschillende Azure-regio's?

Ja. Privézones wordt ondersteund voor DNS-omzetting tussen virtuele netwerken in Azure-regio's. Privézones werkt zelfs zonder expliciet peering van de virtuele netwerken. De virtuele netwerken moeten worden opgegeven als het probleem zou moeten virtuele netwerken voor de privézone. Klanten moet mogelijk de virtuele netwerken aan worden gekoppeld voor TCP/HTTP-verkeer van de ene regio naar een andere.

### <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Verbinding met Internet is van virtuele netwerken die zijn vereist voor privézones?

Nee. Privézones werken samen met virtuele netwerken. Klanten gebruiken ze voor het beheren van domeinen voor virtuele machines of andere resources binnen en tussen virtuele netwerken. Verbinding met Internet is niet vereist voor naamomzetting. 

### <a name="can-the-same-private-zone-be-used-for-several-virtual-networks-for-resolution"></a>Kan de dezelfde privézone worden gebruikt voor verschillende virtuele netwerken voor het omzetten van?

Ja. Klanten kunnen maximaal 10 Resolution virtual networks koppelen aan een enkele privézone.

### <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-resolution-virtual-network-to-a-private-zone"></a>Een virtueel netwerk dat bij een ander abonnement hoort kan worden toegevoegd als een virtuele resolutienetwerk aan een privézone?

Ja. U moet schrijfmachtiging bewerking op de virtuele netwerken en de privé-DNS-zone. De machtiging schrijven kan worden verleend aan verschillende RBAC-rollen. De klassieke netwerk Inzender RBAC-rol heeft bijvoorbeeld schrijfmachtigingen voor de virtuele netwerken. Zie voor meer informatie over RBAC-rollen [Role-based access control van](../role-based-access-control/overview.md).

### <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-the-virtual-machines-are-deleted-by-the-customer"></a>De DNS-records automatisch worden geregistreerd virtuele machine in een privé-zone worden automatisch verwijderd wanneer de virtuele machines worden verwijderd door de klant?

Ja. Als u een virtuele machine binnen een virtueel netwerk voor inschrijving verwijdert, worden automatisch de DNS-records die zijn geregistreerd in de zone verwijderd. 

### <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-registration-virtual-network-be-deleted-manually"></a>Kan een record automatisch worden geregistreerd virtuele machine in een privézone vanuit een virtueel netwerk registratie handmatig worden verwijderd?

Nee. De virtuele machine DNS-records die automatisch worden geregistreerd bij een privézone vanuit een virtueel netwerk van registratie zijn niet zichtbaar of bewerkt door klanten. U kunt de automatisch geregistreerde DNS-records overschrijven met een handmatig gemaakte DNS-record in de zone. De volgende vraag en antwoord-adres in dit onderwerp.

### <a name="what-happens-when-we-try-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-registration-virtual-network"></a>Wat gebeurt er wanneer we handmatig een nieuwe DNS-record maken in een privé-zone met de dezelfde hostnaam als een bestaande virtuele-machine automatisch worden geregistreerd in een virtueel netwerk van de registratie?

U probeert een nieuwe DNS-record handmatig maken in een privé-zone met de dezelfde hostnaam als een virtuele machine van de bestaande, automatisch worden geregistreerd in een virtueel netwerk van de registratie. Wanneer u dit doet, overschrijft de nieuwe DNS-record de record automatisch worden geregistreerd virtuele machine. Als u dit handmatig gemaakte DNS-record verwijderen uit de zone het opnieuw probeert, slaagt de verwijdering. De automatische registratie zich weer voordoet, zolang de virtuele machine nog bestaat en of een privé IP-adres is gekoppeld aan. De DNS-record wordt automatisch opnieuw gemaakt in de zone.

### <a name="what-happens-when-we-unlink-a-registration-virtual-network-from-a-private-zone-will-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-too"></a>Wat gebeurt er wanneer we een virtueel netwerk van de registratie van een privézone ontkoppelen? De records automatisch worden geregistreerd virtuele machine van het virtuele netwerk verwijderd uit de zone te?

Ja. Als u wilt ontkoppelen van een virtueel netwerk van de registratie van een privé-zone, moet u de DNS-zone voor het verwijderen van het bijbehorende Registration virtual network bijwerken. Virtuele machine-records die automatisch zijn geregistreerd worden tijdens dit proces wordt verwijderd uit de zone. 

### <a name="what-happens-when-we-delete-a-registration-or-resolution-virtual-network-thats-linked-to-a-private-zone-do-we-have-to-manually-update-the-private-zone-to-unlink-the-virtual-network-as-a-registration-or-resolution--virtual-network-from-the-zone"></a>Wat gebeurt er wanneer we een registratie- of Resolutienetwerken virtueel netwerk dat gekoppeld aan een privézone verwijderen? Hebben we de privézone als u wilt ontkoppelen van het virtuele netwerk als een registratie- of virtuele resolutienetwerk van de zone handmatig bijwerken?

Ja. Wanneer u een registratie- of Resolutienetwerken virtueel netwerk zonder deze eerst ontkoppelen van een privé-zone verwijdert, wordt de verwijderingsbewerking slaagt. Maar het virtuele netwerk wordt niet automatisch ontkoppeld van uw persoonlijke zone, indien van toepassing. U moet het virtuele netwerk van de persoonlijke zone handmatig ontkoppelen. Om deze reden, het virtuele netwerk van uw privé-zone te ontkoppelen voordat u deze verwijdert.

### <a name="will-dns-resolution-by-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-privatecontosocom-is-linked-to-a-virtual-network"></a>DNS-omzetting met behulp van de standaard FQDN-naam (internal.cloudapp.net) nog steeds werken, zelfs wanneer een privézone (bijvoorbeeld private.contoso.com) is gekoppeld aan een virtueel netwerk?

Ja. Privézones wordt de standaard DNS-oplossingen met behulp van de zone Azure verschafte internal.cloudapp.net niet vervangen. Het wordt aangeboden als een extra functie of een uitbreiding. Of u op de Azure verschafte internal.cloudapp.net of op uw eigen privé-zone vertrouwen, gebruikt u de FQDN-naam van de zone die u wilt oplossen op basis van. 

### <a name="will-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>Wordt het DNS-achtervoegsel op virtuele machines in een gekoppeld virtueel netwerk met die van de persoonlijke zone gewijzigd?

Nee. De DNS-achtervoegsel op de virtuele machines in het gekoppelde virtuele netwerk als het standaard-Azure verschafte achtervoegsel blijft ("*. internal.cloudapp.net '). U kunt dit DNS-achtervoegsel handmatig wijzigen op uw virtuele machines met die van de privézone. 

### <a name="are-there-any-limitations-for-private-zones-during-this-preview"></a>Zijn er beperkingen voor privézones in deze Preview-versie?

Ja. Tijdens de openbare preview bestaat de volgende beperkingen.
* Een registratie-netwerk per privézone.
* Maximaal 10 Resolution virtual networks per privézone.
* Een virtueel netwerkkoppelingen gegeven aan slechts één privézone als een Registration virtual network.
* Een virtuele netwerkkoppelingen maximaal 10 private zones opgegeven als een resolutie van virtueel netwerk.
* Als een virtueel netwerk van de registratie is opgegeven, kunnen de DNS-records voor de virtuele machines van dit virtuele netwerk die zijn geregistreerd bij de privé-zone worden weergegeven of opgehaald vanuit PowerShell, CLI of API's. De VM-records worden geregistreerd en is opgelost.
* Omgekeerde DNS-geldt alleen voor privé-IP-adresruimte in het Registration virtual network.
* Reverse-DNS voor een privé IP-adres dat niet geregistreerd in de privézone retourneert 'internal.cloudapp.net' als het DNS-achtervoegsel. Dit achtervoegsel kan niet worden omgezet. Een voorbeeld is een privé IP-adres voor een virtuele machine in een virtueel netwerk dat gekoppeld als een virtuele resolutienetwerk aan een privézone.
* Een virtueel netwerk moet leeg zijn als voor de eerste keer aan een privézone als een registratie- of Resolutienetwerken virtueel netwerk worden gekoppeld. Het virtuele netwerk vervolgens mag niet leeg zijn voor het toekomstige koppelen als een registratie- of Resolutienetwerken virtueel netwerk naar andere private zones.
* Voorwaardelijk doorsturen wordt niet ondersteund, bijvoorbeeld, zodat het probleem zou moeten tussen Azure en on-premises netwerken. Meer informatie over hoe klanten in dit scenario via andere methoden kunnen realiseren. Zie [naamomzetting voor virtuele machines en rollen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)

### <a name="are-there-any-quotas-or-limits-on-zones-or-records-for-private-zones"></a>Zijn er quota's of limieten voor zones of records voor privézones?

Er zijn geen beperkingen voor het aantal zones per abonnement voor privézones toegestaan. Er zijn geen beperkingen voor het aantal recordsets per zone voor privézones. Openbare en private zones tellen mee voor de algemene DNS-limieten. Zie voor meer informatie de [Azure-abonnement en Servicelimieten](../azure-subscription-service-limits.md#azure-dns-limits)

### <a name="is-there-portal-support-for-private-zones"></a>Is er portal ondersteuning voor privézones?

Privézones die al zijn gemaakt via API's, PowerShell, CLI en SDK's worden weergegeven op de Azure-portal. Maar klanten kunnen maken van nieuwe private zones of koppelingen met virtuele netwerken beheren. Voor virtuele netwerken die zijn gekoppeld als Registration virtual networks, zijn automatisch worden geregistreerd records van de virtuele machine niet zichtbaar zijn vanaf de portal. 

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over Azure DNS](dns-overview.md).

- [Meer informatie over het gebruik van Azure DNS voor persoonlijke domeinen](private-dns-overview.md).

- [Meer informatie over DNS-zones en records](dns-zones-records.md).

- [Aan de slag met Azure DNS](dns-getstarted-portal.md).
