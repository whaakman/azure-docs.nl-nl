---
title: Kies de juiste verificatiemethode voor uw Azure AD-oplossing voor hybride identiteit | Microsoft-documenten
description: Deze handleiding is bedoeld voor het CEO, CIO's, CISOs, Chief identiteit architecten, Enterprise-architecten en beveiliging en besluitvormers IT verantwoordelijk voor het kiezen van een verificatiemethode voor hun Azure AD-oplossing voor hybride identiteit in middelgrote tot grote organisaties.
services: active-directory
keywords: ''
author: martincoetzer
ms.author: martincoetzer
ms.date: 04/12/2018
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.openlocfilehash: e84683fc529ebf91f1879727b478681c8697feb4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="choosing-the-right-authentication-method-for-your-azure-active-directory-hybrid-identity-solution"></a>Het kiezen van de juiste verificatiemethode voor uw Azure Active Directory-oplossing voor hybride identiteit 

Dit artikel is de eerste in een serie artikelen waarmee organisaties implementeren van een complete oplossing voor hybride identiteit Azure AD. Een complete Azure AD-oplossing voor hybride identiteit is beschreven als de hybride identiteit digitale transformatie Framework en dekt de bedrijfsresultaten en doelstellingen organisaties moeten zich richten op om ervoor te zorgen dat ze een robuuste en veilige hybride identiteit hebt geïmplementeerd oplossing. Het eerste zakelijke resultaat van het framework worden de vereisten voor organisaties voor het beveiligen van het verificatieproces als gebruikers toegang hebben tot cloud-apps uitgeschreven. Het eerste zakelijke doel in het resultaat van de beveiligde business verificatie is de mogelijkheid voor gebruikers aan te melden bij cloud-apps met behulp van de lokale gebruikersnamen en wachtwoorden. Dit proces aanmelden en hoe gebruikers verifiëren stelt alles in de cloud.

De juiste verificatie-methode te kiezen, is de allereerst voor organisaties die willen hun apps verplaatsen naar de cloud. Dit besluit mag niet licht worden genomen om de volgende redenen:

1. Het is de eerste beslissing die voor een organisatie willen verplaatsen naar de cloud. 

2. De methode voor verificatie is een essentieel onderdeel van een organisatie aanwezigheid in de cloud beheren van toegang tot alle cloudgegevens en bronnen.

3. Vormt de basis van alle andere geavanceerde beveiliging en functies voor de ervaring in Azure AD.

4. Problemen bij het wijzigen van de verificatiemethode eenmaal geïmplementeerd.

Verificatie is met de identiteit als het nieuwe besturingselement vlak van IT-beveiliging van een organisatie toegang guard met de nieuwe cloud wereld. Organisaties moeten controleren of dat het besturingselement identiteit vlak maakt de sterkere beveiliging en hun cloud-apps te beveiligen tegen indringers.

### <a name="out-of-scope"></a>Buiten het bereik

Organisaties waarvoor geen een bestaande lokale directory footprint zijn niet de focus van dit artikel. Deze bedrijven worden normaal gesproken identiteiten alleen in de cloud, waarvoor een hybride identiteitsoplossing niet maken. Alleen in de cloud-identiteiten uitsluitend in de cloud bestaat en niet zijn gekoppeld aan de bijbehorende on-premises identiteiten.  

## <a name="choosing-the-right-authentication-method"></a>De juiste verificatiemethode kiezen

Met hybride identiteitsoplossing Azure AD als het nieuwe besturingselement vlak is verificatie de basis van de toegang tot de cloud. De juiste verificatie-methode te kiezen, is een belangrijke eerste beslissing bij het instellen van een hybride identiteitsoplossing voor Azure AD. Implementatie van de verificatiemethode is geconfigureerd met Azure AD Connect die ook gebruikers in de cloud hebt ingericht. 

Om een verificatiemethode te selecteren, moet u rekening houden met de tijd, de bestaande infrastructuur, de complexiteit en kosten van de implementatie van uw keuze. Deze factoren zijn voor elke organisatie anders en na verloop van tijd kunnen wijzigen. 

Azure AD ondersteunt de volgende verificatiemethoden voor hybride identiteitsoplossingen:

