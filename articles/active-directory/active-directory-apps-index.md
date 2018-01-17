---
title: Index voor Toepassingsbeheer in Azure Active Directory artikel | Microsoft Azure
description: Informatie over het aanpassen van de vervaldatum voor uw federatiecertificaten en het vernieuwen van certificaten die binnenkort verlopen.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 5321b8e4-2afa-4dfe-8d53-4add7abb5ec8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: 254f1913f1b9cb1a165d007a1f4204f7783472c1
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="article-index-for-application-management-in-azure-active-directory"></a>Article Index for Application Management in Azure Active Directory (Engelstalig)
Deze pagina bevat een uitgebreide lijst met elk document dat is geschreven over de verschillende functies van de toepassing-gerelateerde in Azure Active Directory (Azure AD).

Er is een korte inleiding tot elke primaire functiegebied, evenals richtlijnen op welke artikelen te lezen, afhankelijk van welke informatie u zoekt.

## <a name="overview-articles"></a>Overzicht van artikelen
De onderstaande artikelen zijn goede beginpunt voor gebruikers die alleen een korte uitleg van functies voor Azure AD-toepassing wilt.

| Artikel handleiding |  |
|:---:| --- |
| Een inleiding tot het beheer van toepassingsproblemen Azure AD is opgelost |[Toepassingen beheren met Azure Active Directory (AD)](active-directory-enable-sso-scenario.md) |
| Een overzicht van de verschillende functies in Azure AD betrekking hebben op het inschakelen van eenmalige aanmelding definiëren wie toegang heeft tot apps en hoe gebruikers apps openen |[Toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md) |
| Bekijk de verschillende stappen die betrokken zijn bij het integreren van apps in uw Azure AD |[Integratie van Azure Active Directory met toepassingen](active-directory-integrating-applications-getting-started.md)<br /><br />[Inschakelen van eenmalige aanmelding voor SaaS-Apps](active-directory-enterprise-apps-manage-sso.md)<br /><br />[Toegang tot Apps beheren](active-directory-managing-access-to-apps.md) |
| Een technische uitleg van hoe apps worden weergegeven in Azure AD |[Hoe en waarom toepassingen worden toegevoegd aan Azure AD](active-directory-how-applications-are-added.md) |

## <a name="troubleshooting-articles"></a>Het oplossen van artikelen
Deze sectie biedt snel toegang tot de relevante gidsen voor het oplossen van problemen. Meer informatie over elke functiegebied vindt u op de rest van deze pagina.

