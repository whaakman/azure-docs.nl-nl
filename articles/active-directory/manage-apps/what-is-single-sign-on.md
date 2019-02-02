---
title: Single sign-on bij toepassingen - Azure Active Directory | Microsoft Docs
description: Informatie over het kiezen van een methode voor eenmalige aanmelding bij het configureren van toepassingen in Azure Active Directory (Azure AD). Eenmalige aanmelding gebruikt, zodat gebruikers niet hoeft te onthouden de wachtwoorden voor elke toepassing en te vereenvoudigen het beheer van accountbeheer.
services: active-directory
author: barbkess
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: barbkess
ms.reviewer: arvindh
ms.openlocfilehash: 8216eb9c076111bfa70633b24f36ac97b0ee319a
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55664026"
---
# <a name="single-sign-on-to-applications-in-azure-active-directory"></a>Eenmalige aanmelding voor toepassingen in Azure Active Directory
Eenmalige aanmelding (SSO) voegt beveiliging en gemak wanneer gebruikers zich aanmelden bij toepassingen in Azure Active Directory (Azure AD). Dit artikel beschrijft de methoden voor eenmalige aanmelding en helpt u bij het kiezen van de meest geschikte methode voor eenmalige aanmelding bij het configureren van uw toepassingen.

- **Met eenmalige aanmelding**, gebruikers zich in één keer met één account toegang tot het domein van het apparaat, bedrijfsresources, software als een service (SaaS)-toepassingen en webtoepassingen. Na het aanmelden, kan de gebruiker toepassingen starten vanuit de Office 365-portal of de MyApps van Azure AD-toegangspaneel. Beheerders kunnen communicatiemiddelen zijn gecentraliseerd beheer van gebruikersaccounts, en automatisch toevoegen of verwijderen van toegang tot toepassingen op basis van groepslidmaatschap. 

- **Zonder eenmalige aanmelding**, gebruikers moeten onthouden de wachtwoorden van toepassingsspecifieke en aanmelden bij elke toepassing. IT-personeel moet maken en bijwerken van gebruikersaccounts voor elke toepassing, zoals Office 365, Box en Salesforce. Gebruikers moeten hun wachtwoord onthouden, plus de tijd om aan te melden bij elke toepassing.

## <a name="choosing-a-single-sign-on-method"></a>Een methode voor eenmalige aanmelding kiezen

Er zijn verschillende manieren om een toepassing voor eenmalige aanmelding te configureren. Het kiezen van een methode voor eenmalige aanmelding, is afhankelijk van hoe de toepassing is geconfigureerd voor verificatie. 

- Cloud-toepassingen kunnen OpenID Connect, OAuth, SAML, op basis van wachtwoorden, gekoppelde, of uitgeschakelde methoden gebruiken voor eenmalige aanmelding. 
- On-premises toepassingen kunnen gebruiken op basis van wachtwoorden, geïntegreerde Windows-verificatie, op basis van een koptekst, gekoppelde of uitgeschakeld methoden voor eenmalige aanmelding. De on-premises opties werken wanneer toepassingen zijn geconfigureerd voor de toepassingsproxy.

Deze stroomdiagram kunt u bepalen welke methode voor eenmalige aanmelding wordt aanbevolen voor uw situatie. 

![Kies de methode voor eenmalige aanmelding](./media/what-is-single-sign-on/choose-single-sign-on-method.png)

De volgende tabel geeft een overzicht van de methoden voor eenmalige aanmelding, en koppelingen naar meer informatie. 

