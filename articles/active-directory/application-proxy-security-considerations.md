---
title: Beveiligingsoverwegingen voor Azure AD-toepassingsproxy | Microsoft Docs
description: Bevat informatie over de beveiligingsoverwegingen voor het gebruik van Azure AD-toepassingsproxy
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 2370c4717e2cff6b4b8113b09624ef873b309647
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="security-considerations-for-accessing-apps-remotely-with-azure-ad-application-proxy"></a>Beveiligingsoverwegingen voor toegang tot apps op afstand met Azure AD-toepassingsproxy

Dit artikel wordt uitgelegd dat de onderdelen die werken veilig te houden uw gebruikers en toepassingen wanneer u Azure Active Directory-toepassingsproxy.

Het volgende diagram laat zien hoe Azure AD kunt veilige externe toegang tot uw on-premises toepassingen.

 ![Diagram van veilige externe toegang via Azure AD-toepassingsproxy](./media/application-proxy-security-considerations/secure-remote-access.png)

## <a name="security-benefits"></a>De voordelen van beveiliging

Azure AD-toepassingsproxy biedt de voordelen van de volgende beveiliging:

### <a name="authenticated-access"></a>Geverifieerde toegang 

Als u gebruikmaken van Azure Active Directory vooraf-verificatie wilt, klikt u vervolgens alleen geverifieerde verbindingen toegang tot uw netwerk.

Azure AD-toepassingsproxy is afhankelijk van de Azure AD beveiligingstokenservice (STS) voor alle verificatie.  Vooraf-verificatie, blokkeert door de aard een groot aantal anonieme aanvallen, omdat alleen geauthenticeerde identiteiten toegang heeft tot de back-end-toepassing.

Als u Passthrough als uw methode voor verificatie vooraf kiest, kunt u voordeel niet ophalen. 

### <a name="conditional-access"></a>Voorwaardelijke toegang

Besturingselementen voor uitgebreidere beleid van toepassing voordat verbindingen met uw netwerk tot stand worden gebracht.

Met [voorwaardelijke toegang](active-directory-conditional-access-azure-portal-get-started.md), kunt u beperkingen op welk verkeer is toegestaan voor toegang tot uw back-end-toepassingen. Kunt u beleidsregels die aanmeldingen op basis van locatie, sterkte van verificatie en gebruikersprofiel risico beperken.

U kunt ook voorwaardelijke toegang gebruiken voor het configureren van multi-factor Authentication-beleid, nog een beveiligingslaag toe te voegen aan uw gebruikersverificaties. 

### <a name="traffic-termination"></a>Beëindiging van verkeer

Al het verkeer wordt beëindigd in de cloud.

Omdat Azure AD-toepassingsproxy een reverse proxy is, wordt al het verkeer naar de back-end-toepassingen wordt beëindigd bij de service. De sessie kan ophalen opnieuw tot stand gebracht alleen met de back-end-server, wat betekent dat uw back-endservers niet worden blootgesteld aan direct HTTP-verkeer. Deze configuratie betekent dat u beter beschermd zijn tegen gerichte aanvallen.

### <a name="all-access-is-outbound"></a>Alle gebruikers de toegang is uitgaand 

U hoeft niet te openen van binnenkomende verbindingen met het bedrijfsnetwerk.

Application Proxy connectors alleen uitgaande verbindingen naar de service Azure AD-toepassingsproxy, wat betekent dat niet hoeft te openen, firewall-poorten voor binnenkomende verbindingen gebruiken. Traditionele proxy's vereist een perimeternetwerk (ook wel bekend als *DMZ*, *gedemilitariseerde zone*, of *gescreend subnet*) en toegang hebben tot niet-geverifieerde verbindingen aan de rand van het netwerk. Dit scenario vereist investeringen in web application firewallproducten voor het analyseren van verkeer en de omgeving te beveiligen. Met toepassingsproxy hoeft u niet een perimeternetwerk omdat alle verbindingen uitgaande zijn en plaats via een beveiligd kanaal vindt.

