---
title: Article Index for Application Management in Azure Active Directory | Microsoft Azure
description: Informatie over het aanpassen van de vervaldatum voor de federatiecertificaten en het vernieuwen van certificaten die binnenkort vervallen.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: cde70518164c386697127b079979b5b3b76ae088
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056882"
---
# <a name="article-index-for-application-management-in-azure-active-directory"></a>Article Index for Application Management in Azure Active Directory (Engelstalig)
Deze pagina bevat een uitgebreide lijst van elk document geschreven over de verschillende functies van de toepassing in Azure Active Directory (Azure AD).

Er is een korte inleiding op elke primaire functiegebied, evenals informatie over welke artikelen te lezen, afhankelijk van welke gegevens u zoekt.

## <a name="overview-articles"></a>Overzicht van artikelen
De onderstaande artikelen zijn goed uitgangspunt voor gebruikers die alleen een korte beschrijving van de Azure AD-toepassing-beheerfuncties wilt.

| Artikel-handleiding |  |
|:---:| --- |
| Een inleiding tot de application management-problemen die Azure AD is opgelost |[Toepassingen beheren met Azure Active Directory (AD)](manage-apps/what-is-application-management.md) |
| Een overzicht van de verschillende functies in Azure AD met betrekking tot het inschakelen van single sign-on, definiëren wie toegang heeft tot apps en hoe gebruikers apps starten |[Toegang tot toepassingen en eenmalige aanmelding in Azure Active Directory](manage-apps/what-is-single-sign-on.md) |
| Bekijk de verschillende stappen die betrokken zijn bij het integreren van apps in uw Azure AD |[Azure Active Directory integreren met toepassingen](manage-apps/plan-an-application-integration.md)<br /><br />[Inschakelen van Single Sign-On voor SaaS-Apps](manage-apps/configure-single-sign-on-portal.md)<br /><br />[Toegang tot Apps beheren](manage-apps/what-is-access-management.md) |
| Een technische uitleg van hoe apps worden weergegeven in Azure AD |[Hoe en waarom worden toepassingen toegevoegd aan Azure AD](active-directory-how-applications-are-added.md) |

## <a name="troubleshooting-articles"></a>Artikelen over probleemoplossing
In deze sectie biedt snel toegang tot relevante handleidingen voor probleemoplossing. Meer informatie over elke functie kan worden gevonden op de rest van deze pagina.

