---
title: Kies de juiste verificatiemethode voor uw Azure AD-oplossing voor hybride identiteit | Microsoft-documenten
description: Deze handleiding helpt CEO, CIO's CISOs, Chief identiteit architecten, Enterprise-architecten en beveiliging en besluitvormers IT verantwoordelijk voor het kiezen van een verificatiemethode voor hun Azure AD-oplossing voor hybride identiteit in middelgrote tot grote organisaties.
services: active-directory
keywords: ''
author: martincoetzer
ms.author: martincoetzer
ms.date: 04/12/2018
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.openlocfilehash: 6d107b9264a80c7b280ffed9a50b7bb0ffe354be
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="choose-the-right-authentication-method-for-your-azure-active-directory-hybrid-identity-solution"></a>Kies de juiste verificatiemethode voor uw Azure Active Directory-oplossing voor hybride identiteit 

In dit artikel begint een reeks van artikelen waarmee organisaties implementeren van een volledige hybride identiteitsoplossing met Azure Active Directory (Azure AD). Deze oplossing is als de hybride identiteit digitale transformatie Framework beschreven. Dit omvat de bedrijfsresultaten en doelstellingen organisaties kunnen zich concentreren op voor het implementeren van een robuuste en veilige hybride identiteitsoplossing. 

Het eerste zakelijke resultaat van het framework worden de vereisten voor organisaties voor het beveiligen van het verificatieproces als gebruikers toegang hebben tot cloud-apps uitgeschreven. Het eerste zakelijke doel in het resultaat van de beveiligde business verificatie is de mogelijkheid aan te melden bij cloud-apps met behulp van de lokale gebruikersnamen en wachtwoorden van gebruikers. Dit proces aanmelden naar en hoe gebruikers verifiëren zich ervoor zorgen dat alles in de cloud mogelijk.

De juiste verificatie-methode te kiezen, is de allereerst voor organisaties die willen hun apps verplaatsen naar de cloud. Geen voert u deze beslissing licht, om de volgende redenen:

1. Het is de eerste beslissing die voor een organisatie die wil verplaatsen naar de cloud. 

2. De methode voor verificatie is een essentieel onderdeel van de aanwezigheid van een organisatie in de cloud. Hiermee bepaalt u de toegang tot alle cloudgegevens en resources.

3. Vormt de basis van alle andere geavanceerde beveiliging en functies voor de ervaring in Azure AD.

4. De verificatiemethode is moeilijk te wijzigen nadat deze geïmplementeerd.

Identiteit is het nieuwe besturingselement vlak van IT-beveiliging. Verificatie is daarom een organisatie toegang guard met de nieuwe cloud wereld. Organisaties moeten een identiteit besturingselement vlak die versterkt de beveiliging ervan controleren en ervoor zorgen dat hun cloud-apps beveiligen tegen indringers.

### <a name="out-of-scope"></a>Buiten het bereik
Organisaties waarvoor geen een bestaande lokale directory footprint zijn niet de focus van dit artikel. Deze bedrijven worden normaal gesproken identiteiten alleen in de cloud, waarvoor een hybride identiteitsoplossing niet maken. Alleen in de cloud-identiteiten uitsluitend in de cloud bestaat en niet zijn gekoppeld aan de bijbehorende on-premises identiteiten.

## <a name="authentication-methods"></a>Verificatiemethoden
Als de Azure AD-oplossing voor hybride identiteit het nieuwe besturingselement vlak is, is verificatie de basis van de toegang tot de cloud. De juiste verificatie-methode te kiezen, is een belangrijke eerste beslissing bij het instellen van een hybride identiteitsoplossing voor Azure AD. De verificatiemethode die wordt geconfigureerd met behulp van Azure AD Connect, die ook gebruikers in de cloud bepalingen implementeren.

Om een verificatiemethode te selecteren, moet u rekening houden met de tijd, de bestaande infrastructuur, de complexiteit en kosten van de implementatie van uw keuze. Deze factoren zijn voor elke organisatie anders en na verloop van tijd kunnen wijzigen. 

Azure AD ondersteunt de volgende verificatiemethoden voor oplossingen voor hybride identiteit.