| Functiegebied |  |
|:---:| --- |
| Federatieve eenmalige aanmelding |[Het oplossen van problemen op basis van SAML eenmalige aanmelding](active-directory-saml-debugging.md) |
| Op basis van wachtwoorden eenmalige aanmelding |[De uitbreiding van het deelvenster toegang tot het oplossen van problemen voor Internet Explorer](active-directory-saas-ie-troubleshooting.md) |
| Toepassingsproxy |[Toepassingsproxy-handleiding voor probleemoplossing](active-directory-application-proxy-troubleshoot.md) |
| Eenmalige aanmelding tussen on-premises AD en Azure AD |[Wachtwoordsynchronisatie oplossen](connect/active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization)<br /><br />[Wachtwoord terugschrijven probleemoplossing](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Lidmaatschap van dynamische groepen |[Het lidmaatschap van dynamische groepen oplossen](active-directory-accessmanagement-troubleshooting.md) |

## <a name="single-sign-on-sso"></a>Eenmalige aanmelding (SSO)
### <a name="federated-single-sign-on-sign-into-many-apps-using-one-identity"></a>Federatieve eenmalige aanmelding: Meld u aan bij veel apps met behulp van één identiteit
Eenmalige aanmelding kunt gebruikers toegang krijgen tot een aantal apps en services met behulp van slechts één set referenties. Federatie is een methode waarmee u eenmalige aanmelding kunt inschakelen. Wanneer gebruikers proberen zich aanmeldt bij federatieve apps, deze wordt omgeleid naar hun organisatie officiële aanmeldingspagina weergegeven door Azure Active Directory en wordt vervolgens omgeleid naar de app bij verificatie is geslaagd.

| Artikel handleiding |  |
|:---:| --- |
| Een inleiding tot federatieve en andere typen van eenmalige aanmelding |[Eenmalige aanmelding met Azure AD](active-directory-appssoaccess-whatis.md) |
| SaaS-apps die vooraf geïntegreerd met Azure AD met zijn duizenden vereenvoudigd configuratiestappen voor eenmalige aanmelding |[Aan de slag met Azure AD-toepassingsgalerie](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)<br /><br />[Volledige lijst met vooraf geïntegreerde Apps die ondersteuning bieden voor Federatie](http://aka.ms/aadfederatedapps)<br /><br />[Uw App toevoegen aan de App-galerie van Azure AD](active-directory-app-gallery-listing.md) |
| Meer dan 150 app-zelfstudies over het configureren van eenmalige aanmelding voor apps zoals [Salesforce](active-directory-saas-salesforce-tutorial.md), [ServiceNow](active-directory-saas-servicenow-tutorial.md), [Google Apps](active-directory-saas-google-apps-tutorial.md), [Workday](active-directory-saas-workday-tutorial.md), en nog veel meer |[Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md) |
| Het handmatig instellen en aanpassen van uw configuratie voor eenmalige aanmelding |[Hoe om te configureren voor federatieve eenmalige aanmelding voor Apps die niet zijn opgenomen in de Azure Active Directory-Toepassingsgalerie](application-config-sso-how-to-configure-federated-sso-non-gallery.md)<br /><br />[Het aanpassen van uitgegeven Claims in het SAML-Token voor vooraf geïntegreerde Apps](active-directory-saml-claims-customization.md) |
| Gids voor probleemoplossing voor federatieve apps die gebruikmaken van het SAML-protocol |[Het oplossen van problemen op basis van SAML eenmalige aanmelding](active-directory-saml-debugging.md) |
| Het configureren van de vervaldatum van het certificaat van uw app en uw certificaten vernieuwen |[Het beheer van certificaten voor federatieve eenmalige aanmelding bij Azure Active Directory](active-directory-sso-certs.md) |

Federatieve eenmalige aanmelding is beschikbaar voor alle edities van Azure AD voor maximaal tien apps per gebruiker. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) onbeperkte toepassingen worden ondersteund. Als uw organisatie heeft [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) of [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), kunt u [groepen gebruiken voor het toewijzen van toegang tot federatieve toepassingen](#managing-access-to-applications).

### <a name="password-based-single-sign-on-account-sharing-and-sso-for-non-federated-apps"></a>Op basis van wachtwoorden eenmalige aanmelding: Account delen en eenmalige aanmelding voor apps niet-gefedereerde
Om eenmalige aanmelding te bieden geen ondersteuning voor federatieve toepassingen inschakelen, biedt Azure AD wachtwoord functies die kunnen worden veilig opgeslagen wachtwoorden met SaaS-apps en automatisch gebruikers aanmelden bij deze apps. U kunt eenvoudig referenties voor de zojuist gemaakte accounts distribueren en team accounts delen met meerdere personen. Gebruikers hoeft niet te weten de referenties voor de accounts die ze toegang tot krijgt.

| Artikel handleiding |  |
|:---:| --- |
| Een inleiding tot hoe op wachtwoord gebaseerde SSO werkt en een korte technisch overzicht |[Op basis van wachtwoorden eenmalige aanmelding met Azure AD](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) |
| Een samenvatting van de scenario's die betrekking hebben op account delen en hoe deze problemen kunnen worden opgelost door Azure AD |[Accounts delen met Azure AD](active-directory-sharing-accounts.md) |
| Het wachtwoord voor bepaalde apps automatisch worden gewijzigd met een regelmatig interval |[Geautomatiseerde Rollover van wachtwoord (preview)](https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Implementatie en probleemoplossing van de handleidingen voor de Internet Explorer versie van de Azure AD-extensie voor het beheer van wachtwoord |[De extensie van het Configuratiescherm toegang voor Internet Explorer met behulp van Groepsbeleid implementeren](active-directory-saas-ie-group-policy.md)<br /><br />[De uitbreiding van het deelvenster toegang tot het oplossen van problemen voor Internet Explorer](active-directory-saas-ie-troubleshooting.md) |

Op basis van wachtwoorden eenmalige aanmelding is beschikbaar voor alle edities van Azure AD voor maximaal tien apps per gebruiker. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) onbeperkte toepassingen worden ondersteund. Als uw organisatie heeft [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) of [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), kunt u [groepen gebruiken voor toegang tot toepassingen toewijzen](#managing-access-to-applications). Er wordt een geautomatiseerde wachtwoord rollover een [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) functie.

### <a name="app-proxy-single-sign-on-and-remote-access-to-on-premises-applications"></a>App-Proxy: Eenmalige aanmelding en externe toegang tot on-premises toepassingen
Als u toepassingen in uw particuliere netwerk die moeten worden geopend door gebruikers en apparaten buiten het netwerk hebt, kunt u Azure AD-toepassingsproxy gebruiken voor veilige, externe toegang tot deze apps.

| Artikel handleiding |  |
|:---:| --- |
| Overzicht van Azure AD-toepassingsproxy en hoe het werkt |[Een beveiligde externe toegang tot on-premises toepassingen](active-directory-application-proxy-get-started.md) |
| Zelfstudies over het configureren van Application Proxy en hoe u uw eerste app publiceert |[Het instellen van Azure AD-toepassingsproxy](active-directory-application-proxy-enable.md)<br /><br />[Het installeren van de Connector voor toepassingsproxy van achtergrond](active-directory-application-proxy-silent-installation.md)<br /><br />[Het publiceren van toepassingen via App-Proxy](active-directory-application-proxy-publish.md)<br /><br />[Het gebruik van uw eigen domeinnaam](active-directory-application-proxy-custom-domains.md) |
| Het inschakelen van eenmalige aanmelding en voorwaardelijke toegang voor apps die zijn gepubliceerd met toepassingsproxy |[Single-sign-on met toepassingsproxy](active-directory-application-proxy-sso-using-kcd.md)<br /><br />[Voorwaardelijke toegang en toepassingsproxy](application-proxy-enable-remote-access-sharepoint.md) |
| Hulp bij het gebruik van Application Proxy voor de volgende scenario 's |[Hoe ter ondersteuning van Native Client-toepassingen](active-directory-application-proxy-native-client.md)<br /><br />[Het ondersteunen van Claims-compatibele toepassingen](active-directory-application-proxy-claims-aware-apps.md)<br /><br />[Hoe om toepassingen te ondersteunen die is gepubliceerd op afzonderlijke netwerken en locaties](active-directory-application-proxy-connectors-azure-portal.md) |
| Gids voor probleemoplossing voor toepassingsproxy |[Toepassingsproxy-handleiding voor probleemoplossing](active-directory-application-proxy-troubleshoot.md) |

Toepassingsproxy is beschikbaar voor alle edities van Azure AD voor maximaal tien apps per gebruiker. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) onbeperkte toepassingen worden ondersteund. Als uw organisatie heeft [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) of [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), kunt u [groepen gebruiken voor toegang tot toepassingen toewijzen](#managing-access-to-applications).

Hebt u mogelijk ook geïnteresseerd in [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md), waarmee u uw on-premises toepassingen naar Azure migreren terwijl het nog steeds voldoen aan de behoeften van de identiteit van deze toepassingen.

### <a name="enabling-single-sign-on-between-azure-ad-and-on-premises-ad"></a>Inschakelen van eenmalige aanmelding tussen Azure AD en on-premises AD
Als uw organisatie een Windows Server Active Directory on-premises samen met uw Azure Active Directory in de cloud, onderhoudt en vervolgens waarschijnlijk wilt u eenmalige aanmelding tussen deze twee systemen inschakelen. Azure AD Connect (het hulpprogramma dat deze twee systemen samen integreert) biedt meerdere opties voor het instellen van eenmalige aanmelding: Federatie met AD FS of een andere federatieprovider maken of wachtwoordsynchronisatie inschakelen.

| Artikel handleiding |  |
|:---:| --- |
| Een overzicht van de opties voor eenmalige aanmelding die worden aangeboden in Azure AD Connect, evenals informatie over het beheren van hybride omgevingen |[Aanmelding van de gebruiker op opties in Azure AD Connect](active-directory-aadconnect-user-signin.md) |
| Algemene richtlijnen voor het beheren van omgevingen met zowel on-premises Active Directory en Azure Active Directory |[Ontwerpoverwegingen voor Azure AD-hybride identiteit](active-directory-hybrid-identity-design-considerations-overview.md)<br /><br />[Uw On-Premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md) |
| Hulp bij het gebruik van Wachtwoordsynchronisatie voor het inschakelen van eenmalige aanmelding |[Wachtwoordsynchronisatie implementeren met Azure AD Connect](active-directory-aadconnectsync-implement-password-synchronization.md)<br /><br />[Probleem met Wachtwoordsynchronisatie oplossen](https://support.microsoft.com/en-us/kb/2855271) |
| Hulp bij het gebruik van terugschrijven van wachtwoord voor het inschakelen van eenmalige aanmelding |[Aan de slag met wachtwoordbeheer in Azure AD](active-directory-passwords-getting-started.md)<br /><br />[Problemen met Wachtwoord terugschrijven oplossen](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Hulp bij het gebruik van derden id-providers voor het inschakelen van eenmalige aanmelding |[Lijst met compatibele onafhankelijke ID-Providers die kunnen worden gebruikt voor eenmalige aanmelding inschakelen](https://aka.ms/ssoproviders) |
| Hoe Windows 10-gebruikers kunnen profiteren van de voordelen van eenmalige aanmelding via de Azure AD Join |[Cloudfunctionaliteit uitbreiden naar Windows Join 10-apparaten via Azure Active Directory](active-directory-azureadjoin-overview.md) |

Azure AD Connect is beschikbaar voor [alle edities van Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). Azure AD selfservice voor wachtwoordherstel is beschikbaar voor [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) en [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Wachtwoord terugschrijven naar on-premises AD is een [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) functie.

### <a name="conditional-access-enforce-additional-security-requirements-for-high-risk-apps"></a>Voorwaardelijke toegang: Afdwingen aanvullende beveiligingseisen voor apps met een hoog risico
Als u een eenmalige aanmelding tot uw apps en resources hebt ingesteld, kunt u vervolgens meer gevoelige toepassingen beveiligen door af te dwingen specifieke beveiligingsvereisten op elke aanmelding bij deze app. Bijvoorbeeld, kunt u Azure AD op de vraag dat alle toegang tot een bepaalde app multi-factor authentication-server, ongeacht of die app innately deze functionaliteit ondersteunt is altijd vereist. Een andere algemene voorbeeld van voorwaardelijke toegang is om te vereisen dat gebruikers worden verbonden met vertrouwde netwerk van de organisatie om toegang te krijgen tot een toepassing bijzonder gevoelig zijn.

| Artikel handleiding |  |
|:---:| --- |
| Een inleiding tot de mogelijkheden voor voorwaardelijke toegang wordt geboden door Azure AD, Office365 en Intune |[Risico beheren met voorwaardelijke toegang](active-directory-conditional-access-azure-portal.md) |
| Het inschakelen van voorwaardelijke toegang voor de volgende typen resources |[Voorwaardelijke toegang voor SaaS-Apps](active-directory-conditional-access-azure-portal-get-started.md)<br /><br />[Voorwaardelijke toegang voor Office 365-services](active-directory-conditional-access-device-policies.md)<br /><br />[Voorwaardelijke toegang voor On-Premises toepassingen](active-directory-conditional-access-azure-portal.md)<br /><br />[Voorwaardelijke toegang voor On-Premises toepassingen die zijn gepubliceerd via toepassingsproxy van Azure AD](application-proxy-enable-remote-access-sharepoint.md) |
| Apparaten registreren met Azure Active Directory om het beleid voor voorwaardelijke toegang op basis van apparaten inschakelen |[Overzicht van Azure Active Directory-apparaatregistratie](active-directory-conditional-access-device-registration-overview.md)<br /><br />[Automatische apparaatregistratie voor domein inschakelen die lid zijn van de Windows-apparaten](active-directory-conditional-access-automatic-device-registration.md)<br />— [Stappen voor Windows 8.1-apparaten](active-directory-conditional-access-automatic-device-registration-setup.md)<br />— [Stappen voor Windows 7-apparaten](active-directory-conditional-access-automatic-device-registration-setup.md) |

| Het gebruik van de Microsoft Authenticator-app voor verificatie in twee stappen | [Microsoft Authenticator](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) |

Voorwaardelijke toegang is een [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) functie.

## <a name="apps--azure-ad"></a>Apps en Azure AD
### <a name="cloud-app-discovery-find-which-saas-apps-are-being-used-in-your-organization"></a>Cloud App Discovery: Zoeken welke SaaS-apps in uw organisatie worden gebruikt
Cloud App Discovery helpt IT-afdelingen meer informatie over welke SaaS-apps worden gebruikt in de hele organisatie. Het gebruik van de app kunt meten en populariteit zodat deze welke apps profiteren het meest bepalen kan niet kan worden gebracht onder dat IT controle en waarin wordt geïntegreerd met Azure AD.

| Artikel handleiding |  |
|:---:| --- |
| Een algemeen overzicht van hoe het werkt |[Zoeken naar niet-toegestane cloud-toepassingen met Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md) |
| Zie voor meer informatie in hoe het werkt, met de antwoorden op vragen over privacy |[Beveiliging en Privacy-overwegingen](active-directory-cloudappdiscovery-security-and-privacy-considerations.md) |
| Veelgestelde vragen |[Veelgestelde vragen voor Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx) |
| Zelfstudies voor het implementeren van Cloud App Discovery |[Group Policy Deployment Guide](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)<br /><br />[Implementatiehandleiding van System Center](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)<br /><br />[Installeren op proxyservers met aangepaste poorten](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md) |
| Het logboekbestand op updates voor de agent voor Cloud App Discovery |[Wijzigingslogboek](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx) |

Cloud App Discovery is een [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) functie.

### <a name="automatically-provision-and-deprovision-user-accounts-in-saas-apps"></a>Automatisch in te richten en inrichting ervan ongedaan gebruikersaccounts in de SaaS-apps
Het maken, het onderhoud en het verwijderen van gebruikers-id's in de SaaS-toepassingen zoals Dropbox, Salesforce en ServiceNow automatiseren. Overeenkomen met en bestaande identiteiten tussen Azure AD synchroniseren en uw SaaS-apps en toegang beheren accounts automatisch uit te schakelen wanneer gebruikers de organisatie heeft verlaten.

| Artikel handleiding |  |
|:---:| --- |
| Meer informatie over hoe het werkt en vindt u antwoorden op veelgestelde vragen |[Gebruiker inrichting & opheffen van inrichting tot SaaS-Apps te automatiseren](active-directory-saas-app-provisioning.md) |
| Configureren hoe de gegevens tussen Azure AD is toegewezen en uw SaaS-Apps |[Kenmerktoewijzingen aanpassen](active-directory-saas-customizing-attribute-mappings.md)<br><br>[Expressies voor kenmerktoewijzingen schrijven](active-directory-saas-writing-expressions-for-attribute-mappings.md) |
| Het inschakelen van geautomatiseerde inrichting naar alle Apps die ondersteuning biedt voor het protocol SCIM |[Geautomatiseerde Gebruikersinrichting naar alle Apps SCIM-Enabled instellen](active-directory-scim-provisioning.md) |
| Het rapport op en gebruikersaanvragen oplossen |[Rapportage over automatisch gebruikers inrichten](active-directory-saas-provisioning-reporting.md)<br><br>[Inrichting van meldingen](active-directory-saas-account-provisioning-notifications.md)<br><br>[Het oplossen van gebruikers inrichten](active-directory-application-provisioning-content-map.md) |
| Limiet die voor een toepassing op basis van de kenmerkwaarden opgehaald ingericht |[Bereikfilters](active-directory-saas-scoping-filters.md) |

Geautomatiseerde gebruikersinrichting is beschikbaar voor alle edities van Azure AD voor maximaal tien apps per gebruiker. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) onbeperkte toepassingen worden ondersteund. Als uw organisatie heeft [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) of [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), kunt u [groepen gebruiken voor het beheren van welke gebruikers ophalen ingericht](#managing-access-to-applications).

### <a name="building-applications-that-integrate-with-azure-ad"></a>Toepassingen maken die kunnen worden geïntegreerd met Azure AD
Als uw organisatie ontwikkelt of onderhouden van line-of-business (LoB)-toepassingen, of als u een appontwikkelaars met klanten die gebruikmaken van Azure Active Directory, de volgende zelfstudies helpt integreren u uw toepassingen met Azure AD.

| Artikel handleiding |  |
|:---:| --- |
| Richtlijnen voor IT-professionals en ontwikkelaars van toepassingen over de apps integratie met Azure AD |[IT Pro van handleiding voor het ontwikkelen van toepassingen voor Azure AD](active-directory-applications-guiding-developers-for-lob-applications.md)<br /><br />[De handleiding voor ontwikkelaars voor Azure Active Directory](active-directory-developers-guide.md) |
| Hoe toepassing leveranciers kunnen toevoegen hun apps aan de App-galerie van Azure AD |[Lijst van uw toepassing in de Azure Active Directory-Toepassingsgalerie](active-directory-app-gallery-listing.md) |
| Het beheren van toegang tot ontwikkelde toepassingen met Azure Active Directory |[Gebruikerstoewijzing van de voor ontwikkelde toepassingen inschakelen](active-directory-applications-guiding-developers-requiring-user-assignment.md)<br /><br />[Toewijzen van gebruikers aan uw App](active-directory-applications-guiding-developers-assigning-users.md)<br /><br />[Groep toewijzen aan uw App](active-directory-applications-guiding-developers-assigning-groups.md) |

Als u consumententoepassingen ontwikkelt, hebt u mogelijk geïnteresseerd in met behulp van [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) zodat u niet hebt voor het ontwikkelen van uw eigen identiteitssysteem voor het beheren van uw gebruikers. [Meer informatie](../active-directory-b2c/active-directory-b2c-overview.md).

## <a name="managing-access-to-applications"></a>Het beheren van toegang tot toepassingen
### <a name="using-groups-and-self-service-to-manage-who-has-access-to-which-apps"></a>Met behulp van groepen en selfservice beheren die toegang tot welke apps heeft
Om te beheren die toegang moeten hebben tot welke bronnen, kunt Azure Active Directory u machtigingen en toewijzingen instellen op grote schaal met behulp van groepen. IT kan kiezen selfservice-functies inschakelen zodat gebruikers eenvoudig machtiging aanvragen kunnen als dat nodig.

| Artikel handleiding |  |
|:---:| --- |
| Een overzicht van Azure AD access management-functies |[Inleiding tot het beheren van toegang tot Apps](active-directory-managing-access-to-apps.md)<br /><br />[De werking van toegangsbeheer in Azure AD](active-directory-manage-groups.md)<br /><br />[Groepen gebruiken voor het beheren van toegang tot SaaS-toepassingen](active-directory-accessmanagement-group-saasapps.md) |
| Beheer van apps en groepen selfservice inschakelen |[Selfservice Toepassingsbeheer](active-directory-self-service-application-access.md)<br /><br />[Self-Service Group Management](active-directory-accessmanagement-self-service-group-management.md) |
| Instructies voor het instellen van de groepen in Azure AD |[Het maken van beveiligingsgroepen](active-directory-groups-create-azure-portal.md)<br /><br />[Het opgeven van de eigenaars van een groep](active-directory-accessmanagement-managing-group-owners.md)<br /><br />[Het gebruik van de groep 'Alle gebruikers'](active-directory-accessmanagement-dedicated-groups.md) |
| Dynamische groepen gebruiken voor het automatisch vullen van groepslidmaatschappen met lidmaatschapsregels op basis van kenmerken |[Lidmaatschap van dynamische groep: Geavanceerde regels](active-directory-groups-dynamic-membership-azure-portal.md)<br /><br />[Het lidmaatschap van dynamische groepen oplossen](active-directory-accessmanagement-troubleshooting.md) |

Beheer van toegang van toepassing op basis van een groep is beschikbaar voor [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) en [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Self-service groepsbeheer, selfservice Toepassingsbeheer en dynamische groepen zijn [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) functies.

### <a name="b2b-collaboration-enable-partner-access-to-applications"></a>B2B-samenwerking: Partner toegang tot toepassingen inschakelen
Als uw bedrijf met andere bedrijven samen werkt, is het waarschijnlijk dat u moet voor het beheren van de accountpartner toegang tot uw zakelijke toepassingen. Azure Active Directory B2B-samenwerking biedt een eenvoudige en veilige manier om uw apps delen met partners.

| Artikel handleiding |  |
|:---:| --- |
| Een overzicht van andere Azure AD functies dat kunt u helpt externe gebruikers zoals partners, klanten, enzovoort. |[Vergelijking van mogelijkheden voor het beheren van externe identiteiten in Azure AD](active-directory-b2b-compare-external-identities.md) |
| Een inleiding tot B2B-samenwerking en hoe u aan de slag |[Veilige, eenvoudige Cloud Partner-integratie met Azure AD](active-directory-b2b-what-is-azure-ad-b2b.md)<br /><br />[Azure Active Directory B2B Collaboration](active-directory-b2b-collaboration-overview.md) |
| Zie voor meer informatie naar Azure AD B2B-samenwerking en het gebruik ervan |[B2B-samenwerking: Hoe het werkt](active-directory-b2b-how-it-works.md)<br /><br />[Huidige beperkingen van Azure AD B2B-samenwerking](active-directory-b2b-current-limitations.md)<br /><br />[Gedetailleerd overzicht van het gebruik van Azure AD B2B-samenwerking](active-directory-b2b-detailed-walkthrough.md) |
| Verwijzing naar artikelen met technische gegevens over de werking van Azure AD B2B-samenwerking |[CSV-indeling voor het toevoegen van gebruikers van partners](active-directory-b2b-references-csv-file-format.md)<br /><br />[De kenmerken van de gebruiker is van invloed op een Azure AD B2B-samenwerking](active-directory-b2b-references-external-user-object-attribute-changes.md)<br /><br />[Token indeling van de gebruiker voor de Partner-gebruikers](active-directory-b2b-references-external-user-token-format.md) |

B2B-samenwerking is momenteel beschikbaar voor [alle edities van Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="access-panel-a-portal-for-accessing-apps-and-self-service-features"></a>Toegangspaneel: Een portal voor toegang tot apps en selfservice-functies
De Azure AD-Toegangsvenster is waar eindgebruikers kunnen hun apps starten en toegang tot de selfservicefuncties voor het beheren van hun apps en groepslidmaatschappen. Naast het toegangsvenster andere opties voor toegang tot apps SSO-functionaliteit opgenomen in de onderstaande lijst.

| Artikel handleiding |  |
|:---:| --- |
| Een vergelijking van de verschillende opties die beschikbaar zijn voor het implementeren van apps voor eenmalige aanmelding voor gebruikers |[Implementatie van Azure AD geïntegreerde toepassingen voor gebruikers](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) |
| Een overzicht van het toegangsvenster en de mobiele gelijkwaardige MyApps |[Inleiding tot het toegangsvenster en MyApps](active-directory-saas-access-panel-introduction.md)<br />— [iOS](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8)<br />— [Android](https://play.google.com/store/apps/details?id=com.microsoft.myapps) |
| Toegang tot Azure AD-apps van de Office 365-website |[Via de Office 365-App starten](https://support.office.com/en-us/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a) |
| Toegang tot Azure AD-apps uit de mobiele app van Intune Managed Browser |[Intune Managed Browser](https://technet.microsoft.com/en-us/library/dn878029.aspx)<br />— [iOS](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8)<br />— [Android](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser) |
| Toegang tot Azure AD-apps met dieptekoppelingen initiëren eenmalige aanmelding |[Directe aanmelding koppelingen naar uw Apps ophalen](active-directory-appssoaccess-whatis.md#direct-sign-on-links-for-federated-password-based-or-existing-apps) |

Toegangspaneel is beschikbaar voor [alle edities van Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="reports-easily-audit-app-access-changes-and-monitor-sign-ins-to-apps"></a>Rapporten: Eenvoudig wijzigingen aan de app toegang controleren en aanmeldingen naar apps bewaken
Azure Active Directory biedt diverse rapporten en waarschuwingen om u te helpen bij het bewaken van uw organisatie toegang tot toepassingen. U kunt waarschuwingen voor afwijkende aanmeldingen ontvangen op uw apps en u kunt volgen wanneer en waarom een gebruiker toegang tot een toepassing is gewijzigd.

| Artikel handleiding |  |
|:---:| --- |
| Een overzicht van de rapportagefuncties in Azure Active Directory |[Aan de slag met Azure AD-rapportage](active-directory-reporting-getting-started.md) |
| De aanmeldingen en het gebruik van uw gebruikers Apps bewaken |[Toegangs- en gebruiksrapporten weergeven](active-directory-view-access-usage-reports.md) |
| Het bijhouden van wijzigingen aangebracht aan wie toegang heeft tot een bepaalde toepassing |[Azure Active Directory-Controlerapportgebeurtenissen](active-directory-reporting-audit-events.md) |
| De gegevens van deze rapporten exporteren naar uw voorkeur's met behulp van de rapportage-API |[Aan de slag met Azure AD rapportage-API](active-directory-reporting-api-getting-started.md) |

Om te zien welke rapporten zijn opgenomen in verschillende versies van Azure Active Directory, [Klik hier](active-directory-view-access-usage-reports.md).

## <a name="see-also"></a>Zie ook
[Wat is Azure Active Directory?](active-directory-whatis.md)

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/)

[Azure Active Directory Domain Services](https://azure.microsoft.com/services/active-directory-ds/)

[Azure Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/)