### <a name="cloud-authentication"></a>Cloud-verificatie
Als u deze verificatiemethode kiest, wordt Azure AD het proces aanmelden voor gebruikers. In combinatie met naadloze eenmalige aanmelding (SSO), kunnen gebruikers zich aanmelden bij cloud-apps zonder dat hun referenties opnieuw invoeren. Met cloud-verificatie kunt u kiezen uit twee opties: 

**Synchronisatie voor wachtwoord-Hash (PBS)** : de eenvoudigste manier om de verificatie voor on-premises mapobjecten in Azure AD inschakelen. Wachtwoordhashsynchronisatie kunnen gebruikers dezelfde gebruikersnaam en wachtwoord dat ze lokale gebruiken gebruiken zonder dat u hoeft aanvullende infrastructuur te implementeren. Sommige premium-functies van Azure AD, zoals Identity Protection vereist Wachtwoordhashsynchronisatie ongeacht welke verificatiemethode is geselecteerd.

> [!NOTE] 
> Wachtwoorden worden nooit in leesbare tekst opgeslagen of versleuteld met een algoritme voor omkeerbare in Azure AD. Zie voor meer informatie over het proces van Wachtwoordhashsynchronisatie [hash Wachtwoordsynchronisatie met Azure AD Connect-synchronisatie implementeren](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization). 

**Pass through-verificatie (PTA)** : biedt een eenvoudige wachtwoordvalidatie voor Azure AD authentication services met behulp van een softwareagent uitgevoerd op een of meer lokale servers voor het valideren van de gebruikers rechtstreeks met uw lokale Active Directory zodat de wachtwoordvalidatie van het optreden niet in de cloud. Bedrijven met een beveiligingsvereiste af te dwingen onmiddellijk lokale gebruikersaccount statussen wachtwoordbeleid en aanmeldingstijden deze verificatiemethode gebruikt. Zie voor meer informatie over het werkelijke Pass Through-verificatieproces [gebruiker aanmelden met Azure Active Directory Pass through-verificatie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication).

### <a name="federated-authentication"></a>Federatieve verificatie
Wanneer u deze verificatiemethode Azure AD handen uit het verificatieproces voor een afzonderlijke vertrouwde verificatiesysteem, bijvoorbeeld een lokale Active Directory Federation Services (AD FS) voor het wachtwoord van de gebruiker te valideren. Het verificatiesysteem krijgt u extra verificatievereisten, zoals verificatie op basis van een smartcard of een derde partij multi-factor authentication. Zie voor meer informatie [implementeren van Active Directory Federation Services](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/windows-server-2012-r2-ad-fs-deployment-guide).

De volgende sectie kunt u bepalen welke verificatiemethode is geschikt voor u met behulp van een beslissingsstructuur. Deze methode kunt u bepalen of voor het implementeren van cloud- of federatieve verificatie voor uw Azure AD-oplossing voor hybride identiteit.

## <a name="azure-ad-authentication-decision-tree"></a>Azure AD-beslissingsstructuur voor verificatie

![image1](media/azure-ad/azure-ad-authn-image1.png)

## <a name="detailed-considerations-on-authentication-methods"></a>Gedetailleerde overwegingen op verificatiemethoden

### <a name="cloud-authentication-password-hash-sync"></a>Cloud-verificatie: Wachtwoordhashsynchronisatie 

* **Inspanning:** Wachtwoordhashsynchronisatie is het gemakkelijkst met betrekking tot implementatie, onderhoud en infrastructuur voor organisaties die alleen willen hun gebruikers in staat aan te melden bij Office 365, SaaS-apps en andere Azure AD gebaseerde-resources. Wanneer ingeschakeld, worden de functies Wachtwoordhashsynchronisatie maakt deel uit van het synchronisatieproces van Azure AD Connect en wordt elke twee minuten uitgevoerd. 

* **Gebruikerservaring:** het wordt aanbevolen dat organisaties met Wachtwoordhashsynchronisatie van de gebruiker aanmelden om ervaring te verbeteren door te vermijden onnodige prompts nadat ze ondertekend naadloze eenmalige aanmelding (SSO) implementeren.