Zie voor meer informatie over connectors [inzicht in Azure AD-toepassingsproxy connectors](application-proxy-understand-connectors.md).

### <a name="cloud-scale-analytics-and-machine-learning"></a>Cloud-scale analytics en machine learning 

Geavanceerde beveiliging worden opgehaald.

Omdat deze deel uitmaakt van Azure Active Directory, kunnen gebruikmaken van toepassingsproxy [Azure AD Identity Protection](active-directory-identityprotection.md), met gegevens van de Microsoft Security Response Center en de Digital Crimes Unit. Samen we proactief identificeren waarmee is geknoeid en bieden bescherming tegen aanmeldingen met een hoog risico. We rekening gehouden met meerdere factoren om te bepalen welke attemps aanmelden met een hoog risico zijn. Deze factoren omvatten markeert geïnfecteerde apparaten, anonymizing netwerken en locaties ongebruikelijk of waarschijnlijk niet.

Veel van deze rapporten en gebeurtenissen zijn al beschikbaar via een API voor de integratie met uw security information en event management (SIEM)-systemen.

### <a name="remote-access-as-a-service"></a>Externe toegang als een service

U hoeft niet te hoeven maken over het onderhouden en patchen lokale servers.

Niet-gepatchte software nog steeds accounts voor een groot aantal aanvallen. Azure AD-toepassingsproxy is een Internet-scale-service die Microsoft eigenaar is, zodat u altijd de meest recente beveiligingspatches en upgrades.

Ter verbetering van de beveiliging van toepassingen die zijn gepubliceerd door Azure AD-toepassingsproxy blokkeren we web crawler robots indexeren en uw toepassingen wilt archiveren. Elke keer web crawler robot probeert op te halen van de robot-instellingen voor een gepubliceerde app toepassingsproxy geantwoord met een robots.txt-bestand met `User-agent: * Disallow: /`.

### <a name="ddos-prevention"></a>DDOS voorkomen

Toepassingen die zijn gepubliceerd via toepassingsproxy zijn beschermd tegen aanvallen gedistribueerd DOS van Service (DDOS).

De Webtoepassingsproxy-service controleert de hoeveelheid verkeer probeert toegang te krijgen van de toepassingen en het netwerk. Als het aantal apparaten die externe toegang tot uw toepassingen aanvragen is bereikt, beperkt Microsoft toegang tot uw netwerk. 

Microsoft controleert verkeerspatronen voor afzonderlijke toepassingen en voor uw abonnement als geheel. Als één toepassing is hoger dan normaal aanvragen ontvangt, worden de aanvragen voor toegang tot de toepassing geweigerd voor een korte periode. Als u hoger dan normaal aanvragen over uw hele abonnement ontvangt, worden aanvragen voor toegang tot uw Apps geweigerd. Deze preventieve maatregel houdt uw toepassingsservers overbelast raakt door externe toegangsaanvragen, zodat uw on-premises gebruikers hun apps kunnen blijven gebruiken. 

## <a name="under-the-hood"></a>Onderhuids

Azure AD-toepassingsproxy bestaat uit twee delen:

* De cloudservice: deze service wordt uitgevoerd in Azure, en is waar de verbindingen van de externe client/gebruiker zijn gemaakt.
* [De on-premises connector](application-proxy-understand-connectors.md): een onderdeel van de lokale de connector luistert naar aanvragen van de Azure AD-toepassingsproxy-service en -ingangen verbindingen naar de interne toepassingen. 

Een stroom tussen de connector en de service-toepassingsproxy tot stand wordt gebracht wanneer:

* De connector is eerst instellen.
* De connector haalt de configuratie-informatie van de service-toepassingsproxy.
* Een gebruiker toegang krijgt tot een gepubliceerde toepassing.

>[!NOTE]
>Alle communicatie vindt plaats via SSL en ze altijd afkomstig van de connector met de service-toepassingsproxy. De service is alleen uitgaand.