### <a name="cloud-authentication"></a>Cloud-verificatie
Als u deze verificatiemethode kiest, wordt Azure AD gebruikers aanmelden proces. In combinatie met naadloze eenmalige aanmelding (SSO), kunnen gebruikers zich aanmelden bij cloud-apps zonder dat hun referenties opnieuw invoeren. Met cloud-verificatie, kunt u kiezen uit twee opties: 

**Synchronisatie van Azure AD wachtwoordhash**. De eenvoudigste manier om de verificatie voor on-premises mapobjecten in Azure AD inschakelen. Gebruikers kunnen gebruiken dezelfde gebruikersnaam en wachtwoord dat ze lokale gebruiken zonder dat u hoeft aanvullende infrastructuur te implementeren. Sommige premium-functies van Azure AD, zoals Identity Protection vereisen synchronisatie van wachtwoordhash voor ongeacht welke verificatiemethode die u kiest.

> [!NOTE] 
> Wachtwoorden worden nooit in leesbare tekst opgeslagen of versleuteld met een algoritme voor omkeerbare in Azure AD. Zie voor meer informatie over het proces van synchronisatie van wachtwoordhash [hash Wachtwoordsynchronisatie met Azure AD Connect-synchronisatie implementeren](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization). 

**Azure AD Pass-through-verificatie**. Biedt een eenvoudige wachtwoordvalidatie voor Azure AD-services voor verificatie met behulp van een softwareagent die wordt uitgevoerd op een of meer lokale servers. De servers valideren de gebruikers rechtstreeks met uw lokale Active Directory, die zorgt ervoor dat de wachtwoordvalidatie van het in de cloud gebeurt niet. 

Bedrijven met een beveiligingsvereiste af te dwingen onmiddellijk on-premises gebruiker account statussen, wachtwoordbeleid, en aanmelden uren kunnen deze verificatiemethode te gebruiken. Zie voor meer informatie over het werkelijke Pass Through-verificatieproces [gebruiker aanmelden met Azure AD pass-through-verificatie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication).

### <a name="federated-authentication"></a>Federatieve verificatie
Als u deze verificatiemethode kiest, Azure AD handen het verificatieproces voor een afzonderlijke vertrouwde verificatiesysteem, zoals lokale Active Directory Federation Services (AD FS) uitgeschakeld voor het valideren van het wachtwoord van de gebruiker.

De verificatie kan worden gegeven als u meer geavanceerde verificatie-vereisten. Voorbeelden zijn smartcard gebaseerde verificatie of meervoudige verificatie van derden. Zie voor meer informatie [implementeren van Active Directory Federation Services](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/windows-server-2012-r2-ad-fs-deployment-guide).

De volgende sectie kunt u bepalen welke verificatiemethode is geschikt voor u met behulp van een beslissingsstructuur. Kunt u bepalen of voor het implementeren van cloud- of federatieve verificatie voor uw Azure AD-oplossing voor hybride identiteit.

## <a name="decision-tree"></a>Beslissingsstructuur

![Azure AD-beslissingsstructuur voor verificatie](media/azure-ad/azure-ad-authn-image1.png)

## <a name="detailed-considerations"></a>Gedetailleerde overwegingen

### <a name="cloud-authentication-password-hash-synchronization"></a>Cloud-verificatie: synchronisatie van wachtwoordhash

* **Moeite**. Synchronisatie van wachtwoordhash is het gemakkelijkst met betrekking tot implementatie, onderhoud en infrastructuur.  Dit niveau moeiteloos geldt doorgaans voor organisaties die moeten alleen hun gebruikers aan te melden bij Office 365, SaaS-apps en andere Azure AD gebaseerde-resources. Wanneer ingeschakeld, wordt synchronisatie van wachtwoordhash maakt deel uit van het proces van Azure AD Connect-synchronisatie en wordt elke twee minuten uitgevoerd.

* **Gebruikerservaring**. Ter verbetering van de aanmeldingservaring voor gebruikers implementeert naadloze eenmalige aanmelding met synchronisatie van wachtwoordhash. Naadloze eenmalige aanmelding elimineert onnodige wordt u gevraagd wanneer gebruikers zijn aangemeld.

