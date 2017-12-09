---
title: Met Azure Traffic Manager - Veelgestelde vragen | Microsoft Docs
description: In dit artikel vindt u antwoorden op veelgestelde vragen over Traffic Manager
services: traffic-manager
documentationcenter: 
author: KumudD
manager: timlt
editor: 
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2017
ms.author: kumud
ms.openlocfilehash: d6681a5b46aa352b1aa0dadedad8a51c9d1e5eaf
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="traffic-manager-frequently-asked-questions-faq"></a>Traffic Manager Frequently Asked Questions (FAQ)

## <a name="traffic-manager-basics"></a>Traffic Manager-basisbeginselen

### <a name="what-ip-address-does-traffic-manager-use"></a>Welke IP-adres gebruiken Traffic Manager?

Zoals uitgelegd in [hoe Traffic Manager werkt](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager werkt op het niveau van DNS. DNS-antwoorden op de clients omleiden naar het juiste service-eindpunt wordt verzonden. Clients vervolgens rechtstreeks verbinding gemaakt met de service-eindpunt, niet via het Traffic Manager.

Traffic Manager biedt daarom geen een eindpunt of de IP-adres voor de clients verbinding maken. Als u statische IP-adres voor uw service, die moet worden geconfigureerd op de service, niet in Traffic Manager.

### <a name="does-traffic-manager-support-sticky-sessions"></a>Traffic Manager biedt ondersteuning voor 'een tijdelijke' sessies?

Zoals uitgelegd in [hoe Traffic Manager werkt](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager werkt op het niveau van DNS. Deze DNS-antwoorden gebruikt om clients omleiden naar het juiste service-eindpunt. Clients rechtstreeks verbinding gemaakt met de service-eindpunt, niet via het Traffic Manager. Traffic Manager zien daarom geen HTTP-verkeer tussen de client en de server.

Bovendien wordt het IP-bronadres van de DNS-query ontvangen door Traffic Manager hoort bij de recursieve DNS-service niet door de client. Traffic Manager is daarom geen enkele manier voor het bijhouden van afzonderlijke clients en 'een tijdelijke' sessies kan niet worden geïmplementeerd. Deze beperking geldt voor alle DNS-verkeer management systemen en is niet specifiek aan Traffic Manager.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Waarom krijg ik een HTTP-fout zien wanneer u het Traffic Manager?

Zoals uitgelegd in [hoe Traffic Manager werkt](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager werkt op het niveau van DNS. Deze DNS-antwoorden gebruikt om clients omleiden naar het juiste service-eindpunt. Clients vervolgens rechtstreeks verbinding gemaakt met de service-eindpunt, niet via het Traffic Manager. Traffic Manager biedt geen Zie HTTP-verkeer tussen client en server. Daarom moet HTTP-fout u ziet afkomstig zijn van uw toepassing. Voor de client verbinding maken met de toepassing, zijn alle DNS-omzetting stappen zijn voltooid. Die tussenkomst Traffic Manager op netwerkverkeer van de toepassing is bevat.

Verder onderzoek moet daarom zich richten op de toepassing.

De HTTP host-header verzonden vanuit de browser van de client is de meest voorkomende oorzaak van problemen. Zorg ervoor dat de toepassing is geconfigureerd voor het accepteren van de juiste host-header voor de domeinnaam die u gebruikt. Zie voor eindpunten met behulp van de Azure App Service, [configureren van een aangepaste domeinnaam voor een web-app in Azure App Service met behulp van Traffic Manager](../app-service/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-the-performance-impact-of-using-traffic-manager"></a>Wat is de prestatie-invloed van het gebruik van Traffic Manager?

Zoals uitgelegd in [hoe Traffic Manager werkt](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager werkt op het niveau van DNS. Omdat clients rechtstreeks verbinding met uw service-eindpunten gemaakt, is er geen invloed op de prestaties tijdens de Traffic Manager gebruiken wanneer de verbinding is gemaakt.

Aangezien Traffic Manager is geïntegreerd met toepassingen op het niveau van de DNS-, hoeft een extra DNS-zoekopdracht moet worden ingevoegd in de keten van DNS-omzetting. De gevolgen van Traffic Manager voor DNS-omzettingstijd is minimaal. Traffic Manager maakt gebruik van een wereldwijd netwerk van de naamservers en maakt gebruik van [anycast](https://en.wikipedia.org/wiki/Anycast) netwerken om ervoor te zorgen DNS query's altijd worden omgeleid naar de dichtstbijzijnde beschikbare naam-server. Bovendien caching van DNS-antwoorden houdt in dat de extra DNS-latentie als gevolg met behulp van Traffic Manager alleen voor een fractie van sessies geldt.

De methode prestaties verkeer naar de dichtstbijzijnde beschikbare eindpunt gestuurd. Het resultaat is dat de gevolgen van de algehele prestaties die zijn gekoppeld aan deze methode minimaal moeten. Een toename in latentie DNS moet worden gecompenseerd door lagere netwerklatentie naar het eindpunt.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Welke toepassingsprotocollen kan ik met Traffic Manager gebruiken?

Zoals uitgelegd in [hoe Traffic Manager werkt](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager werkt op het niveau van DNS. Zodra de DNS-zoekopdracht voltooid is, clients rechtstreeks verbinding gemaakt met het toepassingseindpunt, niet via het Traffic Manager. De verbinding Gebruik daarom elk toepassingsprotocol voor de. Als u TCP als de controle van het protocol, Traffic Manager selecteren eindpunt statuscontrole kan plaatsvinden zonder een toepassingsprotocollen. Als u ervoor kiest om de status gecontroleerd met behulp van een toepassingsprotocol, moet het eindpunt kunnen reageren op aanvragen via HTTP of HTTPS ophalen.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Kan ik Traffic Manager gebruiken met een 'Open' domeinnaam?

Nee. De DNS-standaarden staan niet toe dat CNAME-records in combinatie met andere DNS-records met dezelfde naam. De apex (of hoofdmap) van een DNS-zone bevat altijd twee vooraf bestaande DNS-records; de SOA en gezaghebbende NS-records. Dit betekent dat een CNAME-record kan niet worden gemaakt in het toppunt van de zone zonder te schenden van de DNS-standaarden.

Traffic Manager vereist een DNS CNAME-record voor de aangepaste DNS-naam toewijzen. U kunt bijvoorbeeld www.contoso.com toewijzen aan het Traffic Manager profiel DNS-naam contoso.trafficmanager.net. Traffic Manager-profiel retourneert bovendien een tweede DNS CNAME om aan te geven welk eindpunt dat de client verbinding moet maken.

U kunt dit probleem omzeilen, wordt u aangeraden een HTTP-omleiding voor directe verkeer van de Open domeinnaam naar een andere URL, dat vervolgens Traffic Manager kunt gebruiken. De Open domein 'contoso.com' kan bijvoorbeeld gebruikers omleiden naar de CNAME 'www.contoso.com' die naar de Traffic Manager-DNS-naam verwijst.

Volledige ondersteuning voor Open-domeinen in Traffic Manager wordt bijgehouden in onze achterstand functie. U kunt de ondersteuning voor deze aanvraag functie door registreren [stemmen voor op onze community feedback site](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).

### <a name="does-traffic-manager-consider-the-client-subnet-address-when-handling-dns-queries"></a>Beschouwt Traffic Manager de subnet-adres van de client bij de verwerking van DNS-query's? 
Ja, naast het bron-IP-adres van de DNS-query wordt ontvangen (dat meestal het IP-adres van de DNS-resolver), bij het uitvoeren van zoekacties voor geografisch en prestaties van de methoden voor het doorsturen, het traffic manager tevens rekening gehouden met het adres van de client-subnet als het opgenomen in de query door de resolver maken van de aanvraag voor de eindgebruiker.  
In het bijzonder [RFC 7871 – Subnet van de Client in DNS-query's](https://tools.ietf.org/html/rfc7871) die zorgt voor een [extensie mechanisme voor DNS (EDNS0)](https://tools.ietf.org/html/rfc2671) waarmee op de subnet-adres van de client kan doorgeven van resolvers die dit ondersteunen.

### <a name="what-is-dns-ttl-and-how-does-it-impact-my-users"></a>Wat is DNS TTL en hoe deze gevolgen hebben voor Mijn gebruikers?

Als u een DNS-query belandt op Traffic Manager, wordt een waarde in het antwoord time to live (TTL) genoemd. Deze waarde, waarvan eenheid in seconden is, geeft aan DNS-resolvers downstream op hoe lang om dit antwoord in cache. Terwijl de DNS-resolvers zijn niet gegarandeerd dat dit in de cache, caching deze manier kunnen reageren op de volgende query's uit de cache in plaats van Traffic Manager-DNS-servers wilt. Dit heeft gevolgen voor de antwoorden als volgt:
- een hogere TTL vermindert het aantal query's die op de Traffic Manager-DNS-servers, waardoor de kosten verminderen voor een klant, omdat het aantal query's die worden geleverd, is een factureerbare gebruik terechtkomen.
- een hogere TTL kunt verkleint de tijd die nodig zijn om een DNS-zoekactie.
- een hogere TTL betekent ook dat de gegevens komt niet overeen met de meest recente statusgegevens die worden verkregen via de zoek agenten Traffic Manager.

### <a name="how-high-or-low-can-i-set-the-ttl-for-traffic-manager-responses"></a>Hoe hoog of laag kan ik de TTL voor Traffic Manager-reacties instellen?

U kunt instellen, op een per profiel niveau, de TTL DNS moet zo laag 0 seconden en zo hoog 2.147.483.647 seconden (compatibel zijn met het maximumbereik [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt )). Een TTL-waarde 0 betekent dat reacties op query's niet in cache opslaan downstream DNS-resolvers en alle query's wordt verwacht dat het Traffic Manager-DNS-servers voor de omzetting van bereiken.

## <a name="traffic-manager-geographic-traffic-routing-method"></a>Traffic Manager geografisch verkeersrouteringsmethode

### <a name="what-are-some-use-cases-where-geographic-routing-is-useful"></a>Wat zijn enkele gebruiksvoorbeelden waar geografische routering nuttig is? 
Geografische routeringstype kan worden gebruikt in een scenario waarin een Azure-klant moet te onderscheiden van hun gebruikers op basis van geografische regio's. Bijvoorbeeld, kunt de geografische verkeersrouteringsmethode gebruikt, u gebruikers geven van specifieke regio's een andere gebruiker-ervaring dan die van andere regio's. Een ander voorbeeld is die voldoet aan de lokale gegevens onafhankelijkheid vereist die vereisen dat gebruikers van een specifieke regio alleen door de eindpunten in deze regio worden geleverd.

### <a name="what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing"></a>Wat zijn de regio's die door Traffic Manager voor het doorsturen van geografische worden ondersteund? 
De land/regio-hiërarchie die wordt gebruikt door Traffic Manager vindt [hier](traffic-manager-geographic-regions.md). Terwijl deze pagina wordt bijgewerkt met de wijzigingen, kunt u dezelfde gegevens ook programmatisch ophalen met behulp van de [REST-API van Azure Traffic Manager](https://docs.microsoft.com/rest/api/trafficmanager/). 

### <a name="how-does-traffic-manager-determine-where-a-user-is-querying-from"></a>Hoe wordt het traffic manager vastgesteld waar opvragen van een gebruiker uit? 
Traffic Manager wordt gekeken naar de bron-IP van de query (dit is waarschijnlijk een lokale DNS-resolver tijdens het doorzoeken van de query namens de gebruiker) en een intern IP-adres aan kaart van regio gebruikt om te bepalen van de locatie. Deze kaart wordt voortdurend aan vanwege wijzigingen in het internet bijgewerkt. 

### <a name="is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case"></a>Kan het gegarandeerd dat Traffic Manager kan de exacte geografische locatie van de gebruiker in elk geval correct bepalen?
Nee, Traffic Manager biedt geen garantie dat de geografische regio die we uit het bron-IP-adres van een DNS-query afleiden altijd met de locatie van de gebruiker de volgende oorzaken overeen: 

- Eerst, zoals wordt beschreven in de vorige sectie, het IP-bronadres we zien is die van een DNS-resolver tijdens het doorzoeken van de zoekactie namens de gebruiker. Hoewel de geografische locatie van de DNS-resolver een goede proxy voor de geografische locatie van de gebruiker, kan ook worden verschillende, al naar gelang de voetafdruk van de DNS-resolver-service en de specifieke DNS-resolver-service die van de klant ervoor gekozen heeft om te gebruiken. Als u bijvoorbeeld kunt een klant in Maleisië opgeven in hun apparaat instellingen gebruik een DNS-omzettingsservice waarvan DNS-server in Singapore mogelijk ophalen verzameld voor het afhandelen van de query-oplossingen voor die gebruiker/apparaat. In dat geval kan de resolver IP-adres dat overeenkomt met de locatie Singapore alleen zien door Traffic Manager. Zie de eerdere Veelgestelde vragen over ondersteuning voor subnet adres client ook op deze pagina.

- Ten tweede Traffic Manager maakt gebruik van een interne kaart te doen van het IP-adres van de vertaling van de geografische regio. Hoewel deze kaart is gevalideerd en voortdurend de nauwkeurigheid vergroten en de account voor de veranderende aard van het internet bijgewerkt, nog er steeds de mogelijkheid om onze gegevens is geen exacte weergave van de geografische locatie van de IP-adressen.


###  <a name="does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing"></a>Moet een eindpunt zich fysiek bevinden in dezelfde regio als het account dat is geconfigureerd met voor het doorsturen van geografische? 
Nee, de locatie van het eindpunt legt geen beperkingen op welke regio's kunnen worden toegewezen aan deze. Een eindpunt in VS-midden Azure-regio kan bijvoorbeeld India omgeleid naar het voor alle gebruikers hebben.

### <a name="can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing"></a>Kan ik geografische regio's toewijzen aan eindpunten in een profiel dat is niet geconfigureerd om te doen geografische routering? 

Ja, als de methode voor het doorsturen van een profiel niet geografische, kunt u de [REST-API van Azure Traffic Manager](https://docs.microsoft.com/rest/api/trafficmanager/) geografische regio's toewijzen aan eindpunten in dit profiel. In het geval van niet-geografische routering type profielen, wordt deze configuratie genegeerd. Als u zo'n profiel in geografische routeringstype op een later tijdstip wijzigt, kunt Traffic Manager die toewijzingen gebruiken.


### <a name="why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic"></a>Waarom krijg ik een fout wanneer ik probeer te wijzigen in de methode voor het doorsturen van een bestaand profiel geografisch?

De eindpunten met een profiel met geografische routering moeten ten minste één regio is toegewezen. Een bestaand profiel voor conversie naar geografische routeringstype, moet u eerst koppelen geografische regio's op de eindpunten die met behulp van de [REST-API van Azure Traffic Manager](https://docs.microsoft.com/rest/api/trafficmanager/) voordat u het type routering in geografische wijzigt. Als u portal gebruikt, verwijdert u eerst de eindpunten wijzigen in de methode voor het doorsturen van het profiel geografische en voegt u de eindpunten samen met hun toewijzing geografische regio. 


###  <a name="why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled"></a>Waarom is het raadzaam dat klanten geneste profielen in plaats van eindpunten onder een profiel maken met geografische-routering ingeschakeld? 

Een regio kan worden toegewezen aan slechts één eindpunt binnen een profiel als de geografische routeringstype gebruiken. Als dat eindpunt niet een genest type met een onderliggende-profiel dat is gekoppeld is, als dat niet in orde, gaan eindpunt verkeer Manager doorgaat met het verkeer naar sinds het alternatief niet verzenden van al het verkeer wordt elk beter niet verzenden. Traffic Manager biedt geen failover naar een ander eindpunt, zelfs wanneer de regio die toegewezen 'parent' van de regio die aan het eindpunt dat is gegaan slecht is (bijvoorbeeld als een eindpunt dat regio Spanje is slecht dat we doen niet failover naar een ander eindpunt dat wordt toegewezen de regio die Europa toegewezen heeft.) Dit wordt gedaan om ervoor te zorgen dat het Traffic Manager respecteert de geografische grenzen van de klant heeft ingesteld in het profiel. Als u het voordeel van mislukt via een ander eindpunt als een eindpunt slecht gaat, wordt het aanbevolen dat de geografische regio's worden toegewezen aan geneste profielen met meerdere eindpunten binnen dit in plaats van afzonderlijke eindpunten. Als een eindpunt in het profiel voor geneste onderliggende mislukt, verkeer op deze manier kunt failover naar een ander eindpunt binnen hetzelfde geneste onderliggende profiel.

### <a name="are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type"></a>Zijn er beperkingen op de API-versie die ondersteuning biedt voor dit type routering?

Ja, alleen API-versie 2017-03-01 en nieuwere ondersteunt de geografische routering typt. Alle oudere versies van de API kunnen niet worden gebruikt voor het gemaakte profielen van geografische routeringstype of geografische regio's toewijzen aan eindpunten. Als een oudere versie van de API wordt gebruikt voor het ophalen van de profielen van een Azure-abonnement, wordt een profiel van geografische routeringstype niet geretourneerd. Bovendien wanneer u een oudere versie van de API, een profiel geretourneerd die heeft eindpunten met de toewijzing van een geografische regio, heeft niet de toewijzing van de geografische regio die wordt weergegeven.

## <a name="real-user-measurements"></a>Real User Measurements

>[!NOTE]
>De functie echte gebruiker metingen in Traffic Manager kan is openbare preview-versie en geen hetzelfde niveau van beschikbaarheid en betrouwbaarheid zoals functies die in het algemeen beschikbaarheid release. De functie wordt niet ondersteund, kan hebben beperkte mogelijkheden en mogelijk niet beschikbaar in alle Azure-locaties. Voor de meest recente meldingen op beschikbaarheid en de status van deze functie, Controleer de [Azure Traffic Manager-updates](https://azure.microsoft.com/updates/?product=traffic-manager) pagina.

### <a name="what-are-the-benefits-of-using-real-user-measurements"></a>Wat zijn de voordelen van het gebruik van echte metingen van de gebruiker?
Wanneer u routeringsmethode voor prestaties, Traffic Manager haalt het beste Azure-regio voor uw eindgebruikers verbinding maken met door te inspecteren van de bron-IP en het Subnet van de Client EDNS (indien doorgegeven) en controleren op basis van het netwerk latentie intelligence de service onderhoudt. Echte gebruiker metingen verbetert dit voor de eindgebruiker base doordat hun ervaring bijdragen aan deze tabel latentie naast controleren die in deze tabel voldoende de netwerken van de eindgebruiker omvat vanaf waar uw eindgebruikers verbinding met Azure maken. Dit leidt tot een grotere nauwkeurigheid in de routering van uw eindgebruikers.

### <a name="can-i-use-real-user-measurements-with-non-azure-regions"></a>Kan ik echte gebruiker metingen met niet-Azure-regio's gebruiken?
Echte gebruiker metingen metingen en rapporten over alleen de latentie te bereiken Azure-regio's. Als u op basis van prestaties routering met eindpunten die worden gehost in niet-Azure-regio's gebruikt, kunt u nog steeds profiteren van deze functie door gestegen latentiegegevens over de representatieve Azure-regio die u hebt geselecteerd om te worden gekoppeld aan dit eindpunt.

### <a name="which-routing-method-benefits-from-real-user-measurements"></a>Routeringsmethode voor welke voordelen van echte metingen van de gebruiker?
De aanvullende informatie die is opgedaan met echte gebruiker metingen zijn alleen van toepassing op de profielen die gebruikmaken van de routeringsmethode voor prestaties. Houd er rekening mee dat de echte gebruiker metingen-koppeling beschikbaar in alle profielen is wanneer u deze via de Azure portal bekijken.

### <a name="do-i-need-to-enable-real-user-measurements-each-profile-separately"></a>Heb ik nodig om in te schakelen echte gebruiker metingen elk profiel afzonderlijk?
Nee, alleen moet u één keer per abonnement inschakelen en alle latentiegegevens gemeten en gerapporteerd beschikbaar zijn voor alle profielen.

### <a name="how-do-i-turn-off-real-user-measurements-for-my-subscription"></a>Hoe kan ik echte gebruiker metingen uitschakelen voor mijn abonnement?
U kunt stoppen lopen de kosten die betrekking hebben op echte metingen van de gebruiker wanneer u stopt met het verzamelen en verzenden van back-latentie metingen van uw clienttoepassing. Bijvoorbeeld, wanneer meting JavaScript in webpagina's ingesloten, kunt u stoppen met deze functie door het verwijderen van JavaScript of door het uitschakelen van de aanroeping als de pagina wordt weergegeven.
Uitschakelen van echte metingen van de gebruiker op een andere manier is om uw sleutel te verwijderen. Zodra u dat doet, worden alle metingen aan Traffic Manager verzonden met die sleutel verwijderd.

### <a name="can-i-use-real-user-measurements-with-client-applications-other-than-web-pages"></a>Kan ik echte gebruiker metingen met clienttoepassingen dan webpagina's gebruiken?
Ja, echte metingen van de gebruiker is ontworpen voor het opnemen van gegevens die zijn verzameld via een ander type eindgebruiker clients. Deze Veelgestelde vragen wordt bijgewerkt als nieuwe typen van clienttoepassingen get wordt ondersteund.

### <a name="how-many-measurements-are-made-each-time-my-real-user-measurements-enabled-web-page-is-rendered"></a>Hoeveel metingen van elke keer dat mijn echte gebruiker metingen ingeschakeld webpagina wordt gerenderd?
Als echte metingen van de gebruiker wordt gebruikt met de meting opgegeven JavaScript, resulteert elke paginaweergave in zes metingen worden uitgevoerd. Deze worden vervolgens terug gerapporteerd aan de Traffic Manager-service. Houd er rekening mee dat u in rekening voor deze functie op basis van het aantal metingen gerapporteerd aan het Traffic Manager-service gebracht. Bijvoorbeeld, als de gebruiker weg van uw webpagina navigeert terwijl de metingen worden genomen, maar voordat is gemeld, deze metingen zijn niet in aanmerking voor factureringsdoeleinden.

### <a name="is-there-a-delay-before-real-user-measurements-script-runs-in-my-webpage"></a>Is er een vertraging optreden voordat echte gebruiker metingen script wordt uitgevoerd in mijn webpagina?
Nee, is geen geprogrammeerde vertraging voordat het script wordt aangeroepen.

### <a name="can-i-use-configure-real-user-measurements-with-only-the-azure-regions-i-want-to-measure"></a>Kan ik gebruiken echte gebruiker metingen configureren met alleen de Azure-regio's ik wil meten?
Nee, elke keer die deze wordt aangeroepen, wordt het script echte metingen van de gebruiker een reeks zes Azure-regio's zoals wordt bepaald door de service. Deze wijzigingen tussen verschillende aanroepen ingesteld en wanneer er een groot aantal dergelijke aanroepen optreden, wordt de dekking van de meting zich over verschillende Azure-regio's.

### <a name="can-i-limit-the-number-of-measurements-made-to-a-specific-number"></a>Kan ik het aantal metingen tot een bepaald aantal beperken?
De meting JavaScript is ingesloten in uw webpagina's en u zijn in de volledige controle over te starten en stoppen met het. Zolang de Traffic Manager-service een aanvraag ontvangt voor een lijst met Azure-regio's te meten, een reeks regio's worden geretourneerd. Houd er ook rekening mee dat tijdens de evaluatieperiode u niet de factuur voor alle metingen gerapporteerd aan Traffic Manager

### <a name="can-i-see-the-measurements-taken-by-my-client-application-as-part-of-real-user-measurements"></a>Kan ik de metingen door de clienttoepassing als onderdeel van echte metingen van de gebruiker zien?
Aangezien de logica van de meting vanuit de clienttoepassing wordt uitgevoerd, bent u volledige controle van wat er gebeurt met inbegrip van de metingen latentie te zien. Traffic Manager een samengevoegde weergave van de metingen ontvangen onder de sleutel die is gekoppeld aan uw abonnement niet gerapporteerd

### <a name="can-i-modify-the-measurement-script-provided-by-traffic-manager"></a>Kan ik de meting-script dat is opgegeven door Traffic Manager wijzigen?
Hoewel u controle over wat er op de webpagina is ingesloten, afraden we u ten zeerste van u wijzigingen aanbrengt in het script meting om ervoor te zorgen dat het meet en de latenties correct rapporteert.

### <a name="will-it-be-possible-for-others-to-see-the-key-i-use-with-real-user-measurements"></a>Mogelijk zal zijn voor anderen kunnen zien van de sleutel die ik met echte gebruiker metingen gebruiken?
Wanneer u het script meting naar een webpagina insluit is het mogelijk zijn voor anderen het script en uw sleutel echte gebruiker metingen (RUM) te zien. Maar het is belangrijk te weten dat deze sleutel af van uw abonnements-id wijkt en het Traffic Manager worden alleen gebruikt voor dit doel wordt gegenereerd. De sleutel van uw RUMSECTOR weten niet in gevaar brengt de veiligheid van uw Azure-account

### <a name="can-others-abuse-my-rum-key"></a>Kunnen anderen mijn RUMSECTOR sleutel misbruik?
Het is mogelijk voor andere gebruikers van uw sleutel onjuiste gegevens te verzenden naar Azure Houd er rekening mee dat enkele verkeerde metingen heeft geen invloed op de routering omdat deze wordt gehouden, samen met de metingen dat we ontvangen. Als u uw sleutels wijzigen wilt, kunt u de sleutel op dat moment de oude sleutel wordt verwijderd opnieuw genereren.

###  <a name="do-i-need-to-put-the-measurement-javascript-in-all-my-web-pages"></a>Moet ik de meting JavaScript in mijn webpagina's geplaatst?
Echte gebruiker metingen biedt meer waarde als het aantal metingen toename. Gelet aangegeven dat, is uw beslissing over de noodzaak te plaatsen in uw webpagina's of een select weinig. Onze aanbeveling is gestart door de gegevens in uw meest bezochte pagina waar een gebruiker om te blijven op die pagina vijf seconden of langer wordt verwacht.

### <a name="can-information-about-my-end-users-be-identified-by-traffic-manager-if-i-use-real-user-measurements"></a>Kan worden informatie over mijn eindgebruikers door Traffic Manager geïdentificeerd als gebruikmaakt van echte metingen van de gebruiker?
Wanneer de opgegeven JavaScript-meting wordt gebruikt, hebben Traffic Manager inzicht in het client-IP-adres van de eindgebruiker en het IP-bronadres van de lokale DNS-resolver die ze gebruiken. Traffic Manager maakt gebruik van IP-adres van de client alleen nadat ik deze afgekapt zodat het niet mogelijk om u te identificeren van de specifieke gebruiker die de metingen verzonden. 

### <a name="does-the-webpage-measuring-real-user-measurements-need-to-be-using-traffic-manager-for-routing"></a>Ondersteunt de webpagina van de meting echte gebruiker metingen gebruiken wilt voor het doorsturen van Traffic Manager?
Nee, hoeft deze niet Traffic Manager gebruiken. De routing-zijde van Traffic Manager werkt afzonderlijk van de echte gebruiker meting deel en hoewel het een goed idee om beide in de eigenschap met dezelfde web, hoeven ze niet.

### <a name="do-i-need-to-host-any-service-on-azure-regions-to-use-with-real-user-measurements"></a>Heb ik nodig voor het hosten van elke service op Azure-regio's voor gebruik met echte metingen van de gebruiker?
Nee, hoeft u niet voor het hosten van elke server-side-onderdeel op Azure voor echte gebruiker metingen om te werken. De installatiekopie van één pixel gedownload door de meting JavaScript en de service uitvoert in andere Azure-regio's wordt gehost en beheerd door Azure. 

### <a name="will-my-azure-bandwidth-usage-increase-when-i-use-real-user-measurements"></a>Mijn Azure bandbreedtegebruik vergroten bij het gebruik van echte metingen van de gebruiker?
Zoals vermeld in het vorige antwoord, worden de serveronderdelen van echte metingen van de gebruiker die eigendom zijn en worden beheerd door Azure. Dit betekent dat uw Azure-bandbreedtegebruik wordt niet verhogen omdat het gebruik van echte metingen van de gebruiker. Houd er rekening mee dat dit geen eventuele bandbreedtegebruik buiten welke Azure kosten omvat. We minimaliseren de bandbreedte die wordt gebruikt door alleen een installatiekopie van één pixel downloaden naar de latentie voor een Azure-regio meting. 

## <a name="traffic-view"></a>Traffic View

>[!NOTE]
>De functie verkeer weergeven in Traffic Manager kan is openbare preview-versie en geen hetzelfde niveau van beschikbaarheid en betrouwbaarheid zoals functies die in het algemeen beschikbaarheid release. De functie wordt niet ondersteund, kan hebben beperkte mogelijkheden en mogelijk niet beschikbaar in alle Azure-locaties. Voor de meest recente meldingen op beschikbaarheid en de status van deze functie, Controleer de [Azure Traffic Manager-updates](https://azure.microsoft.com/updates/?product=traffic-manager) pagina.

### <a name="what-does-traffic-view-do"></a>Wat doet verkeer weergeven?
Weergave van verkeer is een functie van de verkeer-Manager waarmee u uw gebruikers en hoe hun ervaring wordt meer inzicht. Dit maakt gebruik van de query's ontvangen door het Traffic Manager en het netwerk latentie intelligence tabellen die de service onderhoudt waarmee u het volgende:
- De regio's waar uw gebruikers zijn verbinding kunnen maken met uw Azure-eindpunten.
- Het volume van gebruikers die verbinding maakt vanaf deze regio's.
- Azure-regio's waaraan ze ophalen doorgestuurd naar.
- Hun ervaring latentie voor deze Azure-regio's.

Deze informatie is beschikbaar voor u te gebruiken via een tabellarische weergave in de portal niet alleen beschikbaar als de onbewerkte gegevens voor het downloaden.

### <a name="how-can-i-benefit-from-using-traffic-view"></a>Hoe kan ik profiteren van verkeer weergeven?

Verkeer weergave biedt u de algehele weergave van het verkeer ontvangen van uw Traffic Manager-profielen. In het bijzonder, kan deze worden gebruikt om te zien waarin uw gebruikersgroep verbindt uit en evenveel belangrijker nog wat hun ervaring gemiddelde latentie is. U kunt deze informatie vervolgens gebruiken om te zoeken waarin u richten wilt, bijvoorbeeld door het uitbreiden van uw Azure footprint tot een regio die gebruikers kan worden gebruikt met een lagere latentie gebieden. Een ander inzicht die kunnen worden afgeleid van het verkeer weergave is om te zien van de patronen die verkeer naar verschillende regio's die op zijn beurt u om beslissingen neemt helpen kunnen over oplopende of aflopende volgorde voorraad in die gebieden.

### <a name="how-is-traffic-view-different-from-the-traffic-manager-metrics-available-through-azure-monitor"></a>Hoe wordt verkeer weergave van de metrische gegevens Traffic Manager beschikbaar is via Azure monitor?

Azure Monitor kan worden gebruikt om te begrijpen op een hoger niveau van het verkeer dat is ontvangen door uw profiel en de eindpunten. Ook kunt u de status van de eindpunten te volgen bij het blootstellen van de resultaten van de status. Als u wilt verdergaan dan deze en ervaring voor uw eindgebruikers verbinding maken met Azure op een regionaal niveau begrijpen, kan verkeer weergave bereiken die worden gebruikt.

### <a name="does-traffic-view-use-edns-client-subnet-information"></a>Maakt gebruik van verkeer weergave EDNS Client subnetgegevens?

Verkeer weergeven beschouwd bij het maken van de uitvoer niet als de subnetgegevens EDNS Client. Dit maakt gebruik van de IP-adres van uw gebruikers lokale DNS-resolver ze te groeperen.

### <a name="how-many-days-of-data-does-traffic-view-use"></a>Hoeveel dagen aan gegevens maakt gebruik van verkeer weergeven?

Verkeer weergave maakt uitvoer ervan weergegeven door het verwerken van de gegevens van de zeven dagen vóór de dag vóór wanneer het door u wordt weergegeven. Dit is een zwevend venster en de meest recente gegevens worden gebruikt wanneer die u bezoekt.

### <a name="how-does-traffic-view-handle-external-endpoints"></a>Hoe wordt verkeer weergave externe eindpunten verwerkt?

Wanneer u externe eindpunten die worden gehost buiten Azure-regio's in een Traffic Manager-profiel kunt u deze toegewezen aan een Azure-regio die fungeert als proxy voor de latentie-kenmerken (dit is in feite nodig als u routeringsmethode voor prestaties gebruiken). Als dat zo deze toewijzing van de Azure-regio is, wordt dat Azure regio latentie metrische gegevens worden gebruikt bij het maken van de uitvoer van de weergave van verkeer. Als er geen Azure-regio is opgegeven, wordt de latentie-informatie niet leeg zijn in de gegevens voor deze externe eindpunten zijn.

### <a name="do-i-need-to-enable-traffic-view-for-each-profile-in-my-subscription"></a>Moet ik verkeer weergave inschakelen voor elk uitvoeringsprofiel in mijn abonnement?
Tijdens de evaluatieperiode verkeer weergeven is ingeschakeld op abonnementsniveau en is beschikbaar voor alle Traffic Manager-profielen onder dat abonnement.

### <a name="how-can-i-turn-off-traffic-view"></a>Hoe kan ik verkeer weergave uitschakelen?
Tijdens de evaluatieperiode vragen we een ondersteuningsticket om uit te schakelen verkeer weergeven voor uw abonnement te maken.

### <a name="how-does-traffic-view-billing-work"></a>Hoe werkt de facturering verkeer weergeven?

Prijzen van verkeer weergeven is gebaseerd op het aantal gegevenspunten gebruikt voor het maken van de uitvoer. Het enige gegevenstype ondersteund is momenteel de query's ontvangt van uw profiel. U wordt bovendien alleen gefactureerd voor de verwerking die is uitgevoerd als u de weergave van verkeer ingeschakeld zijn. Dit betekent dat, als u verkeer weergave voor een bepaalde periode in een maand inschakelen en tijdens andere momenten uitschakelen, alleen de gegevenspunten verwerkt terwijl u de functie moest count naar uw factuur ingeschakeld.
Tijdens de evaluatieperiode u worden niet in rekening gebracht voor verkeer weergave.

## <a name="traffic-manager-endpoints"></a>Traffic Manager-eindpunten

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Kan ik Traffic Manager gebruiken met de eindpunten van meerdere abonnementen?

Met behulp van de eindpunten van meerdere abonnementen is niet mogelijk met Azure-Web-Apps. Azure-Web-Apps vereist dat een aangepaste domeinnaam gebruikt met Web-Apps alleen wordt gebruikt binnen een één abonnement. Het is niet mogelijk om te gebruiken van Web-Apps uit meerdere abonnementen met dezelfde domeinnaam.

Voor andere eindpunttypen is het mogelijk te Traffic Manager gebruiken met de eindpunten van meer dan één abonnement. In de Resource-Manager eindpunten van een abonnement kunnen worden toegevoegd als aan Traffic Manager, zolang de persoon die de Traffic Manager-profiel configureren leestoegang tot het eindpunt heeft. Deze machtigingen worden verleend met behulp van [Azure Resource Manager-rol gebaseerde toegangsbeheer (RBAC)](../active-directory/role-based-access-control-configure.md).


### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Kan ik Traffic Manager met Service voor de Cloud 'Staging' sleuven gebruiken?

Ja. Cloudservice 'staging' sleuven kan in Traffic Manager worden geconfigureerd als externe eindpunten. Statuscontroles zijn nog steeds worden in rekening gebracht Azure-eindpunten. Omdat het type van het externe eindpunt gebruikt wordt, zijn wijzigingen in de onderliggende service niet opgenomen automatisch. Traffic Manager kan niet met externe eindpunten detecteren wanneer de Cloud-Service is gestopt of verwijderd. Het Traffic Manager wordt daarom facturering voor statuscontroles vervolgd totdat het eindpunt is uitgeschakeld of verwijderd.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Traffic Manager biedt ondersteuning voor IPv6-eindpunten?

Traffic Manager biedt momenteel geen IPv6-addressible naamservers. Traffic Manager kan nog steeds worden gebruikt door een IPv6-clients verbinding maken met IPv6-eindpunten. Een client maakt DNS-aanvragen niet rechtstreeks aan Traffic Manager. In plaats daarvan gebruikt de client een recursieve DNS-service. Een IPv6-netwerk client verzendt aanvragen naar de recursieve DNS-service via IPv6. De recursieve-service moet vervolgens de Traffic Manager-naamservers met IPv4 contact kunnen maken.

Traffic Manager reageert met de DNS-naam van het eindpunt. Ter ondersteuning van een IPv6-eindpunt, moet een DNS-naam van het eindpunt verwijst naar het IPv6-adres DNS AAAA-record bestaan. Traffic Manager statuscontroles ondersteunen alleen IPv4-adressen. U moet de service te kunnen stellen van een IPv4-eindpunt op dezelfde DNS-naam.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Kan ik Traffic Manager gebruiken met meer dan een Web-App in dezelfde regio?

Traffic Manager wordt doorgaans gebruikt voor het verkeer naar toepassingen die zijn geïmplementeerd in verschillende regio's. Echter kan het ook worden gebruikt wanneer een toepassing heeft meer dan één implementatie in dezelfde regio. De Azure Traffic Manager-eindpunten staan niet meer dan één eindpunt van de Web-App van dezelfde Azure-regio worden toegevoegd aan dezelfde Traffic Manager-profiel.

### <a name="how-do-i-move-my-traffic-manager-profiles-azure-endpoints-to-a-different-resource-group"></a>Hoe kan ik mijn Traffic Manager-profiel Azure-eindpunten naar een andere resourcegroep verplaatsen?

Azure-eindpunten die gekoppeld aan een Traffic Manager-profiel zijn worden bijgehouden met behulp van de resource-id. Wanneer een Azure-resource die wordt gebruikt als een eindpunt (bijvoorbeeld openbare IP-adres, klassieke Cloudservice, Web-App of een andere Traffic Manager-profiel gebruikt in een geneste manier) wordt verplaatst naar een andere resourcegroep in de resource-ID wijzigingen. In dit scenario moet u op dit moment Traffic Manager-profiel eerst verwijderd en vervolgens toe te voegen opnieuw de eindpunten in het profiel bijwerken. 

##  <a name="traffic-manager-endpoint-monitoring"></a>Eindpuntcontrole van Traffic Manager

### <a name="is-traffic-manager-resilient-to-azure-region-failures"></a>Traffic Manager tegen storingen van de Azure-regio is?

Traffic Manager is een belangrijk onderdeel van de levering van maximaal beschikbare toepassingen in Azure.
Voor het leveren van hoge beschikbaarheid moet Traffic Manager hebben een uitzonderlijk hoge mate van beschikbaarheid en netwerkfouten regionale mislukt.

Traffic Manager-onderdelen zijn standaard flexibel omgaan met een volledige fout van Azure-regio. Deze herstelmogelijkheden is van toepassing op alle Traffic Manager-onderdelen: de DNS-naam servers, de API de storage-laag en de bewaking van de service-eindpunt.

In het onwaarschijnlijke geval van een storing van een volledige Azure-regio Traffic Manager naar verwachting blijven werken normaal. Toepassingen die zijn geïmplementeerd in meerdere Azure-regio's kunnen vertrouwen op Traffic Manager om verkeer naar een beschikbare instantie van de toepassing te regelen.

### <a name="how-does-the-choice-of-resource-group-location-affect-traffic-manager"></a>Hoe beïnvloedt de keuze van de locatie voor resourcegroep Traffic Manager?

Traffic Manager is een enkelvoudige, wereldwijde service. Het is niet regionaal. De keuze van de locatie voor resourcegroep maakt niet uit Traffic Manager-profielen die zijn geïmplementeerd in die resourcegroep.

Azure Resource Manager vereist voor alle resourcegroepen een locatie die de standaardlocatie voor resources geïmplementeerd in die resourcegroep bepaalt opgeven. Wanneer u een Traffic Manager-profiel maakt, wordt deze gemaakt in een resourcegroep. Alle profielen voor Traffic Manager **globale** overschrijven als de locatie van de standaardwaarde van de groep resource.

### <a name="how-do-i-determine-the-current-health-of-each-endpoint"></a>Hoe bepaal ik de huidige status van elk eindpunt

De huidige bewakingsstatus van elk eindpunt dat, naast de algehele profiel wordt weergegeven in de Azure portal. Deze informatie is ook beschikbaar via de Monitor verkeer [REST-API](https://msdn.microsoft.com/library/azure/mt163667.aspx), [PowerShell-cmdlets](https://msdn.microsoft.com/library/mt125941.aspx), en [platformoverschrijdende Azure CLI](../cli-install-nodejs.md).

U kunt ook Azure Monitor de status van uw eindpunten bijhouden en een visuele representatie van deze Zie gebruiken. Zie voor meer informatie over het gebruik van Azure-Monitor de [documentatie Azure Monitoring](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics).

### <a name="can-i-monitor-https-endpoints"></a>Kan ik de HTTPS-eindpunten bewaken?

Ja. Traffic Manager biedt ondersteuning voor zoeken via HTTPS. Configureer **HTTPS** als het protocol in de configuratie van bewaking.

Het Traffic manager biedt geen een validatie van het servercertificaat, met inbegrip van:

* Serverzijde certificaten worden niet gevalideerd.
* SNI-serverzijde certificaten worden niet ondersteund
* Clientcertificaten worden niet ondersteund.

### <a name="i-stopped-an-azure-cloud-service--web-application-endpoint-in-my-traffic-manager-profile-but-i-am-not-receiving-any-traffic-even-after-i-restarted-it-how-can-i-fix-this"></a>Ik een Azure-cloud-service gestopt / web-toepassingseindpunt in mijn Traffic Manager-profiel, maar ik ontvang geen verkeer zelfs nadat ik het opnieuw opgestart. Hoe kan ik dit oplossen?

Wanneer een Azure cloud service / web-eindpunt is gestopt Traffic Manager stopt de status controleren en het opnieuw opstarten van de statuscontroles alleen na het detecteren van het eindpunt opnieuw is opgestart. Om te voorkomen dat deze vertraging, uitschakelen en vervolgens dat eindpunt in het Traffic Manager-profiel inschakelen nadat u het eindpunt opnieuw starten.   

### <a name="can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https"></a>Kan ik Traffic Manager zelfs als de toepassing heeft geen ondersteuning voor HTTP of HTTPS gebruiken?

Ja. U kunt TCP opgeven als het controle-protocol en Traffic Manager kan een TCP-verbinding tot stand brengen en wachten op een reactie van het eindpunt. Als de verbindingsaanvraag met een antwoord voor het maken van de verbinding, binnen de time-outperiode beantwoordt het eindpunt is dat eindpunt als goed gemarkeerd.

### <a name="what-specific-responses-are-required-from-the-endpoint-when-using-tcp-monitoring"></a>Welke specifieke antwoorden zijn vereist van het eindpunt met TCP-controle?

Bij gebruik van TCP-controle begint Traffic Manager een TCP-handshake drie richtingen met het verzenden van een SYN-aanvraag naar eindpunt op de opgegeven poort. Deze Wacht enige tijd (zoals opgegeven in de time-outinstellingen) op een reactie van het eindpunt. Het eindpunt reageert op de SYN-aanvraag met een antwoord SYN-ACK binnen de time-outperiode die is opgegeven in de controle-instellingen, klikt u vervolgens dat eindpunt worden beschouwd als in orde. Als het SYN-ACK-antwoord wordt ontvangen, de Traffic Manager de verbinding wordt hersteld door te reageren op een eerste.

### <a name="how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint"></a>Hoe snel Traffic Manager Mijn gebruikers weg van een slecht eindpunt verplaatsen?

Traffic Manager biedt meerdere instellingen die u helpen kunnen bij het bepalen van de failover-werking van uw Traffic Manager-profiel als volgt:
- u kunt opgeven dat de Traffic Manager-tests de eindpunten vaker worden uitgegeven door het scannen van Interval op 10 seconden. Dit zorgt ervoor dat een willekeurig eindpunt slecht gaat zo snel mogelijk kan worden gedetecteerd. 
- u kunt opgeven hoe lang moet worden gewacht voordat een health uitchecken aanvraagtijden (minimale time-outwaarde is 5 per seconde).
- u kunt opgeven hoeveel fouten kunnen optreden voordat het eindpunt is als beschadigd gemarkeerd. Deze waarde mag minimaal 0, in dat geval het eindpunt als slecht is gemarkeerd als de eerste statuscontrole is mislukt. Echter, met de laagste waarde van 0 voor het verdragen aantal fouten kan leiden tot eindpunten die worden uitgevoerd buiten de draaihoek vanwege tijdelijke problemen die op het moment van scannen optreden kunnen.
- u kunt de time-to-live (TTL) opgeven voor het DNS-antwoord moet zo laag 0. Dit doet, betekent dat de DNS-resolvers kunnen niet de reactie in cache en elke nieuwe query een antwoord wordt dat de meest actuele status informatie die de Traffic Manager is opgenomen.

Met deze instellingen voor kunt Traffic Manager bieden failovers onder 10 seconden nadat een eindpunt niet in orde komt en een DNS-query is gemaakt op basis van het bijbehorende profiel.

### <a name="how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile"></a>Hoe kan ik andere controle-instellingen voor verschillende eindpunten in een profiel opgeven?

Traffic Manager-controle-instellingen zijn op een per profiel niveau. Als u gebruiken een ander controle-instelling voor slechts één eindpunt wilt, deze kan worden gedaan door gelet dat eindpunt als een [genest profiel](traffic-manager-nested-profiles.md) waarvan bewakingsinstellingen wijken af van het profiel van de bovenliggende.

### <a name="what-host-header-do-endpoint-health-checks-use"></a>Welke host-header wilt eindpunt health controleert gebruiken?

Traffic Manager maakt gebruik van hostheaders statuscontroles van HTTP en HTTPS. De host-header gebruikt door Traffic Manager is de naam van de doel-eindpunt geconfigureerd in het profiel. De waarde die wordt gebruikt in de host-header kan niet afzonderlijk van de eigenschap target worden opgegeven.

### <a name="what-are-the-ip-addresses-from-which-the-health-checks-originate"></a>Wat zijn de IP-adressen van waaruit de status controleert afkomstig zijn?

De volgende lijst bevat de IP-adressen van waaruit de gezondheid van Traffic Manager controleert kan afkomstig zijn. U kunt deze lijst om ervoor te zorgen dat binnenkomende verbindingen van deze IP-adressen om te controleren van de status op de eindpunten zijn toegestaan.

* 40.68.30.66
* 40.68.31.178
* 137.135.80.149
* 137.135.82.249
* 23.96.236.252
* 65.52.217.19
* 40.87.147.10
* 40.87.151.34
* 13.75.124.254
* 13.75.127.63
* 52.172.155.168
* 52.172.158.37
* 104.215.91.84
* 13.75.153.124
* 13.84.222.37
* 23.101.191.199
* 23.96.213.12
* 137.135.46.163
* 137.135.47.215
* 191.232.208.52
* 191.232.214.62
* 13.75.152.253
* 104.41.187.209
* 104.41.190.203
* 52.173.90.107
* 52.173.250.232
* 104.45.149.110
* 40.114.5.197
* 52.240.151.125
* 52.240.144.45
* 13.65.95.152
* 13.65.92.252
* 40.78.67.110
* 104.42.192.195

### <a name="how-many-health-checks-to-my-endpoint-can-i-expect-from-traffic-manager"></a>Hoeveel statuscontroles naar Mijn eindpunt kan ik verwachten van Traffic Manager?

Het aantal Traffic Manager-health controleert uw eindpunt te bereiken is afhankelijk van het volgende:
- de waarde die u hebt ingesteld voor de controle-interval (kleinere interval betekent meer aanvragen aanvoer op uw eindpunt in een bepaalde periode).
- het aantal locaties van waar de statuscontroles afkomstig (het IP-adressen zijn van waar u kunt verwachten deze controles wordt vermeld in de voorgaande Veelgestelde vragen).

## <a name="traffic-manager-nested-profiles"></a>Traffic Manager geneste profielen

### <a name="how-do-i-configure-nested-profiles"></a>Hoe kan ik geneste profielen configureren?

Geneste Traffic Manager-profielen kunnen worden geconfigureerd met behulp van zowel de Azure Resource Manager en de klassieke Azure REST API's, Azure PowerShell-cmdlets en Azure CLI-opdrachten voor cross-platform. Ze worden ook ondersteund via de nieuwe Azure portal.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Ondersteuning voor het aantal lagen van geneste biedt Traffic Manager?

U kunt profielen maximaal 10 niveaus nesten. De lus' zijn niet toegestaan.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Kan ik andere eindpunttypen meng met geneste onderliggende profielen, in dezelfde Traffic Manager-profiel

Ja. Er zijn geen beperkingen op hoe u de eindpunten van verschillende typen binnen een profiel combineren.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Hoe geldt het facturering model voor geneste profielen

Er is geen negatieve gevolgen van het gebruik van geneste profielen prijzen.

Traffic Manager facturering bestaat uit twee onderdelen: eindpunt statuscontroles en miljoenen DNS-query's

* Eindpunt statuscontroles: Er zijn geen kosten voor een onderliggende profiel wanneer geconfigureerd als een eindpunt in een bovenliggende-profiel. Bewaking van de eindpunten in het onderliggende profiel wordt in rekening gebracht op de gebruikelijke manier.
* DNS-query's: elke query slechts één keer wordt geteld. Een query op een bovenliggende-profiel dat een eindpunt van een onderliggende-profiel retourneert wordt op basis van het profiel van bovenliggende alleen geteld.

Zie voor meer informatie de [Traffic Manager pagina met prijzen](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Is er een invloed op de prestaties voor geneste profielen?

Nee. Er is geen invloed op de prestaties tijdens de geneste profielen gebruiken.

De naamservers Traffic Manager door de hiërarchie profiel intern bladeren bij het verwerken van elke DNS-query. Een DNS-query naar een bovenliggende-profiel kunt u een DNS-antwoord met een eindpunt van een profiel voor een onderliggende ontvangen. Een enkele CNAME-record wordt gebruikt of u van een profiel voor een enkele of geneste profielen gebruikmaakt. Er is niet nodig voor het maken van een CNAME-record voor elk uitvoeringsprofiel in de hiërarchie.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Hoe wordt de status van een geneste eindpunt in een profiel voor een bovenliggende berekend met Traffic Manager?

Het profiel van de bovenliggende uitvoeren rechtstreeks statuscontroles op de onderliggende niet. In plaats daarvan de status van de eindpunten van het onderliggende profiel worden gebruikt voor het berekenen van de algemene status van het onderliggende profiel. Deze informatie wordt doorgegeven omhoog in de hiërarchie geneste profiel de status van de geneste-eindpunt te bepalen. Het profiel van de bovenliggende gebruikt dit geaggregeerde status om te bepalen of het verkeer worden omgeleid naar de onderliggende.

De volgende tabel beschrijft het gedrag van Traffic Manager, health worden gecontroleerd voor een geneste eindpunt.

| Onderliggende profiel Monitor-status | De replicatiestatus van bovenliggende Monitor-eindpunt | Opmerkingen |
| --- | --- | --- |
| Uitgeschakeld. Het onderliggende profiel is uitgeschakeld. |Stopped |De bovenliggende status is gestopt, niet is uitgeschakeld. De status uitgeschakeld is gereserveerd voor die aangeeft dat u het eindpunt in het profiel van de bovenliggende hebt uitgeschakeld. |
| Gedegradeerd. Ten minste één onderliggende profieleindpunt heeft een status gedegradeerd. |Online: het aantal Online eindpunten in het onderliggende profiel is ten minste de waarde van MinChildEndpoints.<BR>CheckingEndpoint: het aantal Online plus CheckingEndpoint eindpunten in het onderliggende profiel is ten minste de waarde van MinChildEndpoints.<BR>Gedegradeerd: anders. |Verkeer wordt doorgestuurd naar een eindpunt van de status CheckingEndpoint. Als MinChildEndpoints te hoog instelt is, wordt het eindpunt altijd gedegradeerd. |
| Online. Ten minste één onderliggende profieleindpunt is een Online status. Er is geen eindpunt heeft de status gedegradeerd. |Zie hierboven. | |
| CheckingEndpoints. Ten minste één onderliggende profieleindpunt is 'CheckingEndpoint'. Er zijn geen eindpunten 'Online' of 'Gedegradeerd' |Hetzelfde als hierboven. | |
| Niet-actief. Alle onderliggende profiel eindpunten zijn uitgeschakeld of gestopt, of dit profiel bevat geen eindpunten. |Stopped | |

## <a name="next-steps"></a>Volgende stappen:
- Meer informatie over het Traffic Manager [eindpunt bewakings- en automatische failover](../traffic-manager/traffic-manager-monitoring.md).
- Meer informatie over het Traffic Manager [verkeersrouteringsmethoden](../traffic-manager/traffic-manager-routing-methods.md).
