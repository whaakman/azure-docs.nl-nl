---
title: Met Azure Traffic Manager - Veelgestelde vragen
description: In dit artikel vindt u antwoorden op veelgestelde vragen over Traffic Manager
services: traffic-manager
documentationcenter: ''
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: kumud
ms.openlocfilehash: a6a8fee942edf4cec98a6d2f46eb2f63b7595c09
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54200044"
---
# <a name="traffic-manager-frequently-asked-questions-faq"></a>Traffic Manager Frequently Asked Questions (FAQ)

## <a name="traffic-manager-basics"></a>Grondbeginselen van Traffic Manager

### <a name="what-ip-address-does-traffic-manager-use"></a>Welke IP-adres maakt gebruik van Traffic Manager?

Zoals uitgelegd in [hoe Traffic Manager werkt](../traffic-manager/traffic-manager-how-it-works.md), Traffic Manager werkt op DNS-niveau. DNS-antwoorden aan clients omleiden naar de juiste service-eindpunt wordt verzonden. Clients vervolgens rechtstreeks verbinding maken met de service-eindpunt, niet via Traffic Manager.

Traffic Manager biedt daarom geen een eindpunt of een IP-adres voor clients verbinding maken met. Als u statische IP-adres voor uw service, die moet worden geconfigureerd op de service, niet in Traffic Manager.

