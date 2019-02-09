---
title: Beveiligingsoverwegingen voor Azure AD-toepassingsproxy | Microsoft Docs
description: Bevat informatie over de beveiligingsoverwegingen voor het gebruik van Azure AD-toepassingsproxy
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/08/2017
ms.author: celested
ms.reviewer: japere
ms.custom: it-pro
ms.openlocfilehash: 2b12e7548d3393419d76cd5e1bbe840743fdc571
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55961072"
---
# <a name="security-considerations-for-accessing-apps-remotely-with-azure-ad-application-proxy"></a>Beveiligingsoverwegingen voor het openen van apps op afstand met Azure AD-toepassingsproxy

In dit artikel wordt uitgelegd dat de onderdelen die werken aan uw gebruikers en toepassingen veilig houden wanneer u Azure Active Directory-toepassingsproxy.

Het volgende diagram laat zien hoe Azure AD kunt veilige externe toegang tot uw on-premises toepassingen.

 ![Diagram van veilige externe toegang via Azure AD-toepassingsproxy](./media/application-proxy-security/secure-remote-access.png)

## <a name="security-benefits"></a>De voordelen van beveiliging

Azure AD Application Proxy biedt de voordelen van de volgende beveiliging:

### <a name="authenticated-access"></a>Geverifieerde toegang 

Als u ervoor Azure Active Directory vooraf-verificatie gebruiken kiest, klikt u vervolgens alleen geverifieerde verbindingen toegang tot uw netwerk.

Azure AD-toepassingsproxy, is afhankelijk van de Azure AD security token service (STS) voor alle verificatie.  Vooraf-verificatie, blokkeert door de aard, een groot aantal anonieme aanvallen, omdat alleen geauthenticeerde identiteiten toegang hebben tot de back-endtoepassing.

Als u ervoor Passthrough als uw methode voor verificatie vooraf kiest, krijgt u geen dit voordeel. 

### <a name="conditional-access"></a>Voorwaardelijke toegang

Besturingselementen voor uitgebreidere beleid van toepassing voordat verbindingen met uw netwerk tot stand worden gebracht.

Met [voorwaardelijke toegang](../conditional-access/overview.md), kunt u beperkingen op welk verkeer is toegestaan voor toegang tot uw back-end-toepassingen. U kunt beleidsregels maken die aanmeldingen op basis van locatie, sterkte van verificatie- en gebruikersprofiel van het risico te beperken.

