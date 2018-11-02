---
title: Veelgestelde vragen over Azure Active Directory B2C | Microsoft Docs
description: Veelgestelde vragen (FAQ) over Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: d17e7abbb3b6e4f091e19b8ab6834461dedffc71
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50914767"
---
# <a name="azure-ad-b2c-frequently-asked-questions-faq"></a>Azure AD B2C: Veelgestelde vragen (FAQ) 
Deze pagina vindt u antwoorden op veelgestelde vragen over de Azure Active Directory (Azure AD) B2C. Houd regelmatig op updates controleren.

### <a name="why-cant-i-access-the-azure-ad-b2c-extension-in-the-azure-portal"></a>Waarom geen ik toegang tot de Azure AD B2C-extensie in de Azure-portal?
Er zijn twee gangbare redenen voor de Azure AD-extensie waarom niet voor u werkt is.  Azure AD B2C vereist uw gebruikersrol in de map globale beheerder.  Neem contact op met uw beheerder als u denkt dat u toegang moet hebben.  Als u globale beheerder-bevoegdheden hebt, zorg ervoor dat u zich in een Azure AD B2C-map en niet in een Azure Active Directory-map.  Ziet u instructies voor het [het maken van een Azure AD B2C-directory](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant).

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Kan ik Azure AD B2C-functies gebruiken in mijn bestaande, op basis van een werknemer Azure AD-tenant?
Azure AD en Azure AD B2C zijn afzonderlijke productaanbod en kunnen niet worden gecombineerd in dezelfde tenant.  Een Azure AD-tenant vertegenwoordigt een organisatie.  Een Azure AD B2C-tenant vertegenwoordigt een verzameling van identiteiten die worden gebruikt met relying party toepassingen.  Met aangepaste beleidsregels (in openbare preview), kan Azure AD B2C federeren naar Azure AD de verificatie van de werknemers toestaan in een organisatie.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Kan ik Azure AD B2C gebruiken voor aanmelden bij sociale media (Facebook en Google +) in Office 365?
Azure AD B2C kan niet worden gebruikt om gebruikers te verifiëren voor Microsoft Office 365.  Azure AD is Microsoft's oplossing voor het beheren van toegang van werknemers tot SaaS-apps en functies die zijn ontworpen voor dit doel, zoals licentie- en voorwaardelijke toegang heeft.  Azure AD B2C biedt een platform voor identiteits- en toegangsbeheer voor het bouwen van webtoepassingen en mobiele toepassingen.  Wanneer Azure AD B2C is geconfigureerd om te federeren met een Azure AD-tenant, beheert de Azure AD-tenant toegang van werknemers tot toepassingen die afhankelijk van Azure AD B2C zijn.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Wat zijn lokale accounts in Azure AD B2C? Hoe verschillen ze van werk-of schoolaccounts in Azure AD?
In een Azure AD-tenant, de gebruikers die deel uitmaken van de tenant-aanmelding met een e-mailadres van het formulier `<xyz>@<tenant domain>`.  De `<tenant domain>` is een van de geverifieerde domeinen in de tenant of in de eerste `<...>.onmicrosoft.com` domein. Dit type account is een account voor werk of school.

In een Azure AD B2C-tenant, de meeste apps wilt dat de gebruiker om aan te melden met een willekeurige e-mailadres (bijvoorbeeld joe@comcast.net, bob@gmail.com, sarah@contoso.com, of jim@live.com). Dit type account is een lokaal account.  We ondersteunen ook willekeurige gebruikersnamen als lokale accounts (bijvoorbeeld: Jan, bob, sarah of jim). U kunt een van deze twee typen van de lokale account bij het configureren van id-providers voor Azure AD B2C in Azure portal. In uw Azure AD B2C-tenant, klikt u op **id-providers** en selecteer vervolgens **gebruikersnaam** onder lokale accounts. 

Gebruikersaccounts voor toepassingen moeten altijd worden gemaakt via een registratiebeleid, meld u aan of het teken-in het beleid of met behulp van de Azure AD Graph API. Gebruikersaccounts die zijn gemaakt in Azure portal worden alleen gebruikt voor het beheren van de tenant.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Welke sociale id-providers bieden u nu ondersteuning? Welke wilt u in de toekomst ondersteunen?
We ondersteunen momenteel Facebook, Google +, LinkedIn, Amazon, Twitter (preview), WeChat (preview), Weibo (preview) en q (Preview). Ondersteuning voor andere populaire sociale id-providers op basis van vraag van klanten, zullen we toevoegen.