### <a name="what-types-of-traffic-can-be-routed-using-traffic-manager"></a>Welke soorten verkeer kunnen worden doorgestuurd met Traffic Manager?
Zoals uitgelegd in [hoe Traffic Manager werkt](../traffic-manager/traffic-manager-how-it-works.md), een Traffic Manager-eindpunt mag een internetgerichte service die wordt gehost binnen en buiten Azure. Traffic Manager kan daarom routeren van verkeer dat afkomstig van het openbare internet naar een set eindpunten is die ook internet ondervindt. Hebt u eindpunten die zich in een particulier netwerk (bijvoorbeeld een interne versie van [Azure Load Balancer](../load-balancer/load-balancer-overview.md#internalloadbalancer)) of een DNS-aanvragen van dergelijke interne netwerken, kunt u Traffic Manager gebruiken voor het routeren van verkeer.


### <a name="does-traffic-manager-support-sticky-sessions"></a>Traffic Manager biedt ondersteuning voor 'sticky' sessies?

Zoals uitgelegd in [hoe Traffic Manager werkt](../traffic-manager/traffic-manager-how-it-works.md), Traffic Manager werkt op DNS-niveau. Het maakt gebruik van DNS-antwoorden aan clients omleiden naar de juiste service-eindpunt. Clients rechtstreeks verbinding maken met de service-eindpunt, niet via Traffic Manager. Traffic Manager ziet daarom niet de HTTP-verkeer tussen de client en de server.

Bovendien wordt de bron-IP-adres van de DNS-query die is ontvangen door Traffic Manager behoort tot de recursieve DNS-service, niet door de client. Traffic Manager is daarom geen enkele manier voor het bijhouden van afzonderlijke clients en 'sticky' sessies kan niet worden geïmplementeerd. Deze beperking is gebruikelijk voor alle verkeer op basis van de DNS beheer van systemen en is niet specifiek op Traffic Manager.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Waarom zie ik een HTTP-fout bij het gebruik van Traffic Manager?

Zoals uitgelegd in [hoe Traffic Manager werkt](../traffic-manager/traffic-manager-how-it-works.md), Traffic Manager werkt op DNS-niveau. Het maakt gebruik van DNS-antwoorden aan clients omleiden naar de juiste service-eindpunt. Clients vervolgens rechtstreeks verbinding maken met de service-eindpunt, niet via Traffic Manager. Traffic Manager biedt geen Zie HTTP-verkeer tussen client en server. Daarom moet een HTTP-fout die wordt weergegeven afkomstig zijn van uw toepassing. Voor de client verbinding maken met de toepassing, zijn alle DNS-omzetting stappen zijn voltooid. Dit is inclusief elke interactie met Traffic Manager op de stroom van de toepassing verkeer.

Verder onderzoek moet daarom zich richten op de toepassing.

De HTTP host-header verzonden vanuit de browser van de client is de meest voorkomende oorzaak van problemen. Zorg ervoor dat de toepassing is geconfigureerd voor het accepteren van de juiste host-header voor de domeinnaam die u gebruikt. Zie voor eindpunten met Azure App Service, [configureren van een aangepaste domeinnaam voor een web-app in Azure App Service met Traffic Manager](../app-service/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-the-performance-impact-of-using-traffic-manager"></a>Wat zijn de prestatiegevolgen van het gebruik van Traffic Manager?

Zoals uitgelegd in [hoe Traffic Manager werkt](../traffic-manager/traffic-manager-how-it-works.md), Traffic Manager werkt op DNS-niveau. Omdat clients rechtstreeks verbinding met uw service-eindpunten maken, is er geen invloed op de prestaties in rekening gebracht bij het gebruik van Traffic Manager nadat de verbinding tot stand is gebracht.

Sinds de Traffic Manager kan worden geïntegreerd met toepassingen op DNS-niveau, hoeft een extra DNS-zoekactie in de keten van DNS-resolutie moet worden ingevoegd. De impact van Traffic Manager op de DNS-omzettingstijd is minimaal. Traffic Manager gebruikt een wereldwijd netwerk met naamservers en maakt gebruik van [anycast](https://en.wikipedia.org/wiki/Anycast) netwerken om ervoor te zorgen DNS worden query's altijd doorgestuurd naar de dichtstbijzijnde beschikbare naam-server. Bovendien houdt in cache plaatsen van DNS-antwoorden die de extra DNS-latentie in rekening gebracht met behulp van Traffic Manager alleen voor een fractie van sessies geldt.

De methode prestaties verkeer routeert naar de dichtstbijzijnde beschikbare eindpunt. Het resultaat is dat de invloed op de algehele prestaties die zijn gekoppeld aan deze methode minimaal moet. Alle verhogingen van DNS-latentie moet worden gecompenseerd door lagere netwerklatentie naar het eindpunt.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Welke toepassingsprotocollen kan ik met Traffic Manager gebruiken?

Zoals uitgelegd in [hoe Traffic Manager werkt](../traffic-manager/traffic-manager-how-it-works.md), Traffic Manager werkt op DNS-niveau. Nadat de DNS-zoekactie voltooid is, clients rechtstreeks verbinding maken met het toepassingseindpunt, niet via Traffic Manager. De verbinding Gebruik daarom elk toepassingsprotocol voor de. Als u TCP als de controle van het protocol, Traffic Manager selecteren statuscontrole eindpunt kan worden uitgevoerd zonder met behulp van een toepassingsprotocollen. Als u ervoor kiest om de status controleren met behulp van een toepassingsprotocol, moet het eindpunt kunnen reageren op aanvragen via HTTP of HTTPS ophalen.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Kan ik Traffic Manager gebruiken met de naam van een domein 'zonder voorvoegsel zijn'?

Nee. De DNS-standaarden staan niet toe dat CNAME-records in combinatie met andere DNS-records met dezelfde naam. De apex (of basiscertificaat) van een DNS-zone bevat altijd twee vooraf bestaande DNS-records. de SOA en gezaghebbende NS-records. Dit betekent dat een CNAME-record in de apex van de zone kan niet worden gemaakt zonder te schenden van de DNS-standaarden.

Traffic Manager is een DNS CNAME-record toe te wijzen de aangepaste DNS-naam vereist. Bijvoorbeeld, wijst u `www.contoso.com` naar de DNS-naam van het Traffic Manager-profiel `contoso.trafficmanager.net`. Traffic Manager-profiel wordt bovendien een tweede DNS CNAME om aan te geven welk eindpunt dat de client verbinding moet maken.

U kunt dit probleem omzeilen, wordt u aangeraden een HTTP-omleiding voor verkeer van de naam van het domein zonder voorvoegsel zijn naar een andere URL, die vervolgens Traffic Manager kunt gebruiken. Het domein zonder voorvoegsel zijn 'contoso.com' kan bijvoorbeeld gebruikers omleiden naar de CNAME 'www.contoso.com' die naar de Traffic Manager-DNS-naam verwijst.

Volledige ondersteuning voor zonder voorvoegsel zijn domeinen in Traffic Manager wordt bijgehouden in de functie gewerkt. U kunt registreren voor deze functieaanvraag door het ondersteuningsteam van het [uw stem op deze op de site voor gebruikersfeedback van onze community](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).

### <a name="does-traffic-manager-consider-the-client-subnet-address-when-handling-dns-queries"></a>Traffic Manager rekening houden met het adres van client subnet bij het verwerken van DNS-query's? 
Ja, naast de bron-IP-adres van de DNS-query wordt ontvangen (dit is meestal het IP-adres van de DNS-resolver), bij het uitvoeren van zoekopdrachten voor geografisch, prestaties en een Subnet routeringsmethoden, traffic manager ook rekening gehouden met het adres van client subnet als het is opgenomen in de query door de resolver die de aanvraag namens de eindgebruiker.  
Specifiek, [RFC 7871 – Subnet van de Client in DNS-query's](https://tools.ietf.org/html/rfc7871) die zorgt voor een [uitbreidingsmechanisme voor DNS (EDNS0)](https://tools.ietf.org/html/rfc2671) waarmee op het adres van client subnet kan doorgeven van resolvers die dit ondersteunen.

### <a name="what-is-dns-ttl-and-how-does-it-impact-my-users"></a>Wat is DNS TTL en hoe is deze van invloed op mijn gebruikers?

Als u een DNS-query terechtkomt op Traffic Manager, wordt een waarde in het antwoord met de naam time-to-live (TTL). Deze waarde, waarvan eenheid in seconden is, geeft u aan DNS-resolvers downstream op hoe lang dit antwoord in de cache. Terwijl de DNS-resolvers zijn niet noodzakelijkerwijs dit resultaat in de cache, ze zorgt ervoor dat ze reageren op de volgende query's uit de cache in plaats van naar Traffic Manager-DNS-servers. Dit heeft gevolgen voor de antwoorden als volgt:
- een hogere TTL-waarde vermindert het aantal query's die naar de Traffic Manager-DNS-servers, waardoor de kosten verminderen voor een klant, omdat het aantal query's die worden geleverd is een factureerbare gebruik.
- een hogere TTL-waarde kan mogelijk de tijd die nodig zijn om een DNS-zoekactie verminderen.
- een hogere TTL-waarde betekent ook dat de gegevens komt niet overeen met de meest recente informatie status Traffic Manager is verkregen via de testinterval agenten.

### <a name="how-high-or-low-can-i-set-the-ttl-for-traffic-manager-responses"></a>Hoe hoog of laag kan ik de TTL voor Traffic Manager antwoorden instellen?

U kunt instellen, op een per profiel niveau, de DNS TTL moet zo laag 0 seconden en zo hoog 2.147.483.647 seconden (compatibel zijn met het maximumbereik [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt )). Een TTL-waarde 0 betekent dat reacties op query's niet in cache opslaan downstream DNS-resolvers en alle query's wordt verwacht dat de Traffic Manager-DNS-servers voor het omzetten van bereiken.

### <a name="how-can-i-understand-the-volume-of-queries-coming-to-my-profile"></a>Hoe kan ik begrijp dat het volume van de query's die aan Mijn profiel? 
Een van de metrische gegevens die door Traffic Manager wordt het aantal query's beantwoord door een profiel. U kunt deze informatie vinden op het niveau aggregatie van een profiel of u kunt splitsen verder Zie het volume van query's waarin specifieke eindpunten zijn geretourneerd. U kunt ook waarschuwingen instellen om u te waarschuwen als het volume van query-antwoord geretourneerd waarin de voorwaarden die u hebt ingesteld. Voor meer informatie, [Traffic Manager-statistieken en -waarschuwingen](traffic-manager-metrics-alerts.md).

## <a name="traffic-manager-geographic-traffic-routing-method"></a>Traffic Manager geografisch verkeersrouteringsmethode

### <a name="what-are-some-use-cases-where-geographic-routing-is-useful"></a>Wat zijn enkele gebruiksvoorbeelden waar de geografische routering nuttig is? 
Geografische routeringstype kan worden gebruikt in een scenario waarin een Azure-klant moet onderscheiden van de gebruikers op basis van geografische regio's. Bijvoorbeeld, kunt met behulp van de geografische verkeersrouteringsmethode, u gebruikers geven in specifieke regio's de ervaring van een andere gebruiker dan die van andere regio's. Een ander voorbeeld is dat voldoet aan de lokale gegevens soevereiniteit mandaten die vereisen dat gebruikers van een specifieke regio's worden geleverd alleen vanaf eindpunten in deze regio.

### <a name="how-do-i-decide-if-i-should-use-performance-routing-method-or-geographic-routing-method"></a>Hoe bepaal ik als ik routeringsmethode voor prestaties of de geografische routering methode moet gebruiken? 
Het belangrijkste verschil tussen deze twee methoden voor het populaire doorsturen is dat u in die het primaire doel is om verkeer te verzenden naar het eindpunt dat de laagste latentie aan de aanroeper vastgesteld bieden kan, terwijl in geografisch routering van het voornaamste doel is om af te dwingen een geografisch gebied routeringsmethode Performance omheining voor uw aanroepers zodat u opzettelijk naar een bepaald eindpunt versturen kunt. De overlapping treedt op omdat er een correlatie tussen geografische mate en lagere latentie, hoewel dit niet altijd de waarde true. Mogelijk zijn er een eindpunt in een andere Geografie bevinden die voor de oproepende functie een betere ervaring voor latentie biedt en in dat geval routeringsoptie prestaties wordt de gebruiker doorsturen naar dit eindpunt, maar de geografische routering wordt altijd ze verzenden naar het eindpunt dat u hebt toegewezen voor hun geografische regio. U routering maken ongebruikelijk toewijzingen, zoals het verzenden van al het verkeer van Azië naar eindpunten in de Verenigde Staten en alle VS-verkeer naar de eindpunten in Azië om te wissen, houd rekening met het volgende voorbeeld: met geografisch verder te maken. In dat geval de geografische routering van opzettelijk doet precies wat u hebt geconfigureerd om te doen en optimalisatie van prestaties is geen overweging. 
>[!NOTE]
>Mogelijk zijn er scenario's waarin u mogelijk beide prestaties en mogelijkheden voor geografische routering, voor deze scenario's voor geneste profielen goed van pas kunnen worden. U kunt bijvoorbeeld een bovenliggende-profiel met de geografische routering van waar u al het verkeer van Noord-Amerika naar een geneste profiel die eindpunten zijn in de Verenigde Staten verzenden instellen en gebruiken van prestaties routering voor het verzenden van die verkeer naar het beste eindpunt binnen deze is ingesteld. 

### <a name="what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing"></a>Wat zijn de regio's die door Traffic Manager worden ondersteund voor de geografische routering? 
De land-/ regiohiërarchie die wordt gebruikt door Traffic Manager kan worden gevonden [hier](traffic-manager-geographic-regions.md). Terwijl deze pagina wordt bijgewerkt met wijzigingen, kunt u ook programmatisch dezelfde informatie ophalen met behulp van de [REST-API van Azure Traffic Manager](https://docs.microsoft.com/rest/api/trafficmanager/). 

### <a name="how-does-traffic-manager-determine-where-a-user-is-querying-from"></a>Hoe wordt traffic manager vastgesteld waar opvragen van een gebruiker uit? 
Traffic Manager kijkt naar de bron-IP-adres van de query (waarschijnlijk is dit doen met het uitvoeren van query's namens de gebruiker een lokale DNS-resolver) en een interne IP-adres op de kaart met regio's gebruikt om te bepalen van de locatie. Deze kaart wordt bijgewerkt regelmatig ter compensatie van wijzigingen in het internet. 

### <a name="is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case"></a>Kan deze gegarandeerd dat de exacte geografische locatie van de gebruiker in alle gevallen correct door Traffic Manager kunt bepalen?
Nee, Traffic Manager kan niet garanderen dat de geografische regio die we van de bron-IP-adres van een DNS-query afleiden wordt altijd overeenkomen met de locatie van de gebruiker de volgende oorzaken: 

- Eerst, zoals wordt beschreven in de vorige sectie, het bron-IP-adres we zien is die van een DNS-resolver doen opzoeken namens de gebruiker. Hoewel de geografische locatie van de DNS-resolver een goede proxy voor de geografische locatie van de gebruiker is, kan ook worden verschillende, afhankelijk van het bereik van de DNS-resolver-service en de specifieke DNS-resolver-service die een klant heeft gekozen om te gebruiken. Als u bijvoorbeeld kan een klant in Maleisië opgeven van hun apparaat instellingen gebruikt een DNS-resolver service waarvan DNS-server in Singapore mogelijk ophalen verzameld voor het afhandelen van de query-oplossingen voor die gebruiker/apparaat. Traffic Manager kunt in dat geval alleen zien van de conflictoplosser IP-adres dat overeenkomt met de locatie van de Singapore. Zie de eerdere Veelgestelde vragen over ondersteuning van client subnet-adres ook op deze pagina.

- Ten tweede Traffic Manager maakt gebruik van een interne kaart te doen, het IP-adres tot vertaling van de geografische regio. Deze kaart is gevalideerd en regelmatig te vergroten van de nauwkeurigheid voor de veranderende aard van de internet-account is bijgewerkt, maar er is nog steeds de mogelijkheid om onze gegevens is niet een exacte weergave van de geografische locatie van alle IP-adres adressen.


###  <a name="does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing"></a>Moet een eindpunt zich fysiek bevinden in dezelfde regio bevinden als het account dat is geconfigureerd met voor de geografische routering? 
Nee, de locatie van het eindpunt legt geen beperkingen op welke regio's kunnen worden toegewezen aan deze. Een eindpunt in VS-centraal-Azure-regio kunt bijvoorbeeld alle gebruikers van India omgeleid naar deze.

### <a name="can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing"></a>Kan ik geografische regio's toewijzen aan de eindpunten in een profiel dat is niet geconfigureerd voor de geografische routering van doen? 

Ja, als de routeringsmethode van een profiel niet geografische is, kunt u de [REST-API van Azure Traffic Manager](https://docs.microsoft.com/rest/api/trafficmanager/) geografische regio's toewijzen aan eindpunten in dit profiel. In het geval van niet-geografische routering type profielen, wordt deze configuratie genegeerd. Als u dit profiel voor een geografische routering type op een later tijdstip wijzigt, kan Traffic Manager kan de toewijzingen gebruiken.


### <a name="why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic"></a>Waarom krijg ik een foutbericht wanneer ik wil de routeringsmethode van een bestaand profiel wijzigen in geografisch?

Alle eindpunten die onder een profiel met de geografische routering van moeten ten minste één regio aan is toegewezen. Als u wilt converteren een bestaand profiel naar het type van geografische routering, moet u eerst om te koppelen van de geografische regio's op de eindpunten met behulp van de [REST-API van Azure Traffic Manager](https://docs.microsoft.com/rest/api/trafficmanager/) voordat u het routeringstype in geografische wijzigt. Als u portal gebruikt, verwijdert u eerst de eindpunten, de routeringsmethode van het profiel wijzigen in geografisch en voegt u de eindpunten, samen met hun toewijzing geografische regio. 


###  <a name="why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled"></a>Waarom is het raadzaam dat klanten geneste profielen in plaats van eindpunten onder een profiel maken met de geografische routering is ingeschakeld? 

Een regio kan worden toegewezen aan slechts één eindpunt binnen een profiel als de geografische routering methode wordt gebruikt. Als dit eindpunt geen geneste type aan een onderliggende-profiel dat is gekoppeld is, als dit eindpunt niet in orde is, gaat het verkeer Manager blijft om verkeer te verzenden naar deze sinds de alternatieve niet verzenden van verkeer dat is niet een betere. Traffic Manager wordt geen failover naar een ander eindpunt, zelfs als de regio die is toegewezen fungeert als "ouder" van de regio die aan het eindpunt dat is een niet in orde fout (bijvoorbeeld, als een eindpunt waarvoor regio Spanje niet in orde komt, dat we doen geen failover naar een ander eindpunt dat is toegewezen de regio die Europa toegewezen heeft). Dit wordt gedaan om ervoor te zorgen dat de geografische grenzen dat een klant in het bijbehorende profiel is rekening gehouden met Traffic Manager. Als u het voordeel van failover wordt uitgevoerd naar een ander eindpunt als een eindpunt niet in orde gaat is, wordt aangeraden dat de geografische regio's aan geneste profielen met meerdere eindpunten binnen deze in plaats van afzonderlijke eindpunten worden toegewezen. Als een eindpunt in het profiel geneste onderliggende mislukt, verkeer op deze manier kunt failover naar een ander eindpunt binnen hetzelfde geneste onderliggende profiel.

### <a name="are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type"></a>Zijn er beperkingen voor de API-versie die ondersteuning biedt voor deze routeringstype?

Ja, alleen API-versie 2017-03-01 en nieuwere ondersteunt de geografische routering van het type. Alle oudere versies van de API kunnen worden gebruikt voor het gemaakte profielen van geografische routeringstype of geografische regio's toewijzen aan eindpunten. Als een oudere API-versie wordt gebruikt om op te halen van profielen van een Azure-abonnement, wordt een profiel van de geografische routering type wordt niet geretourneerd. Bovendien, wanneer u een oudere API-versie, een profiel geretourneerd die eindpunten zijn met de toewijzing van een geografische regio, beschikt niet over de toewijzing van de geografische regio die wordt weergegeven.

## <a name="traffic-manager-subnet-traffic-routing-method"></a>Methode voor verkeersroutering voor Traffic Manager-Subnet

### <a name="what-are-some-use-cases-where-subnet-routing-is-useful"></a>Wat zijn enkele gebruiksvoorbeelden waar subnet routering nuttig is?
Subnet routeren, kunt u onderscheid maken tussen de ervaring die om u voor specifieke sets van gebruikers die zijn geïdentificeerd door de bron-IP-adres van de IP-adres van de DNS-aanvragen te leveren. Een voorbeeld is de andere inhoud weergeven als gebruikers verbinding met een website van uw zakelijke hoofdkantoor. Een andere zou worden gebruikers beperken van bepaalde internetproviders alleen toegang hebben tot de eindpunten die ondersteuning bieden voor alleen IPv4-verbindingen als de ISP's suboptimale prestaties hebt als IPv6 wordt gebruikt.
Een andere reden voor het gebruik van de routeringsmethode Subnet is in combinatie met andere profielen in een geneste profiel ingesteld. Bijvoorbeeld, als u wilt gebruiken uw gebruikers geografische routeringsmethode voor geofencing, maar voor een specifieke provider die u wilt een andere routeringsmethode doen, kunt je een profiel withy Subnet routeringsmethode als het profiel van de bovenliggende en die Internet-provider voor het gebruik van een specifieke onderliggende pro overschrijven bestands- en standaard geografische profiel hebben voor iedereen.

### <a name="how-does-traffic-manager-know-the-ip-address-of-the-end-user"></a>Hoe weet Traffic Manager het IP-adres van de eindgebruiker
Een DNS-resolver gebruik eindgebruikerapparaten doorgaans voor de DNS-zoekactie in hun naam. De uitgaande IP-adres van dergelijke resolvers is Traffic Manager wordt weergegeven als de bron-IP-adres. Bovendien de routeringsmethode Subnet wordt ook gezocht om te zien of er EDNS0 uitgebreide Client Subnet (ECS) informatie die is doorgegeven aan de aanvraag. Als ECS informatie aanwezig is, is dat het adres dat is gebruikt om te bepalen de routering. In het ontbreken van ECS informatie, wordt de bron-IP-adres van de query gebruikt voor routeringsdoeleinden.

### <a name="how-can-i-specify-ip-addresses-when-using-subnet-routing"></a>Hoe kan ik IP-adressen opgeven bij het gebruik van Subnet routering?
De IP-adressen wilt koppelen aan een eindpunt kunnen op twee manieren worden opgegeven. U kunt de notatie quad stippellijn met decimaal octet eerst met een begin- en -adressen gebruiken om op te geven van het bereik (bijvoorbeeld 1.2.3.4-5.6.7.8 of 3.4.5.6-3.4.5.6). Ten tweede kunt u de CIDR-notatie om op te geven van het bereik (bijvoorbeeld 1.2.3.0/24). U kunt meerdere bereikwaarden opgeven en beide typen notatie kunt gebruiken in een set met bereik. Er gelden enkele beperkingen.
-   U kunt geen adresbereiken overlappen omdat elke IP moet worden toegewezen aan slechts aan één eindpunt
-   Het beginadres kan niet meer dan het eindadres
-   In het geval van de CIDR-notatie, het IP-adres voor de '/' moet het beginadres van bereik (bijvoorbeeld 1.2.3.0/24 is geldig maar 1.2.3.4.4/24 is niet geldig)

### <a name="how-can-i-specify-a-fallback-endpoint-when-using-subnet-routing"></a>Hoe kan ik een fallback-eindpunt opgeven bij het gebruik van Subnet routering?
Als u een eindpunt met geen subnetten aan is toegewezen hebt, wordt een profiel met Routering van het Subnet een aanvraag die niet met de andere eindpunten overeenkomt hier omgeleid. Het is raadzaam dat u een alternatieve eindpunt in uw profiel hebt omdat Traffic Manager wordt een antwoord NXDOMAIN als een aanvraag binnenkomt en het is niet toegewezen aan elke eindpunten of als deze is toegewezen aan een eindpunt, maar dat eindpunt niet in orde is.

### <a name="what-happens-if-an-endpoint-is-disabled-in-a-subnet-routing-type-profile"></a>Wat gebeurt er als een eindpunt is uitgeschakeld in een Subnet routering type profiel?
In een profiel met Routering van het Subnet, hebt u een eindpunt dat is uitgeschakeld, Traffic Manager gedraagt zich alsof dit eindpunt en de subnet-toewijzingen heeft bestaat niet. Als een query die zou hebben overeenkomt met de IP-adrestoewijzing wordt ontvangen en het eindpunt is uitgeschakeld, wordt in Traffic Manager een fallback-eindpunt (één met geen toewijzingen) wordt geretourneerd of als deze een eindpunt niet aanwezig is is, wordt een antwoord NXDOMAIN.

## <a name="traffic-manager-multivalue-traffic-routing-method"></a>Methode voor verkeersroutering voor Traffic Manager met meerdere waarden

### <a name="what-are-some-use-cases-where-multivalue-routing-is-useful"></a>Wat zijn gebruiksdoeleinden waarin met meerdere waarden routering nuttig is?
Meerdere eindpunten in orde-routering met meerdere waarden worden geretourneerd in een enkele query-antwoord. Het belangrijkste voordeel hiervan is dat, als een eindpunt niet in orde is, de client meer opties zonder dat een andere DNS-aanroep (die mogelijk dezelfde waarde retourneren van een upstream-cache) opnieuw uit te voeren. Dit is van toepassing voor gevoelige toepassingen beschikbaarheid die u wilt de downtime te minimaliseren.
Een andere toepassing voor meerdere waarden routeringsmethode is als een eindpunt 'dual-homed' naar zowel IPv4 is en IPv6-adressen en u beide opties wilt om de verkeersbelasting bij het maken van verbinding met het eindpunt van de oproepende functie geven.

### <a name="how-many-endpoints-are-returned-when-multivalue-routing-is-used"></a>Het aantal eindpunten worden geretourneerd wanneer met meerdere waarden routering wordt gebruikt?
Kunt u het maximum aantal eindpunten moet worden geretourneerd en meerdere waarden retourneert niet vaker dan die veel eindpunten in orde wanneer een query wordt ontvangen. De maximaal mogelijke waarde voor deze configuratie is 10.

### <a name="will-i-get-the-same-set-of-endpoints-when-multivalue-routing-is-used"></a>Krijg ik dezelfde set eindpunten wanneer met meerdere waarden routering wordt gebruikt?
We kunnen niet garanderen dat de dezelfde set met eindpunten in elke query wordt geretourneerd. Dit wordt ook beïnvloed door het feit dat mogelijk enkele van de eindpunten gaan op het moment waarop ze niet in het antwoord opgenomen worden niet in orde

## <a name="real-user-measurements"></a>Real-user-metingen

### <a name="what-are-the-benefits-of-using-real-user-measurements"></a>Wat zijn de voordelen van het gebruik van Real User Measurements?
Wanneer u routeringsmethode voor prestaties, Traffic Manager haalt het beste Azure-regio voor uw eindgebruikers verbinding maken met door te inspecteren van de bron-IP en Subnet van de Client EDNS (als die wordt doorgegeven in) en controleren op basis van de netwerklatentie-informatie de service onderhoudt. Real User Measurements verbetert dit doordat hun ervaring die bijdragen aan deze tabel latentie naast ervoor te zorgen dat deze tabel omvat de eindgebruiker netwerken waar uw eindgebruikers verbinding met Azure maken voor uw end-gebruikersgroep. Dit leidt tot een grotere nauwkeurigheid van de routering van de eindgebruiker.

### <a name="can-i-use-real-user-measurements-with-non-azure-regions"></a>Kan ik Real User Measurements gebruiken met niet-Azure-regio's?
Real-User-metingen metingen en rapporten over alleen de latentie te bereiken van Azure-regio's. Als u routering op basis van prestaties met eindpunten die worden gehost in niet-Azure-regio's gebruikt, kunt u nog steeds profiteren van deze functie door gestegen latentiegegevens over de representatieve Azure-regio die u hebt geselecteerd om te worden gekoppeld aan dit eindpunt.

### <a name="which-routing-method-benefits-from-real-user-measurements"></a>Welke methode u routering voordelen van Real User Measurements?
De aanvullende informatie die is opgedaan met Real-User-metingen zijn alleen van toepassing op de profielen die gebruikmaken van de routeringsmethode voor prestaties. De koppeling Real User Measurements is beschikbaar in alle profielen bekijkt via Azure portal.

### <a name="do-i-need-to-enable-real-user-measurements-each-profile-separately"></a>Heb ik nodig om in te schakelen Real User Measurements elk profiel afzonderlijk?
Nee, moet u slechts één keer per abonnement inschakelen en alle latentiegegevens gemeten en gerapporteerd, zijn beschikbaar voor alle profielen.

### <a name="how-do-i-turn-off-real-user-measurements-for-my-subscription"></a>Hoe schakel ik uit Real User Measurements voor mijn abonnement?
U kunt stoppen lopen de kosten met betrekking tot Real User Measurements wanneer u stopt met het verzamelen en verzenden van back-replicatielatentie van uw clienttoepassing. Bijvoorbeeld, als meting JavaScript in webpagina's ingesloten, kunt u stoppen met behulp van deze functie door het verwijderen van de JavaScript of door het uitschakelen van de aanroep als de pagina wordt weergegeven.

U kunt ook Real User Measurements uitschakelen door het verwijderen van uw sleutel. Nadat u de sleutel verwijdert, worden alle metingen naar Traffic Manager verzonden met die sleutel verwijderd.

### <a name="can-i-use-real-user-measurements-with-client-applications-other-than-web-pages"></a>Kan ik Real User Measurements met clienttoepassingen dan webpagina's gebruiken?
Ja, Real User Measurements is ontworpen voor opname van gegevens die worden verzameld via verschillende type van de eindgebruiker clients. Deze Veelgestelde vragen wordt bijgewerkt als nieuwe typen van clienttoepassingen get wordt ondersteund.

### <a name="how-many-measurements-are-made-each-time-my-real-user-measurements-enabled-web-page-is-rendered"></a>Hoeveel metingen van telkens wanneer die mijn Real User Measurements ingeschakeld webpagina wordt weergegeven?
Als Real User Measurements wordt gebruikt met de meting JavaScript opgegeven, wordt de rendering van elke pagina resulteert in zes metingen worden uitgevoerd. Deze worden vervolgens terug gerapporteerd aan de service Traffic Manager. U betaalt voor deze functie op basis van het aantal metingen die zijn gerapporteerd aan Traffic Manager-service. Bijvoorbeeld, als de gebruiker weg van de webpagina navigeert terwijl de metingen worden gemaakt, maar voordat deze is gemeld, deze metingen zijn niet in aanmerking voor factureringsdoeleinden.

### <a name="is-there-a-delay-before-real-user-measurements-script-runs-in-my-webpage"></a>Is er een vertraging optreden voordat Real User Measurements script wordt uitgevoerd in mijn webpagina?
Nee, er is geen geprogrammeerde vertraging voordat het script wordt aangeroepen.

### <a name="can-i-use-real-user-measurements-with-only-the-azure-regions-i-want-to-measure"></a>Kan ik Real User Measurements gebruiken met alleen de Azure-regio's die kan ik wilt meten?
Nee, elke keer die de toepassing wordt aangeroepen, meet het script Real User Measurements een set met zes Azure-regio's zoals wordt bepaald door de service. Deze wijzigingen tussen verschillende aanroepen ingesteld en wanneer er een groot aantal van dergelijke aanroepen optreden, de dekking van de meting zich uitstrekt over verschillende Azure-regio's.

### <a name="can-i-limit-the-number-of-measurements-made-to-a-specific-number"></a>Kan ik het aantal metingen naar een specifiek getal beperken?
De meting JavaScript is ingesloten in uw webpagina en zijn in de volledige controle over te starten en stoppen met het. Als de service Traffic Manager een aanvraag ontvangt voor een lijst met Azure-regio's te meten, een reeks regio's worden geretourneerd.

### <a name="can-i-see-the-measurements-taken-by-my-client-application-as-part-of-real-user-measurements"></a>Kan ik de metingen door de clienttoepassing als onderdeel van Real User Measurements zien?
Omdat de logica van de meting vanuit de clienttoepassing wordt uitgevoerd, bent u volledige controle van wat er gebeurt met inbegrip van de replicatielatentie zien. Traffic Manager rapporteert niet een samengevoegde weergave van de metingen ontvangen onder de sleutel die is gekoppeld aan uw abonnement.

### <a name="can-i-modify-the-measurement-script-provided-by-traffic-manager"></a>Kan ik de meting-script dat is opgegeven door Traffic Manager wijzigen?
Wanneer u controle over wat er op uw webpagina is ingesloten, ontmoedigen we u ten zeerste van u wijzigingen aanbrengt in het script meting om ervoor te zorgen dat het meet en de latenties goed rapporteert.

### <a name="will-it-be-possible-for-others-to-see-the-key-i-use-with-real-user-measurements"></a>Is het mogelijk zijn voor anderen kunnen zien van de sleutel die ik met Real User Measurements gebruiken?
Wanneer u het script meting naar een webpagina insluit is het mogelijk zijn voor anderen kunnen zien van het script en de sleutel van uw echte gebruiker metingen (RUM). Maar het is belangrijk te weten dat deze sleutel af van uw abonnements-id wijkt en wordt gegenereerd door Traffic Manager alleen voor dit doel worden gebruikt. De veiligheid van uw Azure-account niet in gevaar brengt wetenschap dat uw sleutel uitvoeren.

### <a name="can-others-abuse-my-rum-key"></a>Anderen misbruiken vaak de mijn sleutel uitvoeren?
Het is mogelijk dat anderen uw sleutel te gebruiken voor het verzenden van onjuiste gegevens naar Azure, wordt in een paar verkeerde metingen niet wijzigen de routering omdat deze wordt gehouden, samen met alle andere metingen dat we ontvangen. Als u uw sleutels wijzigen wilt, kunt u de sleutel op het moment waarop de oude sleutel wordt genegeerd opnieuw genereren.

###  <a name="do-i-need-to-put-the-measurement-javascript-in-all-my-web-pages"></a>Heb ik nodig om de meting JavaScript in mijn webpagina's?
Real User Measurements biedt meer waarde als het aantal toename van metingen. Echter is het beslissen of u moet plaatst deze in uw webpagina's of selecteert u een enkele. Onze aanbeveling is gestart door de gegevens in uw meest bezochte pagina waar een gebruiker om te blijven op die pagina vijf seconden of langer wordt verwacht.

### <a name="can-information-about-my-end-users-be-identified-by-traffic-manager-if-i-use-real-user-measurements"></a>Informatie over mijn eindgebruikers worden geïdentificeerd door Traffic Manager als ik gebruikmaak van Real User Measurements?
Als de opgegeven meting JavaScript wordt gebruikt, wordt Traffic Manager inzicht hebben in het client-IP-adres van de eindgebruiker en de bron-IP-adres van de lokale DNS-resolver die ze gebruiken. Traffic Manager maakt gebruik van de IP-adres van de client alleen nadat ik deze afgebroken zodat het niet mogelijk om te bepalen van de specifieke gebruiker die de metingen verzonden. 

### <a name="does-the-webpage-measuring-real-user-measurements-need-to-be-using-traffic-manager-for-routing"></a>De webpagina van de meting Real User Measurements worden met behulp van Traffic Manager moet voor de routering verandert?
Nee, hoeft niet te gebruiken van Traffic Manager. De routering-zijde van Traffic Manager werkt afzonderlijk van de echte gebruiker meting deel en, maar dit is een goed idee om te hebben ze beide in dezelfde webeigenschap niet hoeven te worden.

### <a name="do-i-need-to-host-any-service-on-azure-regions-to-use-with-real-user-measurements"></a>Heb ik nodig voor het hosten van elke service op Azure-regio's voor gebruik met Real User Measurements?
Nee, u niet nodig voor het hosten van een server-side-onderdeel op Azure voor Real-User-metingen om te werken. De installatiekopie van één pixel gedownload door de meting JavaScript en de service uitgevoerd in verschillende Azure-regio's wordt gehost en beheerd door Azure. 

### <a name="will-my-azure-bandwidth-usage-increase-when-i-use-real-user-measurements"></a>Worden mijn Azure bandbreedtegebruik verhoogd wanneer ik Real User Measurements gebruik?
Zoals vermeld in het vorige antwoord, zijn de serveronderdelen van Real User Measurements eigendom en worden beheerd door Azure. Dit betekent dat uw bandbreedtegebruik van Azure niet worden verhoogd omdat u gebruikmaakt van Real User Measurements. Dit omvat alle bandbreedtegebruik buiten wat Azure kosten in rekening gebracht. We beperken de bandbreedte die wordt gebruikt door het downloaden van alleen een installatiekopie van één pixel meting de latentie aan een Azure-regio. 

## <a name="traffic-view"></a>Verkeersweergave

### <a name="what-does-traffic-view-do"></a>Wat is Traffic View?
Traffic View is een functie van de Traffic-Manager waarmee u meer weten over uw gebruikers en hoe hun ervaring is. Het maakt gebruik van de query's ontvangen door Traffic Manager en het netwerk latentie intelligence tabellen die de service onderhoudt waarmee u het volgende:
- De regio's waar uw gebruikers zijn verbinding kunnen maken met uw eindpunten in Azure.
- Het volume van gebruikers die verbinding maakt vanaf deze regio's.
- De Azure-regio's waarnaar ze ophalen doorgestuurd naar.
- De latentie-ervaring op deze Azure-regio's.

Deze informatie is beschikbaar voor u om te gebruiken via geografische kaart overlay en in tabelvorm weergaven in de portal naast beschikbaarheid als onbewerkte gegevens voor u om te downloaden.

### <a name="how-can-i-benefit-from-using-traffic-view"></a>Hoe kan ik profiteren van Verkeersweergave?

Verkeersweergave biedt u de algehele weergave van het verkeer ontvangen van uw Traffic Manager-profielen. In het bijzonder, kan deze worden gebruikt om te zien waar uw gebruikersgroep verbindt uit en even belangrijker wat hun ervaring gemiddelde latentie is. U kunt deze informatie vervolgens gebruiken om te zoeken waarin u richten wilt, bijvoorbeeld door het uitbreiden van uw Azure-voetafdruk naar een regio die gebruikers kan worden gebruikt met een lagere latentie gebieden. Een andere inzichten die kunnen worden afgeleid van het gebruik van Traffic View is om te zien van de patronen van verkeer naar verschillende regio's die op zijn beurt u om beslissingen te nemen helpen kunnen in oplopende of aflopende volgorde manieren in die regio's.

### <a name="how-is-traffic-view-different-from-the-traffic-manager-metrics-available-through-azure-monitor"></a>Wat is Traffic View verschil met het Traffic Manager metrische gegevens beschikbaar zijn via Azure monitor?

Azure Monitor kan worden gebruikt om te begrijpen op een hoger niveau van het verkeer dat is ontvangen door uw profiel en de eindpunten. Ook kunt u de status van de eindpunten te houden bij het blootstellen van de resultaten van de status. Als u verder gaan dan deze en begrijpen van de eindgebruiker ervaring verbinden met Azure op het niveau van een regionale wilt, kan Traffic View te bereiken die worden gebruikt.

### <a name="does-traffic-view-use-edns-client-subnet-information"></a>Traffic View maakt gebruik van informatie over het EDNS Client Subnet?

De DNS-query's die is geleverd door Azure Traffic Manager moeten u ECS-informatie op de nauwkeurigheid van de routering vergroten. Maar bij het maken van de gegevensset die laat waar de gebruikers verbinding maakt zien vanaf, alleen het IP-adres van de DNS-resolver wordt gebruikt voor Verkeersweergave.

### <a name="how-many-days-of-data-does-traffic-view-use"></a>Hoeveel dagen aan gegevens Traffic View gebruiken?

Traffic View wordt de uitvoer door het verwerken van de gegevens van de zeven dagen vóór de dag vóór wanneer het wel is bekeken door u gemaakt. Dit is een zwevend venster en elke keer dat u naar wordt gebruikt door de meest recente gegevens.

### <a name="how-does-traffic-view-handle-external-endpoints"></a>Hoe wordt Traffic View externe eindpunten verwerkt?

Wanneer u externe eindpunten die worden gehost buiten Azure-regio's in een Traffic Manager-profiel kunt u kiezen om dit toegewezen aan een Azure-regio die fungeert als proxy voor de latentie-kenmerken (dit is in feite nodig als u de routeringsmethode performance). Als dat zo de toewijzing van deze Azure-regio is, wordt metrieken voor latentie bij die Azure regio gebruikt bij het maken van de Traffic View-uitvoer. Als er geen Azure-regio is opgegeven, is de latentie-informatie is leeg in de gegevens voor deze externe eindpunten.

### <a name="do-i-need-to-enable-traffic-view-for-each-profile-in-my-subscription"></a>Moet ik de weergave verkeer inschakelen voor elk uitvoeringsprofiel in mijn abonnement?

Traffic View is tijdens de preview-periode ingeschakeld op het abonnementsniveau van een. Als onderdeel van de verbeteringen die we voor de algemene beschikbaarheid hebt aangebracht, kunt u nu Traffic View op het niveau van een profiel waarmee u meer gedetailleerde inschakelen van deze functie inschakelen. Traffic View wordt standaard uitgeschakeld voor een profiel.

>[!NOTE]
>Als u tijdens de preview-periode Traffic View op het abonnementsniveau van een hebt ingeschakeld, moet u nu opnieuw inschakelen voor elk van het profiel dat aan het abonnement.
 
### <a name="how-can-i-turn-off-traffic-view"></a>Hoe kan ik Verkeersweergave uitschakelen? 
U kunt de Verkeersweergave uitschakelen voor een profiel met behulp van de Portal of REST-API. 

### <a name="how-does-traffic-view-billing-work"></a>Hoe werkt facturering voor Traffic View?

Traffic View prijzen zijn gebaseerd op het aantal gegevenspunten dat wordt gebruikt om te maken van de uitvoer. Momenteel het enige type ondersteund door de query's uw profiel ontvangt. U wordt bovendien alleen gefactureerd voor de verwerking die is uitgevoerd wanneer u Traffic View is ingeschakeld. Dit betekent dat, als u de weergave verkeer voor een bepaalde tijd in een maand inschakelen en tijdens in andere gevallen uitschakelen, alleen de gegevenspunten verwerkt sinds u de functie count op uw factuur ingeschakeld.

## <a name="traffic-manager-endpoints"></a>Traffic Manager-eindpunten

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Kan ik Traffic Manager gebruiken met de eindpunten van meerdere abonnementen?

Met behulp van eindpunten vanuit meerdere abonnementen is niet mogelijk met Azure Web Apps. Azure Web Apps vereist dat een aangepaste domeinnaam gebruikt in combinatie met de Web-Apps alleen worden gebruikt binnen één abonnement. Het is niet mogelijk met gebruik van Web-Apps uit meerdere abonnementen met dezelfde domeinnaam.

Voor andere eindpunttypen is het mogelijk om te gebruiken van Traffic Manager met de eindpunten van meer dan één abonnement. In Resource Manager-eindpunten voor elk abonnement kunnen worden toegevoegd als naar Traffic Manager, zolang de persoon die de Traffic Manager-profiel configureren, leestoegang tot het eindpunt heeft. Deze machtigingen worden verleend met behulp van [Azure Resource Manager-rol gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/role-assignments-portal.md).


### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Kan ik Traffic Manager gebruiken met de Service in de Cloud 'Staging' sleuven?

Ja. Cloudservice 'staging' sleuven kan in Traffic Manager worden geconfigureerd als externe eindpunten. Statuscontroles zijn nog steeds in rekening gebracht tegen het tarief voor Azure-eindpunten. Omdat het externe eindpunt in gebruik is, worden wijzigingen in de onderliggende service niet automatisch doorgevoerd. Traffic Manager kan met externe eindpunten detecteren wanneer de Cloudservice is gestopt of verwijderd. Traffic Manager wordt daarom facturering voor statuscontroles voortgezet tot het eindpunt is uitgeschakeld of verwijderd.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Traffic Manager biedt ondersteuning voor IPv6-eindpunten?

Traffic Manager biedt momenteel geen IPv6-addressible naamservers. Traffic Manager kan echter nog steeds worden gebruikt door de IPv6-clients verbinding maken met IPv6-eindpunten. Een client maakt DNS-aanvragen niet rechtstreeks aan Traffic Manager. De client gebruikt in plaats daarvan een recursieve DNS-service. Een IPv6-client verzendt aanvragen naar de recursieve DNS-service via IPv6. De recursieve-service moet vervolgens verbinding kunnen maken met de naamservers van Traffic Manager met behulp van IPv4.

Traffic Manager reageert met de DNS-naam of IP-adres van het eindpunt. Ter ondersteuning van een IPv6-eindpunt, zijn er twee opties. U kunt het eindpunt toevoegen als een DNS-naam met een gekoppelde AAAA-record en Traffic Manager wordt controle van gatewayservicestatus dat eindpunt en als een CNAME-record typt in het query-antwoord geretourneerd. U kunt ook een eindpunt toevoegen direct met behulp van de IPv6-adres en Traffic Manager een AAAA-record type in het queryantwoord wordt geretourneerd. 

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Kan ik Traffic Manager gebruiken met meer dan één Web-App in dezelfde regio?

Traffic Manager wordt meestal gebruikt om verkeer naar toepassingen die zijn geïmplementeerd in verschillende regio's. Maar kan het ook worden gebruikt waarbij een toepassing meer dan één implementatie heeft in dezelfde regio. De Azure Traffic Manager-eindpunten niet meer dan één eindpunt van de Web-App toe in dezelfde Azure-regio worden toegevoegd aan dezelfde Traffic Manager-profiel.

### <a name="how-do-i-move-my-traffic-manager-profiles-azure-endpoints-to-a-different-resource-group"></a>Hoe verplaats ik mijn Traffic Manager-profiel Azure-eindpunten naar een andere resourcegroep?

Azure-eindpunten die gekoppeld aan een Traffic Manager-profiel zijn worden bijgehouden met behulp van de resource-id's. Als een Azure-resource die wordt gebruikt als een eindpunt (bijvoorbeeld: openbare IP-adres, klassieke Cloudservice, Web-App of een andere Traffic Manager-profiel gebruikt in een geneste manier) wordt verplaatst naar een andere resourcegroep, de resource-ID wordt gewijzigd. In dit scenario, moet u op dit moment de Traffic Manager-profiel door eerst verwijderd en vervolgens toe te voegen opnieuw de eindpunten in het profiel bijwerken. 

##  <a name="traffic-manager-endpoint-monitoring"></a>Eindpuntcontrole van Traffic Manager

### <a name="is-traffic-manager-resilient-to-azure-region-failures"></a>Traffic Manager Azure-regio opslagbuscache is?

Traffic Manager is een belangrijk onderdeel van de levering van toepassingen met hoge beschikbaarheid in Azure.
Met het oog op hoge beschikbaarheid moet Traffic Manager een uitzonderlijk hoog niveau van beschikbaarheid en moet een bestand is tegen regionale fouten.

Traffic Manager-onderdelen zijn standaard, tegen een volledige uitval van een Azure-regio. Deze flexibiliteit is van toepassing op alle Traffic Manager-onderdelen: de DNS-naam-servers, de API, de storage-laag en het eindpunt van de service te controleren.

In het onwaarschijnlijke geval van een storing van een hele Azure-regio, Traffic Manager naar verwachting blijven normaal werken. Toepassingen die zijn geïmplementeerd in meerdere Azure-regio's vertrouwen op Traffic Manager om verkeer naar een beschikbare instantie van hun toepassing te regelen.

### <a name="how-does-the-choice-of-resource-group-location-affect-traffic-manager"></a>Hoe beïnvloedt de keuze van de locatie voor resourcegroep Traffic Manager?

Traffic Manager is een enkele, wereldwijde service. Het is niet regionaal. De keuze van de locatie voor resourcegroep heeft geen invloed op Traffic Manager-profielen die zijn geïmplementeerd in die resourcegroep.

Azure Resource Manager is vereist voor alle resourcegroepen een locatie, waarmee wordt bepaald hoe de standaardlocatie voor resources die zijn geïmplementeerd in die resourcegroep op te geven. Wanneer u een Traffic Manager-profiel maakt, wordt deze in een resourcegroep gemaakt. Alle Traffic Manager-profielen gebruiken **globale** overschrijven als de locatie, de standaardwaarde van de groep resource.

### <a name="how-do-i-determine-the-current-health-of-each-endpoint"></a>Hoe bepaal ik de huidige status van elk eindpunt?

De huidige bewakingsstatus van elk eindpunt, naast de algemene profiel wordt weergegeven in de Azure-portal. Deze informatie is ook beschikbaar via de Monitor verkeer [REST-API](https://msdn.microsoft.com/library/azure/mt163667.aspx), [PowerShell-cmdlets](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager), en [platformoverschrijdende Azure CLI](../cli-install-nodejs.md).

U kunt ook Azure Monitor gebruiken bij te houden van de status van uw eindpunten en een visuele representatie van deze zien. Zie voor meer informatie over het gebruik van Azure Monitor, de [documentatie Azure Monitoring](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics).

### <a name="can-i-monitor-https-endpoints"></a>Kan ik de HTTPS-eindpunten controleren?

Ja. Traffic Manager biedt ondersteuning voor scannen via HTTPS. Configureer **HTTPS** als het protocol in de configuratie van de bewaking.

Traffic manager biedt geen een validatie van het servercertificaat, met inbegrip van:

* Server-side-certificaten worden niet gevalideerd
* SNI-serverzijde certificaten worden niet ondersteund
* Clientcertificaten worden niet ondersteund.

### <a name="do-i-use-an-ip-address-or-a-dns-name-when-adding-an-endpoint"></a>Gebruik ik een IP-adres of een DNS-naam bij het toevoegen van een eindpunt?
Traffic Manager ondersteunt het gebruik van eindpunten met behulp van drie manieren om te verwijzen ze – als een DNS-naam, als een IPv4-adres en als een IPv6-adres. Als het eindpunt wordt toegevoegd als een IPv4- of IPv6-adres het query-antwoord niet van het type record A of AAAA, respectievelijk. Als het eindpunt is toegevoegd als een DNS-naam, worden de query-antwoord van CNAME-recordtype. Eindpunten toevoegen, als IPv4 of IPv6-adres is alleen toegestaan als het eindpunt van het type is **externe**.
Alle methoden Routering en controle-instellingen worden ondersteund door de drie typen van de eindpunt-adressering.

### <a name="what-types-of-ip-addresses-can-i-use-when-adding-an-endpoint"></a>Welke typen IP-adressen kan ik gebruiken bij het toevoegen van een eindpunt?
Traffic Manager kunt u IPv4 of IPv6-adressen gebruiken om op te geven van eindpunten. Er zijn enkele beperkingen die hieronder worden:
- Adressen die overeenkomen met de gereserveerde privé IP-adresruimten zijn niet toegestaan. Deze adressen zijn apparaten die worden beschreven in RFC 1918, RFC 6890, RFC 5737, RFC 3068, RFC 2544 en RFC 5771
- Het adres mag poortnummers (u kunt de poorten die worden gebruikt in de configuratie-instellingen van het certificaatprofiel opgeven) 
- Er zijn geen eindpunten in hetzelfde profiel kunnen hebben hetzelfde doel-IP-adres

### <a name="can-i-use-different-endpoint-addressing-types-within-a-single-profile"></a>Kan ik een ander eindpunt adressering typen binnen één profiel gebruiken?
Nee, Traffic Manager kunt u geen om adressering typen in een profiel, met uitzondering van het geval van een profiel met met meerdere waarden routeringstype waar u met IPv4 combineren kunt en IPv6-adressen typen eindpunten

### <a name="what-happens-when-an-incoming-querys-record-type-is-different-from-the-record-type-associated-with-the-addressing-type-of-the-endpoints"></a>Wat gebeurt er wanneer een binnenkomende query recordtype af van het recordtype die zijn gekoppeld aan het adresschema type van de eindpunten wijkt?
Wanneer een query wordt ontvangen op basis van een profiel, wordt het eindpunt dat moet worden geretourneerd aan de hand van de routeringsmethode die is opgegeven en de status van de eindpunten eerst Traffic Manager gevonden. Deze vervolgens kijkt naar het recordtype die is opgegeven in de binnenkomende query en het recordtype die zijn gekoppeld aan het eindpunt voordat een antwoord op basis van de onderstaande tabel wordt geretourneerd.

Voor profielen met de routeringsmethode die dan met meerdere waarden:
|Binnenkomende queryaanvraag|    Eindpunttype|  Respons|
|--|--|--|
|ALLE |  A / AAAA / CNAME |  Doel-eindpunt| 
|A |    A / CNAME | Doel-eindpunt|
|A |    AAAA |  NODATA |
|AAAA | AAAA / CNAME |  Doel-eindpunt|
|AAAA | A | NODATA |
|CNAME |    CNAME | Doel-eindpunt|
|CNAME  |A / AAAA | NODATA |
|
Voor profielen met routeringsmethode ingesteld op meerdere waarden:

|Binnenkomende queryaanvraag|    Eindpunttype | Respons|
|--|--|--|
|ALLE |  Combinatie van A en AAAA | Doeleindpunten|
|A |    Combinatie van A en AAAA | Alleen doeleindpunten van type A|
|AAAA   |Combinatie van A en AAAA|     Alleen doeleindpunten van het type AAAA|
|CNAME |    Combinatie van A en AAAA | NODATA |

### <a name="can-i-use-a-profile-with-ipv4--ipv6-addressed-endpoints-in-a-nested-profile"></a>Kan ik een profiel gebruiken met IPv4 / IPv6-eindpunten in een geneste profiel opgelost?
Ja, u kunt met de uitzondering dat een profiel van het type met meerdere waarden mag niet een bovenliggende-profiel in een geneste profiel instellen.


### <a name="i-stopped-an-azure-cloud-service--web-application-endpoint-in-my-traffic-manager-profile-but-i-am-not-receiving-any-traffic-even-after-i-restarted-it-how-can-i-fix-this"></a>Ik ben gestopt met een Azure-cloud-service / web-toepassingseindpunt in mijn Traffic Manager-profiel, maar ik krijg geen verkeer dat is zelfs nadat ik deze opnieuw opgestart. Hoe kan ik dit oplossen?

Wanneer een Azure cloud service / web-toepassingseindpunt wordt gestopt Traffic Manager stopt de status controleren en de statuscontrole opnieuw wordt opgestart nadat er wordt gedetecteerd dat het eindpunt opnieuw is opgestart. Om te voorkomen dat deze vertraging, uitschakelen en vervolgens dat eindpunt in Traffic Manager-profiel weer inschakelen nadat u het eindpunt opnieuw starten.   

### <a name="can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https"></a>Kan ik Traffic Manager, zelfs als de toepassing heeft geen ondersteuning voor HTTP of HTTPS gebruiken?

Ja. Kunt u TCP als protocol voor bewaking en Traffic Manager kan een TCP-verbinding tot stand brengen en wachten op een reactie van het eindpunt. Als het eindpunt van de antwoorden op de verbindingsaanvraag is met een antwoord voor het maken van de verbinding, binnen de time-outperiode, is dit eindpunt als in orde gemarkeerd.

### <a name="what-specific-responses-are-required-from-the-endpoint-when-using-tcp-monitoring"></a>Welke specifieke antwoorden zijn vereist van het eindpunt bij het gebruik van TCP-controle?

Wanneer TCP-bewaking wordt gebruikt, wordt in Traffic Manager een TCP-handshake drie richtingen begint met het verzenden van een SYN-aanvraag naar eindpunt op de opgegeven poort. Vervolgens wordt gewacht voor een bepaalde periode (zoals aangegeven in de time-outinstellingen) voor een reactie van het eindpunt. Als het eindpunt op de SYN-aanvraag met een antwoord SYN-ACK binnen de time-outperiode die is opgegeven in de instellingen voor controle reageert, wordt dit eindpunt in orde beschouwd. Als het SYN-ACK-antwoord wordt ontvangen, Traffic Manager de verbinding wordt hersteld door opnieuw met een eerste te reageren.

### <a name="how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint"></a>Hoe snel Mijn gebruikers weg van een slechte status eindpunt in Traffic Manager verplaatsen?

Traffic Manager biedt meerdere instellingen waarmee u voor het beheren van het gedrag bij een failover van uw Traffic Manager-profiel als volgt:
- u kunt opgeven dat de Traffic Manager de eindpunten vaker tests door het instellen van het scannen van Interval op 10 seconden. Dit zorgt ervoor dat elk willekeurig eindpunt dat niet in orde gaan zo snel mogelijk kan worden gedetecteerd. 
- u kunt opgeven hoe lang moet worden gewacht voordat een health Bekijk aanvraagtijden (minimale time-outwaarde is 5 per seconde).
- u kunt opgeven hoeveel fouten kunnen optreden voordat het eindpunt is gemarkeerd als niet in orde. Deze waarde kan zijn laag 0, in dat geval het eindpunt als slecht is gemarkeerd als de eerste statuscontrole is mislukt. Echter kan met behulp van de minimale waarde van 0 voor de geaccepteerd aantal fouten leiden tot eindpunten uit rotatie vanwege een tijdelijke problemen die op het moment van probing optreden kunnen wordt gehaald.
- u kunt de time-to-live (TTL) opgeven voor de DNS-antwoord moet zo laag 0. Hierdoor betekent dat dat DNS-resolvers de reactie kunnen niet in cache en elke nieuwe query een antwoord wordt bevat dat de meest recente statusgegevens met Traffic Manager.

Met behulp van deze instellingen, krijgt Traffic Manager u een failover onder 10 seconden nadat een eindpunt niet in orde komt en een DNS-query wordt uitgevoerd op basis van het bijbehorende profiel.

### <a name="how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile"></a>Hoe kan ik verschillende controle-instellingen voor verschillende eindpunten opgeven in een profiel?

Traffic Manager-instellingen voor controle zijn op een per profiel niveau. Als u nodig hebt met een ander controle-instelling voor slechts één eindpunt, deze kan worden gedaan door dit eindpunt als een [genest profiel](traffic-manager-nested-profiles.md) waarvan bewakingsinstellingen verschillen van het profiel van de bovenliggende.

### <a name="how-can-i-assign-http-headers-to-the-traffic-manager-health-checks-to-my-endpoints"></a>Hoe kan ik van HTTP-headers op het Traffic Manager statuscontroles naar Mijn eindpunten toewijzen?
Traffic Manager kunt u aangepaste kopteksten in de HTTP (S) statuscontroles met dat het maken van uw eindpunten opgeven. Als u opgeven van een aangepaste header wilt, kunt u dat doen op het niveau van het profiel (van toepassing zijn op alle eindpunten) of op het niveau van het eindpunt opgeven. Als een header is gedefinieerd op beide niveaus, wordt het niveau van het profiel een overschreven door de versie die is opgegeven op het niveau van het eindpunt.
Een veelvoorkomende use-case voor dit is hostheaders op te geven zodat Traffic Manager-aanvragen kunnen correct ophalen doorgestuurd naar een eindpunt die wordt gehost in een omgeving met meerdere tenants. Een andere gebruiksvoorbeeld hiervan is het identificeren van Traffic Manager-aanvragen uit van een eindpunt-logboeken voor HTTP (S)-aanvraag

## <a name="what-host-header-do-endpoint-health-checks-use"></a>Welke host-header wilt eindpunt statuscontroles gebruiken?
Als er geen aangepaste host-header-instelling is opgegeven, is de host-header die door Traffic Manager gebruikt de DNS-naam van de doel-eindpunt in het profiel is geconfigureerd als die beschikbaar is. 


### <a name="what-are-the-ip-addresses-from-which-the-health-checks-originate"></a>Wat zijn de IP-adressen van waaruit de statuscontroles afkomstig zijn?

Klik op [hier](https://azuretrafficmanagerdata.blob.core.windows.net/probes/azure/probe-ip-ranges.json) om weer te geven van het JSON-bestand met een lijst met de IP-adressen van welke Traffic Manager statuscontroles kunnen afkomstig zijn. Bekijk de IP-adressen die worden vermeld in het JSON-bestand om ervoor te zorgen dat binnenkomende verbindingen van deze IP-adressen zijn toegestaan op de eindpunten om de status van de status te controleren.

### <a name="how-many-health-checks-to-my-endpoint-can-i-expect-from-traffic-manager"></a>Het aantal statuscontroles met mijn eindpunt kan ik verwachten van Traffic Manager?

Het nummer van de gezondheid van Traffic Manager controleert bereikt van het eindpunt is afhankelijk van het volgende:
- de waarde die u hebt ingesteld voor de controle-interval (kleinere interval betekent meer aanvragen op uw eindpunt terechtkomen in een bepaalde periode).
- het aantal locaties van waaruit de statuscontroles uitgevoerd (de IP-adressen uit waar u kunt verwachten deze controles wordt vermeld in de voorgaande Veelgestelde vragen).

### <a name="how-can-i-get-notified-if-one-of-my-endpoints-goes-down"></a>Hoe kan ik blijf op de hoogte als een van mijn eindpunten uitgeschakeld wordt? 
Een van de metrische gegevens die door Traffic Manager is de status van eindpunten in een profiel. U kunt dit zien als een statistische functie van alle eindpunten binnen een profiel (bijvoorbeeld: 75% van de eindpunten in orde zijn), of op een per eindpunt niveau. Traffic Manager metrische gegevens worden weergegeven via Azure Monitor en kunt u de [waarschuwingsfuncties](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) meldingen wilt ontvangen wanneer er een wijziging in de status van uw eindpunt. Zie voor meer informatie, [Traffic Manager-statistieken en -waarschuwingen](traffic-manager-metrics-alerts.md).  

## <a name="traffic-manager-nested-profiles"></a>Traffic Manager-geneste profielen

### <a name="how-do-i-configure-nested-profiles"></a>Hoe configureer ik geneste profielen

Geneste Traffic Manager-profielen kunnen worden geconfigureerd met behulp van zowel de Azure Resource Manager en de klassieke Azure REST API's, Azure PowerShell-cmdlets en platformoverschrijdende Azure CLI-opdrachten. Ze worden ook ondersteund via de nieuwe Azure portal.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Ondersteuning voor het aantal lagen van geneste kiest, wordt er Traffic Manager?

U kunt profielen maximaal 10 niveaus diep genest. De lus' zijn niet toegestaan.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Kan ik andere eindpunttypen combineren met geneste onderliggende profielen, in dezelfde Traffic Manager-profiel?

Ja. Er zijn geen beperkingen voor hoe u de eindpunten van verschillende typen in een profiel combineren.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Hoe is het factureringsmodel voor geneste profielen toepassing?

Er is geen negatieve gevolgen van het gebruik van geneste profielen voor de prijs.

Facturering voor Traffic Manager bestaat uit twee onderdelen: van eindpunten en miljoenen DNS-query's

* Statuscontroles eindpunt: Er zijn geen kosten voor een onderliggende profiel wanneer geconfigureerd als een eindpunt in een bovenliggende-profiel. Bewaking van de eindpunten in het onderliggende profiel wordt in rekening gebracht op de gebruikelijke manier.
* DNS-query's: Elke query is slechts één keer worden geteld. Een query op een bovenliggende-profiel dat een eindpunt geactiveerd vanuit een onderliggende-profiel wordt wordt geteld tegen het bovenliggende profiel alleen.

Zie voor meer informatie, de [Traffic Manager-pagina met prijzen](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Is er een prestatie-impact voor geneste profielen?

Nee. Er is geen invloed op de prestaties in rekening gebracht bij het gebruik van geneste profielen.

De naamservers van Traffic Manager door de profielhiërarchie intern gaan bij het verwerken van elke DNS-query. Een DNS-query naar een bovenliggende-profiel kan een DNS-antwoord met een eindpunt ontvangen van een onderliggende-profiel. Een enkele CNAME-record wordt gebruikt of u van het profiel voor een enkele of geneste profielen gebruikmaakt. Er is niet nodig om te maken van een CNAME-record voor elk profiel in de hiërarchie.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Hoe de status van een geneste eindpunten in het profiel van een bovenliggende compute-Traffic Manager?

Het profiel van de bovenliggende uitvoeren niet statuscontroles voor de onderliggende rechtstreeks. In plaats daarvan de status van de eindpunten van het onderliggende profiel worden gebruikt voor het berekenen van de algemene status van het onderliggende profiel. Deze informatie wordt doorgegeven omhoog in de hiërarchie genest om te bepalen van de status van de geneste eindpunten. Het profiel van de bovenliggende gebruikt dit wordt de geaggregeerde status om te bepalen of het verkeer worden omgeleid naar de onderliggende.

De volgende tabel beschrijft het gedrag van Traffic Manager, statuscontroles voor een geneste-eindpunt.

| Status van onderliggende profiel Monitor | Status van de bovenliggende Monitor-eindpunt | Opmerkingen |
| --- | --- | --- |
| Uitgeschakeld. Het onderliggende profiel is uitgeschakeld. |Gestopt |De status van de bovenliggende eindpunt is gestopt, niet uitgeschakeld. De status uitgeschakeld is gereserveerd voor die aangeeft dat u het eindpunt in het profiel van de bovenliggende hebt uitgeschakeld. |
| Gedegradeerd. Ten minste één onderliggende profiel eindpunt heeft de status gedegradeerd. |Online: het aantal Online eindpunten in het onderliggende profiel is ten minste de waarde van MinChildEndpoints.<BR>CheckingEndpoint: het aantal Online plus CheckingEndpoint eindpunten in het onderliggende profiel is ten minste de waarde van MinChildEndpoints.<BR>Gedegradeerd: anders. |Verkeer wordt doorgestuurd naar een eindpunt van de status CheckingEndpoint. Als MinChildEndpoints te hoog instelt is, wordt het eindpunt is altijd verminderd. |
| Online. Ten minste één onderliggende eindpunt van het profiel is een Online status. Er is geen eindpunt heeft de status gedegradeerd. |Zie hierboven. | |
| CheckingEndpoints. Ten minste één onderliggende profiel eindpunt is 'CheckingEndpoint'. Er zijn geen eindpunten 'Online' of 'Verminderde' |Hetzelfde als hierboven. | |
| Niet-actief. Alle onderliggende profiel eindpunten zijn uitgeschakeld of gestopt, of dit profiel bevat geen eindpunten. |Gestopt | |

## <a name="next-steps"></a>Volgende stappen:
- Meer informatie over Traffic Manager [eindpunt bewakings- en automatische failover](../traffic-manager/traffic-manager-monitoring.md).
- Meer informatie over Traffic Manager [verkeersrouteringsmethoden](../traffic-manager/traffic-manager-routing-methods.md).
