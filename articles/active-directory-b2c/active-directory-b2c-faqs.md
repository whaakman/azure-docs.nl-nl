---
title: Veelgestelde vragen (FAQ) - Azure AD B2C | Microsoft Docs
description: Veelgestelde vragen over Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: saeeda
manager: krassk
editor: bryanla
ms.assetid: ed33c2ca-76d0-442a-abb1-8b7b7bb92d6a
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeeda
ms.openlocfilehash: e8b28bc9ccc12b280b1746272519bd4c9ea9e4a4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-ad-b2c-frequently-asked-questions-faq"></a>Azure AD B2C: Veelgestelde vragen (FAQ) 
Deze pagina antwoorden op veelgestelde vragen over de Azure Active Directory (Azure AD) B2C. Houd regelmatig op updates controleren.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Kan ik Azure AD B2C-functies gebruiken in mijn bestaande, op basis van een werknemer Azure AD-tenant?
Azure AD en Azure AD B2C afzonderlijke producten zijn en kunnen niet worden gecombineerd in dezelfde tenant.  Een Azure AD-tenant vertegenwoordigt een organisatie.  Een Azure AD B2C-tenant vertegenwoordigt een verzameling van identiteiten die worden gebruikt met relying party-toepassingen.  Azure AD B2C kunnen met aangepast beleid (in de openbare preview) communiceren met Azure AD, zodat verificatie van werknemers in een organisatie.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Kan ik Azure AD B2C gebruiken voor aanmelden bij sociale media (Facebook en Google +) in Office 365?
Azure AD B2C kan niet worden gebruikt om gebruikers te verifiëren voor Microsoft Office 365.  Azure AD is de oplossing van Microsoft voor het beheren van werknemers toegang hebben tot de SaaS-apps en functies die zijn ontworpen voor dit doel zoals licentieverlening en voorwaardelijke toegang heeft.  Azure AD B2C biedt een platform voor identiteits- en toegangsbeheer voor het bouwen van webtoepassingen en mobiele toepassingen.  Wanneer Azure AD B2C is geconfigureerd voor het federeren van een Azure AD-tenant, beheert de Azure AD-tenant werknemers toegang hebben tot toepassingen die afhankelijk van Azure AD B2C zijn.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Wat zijn lokale accounts in Azure AD B2C? Hoe zijn ze verschilt van het werk-of schoolaccounts in Azure AD?
In een Azure AD-tenant gebruikers die deel uitmaken van de tenant die aanmelden met een e-mailadres van het formulier `<xyz>@<tenant domain>`.  De `<tenant domain>` is een van de geverifieerde domeinen in de tenant of de eerste `<...>.onmicrosoft.com` domein. Dit type account is een werk- of schoolaccount-account.

In een Azure AD B2C-tenant wilt dat de meeste apps door de gebruiker zich aanmeldt met een willekeurige e-mailadres (bijvoorbeeld joe@comcast.net, bob@gmail.com, sarah@contoso.com, of jim@live.com). Dit type account is een lokale account.  We bieden ook ondersteuning voor willekeurige gebruikersnamen als lokale accounts (bijvoorbeeld Jan, bob, sarah of jim). U kunt een van deze twee lokale accounttypen door Azure AD B2C configureren in de Azure-portal.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Welke sociale identiteitsproviders ondersteund nu? Welke u wilt u in de toekomst ondersteunen?
Wordt ondersteund Facebook, Google + LinkedIn, Amazon, Twitter (preview), WeChat (preview), Weibo (preview) en q (Preview). Er wordt ondersteuning toevoegen voor andere populaire social identiteitsproviders op basis van vraag van klanten.

Azure AD B2C is ook ondersteuning toegevoegd voor [aangepast beleid](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview-custom).  Deze [aangepast beleid](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview-custom) toestaan dat een ontwikkelaar hun eigen om beleid te maken die met een id-provider die ondersteuning biedt voor [OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) of SAML. 

Aan de slag met aangepast beleid door het uitchecken van onze [aangepast beleid starter pack](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack).

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Kan ik scopes voor meer informatie over consumenten te verzamelen van verschillende sociale id-providers configureren?
Nee, maar deze functie is op onze roadmap. De standaard-bereiken die worden gebruikt voor onze ondersteunde set sociale identiteitsproviders zijn:

* Facebook: e-mailadres
* Google +: e-mailadres
* Microsoft-account: openid e-mailprofiel
* Amazon: profiel
* LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Heeft mijn toepassing in Azure worden uitgevoerd voor het werken met Azure AD B2C?
Nee, kunt u uw toepassing overal (in de cloud of on-premises) hosten. Alle moet communiceren met Azure AD B2C is de mogelijkheid om te verzenden en ontvangen van HTTP-aanvragen op openbaar toegankelijk eindpunten.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Ik heb meerdere tenants van Azure AD B2C. Hoe kan ik ze beheren in de Azure portal?
Voordat u opent 'Azure AD B2C' in het menu links van de Azure portal, moet u overschakelen naar de map die u wilt beheren.  Overschakelen van mappen door te klikken op uw identiteit in de rechterbovenhoek van de Azure-portal en kies vervolgens een map in de vervolgkeuzelijst wordt weergegeven.  Zie voor stapsgewijze met afbeeldingen, [navigeren naar Azure AD B2C-instellingen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Hoe aanpassen bevestigingsmails (de inhoud en de ' uit: ' veld) die door Azure AD B2C verzonden?
U kunt de [functie huisstijl](../active-directory/active-directory-add-company-branding.md) voor het aanpassen van de inhoud van de verificatie-e-mailberichten. In het bijzonder kunnen deze twee elementen van de e-mail worden aangepast:

* **Logo banner**: weergegeven in de rechterbenedenhoek.
* **De achtergrondkleur**: weergegeven aan de bovenkant.

    ![Schermopname van een aangepaste bevestigingsmail](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

De e-handtekening bevat van de B2C-tenant-naam die u hebt opgegeven tijdens het maken van de B2C-tenant. U kunt de naam van deze instructies wijzigen:

1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com/) als abonnementsbeheerder.
1. Navigeer naar uw B2C-tenant.
1. Klik op het tabblad **Configureren**.
1. Wijzig de **naam** veld onder de **mapeigenschappen** sectie.
1. Klik op **Opslaan** onder aan de pagina.

Er is momenteel geen manier om de ' uit: ' op het e-mailbericht. Stemmen over [feedback.azure.com](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails) u geïnteresseerd bent in de hoofdtekst van de bevestigingsmail aanpassen.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Hoe kan ik migreren mijn bestaande gebruikersnamen, wachtwoorden en profielen uit mijn database naar Azure AD B2C?
De Azure AD Graph API kunt u het hulpprogramma voor migratie van schrijven. Zie de [Graph API-voorbeeld](active-directory-b2c-devquickstarts-graph-dotnet.md) voor meer informatie.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Welke beleidsregels voor wachtwoorden voor lokale accounts in Azure AD B2C wordt gebruikt?
De Azure AD B2C-beleidsregels voor wachtwoorden voor lokale accounts is gebaseerd op het beleid voor Azure AD. Azure AD B2C wordt de registratie, registreren of aanmelden en het wachtwoord opnieuw instellen van beleidsregels gebruikt de sterkte 'sterk' wachtwoord en wachtwoorden niet verloopt. Lees de [Azure AD-wachtwoordbeleid](https://msdn.microsoft.com/library/azure/jj943764.aspx) voor meer informatie.

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Kan ik Azure AD Connect gebruiken voor het migreren van consumentidentiteiten die zijn opgeslagen op mijn lokale Active Directory naar Azure AD B2C?
Nee, Azure AD Connect is niet ontworpen voor gebruik met Azure AD B2C. Overweeg het gebruik van de [Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) voor gebruikersmigratie.

### <a name="can-my-app-open-up-azure-ad-b2c-pages-within-an-iframe"></a>Kan mijn app openen om Azure AD B2C-pagina's binnen een iFrame?
Nee, uit veiligheidsoverwegingen Azure AD B2C-pagina's niet openen binnen een iFrame.  Onze service communiceert met de browser om te voorkomen dat iFrames.  De beveiligingscommunity in het algemeen en de specificatie OAUTH2 u wordt aangeraden met iFrames voor identiteitservaringen vanwege het risico van steunpunten voor van klikken.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Werkt Azure AD B2C met zoals Microsoft Dynamics CRM-systemen?
Integratie met Microsoft Dynamics 365 Portal is beschikbaar.  Zie [Dynamics 365-Portal configureren voor Azure AD B2C verificatie](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/portals/azure-ad-b2c).

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Azure AD B2C komt met SharePoint on-premises 2016 werk- of eerdere?
Azure AD B2C is niet bedoeld voor het SharePoint externe partner delen scenario; Zie [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) in plaats daarvan.

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Moet ik Azure AD B2C of B2B gebruiken om externe identiteiten te beheren?
Lees dit artikel over [externe identiteiten](../active-directory/active-directory-b2b-compare-external-identities.md) voor meer informatie over het toepassen van de desbetreffende functies op uw scenario's voor externe identiteit.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Welke rapportage- en controle van functies Azure AD B2C biedt? Zijn ze hetzelfde als in Azure AD Premium?
Nee, Azure AD B2C ondersteunt geen dezelfde set rapporten als Azure AD Premium. Er zijn echter veel commonalities:

* De aanmeldingspagina rapporten bieden een record van elke aanmelden met verminderde details.
* Controlerapporten zijn beschikbaar in de Azure-portal onder Azure Active Directory > activiteit controlelogboeken > Kies B2C en filters naar wens toepassen. Zowel admin-activiteit als activiteit van de toepassing worden behandeld. 
* Een gebruiksrapport die betrekking hebben op het aantal gebruikers, aantal aanmeldingen en volume van MFA is beschikbaar op [gebruik rapportage-API](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-usage-reporting-api)

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Kan ik lokalisatie van de gebruikersinterface van pagina's die worden bediend door Azure AD B2C? Welke talen worden ondersteund?
Ja.  Meer informatie over [taal aanpassing](active-directory-b2c-reference-language-customization.md), deze bevindt zich in de openbare preview.  Wij vertalingen voor 36 talen en u kunt een willekeurige tekenreeks aan uw behoeften te overschrijven.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Kan ik mijn eigen URL's op mijn zich kunnen registreren en aanmelden pagina's die worden behandeld door Azure AD B2C gebruiken? Bijvoorbeeld, kan ik de URL van login.microsoftonline.com naar login.contoso.com wijzigen?
Momenteel niet. Deze functie is op onze roadmap. Verifiëren van uw domein in de **domeinen** tabblad in de klassieke Azure portal biedt geen dit doel te bereiken.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Hoe verwijder ik mijn Azure AD B2C-tenant?
Volg deze stappen voor het verwijderen van uw Azure AD B2C-tenant:

1. Volg deze stappen voor [gaat u naar Azure AD B2C-instellingen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in de Azure portal.
1. Navigeer naar de **toepassingen**, **identiteitsproviders**, en **alle beleidsregels** en verwijder alle vermeldingen in elk van deze servers.
1. Nu aanmelden bij de [klassieke Azure-portal](https://manage.windowsazure.com/) als Abonnementsbeheerder. (Gebruik hetzelfde werk- of schoolaccount of hetzelfde Microsoft-account waarmee u zich aanmelden voor Azure.)
1. Ga naar de Active Directory-extensie aan de linkerkant en klik op uw B2C-tenant.
1. Klik op de **gebruikers** tabblad.
1. Selecteer elke gebruiker daarmee (de beheerder van abonnement u momenteel bent aangemeld als uitsluiten). Klik op **verwijderen** aan de onderkant van de pagina en klik op **Ja** wanneer u wordt gevraagd.
1. Klik op de **toepassingen** tabblad.
1. Selecteer **toepassingen mijn bedrijf eigenaar is van** in de **weergeven** vervolgkeuzelijst en klik op het vinkje.
1. Een toepassing aangeroepen **b2c-uitbreidingen-app**. Klik op **verwijderen** aan de onderkant van de pagina en klik op **Ja** wanneer u wordt gevraagd.
1. Navigeer naar de Active Directory-extensie opnieuw en selecteer uw B2C-tenant.
1. Klik op **verwijderen** aan de onderkant van de pagina. Volg de instructies op het scherm voor het voltooien van het proces.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Kan ik Azure AD B2C als onderdeel van Enterprise Mobility Suite krijgen?
Nee, Azure AD B2C wordt een betalen naar gebruik Azure-service en is geen onderdeel van Enterprise Mobility Suite.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Hoe meld ik problemen met Azure AD B2C
Zie [bestand ondersteuningsaanvragen voor Azure Active Directory B2C](active-directory-b2c-support.md).