Azure AD B2C heeft ook ondersteuning toegevoegd voor [aangepast beleid](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom).  Deze [aangepast beleid](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom) toestaan van een ontwikkelaar van hun eigen om beleid te maken die met een id-provider die ondersteuning biedt voor [OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) of SAML. 

Aan de slag met aangepast beleid door het uitchecken van onze [aangepast beleid beginnerspakket](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack).

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Kan ik bereiken voor het verzamelen van informatie over gebruikers uit verschillende sociale id-providers configureren?
Nee, maar deze functie is op ons schema groter. De standaard-bereiken die wordt gebruikt voor onze ondersteunde set sociale id-providers zijn:

* Facebook: e-mailadres
* Google +: e-mailadres
* Microsoft-account: openid e-mailprofiel
* Amazon: profiel
* LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Zijn mijn toepassing uitgevoerd op Azure voor het werken met Azure AD B2C?
Nee, u kunt hosten op uw toepassing een willekeurige plaats (in de cloud of on-premises). Nodig is om te communiceren met Azure AD B2C, is de mogelijkheid om te verzenden en ontvangen van HTTP-aanvragen op openbaar toegankelijke eindpunten.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Ik heb meerdere Azure AD B2C-tenants. Hoe kan ik ze beheren in Azure portal?
Voordat u het 'Azure AD B2C' in het menu links van de Azure-portal opent, moet u overschakelen naar de map die u wilt beheren.  Schakelen tussen mappen door te klikken op uw identiteit in de rechterbovenhoek van de Azure-portal en kies vervolgens een map in de vervolgkeuzelijst die wordt weergegeven.  Zie voor stapsgewijze met afbeeldingen, [navigeren naar Azure AD B2C-instellingen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Hoe aanpassen e-mails voor domeinverificatie (de inhoud en de ' uit: ' veld) die is verzonden door Azure AD B2C?
U kunt de [functie huisstijl van bedrijf](../active-directory/fundamentals/customize-branding.md) om aan te passen van de inhoud van verificatie-e-mailberichten. Specifiek, kunnen deze twee elementen van het e-mailbericht worden aangepast:

* **Logo banner**: wordt weergegeven in de rechterbenedenhoek.
* **De achtergrondkleur**: wordt weergegeven aan de bovenkant.

    ![Schermopname van een aangepaste verificatie-e-mailbericht](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

De e-handtekening bevat de naam van de Azure AD B2C-tenant die u hebt opgegeven tijdens het maken van de Azure AD B2C-tenant. U kunt de naam van deze instructies wijzigen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als globale beheerder.
1. Open de **Azure Active Directory** blade.
1. Klik op de **eigenschappen** tabblad.
1. Wijzig de **naam** veld.
1. Klik bovenaan de pagina op **Opslaan**.

Er is momenteel geen manier om te wijzigen de ' uit: ' op het e-mailbericht. Stemmen op [feedback.azure.com](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails) u geïnteresseerd bent in de hoofdtekst van het verificatie-e-mailbericht aan te passen.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Hoe Migreer ik mijn bestaande gebruikersnamen, wachtwoorden en -profielen van mijn database naar Azure AD B2C?
De Azure AD Graph API kunt u het hulpprogramma voor migratie van schrijven. Zie de [gebruikershandleiding voor de migratie](active-directory-b2c-user-migration.md) voor meer informatie.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Welke wachtwoordbeleid wordt gebruikt voor lokale accounts in Azure AD B2C?
Het wachtwoordbeleid van Azure AD B2C voor lokale accounts is gebaseerd op het beleid voor Azure AD. Azure AD B2C de gebruikersregistratie, meld u aan of aanmelden en het wachtwoord opnieuw instellen van beleidsregels gebruikt de sterkte 'sterk' wachtwoord en de wachtwoorden niet verloopt. Lees de [Azure AD-wachtwoordbeleid](https://msdn.microsoft.com/library/azure/jj943764.aspx) voor meer informatie. Zie voor meer informatie over accountvergrendelingen en wachtwoorden [bedreigingen voor resources en -gegevens in Azure Active Directory B2C beheert](active-directory-b2c-reference-threat-management.md).

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Kan ik Azure AD Connect gebruiken voor het migreren van consumentidentiteiten uit die zijn opgeslagen op mijn on-premises Active Directory naar Azure AD B2C?
Nee, Azure AD Connect is niet ontworpen om te werken met Azure AD B2C. Overweeg het gebruik van de [Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) voor gebruikersmigratie.  Zie de [gebruikershandleiding voor de migratie](active-directory-b2c-user-migration.md) voor meer informatie.

### <a name="can-my-app-open-up-azure-ad-b2c-pages-within-an-iframe"></a>Kan mijn app openen om Azure AD B2C-pagina's in een iFrame?
Nee, uit veiligheidsoverwegingen, Azure AD B2C-pagina's niet openen in een iFrame.  Onze service communiceert met de browser om te voorkomen dat iFrames.  De beveiligingscommunity in het algemeen en de OAUTH2-specificatie raden het gebruik van iFrames voor identiteitservaringen vanwege het risico van steunpunten voor de klik.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Werkt Azure AD B2C met systemen zoals Microsoft Dynamics CRM?
Integratie met Microsoft Dynamics 365-Portal is beschikbaar.  Zie [Dynamics 365-Portal configureren voor Azure AD B2C gebruiken voor verificatie](https://docs.microsoft.com/dynamics365/customer-engagement/portals/azure-ad-b2c).

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Azure AD B2C biedt werk met SharePoint on-premises 2016 of lager?
Azure AD B2C is niet bedoeld voor het SharePoint externe partner delen scenario; Zie [Azure AD B2B](https://cloudblogs.microsoft.com/enterprisemobility/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview/) in plaats daarvan.

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Moet ik Azure AD B2C of B2B gebruiken om externe identiteiten te beheren?
In dit artikel meer informatie over [externe identiteiten](../active-directory/active-directory-b2b-compare-external-identities.md) voor meer informatie over het toepassen van de desbetreffende functies op uw externe id-scenario's.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Welke rapportage- en controle van functies Azure AD B2C biedt? Zijn ze hetzelfde als in Azure AD Premium?
Nee, Azure AD B2C ondersteunt niet de dezelfde set met rapporten als Azure AD Premium. Er zijn echter veel commonalities:

* **-Aanmeldingsrapporten** een record van elke aanmelding bieden met minder details.
* **Controlerapporten** omvatten zowel beheerdersactiviteiten bewerkstelligen als activiteit van de toepassing. 
* **Gebruiksrapporten** zijn onder andere het aantal gebruikers, het aantal aanmeldingen en volume van MFA. 

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Kan ik lokalisatie van de gebruikersinterface van pagina's die worden geleverd door Azure AD B2C? Welke talen worden ondersteund?
Ja.  Meer informatie over [taalaanpassing](active-directory-b2c-reference-language-customization.md), deze bevindt zich in openbare preview.  We bieden vertalingen voor 36 talen en kunt u een willekeurige tekenreeks om aan uw behoeften te overschrijven.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Kan ik mijn eigen URL's op mijn zich kunnen registreren en aanmelden pagina's die worden aangeboden door Azure AD B2C gebruiken? Bijvoorbeeld, kan ik de URL van login.microsoftonline.com naar login.contoso.com wijzigen?
Momenteel niet. Deze functie is op ons schema groter. Controleren van uw domein in de **domeinen** dit doel niet wordt voltooid door tabblad in de Azure-portal.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Hoe kan ik mijn Azure AD B2C-tenant verwijderen?
Volg deze stappen voor het verwijderen van uw Azure AD B2C-tenant:

1. Volg deze stappen om [gaat u naar Azure AD B2C-instellingen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in Azure portal.
1. Navigeer naar de **toepassingen**, **id-providers**, en **alle beleidsregels** en verwijder alle vermeldingen in elk van deze.
1. Nu aanmelden bij de [Azure-portal](https://portal.azure.com/) als beheerder van het abonnement. (Gebruik de dezelfde werk- of schoolaccount of hetzelfde Microsoft-account die u hebt gebruikt om u te registreren voor Azure.)
1. Switch voor de Azure AD B2C-tenant die u wilt verwijderen.
2. Navigeer naar de Active Directory-menu aan de linkerkant.
3. Selecteer **Gebruikers en groepen**.
4. Selecteer elke gebruiker in inschakelen (de beheerder van abonnement-gebruiker die u momenteel bent aangemeld als uitsluiten). Klik op **verwijderen** aan de onderkant van de pagina en klik op **Ja** wanneer hierom wordt gevraagd.
5. Klik op de **App-registraties**.
6. Selecteer de toepassing met de naam **b2c-extensions-app**. Klik op **verwijderen** en klikt u op **Ja** wanneer hierom wordt gevraagd.
7. Selecteer **Overzicht**.
8. Klik op **verwijderen directory**. Volg de instructies op het scherm voor het voltooien van het proces.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Kan ik Azure AD B2C als onderdeel van Enterprise Mobility Suite krijgen?
Nee, Azure AD B2C is een betalen per gebruik van de Azure-service en is geen onderdeel van Enterprise Mobility Suite.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Hoe rapporteer ik problemen met Azure AD B2C?
Zie [bestand ondersteuningsaanvragen voor Azure Active Directory B2C](active-directory-b2c-support.md).
