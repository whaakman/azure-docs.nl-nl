---
title: Een Azure Active Directory Application Proxy-implementatie plannen
description: Een end-to-end-handleiding voor het plannen van de implementatie van de toepassingsproxy binnen uw organisatie
services: active-directory
documentationcenter: azure
author: barbaraselden
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: baselden
ms.reviewer: ''
ms.openlocfilehash: 04a2e9968e8716818637a34adea86de88e1f848c
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388311"
---
# <a name="plan-an-azure-ad-application-proxy-deployment"></a>Een Azure AD Application Proxy-implementatie plannen

Toepassingsproxy van Azure Active Directory (Azure AD) is een veilige en voordelige RAS-oplossing voor on-premises toepassingen. Het biedt een pad direct overgang voor 'Cloud First' organisaties voor het beheren van toegang tot bestaande on-premises toepassingen die nog niet kan worden van het gebruik van moderne protocollen. Zie voor aanvullende inleidende informatie [wat is Application Proxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy).

De toepassingsproxy wordt aanbevolen voor externe gebruikers toegang geven tot interne bronnen. Toepassingsproxy vervangt de noodzaak van een VPN of een omgekeerde proxy voor deze gebruiksvoorbeelden voor externe toegang. Het is niet bedoeld voor gebruikers die zich op het bedrijfsnetwerk bevinden. Deze gebruikers die de Application Proxy voor toegang tot het intranet gebruiken kunnen ongewenste prestatieproblemen ondervinden.

Dit artikel bevat de resources die u wilt plannen, te gebruiken en beheren van Azure AD-toepassingsproxy. 

## <a name="plan-your-implementation"></a>Uw implementatie plannen

De volgende sectie biedt een brede weergave van de sleutel voor het plannen van elementen die u voor een efficiënte implementatie-ervaring wordt ingesteld. 

### <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u begint met uw implementatie. Ziet u meer informatie over het instellen van uw omgeving, met inbegrip van deze vereisten in deze [zelfstudie](application-proxy-add-on-premises-application.md).

* **Connectors**: Connectors zijn lichtgewicht agents die u naar implementeren kunt:
   * Lokale fysieke hardware
   * Een virtuele machine die worden gehost in een hypervisor-oplossing
   * Een virtuele machine die wordt gehost in Azure om in te schakelen uitgaande verbinding met de Application Proxy-service.