U kunt ook gebruik van voorwaardelijke toegang configureren van multi-factor Authentication-beleid, nog een beveiligingslaag toe te voegen aan uw gebruikersverificaties. Bovendien uw toepassingen kunnen ook worden doorgestuurd naar Microsoft Cloud App Security via de Azure AD voor voorwaardelijke toegang om realtime-controle en besturingselementen, via [toegang](https://docs.microsoft.com/cloud-app-security/access-policy-aad) en [sessie](https://docs.microsoft.com/cloud-app-security/session-policy-aad) beleid

### <a name="traffic-termination"></a>Beëindiging van verkeer

Al het verkeer wordt beëindigd in de cloud.

Omdat Azure AD Application Proxy reverse proxy is, wordt al het verkeer naar de back-end-toepassingen wordt beëindigd bij de service. De sessie kan ophalen opnieuw tot stand gebracht met de back-end-server, wat betekent dat uw back-endservers niet worden blootgesteld aan HTTP-verkeer. Deze configuratie betekent dat u beter beschermd tegen gerichte aanvallen.

### <a name="all-access-is-outbound"></a>Alle toegang is uitgaand 

U hoeft niet te openen van binnenkomende verbindingen met het bedrijfsnetwerk.

Toepassingsproxyconnectors gebruik alleen uitgaande verbindingen naar de Azure AD Application Proxy-service, wat betekent dat is het niet nodig om firewallpoorten voor binnenkomende verbindingen te openen. Traditionele proxy vereist een perimeternetwerk (ook wel bekend als *DMZ*, *gedemilitariseerde zone*, of *screened subnet genoemd*) en toegang hebben tot niet-geverifieerde verbindingen aan de rand van het netwerk. In dit scenario vereist investeringen in web application firewallproducten voor het analyseren van netwerkverkeer en de omgeving te beschermen. Met Application Proxy hoeft u geen een perimeternetwerk omdat alle verbindingen uitgaande zijn en plaats via een beveiligd kanaal vindt.

Zie voor meer informatie over connectors [over Azure AD Application Proxy connectors](application-proxy-connectors.md).

### <a name="cloud-scale-analytics-and-machine-learning"></a>Schaalbare analytics en machine learning 

Geavanceerde beveiliging en bescherming krijgen.

Omdat deze deel uitmaakt van Azure Active Directory, Application Proxy kan gebruikmaken van [Azure AD Identity Protection](../active-directory-identityprotection.md), met gegevens van de Microsoft Security Response Center en de Digital Crimes Unit. Samen we proactief identificeren accounts waarmee is geknoeid en bieden van bescherming tegen riskante aanmeldingen. We houden rekening talloze factoren om te bepalen welke probeert aanmelding goed met een hoog risico. Deze factoren omvatten markeringen geïnfecteerde apparaten, anoniem netwerken en ongewone of onwaarschijnlijk locaties.

Veel van deze rapporten en gebeurtenissen zijn al beschikbaar via een API voor integratie met uw security information en event management (SIEM) systemen.

### <a name="remote-access-as-a-service"></a>Externe toegang als een service

U hebt geen zorgen te hoeven maken over het onderhoud en patching uit handen on-premises servers.

Nog steeds niet-gepatchte software-accounts voor een groot aantal aanvallen. Azure AD Application Proxy is een internetschaal-service die eigendom zijn van Microsoft, zodat u altijd de meest recente beveiligingspatches en upgrades.

Ter verbetering van de beveiliging van toepassingen die zijn gepubliceerd door de Azure AD-toepassingsproxy, blokkeren we crawler Webrobots van indexeren en archiveren van uw toepassingen. Telkens wanneer een web crawler robot probeert op te halen van de robot-instellingen voor een gepubliceerde app Application Proxy reageert met een robots.txt-bestand met `User-agent: * Disallow: /`.

### <a name="ddos-prevention"></a>DDOS-preventie

Toepassingen die zijn gepubliceerd via toepassingsproxy zijn beschermd tegen aanvallen van Distributed Denial-of van de Service (DDOS).

De Application Proxy-service controleert de hoeveelheid verkeer er wordt geprobeerd om uw toepassingen en het netwerk te bereiken. Als het aantal apparaten extern toegang krijgen tot uw toepassingen pieken, beperkt Microsoft toegang tot uw netwerk. 

Microsoft controleert verkeerspatronen voor afzonderlijke toepassingen en voor uw abonnement als geheel. Als één toepassing is hoger dan normaal aanvragen ontvangt, worden de verzoeken voor toegang tot deze toepassing geweigerd voor een korte periode. Als u hoger dan normaal aanvragen in uw hele abonnement ontvangt, klikt u vervolgens verzoeken voor toegang tot uw Apps geweigerd. Deze preventieve maatregel zorgt ervoor dat uw toepassingsservers niet wordt overbelast door aanvragen van externe toegang, zodat uw on-premises gebruikers houden toegang hun apps tot kunnen. 

## <a name="under-the-hood"></a>Achter de schermen

Azure AD-toepassingsproxy bestaat uit twee onderdelen:

* De cloud gebaseerde service: Deze service wordt uitgevoerd in Azure, en is waar de verbindingen van de externe client/gebruiker zijn gemaakt.
* [De on-premises connector](application-proxy-connectors.md): Een on-premises-onderdeel, de connector luistert naar aanvragen van de Azure AD Application Proxy-service en verwerkt de verbindingen naar de interne toepassingen. 

Een stroom tussen de connector en de Application Proxy-service tot stand is gebracht wanneer:

* De connector is eerst ingesteld.
* De connector haalt configuratie-informatie uit de Application Proxy-service.
* Een gebruiker toegang heeft tot een gepubliceerde toepassing.

>[!NOTE]
>Alle communicatie vindt plaats via SSL en ze altijd afkomstig van de connector met de Application Proxy-service. De service is alleen uitgaande.

De connector gebruikt een certificaat voor verificatie bij de service voor toepassingsproxy voor bijna alle aanroepen. De enige uitzondering op dit proces is de stap van de eerste configuratie waarbij het clientcertificaat tot stand is gebracht.

### <a name="installing-the-connector"></a>De connector installeren

Als de connector eerst is ingesteld, worden de volgende stroom gebeurtenissen plaatsvinden:

1. De registratie van de connector met de service wordt uitgevoerd als onderdeel van de installatie van de connector. Gebruikers zijn gevraagd hun Azure AD-beheerder-referenties in te voeren. Het token dat is verkregen via deze verificatie wordt vervolgens weergegeven met de Azure AD Application Proxy-service.
2. De Application Proxy-service beoordeelt wat het token. Er wordt gecontroleerd of de gebruiker een bedrijfsbeheerder in de tenant is. Als de gebruiker niet een beheerder is, wordt het proces beëindigd.
3. De connector een certificaataanvraag client genereert en geeft deze, samen met het token, naar de Application Proxy-service. De service wordt op zijn beurt controleert of het token en de aanvraag van de client-certificaat ondertekent.
4. De connector maakt gebruik van het clientcertificaat voor toekomstige communicatie met de Application Proxy-service.
5. De connector voert een initiële pull van het systeem-configuratiegegevens van de service met behulp van het clientcertificaat en het is nu klaar zijn voor aanvragen.

### <a name="updating-the-configuration-settings"></a>De configuratie-instellingen bijwerken

Wanneer de Application Proxy-service-updates de configuratie-instellingen, worden de volgende stroom gebeurtenissen plaatsvinden:

1. De connector maakt verbinding met het eindpunt van de configuratie in de Application Proxy-service met behulp van het clientcertificaat.
2. Nadat het clientcertificaat is gevalideerd, stuurt de Application Proxy-service configuratiegegevens naar de connector (bijvoorbeeld de connectorgroep die de connector deel uitmaken van moeten).
3. Als het huidige certificaat meer dan 180 dagen oud is, genereert de connector een nieuwe certificaataanvraag, waarmee het clientcertificaat effectief per 180 dagen bijgewerkt.

### <a name="accessing-published-applications"></a>Toegang tot gepubliceerde toepassingen

Wanneer gebruikers toegang krijgen een gepubliceerde toepassing tot, plaatsvinden de volgende gebeurtenissen tussen de Application Proxy-service en de Application Proxy-connector:

1. De service verifieert de gebruiker voor de app.
2. De service plaatst een aanvraag in de wachtrij van de connector
3. Een connector verwerkt de aanvraag uit de wachtrij
4. De connector wachten op antwoord
5. De service streams gegevens naar de gebruiker

Blijf lezen voor meer informatie over wat in elk van deze stappen plaatsvindt.


#### <a name="1-the-service-authenticates-the-user-for-the-app"></a>1. De service verifieert de gebruiker voor de app.

Als u de app voor het gebruik van Passthrough als de methode voor verificatie vooraf is geconfigureerd, worden de stappen in deze sectie worden overgeslagen.

Als u de app met Azure AD worden geconfigureerd, worden gebruikers omgeleid naar de Azure AD-STS om te verifiëren en de volgende stappen worden uitgevoerd:

1. Toepassingsproxy controleert op eventuele vereisten van het beleid voor voorwaardelijke toegang voor de specifieke toepassing. Deze stap zorgt ervoor dat de gebruiker is toegewezen aan de toepassing. Als verificatie in twee stappen vereist is, vraagt de verificatiemethode die de gebruiker om een tweede verificatiemethode.

2. Nadat alle controles zijn verstreken, wordt de Azure AD STS een ondertekende token voor de toepassing en wordt de gebruiker omgeleid naar de Application Proxy-service.

3. Toepassingsproxy worden geverifieerd of het token is uitgegeven aan de juiste toepassing. Deze andere voert controles uit ook, zoals ervoor te zorgen dat het token is ondertekend door Azure AD en is nog steeds binnen het geldige venster.

4. Application Proxy stelt in een versleutelde verificatiecookie om aan te geven dat de verificatie naar de toepassing is opgetreden. De cookie bevat de timestamp van een vervaldatum die gebaseerd op het token uit Azure AD en andere gegevens, zoals de naam van de gebruiker die de verificatie is gebaseerd op. De cookie is versleuteld met een persoonlijke sleutel die alleen bekend bij de Application Proxy-service.

5. Toepassingsproxy leidt de gebruiker terug naar de oorspronkelijk aangevraagde URL.

Als een onderdeel van de stappen die vooraf-verificatie is mislukt, van de gebruiker de aanvraag is geweigerd en de gebruiker een bericht weergegeven dat aangeeft van de bron van het probleem wordt weergegeven.


#### <a name="2-the-service-places-a-request-in-the-connector-queue"></a>2. De service plaatst een aanvraag in de wachtrij van de connector

Connectors Houd een uitgaande verbinding geopend met de Application Proxy-service. Wanneer een aanvraag binnenkomt, wachtrijen voor de service van de aanvraag op een van de geopende verbindingen voor de connector om op te halen.

De aanvraag bevat de items uit de toepassing, zoals de aanvraagheaders, gegevens van de versleutelde cookie, de gebruiker de aanvraag en de aanvraag-ID. Hoewel de gegevens van de versleutelde cookie wordt verzonden met de aanvraag, wordt de verificatiecookie zelf niet is.

#### <a name="3-the-connector-processes-the-request-from-the-queue"></a>3. De connector verwerkt de aanvraag uit de wachtrij. 

Application Proxy voert op basis van de aanvraag, een van de volgende acties:

* Als de aanvraag een eenvoudige operatie is (bijvoorbeeld, er zijn geen gegevens in de hoofdtekst is met een RESTful *ophalen* aanvraag), de connector maakt verbinding met de doelresource van de interne en wacht dan een reactie.

* Als de aanvraag gegevens die zijn gekoppeld aan deze in de hoofdtekst heeft (bijvoorbeeld een RESTful *POST* bewerking), de connector een uitgaande verbinding maakt met behulp van het clientcertificaat met het exemplaar van de Application Proxy. Het maakt deze verbinding om te vragen van de gegevens en geen verbinding met de interne resource openen. Zodra het verzoek van de connector is ontvangen, wordt de service voor toepassingsproxy begint met het accepteren van de inhoud van de gebruiker en verzendt gegevens naar de connector. De connector verzendt op zijn beurt de gegevens naar de interne resource.

#### <a name="4-the-connector-waits-for-a-response"></a>4. De connector wachten op antwoord.

Nadat de aanvraag en de overdracht van alle inhoud naar de back-end is voltooid, wacht de connector op een antwoord.

Zodra er een antwoord is ontvangen, kunt u de connector een uitgaande verbinding met de Application Proxy-service om te retourneren van de details van de koptekst en beginnen met streamen, de geretourneerde gegevens.

#### <a name="5-the-service-streams-data-to-the-user"></a>5. De service streams gegevens naar de gebruiker. 

Sommige verwerking van de toepassing kan hier optreden. Als u Application Proxy voor de omzetting van headers of URL's geconfigureerd in uw toepassing, gebeurt dat verwerking zo nodig tijdens deze stap.


## <a name="next-steps"></a>Volgende stappen

[Netwerk-topologie overwegingen bij het gebruik van Azure AD-toepassingsproxy](application-proxy-network-topology.md)

[Meer informatie over Azure AD Application Proxy connectors](application-proxy-connectors.md)