| Functiegebied |  |
|:---:| --- |
| Federatieve eenmalige aanmelding |[Oplossen van problemen met SAML gebaseerde eenmalige aanmelding](develop/howto-v1-debug-saml-sso-issues.md) |
| Wachtwoord gebaseerde Single Sign-On |[Het oplossen van het Configuratiescherm-extensie voor toegang voor Internet Explorer](active-directory-saas-ie-troubleshooting.md) |
| Toepassingsproxy |[App-Proxy oplossen handleiding](manage-apps/application-proxy-troubleshoot.md) |
| Eenmalige aanmelding tussen on-premises AD en Azure AD |[Oplossen van problemen met wachtwoord-Hashsynchronisatie](connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md#troubleshoot-password-hash-synchronization)<br /><br />[Oplossen van problemen met wachtwoord terugschrijven](authentication/active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Lidmaatschap van dynamische groepen |[Het oplossen van lidmaatschap van dynamische groepen](users-groups-roles/groups-troubleshooting.md) |

## <a name="single-sign-on-sso"></a>Eenmalige aanmelding (SSO)
### <a name="federated-single-sign-on-sign-into-many-apps-using-one-identity"></a>Federatieve eenmalige aanmelding: Meld u aan bij veel apps met één identiteit
Hiermee kunnen gebruikers toegang tot een aantal apps en services met behulp van slechts één set referenties voor eenmalige aanmelding. Federatie is een methode waarmee u eenmalige aanmelding kunt inschakelen. Wanneer gebruikers zich proberen aan te melden bij federatieve apps, ze wordt u omgeleid naar van hun organisatie officiële aanmeldingspagina weergegeven door Azure Active Directory en wordt vervolgens omgeleid naar de app bij een geslaagde verificatie.

| Artikel-handleiding |  |
|:---:| --- |
| Een inleiding tot het onderwerp Federatie en andere typen van aanmelding |[Eenmalige aanmelding met Azure AD](manage-apps/what-is-single-sign-on.md) |
| Duizenden SaaS-apps die vooraf geïntegreerd met Azure AD met zijn vereenvoudigde configuratie voor eenmalige aanmelding stappen |[Aan de slag met de Azure AD-toepassingsgalerie](manage-apps/what-is-single-sign-on.md#get-started-with-the-azure-ad-application-gallery)<br /><br />[Volledige lijst met vooraf geïntegreerde Apps die ondersteuning bieden voor Federatie](saas-apps/tutorial-list.md)<br /><br />[Uw App toevoegen aan de App-galerie van Azure AD](develop/howto-app-gallery-listing.md) |
| Meer dan 150 app-zelfstudies over het configureren van eenmalige aanmelding voor apps zoals [Salesforce](saas-apps/salesforce-tutorial.md), [ServiceNow](saas-apps/servicenow-tutorial.md), [Google Apps](saas-apps/google-apps-tutorial.md), [Workday](saas-apps/workday-tutorial.md), en nog veel meer |[Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](saas-apps/tutorial-list.md) |
| Het handmatig instellen en aanpassen van uw configuratie voor eenmalige aanmelding |[Hoe om te configureren voor federatieve eenmalige aanmelding naar Apps die zich niet in de galerie van Azure Active Directory-toepassing](application-config-sso-how-to-configure-federated-sso-non-gallery.md)<br /><br />[In het SAML-Token voor vooraf geïntegreerde Apps uitgegeven Claims aanpassen](active-directory-saml-claims-customization.md) |
| Gids voor probleemoplossing voor federatieve apps die gebruikmaken van het SAML-protocol |[Oplossen van problemen met SAML gebaseerde eenmalige aanmelding](develop/howto-v1-debug-saml-sso-issues.md) |
| Vervaldatum van het certificaat van uw app configureren en hoe u uw certificaten vernieuwen |[Certificaten voor federatieve eenmalige aanmelding in Azure Active Directory beheren](manage-apps/manage-certificates-for-federated-single-sign-on.md) |

Federatieve eenmalige aanmelding is beschikbaar voor alle edities van Azure AD voor maximaal tien apps per gebruiker. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) onbeperkt aantal toepassingen ondersteunt. Als uw organisatie heeft [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) of [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), kunt u [groepen gebruiken voor toegang tot federatieve toepassingen toewijzen](#managing-access-to-applications).

### <a name="password-based-single-sign-on-account-sharing-and-sso-for-non-federated-apps"></a>Op basis van wachtwoorden Single Sign-On: Account delen en eenmalige aanmelding voor niet-federatieve apps
Om in te schakelen single sign-on bij toepassingen die bieden geen ondersteuning voor Federatie, biedt Azure AD wachtwoord functies waarmee u kunnen veilig opslaan van wachtwoorden met SaaS-apps en gebruikers automatisch aangemeld bij deze apps. U kunt eenvoudig referenties voor nieuwe accounts distribueren en team accounts delen met meerdere personen. Gebruikers hoeft niet te weten de referenties voor de accounts die deze toegang tot krijgen rapporten.

| Artikel-handleiding |  |
|:---:| --- |
| Een inleiding tot hoe op wachtwoord gebaseerde SSO werkt en een korte technisch overzicht |[Wachtwoord gebaseerde eenmalige aanmelding met Azure AD](manage-apps/what-is-single-sign-on.md#password-based-single-sign-on) |
| Een samenvatting van de scenario's met betrekking tot het delen van account en hoe deze problemen worden opgelost door Azure AD |[Accounts delen met Azure AD](active-directory-sharing-accounts.md) |
| Het wachtwoord voor bepaalde apps automatisch op een vast interval wijzigen |[Automatische Wachtwoordoverschakeling (preview)](https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Implementatie en probleemoplossing handleidingen voor de Internet Explorer versie van de Azure AD wachtwoord-beheeruitbreiding |[Over het implementeren van het Configuratiescherm-extensie voor toegang voor Internet Explorer met behulp van Groepsbeleid](active-directory-saas-ie-group-policy.md)<br /><br />[Het oplossen van het Configuratiescherm-extensie voor toegang voor Internet Explorer](active-directory-saas-ie-troubleshooting.md) |

Wachtwoord gebaseerde eenmalige aanmelding is beschikbaar voor alle edities van Azure AD voor maximaal tien apps per gebruiker. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) onbeperkt aantal toepassingen ondersteunt. Als uw organisatie heeft [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) of [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), kunt u [groepen gebruiken voor toegang tot toepassingen toewijzen](#managing-access-to-applications). Automatische wachtwoordoverschakeling is een [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) functie.

### <a name="app-proxy-single-sign-on-and-remote-access-to-on-premises-applications"></a>App-Proxy: Single sign-on en externe toegang tot on-premises toepassingen
Als u toepassingen in uw particuliere netwerk die moeten worden geopend door gebruikers en apparaten buiten het netwerk hebt, kunt u Azure AD-toepassingsproxy gebruiken om in te schakelen van veilige externe toegang tot apps die worden.

| Artikel-handleiding |  |
|:---:| --- |
| Overzicht van Azure AD-toepassingsproxy en hoe het werkt |[Beveiligde toegang externe tot on-premises toepassingen](manage-apps/application-proxy.md) |
| Zelfstudies over het configureren van Application Proxy en hoe u uw eerste app publiceren |[Over het instellen van Azure AD-toepassingsproxy](manage-apps/application-proxy-enable.md)<br /><br />[Op de achtergrond installeren de App Proxy-Connector](manage-apps/application-proxy-register-connector-powershell.md)<br /><br />[Het publiceren van toepassingen via App Proxy](manage-apps/application-proxy-publish-azure-portal.md)<br /><br />[Het gebruik van uw eigen domeinnaam](manage-apps/application-proxy-configure-custom-domain.md) |
| Het inschakelen van eenmalige aanmelding en voorwaardelijke toegang voor apps die zijn gepubliceerd met toepassingsproxy |[Single-sign-on met Application Proxy](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)<br /><br />[Voorwaardelijke toegang en Application Proxy](manage-apps/application-proxy-integrate-with-sharepoint-server.md) |
| Richtlijnen voor het gebruik van Application Proxy voor de volgende scenario 's |[Hoe u ter ondersteuning van Native Client-toepassingen](manage-apps/application-proxy-configure-native-client-application.md)<br /><br />[Het ondersteunen van Claims-compatibele toepassingen](manage-apps/application-proxy-configure-for-claims-aware-applications.md)<br /><br />[Hoe u ondersteuning bieden voor toepassingen die zijn gepubliceerd op afzonderlijke netwerken en -locaties](manage-apps/application-proxy-connector-groups.md) |
| Gids voor probleemoplossing voor toepassingsproxy |[App-Proxy oplossen handleiding](manage-apps/application-proxy-troubleshoot.md) |

Application Proxy is beschikbaar voor alle edities van Azure AD voor maximaal tien apps per gebruiker. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) onbeperkt aantal toepassingen ondersteunt. Als uw organisatie heeft [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) of [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), kunt u [groepen gebruiken voor toegang tot toepassingen toewijzen](#managing-access-to-applications).

Bent u mogelijk ook geïnteresseerd in [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md), waarmee u uw on-premises toepassingen naar Azure migreren terwijl nog steeds voldoen aan de behoeften van de identiteit van deze toepassingen.

### <a name="enabling-single-sign-on-between-azure-ad-and-on-premises-ad"></a>Inschakelen van eenmalige aanmelding tussen Azure AD en on-premises AD
Als uw organisatie een Windows Server Active Directory on-premises, samen met uw Azure Active Directory in de cloud, onderhoudt en vervolgens u wellicht wilt, om in te schakelen eenmalige aanmelding tussen deze twee systemen. Azure AD Connect (het hulpprogramma die samen deze twee systemen integreert) biedt meerdere opties voor het instellen van eenmalige aanmelding: Federatie met AD FS of een andere federatieprovider tot stand brengen of wachtwoordsynchronisatie inschakelen.

| Artikel-handleiding |  |
|:---:| --- |
| Een overzicht van de opties voor eenmalige aanmelding wordt aangeboden in Azure AD Connect, evenals informatie over het beheren van hybride omgevingen |[Aanmelding door een gebruiker op opties in Azure AD Connect](active-directory-aadconnect-user-signin.md) |
| Algemene richtlijnen voor het beheren van omgevingen met zowel on-premises Active Directory en Azure Active Directory |[Ontwerpoverwegingen voor Azure AD-hybride identiteit](active-directory-hybrid-identity-design-considerations-overview.md)<br /><br />[Uw On-Premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md) |
| Informatie over het gebruik van Password Sync SSO inschakelen |[Implement Password Synchronization met Azure AD Connect](connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md)<br /><br />[Problemen met Wachtwoordsynchronisatie oplossen](https://support.microsoft.com/kb/2855271) |
| Informatie over het gebruik van wachtwoord terugschrijven SSO inschakelen |[Aan de slag met wachtwoordbeheer in Azure AD](authentication/quickstart-sspr.md)<br /><br />[Problemen met Wachtwoord terugschrijven oplossen](authentication/active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Informatie over het gebruik van de id-providers van derden SSO inschakelen |[Lijst met compatibele externe id-Providers die kunnen worden gebruikt voor eenmalige aanmelding inschakelen](https://aka.ms/ssoproviders) |
| Hoe Windows 10-gebruikers kunnen profiteren van de voordelen van eenmalige aanmelding via de Azure AD Join |[Cloudmogelijkheden worden uitgebreid naar Windows Join 10-apparaten via Azure Active Directory](active-directory-azureadjoin-overview.md) |

Azure AD Connect is beschikbaar voor [alle edities van Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). Azure AD selfservice voor wachtwoordherstel is beschikbaar voor [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) en [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Wachtwoord terugschrijven naar on-premises AD is een [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) functie.

### <a name="conditional-access-enforce-additional-security-requirements-for-high-risk-apps"></a>Voorwaardelijke toegang: Afdwingen aanvullende beveiligingsvereisten voor apps met een hoog risico
Als u een eenmalige aanmelding voor uw apps en resources hebt ingesteld, kunt u vervolgens verder gevoelige toepassingen beveiligen door af te dwingen van specifieke beveiligingsvereisten in elke aanmelding van de App. Bijvoorbeeld, kunt u Azure AD aan de vraag alle toegang tot een bepaalde app vereisen altijd meervoudige verificatie, ongeacht of die app innately deze functionaliteit ondersteunt. Er is een ander algemeen voorbeeld van voorwaardelijke toegang om te vereisen dat gebruikers worden verbonden met het vertrouwde netwerk van de organisatie voor toegang tot een toepassing bijzonder gevoelig zijn.

| Artikel-handleiding |  |
|:---:| --- |
| Een inleiding tot de mogelijkheden voor voorwaardelijke toegang wordt aangeboden via Azure AD, Office 365 en Intune |[Risico beheren met voorwaardelijke toegang](active-directory-conditional-access-azure-portal.md) |
| Het inschakelen van voorwaardelijke toegang voor de volgende typen resources |[Voorwaardelijke toegang voor SaaS-Apps](conditional-access/app-based-conditional-access.md)<br /><br />[Voorwaardelijke toegang voor Office 365-services](active-directory-conditional-access-device-policies.md)<br /><br />[Voorwaardelijke toegang voor On-Premises toepassingen](active-directory-conditional-access-azure-portal.md)<br /><br />[Voorwaardelijke toegang voor On-Premises toepassingen die zijn gepubliceerd via toepassingsproxy van Azure AD](manage-apps/application-proxy-integrate-with-sharepoint-server.md) |
| Informatie over het registreren van apparaten met Azure Active Directory om het beleid voor voorwaardelijke toegang op basis van apparaat inschakelen |[Overzicht van Azure Active Directory Device Registration service](active-directory-conditional-access-device-registration-overview.md)<br /><br />[Automatische apparaatregistratie inschakelen voor domein gekoppelde Windows-apparaten](active-directory-conditional-access-automatic-device-registration.md)<br />— [Stappen voor Windows 8.1-apparaten](active-directory-conditional-access-automatic-device-registration-setup.md)<br />— [Stappen voor Windows 7-apparaten](active-directory-conditional-access-automatic-device-registration-setup.md) |

| Over het gebruik van de Microsoft Authenticator-app voor verificatie in twee stappen | [Microsoft Authenticator](user-help/microsoft-authenticator-app-how-to.md) |

Voorwaardelijke toegang is een [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) functie.

## <a name="apps--azure-ad"></a>Apps en Azure AD
### <a name="cloud-discovery-find-which-saas-apps-are-being-used-in-your-organization"></a>Cloud Discovery: Vinden welke SaaS-apps in uw organisatie worden gebruikt
Cloud Discovery worden uw verkeerslogboeken op basis van Microsoft Cloud App Security van cloud app-catalogus van meer dan 16.000 cloud-apps die worden beoordeeld en gewaardeerd op basis van meer dan 70 risicofactoren geanalyseerd, zodat u altijd inzicht hebt in de cloud gebruikt, schaduw-IT, en het risico Schaduw IT poses in uw organisatie.

| Artikel-handleiding |  |
|:---:| --- |
| Een algemeen overzicht van hoe het werkt |[Cloud Discovery instellen](/cloud-app-security/set-up-cloud-discovery) |


### <a name="automatically-provision-and-deprovision-user-accounts-in-saas-apps"></a>Automatisch inrichten en de inrichting ongedaan maken van gebruikersaccounts in SaaS-apps
Automatiseer het maken, onderhoud en verwijderen van gebruikers-id's in SaaS-toepassingen, zoals Dropbox, Salesforce, ServiceNow en meer. Overeenkomen en synchronisatie van bestaande identiteiten tussen Azure AD en uw SaaS-apps en toegang beheren accounts automatisch uit te schakelen wanneer gebruikers de organisatie verlaten.

| Artikel-handleiding |  |
|:---:| --- |
| Meer informatie over hoe het werkt en vind antwoorden op veelgestelde vragen |[Gebruiker inrichting en ongedaan maken van inrichting voor SaaS-toepassingen automatiseren](active-directory-saas-app-provisioning.md) |
| Configureren hoe gegevens tussen Azure AD is toegewezen en uw SaaS-app |[Kenmerktoewijzingen aanpassen](active-directory-saas-customizing-attribute-mappings.md)<br><br>[Expressies schrijven voor kenmerktoewijzingen](active-directory-saas-writing-expressions-for-attribute-mappings.md) |
| Het inschakelen van geautomatiseerde inrichting naar alle Apps die ondersteuning biedt voor het protocol SCIM |[Instellen van geautomatiseerde Gebruikersinrichting naar alle Apps SCIM-Enabled](manage-apps/use-scim-to-provision-users-and-groups.md) |
| Het rapport op en inrichten van gebruikers oplossen |[Rapportage over het automatisch inrichten van gebruikers](active-directory-saas-provisioning-reporting.md)<br><br>[Problemen met het inrichten van gebruikers oplossen](active-directory-application-provisioning-content-map.md) |
| Beperken aan wie die voor een toepassing op basis van hun kenmerkwaarden opgehaald ingericht |[Bereikfilters](active-directory-saas-scoping-filters.md) |

Geautomatiseerde gebruikersinrichting is beschikbaar voor alle edities van Azure AD voor maximaal tien apps per gebruiker. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) onbeperkt aantal toepassingen ondersteunt. Als uw organisatie heeft [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) of [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), kunt u [groepen gebruiken voor het beheren van welke gebruikers ingericht](#managing-access-to-applications).

### <a name="building-applications-that-integrate-with-azure-ad"></a>Toepassingen ontwikkelen die kunnen worden geïntegreerd met Azure AD
Als uw organisatie ontwikkelt of onderhouden van line-of-business (LoB)-toepassingen, of als u een app-ontwikkelaar bij klanten die gebruikmaken van Azure Active Directory bent, in de volgende zelfstudies kunt integreren u uw toepassingen met Azure AD.

| Artikel-handleiding |  |
|:---:| --- |
| Richtlijnen voor IT-professionals en ontwikkelaars van toepassingen voor het integreren van apps met Azure AD |[De IT van de Pro-handleiding voor het ontwikkelen van toepassingen voor Azure AD](active-directory-applications-guiding-developers-for-lob-applications.md)<br /><br />[De Developer's Guide voor Azure Active Directory](develop/azure-ad-developers-guide.md) |
| Hoe aan toepassing leveranciers kunnen hun apps toevoegen aan de App-galerie van Azure AD |[Lijst van uw toepassing in de Azure Active Directory-Toepassingsgalerie](develop/howto-app-gallery-listing.md) |
| Over het beheren van toegang tot ontwikkelde toepassingen met Azure Active Directory |[Gebruikerstoewijzing voor ontwikkelde toepassingen inschakelen](active-directory-applications-guiding-developers-requiring-user-assignment.md)<br /><br />[Gebruikers toewijzen aan uw App](active-directory-applications-guiding-developers-assigning-users.md)<br /><br />[Groep toewijzen aan uw App](active-directory-applications-guiding-developers-assigning-groups.md) |

Als u consumentgerichte toepassingen ontwikkelt, kunt u mogelijk geïnteresseerd in met behulp van [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) zodat u uw eigen identiteitssysteem voor het beheren van uw gebruikers te ontwikkelen. [Meer informatie](../active-directory-b2c/active-directory-b2c-overview.md).

## <a name="managing-access-to-applications"></a>Beheer van toegang tot toepassingen
### <a name="using-groups-and-self-service-to-manage-who-has-access-to-which-apps"></a>Groepen en selfservice voor het beheren van wie toegang tot welke apps heeft gebruiken
Kunt u beheren wie toegang moeten hebben tot welke resources kunt Azure Active Directory u machtigingen voor en toewijzingen op schaal met behulp van groepen instellen. IT kan kiezen om in te schakelen van selfservice-functies, zodat gebruikers alleen machtigingen aanvragen kunnen wanneer ze deze nodig.

| Artikel-handleiding |  |
|:---:| --- |
| Een overzicht van Azure AD access management-functies |[Inleiding tot het beheren van toegang tot Apps](manage-apps/what-is-access-management.md)<br /><br />[Hoe toegangsbeheer werkt in Azure AD](fundamentals/active-directory-manage-groups.md)<br /><br />[Hoe u groepen gebruiken voor het beheren van toegang tot SaaS-toepassingen](users-groups-roles/groups-saasapps.md) |
| Zelfservicebeheer van apps en -groepen inschakelen |[Self-Servicetoepassingen beheren](active-directory-self-service-application-access.md)<br /><br />[Groepsbeheer via self-Service](users-groups-roles/groups-self-service-management.md) |
| Instructies voor het instellen van uw groepen in Azure AD |[Over het maken van beveiligingsgroepen](fundamentals/active-directory-groups-create-azure-portal.md)<br /><br />[Het opgeven van eigenaars voor een groep](fundamentals/active-directory-accessmanagement-managing-group-owners.md)<br /><br />[Het gebruik van de groep 'Alle gebruikers'](active-directory-accessmanagement-dedicated-groups.md) |
| Dynamische groepen gebruiken voor het automatisch vullen van groepslidmaatschappen met behulp van regels voor groepslidmaatschap op basis van een kenmerk |[Dynamisch groepslidmaatschap: Geavanceerde regels](active-directory-groups-dynamic-membership-azure-portal.md)<br /><br />[Het oplossen van lidmaatschap van dynamische groepen](users-groups-roles/groups-troubleshooting.md) |

Toegangsbeheer voor toepassingen op basis van een groep is beschikbaar voor [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) en [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Self-service groepsbeheer, self-Servicetoepassingen beheren en dynamische groepen zijn [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) functies.

### <a name="b2b-collaboration-enable-partner-access-to-applications"></a>B2B-samenwerking: Partner toegang tot toepassingen inschakelen
Als uw bedrijf is een partnerschap aangegaan met andere bedrijven, is het waarschijnlijk dat u wilt beheren partner toegang tot uw zakelijke toepassingen. Active Directory B2B-samenwerking van Azure biedt een eenvoudige en veilige manier om uw apps delen met partners.

| Artikel-handleiding |  |
|:---:| --- |
| Een overzicht van andere Azure AD-functies die kunt u helpt externe gebruikers, zoals partners, klanten, enzovoort. |[Vergelijking van mogelijkheden voor het beheer van externe identiteiten in Azure AD](active-directory-b2b-compare-external-identities.md) |
| Een inleiding tot B2B-samenwerking en hoe u aan de slag |[Eenvoudige, veilige Cloud Partner-integratie met Azure AD](active-directory-b2b-what-is-azure-ad-b2b.md)<br /><br />[Azure Active Directory B2B-samenwerking](active-directory-b2b-collaboration-overview.md) |
| Meer informatie over Azure AD B2B-samenwerking en het gebruik ervan |[B2B-samenwerking: Hoe het werkt](active-directory-b2b-how-it-works.md)<br /><br />[Huidige beperkingen van Azure AD B2B-samenwerking](active-directory-b2b-current-limitations.md)<br /><br />[Gedetailleerd overzicht van het gebruik van Azure AD B2B-samenwerking](active-directory-b2b-detailed-walkthrough.md) |
| Naslaginformatie over artikelen met technische gegevens over de werking van Azure AD B2B-samenwerking |[CSV-indeling voor het toevoegen van gebruikers van partnerorganisaties](active-directory-b2b-references-csv-file-format.md)<br /><br />[Gebruikerskenmerken die worden beïnvloed door Azure AD B2B-samenwerking](active-directory-b2b-references-external-user-object-attribute-changes.md)<br /><br />[Indeling van Token voor gebruikers van partnerorganisaties](active-directory-b2b-references-external-user-token-format.md) |

B2B-samenwerking is momenteel beschikbaar voor [alle edities van Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="access-panel-a-portal-for-accessing-apps-and-self-service-features"></a>Toegangsvenster: Een portal voor toegang tot apps en selfservice-functies
De Azure AD-Toegangsvenster is waar eindgebruikers kunnen hun apps starten en toegang tot de selfservice-functies waarmee ze voor het beheren van hun apps en groepslidmaatschappen. Naast het toegangsvenster, worden de andere opties voor toegang tot apps met eenmalige aanmelding ingeschakeld opgenomen in de onderstaande lijst.

| Artikel-handleiding |  |
|:---:| --- |
| Een vergelijking van de verschillende opties die beschikbaar zijn voor het implementeren van apps voor eenmalige aanmelding voor gebruikers |[Implementatie van Azure AD geïntegreerde toepassingen voor gebruikers](manage-apps/what-is-single-sign-on.md#deploying-azure-ad-integrated-applications-to-users) |
| Een overzicht van het toegangsvenster en de mobiele gelijkwaardige MyApps |[Inleiding tot het toegangsvenster en de MyApps](user-help/active-directory-saas-access-panel-introduction.md)<br />— [iOS](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8)<br />— [Android](https://play.google.com/store/apps/details?id=com.microsoft.myapps) |
| Over toegang tot Azure AD-apps vanuit de Office 365-website |[Met behulp van het startprogramma voor Office 365](https://support.office.com/en-us/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a) |
| Over toegang tot Azure AD-apps vanuit de mobiele app van Intune Managed Browser |[Intune Managed Browser](https://technet.microsoft.com/library/dn878029.aspx)<br />— [iOS](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8)<br />— [Android](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser) |
| Toegang tot Azure AD-apps met dieptekoppelingen initiëren eenmalige aanmelding |[Directe aanmelding koppelingen naar uw Apps ophalen](manage-apps/what-is-single-sign-on.md#direct-sign-on-links-for-federated-password-based-or-existing-apps) |

Toegangsvenster is beschikbaar voor [alle edities van Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="reports-easily-audit-app-access-changes-and-monitor-sign-ins-to-apps"></a>Rapporten: Controle van wijzigingen in de app eenvoudig en aanmeldingen voor apps bewaken
Azure Active Directory biedt verschillende rapporten en waarschuwingen om u te helpen bij het controleren van uw organisatie toegang tot toepassingen. U kunt waarschuwingen voor afwijkende aanmeldingen ontvangt op uw apps en u kunt volgen wanneer en waarom een gebruikers toegang tot een toepassing is gewijzigd.

| Artikel-handleiding |  |
|:---:| --- |
| Een overzicht van de rapportagefuncties in Azure Active Directory |[Aan de slag met Azure AD-rapportage](active-directory-reporting-getting-started.md) |
| Het bewaken van de aanmeldingen en het app-gebruik van uw gebruikers |[Uw toegang en gebruiksrapporten weergeven](active-directory-view-access-usage-reports.md) |
| Het bijhouden van wijzigingen aangebracht aan wie toegang heeft tot een bepaalde toepassing |[Auditrapport gebeurtenissen Azure Active Directory](active-directory-reporting-audit-events.md) |
| De gegevens van deze rapporten exporteren naar uw favoriete hulpprogramma's met behulp van de rapportage-API |[Aan de slag met de rapportage-API van Azure AD](active-directory-reporting-api-getting-started.md) |

Om te zien welke rapporten zijn opgenomen in verschillende versies van Azure Active Directory, [Klik hier](active-directory-view-access-usage-reports.md).

## <a name="see-also"></a>Zie ook
[Wat is Azure Active Directory?](fundamentals/active-directory-whatis.md)

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/)

[Azure Active Directory Domain Services](https://azure.microsoft.com/services/active-directory-ds/)

[Azure multi-factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/)
