---
title: Selecteer de juiste verificatiemethode voor uw Azure AD-oplossing voor hybride identiteit | Microsoft Docs
description: Deze handleiding helpt CEO, CIO, gebied, Chief identiteit architecten, Enterprise Architects en beveiliging en IT-besluitvormers die verantwoordelijk is voor het kiezen van een verificatiemethode voor hun Azure AD-oplossing voor hybride identiteit in middelgrote tot grote organisaties.
services: active-directory
keywords: ''
author: martincoetzer
ms.author: martincoetzer
ms.date: 04/12/2018
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.openlocfilehash: 7acf18c4624373dff8994a1996e9082770b90270
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283684"
---
# <a name="choose-the-right-authentication-method-for-your-azure-active-directory-hybrid-identity-solution"></a>Selecteer de juiste verificatiemethode voor uw Azure Active Directory-oplossing voor hybride identiteit 

In dit artikel begint met een reeks artikelen waarmee organisaties implementeren van een complete hybride identiteitsoplossing met Azure Active Directory (Azure AD). Deze oplossing is beschreven als de [hybride identiteit digitale transformatie Framework](https://aka.ms/aadframework). Dit omvat de bedrijfsresultaten en doelstellingen organisaties kunnen zich concentreren op voor het implementeren van een robuuste en veilige hybride identiteitsoplossing. 

De eerste bedrijfsuitkomst van het framework worden de vereisten voor organisaties die voor het beveiligen van het verificatieproces als gebruikers toegang hebben tot cloud-apps uitgeschreven. Het eerste bedrijf doel in het resultaat van de beveiligde bedrijven verificatie is de mogelijkheid voor aanmelding bij cloud-apps met behulp van de on-premises gebruikersnamen en wachtwoorden van gebruikers. Dit proces aanmelden op en hoe gebruikers worden geverifieerd maken alles in de cloud mogelijk.

De juiste verificatiemethode kiezen, is het eerste aandachtspunt voor organisaties die willen hun apps verplaatsen naar de cloud. Geen Denk goed na voordat deze beslissing, om de volgende redenen:

1. Het is de eerste beslissing voor een organisatie die wil overstappen naar de cloud. 

2. De verificatiemethode is een essentieel onderdeel van de aanwezigheid van een organisatie in de cloud. Hiermee bepaalt u de toegang tot alle cloudgegevens en resources.

3. Dit vormt de basis van alle andere geavanceerde beveiliging en functies voor de gebruikerservaring in Azure AD.

4. De verificatiemethode is moeilijk te wijzigen nadat deze geïmplementeerd.

Identiteit is het nieuwe besturingselement vlak van IT-beveiliging. Verificatie is dus van een organisatie toegang guard naar de nieuwe cloudwereld. Organisaties moeten de controlelaag van een identiteit die resulteert in zwaardere beveiliging en hun cloud-apps beveiligen tegen indringers houdt.

### <a name="out-of-scope"></a>Buiten het bereik
Organisaties die niet beschikken over een bestaande on-premises directory footprint zijn niet de focus van dit artikel. Deze bedrijven worden normaal gesproken identiteiten alleen in de cloud, waarvoor een oplossing voor hybride identiteit niet maken. Cloud-only identiteiten bestaan enkel in de cloud en niet zijn gekoppeld aan bijbehorende on-premises identiteiten.

## <a name="authentication-methods"></a>Verificatiemethoden
Wanneer de Azure AD-oplossing voor hybride identiteit het nieuwe besturingselement vlak is, is verificatie de basis van cloudtoegang. De juiste verificatiemethode kiezen is een belangrijke eerste besluit om een Azure AD-oplossing voor hybride identiteit in te stellen. De verificatiemethode die is geconfigureerd met behulp van Azure AD Connect, die ook gebruikers in de cloud bepalingen implementeren.

Als u een verificatiemethode, moet u rekening houden met de tijd, de bestaande infrastructuur, de complexiteit en de kosten van de implementatie van uw keuze. Deze factoren zijn voor elke organisatie en kunnen na verloop van tijd worden gewijzigd. 

>[!VIDEO https://www.youtube.com/embed/YtW2cmVqSEw]

Azure AD ondersteunt de volgende verificatiemethoden voor hybride identiteitsoplossingen.

### <a name="cloud-authentication"></a>Cloud-verificatie
Als u ervoor deze verificatiemethode kiest, zorgt Azure AD aanmeldingsproces voor gebruikers. Gebruikers kunnen in combinatie met naadloze eenmalige aanmelding (SSO), zich aanmelden bij cloud-apps zonder hun referenties opnieuw in te voeren. Met cloud-verificatie, kunt u kiezen uit twee opties: 

**Azure AD wachtwoord-hashsynchronisatie**. De eenvoudigste manier om in te schakelen in Azure AD-verificatie voor on-premises directory-objecten. Gebruikers kunnen gebruiken dezelfde gebruikersnaam en wachtwoord dat ze on-premises gebruiken zonder dat u hoeft geen aanvullende infrastructuur te implementeren. Wachtwoord-hashsynchronisatie voor sommige premiumfuncties van Azure AD, zoals Identity Protection, vereist voor ongeacht welke verificatiemethode die u kiest.

> [!NOTE] 
> Wachtwoorden worden nooit opgeslagen in ongecodeerde tekst of versleuteld met een algoritme voor omkeerbare in Azure AD. Zie voor meer informatie over het proces van synchronisatie van wachtwoordhashes, [implementeren u wachtwoord-hashsynchronisatie met Azure AD Connect-synchronisatie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization). 

**Azure AD Pass-through-verificatie**. Biedt een eenvoudige wachtwoordvalidatie voor services van Azure AD-verificatie met behulp van een softwareagent die wordt uitgevoerd op een of meer on-premises servers. De servers valideren de gebruikers rechtstreeks met uw on-premises Active Directory, die zorgt ervoor dat de validatie van het wachtwoord in de cloud gebeurt niet. 

Bedrijven die een beveiligingsvereiste af te dwingen onmiddellijk on-premises gebruiker account Staten, wachtwoordbeleid, en aanmelden uren kunnen deze verificatiemethode te gebruiken. Zie voor meer informatie over het proces van het werkelijke Pass through-verificatie, [aanmelden van gebruikers met Azure AD-Pass through-verificatie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication).

### <a name="federated-authentication"></a>Federatieve verificatie
Als u ervoor deze verificatiemethode kiest, uitgeschakeld handen van Azure AD het verificatieproces uit naar een afzonderlijke verificatiesysteem vertrouwde, zoals on-premises Active Directory Federation Services (AD FS), voor het valideren van het wachtwoord van gebruikers.

Het verificatiesysteem krijgt u een meer geavanceerde verificatie-vereisten. Voorbeelden zijn smartcard-gebaseerde authenticatie of meervoudige verificatie van derden. Zie voor meer informatie, [implementeren van Active Directory Federation Services](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/windows-server-2012-r2-ad-fs-deployment-guide).

De volgende sectie kunt u bepalen welke verificatiemethode is geschikt voor u met behulp van een beslissingsstructuur. Hiermee kunt u bepalen of u cloud- of federatieve verificatie voor uw Azure AD hybride identiteitsoplossing implementeren.

## <a name="decision-tree"></a>Beslissingsstructuur

![Azure AD-verificatie-beslissingsstructuur](media/azure-ad/azure-ad-authn-image1.png)

## <a name="detailed-considerations"></a>Gedetailleerde overwegingen

### <a name="cloud-authentication-password-hash-synchronization"></a>Cloud-verificatie: wachtwoord-hashsynchronisatie

* **Inspanning**. Wachtwoord-hashsynchronisatie is het gemakkelijkst met betrekking tot implementatie, onderhoud en infrastructuur.  De hand van deze inspanningen geldt doorgaans voor organisaties die alleen hun gebruikers hoeft aanmelden bij Office 365, SaaS-apps en andere Azure AD gebaseerde-resources. Indien ingeschakeld, wordt wachtwoord-hashsynchronisatie maakt deel uit van de Azure AD Connect sync-proces en wordt elke twee minuten uitgevoerd.

* **Gebruikerservaring**. Ter verbetering van de aanmeldingservaring voor gebruikers implementeren naadloze eenmalige aanmelding met wachtwoord-hashsynchronisatie. Naadloze eenmalige aanmelding wordt voorkomen dat onnodige prompts wanneer gebruikers zijn aangemeld.

* **Geavanceerde scenario's**. Als organisaties, is het mogelijk om te verkrijgen op basis van identiteiten met Azure AD Identity Protection rapporten gebruiken. Een voorbeeld is als het rapport de referenties zijn gelekt. Windows Hello voor bedrijven is een andere optie waarvoor [specifieke vereisten als u wachtwoord-hashsynchronisatie](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification). 

    Organisaties waarvoor multi-factor authentication met wachtwoord-hashsynchronisatie moeten multi-factor authentication van Azure AD gebruiken. Organisaties van derden of on-premises meervoudige verificatie-methoden niet gebruiken.

* **Zakelijke continuïteit**. Met behulp van wachtwoord-hashsynchronisatie met cloud-verificatie is maximaal beschikbaar als een cloudservice die kan worden geschaald naar alle Microsoft-datacenters. Om er zeker van te zijn wachtwoord-hashsynchronisatie niet verder omlaag gedurende langere perioden, implementeert u een tweede Azure AD Connect-server in de faseringsmodus in een stand-by-configuratie.

* **Overwegingen met betrekking tot**. Op dit moment afdwingen geen wachtwoord-hashsynchronisatie direct wijzigingen in on-premises account Staten. In dit geval heeft een gebruiker toegang tot cloud-apps totdat de status van de gebruiker is gesynchroniseerd met Azure AD. Organisaties kunt strijden tegen deze beperking door het uitvoeren van een nieuwe synchronisatiecyclus nadat beheerders bulksgewijs updates voor de status van de on-premises gebruiker-account. Een voorbeeld is het uitschakelen van accounts.

> [!NOTE]
> Het wachtwoord verlopen en account vergrendelde Staten op dit moment niet worden gesynchroniseerd met Azure AD met Azure AD Connect. 

Raadpleeg [implementatie van wachtwoord-hashsynchronisatie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization) voor instructies voor implementatie.

### <a name="cloud-authentication-pass-through-authentication"></a>Cloud-verificatie: Pass through-verificatie  

* **Inspanning**. Voor Pass through-verificatie, moet u een of meer (we raden drie) lightweight agents zijn geïnstalleerd op bestaande servers. Deze agents moeten toegang hebben tot uw on-premises Active Directory Domain Services, met inbegrip van uw on-premises AD-domeincontrollers. Ze moeten uitgaande toegang tot het Internet en toegang tot uw domeincontrollers. Daarom wordt deze niet ondersteund voor het implementeren van de agents in een perimeternetwerk bevinden. 

    Pass through-verificatie is vereist voor onbeperkte netwerktoegang tot domeincontrollers. Alle netwerkverkeer is versleuteld en beperkt tot verificatieaanvragen. Zie voor meer informatie over dit proces de [grondig onderzoek van beveiliging](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-security-deep-dive) op Pass through-verificatie.

* **Gebruikerservaring**. Ter verbetering van de aanmeldingservaring voor gebruikers implementeren naadloze eenmalige aanmelding met Pass through-verificatie. Naadloze eenmalige aanmelding wordt voorkomen dat onnodige prompts nadat gebruikers zich aanmelden.

* **Geavanceerde scenario's**. Pass through-verificatie wordt de account on-premises beleid afgedwongen op het moment van aanmelden. Bijvoorbeeld: toegang is geweigerd bij het account van een on-premises gebruiker status is uitgeschakeld, vergrendeld of [wachtwoord verlopen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq#what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication) of valt buiten het aantal uren wanneer de gebruiker zich aanmeldt is toegestaan. 

    Organisaties waarvoor meervoudige verificatie met pass-through-verificatie moeten de Azure multi-factor Authentication (MFA) gebruiken. Organisaties niet een methode van derden of on-premises meervoudige verificatie gebruiken. Geavanceerde functies is vereist dat de wachtwoord-hashsynchronisatie wordt geïmplementeerd of er u pass-through-verificatie. Een voorbeeld is het rapport de referenties zijn gelekt van Identity Protection.

* **Zakelijke continuïteit**. Het is raadzaam dat u twee extra Pass through-verificatie-agents implementeren. Deze extra's zijn naast de eerste agent op de Azure AD Connect-server. Deze extra implementatie zorgt voor hoge beschikbaarheid van verificatieaanvragen. Wanneer u drie agents die zijn geïmplementeerd hebt, kan nog steeds één agent mislukken wanneer een andere agent is niet beschikbaar vanwege onderhoud. 

    Er is een ander voordeel tot het implementeren van wachtwoord-hashsynchronisatie naast Pass through-verificatie. Het fungeert als een back-verificatiemethode wanneer de primaire verificatiemethode niet meer beschikbaar is.

* **Overwegingen met betrekking tot**. Wanneer de agents de referenties van een gebruiker vanwege een fout aanzienlijke on-premises kunnen niet worden gevalideerd, kunt u wachtwoord-hashsynchronisatie als een back-verificatiemethode voor Pass through-verificatie. Failover naar wachtwoord-hashsynchronisatie niet automatisch plaatsvindt en moet u Azure AD Connect gebruiken om over te schakelen van de methode voor eenmalige aanmelding handmatig. 

    Voor andere overwegingen op Pass through-verificatie, met inbegrip van alternatieve ID-ondersteuning, Zie [Veelgestelde vragen over](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq).

Raadpleeg [Pass through-verificatie implementeren](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) voor instructies voor implementatie.

### <a name="federated-authentication"></a>Federatieve verificatie

* **Inspanning**. Een systeem federatieve verificatie, is afhankelijk van een extern vertrouwde systeem om gebruikers te verifiëren. Sommige bedrijven willen hun bestaande federatieve system-investering met hun Azure AD-oplossing voor hybride identiteit gebruiken. Het onderhoud en beheer van het systeem federatieve valt buiten het besturingselement van Azure AD. Het is aan de organisatie met behulp van de federatieve systeem om te controleren of deze veilig geïmplementeerd en de verificatie-belasting kan worden verwerkt. 

* **Gebruikerservaring**. De gebruikerservaring van federatieve verificatie, is afhankelijk van de implementatie van de functies, de topologie en de configuratie van de federation-serverfarm. Sommige organisaties moeten deze flexibiliteit aan te passen en de toegang tot de federation-serverfarm op basis van hun beveiligingsvereisten configureren. Het is bijvoorbeeld mogelijk om intern verbonden gebruikers en apparaten aan te melden bij gebruikers automatisch, zonder dat ze wordt gevraagd om referenties te configureren. Deze configuratie werkt omdat ze al aangemeld bij hun apparaten. Indien nodig, moeilijker sommige geavanceerde beveiligingsfuncties aanmeldingsproces voor gebruikers.

* **Geavanceerde scenario's**. Een oplossing voor federatieve verificatie is meestal vereist wanneer klanten hebben een vereiste voor de verificatie die Azure AD biedt geen ondersteuning voor systeemeigen. Gedetailleerde informatie om u te helpen [kiest u de optie rechts aanmelding](https://blogs.msdn.microsoft.com/samueld/2017/06/13/choosing-the-right-sign-in-option-to-connect-to-azure-ad-office-365/). Houd rekening met de volgende algemene vereisten:

    * Verificatie waarvoor smartcards of certificaten.
    * On-premises MFA-servers of meervoudige providers van derden.
    * Verificatie met behulp van oplossingen voor verificatie van derden. Zie de [federatiecompatibiliteitslijst van Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility).
    * Meld u op dat vereist een sAMAccountName, bijvoorbeeld domein\gebruikersnaam, in plaats van een UPN (User Principal Name), bijvoorbeeld user@domain.com.

* **Zakelijke continuïteit**. Federatieve systemen moeten doorgaans een matrix met load balancing van servers, bekend als een farm. Deze farm is geconfigureerd in een interne netwerk en de topologie van de perimeter-netwerk om te hoge beschikbaarheid voor verificatieaanvragen.

    Wachtwoord-hashsynchronisatie samen met federatieve verificatie als een back-verificatiemethode implementeren wanneer de primaire verificatiemethode niet meer beschikbaar is. Een voorbeeld daarvan is wanneer de on-premises servers zijn niet beschikbaar. Sommige organisaties grote onderneming vereist een federatieoplossing voor de ondersteuning van meerdere Internet inkomend toegangspunten geconfigureerd met geo-DNS voor lage latentie verificatieaanvragen.

* **Overwegingen met betrekking tot**. Federatieve systemen moeten doorgaans een meer aanzienlijke investering in on-premises infrastructuur. De meeste organisaties Kies deze optie als er al een on-premises Federatie-investering. En als het een sterke bedrijfsvereiste aanbieder van één identiteit gebruiken. Federatie is complexer om te werken en op te lossen vergeleken met oplossingen voor cloud-verificatie.

Voor een nonroutable-domein dat kan worden geverifieerd in Azure AD, moet u extra configuratie voor het implementeren van gebruiker-ID aanmelden. Deze vereiste staat bekend als alternatieve aanmeldings ID-ondersteuning. Zie [Configuring Alternate Login ID](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) voor beperkingen en vereisten. Als u gebruiken van een derde partij multi-factor authentication-provider met Federatie wilt, controleert u of dat de provider ondersteunt WS-Trust zodat apparaten worden gekoppeld aan Azure AD.

Raadpleeg [Federation-Servers implementeren](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/deploying-federation-servers) voor instructies voor implementatie.

> [!NOTE] 
> Wanneer u uw Azure AD-oplossing voor hybride identiteit implementeert, moet u een van de ondersteunde topologieën van Azure AD Connect implementeren. Meer informatie over ondersteunde en niet-ondersteunde configuraties op [topologieën voor Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-topologies).

## <a name="architecture-diagrams"></a>Architectuurdiagrammen

De architectuur op hoog niveau onderdelen die vereist zijn voor elke methode voor netwerkverificatie die met uw Azure AD-oplossing voor hybride identiteit kunt u een overzicht van de volgende diagrammen. Ze bieden een overzicht om te helpen bij het vergelijken van de verschillen tussen de oplossingen.

* De eenvoud van een oplossing voor synchronisatie van wachtwoord-hash:

    ![Azure AD voor hybride identiteit met wachtwoord-hashsynchronisatie](media/azure-ad/azure-ad-authn-image2.png)

* De agentvereisten van Pass through-verificatie:

    ![Azure AD voor hybride identiteit met Pass through-verificatie](media/azure-ad/azure-ad-authn-image3.png)

* Onderdelen die vereist zijn voor Federatie in uw perimeternetwerk en het interne netwerk van uw organisatie:

    ![Azure AD voor hybride identiteit met federatieve verificatie](media/azure-ad/azure-ad-authn-image4.png)

## <a name="comparing-methods"></a>Vergelijking van methoden

|Overweging|Wachtwoord-hashsynchronisatie en naadloze eenmalige aanmelding|Pass through-verificatie en naadloze eenmalige aanmelding|Federatie met AD FS|
|:-----|:-----|:-----|:-----|
|Waar wordt authenticatie uitgevoerd?|In de cloud|In de cloud na de uitwisseling van een beveiligd-wachtwoordverificatie verificatie met de on-premises authentication-agent|On-premises|
|Wat zijn de vereisten van de on-premises server buiten de provisioning-systeem: Azure AD Connect?|Geen|Een server voor elke extra verificatie-agent|Twee of meer AD FS-servers<br><br>Twee of meer WAP-servers in het perimeternetwerk/DMZ-netwerk|
|Wat zijn de vereisten voor on-premises Internet en netwerken buiten de provisioning-systeem?|Geen|[Uitgaande toegang tot Internet](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start) vanaf de servers met verificatie-agents|[Binnenkomende toegang tot Internet](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/ad-fs-requirements) aan de WAP-servers in het perimeternetwerk<br><br>Inkomend verkeer van toegang tot het netwerk naar AD FS-servers van WAP-servers in het perimeternetwerk<br><br>Netwerktaakverdeling|
|Is er een SSL-certificaat is vereist?|Nee|Nee|Ja|
|Is er een oplossing voor statuscontrole?|Niet vereist|De status van de agent is geleverd door [Azure Active Directory-beheercentrum](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-pass-through-authentication)|[Azure AD Connect Health (Engelstalig)](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs)|
|Krijg de gebruikers eenmalige aanmelding bronnen in de cloud van apparaten binnen het bedrijfsnetwerk domein?|Ja, met [naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)|Ja, met [naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)|Ja|
|Welke typen aanmelden worden ondersteund?|UserPrincipalName + wachtwoord<br><br>Geïntegreerde Windows-verificatie met behulp van [naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)<br><br>[Alternatieve aanmeldings-ID](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom)|UserPrincipalName + wachtwoord<br><br>Geïntegreerde Windows-verificatie met behulp van [naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)<br><br>[Alternatieve aanmeldings-ID](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq)|UserPrincipalName + wachtwoord<br><br>sAMAccountName + wachtwoord<br><br>Geïntegreerde Windows-verificatie<br><br>[Certificaat- en smart card-verificatie](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-user-certificate-authentication)<br><br>[Alternatieve aanmeldings-ID](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id)|
|Is Windows Hello voor bedrijven ondersteund?|[Model van de belangrijkste vertrouwensrelatie](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Model van de vertrouwensrelatie certificaat met Intune](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Model van de belangrijkste vertrouwensrelatie](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Model van de vertrouwensrelatie certificaat met Intune](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Model van de belangrijkste vertrouwensrelatie](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Model van de vertrouwensrelatie certificaat](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-key-trust-adfs)|
|Wat zijn de opties voor meervoudige verificatie?|[Azure MFA](https://docs.microsoft.com/azure/multi-factor-authentication/)|[Azure MFA](https://docs.microsoft.com/azure/multi-factor-authentication/)|[Azure MFA](https://docs.microsoft.com/azure/multi-factor-authentication/)<br><br>[Azure MFA-server](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)<br><br>[Externe MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs)|
|De status van de gebruiker-account worden ondersteund?|Uitgeschakelde accounts<br>(maximaal 30 minuten vertraging)|Uitgeschakelde accounts<br><br>Account vergrendeld<br><br>Wachtwoord is verlopen<br><br>Aanmelden-uren|Uitgeschakelde accounts<br><br>Account vergrendeld<br><br>Wachtwoord is verlopen<br><br>Aanmelden-uren|
|Wat zijn de opties voor voorwaardelijke toegang?|[Voorwaardelijke toegang van Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)|[Voorwaardelijke toegang van Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)|[Voorwaardelijke toegang van Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)<br><br>[AD FS claimregels](https://adfshelp.microsoft.com/AadTrustClaims/ClaimsGenerator)|
|Blokkeert verouderde protocollen die worden ondersteund?|[Ja](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication)|[Ja](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication)|[Ja](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/access-control-policies-w2k12)|
|Kunt u het logo, afbeelding en beschrijving op de aanmeldingspagina's aanpassen?|[Ja, met Azure AD Premium](https://docs.microsoft.com/azure/active-directory/customize-branding)|[Ja, met Azure AD Premium](https://docs.microsoft.com/azure/active-directory/customize-branding)|[Ja](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-management#customlogo)|
|Welke geavanceerde scenario's worden ondersteund?|[Slimme wachtwoord vergrendelen](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords)<br><br>[Gelekte referenties rapporten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events)|[Slimme wachtwoord vergrendelen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-smart-lockout)|Meerdere locaties met lage latentie verificatiesysteem<br><br>[AD FS-extranetvergrendeling](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-lockout-protection)<br><br>[Integratie met derden identiteitssystemen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility)|

## <a name="recommendations"></a>Aanbevelingen
Uw identiteitssysteem zorgt ervoor dat uw gebruikers toegang tot cloud-apps en de line-of-business-apps die u migreert en beschikbaar in de cloud maken. Om te voorkomen dat gemachtigde gebruikers productief en ongewenste actoren buiten de gevoelige gegevens van uw organisatie, verificatie controleert de toegang tot apps.

Gebruik of inschakelen van wachtwoord-hashsynchronisatie voor welke verificatiemethode die u, om de volgende redenen kiest:

1. **Hoge beschikbaarheid en herstel na noodgevallen**. Pass through-verificatie en Federatie, is afhankelijk van on-premises infrastructuur. De voetafdruk van de on-premises bevat de serverhardware voor Pass through-verificatie en netwerken de Pass through-verificatie-agents vereist. Voor Federatie is de voetafdruk van de on-premises nog grotere. Dit is vereist voor servers in uw perimeternetwerk op verificatieaanvragen proxy en de interne federatieservers. 

    Om te voorkomen dat een single point of storingen, redundante servers te implementeren. Vervolgens worden verificatieaanvragen altijd verwerkt als een onderdeel is mislukt. Zowel Pass through-verificatie en Federatie, ook zijn afhankelijk van de domeincontrollers om te reageren op aanvragen voor authenticatie, kunnen ook mislukken. Veel van deze onderdelen moeten de onderhoud om te blijven in orde. Storingen worden waarschijnlijk wanneer onderhoud is niet gepland en correct is geïmplementeerd. Vermijd storingen met behulp van wachtwoord-hashsynchronisatie omdat de Microsoft Azure AD-cloudservice verificatie meegroeit en altijd beschikbaar is.

2. **On-premises storing survival**.  De gevolgen van een on-premises storing veroorzaakt door een cyberaanval of ramp kunnen aanzienlijke, variërend van reputatie merk schade tot een paralyzed organisatie kan niet te bekommeren om de aanval zijn. Onlangs zijn veel organisaties het slachtoffer geworden van malware-aanvallen, met inbegrip van gerichte ransomware, waardoor hun on-premises servers omlaag gaan. Als Microsoft klanten omgaan met dit soort aanvallen helpt, ziet deze twee categorieën van organisaties:

   * Organisaties die eerder is ingeschakeld op de wachtwoord-hashsynchronisatie gewijzigd de verificatiemethode voor het gebruik van wachtwoord-hashsynchronisatie. Ze zijn weer online binnen een paar uur. Met behulp van toegang tot e-mail via Office 365, hebben ze voor het oplossen van problemen en andere werkbelastingen cloud-gebaseerde openen.

   * Organisaties die niet eerder synchronisatie van wachtwoordhashes inschakelen moest gebruik te maken van niet-vertrouwde externe consumenten-e-mailsystemen voor communicatie en problemen oplossen. In deze gevallen nam ze weken of meer opnieuw actief zijn.

3. **Identiteitsbeveiliging**. Een van de beste manieren om gebruikers in de cloud te beschermen is Azure AD Identity Protection. Microsoft controleert voortdurend Internet voor de gebruiker en wachtwoord bevat dat ongewenste actoren verkopen en beschikbaar op de donkere web maken. Azure AD kunt u deze informatie gebruiken om te controleren of als een van de gebruikersnamen en wachtwoorden in uw organisatie zijn aangetast. Het is daarom essentieel om in te schakelen van wachtwoord-hashsynchronisatie, ongeacht welke verificatiemethode die u gebruikt, of die gefedereerd of Pass through-verificatie. Gelekte referenties worden weergegeven als een rapport. Gebruik deze informatie om te blokkeren of afdwingen dat gebruikers hun wachtwoord moeten wijzigen wanneer ze proberen zich aanmelden met gestolen wachtwoorden.

Ten slotte op [Gartner](https://info.microsoft.com/landingIAMGartnerreportregistration.html), Microsoft heeft de meest uitgebreide set functies voor identiteits- en beheer. Microsoft verwerkt [450 miljard verificatieaanvragen](https://www.microsoft.com/en-us/security/intelligence-report) elke maand om toegang te bieden tot duizenden SaaS-toepassingen zoals Office 365 vanaf vrijwel elk apparaat. 

## <a name="conclusion"></a>Conclusie

In dit artikel bevat een overzicht van de verschillende verificatie-opties die organisaties kunnen configureren en implementeren ter ondersteuning van toegang tot cloud-apps. Organisaties kunnen om te voldoen aan verschillende bedrijven, beveiliging en technische vereisten, ervoor kiezen tussen de synchronisatie van wachtwoordhashes, Pass-through-verificatie en Federatie. 

U kunt elke methode voor netwerkauthenticatie. Biedt de inspanningen om de oplossing en de gebruikerservaring van het aanmeldingsproces implementeren, het adres van uw zakelijke vereisten? Evalueren of uw organisatie nodig heeft voor de geavanceerde scenario's en functies voor bedrijfscontinuïteit van elke verificatiemethode. Ten slotte, evalueren de overwegingen van elke verificatiemethode. Een van deze te voorkomen dat u bij de implementatie van uw keuze?

## <a name="next-steps"></a>Volgende stappen

In de wereld van vandaag, bedreigingen aanwezig 24 uur per dag en worden geleverd vanaf elke locatie. De juiste verificatiemethode implementeren, en wordt uw beveiligingsrisico's te beperken en uw identiteiten beveiligen.

[Aan de slag](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad) met Azure AD en de juiste verificatie-oplossing voor uw organisatie implementeert.

Als u erover denkt over het migreren van federatieve verificatie in de cloud, meer informatie over [wijzigen van de aanmeldingsmethode](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-user-signin#changing-the-user-sign-in-method). Gebruiken om te plannen en implementeren van de migratie, [deze implementatie projectplannen](http://aka.ms/deploymentplans).