* **Geavanceerde scenario's:** als organisaties, het is mogelijk inzicht in identiteiten met Azure AD Identity Protection rapporten, zoals het rapport gelekte referenties gebruiken. Windows Hello voor bedrijven is een andere optie met [specifieke vereisten voor het gebruik van Wachtwoordhashsynchronisatie](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification). Organisaties die behoefte hebben multi-factor authentication met Wachtwoordhashsynchronisatie Azure AD multi-factor authentication-server moeten gebruiken en een derde partij of lokale meervoudige verificatiemethoden niet gebruiken.

* **Bedrijfscontinuïteit:** Wachtwoordhashsynchronisatie is inherent maximaal beschikbaar als een cloudservice voor alle Microsoft-datacenters. Het verdient aanbeveling dat een tweede Azure AD Connect-server wordt geïmplementeerd in de faseringsmodus in een stand-by-configuratie voor noodherstel.

* **Overwegingen:** Wachtwoordhashsynchronisatie worden niet onmiddellijk afgedwongen door wijzigingen in het lokale account statussen momenteel. In dit geval heeft een gebruiker toegang tot de cloud-apps totdat de status van de gebruiker is gesynchroniseerd met Azure AD. Als deze beperking ondervangen dat organisaties willen, is het raadzaam om een nieuwe synchronisatiecyclus wordt geactiveerd nadat beheerders bulksgewijs updates voor de lokale gebruiker account status, zoals het uitschakelen van accounts. Vergrendelde-account is een andere status van de account die in de volgende cyclus wordt gesynchroniseerd. 

> [!NOTE] 
> Het wachtwoord verlopen staat momenteel niet wordt gesynchroniseerd naar Azure AD met de Azure AD Connect. 

Raadpleeg [Wachtwoordhashsynchronisatie implementeren](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization) voor instructies voor implementatie.

### <a name="cloud-authentication-pass-through-authentication"></a>Cloud-verificatie: Pass through-verificatie  

* **Inspanning:** voor Pass-through-verificatie, moet u een of meer (drie aanbevolen) lightweight agents zijn geïnstalleerd op bestaande servers met toegang tot uw lokale Active Directory Domain Services, waaronder toegang tot uw on-premises AD-domein domeincontrollers. Deze agenten moeten uitgaande toegang tot het Internet en hebben toegang tot uw domeincontrollers. Daarom is het niet ondersteund voor het implementeren van de agents in een perimeternetwerk, omdat hiervoor onbeperkte netwerktoegang tot domeincontrollers. Alle netwerkverkeer is versleuteld en beperkt tot verificatieaanvragen. Zie voor meer informatie over dit proces de [beveiliging diepgaand](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-security-deep-dive) op Pass through-verificatie.

* **Gebruikerservaring:** het wordt aanbevolen dat organisaties naadloze eenmalige aanmelding met Pass through-verificatie van de gebruiker aanmelden om ervaring te verbeteren door te vermijden onnodige prompts nadat ze ondertekend implementeren.

* **Geavanceerde scenario's:** Pass through-verificatie zorgt ervoor verificatieaanvragen onmiddellijk worden geweigerd als de status van een on-premises-gebruiker is uitgeschakeld, vergrendeld, het wachtwoord verlopen of valt buiten de gebruiker zich kan aanmelden toegestaan. Organisaties die multi-factor-verificatie met pass-through-verificatie vereist Azure AD multi-factor authentication-server moeten gebruiken en een methode van derden of on-premises meervoudige verificatie niet gebruiken. Geavanceerde functies, zoals het rapport gelekte referenties van de beveiliging van de identiteit is vereist dat Wachtwoordhashsynchronisatie ongeacht wordt geïmplementeerd als u ervoor Pass through-verificatie kiest.

* **Bedrijfscontinuïteit:** het wordt aanbevolen dat u twee extra Pass through-verificatie-agents, behalve de eerste agent op de Azure AD Connect-server om ervoor te zorgen hoge beschikbaarheid van verificatieaanvragen implementeert. Wanneer u drie agents die zijn geïmplementeerd hebt, wordt een agent kan nog steeds mislukken wanneer een andere agent uitgeschakeld voor onderhoud is. Een ander voordeel van het implementeren van Wachtwoordhashsynchronisatie naast Pass through-verificatie, is deze kan fungeren als back-verificatiemethode wanneer de primaire authenticatiemethode niet langer beschikbaar is, bijvoorbeeld wanneer de on-premises servers zijn niet beschikbaar.

