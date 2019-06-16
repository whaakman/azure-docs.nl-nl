---
title: On-premises apps publiceren met Azure AD-toepassingsproxy
description: Begrijpen waarom Application Proxy gebruiken om te publiceren van on-premises webtoepassingen extern aan externe gebruikers. Meer informatie over Application Proxy-architectuur, connectors, verificatiemethoden en de voordelen van beveiliging.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: overview
ms.workload: identity
ms.date: 05/31/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f23b20d460952ae582c292c8015851b9dc2ea98
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108164"
---
# <a name="using-azure-ad-application-proxy-to-publish-on-premises-apps-for-remote-users"></a>Publiceren met behulp van Azure AD-toepassingsproxy on-premises-apps voor externe gebruikers

Azure Active Directory (Azure AD) biedt veel mogelijkheden voor het beveiligen van gebruikers, apps en gegevens in de cloud en on-premises. In het bijzonder kan de functie voor Azure AD Application Proxy worden geïmplementeerd door de IT-professionals die u wilt publiceren van on-premises webtoepassingen extern. Externe gebruikers die toegang nodig tot interne apps kunnen vervolgens toegang tot deze op een veilige manier.

De mogelijkheid om veilig toegang krijgen tot interne apps van buiten uw netwerk wordt in de moderne werkplek nog meer kritiek. Met scenario's zoals BYOD (Bring Your Own Device) en mobiele apparaten, IT-professionals gebruiken om te voldoen aan twee doelen:

* Zorg ervoor dat gebruikers om productief te zijn op elk gewenst moment en overal einde
* Zakelijke activa te allen tijde beschermen

Veel organisaties denkt dat ze zelf en beveiligd wanneer resources binnen de grenzen van hun zakelijke netwerken bestaan. Maar in digitale vandaag de dag die grens is uitgebreid met beheerde mobiele apparaten en resources en services in de cloud. Nu moet u voor het beheren van de complexiteit van het beveiligen van uw gebruikers-id's en gegevens die zijn opgeslagen op hun apparaten en apps.

Mogelijk gebruikt u Azure AD al voor het beheren van gebruikers in de cloud die toegang moeten krijgen tot Office 365 en andere SaaS-toepassingen, evenals webtoepassingen apps die worden gehost die on-premises. Als u Azure AD al hebt, kunt u deze kunt gebruiken als één controlelaag voor naadloze en veilige toegang tot uw on-premises toepassingen. Of misschien bent u nog steeds een verplaatsen naar de cloud overweegt. Als dit het geval is, kunt u uw overstap naar de cloud kunt beginnen met Application Proxy implementeren en waarbij de eerste stap voor het bouwen van een sterke identiteit foundation.

Terwijl niet uitgebreid en de onderstaande lijst ziet u enkele van de dingen die u inschakelen kunt door het implementeren van App-Proxy in een co-existentie hybride scenario:

* Extern on-premises web-apps publiceren op een vereenvoudigde manier zonder een DMZ
* Ondersteuning voor eenmalige aanmelding (SSO) alle apparaten, middelen en apps in de cloud en on-premises
* Ondersteuning voor multi-factor authentication voor apps in de cloud en on-premises
* Snel gebruikmaken van cloudfuncties met de beveiliging van de Microsoft-Cloud
* Centraliseer het beheer van gebruiker-account
* Beheer van identiteit en beveiliging centraliseren
* Automatisch toevoegen of verwijderen van de gebruikerstoegang tot toepassingen op basis van groepslidmaatschap

Dit artikel wordt uitgelegd hoe Azure AD en de toepassingsproxy zodat externe gebruikers een ervaring voor eenmalige aanmelding (SSO). Gebruikers is veilig verbinding maken met on-premises toepassingen zonder dat u een VPN of multihomed servers en firewall-regels. In dit artikel helpt u begrijpen hoe Application Proxy zorgt voor de mogelijkheden en beveiligingsvoordelen van de cloud in uw on-premises webtoepassingen. Ook wordt de architectuur en topologieën die mogelijk zijn beschreven.

## <a name="remote-access-in-the-past"></a>Externe toegang in het verleden

