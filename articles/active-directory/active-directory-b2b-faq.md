---
title: Azure Active Directory B2B-samenwerking Veelgestelde vragen | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over Azure Active Directory B2B-samenwerking.
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: bfedbbf8b26e1b129584a6a644e64a15635f5723
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2b-collaboration-faqs"></a>Azure Active Directory B2B-samenwerking Veelgestelde vragen

Deze Veelgestelde vragen over Azure Active Directory (Azure AD) business-to-business (B2B) samenwerking worden regelmatig bijgewerkt met nieuwe onderwerpen.

### <a name="is-azure-ad-b2b-collaboration-available-in-the-azure-classic-portal"></a>Azure AD B2B-samenwerking beschikbaar is in de klassieke Azure portal?
Nee. Azure AD B2B-samenwerkingsfuncties zijn alleen beschikbaar in de [Azure-portal](https://portal.azure.com) en in de [Toegangspaneel](https://myapps.microsoft.com/). 

### <a name="can-we-customize-our-sign-in-page-so-it-is-more-intuitive-for-our-b2b-collaboration-guest-users"></a>Kunnen we onze aanmeldingspagina aanpassen zodat u meer intuïtieve van onze gastgebruikers B2B-samenwerking?
Absoluut! Zie onze [blogbericht over deze functie](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/). Zie voor meer informatie over het aanpassen van de aanmeldingspagina van uw organisatie [huisstijl om aan te melden en de Toegangsvensterpagina's toevoegen](active-directory-add-company-branding.md).

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>B2B-samenwerking gebruikers toegang tot SharePoint Online en OneDrive?
Ja. De mogelijkheid om te zoeken naar bestaande gastgebruikers in SharePoint Online met behulp van de personen selecteren is echter **uit** standaard. U schakelt de optie om te zoeken naar bestaande gastgebruikers stellen **ShowPeoplePickerSuggestionsForGuestUsers** naar **op**. U kunt deze instelling inschakelen op het niveau van de tenant of op het niveau van de site. U kunt deze instelling wijzigen met behulp van de Set SPOTenant en Set-SPOSite-cmdlets. Met deze cmdlets kunnen leden alle bestaande gastgebruikers zoeken in de map. Wijzigingen in het tenantbereik is niet van invloed op SharePoint Online-sites die al zijn ingericht.

### <a name="is-the-csv-upload-feature-still-supported"></a>Is de CSV-upload-functie nog steeds ondersteund?
Ja. Zie voor meer informatie over het gebruik van de CSV-bestand uploaden functie [deze PowerShell-voorbeeld](active-directory-b2b-code-samples.md).

### <a name="how-can-i-customize-my-invitation-emails"></a>Hoe kan ik mijn e-mailberichten voor de uitnodiging aanpassen?
U kunt bijna alles over het proces uitnodiging antwoorden aanpassen met behulp van de [B2B uitnodiging API's](active-directory-b2b-api.md).

### <a name="can-an-invited-external-user-leave-the-organization-after-being-invited"></a>Kan een uitgenodigde externe gebruiker de organisatie na worden uitgenodigd verlaten?
De uitnodiging beheerder van de organisatie een gastgebruiker B2B-samenwerking van hun adreslijst kunt verwijderen, maar de gastgebruiker de uitnodiging organisatiemap zelf kan niet worden achtergelaten. 

### <a name="can-guest-users-reset-their-multi-factor-authentication-method"></a>Gastgebruikers kunnen opnieuw hun methode multi-factor authentication-server instellen?
Ja. Gastgebruikers kunnen hun multi-factor authentication-server de methode reset dezelfde manier als normale gebruikers doen.

### <a name="which-organization-is-responsible-for-multi-factor-authentication-licenses"></a>Welke organisatie is verantwoordelijk voor meervoudige verificatie licenties?
De uitnodiging organisatie wordt multi-factor authentication uitgevoerd. De organisatie uitnodigen moet ervoor zorgen dat de organisatie heeft voldoende licenties voor hun B2B-gebruikers met multi-factor authentication-server.

### <a name="what-if-a-partner-organization-already-has-multi-factor-authentication-set-up-can-we-trust-their-multi-factor-authentication-and-not-use-our-own-multi-factor-authentication"></a>Wat gebeurt er als heeft al een partnerorganisatie meervoudige verificatie instellen? Kunnen we vertrouwt hun multi-factor authentication-server, en geen gebruik van ons eigen multi-factor authentication?
Deze functie is gepland voor een toekomstige release, zodat u vervolgens een die specifieke partners moeten worden uitgesloten selecteren kunt van de multi-factor Authentication-verificatie (uitnodigen van de organisatie).

### <a name="how-can-i-use-delayed-invitations"></a>Hoe kan ik vertraagde uitnodigingen gebruiken?
Een organisatie wil mogelijk B2B-samenwerking gebruikers toevoegen en vervolgens de uitnodiging toepassingen inrichten, indien nodig. U kunt de B2B-samenwerking uitnodiging API gebruiken voor het aanpassen van de werkstroom voorbereiden.

### <a name="can-i-make-a-guest-user-a-limited-administrator"></a>Kan ik maken een gastgebruiker beperkt beheerder?
Absoluut. Zie voor meer informatie [gastgebruikers toe te voegen aan een rol](active-directory-users-assign-role-azure-portal.md).

### <a name="does-azure-ad-b2b-collaboration-allow-b2b-users-to-access-the-azure-portal"></a>Kan Azure AD B2B-samenwerking B2B-gebruikers toegang tot de Azure portal
Tenzij een gebruiker de rol van beperkt beheerder of de globale beheerdersrol is toegewezen, won't B2B-samenwerking gebruikers toegang tot de Azure-portal nodig. B2B-samenwerking gebruikers die de rol van beperkt beheerder of de globale beheerdersrol is toegewezen kunnen echter toegang tot de portal. Ook als een gastgebruiker die een van deze rollen niet is toegewezen naar de portal, de gebruiker mogelijk toegang tot bepaalde delen van de gebruikerservaring. De Gast-gebruikersrol heeft bepaalde machtigingen in de map.

### <a name="can-i-block-access-to-the-azure-portal-for-guest-users"></a>Kan ik toegang tot de Azure-portal voor gastgebruikers blokkeren?
Ja. Als u dit beleid configureert, Let erop om te voorkomen dat per ongeluk blokkeren van toegang tot de leden en beheerders.
Toegang tot een gastgebruiker blokkeren de [Azure-portal](https://portal.azure.com), beleid voor voorwaardelijke toegang gebruiken in de Windows Azure classic deployment model API:
1. Wijzig de **alle gebruikers** groeperen, zodat deze alleen leden bevat.
  ![wijzigen van de groep-schermafbeelding](media/active-directory-b2b-faq/modify-all-users-group.png)
2. Maak een dynamische groep waarin gastgebruikers.
  ![Schermafbeelding van de groep maken](media/active-directory-b2b-faq/group-with-guest-users.png)
3. Kunt instellen beleid voor voorwaardelijke toegang voor gastgebruikers blokkeren van toegang tot de portal zoals weergegeven in de volgende video:
  
  > [!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player] 

### <a name="does-azure-ad-b2b-collaboration-support-multi-factor-authentication-and-consumer-email-accounts"></a>Ondersteunt Azure AD B2B-samenwerking multi-factor authentication en e-mailaccounts consument?
Ja. Meervoudige verificatie- en e-mailaccounts worden beide ondersteund voor Azure AD B2B-samenwerking.

### <a name="do-you-plan-to-support-password-reset-for-azure-ad-b2b-collaboration-users"></a>Wilt u ondersteuning voor wachtwoordherstel voor gebruikers van Azure AD B2B-samenwerking?
Ja. Hier volgen de belangrijke informatie voor selfservice voor wachtwoordherstel (SSPR) voor een B2B-gebruiker die van een partnerorganisatie wordt verzocht:
 
* SSPR treedt alleen op de tenant van de identiteit van de B2B-gebruiker.
* Als de identiteit van de tenant een Microsoft-account is, wordt het Microsoft-account SSPR-mechanisme gebruikt.
* Als de identiteit van de tenant een just-in-time (Just in time is) of 'een' tenant, wordt er een wachtwoord opnieuw instellen van e-mailadres verzonden.
* Voor andere tenants, wordt het standaardproces SSPR gevolgd voor B2B-gebruikers. Zoals member SSPR voor B2B-gebruikers in de context van de resource is tenancymodus geblokkeerd. 

### <a name="is-password-reset-available-for-guest-users-in-a-just-in-time-jit-or-viral-tenant-who-accepted-invitations-with-a-work-or-school-email-address-but-who-didnt-have-a-pre-existing-azure-ad-account"></a>Wachtwoord opnieuw ingesteld beschikbaar voor gastgebruikers in een just-in-time (Just in time) of 'een' tenant die geaccepteerd uitnodigingen met een werk of school e-mailadres, maar die geen bestaande Azure AD-account?
Ja. Een wachtwoord opnieuw instellen van e-mailbericht kan die kunnen gebruikers hun wachtwoord opnieuw instellen in de JIT-tenancymodus worden verzonden.

### <a name="does-microsoft-dynamics-crm-provide-online-support-for-azure-ad-b2b-collaboration"></a>Biedt Microsoft Dynamics CRM online ondersteuning voor Azure AD B2B-samenwerking?
Op dit moment biedt Microsoft Dynamics CRM geen ondersteuning voor online voor Azure AD B2B-samenwerking. We zullen echter deze in de toekomst te ondersteunen.

### <a name="what-is-the-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Wat is de levensduur van een eerste wachtwoord voor een nieuwe gebruiker voor B2B-samenwerking?
Azure AD heeft een vaste set van teken, Wachtwoordsterkte en account lockout vereisten die gelden voor zowel alle Azure AD cloud gebruikersaccounts. Cloud-gebruikersaccounts zijn accounts die zijn niet federatief met een andere id-provider, zoals 
* Microsoft-account
* Facebook
* Active Directory Federatieservices
* Een andere cloudtenant (voor B2B-samenwerking)

Voor federatieve accounts afhankelijk wachtwoordbeleid van het beleid dat is toegepast in de lokale tenancymodus en instellingen voor Microsoft-account van de gebruiker.

### <a name="an-organization-might-want-to-have-different-experiences-in-their-applications-for-tenant-users-and-guest-users-is-there-standard-guidance-for-this-is-the-presence-of-the-identity-provider-claim-the-correct-model-to-use"></a>Een organisatie mogelijk wil hebben verschillende ervaringen in hun toepassingen voor gebruikers van de tenant en gastgebruikers. Is er standaard richtlijnen voor dit? Is de aanwezigheid van de identiteitsprovider claim het juiste model te gebruiken?
 Een gastgebruiker kan id-provider voor verificatie gebruiken. Zie voor meer informatie [eigenschappen van een gebruiker B2B-samenwerking](active-directory-b2b-user-properties.md). Gebruik de **UserType** eigenschap om te bepalen van de gebruikerservaring. De **UserType** claim is momenteel niet opgenomen in het token. Toepassingen moeten de Graph API gebruiken om op te vragen van de map voor de gebruiker en het UserType ophalen.

### <a name="where-can-i-find-a-b2b-collaboration-community-to-share-solutions-and-to-submit-ideas"></a>Waar vind ik een B2B-samenwerking community delen oplossingen en ideeën verzenden?
We zijn voortdurend luisteren naar uw feedback B2B-samenwerking verbeteren. We nodigen u uw gebruikers kunt delen scenario's, aanbevolen procedures en wat u leuk vindt aan Azure AD B2B-samenwerking. Deelnemen aan de bespreking van de [Microsoft Tech-Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).
 
We nodigen ook u uw ideeën en stem voor toekomstige functies op verzenden [B2B-samenwerking ideeën](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas).

### <a name="can-we-send-an-invitation-that-is-automatically-redeemed-so-that-the-user-is-just-ready-to-go-or-does-the-user-always-have-to-click-through-to-the-redemption-url"></a>We een uitnodiging kunt verzenden die automatisch wordt ingewisseld, zodat de gebruiker zojuist 'nu klaar is voor'? Of de gebruiker altijd heeft door te klikken naar de URL inwisseling?
Uitnodigingen die zijn verzonden door de gebruiker in de uitnodiging organisatie die ook lid zijn van de partnerorganisatie vereisen geen inwisseling door de gebruiker B2B.

Het is raadzaam dat u één gebruiker van de partnerorganisatie lid worden van de uitnodiging organisatie uitnodigen. [Voeg deze gebruiker toe aan de rol van Gast uitnodiging antwoorden in de bronorganisatie](active-directory-b2b-add-guest-to-role.md). Deze gebruiker andere gebruikers in de organisatie van de accountpartner met behulp van de aanmeldingspagina-gebruikersinterface, PowerShell-scripts kunt uitnodigen of API's. Vervolgens zijn niet B2B-samenwerking gebruikers van die organisatie vereist om in te wisselen hun uitnodigingen.

### <a name="how-does-b2b-collaboration-work-when-the-invited-partner-is-using-federation-to-add-their-own-on-premises-authentication"></a>Hoe werkt B2B-samenwerking wanneer de uitgenodigde partner van Federatie gebruikmaakt hun eigen lokale verificatie toevoegen?
Als de partner een Azure AD-tenant die aan de on-premises verificatie-infrastructuur is gefedereerd heeft, lokale eenmalige aanmelding (SSO) automatisch bereikt. Als de partner geen Azure AD-tenant, wordt een Azure AD-account gemaakt voor nieuwe gebruikers. 

### <a name="i-thought-azure-ad-b2b-didnt-accept-gmailcom-and-outlookcom-email-addresses-and-that-b2c-was-used-for-those-kinds-of-accounts"></a>Ik Azure AD B2B niet geaccepteerd door de gmail.com en outlook.com e-mailadressen en dat B2C is gebruikt voor deze typen accounts beschouwd?
De verschillen tussen B2B- en business naar bedrijf (B2C) samenwerking in termen van welke identiteiten worden ondersteund worden verwijderd. De identiteit die wordt gebruikt, is niet een goede reden B2B gebruiken of B2C kiezen. Zie voor meer informatie over het kiezen van de optie voor samenwerking [vergelijken B2B-samenwerking en B2C in Azure Active Directory](active-directory-b2b-compare-b2c.md).

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>Welke toepassingen en services ondersteuning voor Azure B2B gastgebruikers?
Alle Azure AD-geïntegreerde toepassingen ondersteuning bieden voor Azure B2B gastgebruikers. 

### <a name="can-we-force-multi-factor-authentication-for-b2b-guest-users-if-our-partners-dont-have-multi-factor-authentication"></a>Kunnen we multi-factor authentication voor gastgebruikers B2B forceren als onze partners geen multi-factor authentication-server hebt?
Ja. Zie voor meer informatie [voorwaardelijke toegang voor gebruikers voor B2B-samenwerking](active-directory-b2b-mfa-instructions.md).

### <a name="in-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-can-we-do-this-in-azure"></a>In SharePoint, kunt u een lijst met voor 'toestaan' of 'weigeren' voor externe gebruikers definiëren. We dit kunt doen in Azure?
Ja. Azure AD B2B-samenwerking ondersteunt lijsten toestaan en weigeren van lijsten. 

### <a name="what-licenses-do-we-need-to-use-azure-ad-b2b"></a>Welke licenties hebben we het gebruik van Azure AD B2B nodig?
Zie voor meer informatie over wat de behoeften van uw organisatie te gebruiken van Azure AD B2B licenties [Azure Active Directory B2B-samenwerking richtlijnen licentieverlening](active-directory-b2b-licensing.md).

### <a name="next-steps"></a>Volgende stappen

Lees ook onze andere artikelen over Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hoe voeg beheerders van Azure AD B2B-samenwerking gebruikers?](active-directory-b2b-admin-add-users.md)
* [Hoe kunnen IT-medewerkers B2B-samenwerking gebruikers toevoegen](active-directory-b2b-iw-add-users.md)
* [De elementen van de uitnodigingsmail voor B2B-samenwerking](active-directory-b2b-invitation-email.md)
* [B2B-samenwerking uitnodiging inwisseling](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B-samenwerking licentieverlening](active-directory-b2b-licensing.md)
* [Het oplossen van Azure AD B2B-samenwerking](active-directory-b2b-troubleshooting.md)
* [Azure AD B2B-samenwerking API en de aanpassing](active-directory-b2b-api.md)
* [Multi-Factor Authentication voor gebruikers van B2B-samenwerking](active-directory-b2b-mfa-instructions.md)
* [B2B-samenwerking gebruikers zonder een uitnodiging toevoegen](active-directory-b2b-add-user-without-invite.md)
* [Artikel index voor application management in Azure AD](active-directory-apps-index.md)