* **Overwegingen:** als u Wachtwoordhashsynchronisatie als een back-verificatiemethode voor Pass through-verificatie gebruiken en de agents referenties van de gebruiker kunnen niet worden gevalideerd, wordt de failover naar Wachtwoordhashsynchronisatie niet automatisch gebeurt. U moet overschakelen van de aanmelding-methode handmatig met behulp van Azure AD Connect. Pass through-verificatie biedt alleen ondersteuning voor cloud-apps die gebruikmaken van moderne verificatie en specifieke Exchange Online-protocollen zoals ActiveSync-, POP3- en IMAP4. Bijvoorbeeld: [Microsoft Office 2013 en hoger ondersteunt moderne verificatie, maar geen eerdere versies](https://blogs.office.com/en-us/2015/11/19/updated-office-365-modern-authentication-public-preview/) op meer informatie van de ondersteuning van Office-Apps. Zie [Veelgestelde vragen over](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq) en andere overwegingen van Pass through-verificatie, met inbegrip van alternatieve ID ondersteunen.

Raadpleeg [Pass through-verificatie implementeren](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) voor instructies voor implementatie.

### <a name="federated-authentication"></a>Federatieve verificatie

* **Inspanning:** met behulp van een systeem federatieve verificatie zijn afhankelijk van een extern systeem om gebruikers te verifiëren. Sommige bedrijven wilt hergebruiken van hun bestaande investeringen federatieve systeem met hun Azure AD-oplossing voor hybride identiteit. Het onderhoud en beheer van het systeem federatieve valt buiten de controle van Azure AD. Het is tot de organisatie met behulp van het systeem federatieve om te controleren of deze veilig is geïmplementeerd en de belasting van de verificatie kan verwerken. 

* **Gebruikerservaring:** de gebruikerservaring van federatieve verificatie is afhankelijk van de implementatie van de functies, de topologie en de configuratie van de federation-farm. Sommige organisaties moeten deze flexibiliteit om te passen en de toegang tot de federation-serverfarm aanpassen aan de beveiligingsvereisten configureren. Het is bijvoorbeeld mogelijk om intern verbonden gebruikers en apparaten automatisch aan te melden gebruikers, zonder dat ze hoeven referenties, omdat ze hebben al aangemeld bij hun apparaten te configureren. Anderzijds, indien nodig, kunnen sommige geavanceerde beveiligingsfuncties moeilijker van de gebruiker aanmelden proces.

* **Geavanceerde scenario's:** oplossing voor federatieve verificatie is doorgaans vereist wanneer klanten een verificatievereiste niet ondersteund door Azure AD hebt, gedetailleerde informatie is [hier vermeld](https://blogs.msdn.microsoft.com/samueld/2017/06/13/choosing-the-right-sign-in-option-to-connect-to-azure-ad-office-365/), maar algemene vereisten zijn onder andere:

    * Verificatie vereisen smartcards of certificaten
    * Gebruik on-premises MFA-Server of van derden multi-factor Authentication-provider.
    * Verificatie met behulp van de oplossing van derden verificatie. Zie de [Azure AD-federatiecompatibiliteitslijst](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility).
    * Moeten gebruikers zich aanmelden bij het gebruik van hun sAMAccountName, bijvoorbeeld DOMEIN\gebruikersnaam in plaats van een UPN (User Principal Name), bijvoorbeeld: user@domain.com
    * Een Windows Hello voor bedrijven implementatie afhankelijke op een interne Enterprise Public Key Infrastructure (PKI) voor certificaatvertrouwenslijsten

* **Bedrijfscontinuïteit:** federatieve-systemen is doorgaans vereist voor een load balanced matrix van servers, ook wel bekend als een farm, geconfigureerd in een intern netwerk- en perimeter netwerktopologie hoge beschikbaarheid voor verificatieaanvragen. Wachtwoordhashsynchronisatie kan samen met federatieve verificatie worden geïmplementeerd als back-verificatiemethode wanneer de primaire authenticatiemethode niet langer beschikbaar is, bijvoorbeeld wanneer de on-premises servers zijn niet beschikbaar. Sommige organisaties grote onderneming vereist een federatieve oplossing voor de ondersteuning van meerdere Internet inkomend punten geconfigureerd met geo-DNS voor verificatieaanvragen van de lage latentie.

* **Overwegingen:** federatieve systemen doorgaans meer geïnvesteerd in on-premises infrastructuur vereist. De meeste organisaties Kies deze optie als er al een lokale federation-investering en een sterke bedrijfsvereiste gebruik van een enkele id-provider is. Federation is complexer om te werken en het oplossen van vergeleken met oplossingen voor cloud-verificatie. Gebruikers-id's wordt gebruikt met een niet-routeerbare domein dat kan niet worden geverifieerd in Azure AD aan te melden nodig extra configuratie te implementeren. Deze vereiste staat bekend als alternatieve aanmeldings ID ondersteuning. Zie [Configuring Alternate Login ID](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) voor beperkingen en vereisten.

Raadpleeg [federatieservices implementeren](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/deploying-federation-servers) voor instructies voor implementatie.

> [!NOTE] 
> Wanneer u uw Azure AD-oplossing voor hybride identiteit implementeert, moet u dat een van de ondersteunde topologieën van Azure AD Connect is geïmplementeerd. Meer informatie over ondersteunde en niet-ondersteunde configuraties op [topologieën voor Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-topologies).

## <a name="architecture-diagrams"></a>Architectuur diagrammen

Het volgende diagram overzichten op hoog niveau architectuur onderdelen vereist voor elke verificatiemethode die u met uw Azure AD-oplossing voor hybride identiteit gebruiken kunt. Dit biedt u een overzicht van de verschillen tussen de oplossingen te vergelijken.

Het volgende diagram geeft een overzicht van de eenvoud van een oplossing Wachtwoordhashsynchronisatie:

![PBS](media/azure-ad/azure-ad-authn-image2.png)

Het volgende diagram geeft een overzicht van de agentvereisten van Pass through-verificatie:

![PTA](media/azure-ad/azure-ad-authn-image3.png)

Het volgende diagram geeft een overzicht van de onderdelen die vereist zijn voor Federatie in uw perimeternetwerk en het interne netwerk van uw organisatie.

![ADFS](media/azure-ad/azure-ad-authn-image4.png)

## <a name="recommendations-and-considerations-from-azure-ad"></a>Aanbevelingen en overwegingen van Azure AD

Uw identiteitssysteem zorgt ervoor dat uw gebruikers toegang hebben tot cloud-apps en line-of-business-apps u migreert en beschikbaar maken in de cloud. Verificatie controleert de toegang tot apps, ervoor zorgen dat gemachtigde gebruikers productief en ongewenste actoren buiten de gevoelige gegevens van uw organisatie. Overweeg daarom de volgende aanbevelingen bij het kiezen van de juiste verificatiemethode voor uw organisatie. 

Gebruik of inschakelen Wachtwoordhashsynchronisatie ongeacht welke verificatiemethode u, om de volgende redenen kiest:

1. **Hoge beschikbaarheid en herstel na noodgevallen:** Pass through-verificatie en federation zijn afhankelijk van de on-premises infrastructuur. Voor Pass through-verificatie is de voetafdruk van de lokale de serverhardware en netwerken die de agents Pass through-verificatie vereist. Voor Federatie is de voetafdruk van de lokale nog grotere omdat hiervoor servers in uw perimeternetwerk op proxy verificatieaanvragen en interne federatieservers. Om te voorkomen potentiële fouten, moet uw organisatie redundante servers om ervoor te zorgen verificatieaanvragen altijd worden verwerkt als er een onderdeel niet implementeren. Pass through-verificatie en federation zijn ook vertrouwen op domeincontrollers te reageren op verificatieaanvragen voor, die kunnen ook mislukken. Veel van deze onderdelen moeten op gezond te blijven van onderhoud en storingen zijn geneigd wanneer onderhoud niet is gepland en correct is geïmplementeerd. Storingen kunnen worden vermeden door Wachtwoordhashsynchronisatie met omdat de service van Microsoft Azure AD cloud-verificatie wordt globaal worden geschaald en altijd beschikbaar is.

2. **Lokale onderbreking survival:** de gevolgen van een lokale uitval vanwege een cyberbeveiliging aanval of noodgeval aanzienlijke, variërend van reputatie merk schade tot een paralyzed organisatie kan niet omgaan met de aanval kunnen worden. In het afgelopen jaar zijn veel organisaties slachtoffer van malware-aanvallen, met inbegrip van gerichte ransomware, waardoor hun lokale servers als niet-actief. Helpt klanten bekommeren om dergelijke aanvallen, opgemerkt Microsoft twee categorieën van organisaties:

   a. Organisaties die voordien ingeschakeld was Wachtwoordhashsynchronisatie is door het wijzigen van de verificatiemethode voor het gebruik van Wachtwoordhashsynchronisatie weer online binnen een paar uur. Met behulp van toegang tot e-mail via Office 365, kunnen ze werken voor het oplossen van problemen en hebben toegang tot andere werkbelastingen op basis van een cloud.

   b. Organisaties die niet eerder Wachtwoordhashsynchronisatie inschakelen moest gebruik te maken van niet-vertrouwde externe consumer e-mailsystemen voor communicatie en problemen oplossen. In deze gevallen geduurd ze weken of meer opnieuw actief zijn.

3. **Identiteitsbeveiliging van de:** een van de aanbevolen manieren om gebruikers in de cloud te beschermen Azure AD Identity Protection is. Microsoft scant voortdurend Internet voor gebruiker en lijsten met wachtwoorden, ongewenste actoren verkopen en beschikbaar maken op de donker web. Azure AD kunt u deze informatie gebruiken om te controleren of als een van de gebruikersnamen en wachtwoorden in uw organisatie worden getroffen. Daarom is het essentieel is voor het inschakelen van Wachtwoordhashsynchronisatie ongeacht welke verificatiemethode die u gebruikt, of dat nu federatieve of Pass through-verificatie. Gelekte aanmeldingsreferenties worden weergegeven als een rapport en kunnen worden gebruikt om te blokkeren of een gebruiker te dwingen hun wachtwoord wijzigen wanneer ze proberen zich kunnen aanmelden met een gestolen wachtwoord.

Ten slotte op [Gartner](https://info.microsoft.com/landingIAMGartnerreportregistration.html), Microsoft heeft de meest uitgebreide reeks Identity and Access Management-functies. Microsoft fends uit zeven miljard cyberbeveiliging gebeurtenissen per dag en tegelijkertijd eenmalige aanmelding tot duizenden SaaS-toepassingen zoals Office 365 geautoriseerde gebruikers vanaf vrijwel elk apparaat. 

## <a name="conclusion"></a>Conclusie

In dit Artikeloverzichten verschillende verificatieopties, organisaties kunnen configureren en implementeren ter ondersteuning van toegang tot de cloud-apps. Organisaties kunnen om te voldoen aan verschillende bedrijven, beveiliging en technische vereisten, ervoor kiezen tussen Wachtwoordhashsynchronisatie, Pass through-verificatie en Federatie. Uw organisatie kunt kiezen of uw zakelijke vereisten worden verholpen door de inspanning om de oplossing en de gebruikerservaring van het proces van aanmeldingspagina te implementeren met elke verificatiemethode. U moet ook evalueren of uw organisatie nodig heeft voor de geavanceerde scenario's en functies van de zakelijke continuïteit van elke verificatiemethode. Ten slotte moet u evalueren van de overwegingen van elke verificatiemethode om te zien als een voorkomen u dat bij de implementatie van uw keuze.

## <a name="next-steps"></a>Volgende stappen

Tegenwoordig, bedreigingen aanwezig 24 uur per dag zijn en afkomstig zijn van overal. Implementatie van de juiste verificatiemethode helpt uw beveiligingsrisico's verminderen en de identiteiten van uw beveiligen. 

[Aan de slag](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad) met Azure AD en implementeren van de juiste verificatie-oplossing voor uw organisatie.

Als u migreren overweegt van federatieve voor cloud-verificatie, meer [over het wijzigen van de aanmeldingspagina methode](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-user-signin#changing-the-user-sign-in-method). U kunt gebruiken om te plannen en implementeren van de migratie, [deze plannen om u te helpen project](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/tree/master/Authentication).