Voorheen was de controlelaag voor het beveiligen van interne bronnen kwaadwillende gebruikers gemakkelijker toegang door externe gebruikers en alle in de DMZ of perimeternetwerk. Maar de VPN- en reverse proxyoplossingen die zijn geïmplementeerd in het Perimeternetwerk door externe clients wordt gebruikt voor toegang tot zakelijke resources zijn niet geschikt in de cloudwereld. Ze maken doorgaans krijgen met de volgende nadelen:

* Hardwarekosten
* Beveiliging (patches, bewaking van poorten, enz.)
* Verifiëren van gebruikers aan de rand
* Verifiëren van gebruikers tot webservers in het perimeternetwerk
* Het onderhouden van VPN-toegang voor externe gebruikers met de distributie en de configuratie van VPN-clientsoftware. Bovendien onderhouden domein de servers die u in het Perimeternetwerk, die kwetsbaar zijn voor buiten aanvallen kan zijn.

In de cloudgeoriënteerde wereld van vandaag is Azure AD het meest geschikt is om te bepalen wie en wat in uw netwerk opgehaald. Azure AD Application Proxy kan worden geïntegreerd met moderne verificatie en cloud-gebaseerde technologieën, zoals SaaS-toepassingen en id-providers. Dankzij deze integratie kunnen gebruikers toegang krijgen tot apps vanaf elke locatie. App-Proxy is meer geschikt is voor vandaag digitale werkplek, is het veiliger dan het VPN- en reverse proxy-oplossingen en gemakkelijker te implementeren is niet alleen. Externe gebruikers kunnen toegang krijgen tot uw on-premises toepassingen dezelfde manier als ze toegang krijgen tot O365 en andere SaaS-apps die is geïntegreerd met Azure AD. U hoeft niet te wijzigen of bijwerken van uw toepassingen werken met Application Proxy. Bovendien vereist App Proxy geen binnenkomende verbindingen via de firewall openen. Met App-Proxy, u gewoon instellen en dit vergeet.

## <a name="the-future-of-remote-access"></a>De toekomst van externe toegang