* Zie [inzicht in Azure AD App Proxy Connectors](application-proxy-connectors.md) voor een gedetailleerder overzicht.

     * Connector machines moet [worden ingeschakeld voor TLS 1.2](application-proxy-add-on-premises-application.md) voordat u de connectors installeert.

     * Indien mogelijk implementeren connectors in de [hetzelfde netwerk](application-proxy-network-topology.md) en segment als de back-end web-toepassingsservers. Het is raadzaam om te implementeren connectors na het voltooien van een detectie van toepassingen.
     * U wordt aangeraden dat elke connectorgroep ten minste twee connectors heeft voor hoge beschikbaarheid en schaal. Met drie connectors is optimale geval moet u mogelijk een virtuele machine op elk gewenst moment-service. Controleer de [gegevenstabel voor capaciteit connector](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connectors#capacity-planning) om te helpen bij het bepalen van welk type machine om connectors te installeren op. Hoe groter de machine de meer buffer en goed presterende is de connector.

* **Instellingen voor toegang tot het netwerk**: Azure AD Application Proxy connectors [verbinding maken met Azure via HTTPS (TCP-poort 443) en HTTP (TCP-poort 80)](application-proxy-add-on-premises-application.md). 

   * Afsluitende connector TLS-verkeer wordt niet ondersteund en wordt voorkomen dat connectors tot stand brengen van een beveiligd kanaal met hun respectieve toepassingsproxy van Azure-eindpunten.

   * Voorkomen dat alle vormen van inline inspectie op uitgaande TLS-communicatie tussen connectors en Azure. Interne controle tussen een connector en back-end-toepassingen is mogelijk, maar de gebruikerservaring kan afnemen en als zodanig wordt niet aanbevolen.

   * De taakverdeling van de connectors zelf wordt ook niet ondersteund, of zelfs nodig.

### <a name="important-considerations-before-configuring-azure-ad-application-proxy"></a>Belangrijke overwegingen voor het configureren van Azure AD-toepassingsproxy

De volgende belangrijke vereisten moeten worden voldaan om te configureren en implementeren van Azure AD-toepassingsproxy.

*  **Azure onboarding**: Voordat u implementeert toepassingsproxy, moeten gebruikers-id's worden gesynchroniseerd vanuit een on-premises directory of die rechtstreeks in uw Azure AD-tenants zijn gemaakt. Identiteitssynchronisatie kunt vooraf om gebruikers te verifiëren voordat ze toegang verlenen tot de App Proxy toepassingen gepubliceerde en dat de id-gegevens van de gebruiker nodig om uit te voeren eenmalige aanmelding (SSO) met Azure AD.

* **Vereisten voor voorwaardelijke toegang**: We raden niet toepassingsproxy gebruiken voor toegang tot het intranet omdat deze wordt toegevoegd latentie die is van invloed op gebruikers. U wordt aangeraden met Application Proxy met vooraf-verificatie en voorwaardelijke toegangsbeleid voor externe toegang via internet.  Een benadering voor voorwaardelijke toegang voor gebruik van het intranet is het moderniseren van toepassingen zodat ze diretly kunnen worden geverifieerd met AAD. Raadpleeg [Resources voor het migreren van toepassingen voor AAD](https://docs.microsoft.com/azure/active-directory/manage-apps/migration-resources) voor meer informatie. 

* **Servicelimieten**: Ter bescherming tegen zijn overmatig van resources door er afzonderlijke tenants bandbreedtebeperking limieten instellen per toepassing en tenant. Om te zien van deze limieten die verwijzen naar [limieten en beperkingen voor Azure AD-service](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-service-limits-restrictions). Deze beperkingslimieten zijn gebaseerd op een benchmark tot nu toe boven de typische gebruiksvolume en ruime buffer biedt voor een meerderheid van implementaties.

* **Openbaar certificaat**: Als u aangepaste domeinnamen gebruikt, moet u een openbaar certificaat dat is uitgegeven door een vertrouwde certificeringsinstantie van niet-Microsoft aanschaffen. Afhankelijk van de behoeften van uw organisatie aan een certificaat kan enige tijd duren en het is raadzaam dat begint met het proces zo vroeg mogelijk. Azure Application Proxy biedt ondersteuning voor standaard, [jokertekens](application-proxy-wildcard.md), of op basis van SAN-certificaten.

* **Vereisten voor het domein**: Single sign-on bij uw gepubliceerde toepassingen met behulp van Kerberos-beperkte delegatie (KCD) is vereist dat de host van een connector domein hetzelfde AD-domein als de toepassingen die wordt gepubliceerd. Zie voor gedetailleerde informatie over het onderwerp [KCD voor eenmalige aanmelding](application-proxy-configure-single-sign-on-with-kcd.md) met Application Proxy. De connectorservice wordt uitgevoerd in de context van het lokale systeem en mogen niet worden geconfigureerd voor het gebruik van een aangepaste identiteit.

* **DNS-records voor URL 's**

   * Voordat u met behulp van aangepaste domeinen in Application Proxy moet u een CNAME-record maken in de openbare DNS, zodat clients kunnen omzetten van de aangepaste gedefinieerde externe URL naar de vooraf gedefinieerde Application Proxy-adres. Failover-overschakeling naar het maken van een CNAME-record voor een toepassing die gebruikmaakt van een aangepast domein wordt voorkomen dat externe gebruikers verbinding maken met de toepassing. Stappen die nodig zijn om toe te voegen CNAME-records kunnen verschillen van DNS-provider tot provider, zodat informatie over hoe u [DNS-records en -recordsets beheren met behulp van de Azure-portal](https://docs.microsoft.com/azure/dns/dns-operations-recordsets-portal).

   * Connector-hosts moeten op dezelfde manier kunnen omzetten van de interne URL van de toepassingen die worden gepubliceerd.

* **Beheerdersrechten en -rollen**

   * **Installatie van connector** moet lokale beheerdersrechten voor de Windows-server die wordt geïnstalleerd op. Het is ook vereist een minimum van een *toepassingsbeheerder* rol te verifiëren en het exemplaar van de connector voor uw Azure AD-tenant te registreren. 

   * **Toepassingen publiceren en beheer** vereisen de *toepassingsbeheerder* rol. Beheerders kunnen alle toepassingen in de map inclusief registraties, SSO-instellingen, gebruiker en toewijzingen van groepen en licentieverlening, Application Proxy-instellingen en toestemming beheren. Het verlenen niet de mogelijkheid voor het beheren van voorwaardelijke toegang. De *beheerder van de Cloudtoepassing* rol heeft alle mogelijkheden van de beheerder van de toepassing, behalve dat het beheer van de Application Proxy-instellingen niet toelaat.

* **Licentieverlening**: Application Proxy is beschikbaar via de Azure AD Basic-abonnement. Raadpleeg de [prijzen Azure Active Directory-pagina](https://azure.microsoft.com/pricing/details/active-directory/) voor een volledige lijst van licenties en -functies.  

### <a name="application-discovery"></a>Toepassingsdetectie

Een telling van alle binnen de regeling vallen toepassingen die worden gepubliceerd via toepassingsproxy door het verzamelen van de volgende informatie:

| Informatietype| Gegevens moeten worden verzameld |
|---|---|
| Servicetype| Bijvoorbeeld: SharePoint, SAP, CRM, aangepaste Web-App, -API |
| Platform voor toepassingen | Bijvoorbeeld: Windows IIS, Apache on Linux, Tomcat, NGINX |
| Lidmaatschap van domein| Webserver en de volledig gekwalificeerde domeinnaam (FQDN) |
| Locatie van de toepassing | Waar de webserver of farm bevindt zich in uw infrastructuur |
| Interne toegang | De exacte URL gebruikt bij het openen van de toepassing intern. <br> Als een farm, welk type load balancing wordt gebruikt? <br> De toepassing tekent of inhoud van bronnen dan zichzelf.<br> Bepaal als de toepassing via WebSockets werkt. |
| Externe toegang | De leveranciersoplossing die de toepassing wordt al blootgesteld aan extern. <br> De URL die u wilt gebruiken voor externe toegang. Als SharePoint, zorg ervoor dat alternatieve toewijzingen voor toegang worden geconfigureerd [deze richtlijnen](https://docs.microsoft.com/SharePoint/administration/configure-alternate-access-mappings). Als dat niet het geval is, moet u voor het definiëren van de externe URL's. |
| Openbaar certificaat | Als u een aangepast domein, aanschaffen een certificaat met een overeenkomende naam voor onderwerp. Houd er rekening mee het serienummer en de locatie waar het kan worden verkregen als een certificaat bestaat. |
| Verificatietype| Het type van de authenticatie wordt ondersteund door de ondersteuning voor toepassingen zoals Basic, Windows-integratie verificatie op basis van formulieren, op basis van koptekst en claims. <br>Als de toepassing is geconfigureerd om te worden uitgevoerd onder een specifieke domeinaccount, houd er rekening mee de volledig domein naam (Fully Qualified Domain Name) van het serviceaccount.<br> Als op basis van SAML-id en antwoord-URL's. <br> Als op basis van koptekst van de leveranciersoplossing en de specifieke vereisten voor het afhandelen van verificatie-typen. |
| Naam van connector | De logische naam voor de groep van connectors die wordt aangewezen voor het kanaal en eenmalige aanmelding voor deze back endtoepassing. |
| Gebruikers/groepen toegang | De gebruikers of gebruikersgroepen die externe toegang tot de toepassing wordt verleend. |
| Aanvullende vereisten | Houd er rekening mee eventuele aanvullende externe toegang of de beveiligingsvereisten die u moeten rekening worden gehouden bij het publiceren van de toepassing. |

U kunt dit downloaden [toepassing inventariswerkblad](https://aka.ms/appdiscovery) het inventariseren van uw apps.

### <a name="define-organizational-requirements"></a>Organisatorische vereisten definiëren

Hieronder vindt u gebieden waarvoor u de bedrijfsbehoeften van uw organisatie moet definiëren. Elk gebied bevat voorbeelden van vereisten

 **Toegang**

* Externe gebruikers met een domein of Azure AD gekoppelde apparaten gebruikers hebben toegang tot gepubliceerde toepassingen veilig met naadloze eenmalige aanmelding (SSO).

* Externe gebruikers met goedgekeurde persoonlijke apparaten kunnen veilig toegang krijgen tot gepubliceerde toepassingen mits ze zijn ingeschreven bij MFA en de Microsoft Authenticator-app op hun mobiele telefoon hebt geregistreerd als een verificatiemethode.

**Governance** 

* Beheerders kunnen definiëren en bewaken van de levenscyclus van de gebruikertoewijzingen aan toepassingen die zijn gepubliceerd via toepassingsproxy.

**Beveiliging**

* Alleen gebruikers toegewezen aan toepassingen via het lidmaatschap van of afzonderlijk hebben toegang tot deze toepassingen.

**Prestaties**

* Er is geen verslechtering van toepassingsprestaties in vergelijking met toegang tot de toepassing van het interne netwerk.

**Gebruikerservaring**

* Gebruikers zich bewust bent van hoe u toegang tot hun toepassingen met behulp van bekende bedrijf URL's voor elk apparaatplatform.

**Controle**
* Beheerders kunnen toegang gebruikersactiviteiten.


### <a name="best-practices-for-a-pilot"></a>Aanbevolen procedures voor een proef

Bepaalt de hoeveelheid tijd en moeite die nodig zijn voor volledig Commissie één toepassing voor externe toegang met eenmalige aanmelding (SSO). Dit doen door het uitvoeren van een leider die rekening gehouden met de initiële detectie, publiceren en algemene testen. Met behulp van een eenvoudige op basis van een IIS-web-App die al is vooraf geconfigureerd voor geïntegreerde Windows-verificatie wilt helpen basislijn, als deze instelling minimale inspanning voor is pilot externe toegang en SSO vereist.

De volgende ontwerpelementen moeten het succes van uw implementatie test rechtstreeks in een productietenant te verhogen.  

**Beheer van de connector**:  

* Connectors spelen een belangrijke rol bij het verstrekken van de on-premises-kanaal aan uw toepassingen. Met behulp van de **standaard** connectorgroep is voldoende voor de eerste test testen van gepubliceerde toepassingen voordat ze in productie. Is getest toepassingen kunnen vervolgens worden verplaatst naar productie connectorgroepen.

**Toepassingsbeheer**:

* Uw werknemers in staat is waarschijnlijk te onthouden van dat een externe URL wordt vertrouwd en relevant zijn. Vermijd het publiceren van uw toepassing met behulp van onze vooraf gedefinieerde msappproxy.net of onmicrosoft.com-achtervoegsels. In plaats daarvan bieden een vertrouwd op het hoogste niveau geverifieerd domein, zoals voorafgegaan door een logische hostnaam *intranet. < customers_domain > .com*.

* Zichtbaarheid van de pilot toepassingspictogram voor een testfasegroep beperken door het verbergen van de vorm van starten pictogram van de Azure-MyApps-portal. Wanneer u klaar bent voor productie kunt u het bereik van de app aan de respectieve doelgroep, in dezelfde tenant Pre-productie, of door het publiceren van de toepassing ook in uw productietenant.

**Eenmalige aanmelding instellingen**: Sommige instellingen voor eenmalige aanmelding hebben specifieke afhankelijkheden die het even duren kunnen te stellen, dus Vermijd wijzigingsbeheer vertragingen door ervoor te zorgen dat afhankelijkheden vooraf worden behandeld. Dit omvat connector hosts om uit te voeren van eenmalige aanmelding met behulp van Kerberos-beperkte delegatie (KCD) en zorg ervoor dat u van andere tijdrovende activiteiten voor domeindeelname. Bijvoorbeeld, instellen van een PING-toegang-exemplaar als koptekst gebaseerde SSO hoeven.

**SSL tussen Connector Host en de doeltoepassing**: Beveiliging is cruciaal, zodat TLS tussen de connector-toepassingen voor host en het doel moet altijd worden gebruikt. Met name als de web-App is geconfigureerd voor verificatie op basis van formulieren (FBA), zoals gebruikersreferenties worden vervolgens effectief in niet-versleutelde tekst verzonden.

**Incrementeel implementeren en testen van elke stap**. Uitvoeren van elementaire functionele tests na het publiceren van een toepassing om ervoor te zorgen dat alle gebruikers- en zakelijke vereisten wordt voldaan door de onderstaande instructies te volgen:

1. Testen en valideren van algemene toegang tot de web-App met vooraf-verificatie uitgeschakeld.
2. Als dit lukt inschakelen vooraf-verificatie en toewijzen van gebruikers en groepen. Testen en valideren van toegang.
3. Vervolgens voegt u de methode voor eenmalige aanmelding voor uw toepassing en test opnieuw voor het valideren van toegang.
4. Voorwaardelijke toegang en de MFA-beleid als vereiste toepassen. Testen en valideren van toegang.

**Hulpprogramma's voor probleemoplossing**: Als het oplossen van problemen altijd starten door het valideren van toegang tot de gepubliceerde toepassing vanuit de browser op de host van de connector en bevestigt u dat de toepassing werkt zoals verwacht. De eenvoudiger uw instellingen, het gemakkelijker om te bepalen van de hoofdoorzaak te achterhalen, dus houd rekening met probeert te reproduceren van problemen met een minimale configuratie zoals het gebruik van slechts één connector en er is geen eenmalige aanmelding. In sommige gevallen kan Webfoutopsporing hulpprogramma's, zoals van Telerik Fiddler onmisbaar om toegang of inhoud problemen in toepassingen die worden geopend via een proxy te bewijzen. Fiddler kan ook fungeren als een proxy te traceren en foutopsporing kunt uitvoeren voor mobiele platforms zoals iOS en Android verkeer en vrijwel alles dat kan worden geconfigureerd om te worden gerouteerd via een proxy. Zie de [problemen oplossen met](application-proxy-troubleshoot.md) voor meer informatie.

## <a name="implement-your-solution"></a>Uw oplossing implementeren

### <a name="deploy-application-proxy"></a>Toepassingsproxy implementeren

De stappen voor het implementeren van de toepassingsproxy worden behandeld in deze [zelfstudie voor het toevoegen van een on-premises toepassing voor externe toegang](application-proxy-add-on-premises-application.md). Als de installatie niet met succes is voltooid, selecteert u **Troubleshoot Application Proxy** in de portal of gebruik de gids voor probleemoplossing [voor problemen met het installeren van de Toepassingsproxyagent](application-proxy-connector-installation-problem.md).

### <a name="publish-applications-via-application-proxy"></a>Publiceren van toepassingen via de toepassingsproxy

Publiceren van toepassingen wordt ervan uitgegaan dat u aan alle vereisten hebt voldaan en dat er verschillende connectors weergegeven als geregistreerd en actief op de pagina Application Proxy.

U kunt ook toepassingen publiceren met behulp van [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview).

Hieronder volgen enkele aanbevolen procedures te volgen bij het publiceren van een toepassing:

* **Connectorgroepen gebruiken**: Een connectorgroep die is aangewezen voor het publiceren van de desbetreffende toepassing toewijzen. U wordt aangeraden dat elke connectorgroep ten minste twee connectors heeft voor hoge beschikbaarheid en schaal. Met drie connectors is optimale geval moet u mogelijk een virtuele machine op elk gewenst moment-service. Zie ook [publiceren van toepassingen op afzonderlijke netwerken en locaties met connectorgroepen](application-proxy-connector-groups.md) om te zien hoe u ook connectorgroepen kunt gebruiken bij het segmenteren van uw connectors door network of een locatie.

* **De time-out van de back-end-toepassing instelt**: Deze instelling is nuttig in scenario's waar de toepassing mogelijk meer dan 75 seconden voor het verwerken van een clienttransactie. Bijvoorbeeld fungeert wanneer een client stuurt een query naar een webtoepassing die als een front-end naar een database. De front-end verzendt deze query naar de back-end-database-server en wachten op antwoord, maar op het moment dat het een antwoord is ontvangen, wordt de client-zijde van de conversatie time-out optreedt. De time-out wordt ingesteld op de lange biedt 180 seconden langer transacties te voltooien.

* **Juiste Cookie typen gebruiken**

   * **Alleen HTTP-Cookie**: Biedt extra beveiliging doordat de vlag HTTPOnly opnemen in de HTTP-antwoordheaders instellen-cookie-toepassingsproxy. Deze instelling kunt u beperken van aanvallen zoals cross-site scripting (XSS). Laat deze niet voor clients/Gebruikeragents die toegang tot de sessiecookie nodig hebben. Bijvoorbeeld, gepubliceerd RDP/MTSC-client maakt verbinding met een extern bureaublad-Gateway via App Proxy.

   * **Beveiligde Cookie**: Wanneer een cookie is ingesteld met het kenmerk Secure, wordt de gebruikersagent (Client-side-app) alleen de cookie opgenomen in de HTTP-aanvragen als de aanvraag wordt verzonden via een veilig TLS-kanaal. Dit helpt het risico van een cookie worden aangetast via niet-versleutelde tekst kanalen, dus moet worden ingeschakeld.

   * **Permanente cookies**: Hiermee kunt de Application Proxy-sessiecookie om vast te leggen tussen de browser is gesloten door nog geldig totdat deze verloopt of is verwijderd. Gebruikt voor scenario's waarbij een rijke, zoals office-toepassing toegang heeft tot een document in een gepubliceerde webtoepassing, zonder dat de gebruiker wordt opnieuw om verificatie wordt gevraagd. Inschakelen met waarschuwing echter als permanente cookies kunt uiteindelijk een service laten lopen van niet-geautoriseerde toegang als niet wordt gebruikt in combinatie met andere compenserende besturingselementen. Deze instelling moet alleen worden gebruikt voor oudere toepassingen die cookies tussen processen niet delen. Het is beter om uw toepassing bijwerken voor het verwerken van delen cookies tussen processen in plaats van deze instelling.

* **URL's vertalen in Headers**: U inschakelen dit voor scenario's waarbij interne DNS kan niet worden geconfigureerd zodat deze overeenkomen met de naamruimte van de organisatie public (ook Split DNS). Tenzij uw toepassing de oorspronkelijke host-header in de clientaanvraag vereist, laat u deze waarde is ingesteld op Ja. Het alternatief is dat de connector gebruik van de FQDN-naam in de interne URL voor de routering van het werkelijke verkeer en de FQDN-naam in de externe URL, als de host-header. In de meeste gevallen deze alternatieve werkwijze mag de toepassing goed te laten die normaal werken, wanneer deze extern, maar uw gebruikers de voordelen van een overeenkomende binnen en buiten URL gaan verloren.

* **URL's vertalen in de hoofdtekst van de toepassing**: Hoofdtekst van de toepassing koppeling vertaling voor een app inschakelen als u wilt dat de koppelingen vanuit deze app moet worden vertaald in antwoorden terug naar de client. Indien ingeschakeld, biedt deze functie een poging moeite bij het vertalen van alle interne koppelingen die App Proxy worden gevonden in HTML en CSS-antwoorden aan clients wordt geretourneerd. Dit is handig bij het publiceren van apps die vastgelegde absolute of NetBIOS-shortname koppelingen in de inhoud bevatten of apps met inhoud die is gekoppeld aan andere on-premises toepassingen.

Inschakelen voor scenario's waarbij een gepubliceerde app wordt gekoppeld aan andere apps gepubliceerde, koppeling vertaling voor elke toepassing die u controle over de gebruikerservaring op het niveau van de per-app hebt.

Stel bijvoorbeeld dat u hebt drie toepassingen die zijn gepubliceerd via toepassingsproxy dat alle aan elkaar koppelen: Voordelen, kosten, en reizen, plus een vierde Feedback, die niet worden gepubliceerd via toepassingsproxy-app.

![Afbeelding 1](media/App-proxy-deployment-plan/link-translation.png)

Wanneer u een koppeling vertaling voor de app voordelen inschakelt, wordt de koppeling naar de kosten en reizen worden omgeleid naar de externe URL's voor deze apps, zodat gebruikers toegang hebben tot de toepassingen van buiten het bedrijfsnetwerk toegang hebben tot deze. Koppelingen van uitgaven en reizen naar voordelen werken niet omdat de vertaling van koppelingen van deze twee apps nog niet is ingeschakeld. De koppeling naar Feedback is niet omgeleid omdat er geen externe URL, zodat gebruikers met behulp van de voordelen-app is niet mogelijk toegang tot de app feedback van buiten het bedrijfsnetwerk. Zie de gedetailleerde informatie op [vertaling en andere opties omleiden koppelen](application-proxy-configure-hard-coded-link-translation.md).

### <a name="access-your-application"></a>Toegang tot uw toepassing

Er zijn diverse opties voor het beheren van toegang voor het App-Proxy gepubliceerde bronnen, dus kiest u het meest geschikt is voor de behoeften van uw opgegeven scenario en de schaalbaarheid. Algemene methoden zijn: met behulp van on-premises-groepen die zijn gesynchroniseerd wordt via Azure AD Connect, het maken van dynamische groepen in Azure AD op basis van gebruikerskenmerken, met behulp van selfservice-groepen die worden beheerd door een resource-eigenaar of een combinatie van al deze waarden. Zie de gekoppelde resources voor de voordelen van elk.

De meeste duidelijk manier om gebruikers toegang toewijzen aan een toepassing gaat in de **gebruikers en groepen** opties in het linkerdeelvenster van groepen of personen rechtstreeks toe te wijzen en de gepubliceerde toepassing.

![Afbeelding 24 uur per dag](media/App-proxy-deployment-plan/add-user.png)

U kunt kunnen gebruikers ook selfservice toegang tot uw toepassing door het toewijzen van een groep waarvan ze geen lid van en het configureren van de opties voor self-service voor stroomactiviteitvoortgang.

![Afbeelding 25](media/App-proxy-deployment-plan/allow-access.png)

Indien ingeschakeld, gebruikers vervolgens zich aan te melden bij de MyApps-portal en toegang aanvragen en een worden automatisch goedgekeurd en die zijn toegevoegd aan de al toegestane groepsbeheer met Self-service of moeten worden goedgekeurd van een aangewezen fiatteur.

Gastgebruikers kunnen ook worden [uitgenodigd voor toegang tot interne toepassingen die zijn gepubliceerd via toepassingsproxy via Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/add-users-information-worker).

Voor on-premises toepassingen die anoniem toegankelijk zijn normaal gesproken, waarvoor geen verificatie, kunt u wellicht beter om uit te schakelen van de optie die zich in een van de toepassing **eigenschappen**.

![Afbeelding 26](media/App-proxy-deployment-plan/assignment-required.png)


Deze optie is ingesteld op Nee, kunnen gebruikers toegang krijgen tot de on-premises toepassing via Azure AD App Proxy zonder machtigingen, dus wees voorzichtig.

Zodra uw toepassing is gepubliceerd, moet toegankelijk is door de externe URL in een browser te typen of door op het pictogram [ https://myapps.microsoft.com ](https://myapps.microsoft.com/).

### <a name="enable-pre-authentication"></a>Verificatie vooraf inschakelen

Controleer of uw toepassing toegankelijk zijn via de toepassingsproxy wordt benaderd via de externe URL. 

1. Navigeer naar **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen** en kies de app die u wilt beheren.

2. Selecteer **toepassingsproxy**.

3. In de **pre-authenticatie** veld, gebruikt u de vervolgkeuzelijst selecteren **Azure Active Directory**, en selecteer **opslaan**.

Azure AD een uitdaging voor gebruikers eerst voor verificatie met vooraf-verificatie ingeschakeld, en als eenmalige aanmelding configued is vervolgens de back-endtoepassing ook controleert of de gebruiker om toegang tot de toepassing te krijgen. De externe URL met HTTPS, het wijzigen van de pre-authenticatie-modus van Passthrough naar Azure AD ook worden geconfigureerd, zodat elke toepassing in eerste instantie geconfigureerd voor HTTP wordt nu beveiligd met HTTPS.

### <a name="enable-single-sign-on"></a>Eenmalige aanmelding inschakelen

Eenmalige aanmelding biedt de best mogelijke gebruikerservaring en de beveiliging omdat gebruikers hoeft aan te melden bij één keer bij het openen van Azure AD. Wanneer een gebruiker vooraf is geverifieerd, wordt eenmalige aanmelding uitgevoerd door de Application Proxy connector worden geverifieerd bij de on-premises toepassing, namens de gebruiker. De back-endtoepassing verwerkt de aanmelding alsof het de gebruiker zelf. 

Kies de **Passthrough** optie kunnen gebruikers toegang krijgen tot de gepubliceerde toepassing zonder te verifiëren met Azure AD.

Uitvoeren van eenmalige aanmelding is alleen mogelijk dat als Azure AD kan de gebruiker toegang tot een bron aanvraagt, zodat uw toepassing moet zijn geconfigureerd voor verificatie vooraf van gebruikers met Azure AD Access voor eenmalige aanmelding functie hebt geïdentificeerd, anders wordt de SSO-opties uitgeschakeld.

Lezen [Single sign-on bij toepassingen in Azure AD](what-is-single-sign-on.md) aan bij het kiezen van de meest geschikte methode voor eenmalige aanmelding bij het configureren van uw toepassingen.

###  <a name="working-with-other-types-of-applications"></a>Werken met andere soorten toepassingen

Azure AD Application Proxy kan ook ondersteuning voor toepassingen die zijn ontwikkeld voor het gebruik van onze Azure AD Authentication Library ([ADAL](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)) of Microsoft Authentication Library ([MSAL](https://azure.microsoft.com/blog/start-writing-applications-today-with-the-new-microsoft-authentication-sdks/)). Deze biedt ondersteuning voor systeemeigen client-apps door de gebruikte Azure AD dat is uitgegeven tokens ontvangen in de header-informatie van de clientaanvraag voor het uitvoeren van pre-authenticatie namens de gebruikers.

Lezen [systeemeigen en mobiele client-apps publiceren](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-native-client) en [op claims gebaseerde toepassingen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-claims-aware-apps) voor meer informatie over de beschikbare configuraties van Application Proxy.

### <a name="use-conditional-access-to-strengthen-security"></a>Gebruik voorwaardelijke toegang om de beveiliging versterken

Beveiliging van toepassingen is vereist voor een geavanceerde set met mogelijkheden voor beveiliging die kan beveiligen van en reageren op bedreigingen van complexe on-premises en in de cloud. Meestal de deur krijgen toegang tot het bedrijfsnetwerk via zwakke, standaard of gestolen gebruikersreferenties.  Microsoft identity-driven security beperkt gebruik van gestolen referenties door te beheren en beveiligen van bevoegde en niet-gemachtigde identiteiten.

De volgende mogelijkheden kunnen worden gebruikt voor de ondersteuning van Azure AD Application Proxy:

* Gebruikers- en locatie gebaseerde voorwaardelijke toegang: Om gevoelige gegevens te beschermen door het beperken van toegang voor gebruikers op basis van geografische locatie of een IP-adres met [beleid voor voorwaardelijke toegang op basis van locatie](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations).

* Apparaat gebaseerde voorwaardelijke toegang: Zorg ervoor dat alleen geregistreerde, goedgekeurde en compatibele apparaten hebben toegang tot bedrijfsgegevens met [apparaat gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications).

* Voorwaardelijke toegang op basis van toepassing: Werk beschikt niet over te stoppen wanneer een gebruiker is niet op het bedrijfsnetwerk bevinden. [Beveiligde toegang tot zakelijke apps van cloud en on-premises](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam) en beheren met voorwaardelijke toegang.

* Risico's gebaseerde voorwaardelijke toegang: Uw gegevens te beschermen tegen schadelijke hackers met een [beleid voor voorwaardelijke toegang op basis van risico](https://www.microsoft.com/cloud-platform/conditional-access) die kunnen worden toegepast op alle apps en voor alle gebruikers, of on-premises of in de cloud.

* Azure AD-Toegangsvenster: Met uw service voor toepassingsproxy geïmplementeerd, en de toepassingen veilig gepubliceerd, biedt uw gebruikers een eenvoudige hub om te ontdekken en toegang tot al hun toepassingen. Verhoog de productiviteit met selfservice mogelijkheden, zoals de mogelijkheid aan te vragen van toegang tot nieuwe apps en -groepen of beheren van toegang tot deze bronnen namens anderen, via de [Toegangsvenster](https://aka.ms/AccessPanelDPDownload).

## <a name="manage-your-implementation"></a>Beheren van uw implementatie

### <a name="required-roles"></a>Vereiste functies

Microsoft, wordt het principe van het verlenen van de geringst mogelijke bevoegdheden om uit te voeren van de noodzakelijke taken met Azure AD. [Bekijk de verschillende Azure-rollen die beschikbaar zijn](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) en kies de juiste is voor de behoeften van elke persona. Sommige functies mogelijk tijdelijk wordt toegepast en wordt verwijderd wanneer de implementatie is voltooid.

| Zakelijke rol| Zakelijke taken| Azure AD-rollen |
|---|---|---|
| Helpdesk-beheerder helpen | Doorgaans is beperkt tot het in aanmerking komende gebruiker gemelde problemen en het uitvoeren van beperkte taken, zoals het wijzigen van wachtwoorden van gebruikers, ongeldig vernieuwingstokens en de bewaking van status van de service. | Helpdesk-beheerder |
| Beheer van identiteit| Problemen met betrekking tot Azure lezen AD-aanmelding in rapporten en auditlogboeken voor foutopsporing van App-Proxy.| Beveiligingslezer |
| De eigenaar van toepassing| Maken en beheren van alle aspecten van zakelijke toepassingen, registratie en instellingen van de toepassingsproxy.| Beheerder van de toepassing |
| Infrastructuur beheerder | Rollover van de eigenaar van certificaat | Beheerder van de toepassing |

Door het aantal gebruikers die toegang hebben tot het beveiligen van gegevens of resources kunnen verminderen de kans dat een kwaadwillende actor onbevoegde toegang of een geautoriseerde gebruiker per ongeluk die invloed hebben op een gevoelige resource verkrijgen. 
 
Gebruikers moeten echter nog steeds te verrichten dagelijkse beschermde bewerkingen, dus het afdwingen van just-in-time (JIT) op basis van [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) beleidsregels voor on-demand bevoegde toegang tot Azure-resources en Azure AD is onze aanbevolen benadering van effectief beheerderstoegang beheren en controleren.

### <a name="reporting-and-monitoring"></a>Rapportage- en bewakingsdoeleinden

Als u meer inzicht in uw organisatie gebruikersinrichting gebruik en de operationele status via controlelogboeken en -rapporten krijgt u een Azure AD. 

#### <a name="application-audit-logs"></a>Auditlogboeken van toepassingen

Deze logboeken bevatten gedetailleerde informatie over aanmeldingen bij de toepassingen die zijn geconfigureerd met Application Proxy en het apparaat en de gebruiker toegang tot de toepassing. Auditlogboeken bevinden zich in de Azure-portal en Audit-API voor het exporteren.

#### <a name="windows-event-logs-and-performance-counters"></a>Windows-gebeurtenislogboeken en prestatiemeteritems

Connectors zijn zowel de beheerder als de sessie Logboeken. De beheerder-logboeken bevatten belangrijke gebeurtenissen en hun fouten. De sessielogboeken bevatten alle transacties en de bijbehorende verwerkingsgegevens. Logboeken en prestatiemeteritems bevinden zich in de Windows-gebeurtenislogboeken en volg deze [zelfstudie gebeurtenislogboek gegevensbronnen configureren in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-windows-events).

### <a name="troubleshooting-guide-and-steps"></a>Gids voor probleemoplossing en stappen

Meer informatie over veelvoorkomende problemen en hoe u deze kunt oplossen met onze handleiding voor [probleemoplossing](application-proxy-troubleshoot.md) foutberichten. 

De volgende artikelen voor de algemene scenario's die kunnen ook worden gebruikt om te maken van de handleidingen voor probleemoplossing voor uw ondersteuningsorganisatie. 

* [Probleem bij weergeven app-pagina](application-proxy-page-appearance-broken-problem.md)
* [Applicatie laden duurt te lang](application-proxy-page-load-speed-problem.md)
* [Koppelingen op de toepassingspagina werken niet](application-proxy-page-links-broken-problem.md)
* [Welke poorten moet ik openen voor mijn app?](application-proxy-connectivity-ports-how-to.md)
* [Geen werkende connector in een connectorgroep voor mijn app](application-proxy-connectivity-no-working-connector.md)
* [Configureren in beheerportal](application-proxy-config-how-to.md)
* [Eenmalige aanmelding voor mijn app configureren](application-proxy-config-sso-how-to.md)
* [Probleem bij het maken van een app in de beheerportal](application-proxy-config-problem.md)
* [Kerberos-beperkte overdracht configureren](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
* [Configureren met PingAccess](application-proxy-back-end-ping-access-how-to.md)
* [Geen toegang tot deze zakelijke toepassing-fout](application-proxy-sign-in-bad-gateway-timeout-error.md)
* [Probleem bij het installeren van de connector voor de toepassingsproxyagent](application-proxy-connector-installation-problem.md)
* [Aanmeldingsprobleem](application-sign-in-problem-on-premises-application-proxy.md)