* **Ingewikkelde**. Als organisaties kiest, is het mogelijk te gebruiken van inzicht in identiteiten met Azure AD Identity Protection-rapporten. Een voorbeeld is als het rapport gelekte aanmeldingsreferenties. Windows Hello voor bedrijven is een andere optie met [specifieke vereisten voor het gebruik van synchronisatie van wachtwoordhash](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification). 

    Organisaties waarvoor multifactor-verificatie met synchronisatie van wachtwoordhash moeten Azure AD multifactor-verificatie gebruiken. Deze organisaties niet methoden van derden of on-premises meervoudige verificatie gebruiken.

* **Zakelijke continuïteit**. Met behulp van de synchronisatie van wachtwoordhash met cloud-verificatie is maximaal beschikbaar als een cloudservice voor alle Microsoft-datacenters. Om te controleren of de synchronisatie van wachtwoordhash niet verder omlaag gedurende langere perioden, implementeert u een tweede Azure AD Connect-server in de faseringsmodus in een stand-by-configuratie.

* **Overwegingen met betrekking tot**. Op dit moment afdwingen geen synchronisatie van wachtwoordhash onmiddellijk wijzigingen in het lokale account statussen. In dit geval een gebruiker heeft toegang tot cloud-apps totdat de status van de gebruiker is gesynchroniseerd naar Azure AD. Organisaties mogelijk deze beperking omzeilen door het uitvoeren van een nieuwe synchronisatiecyclus nadat beheerders updates voor account-status van lokale gebruikers bulksgewijs wilt. Een voorbeeld met het uitschakelen van accounts.

> [!NOTE]
> Het wachtwoord is verlopen en account vergrendelde statussen op dit moment niet worden gesynchroniseerd naar Azure AD met Azure AD Connect. 

Raadpleeg [uitvoering van synchronisatie van wachtwoordhash](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization) voor instructies voor implementatie.

### <a name="cloud-authentication-pass-through-authentication"></a>Cloud-verificatie: Pass through-verificatie  

* **Moeite**. Pass through-verificatie, moet u een of meer (we raden u aan drie) lightweight agents zijn geïnstalleerd op bestaande servers. Deze agenten moeten toegang hebben tot uw lokale Active Directory Domain Services, waaronder uw on-premises AD-domeincontrollers. Ze moeten uitgaande toegang tot het Internet en toegang tot uw domeincontrollers. Om deze reden wordt niet ondersteund de agents in een perimeternetwerk te implementeren. 

    Pass through-verificatie vereist onbeperkte netwerktoegang tot domeincontrollers. Alle netwerkverkeer is versleuteld en beperkt tot verificatieaanvragen. Zie voor meer informatie over dit proces de [beveiliging diepgaand](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-security-deep-dive) op Pass through-verificatie.

* **Gebruikerservaring**. Ter verbetering van de aanmeldingservaring voor gebruikers, naadloze eenmalige aanmelding met Pass through-verificatie te implementeren. Naadloze eenmalige aanmelding elimineert onnodige prompts nadat gebruikers zich aanmelden.

