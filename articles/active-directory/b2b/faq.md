---
title: Azure Active Directory B2B-samenwerking Veelgestelde vragen | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over Azure Active Directory B2B-samenwerking.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: reference
ms.date: 10/29/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.openlocfilehash: 481fb929a8f937b1d822755e93fae09d9f585fad
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54436821"
---
# <a name="azure-active-directory-b2b-collaboration-faqs"></a>Azure Active Directory B2B-samenwerking Veelgestelde vragen

Deze Veelgestelde vragen over Azure Active Directory (Azure AD) business-to-business (B2B) samenwerking worden regelmatig bijgewerkt met nieuwe onderwerpen.

### <a name="can-we-customize-our-sign-in-page-so-its-more-intuitive-for-our-b2b-collaboration-guest-users"></a>Kunnen we onze aanmeldingspagina aanpassen zodat u meer intuïtieve benadering voor onze gastgebruikers voor B2B-samenwerking?
Absoluut! Zie onze [blogbericht over deze functie](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/). Zie voor meer informatie over het aanpassen van de aanmeldingspagina van uw organisatie [huisstijl om aan te melden en Toegangsvenster pagina's toevoegen](../fundamentals/customize-branding.md).

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>Gebruikers van B2B-samenwerking toegang tot SharePoint Online en OneDrive?
Ja. De mogelijkheid om te zoeken naar bestaande gastgebruikers in SharePoint Online met behulp van de personen selecteren is echter **uit** standaard. Als u wilt inschakelen op de optie om te zoeken naar bestaande gastgebruikers, stel **ShowPeoplePickerSuggestionsForGuestUsers** naar **op**. U kunt deze instelling inschakelen op op het tenantniveau van de of op het niveau van de site. U kunt deze instelling wijzigen met behulp van de cmdlets Set-SPOTenant en Set-SPOSite. Met deze cmdlets kunnen leden alle bestaande gastgebruikers ook kunnen zoeken in de map. Wijzigingen in het tenantbereik heeft geen invloed op SharePoint Online-sites die al zijn ingericht.

### <a name="is-the-csv-upload-feature-still-supported"></a>Wordt de Uploadfunctie CSV nog steeds ondersteund?
Ja. Zie voor meer informatie over het gebruik van de functionaliteit voor het CSV-bestand uploaden [deze PowerShell-voorbeeld](code-samples.md).

### <a name="how-can-i-customize-my-invitation-emails"></a>Hoe kan ik mijn uitnodigingen per e-mail aanpassen?
U kunt bijna alles over het proces van de afzender van de uitnodiging aanpassen met behulp van de [B2B uitnodiging API's](customize-invitation-api.md).

### <a name="can-guest-users-reset-their-multi-factor-authentication-method"></a>Gastgebruikers opnieuw kunnen instellen hun multi-factor authenticatiemethode?
Ja. Gastgebruikers kunnen herstellen van de multi-factor authenticatiemethode dezelfde manier als normale gebruikers doen.

### <a name="which-organization-is-responsible-for-multi-factor-authentication-licenses"></a>Welke organisatie verantwoordelijk is voor verificatie met meerdere factoren licenties?
De uitnodigende organisatie uitvoert meervoudige verificatie. De uitnodigende organisatie moet ervoor zorgen dat de organisatie heeft voldoende licenties voor hun B2B-gebruikers dat van verificatie met meerdere factoren gebruikmaakt.

### <a name="what-if-a-partner-organization-already-has-multi-factor-authentication-set-up-can-we-trust-their-multi-factor-authentication-and-not-use-our-own-multi-factor-authentication"></a>Wat gebeurt er als heeft al een partnerorganisatie verificatie met meerdere factoren instellen? Kunnen we de multi-factor Authentication-verificatie, niet worden gebruikt en vertrouwd onze eigen meervoudige verificatie?
Deze functie is gepland voor een toekomstige release, zodat die vervolgens u kunt specifieke partners moeten worden uitgesloten van de multi-factor Authentication-verificatie (de uitnodigende organisatie).

### <a name="how-can-i-use-delayed-invitations"></a>Hoe kan ik vertraagde uitnodigingen gebruiken?
Een organisatie wil mogelijk gebruikers van B2B-samenwerking toevoegen en vervolgens uitnodigingen verzenden naar toepassingen inrichten, indien nodig. U kunt de B2B-samenwerking uitnodiging API gebruiken om aan te passen van de onboarding-werkstroom.

### <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>Kan ik gastgebruikers zichtbaar maken in de algemene Exchange-adreslijst?
Ja. Gast-objecten zijn niet zichtbaar in de globale adreslijst van uw organisatie standaard, maar u kunt Azure Active Directory PowerShell gebruiken om ze zichtbaar. Zie **kan ik objecten Gast zichtbaar maken in de globale adreslijst?** in [toegang voor gasten in Office 365-groepen](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6#PickTab=FAQ).

### <a name="can-i-make-a-guest-user-a-limited-administrator"></a>Ik maak een gastgebruiker een beperkte beheerder?
Absoluut. Zie voor meer informatie, [gastgebruikers toevoegen aan een rol](add-guest-to-role.md).

### <a name="does-azure-ad-b2b-collaboration-allow-b2b-users-to-access-the-azure-portal"></a>Staat Azure AD B2B-samenwerking B2B-gebruikers toegang tot de Azure portal?
Tenzij een gebruiker de rol van beperkte beheerder of de globale beheerdersrol is toegewezen, wordt niet toegang tot de Azure-portal nodig hebben gebruikers van B2B-samenwerking. Gebruikers van B2B-samenwerking die zijn toegewezen de rol van beperkte beheerder of globale beheerder kunnen echter toegang tot de portal. Ook als een gastgebruiker die een van deze beheerdersrollen is niet toegewezen toegang heeft tot de portal, de gebruiker mogelijk toegang kunnen krijgen tot bepaalde onderdelen van de ervaring. De Gast-gebruikersrol heeft bepaalde machtigingen in de map.

### <a name="can-i-block-access-to-the-azure-portal-for-guest-users"></a>Kan ik toegang tot de Azure-portal voor gastgebruikers blokkeren?
Ja. Wanneer u dit beleid configureren, Let erop om te voorkomen dat per ongeluk blokkeren van toegang tot de leden en beheerders.
Een gastgebruiker toegang tot blokkeren de [Azure-portal](https://portal.azure.com), gebruikt u een beleid voor voorwaardelijke toegang in de Windows Azure classic deployment model-API:
1. Wijzig de **alle gebruikers** groeperen zodat deze alleen leden bevat.
  ![Schermafbeelding van de groep wijzigen](media/faq/modify-all-users-group.png)
2. Maak een dynamische groep met gastgebruikers die.
  ![Schermafbeelding van de groep maken](media/faq/group-with-guest-users.png)
3. Stellen beleid voor voorwaardelijke toegang voor gastgebruikers blokkeren van toegang tot de portal, zoals wordt weergegeven in de volgende video:
  
  > [!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player] 

### <a name="does-azure-ad-b2b-collaboration-support-multi-factor-authentication-and-consumer-email-accounts"></a>Ondersteunt Azure AD B2B-samenwerking multi-factor authentication en consumenten-e-mailaccounts?
Ja. Multi-factor authentication en consument e-mailaccounts worden beide ondersteund voor Azure AD B2B-samenwerking.

### <a name="do-you-support-password-reset-for-azure-ad-b2b-collaboration-users"></a>Wachtwoorden opnieuw instellen voor gebruikers van Azure AD B2B-samenwerking ondersteund?
Als uw Azure AD-tenant de basismap van een gebruiker is, kunt u [opnieuw instellen van wachtwoord van de gebruiker](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-reset-password-azure-portal#how-to-reset-the-password-for-a-user) vanuit Azure portal. Maar u niet rechtstreeks opnieuw instellen met een wachtwoord voor een gastgebruiker die zich aanmeldt met een account dat wordt beheerd door een andere Azure AD-directory of externe id-provider. Alleen de gastgebruiker of een beheerder in de basismap van de gebruiker kan het wachtwoord opnieuw instellen. Hier volgen enkele voorbeelden van hoe wachtwoord opnieuw moet worden ingesteld voor gastgebruikers ook kunnen:
 
* Gastgebruikers die zich aanmelden met een Microsoft-account (bijvoorbeeld guestuser@live.com) kunnen hun eigen wachtwoorden via Self-service voor wachtwoordherstel (SSPR) van Microsoft-account opnieuw instellen. Zie [opnieuw instellen van het wachtwoord van uw Microsoft-account](https://support.microsoft.com/help/4026971/microsoft-account-how-to-reset-your-password).
* Gastgebruikers die zich aanmelden met een Google-account of een andere externe id-provider kunnen opnieuw instellen van hun eigen wachtwoorden met behulp van de id-provider SSPR-methode. Bijvoorbeeld, een gastgebruiker met het Google-account guestuser@gmail.com kunnen hun wachtwoord opnieuw door de instructies in [wijzigen of uw wachtwoord opnieuw instellen](https://support.google.com/accounts/answer/41078).
* Als de tenant-identiteit een just-in-time (JIT is) of 'viraal' tenant (wat betekent dat het een afzonderlijke, niet-beheerde Azure-tenant), alleen de gastgebruiker kan hun wachtwoord opnieuw instellen.
* Als de basismap van de gastgebruiker uw Azure AD-tenant is, kunt u het wachtwoord van de gebruiker opnieuw instellen. Bijvoorbeeld, u mogelijk hebt gemaakt als een gebruiker klikt of een gebruiker is gesynchroniseerd vanuit uw on-premises Active Directory en hun UserType ingesteld op Gast. Omdat deze gebruiker is toegewezen in uw directory, kunt u het wachtwoord van de Azure-portal opnieuw instellen.

### <a name="does-microsoft-dynamics-365-provide-online-support-for-azure-ad-b2b-collaboration"></a>Biedt Microsoft Dynamics 365 online ondersteuning voor Azure AD B2B-samenwerking?
Ja, Dynamics 365 (online) ondersteunt Azure AD B2B-samenwerking. Zie voor meer informatie het artikel Dynamics 365 [gebruikers met Azure AD B2B-samenwerking uitnodigen](https://docs.microsoft.com/dynamics365/customer-engagement/admin/invite-users-azure-active-directory-b2b-collaboration).

### <a name="what-is-the-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Wat is de levensduur van een eerste wachtwoord voor een nieuwe gebruiker van de B2B-samenwerking?
Azure AD heeft een vaste set teken Wachtwoordsterkte en gebruikersaccounts in de cloud accountvergrendeling vereisten die ook voor alle Azure AD gelden-account. Cloud-gebruikersaccounts zijn accounts die niet worden gefedereerd met een andere id-provider, zoals 
* Microsoft-account
* Facebook
* Active Directory Federation Services
* Een andere cloud-tenant (voor B2B-samenwerking)

Voor federatieve accounts afhankelijk wachtwoordbeleid van het beleid dat wordt toegepast in de on-premises-tenants en instellingen voor Microsoft-account van de gebruiker.

### <a name="an-organization-might-want-to-have-different-experiences-in-their-applications-for-tenant-users-and-guest-users-is-there-standard-guidance-for-this-is-the-presence-of-the-identity-provider-claim-the-correct-model-to-use"></a>Een organisatie mogelijk wil hebben verschillende ervaringen in hun toepassingen voor tenantgebruikers en gastgebruikers. Is er standaard richtlijnen voor dit? Is de aanwezigheid van de id-provider aanspraak op de juiste model te gebruiken?
Een gastgebruiker kan id-provider gebruiken om te verifiëren. Zie voor meer informatie, [eigenschappen van een gebruiker B2B-samenwerking](user-properties.md). Gebruik de **UserType** eigenschap om te bepalen van de gebruikerservaring. De **UserType** claim op dit moment niet is opgenomen in het token. Toepassingen moeten de Graph API gebruiken om op te vragen van de map voor de gebruiker en het UserType ophalen.

### <a name="where-can-i-find-a-b2b-collaboration-community-to-share-solutions-and-to-submit-ideas"></a>Waar vind ik een B2B-samenwerking-community voor het delen van oplossingen en ideeën wilt indienen?
We zijn voortdurend luisteren naar uw feedback, voor het verbeteren van B2B-samenwerking. Deel uw gebruiker scenario's, aanbevolen procedures en waar u tevreden over Azure AD B2B-samenwerking. Deelnemen aan de discussie de [Microsoft Tech-Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).
 
We nodigen u ook uit om in te dienen uw ideeën en stem voor toekomstige functies op [B2B-samenwerking ideeën](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas).

### <a name="can-we-send-an-invitation-that-is-automatically-redeemed-so-that-the-user-is-just-ready-to-go-or-does-the-user-always-have-to-click-through-to-the-redemption-url"></a>Kunnen we een uitnodiging die automatisch wordt ingewisseld, zodat de gebruiker alleen 'klaar is voor gebruik ' verzenden? Of de gebruiker altijd hoeft door te klikken op de URL voor inschrijving?
U kunt andere gebruikers in de andere organisatie uitnodigen met behulp van de gebruikersinterface, PowerShell-scripts of API's. U kunt vervolgens de gastgebruiker een directe koppeling verzenden naar een gedeelde app. In de meeste gevallen is niet langer een nodig om te openen de e-mailuitnodiging en klikt u op een URL voor inwisselen. Zie [Azure Active Directory B2B-samenwerking uitnodiging inwisselen](redemption-experience.md).

### <a name="how-does-b2b-collaboration-work-when-the-invited-partner-is-using-federation-to-add-their-own-on-premises-authentication"></a>Hoe werkt B2B-samenwerking wanneer de uitgenodigde partner federation om toe te voegen van hun eigen on-premises verificatie?
Als de partner een Azure AD-tenant die is gefedereerd naar de on-premises infrastructuur voor verificatie heeft, on-premises eenmalige aanmelding (SSO) automatisch bereikt. Als de partner niet een Azure AD-tenant hebt, wordt een Azure AD-account gemaakt voor nieuwe gebruikers. 

### <a name="i-thought-azure-ad-b2b-didnt-accept-gmailcom-and-outlookcom-email-addresses-and-that-b2c-was-used-for-those-kinds-of-accounts"></a>Kan ik Azure AD B2B gmail.com en outlook.com e-mailadressen niet hebt geaccepteerd, en dat B2C is gebruikt voor deze soorten accounts beschouwd?
De verschillen tussen B2B- en business-to-consumer (B2C) samenwerking in termen van welke identiteiten worden ondersteund worden verwijderd. De identiteit die wordt gebruikt, is een goede reden om te kiezen tussen met behulp van B2B of B2C niet. Zie voor meer informatie over het kiezen van de optie voor samenwerking [vergelijken B2B-samenwerking en B2C in Azure Active Directory](compare-with-b2c.md).

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>Welke toepassingen en services ondersteuning voor Azure B2B-gastgebruikers?
Alle Azure AD geïntegreerde toepassingen kunnen ondersteuning bieden voor Azure B2B-gastgebruikers, maar ze moeten een tenants-eindpunt gebruiken voor verificatie van gastgebruikers. Mogelijk moet u ook aan [aanpassen van de claims](claims-mapping.md) in het SAML-token dat is uitgegeven als een gastgebruiker zich bij de app verifieert. 

### <a name="can-we-force-multi-factor-authentication-for-b2b-guest-users-if-our-partners-dont-have-multi-factor-authentication"></a>Kunnen we multi-factor authentication voor B2B-gastgebruikers afdwingen als onze partners geen meervoudige verificatie?
Ja. Zie voor meer informatie, [voorwaardelijke toegang voor gebruikers van B2B-samenwerking](conditional-access.md).

### <a name="in-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-can-we-do-this-in-azure"></a>U kunt een lijst met 'toestaan' of 'geweigerd' voor externe gebruikers definiëren in SharePoint. We dit kunt doen in Azure?
Ja. Azure AD B2B-samenwerking ondersteunt lijsten toestaan en weigeren van lijsten. 

### <a name="what-licenses-do-we-need-to-use-azure-ad-b2b"></a>Welke licenties moeten we Azure AD B2B gebruiken?
Zie voor meer informatie over de licenties die uw organisatie nodig heeft om te gebruiken van Azure AD B2B [Azure Active Directory B2B-samenwerking licentierichtlijnen](licensing-guidance.md).

### <a name="next-steps"></a>Volgende stappen

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)