| Methode voor eenmalige aanmelding | Toepassingstypen | Wanneer gebruikt u dit? |
| :------ | :------- | :----- |
| [OpenID Connect en OAuth](#openid-connect-and-oauth) | Alleen cloud | OpenID Connect en OAuth gebruiken bij het ontwikkelen van een nieuwe toepassing. Dit protocol vereenvoudigt de configuratie van toepassing, is eenvoudig te gebruiken SDK's en kan uw toepassing MS Graph gebruiken.
| [SAML](#saml-sso) | Alleen cloud | Kies SAML indien mogelijk voor bestaande toepassingen die niet met OpenID Connect of OAuth. SAML werkt voor toepassingen die worden geverifieerd met een van de SAML-protocollen.|
| [Op basis van wachtwoorden](#password-based-sso) | cloud en on-premises | Kiezen op basis van wachtwoorden wanneer de toepassing wordt geverifieerd met gebruikersnaam en wachtwoord. Wachtwoord gebaseerde eenmalige aanmelding kunt u beveiligde toepassingen wachtwoorden worden opgeslagen en opnieuw afspelen met behulp van een uitbreiding van web browser of mobiele app. Deze methode maakt gebruik van het bestaande aanmeldingsproces geleverd door de toepassing, maar Hiermee kan een beheerder om de wachtwoorden te beheren. |
| [Gekoppelde](#linked-sso) | cloud en on-premises | Kies gekoppelde eenmalige aanmelding bij de toepassing is geconfigureerd voor eenmalige aanmelding in een andere id-provider-service. Deze optie toevoegen niet eenmalige aanmelding aan de toepassing. De toepassing mogelijk echter al eenmalige aanmelding geïmplementeerd met behulp van een andere service, zoals Active Directory Federation Services.|
| [Uitgeschakeld](#disabled-sso) | cloud en on-premises | Kies uitgeschakelde eenmalige aanmelding wanneer de app niet gereed om te worden geconfigureerd voor eenmalige aanmelding. Gebruikers moeten hun gebruikersnaam en wachtwoord invoeren telkens wanneer ze deze toepassing starten.|
| [Geïntegreerde Windows-verificatie (IWA)](#integrated-windows-authentication-iwa-sso) | alleen on-premises | Kies IWA eenmalige aanmelding voor toepassingen die gebruikmaken van [geïntegreerde Windows-verificatie (IWA)](/aspnet/web-api/overview/security/integrated-windows-authentication), of de claimbewuste toepassingen. Voor IWA, wordt in de Application Proxy connectors Kerberos-beperkte delegatie (KCD) gebruiken om te verifiëren voor toegang tot de toepassing. | 
| [Op basis van koptekst](#header-based-sso) | alleen on-premises | Gebruik op basis van een koptekst van eenmalige aanmelding wanneer de toepassing headers voor verificatie gebruikt. Headers gebaseerde eenmalige aanmelding moet PingAccess voor Azure AD. Application Proxy maakt gebruik van Azure AD de gebruiker te verifiëren en vervolgens doorgegeven verkeer via de connector-service.  | 

## <a name="openid-connect-and-oauth"></a>OpenID Connect en OAuth
Bij het ontwikkelen van nieuwe toepassingen, kunt u de moderne protocollen, zoals OpenID Connect en OAuth gebruiken om te bereiken het beste eenmalige aanmelding mogelijk voor uw app voor meerdere apparaten. OAuth kan gebruikers of beheerders [toestemming verlenen](configure-user-consent.md) voor beveiligde bronnen zoals [MS Graph](/graph/overview). We bieden eenvoudig vast te stellen [SDK's](../develop/reference-v2-libraries.md) voor uw app, en daarnaast uw app is klaar voor gebruik [MS Graph](/graph/overview).

Zie voor meer informatie:

- [OAuth 2.0](../develop/v2-oauth2-auth-code-flow.md)
- [OpenID Connect 1.0](../develop/v2-protocols-oidc.md)
- [Azure Active Directory-Ontwikkelaarshandleiding voor](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide).

## <a name="saml-sso"></a>SAML-EENMALIGE AANMELDING
Met **eenmalige aanmelding SAML**, Azure AD verifieert de toepassing met behulp van Azure AD-account van de gebruiker. Azure AD communiceert de informatie aanmelding voor de toepassing via een verbindingsprotocol. Met SAML gebaseerde eenmalige aanmelding, kunt u gebruikers toewijzen aan de rollen van de specifieke toepassing is op basis van regels die u in de claims van SAML definieert

Kies SAML gebaseerde eenmalige aanmelding wanneer de toepassing wordt ondersteund.


SAML gebaseerde eenmalige aanmelding wordt ondersteund voor toepassingen die gebruikmaken van een van deze protocollen:

- SAML 2.0
- Webservices-federatie

Zie configureren van een toepassing voor SAML gebaseerde eenmalige aanmelding [configureren SAML gebaseerde eenmalige aanmelding](configure-single-sign-on-portal.md). Ook veel Software als een Service (SaaS)-toepassingen hebben een [toepassingsspecifieke zelfstudie](../saas-apps/tutorial-list.md) die doorloopt u de configuratie voor SAML gebaseerde eenmalige aanmelding. 

Zie voor meer informatie over het SAML-protocol, [eenmalige aanmelding in de SAML-protocol](../develop/single-sign-on-saml-protocol.md).

## <a name="password-based-sso"></a>Wachtwoord gebaseerde SSO
In op basis van wachtwoorden aanmeldings-, gebruikers zich bij de eerste keer dat ze toegang krijgen tot de toepassing met een gebruikersnaam en wachtwoord op. Na de eerste aanmelding levert Azure AD de gebruikersnaam en het wachtwoord voor de toepassing. 

Maakt gebruik van het bestaande verificatieproces geleverd door de toepassing op basis van wachtwoorden eenmalige aanmelding. Wanneer u wachtwoord eenmalige aanmelding voor een toepassing inschakelen, wordt Azure AD worden verzameld en worden veilig opgeslagen gebruikersnamen en wachtwoorden voor de toepassing. Gebruikersreferenties worden opgeslagen in een versleutelde status in de map. 

Kies één wachtwoord gebaseerde aanmelding wanneer:

- Een toepassing biedt geen ondersteuning voor eenmalige SAML-aanmelding protocol.
- Een toepassing verifieert met een gebruikersnaam en wachtwoord in plaats van tokens voor toegang en -koppen.

Wachtwoord gebaseerde eenmalige aanmelding wordt ondersteund voor alle cloud-gebaseerde toepassingen waarvoor een op HTML gebaseerde aanmeldingspagina. De gebruiker kan een van de volgende browsers gebruiken:

- Internet Explorer 11 op Windows 7 of hoger
- Microsoft Edge op Windows 10 Anniversary Edition of hoger 
- Chrome op Windows 7 of hoger, en op Mac OS X of hoger
- Firefox 26,0 in Windows XP SP2 of hoger, en op Mac OS X 10.6 of hoger

Zie configureren van een cloudtoepassing voor wachtwoord gebaseerde eenmalige aanmelding [de toepassing configureren voor eenmalige aanmelding wachtwoord](application-sign-in-problem-password-sso-gallery.md#configure-the-application-for-password-single-sign-on).

Zie configureren van een on-premises toepassing voor eenmalige aanmelding via toepassingsproxy [wachtwoord vaulting voor eenmalige aanmelding met Application Proxy](application-proxy-configure-single-sign-on-password-vaulting.md)

### <a name="how-authentication-works-for-password-based-sso"></a>De werking van verificatie voor eenmalige aanmelding op basis van wachtwoorden

Voor het verifiëren van een gebruiker aan een toepassing, Azure AD-referenties van de gebruiker opgehaald uit de map en voert deze in de aanmeldings-pagina van de toepassing.  Referenties van de gebruiker doorgegeven Azure AD veilig via een browserextensie voor web- of mobiele app. Dit proces kan een beheerder voor het beheren van de referenties van gebruiker en niet vereisen dat gebruikers hun wachtwoord te onthouden.

> [!IMPORTANT]
> De referenties zijn verscholen van de gebruiker tijdens het proces voor automatische aanmelding. De referenties zijn echter kunnen worden gedetecteerd met behulp van hulpprogramma's voor foutopsporing voor web. Gebruikers en beheerders moeten de dezelfde beleidsregels voor veiligheid volgen als de referenties zijn ingevoerd rechtstreeks door de gebruiker.

### <a name="managing-credentials-for-password-based-sso"></a>Referenties voor eenmalige aanmelding op basis van wachtwoorden beheren

Wachtwoorden voor elke toepassing kunnen ofwel worden beheerd door de Azure AD-beheerder of de gebruikers.

De referenties worden als beheerd in de Azure AD-beheerder:  

- De gebruiker hoeft niet opnieuw instellen of de gebruikersnaam en wachtwoord onthouden. De gebruiker kan toegang krijgen tot de toepassing door erop te klikken in het toegangsvenster of via een opgegeven koppeling.
- De beheerder kan doen beheertaken op de referenties. De beheerder kan bijvoorbeeld toegang tot toepassingen op basis van groepslidmaatschappen en de Werknemerstatus bijwerken.
- De beheerder kan beheerdersreferenties gebruiken voor toegang tot toepassingen die worden gedeeld door veel gebruikers. De beheerder kan bijvoorbeeld iedereen wie toegang heeft tot een toepassing toegang hebben tot een sociale media of een document-toepassing voor delen toestaan.

De referenties worden als beheerd in de eindgebruiker:

- Gebruikers kunnen hun wachtwoorden beheren door bij te werken of deze zo nodig worden verwijderd. 
- Beheerders nog kunnen steeds om in te stellen van nieuwe referenties voor de toepassing.


## <a name="linked-sso"></a>Gekoppelde eenmalige aanmelding
Gekoppelde aanmelding kan Azure AD voor eenmalige aanmelding tot een toepassing die al is geconfigureerd voor eenmalige aanmelding in een andere service. De gekoppelde toepassing kan worden weergegeven voor eindgebruikers in de Office 365-portal of Azure AD MyApps-portal. Een gebruiker kan bijvoorbeeld een toepassing die is geconfigureerd voor eenmalige aanmelding in Active Directory Federation Services 2.0 (AD FS) in de Office 365-portal starten. Extra reporting is ook beschikbaar voor de gekoppelde toepassingen die worden gestart vanuit de Office 365-portal of de Azure AD MyApps-portal. 

### <a name="linked-sso-for-application-migration"></a>Gekoppelde eenmalige aanmelding voor toepassingen migreren

Gekoppelde SSO krijgt u een consistente gebruikerservaring tijdens de migratie van toepassingen gedurende een bepaalde periode. Als u toepassingen te naar Azure Active Directory migreren bent, kunt u snel kunt publiceren koppelingen voor alle toepassingen die u van plan bent om te migreren gekoppelde eenmalige aanmelding gebruiken.  Gebruikers kunnen vinden alle bijbehorende koppelingen in de [MyApps-portal](../user-help/active-directory-saas-access-panel-introduction.md) of de [startprogramma voor toepassingen van Office 365](https://support.office.com/article/meet-the-office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a). Gebruikers kennen niet dat ze toegang krijgen tot een gekoppelde toepassing of een gemigreerde toepassing.  

Wanneer een gebruiker is geverifieerd met een gekoppelde toepassing, moet een accountrecord voor een worden gemaakt voordat de gebruiker toegang voor eenmalige aanmelding is opgegeven. Een record voor dit account wordt ingericht ofwel automatisch kan worden uitgevoerd, of deze handmatig door een beheerder kan optreden.

## <a name="disabled-sso"></a>Uitgeschakelde eenmalige aanmelding

Modus uitgeschakeld betekent dat eenmalige aanmelding wordt niet gebruikt voor de toepassing. Wanneer eenmalige aanmelding is uitgeschakeld, kunnen gebruikers moeten zich twee keer verifiëren. Eerst, gebruikers verifiëren met Azure AD en vervolgens ze zich aanmelden bij de toepassing. 

Gebruik uitgeschakeld modus voor eenmalige aanmelding:

- Als u niet klaar bent om deze toepassing integreren met Azure AD eenmalige aanmelding, of
- Als u andere aspecten van de toepassing test of
- Als een beveiligingslaag aan een on-premises toepassing die zijn vereist om gebruikers te verifiëren. En is uitgeschakeld, wordt de gebruiker moet worden geverifieerd. 

## <a name="integrated-windows-authentication-iwa-sso"></a>Geïntegreerde Windows-verificatie (IWA) eenmalige aanmelding

[Toepassingsproxy](application-proxy.md) biedt eenmalige aanmelding (SSO) voor toepassingen die gebruikmaken van [geïntegreerde Windows-verificatie (IWA)](/aspnet/web-api/overview/security/integrated-windows-authentication), of de claimbewuste toepassingen. Als uw toepassing gebruikmaakt van IWA, verifieert de Application Proxy voor de toepassing met behulp van Kerberos-beperkte delegatie (KCD). Werkt eenmalige aanmelding voor een claimbewuste toepassing die door Azure Active Directory wordt vertrouwd, omdat de gebruiker is al geverifieerd met behulp van Azure AD.

Kies één geïntegreerde Windows-aanmeldings-verificatiemodus:

- Om te voorzien van single sign-on bij een on-premises-app die wordt geverifieerd IWA. 

Zie voor meer informatie over het configureren van een on-premises app voor IWA [beperkte Kerberos-delegering voor eenmalige aanmelding voor uw toepassingen met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md). 

### <a name="how-single-sign-on-with-kcd-works"></a>Hoe eenmalige aanmelding met KCD werkt
In dit diagram wordt de stroom uitgelegd wanneer een gebruiker toegang heeft tot een on-premises toepassing die gebruikmaakt van IWA.

![Stroomdiagram van Microsoft AAD-verificatie](./media/application-proxy-configure-single-sign-on-with-kcd/AuthDiagram.png)

1. De gebruiker voert de URL voor toegang tot de on-premises toepassing via Application Proxy.
2. Application Proxy stuurt de aanvraag naar Azure AD authentication-services kunnen worden. Azure AD van toepassing op dit moment voor alle van toepassing verificatie en autorisatiebeleid, zoals meervoudige verificatie. Als de gebruiker is gevalideerd, wordt Azure AD wordt een token gemaakt en verzendt ze naar de gebruiker.
3. De gebruiker wordt het token doorgegeven aan Application Proxy.
4. Application Proxy valideert het token en de User Principal Name (UPN) opgehaald uit het token. Deze stuurt de aanvraag, de UPN en de Service Principal Name (SPN) naar de Connector via een beveiligd kanaal voor zowel geverifieerde.
5. De connector maakt gebruik van Kerberos-beperkte delegatie (KCD)-onderhandeling met de on-premises AD, het imiteren van de gebruiker om op te halen van een Kerberos-token naar de toepassing.
6. Active Directory, verzendt de Kerberos-token voor de toepassing naar de connector.
7. De connector verzendt de oorspronkelijke aanvraag naar de application server, met behulp van het Kerberos-token dat het ontvangen van AD.
8. De toepassing stuurt het antwoord op de connector, die vervolgens wordt geretourneerd naar de Application Proxy-service en ten slotte naar de gebruiker.

## <a name="header-based-sso"></a>Koptekst gebaseerde SSO

Werkt voor toepassingen die HTTP-headers voor verificatie gebruiken op basis van een koptekst van eenmalige aanmelding. Deze aanmeldingsmethode maakt gebruik van een derde partij verificatieservice PingAccess genoemd. Een gebruiker moet alleen verifiëren met Azure AD. 

Kies één headers gebaseerde aanmelding wanneer:

- De toepassingsproxy en PingAccess zijn geconfigureerd voor de toepassing

Zie configureren van verificatie op basis van een koptekst [koptekst gebaseerde verificatie voor eenmalige aanmelding met Application Proxy](application-proxy-configure-single-sign-on-with-ping-access.md). 

### <a name="what-is-pingaccess-for-azure-ad"></a>Wat is PingAccess voor Azure AD?

Met behulp van PingAccess voor Azure AD, gebruikers kunnen toegang en eenmalige aanmelding bij toepassingen die headers voor verificatie gebruiken. Application Proxy behandelt deze toepassingen, zoals een andere, met behulp van Azure AD verifiëren van toegang en vervolgens verkeer via de connector-service. Nadat verificatie heeft plaatsgevonden, zet de PingAccess-service om het Azure AD-toegangstoken in een headerindeling die wordt verzonden naar de toepassing.

Uw gebruikers wordt niet ziet u iets anders wanneer ze zich aanmelden bij het gebruik van uw zakelijke toepassingen. Ze kunnen nog steeds overal werken vanaf op elk apparaat. De Application Proxy connectors direct extern verkeer naar alle toepassingen en blijven ze automatisch verdelen.

### <a name="how-do-i-get-a-license-for-pingaccess"></a>Hoe krijg ik een licentie van PingAccess?

Omdat dit scenario wordt aangeboden via een verbinding tussen Azure AD en PingAccess, moet u licenties voor beide services. Azure AD Premium-abonnementen hebben echter een basic PingAccess-licentie die betrekking heeft op maximaal 20 toepassingen. Als u nodig hebt om meer dan 20 op basis van een header toepassingen te publiceren, kunt u een extra licentie van PingAccess aanschaffen. 

Zie [Azure Active Directory-edities](../fundamentals/active-directory-whatis.md) voor meer informatie.


## <a name="related-articles"></a>Verwante artikelen:
* [Zelfstudies voor het integreren van SaaS-toepassingen met Azure Active Directory](../saas-apps/tutorial-list.md)
* [Zelfstudie voor het configureren van eenmalige aanmelding](configure-single-sign-on-portal.md)
* [Inleiding tot beheer van toegang tot toepassingen](what-is-access-management.md)
* Downloadkoppeling: [Implementatieplan voor eenmalige aanmelding](https://aka.ms/SSODeploymentPlan).