* **Ingewikkelde**. Pass through-verificatie wordt het beleid van de account on-premises afgedwongen op het moment van aanmelden. Bijvoorbeeld: toegang is geweigerd bij een on-premises gebruiker account status is uitgeschakeld, vergrendeld of [wachtwoord verlopen](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq#what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication) of valt buiten de uur wanneer de gebruiker is toegestaan aan te melden. 

    Organisaties waarvoor multifactor-verificatie met pass-through-verificatie moeten gebruiken om Azure multi-factor Authentication (MFA). Deze organisaties niet een methode van derden of on-premises meervoudige verificatie gebruiken. Geavanceerde functies vereisen dat de synchronisatie van wachtwoordhash wordt geïmplementeerd of kies van Pass through-verificatie of niet. Een voorbeeld is het rapport gelekte referenties van Identity Protection.

* **Zakelijke continuïteit**. Het is raadzaam dat u twee extra Pass through-verificatie agents te implementeren. Deze extra's zijn naast de eerste agent op de Azure AD Connect-server. Deze extra implementatie zorgt ervoor dat hoge beschikbaarheid van verificatieaanvragen. Wanneer u drie agents die zijn geïmplementeerd hebt, wordt een agent kan nog steeds mislukken wanneer een andere agent uitgeschakeld voor onderhoud is. 

    Er is een ander voordeel van synchronisatie van wachtwoordhash naast Pass through-verificatie implementeren. Het fungeert als een back-verificatiemethode wanneer de primaire authenticatiemethode niet meer beschikbaar is.

* **Overwegingen met betrekking tot**. U kunt de synchronisatie van wachtwoordhash gebruiken als een back-verificatiemethode voor Pass through-verificatie en de referenties van een gebruiker kunnen niet worden gevalideerd door de agents. Vervolgens de failover naar de synchronisatie van wachtwoordhash gebeurt niet automatisch. De methode eenmalige aanmelding handmatig overschakelen met behulp van Azure AD Connect. 

    Pass through-verificatie biedt alleen ondersteuning voor cloud-apps die gebruikmaken van moderne verificatie en specifieke Exchange Online-protocollen. Sommige protocollen zijn ActiveSync-, POP3- en IMAP4. Bijvoorbeeld: Microsoft Office 2013 en hoger ondersteuning moderne verificatie, maar eerdere versies niet. Zie voor meer informatie over ondersteuning van Office-Apps [moderne authenticatie van Office 365 bijgewerkt](https://blogs.office.com/en-us/2015/11/19/updated-office-365-modern-authentication-public-preview/). Zie voor andere overwegingen van Pass through-verificatie, met inbegrip van alternatieve ID ondersteunen, [Veelgestelde vragen over](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq).

Raadpleeg [Pass through-verificatie implementeren](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) voor instructies voor implementatie.

### <a name="federated-authentication"></a>Federatieve verificatie

* **Moeite**. Er is een systeem federatieve verificatie afhankelijk van vertrouwde externe systemen om gebruikers te verifiëren. Sommige bedrijven wilt hergebruiken van hun bestaande investeringen federatieve systeem met hun Azure AD-oplossing voor hybride identiteit. Het onderhoud en beheer van het systeem federatieve valt buiten de controle van Azure AD. Het is aan de organisatie met behulp van het systeem federatieve om te controleren of deze veilig geïmplementeerd en de belasting van de verificatie kan verwerken. 

* **Gebruikerservaring**. De gebruikerservaring van federatieve verificatie, is afhankelijk van de implementatie van de functies, de topologie en de configuratie van de federation-farm. Sommige organisaties moeten deze flexibiliteit om te passen en de toegang tot de federation-serverfarm aanpassen aan de beveiligingsvereisten configureren. Het is bijvoorbeeld mogelijk configureren intern verbonden gebruikers en apparaten automatisch aan te melden gebruikers, zonder dat ze om referenties wordt gevraagd. Deze configuratie werkt, omdat ze al aangemeld bij hun apparaten. Indien nodig, moeilijker sommige geavanceerde beveiligingsfuncties gebruikers aanmelden proces.

* **Ingewikkelde**. Een oplossing voor federatieve verificatie is meestal vereist wanneer klanten een verificatievereiste die Azure AD biedt geen ondersteuning voor systeemeigen hebben. Gedetailleerde informatie om u te helpen [optie het recht Aanmelden](https://blogs.msdn.microsoft.com/samueld/2017/06/13/choosing-the-right-sign-in-option-to-connect-to-azure-ad-office-365/). Houd rekening met de volgende algemene vereisten:

    * Verificatie waarvoor smartcards of certificaten.
    * On-premises MFA-servers of meervoudige derden.
    * Verificatie met behulp van oplossingen voor verificatie van derden. Zie de [Azure AD-federatiecompatibiliteitslijst](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility).
    * Meld u aan dat vereist dat een sAMAccountName, bijvoorbeeld domein\gebruikersnaam, in plaats van een User Principal Name (UPN), bijvoorbeeld user@domain.com.

* **Zakelijke continuïteit**. Federatieve systemen is doorgaans vereist voor een matrix taakverdeling van servers, bekend als een farm. Deze farm is geconfigureerd in een intern netwerk en de netwerktopologie perimeter hoge beschikbaarheid voor verificatieaanvragen.

    Synchronisatie van wachtwoordhash samen met federatieve verificatie als een back-verificatiemethode implementeren wanneer de primaire authenticatiemethode niet meer beschikbaar is. Een voorbeeld is wanneer de on-premises servers zijn niet beschikbaar. Sommige organisaties grote onderneming vereist een federatieve oplossing voor de ondersteuning van meerdere Internet inkomend punten geconfigureerd met geo-DNS voor lage latentie verificatieaanvragen.

* **Overwegingen met betrekking tot**. Federatieve systemen moeten normaal meer geïnvesteerd in on-premises infrastructuur. De meeste organisaties Kies deze optie als ze al een lokale federation-investering. En als het een vereiste sterke zakelijke gebruik van een één-id-provider. Federation is complexer om te werken en het oplossen van vergeleken met oplossingen voor cloud-verificatie.

Voor een nonroutable-domein dat kan niet worden geverifieerd in Azure AD, moet u extra configuratie voor het implementeren van de gebruiker-ID aanmelden. Deze vereiste staat bekend als alternatieve aanmeldings ID ondersteuning. Zie [Configuring Alternate Login ID](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) voor beperkingen en vereisten. Als u kiest voor het gebruik van een derde partij multi-factor authentication-provider met federatieve, zorg ervoor dat de provider ondersteunt WS-Trust zodat apparaten bij Azure AD.

Raadpleeg [federatieservers implementeren](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/deploying-federation-servers) voor instructies voor implementatie.

> [!NOTE] 
> Wanneer u uw Azure AD-oplossing voor hybride identiteit implementeert, moet u een van de ondersteunde topologieën van Azure AD Connect implementeren. Meer informatie over ondersteunde en niet-ondersteunde configuraties op [topologieën voor Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-topologies).

## <a name="architecture-diagrams"></a>Architectuur diagrammen

De architectuur op hoog niveau onderdelen vereist voor elke verificatiemethode die met uw Azure AD-oplossing voor hybride identiteit kunt u een overzicht van de volgende diagrammen. Ze bieden een overzicht om u te helpen bij het vergelijken van de verschillen tussen de oplossingen.

* Eenvoud van een wachtwoord-hash synchronisatie-oplossing:

    ![Azure AD hybride identiteit met synchronisatie van wachtwoordhash](media/azure-ad/azure-ad-authn-image2.png)

* De agentvereisten van Pass through-verificatie:

    ![Azure AD hybride identiteit met Pass through-verificatie](media/azure-ad/azure-ad-authn-image3.png)

* Onderdelen die vereist zijn voor Federatie in uw perimeternetwerk en het interne netwerk van uw organisatie:

    ![Azure AD hybride identiteit met federatieve verificatie](media/azure-ad/azure-ad-authn-image4.png)

## <a name="comparing-methods"></a>Vergelijking van methoden

|Overweging|Synchronisatie van wachtwoordhash + naadloze eenmalige aanmelding|Pass through-verificatie + naadloze eenmalige aanmelding|Federatie met AD FS|
|:-----|:-----|:-----|:-----|
|Waar treedt verificatie?|In de cloud|In de cloud na de uitwisseling van een beveiligd wachtwoord verificatie met de lokale verificatie-agent|On-premises|
|Wat zijn de vereisten voor on-premises server buiten het provisioning-systeem: Azure AD Connect?|Geen|Een server voor elke aanvullende verificatie-agent|Twee of meer AD FS-servers<br><br>Twee of meer WAP-servers in het perimeternetwerk/DMZ-netwerk|
|Wat zijn de vereisten voor lokale Internet en netwerken buiten het inrichtingsproces systeem?|Geen|[Uitgaande toegang tot Internet](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start) vanaf de servers met verificatie agents|[Toegang tot Internet voor binnenkomende](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/overview/ad-fs-requirements) naar WAP-servers in het perimeternetwerk<br><br>Inkomende netwerktoegang tot AD FS-servers vanaf WAP-servers in het perimeternetwerk<br><br>Netwerktaakverdeling|
|Is er een SSL-certificaat is vereist?|Nee|Nee|Ja|
|Is er een health monitoring oplossing?|Niet vereist|Agent-status geleverd door [Azure Active Directory-beheercentrum](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-pass-through-authentication)|[Azure AD Connect Health (Engelstalig)](https://docs.microsoft.com/en-us/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs)|
|Krijg de gebruikers eenmalige aanmelding bronnen in de cloud van apparaten binnen het bedrijfsnetwerk domein?|Ja, met [naadloze eenmalige aanmelding](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)|Ja, met [naadloze eenmalige aanmelding](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)|Ja|
|Welke typen aanmelden worden ondersteund?|UserPrincipalName + wachtwoord<br><br>Geïntegreerde Windows-verificatie met behulp van [naadloze eenmalige aanmelding](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)<br><br>[Alternatieve aanmeldings-ID](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-get-started-custom)|UserPrincipalName + wachtwoord<br><br>Geïntegreerde Windows-verificatie met behulp van [naadloze eenmalige aanmelding](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)<br><br>[Alternatieve aanmeldings-ID](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq)|UserPrincipalName + wachtwoord<br><br>sAMAccountName + wachtwoord<br><br>Geïntegreerde Windows-verificatie<br><br>[Certificaat en de smartcard voor verificatie](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-user-certificate-authentication)<br><br>[Alternatieve aanmeldings-ID](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configuring-alternate-login-id)|
|Is Windows Hello voor bedrijven ondersteund?|[Sleutel vertrouwensmodel](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Certificaat vertrouwensmodel met Intune](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Sleutel vertrouwensmodel](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Certificaat vertrouwensmodel met Intune](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Sleutel vertrouwensmodel](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Vertrouwensmodel van certificaat](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-key-trust-adfs)|
|Wat zijn de opties voor meervoudige verificatie?|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)<br><br>[Azure MFA-server](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)<br><br>[MFA van derden](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs)|
|Welke status voor het account van gebruikers worden ondersteund?|Uitgeschakelde accounts<br>(maximaal 30 minuten vertraging)|Uitgeschakelde accounts<br><br>Account vergrendeld<br><br>Wachtwoord is verlopen<br><br>-In uren|Uitgeschakelde accounts<br><br>Account vergrendeld<br><br>Wachtwoord is verlopen<br><br>-In uren|
|Wat zijn de opties voor voorwaardelijke toegang?|[Voorwaardelijke toegang van Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)|[Voorwaardelijke toegang van Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)|[Voorwaardelijke toegang van Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)<br><br>[Claimregels van AD FS](https://adfshelp.microsoft.com/AadTrustClaims/ClaimsGenerator)|
|Blokkeert verouderde protocollen ondersteund?|Nee|Nee|[Ja](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/access-control-policies-w2k12)|
|Kunt u het logo, de installatiekopie en de beschrijving op de aanmeldingspagina's aanpassen?|[Ja, met Azure AD Premium](https://docs.microsoft.com/en-us/azure/active-directory/customize-branding)|[Ja, met Azure AD Premium](https://docs.microsoft.com/en-us/azure/active-directory/customize-branding)|[Ja](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-federation-management#customlogo)|
|Welke geavanceerde scenario's worden ondersteund?|[Smart wachtwoord vergrendelen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-secure-passwords)<br><br>[Gelekte referenties rapporten](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-risk-events)|[Smart wachtwoord vergrendelen](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-smart-lockout)|Meerdere locaties verificatiesysteem lage latentie<br><br>[AD FS-extranetvergrendeling](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-lockout-protection)<br><br>[Integratie met identiteitssystemen van derden](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility)|

## <a name="recommendations"></a>Aanbevelingen
Uw identiteitssysteem zorgt ervoor dat uw gebruikers toegang tot cloud-apps en de line-of-business-apps u migreert en beschikbaar maken in de cloud. Als u wilt behouden geautoriseerde gebruikers productief en ongewenste actoren buiten uw organisatie gevoelige gegevens, verificatie controleert de toegang tot apps.

Gebruik of het inschakelen van synchronisatie van wachtwoordhash voor welke verificatiemethode die u, om de volgende redenen kiest:

1. **Hoge beschikbaarheid en herstel na noodgevallen**. Pass through-verificatie en Federatie zijn afhankelijk van on-premises infrastructuur. Voor Pass through-verificatie, de voetafdruk van de lokale omvat de serverhardware en de Pass through-verificatie networking agents vereisen. Voor Federatie is de voetafdruk van de lokale nog groter. Hiervoor moeten de servers in uw perimeternetwerk op proxy verificatieaanvragen en interne federatieservers. 

    Om te voorkomen potentiële fouten, redundante servers te implementeren. Vervolgens worden verificatieaanvragen altijd verwerkt als een onderdeel is mislukt. Pass through-verificatie en federation ook afhankelijk van domeincontrollers om te reageren op verificatieaanvragen voor, die kunnen ook mislukken. Veel van deze onderdelen moeten op gezond te blijven van onderhoud. Storingen zijn geneigd wanneer onderhoud is niet gepland en correct is geïmplementeerd. Vermijd storingen met behulp van synchronisatie van wachtwoordhash omdat de verificatieservice van Microsoft Azure AD cloud globaal worden geschaald en altijd beschikbaar is.

2. **Lokale onderbreking survival**.  De gevolgen van een lokale uitval vanwege een cyberbeveiliging aanval of noodgeval kunnen aanzienlijke, variërend van reputatie merk schade tot een paralyzed organisatie kan niet omgaan met de aanval worden. Veel organisaties zijn onlangs slachtoffer van malware-aanvallen, met inbegrip van gerichte ransomware, waardoor hun lokale servers omlaag gaan. Wanneer Microsoft klanten bekommeren om dergelijke aanvallen helpt, ziet deze twee categorieën van organisaties:

   * Organisaties die eerder zijn ingeschakeld op de synchronisatie van wachtwoordhash gewijzigd de verificatiemethode voor het gebruik van synchronisatie van wachtwoordhash. Ze zijn weer online binnen een paar uur. Met behulp van toegang tot e-mail via Office 365, hebben ze voor het oplossen van problemen en toegang tot andere werkbelastingen cloud-gebaseerde.

   * Organisaties die niet eerder voor synchronisatie van wachtwoordhash inschakelen moest gebruik te maken van niet-vertrouwde externe consumer e-mailsystemen voor communicatie en problemen oplossen. In deze gevallen geduurd ze weken of meer opnieuw actief zijn.

3. **Identiteitsbeveiliging**. Een van de beste manieren om gebruikers in de cloud te beschermen is Azure AD Identity Protection. Microsoft controleert voortdurend Internet voor gebruiker en wachtwoord bevat dat ongewenste actoren verkopen en beschikbaar op de donker web maken. Azure AD kunt u deze informatie gebruiken om te controleren of als een van de gebruikersnamen en wachtwoorden in uw organisatie worden getroffen. Het is daarom essentieel is voor synchronisatie van wachtwoordhash ongeacht welke verificatiemethode die u gebruikt, of die gefedereerd of Pass through-verificatie inschakelen. Gelekte referenties worden weergegeven als een rapport. Gebruik deze informatie om te blokkeren of afdwingen dat gebruikers hun wachtwoord moeten wijzigen wanneer ze proberen zich aanmelden met een gestolen wachtwoord.

Ten slotte op [Gartner](https://info.microsoft.com/landingIAMGartnerreportregistration.html), Microsoft heeft de meest uitgebreide reeks beheerfuncties voor identiteits- en toegangsbeheer. Microsoft-ingangen [450 miljard verificatieaanvragen](https://www.microsoft.com/en-us/security/intelligence-report) elke maand om toegang te bieden tot duizenden SaaS-toepassingen zoals Office 365 vanaf vrijwel elk apparaat. 

## <a name="conclusion"></a>Conclusie

In dit artikel bevat een overzicht van de verschillende verificatieopties die organisaties kunnen configureren en implementeren ter ondersteuning van toegang tot de cloud-apps. Organisaties kunnen om te voldoen aan verschillende bedrijven, beveiliging en technische vereisten, ervoor kiezen tussen Wachtwoordsynchronisatie hash, Pass through-verificatie en Federatie. 

U kunt elke verificatiemethode. Ondersteunt de inspanning om het implementeren van de oplossing en de gebruikerservaring van het proces aanmelden, het adres van uw zakelijke vereisten? Evalueren of uw organisatie nodig heeft voor de geavanceerde scenario's en functies van de zakelijke continuïteit van elke verificatiemethode. Ten slotte de overwegingen van elke verificatiemethode evalueren. Een van deze te voorkomen dat u bij de implementatie van uw keuze?

## <a name="next-steps"></a>Volgende stappen

Tegenwoordig, bedreigingen aanwezig 24 uur per dag zijn en afkomstig zijn van overal. De juiste verificatiemethode implementeren en wordt uw beveiligingsrisico's verminderen en beveiligen van uw identiteiten.

[Aan de slag](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad) met Azure AD en implementeren van de juiste verificatie-oplossing voor uw organisatie.

Als u denkt over het migreren van federatieve verificatie cloud, meer informatie over [wijzigen van de methode aanmelden](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-user-signin#changing-the-user-sign-in-method). Gebruiken om te plannen en implementeren van de migratie, [deze implementatieplannen project](http://aka.ms/deploymentplans).