De connector gebruikt een certificaat voor verificatie bij de service-toepassingsproxy voor bijna alle aanroepen. De enige uitzondering op dit proces is de initiële installatie stap, waarbij het clientcertificaat tot stand is gebracht.

### <a name="installing-the-connector"></a>De connector installeren

Wanneer de connector eerst instelt is, wordt de volgende stroom gebeurtenissen plaatsvinden:

1. De registratie van de connector met de service wordt uitgevoerd als onderdeel van de installatie van de connector. Gebruikers gevraagd hun Azure AD-referenties invoeren. Het token dat is verkregen van deze verificatie wordt vervolgens gepresenteerd aan de service Azure AD-toepassingsproxy.
2. De service voor toepassingsproxy evalueert het token. Er wordt gecontroleerd of de gebruiker een bedrijfsbeheerder zijn in de tenant is. Als de gebruiker geen beheerder is, wordt het proces beëindigd.
3. De connector genereert een certificaataanvraag van de client en wordt doorgegeven, samen met het token, met de service-toepassingsproxy. De service het token heeft geverifieerd op zijn beurt en de clientaanvraag certificaat ondertekent.
4. De connector gebruikt het clientcertificaat voor toekomstige communicatie met de service-toepassingsproxy.
5. De connector voert een initiële pull van het systeem-configuratiegegevens van de service met behulp van het clientcertificaat en het is nu klaar om aanvragen te.

### <a name="updating-the-configuration-settings"></a>Bijwerken van de configuratieinstellingen

Wanneer de service Application Proxy-updates voor de configuratie-instellingen, worden de volgende stroom gebeurtenissen plaatsvinden:

1. De connector maakt verbinding met het eindpunt van de configuratie in de service voor toepassingsproxy met behulp van het clientcertificaat.
2. Nadat het clientcertificaat is gevalideerd, stuurt de service-toepassingsproxy configuratiegegevens naar de connector (bijvoorbeeld de connector groep die de connector deel uitmaken van moeten).
3. Als het huidige certificaat meer dan 180 dagen is, genereert de connector een nieuwe certificaataanvraag waarmee het clientcertificaat effectief per 180 dagen bijgewerkt.

### <a name="accessing-published-applications"></a>Toegang tot gepubliceerde toepassingen

Als gebruikers toegang hebben tot een gepubliceerde toepassing, plaatsvinden de volgende gebeurtenissen tussen de toepassingsproxy-service en de Application Proxy connector:

1. [De service verifieert de gebruiker voor de app](#the-service-checks-the-configuration-settings-for-the-app)
2. [De service plaatst een aanvraag in de connectorwachtrij](#The-service-places-a-request-in-the-connector-queue)
3. [De aanvraag uit de wachtrij wordt verwerkt door een connector](#the-connector-receives-the-request-from-the-queue)
4. [De connector wordt gewacht op een reactie](#the-connector-waits-for-a-response)
5. [De service streams gegevens naar de gebruiker](#the-service-streams-data-to-the-user)

Houd lezen voor meer informatie over wat in elk van deze stappen plaatsvindt.


#### <a name="1-the-service-authenticates-the-user-for-the-app"></a>1. De service verifieert de gebruiker voor de app

Als u de app om Passthrough gebruiken als de methode voor verificatie vooraf geconfigureerd, worden de stappen in deze sectie worden overgeslagen.

Als u de app met Azure AD worden geconfigureerd, worden gebruikers omgeleid naar de Azure AD-STS om te verifiëren en de volgende stappen worden uitgevoerd:

1. Toepassingsproxy controleert u alle vereisten voor het beleid van voorwaardelijke toegang voor de specifieke toepassing. Deze stap zorgt ervoor dat de gebruiker is toegewezen aan de toepassing. Als verificatie in twee stappen vereist is, vraagt de verificatiemethode die de gebruiker om een tweede methode voor verificatie.

2. Nadat alle controles zijn verstreken, wordt de Azure AD STS een ondertekende token voor de toepassing en wordt de gebruiker omgeleid naar de service-toepassingsproxy.

3. Toepassingsproxy controleert of het token is uitgegeven voor het corrigeren van de toepassing. Het andere controles ook, zoals ervoor te zorgen dat het token is ondertekend door Azure AD en is nog steeds binnen het geldige venster uitgevoerd.

4. Application Proxy sets een gecodeerde verificatiecookie om aan te geven dat de verificatie bij de toepassing heeft plaatsgevonden. De cookie bevat de tijdstempel van een vervaldatum die gebaseerd op het token van Azure AD en andere gegevens, zoals de naam van de gebruiker die de verificatie is gebaseerd op. De cookie wordt versleuteld met een persoonlijke sleutel die alleen bekend is bij de service-toepassingsproxy.

5. Toepassingsproxy leidt de gebruiker terug naar de oorspronkelijk aangevraagde URL.

Als een deel van de stappen die vooraf-verificatie is mislukt, wordt van de gebruiker de aanvraag is geweigerd en wordt de gebruiker een bericht met de bron van het probleem wordt weergegeven.


#### <a name="2-the-service-places-a-request-in-the-connector-queue"></a>2. De service plaatst een aanvraag in de connectorwachtrij

Connectors openhouden die een uitgaande verbinding met de service-toepassingsproxy. Als een aanvraag binnenkomt, wordt de service wachtrijen van de aanvraag op een van de geopende verbindingen voor de connector om op te halen.

De aanvraag bevat de items uit de toepassing, zoals de aanvraagheaders gegevens uit de gecodeerde cookie, de gebruiker de aanvraag en/of de aanvraag-ID. Hoewel de gegevens van de versleutelde cookie aan de aanvraag is verzonden, is de verificatiecookie zelf niet.

#### <a name="3-the-connector-processes-the-request-from-the-queue"></a>3. De aanvraag uit de wachtrij wordt verwerkt door de connector. 

Toepassingsproxy voert op basis van de aanvraag, een van de volgende acties:

* Als de aanvraag een eenvoudige operatie is (bijvoorbeeld: Er zijn geen gegevens in de hoofdtekst is met een RESTful *ophalen* aanvraag), de connector maakt verbinding met de interne doel-resource en vervolgens wordt gewacht op een antwoord.

* Als de aanvraag bevat de gegevens die zijn gekoppeld in de hoofdtekst (bijvoorbeeld een RESTful *POST* bewerking), de connector een uitgaande verbinding maakt met behulp van het clientcertificaat met het exemplaar van de toepassingsproxy. Dit maakt deze verbinding met het aanvragen van de gegevens en een verbinding met de interne resource openen. Nadat deze de aanvraag van de connector is ontvangen, wordt de service voor toepassingsproxy begint met het accepteren van de inhoud van de gebruiker en stuurt gegevens naar de connector. De connector stuurt op zijn beurt de gegevens naar de interne resource.

#### <a name="4-the-connector-waits-for-a-response"></a>4. De connector wacht een antwoord.

Nadat de aanvraag en de overdracht van alle inhoud naar de back-end is voltooid, wordt de connector wordt gewacht op een antwoord.

Nadat hij een antwoord ontvangt, de connector een uitgaande verbinding maakt met de service-toepassingsproxy, om te retourneren van de details van de header en beginnen met het streamen van de geretourneerde gegevens.

#### <a name="5-the-service-streams-data-to-the-user"></a>5. De service streams gegevens naar de gebruiker. 

Sommige verwerking van de toepassing optreden hier. Als u toepassingsproxy headers omzetten of URL's in uw toepassing hebt geconfigureerd, gebeurt dat verwerking zo nodig tijdens deze stap.


## <a name="next-steps"></a>Volgende stappen

[Netwerk topologie overwegingen bij het gebruik van Azure AD-toepassingsproxy](application-proxy-network-topology-considerations.md)

[Azure AD-toepassingsproxy connectors begrijpen](application-proxy-understand-connectors.md)