In de hedendaagse digitale werkplek, wordt gebruikers overal werken met meerdere apparaten en apps. Alleen constante is de gebruikersidentiteit. Daarom is de eerste stap naar een beveiligd netwerk is vandaag nog met [van Azure AD-identiteitsbeheer](https://docs.microsoft.com/azure/security/security-identity-management-overview) mogelijkheden als uw besturingselement vlak van beveiliging. Een model die gebruikmaakt van identiteit als de controlelaag bestaat meestal uit de volgende onderdelen:

* Een id-provider om gebruikers en informatie over de gebruiker bij te houden.
* Apparaat-map voor het onderhouden van een lijst met apparaten die toegang tot bedrijfsresources hebben. Deze map bevat informatie over de bijbehorende apparaten (bijvoorbeeld: type apparaat, integriteit enz.).
* Beleid voor evaluatie-service om te bepalen of een gebruiker en apparaat voldoet aan het beleid door Beveiligingsbeheerders uiteengezet.
* De mogelijkheid om te verlenen of weigeren van toegang tot resources van de organisatie.

Met Application Proxy wordt Azure AD bijgehouden van gebruikers die toegang krijgen tot webtoepassingen apps gepubliceerd die moeten on-premises en in de cloud. Het biedt een centraal beheerpunt voor deze apps. Hoewel het niet vereist, verdient het aanbeveling dat u ook Azure AD voor voorwaardelijke toegang inschakelen. Door te definiëren voorwaarden voor hoe gebruikers verifiëren en toegang krijgen, u verdere Zorg ervoor dat de juiste personen toegang hebben tot toepassingen.

**Opmerking:** Het is belangrijk om te weten dat Azure AD Application Proxy is bedoeld als een VPN- of vervanging van de omgekeerde proxy voor roaming (of extern) gebruikers die toegang tot interne bronnen nodig hebben. Het niet bedoeld voor interne gebruikers op het bedrijfsnetwerk bevinden. Interne gebruikers die gebruikmaken van onnodig Application Proxy kunnen leiden tot onverwachte of ongewenste prestatieproblemen.

![Azure Active Directory en al uw apps](media/what-is-application-proxy/azure-ad-and-all-your-apps.png)

### <a name="an-overview-of-how-app-proxy-works"></a>Een overzicht van de werking van App-Proxy

Application Proxy is een Azure AD-service die u in Azure portal configureert. Hiermee kunt u voor het publiceren van een externe openbare URL voor HTTP/HTTPS-eindpunt in de Azure-Cloud maakt verbinding met de URL van een interne applicatie-server in uw organisatie. Deze on-premises web-apps kunnen worden geïntegreerd met Azure AD voor de ondersteuning van eenmalige aanmelding. Eindgebruikers kunnen vervolgens toegang tot on-premises web-apps op dezelfde manier als die ze toegang krijgen Office 365 en andere SaaS-apps tot.

Onderdelen van deze functie zijn onder andere de Application Proxy-service, die wordt uitgevoerd in de cloud, de Application Proxy-connector is een lichtgewicht agent die wordt uitgevoerd op een on-premises-server en Azure AD, dit is de id-provider. Alle drie de onderdelen werken samen om de gebruiker met een eenmalige aanmelding voor toegang tot on-premises webtoepassingen.

Na het aanmelden, externe gebruikers kunnen toegang krijgen tot on-premises webtoepassingen met behulp van een vertrouwde URL of de [MyApps Toegangsvenster](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access) vanaf hun bureaublad of iOS/MAC-apparaten. Bijvoorbeeld, App-Proxy krijgt u externe toegang en eenmalige aanmelding voor extern bureaublad, SharePoint-sites, Tableau, Qlik, Outlook op het web en line-of-business (LOB)-toepassingen.

![Azure AD-toepassingsproxy-architectuur](media/what-is-application-proxy/azure-ad-application-proxy-architecture.png)

### <a name="authentication"></a>Verificatie

Er zijn verschillende manieren om een toepassing voor eenmalige aanmelding te configureren en de methode die u selecteert, is afhankelijk van de verificatie die gebruikmaakt van uw toepassing. Application Proxy biedt ondersteuning voor de volgende typen toepassingen:

* Webtoepassingen
* Web-API's die u wilt weergeven voor de uitgebreide toepassingen op verschillende apparaten
* Toepassingen die worden gehost achter een extern bureaublad-Gateway
* Rich client-apps die kunnen worden geïntegreerd met de Active Directory Authentication Library (ADAL)

App-Proxy werkt met apps die gebruikmaken van de volgende systeemeigen verificatieprotocollen:

* **[Geïntegreerde Windows-verificatie (IWA)](application-proxy-configure-single-sign-on-with-kcd.md).** Voor IWA, wordt in de Application Proxy connectors Kerberos-beperkte delegatie (KCD) gebruiken om te verifiëren voor toegang tot de Kerberos-toepassing.

App-Proxy biedt ook ondersteuning voor de volgende verificatieprotocollen voor met integratie van derden of in de configuratie van specifieke scenario's:

* [**Verificatie op basis van een koptekst**](application-proxy-configure-single-sign-on-with-ping-access.md). Deze aanmeldingsmethode maakt gebruik van een derde partij verificatieservice PingAccess genoemd en wordt gebruikt wanneer de toepassing headers voor verificatie gebruikt. Verificatie wordt in dit scenario wordt verwerkt door PingAccess.
* [**Formulieren of wachtwoord gebaseerde verificatie**](application-proxy-configure-single-sign-on-password-vaulting.md). In deze verificatiemethode gebruikers zich bij de eerste keer dat ze toegang krijgen tot de toepassing met een gebruikersnaam en wachtwoord op. Na de eerste aanmelding levert Azure AD de gebruikersnaam en het wachtwoord voor de toepassing. Verificatie wordt in dit scenario wordt verwerkt door Azure AD.
* [**SAML-verificatie**](application-proxy-configure-single-sign-on-on-premises-apps.md). SAML gebaseerde eenmalige aanmelding wordt ondersteund voor toepassingen die gebruikmaken van SAML 2.0 of WS-Federation-protocollen. Met SAML eenmalige aanmelding verifieert Azure AD met behulp van Azure AD-account van de gebruiker naar de toepassing.

Zie voor meer informatie over de ondersteunde methoden [kiezen van een methode voor eenmalige aanmelding](what-is-single-sign-on.md#choosing-a-single-sign-on-method).

### <a name="security-benefits"></a>De voordelen van beveiliging

De oplossing voor externe toegang die worden aangeboden door Application Proxy en Azure AD ondersteunen verschillende beveiligingsvoordelen klanten kunnen profiteren van, met inbegrip van:

* **Geverifieerde toegang**. Application Proxy is het meest geschikt is voor het publiceren van toepassingen met [pre-authenticatie](application-proxy-security.md#authenticated-access) om ervoor te zorgen dat alleen geverifieerde verbindingen uw netwerk bereikt. Voor toepassingen die zijn gepubliceerd met vooraf-verificatie, is geen verkeer toegestaan worden doorgegeven via de App Proxy-service aan uw on-premises omgeving, zonder een geldig token. Vooraf-verificatie, blokkeert door de aard, een groot aantal gerichte aanvallen, zoals alleen geverifieerde identiteiten toegang de back endtoepassing tot hebben.
* **Voorwaardelijke toegang**. Besturingselementen voor uitgebreidere beleid kunnen worden toegepast voordat verbindingen met uw netwerk tot stand worden gebracht. Met voorwaardelijke toegang, kunt u beperkingen kunt definiëren op het verkeer waarmee u uw back-endtoepassing bereikt. U maken beleidsregels die aanmeldingen op basis van locatie, sterkte van verificatie- en gebruikersprofiel van het risico te beperken. Als voorwaardelijke toegang zich verder ontwikkelt, worden meer besturingselementen toegevoegd om te voorzien in aanvullende beveiliging zoals integratie met Microsoft Cloud App Security (MCAS). De MCAS-integratie kunt u het configureren van een on-premises toepassing voor [realtime-controle](application-proxy-integrate-with-microsoft-cloud-application-security.md) door gebruik te maken van voorwaardelijke toegang bewaken en beheren sessies in realtime op basis van beleid voor voorwaardelijke toegang.
* **Verkeer van beëindiging**. Al het verkeer naar de back-endtoepassing wordt beëindigd bij de service voor toepassingsproxy in de cloud terwijl de sessie opnieuw wordt ingesteld met de back-endserver. Deze strategie verbinding betekent dat uw back-end servers zijn niet beschikbaar in de HTTP-verkeer. Ze zijn beter beschermd tegen aanvallen van gerichte DoS (denial-of-service), omdat de firewall niet bij een aanval.
* **Alle toegang is uitgaand**. De Application Proxy connectors alleen uitgaande verbindingen naar de service voor toepassingsproxy in de cloud gebruiken via de poorten 80 en 443. Met geen binnenkomende verbindingen is er niet nodig om te openen van firewall-poorten voor binnenkomende verbindingen of onderdelen in het Perimeternetwerk. Alle verbindingen zijn uitgaand en via een beveiligd kanaal.
* **Beveiliging-analyse en Machine Learning (ML) op basis van intelligence**. Omdat deze deel uitmaakt van Azure Active Directory, Application Proxy kan gebruikmaken van [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/identity-protection/overview) (vereist [Premium P2-licentieverlening](https://azure.microsoft.com/pricing/details/active-directory/)). Azure AD Identity Protection beveiligingsinformatie voor machine learning combineert met gegevensfeeds van Microsoft van [Digital Crimes Unit](https://news.microsoft.com/stories/cybercrime/index.html) en [Microsoft Security Response Center](https://www.microsoft.com/msrc) proactief identificeren accounts waarmee is geknoeid. Identity Protection biedt realtime-beveiliging van riskante aanmeldingen. Het duurt in overweging factoren zoals toegang vanaf geïnfecteerde apparaten, via anoniem netwerken, of vanuit locaties ongewone en waarschijnlijk niet aan het verhogen van het risicoprofiel van een sessie. Dit risicoprofiel wordt gebruikt voor realtime-beveiliging. Veel van deze rapporten en gebeurtenissen zijn al beschikbaar via een API voor integratie met uw SIEM-systemen.

* **Externe toegang als een service**. U hebt geen zorgen te hoeven maken over het onderhoud en patching uit handen on-premises servers voor externe toegang. Application Proxy is een service voor het schalen van internet die eigendom zijn van Microsoft, zodat u altijd de meest recente beveiligingspatches en upgrades. Nog steeds niet-gepatchte software-accounts voor een groot aantal aanvallen. Op basis van het Ministerie van binnenlandse beveiliging, meerdere [85 procent van gerichte aanvallen worden voorkomen](https://www.us-cert.gov/ncas/alerts/TA15-119A). Met deze servicemodel hoeft u de zware last van het beheer van uw edge-servers niet meer en coderen voor het vullen van deze indien nodig.

* **Intune-integratie**. Met Intune, zakelijke verkeer afzonderlijk gerouteerd van persoonlijke verkeer. Application Proxy zorgt ervoor dat de zakelijke verkeer wordt geverifieerd. [Application Proxy en de Intune Managed Browser](https://docs.microsoft.com/intune/app-configuration-managed-browser#how-to-configure-application-proxy-settings-for-protected-browsers) mogelijkheid kan ook samen zodat externe gebruikers veilig toegang krijgen tot interne websites vanaf iOS- en Android-apparaten worden gebruikt.

### <a name="roadmap-to-the-cloud"></a>Roadmap naar de cloud

Een ander groot voordeel van de implementatie van de toepassingsproxy is Azure AD naar uw on-premises-omgeving uitbreiden. Implementatie van App-Proxy is in feite een belangrijke stap in uw organisatie en de apps verplaatsen naar de cloud. Door te verplaatsen naar de cloud en er vandaan lopen on-premises verificatie, verminderen de footprint van uw on-premises en mogelijkheden voor identiteitsbeheer van Azure AD gebruiken als uw vlak van het besturingselement. Met minimale of geen updates aan bestaande toepassingen, hebt u toegang tot de mogelijkheden, zoals eenmalige aanmelding, meervoudige verificatie en Centraal beheer van de cloud. De vereiste onderdelen installeren op App-Proxy is een eenvoudig proces voor het opzetten van een RAS-framework. En door te verplaatsen naar de cloud, die u hebt toegang tot de meest recente Azure AD-functies, updates en functionaliteit, zoals hoge beschikbaarheid en herstel na noodgevallen.

Zie voor meer informatie over het migreren van uw apps naar Azure AD, de [uw toepassingen migreren naar Azure Active Directory](https://aka.ms/migrateapps/whitepaper) technisch document.

## <a name="architecture"></a>Architectuur

Het volgende diagram wordt geïllustreerd in het algemeen hoe Azure AD-verificatie-services en Application Proxy-werken samen om te bieden single sign-on bij on-premises toepassingen voor eindgebruikers.

![Azure AD-toepassingsproxy-verificatiestroom](media/what-is-application-proxy/azure-ad-application-proxy-authentication-flow.png)

1. Nadat de gebruiker toegang gekregen de toepassing via een eindpunt tot heeft, wordt de gebruiker omgeleid naar de aanmeldingspagina van Azure AD. Als u beleid voor voorwaardelijke toegang hebt geconfigureerd, worden bepaalde voorwaarden gecontroleerd op dit moment om ervoor te zorgen dat u aan de beveiligingsvereisten van uw organisatie voldoet.
2. Azure AD wordt na een geslaagde aanmelding, een token naar de client-apparaat van de gebruiker verzonden.
3. De client stuurt het token naar de Application Proxy-service, die de user principal name (UPN) en de naam van de beveiligingsprincipal (SPN) opgehaald uit het token.
4. Application Proxy stuurt de aanvraag, die is opgehaald door de toepassingsproxy [connector](application-proxy-connectors.md).
5. De connector voert aanvullende verificatie vereist namens de gebruiker (*optioneel, afhankelijk van de methode voor netwerkverificatie*), vraagt het interne eindpunt van de toepassingsserver en verzendt de aanvraag naar de on-premises de toepassing.
6. Het antwoord van de toepassingsserver wordt verzonden via de connector met de Application Proxy-service.
7. Het antwoord wordt verzonden vanaf de Application Proxy-service voor de gebruiker.

|**Onderdeel**|**Beschrijving**|
|:-|:-|
|Eindpunt|Het eindpunt is een URL of een [eindgebruikersportal](end-user-experiences.md). Gebruikers kunnen toepassingen terwijl buiten uw netwerk bereiken door het openen van een externe URL. Gebruikers in uw netwerk kunnen de toepassing openen via een URL of een eindgebruikersportal. Wanneer gebruikers naar een van deze eindpunten gaat, worden ze verifiëren in Azure AD en vervolgens worden gerouteerd via de connector op de on-premises toepassing.|
|Azure AD|Azure AD voert de verificatie met behulp van de tenant-directory die zijn opgeslagen in de cloud.|
|Application Proxy-service|Deze Application Proxy-service wordt uitgevoerd in de cloud als onderdeel van Azure AD. Deze geeft de token van de aanmelding van de gebruiker aan de Connector voor toepassingsproxy. Application Proxy stuurt een toegankelijk headers van de aanvraag en de headers aan de hand van het protocol ingesteld op de client-IP-adres. Als de inkomende aanvraag naar de proxy al die header heeft, wordt het client-IP-adres toegevoegd aan het einde van de door komma's gescheiden lijst die de waarde van de header.|
|Connector voor toepassingsproxy|De connector is een lichtgewicht-agent die wordt uitgevoerd op een Windows-Server in uw netwerk. De connector beheert de communicatie tussen de service voor toepassingsproxy in de cloud en de on-premises toepassing. Uitgaande verbindingen maakt alleen gebruik van de connector, zodat u hoeft inkomende poorten openen of iets in de DMZ te plaatsen. De connectors zijn staatloos en ophalen van informatie vanuit de cloud naar behoefte. Voor meer informatie over connectors, zoals hoe ze verdelen en geverifieerd, Zie [over Azure AD Application Proxy connectors](application-proxy-connectors.md).|
|Active Directory (AD)|Active Directory wordt on-premises, voor verificatie uitvoeren voor domeinaccounts uitgevoerd. Wanneer eenmalige aanmelding is geconfigureerd, wordt de connector communiceert met AD om uit te voeren van aanvullende verificatie vereist.|
|On-premises toepassing|Ten slotte heeft de gebruiker toegang tot een on-premises toepassing.|

Azure AD-toepassingsproxy bestaat uit de cloud gebaseerde service voor toepassingsproxy en een on-premises connector. De connector luistert naar aanvragen van de service voor toepassingsproxy en verbindingen met de interne toepassingen worden verwerkt. Het is belangrijk te weten dat alle communicatie vindt plaats via SSL en altijd afkomstig van de connector met de Application Proxy-service. De communicatie is dat wil zeggen, alleen uitgaande. De connector gebruikt een certificaat voor verificatie bij de Application Proxy-service voor alle aanroepen. De enige uitzondering op de beveiliging van de verbinding is de eerste configuratie stap waarbij het clientcertificaat tot stand is gebracht. Zie de Application Proxy [achter de schermen](application-proxy-security.md#under-the-hood) voor meer informatie.

### <a name="application-proxy-connectors"></a>Application Proxy Connectors

[Toepassingsproxyconnectors](application-proxy-connectors.md) lichtgewicht geïmplementeerde agents on-premises die het eenvoudiger maken van de uitgaande verbinding met de service voor toepassingsproxy in de cloud. De connectors moeten worden geïnstalleerd op een Windows-Server die toegang tot de back endtoepassing heeft. Gebruikers verbinding maken met de cloudservice-App-Proxy die routes die hun verkeer naar de apps via de connectors als hieronder weergegeven.

![Azure AD-toepassingsproxy-netwerkverbindingen](media/what-is-application-proxy/azure-ad-application-proxy-network-connections.png)

Installatie en registratie tussen een connector en de App Proxy-service wordt als volgt uitgevoerd:

1. De IT-beheerder poorten 80 en 443 voor uitgaand verkeer wordt geopend en geeft toegang tot verschillende URL's die nodig zijn voor de connector, de App Proxy-service en Azure AD.
2. De beheerder zich aanmeldt bij de Azure-portal en een uitvoerbaar bestand voor het installeren van de connector op een on-premises Windows server wordt uitgevoerd.
3. De connector wordt gestart ' luisteren ' naar de App Proxy-service.
4. De beheerder voegt de on-premises toepassing naar Azure AD en configureert de instellingen zoals de gebruikers van de URL's moeten verbinding maken met hun apps.

Zie voor meer informatie, [een Azure AD Application Proxy-implementatie plannen](application-proxy-deployment-plan.md).

Het verdient aanbeveling dat u altijd meerdere connectors voor redundantie en schaal implementeert. De connectors, in combinatie met de service, kunnen Let erop dat van alle taken die hoge beschikbaarheid en worden toegevoegd of verwijderd dynamisch. Telkens wanneer die een nieuwe aanvraag binnenkomt wordt deze doorgestuurd naar een van de connectors die beschikbaar is. Wanneer een connector wordt uitgevoerd, blijft deze actief als deze verbinding met de service maakt. Als u een connector tijdelijk niet beschikbaar is, heeft niet deze dit verkeer reageren. Niet-gebruikte connectors zijn gemarkeerd als inactief en verwijderd na tien dagen van inactiviteit.

Connectors pollen ook de server als u wilt weten als er een nieuwere versie van de connector is. Hoewel u een handmatige update doen kunt, wordt automatisch connectors bijgewerkt, zolang de Application Proxy Connector Updater-service wordt uitgevoerd. Voor tenants met meerdere connectors gericht op de automatische updates één connector op een tijdstip in elke groep om te voorkomen dat downtime in uw omgeving.

**Opmerking**: U kunt de toepassingsproxy bewaken [versie geschiedenispagina](application-proxy-release-version-history.md) wilt worden gewaarschuwd als er updates zijn uitgebracht met een abonnement op de RSS-feed.

Elke Application Proxy-connector is toegewezen aan een [connectorgroep](application-proxy-connector-groups.md). Connectors in dezelfde groep verbindingslijn fungeren als één eenheid voor hoge beschikbaarheid en taakverdeling. U kunt nieuwe groepen maken, connectors aan ze toewijzen in Azure portal en toewijzen van specifieke connectors voor het bieden van specifieke toepassingen. Het is aanbevolen dat ten minste twee connectors in elke connectorgroep voor hoge beschikbaarheid.

Connectorgroepen kunnen nuttig zijn wanneer u nodig hebt ter ondersteuning van de volgende scenario's:

* Geografische app publiceren
* Toepassingsisolatie segmentatie
* Web-apps die worden uitgevoerd in de cloud of on-premises publiceren

Voor meer informatie over het kiezen waar u uw connectors installeren en optimaliseren van uw netwerk, Zie [topologie overwegingen met betrekking tot het netwerk bij het gebruik van Azure Active Directory-toepassingsproxy](application-proxy-network-topology.md).

## <a name="other-use-cases"></a>Andere use-cases

Tot nu toe hebben we ons gericht op toepassingsproxy gebruiken voor het publiceren van on-premises toepassingen extern tijdens het inschakelen van single sign-on bij al uw cloud en on-premises apps. Er zijn echter andere gebruiksvoorbeelden voor App-Proxy die opgemerkt worden. Ze omvatten:

* **REST-API's veilig te publiceren**. Als u bedrijfslogica of API's met on-premises of gehost op virtuele machines in de cloud, biedt Application Proxy een openbaar eindpunt voor API-toegang. API-eindpunt toegang kunt u voor verificatie en autorisatie zonder inkomende poorten. Het biedt extra beveiliging via Azure AD Premium-functies, zoals meervoudige verificatie en voorwaardelijke toegang op basis van een apparaat voor desktops, iOS, MAC en Android-apparaten met Intune. Zie voor meer informatie, [native client-toepassingen om te communiceren met de proxy-toepassingen inschakelen](application-proxy-configure-native-client-application.md) en [een API beveiligen met behulp van OAuth 2.0 met Azure Active Directory en API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad).
* **Extern bureaublad-Services** **(RDS)** . Standard extern bureaublad-services-implementaties vereisen open binnenkomende verbindingen. Echter, de [RDS-implementatie met Application Proxy](application-proxy-integrate-with-remote-desktop-services.md) heeft een permanente uitgaande verbinding van de server waarop de connectorservice wordt uitgevoerd. Op deze manier kunt u meer toepassingen voor eindgebruikers door publishing on-premises toepassingen via Extern bureaublad-Services aanbieden. U kunt ook de kwetsbaarheid voor aanvallen van de implementatie met een beperkte set van verificatie in twee stappen en besturingselementen voor voorwaardelijke toegang voor RDS. beperken
* **Toepassingen publiceren die verbinding maken met behulp van WebSockets**. Ondersteuning voor met [Qlik Sense](application-proxy-qlik.md) is in openbare Preview en zal worden uitgebreid naar andere apps in de toekomst.
* **Systeemeigen client-toepassingen om te communiceren met de proxy-toepassingen inschakelen**. U kunt Azure AD-toepassingsproxy gebruiken voor web-apps publiceren, maar het kan ook worden gebruikt om te publiceren [native clienttoepassingen](application-proxy-configure-native-client-application.md) die zijn geconfigureerd met de Azure AD Authentication Library (ADAL). Native clienttoepassingen verschillen van web-apps omdat ze zijn geïnstalleerd op een apparaat, terwijl de web-apps worden geopend via een browser.

## <a name="conclusion"></a>Conclusie

De manier waarop we werken en de hulpprogramma's we gebruiken er veranderen nog. Met meer werknemers hun eigen apparaten meebrengen om te werken en het verbreid gebruik van Software-as-a-Service (SaaS)-toepassingen, de manier waarop organisaties beheren en veilig hun gegevens moet ook veranderen. Bedrijven werken niet meer uitsluitend binnen hun eigen muren, beveiligd door een moat rond de rand. Gegevens verzonden naar meerdere locaties dan ooit tevoren--over meerdere on-premises en cloudomgevingen. Deze evolutie heeft geholpen Verhoog de productiviteit en de mogelijkheid om samen te werken gebruikers, maar ook wordt beveiliging van gevoelige gegevens moeilijker.

Of u momenteel Azure AD gebruikt voor het beheren van gebruikers in een co-existentie hybride scenario of geïnteresseerd bent in het begin uw reis naar de cloud, de implementatie van Azure AD Application Proxy kan helpen Reduceer de grootte van uw on-premises footprint door externe toegang tot als een service.

Organisaties moeten beginnen met het gebruik van App-Proxy vandaag nog om te profiteren van de volgende voordelen:

* Extern publiceren van on-premises toepassingen zonder de overhead die is gekoppeld aan het onderhouden van traditionele VPN- of andere on-premises web-oplossingen en DMZ-benadering publiceren
* Eenmalige aanmelding voor alle toepassingen, worden ze Office 365 of andere SaaS-apps en met inbegrip van on-premises toepassingen
* Cloudbeveiliging van schaal waar Azure AD maakt gebruik van Office 365-telemetrie om te voorkomen dat onbevoegde toegang
* Integratie van Intune om ervoor te zorgen zakelijke verkeer wordt geverifieerd
* Gecentraliseerd van Gebruikersaccountbeheer
* Automatische updates om ervoor te zorgen dat u hebt de meest recente beveiligingspatches
* Nieuwe functies zoals ze zijn vrijgegeven; de meest recente wordt ondersteuning voor SAML eenmalige aanmelding en meer gedetailleerd beheer van cookies van de toepassing

## <a name="next-steps"></a>Volgende stappen

* Zie voor informatie over het plannen van besturingssysteem en het beheren van Azure AD-toepassingsproxy, [een Azure AD Application Proxy-implementatie plannen](application-proxy-deployment-plan.md).
* Zie voor het plannen van een live demo en een gratis proefversie van 90 dagen voor de evaluatie, [aan de slag met Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-trial).
