---
title: Azure-beveiliging waarmee met identity management-functies | Microsoft Docs
description: " Dit artikel bevat een overzicht van de belangrijkste functies van Azure-beveiliging die u bij het identiteitsbeheer helpen. Microsoft identiteits- en toegangsbeheer management oplossingen zorgen ervoor dat IT beveiligen toegang tot toepassingen en bronnen in het zakelijke datacenter en in de cloud, extra niveaus van validatie van de multi-factor authentication en voorwaardelijke toegang inschakelen beleid. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 5aa0a7ac-8f18-4ede-92a1-ae0dfe585e28
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 921d7eef90a86216dc13804d3dc4dd216247fb87
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-identity-management-security-overview"></a>Overzicht van Azure identity management-beveiliging
Microsoft identiteits- en toegangsbeheer management oplossingen zorgen ervoor dat IT beveiligen toegang tot toepassingen en bronnen in het zakelijke datacenter en in de cloud, extra niveaus van validatie van de multi-factor authentication en voorwaardelijke toegang inschakelen beleid. Bewaking verdachte activiteiten via geavanceerde beveiliging rapportage, controle en waarschuwingen helpt bij het verminderen mogelijke beveiligingsproblemen. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) biedt eenmalige aanmelding tot duizenden cloud-apps (SaaS) en toegang tot web-apps die u on-premises uitgevoerd.

Beveiligingsvoordelen van Azure Active Directory (AD) omvatten de mogelijkheid om te:

* Maken en beheren van één identiteit voor elke gebruiker in uw onderneming hybride, gebruikers, groepen en apparaten synchroon te houden
* Eenmalige aanmelding toegang bieden tot uw toepassingen met inbegrip van duizenden vooraf geïntegreerde SaaS-apps
* Toepassingsbeveiliging toegang inschakelen door het afdwingen van multi-factor Authentication op basis van regels voor zowel on-premises en cloudtoepassingen
* Veilige externe toegang tot on-premises webtoepassingen via Azure AD-toepassingsproxy inrichten

Het doel van dit artikel is een overzicht van de kern van het Azure-beveiliging om functies te leveren die u bij het identiteitsbeheer helpen. We bieden ook koppelingen naar artikelen waarmee details van elk onderdeel, zodat u meer informatie.  

Het artikel is gericht op de volgende mogelijkheden voor core Azure Identity management:

* Eenmalige aanmelding
* Omgekeerde proxy
* Multi-Factor Authentication
* Beveiligingsbewaking, waarschuwingen en machine learning gebaseerde rapporten
* Identiteits- en toegangsbeheer van consumenten
* Apparaatregistratie
* Beschermde identiteitsbeheer
* Identiteitsbeveiliging
* Beheer van hybride identiteit

## <a name="single-sign-on"></a>Eenmalige aanmelding
Één eenmalige aanmelding (SSO) beschikt u toegang tot alle toepassingen en bronnen die u nodig hebt om zaken te doen, wanneer u zich aanmeldt bij het gebruik van slechts één keer één gebruikersaccount. Wanneer u bent aangemeld, kunt u alle toepassingen zonder verificatie openen (bijvoorbeeld: Typ een wachtwoord) een tweede keer.

Veel organisaties zijn afhankelijk van de software als een dienst (SaaS)-toepassingen zoals Office 365, het selectievakje en Salesforce voor de productiviteit van eindgebruikers. In het verleden hebben IT-personeel nodig afzonderlijk maken en bijwerken van gebruikersaccounts in elke SaaS-toepassing en moesten gebruikers een wachtwoord voor elke SaaS-toepassing.

Azure AD breidt on-premises Active Directory-omgevingen in de cloud, zodat gebruikers kunnen hun primaire organisatie-account niet alleen op hun apparaten domein aanmelden en bedrijfsresources gebruiken, maar ook alle de web- en SaaS-toepassingen die nodig zijn voor de taak.

Niet alleen gebruikers hoeven niet te beheren van meerdere sets met gebruikersnamen en wachtwoorden, toegang tot de toepassing kan worden automatisch ingerichte of ongedaan ingerichte op basis van organisatie-groepen en hun status als een werknemer. Azure AD introduceert beveiligings- en toegangsbeheer governance waarmee u kunt het centraal beheren van toegang van gebruikers over SaaS-toepassingen.

Meer informatie:

* [Overzicht van eenmalige aanmelding](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../active-directory/active-directory-appssoaccess-whatis.md)
* [Eenmalige aanmelding Azure Active Directory integreren met SaaS-apps](../active-directory/active-directory-enterprise-apps-manage-sso.md)

## <a name="reverse-proxy"></a>Omgekeerde proxy
Azure AD-toepassingsproxy kunt u on-premises toepassingen, zoals publiceren [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) sites, [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx), en [IIS](http://www.iis.net/)-gebaseerde apps binnen uw particuliere netwerk en veilig toegang verleent aan gebruikers buiten uw netwerk. Toepassingsproxy biedt externe toegang en eenmalige aanmelding (SSO) voor veel soorten on-premises webtoepassingen met de duizenden SaaS-toepassingen die ondersteuning biedt voor Azure AD. Werknemers kunnen aanmelden bij uw apps op hun eigen apparaten home en verifiëren via deze cloud-gebaseerde proxy.

Meer informatie:

* [Azure AD-toepassingsproxy inschakelen](../active-directory/active-directory-application-proxy-enable.md)
* [Toepassingen publiceren met Azure AD-toepassingsproxy](../active-directory/active-directory-application-proxy-publish.md)
* [Single-sign-on met toepassingsproxy](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
* [Werken met voorwaardelijke toegang](../active-directory/application-proxy-enable-remote-access-sharepoint.md)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Azure multi-factor authentication (MFA) is een authenticatiemethode die vereist het gebruik van meer dan één verificatiemethode en een kritieke tweede beveiligingslaag wordt toegevoegd aan de gebruikersaanmeldingen en transacties. MFA helpt beveiliging toegang tot gegevens en toepassingen en te voldoen aan de behoeften van de gebruiker voor een eenvoudig proces aanmelden. Levert sterke verificatie via een aantal opties voor verificatie: telefoonoproep, tekstbericht of mobiele app melding of verificatie van code en van derden OAuth-tokens.

Meer informatie:

* [Multi-factor authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Wat is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* [De werking van Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Beveiligingsbewaking, waarschuwingen en machine learning gebaseerde rapporten
Beveiligingsbewaking en waarschuwingen en machine learning gebaseerde rapporten die inconsistente toegangspatronen aangeven kunt u uw bedrijf te beveiligen. U kunt toegang tot Azure Active Directory- en gebruiksrapporten meer inzicht verkrijgen in de integriteit en beveiliging van de directory van uw organisatie. Met deze informatie kunt kan directory-beheerder beter bepalen waar mogelijk beveiligingsrisico's mogelijk liggen zodat ze voldoende plannen kunnen die risico's te beperken.

Rapporten zijn in de Azure portal onderverdeeld in de volgende manieren:

* Rapporten voor afwijkingsdetectie – bevatten aanmelden gebeurtenissen die we afwijkende worden gevonden. Ons doel is u rekening houden met deze activiteit en kunt u om te bepalen of een gebeurtenis verdacht is.
* Geïntegreerde toepassing rapporten – bieden inzicht in hoe cloud-toepassingen in uw organisatie worden gebruikt. Azure Active Directory biedt integratie met duizenden cloud-toepassingen.
* Foutenrapporten – duiden op fouten die zich kunnen voordoen bij het inrichten van accounts kunnen externe toepassingen.
* Apparaat/teken weergeven gebruikersspecifieke rapporten – in de activiteitsgegevens voor een specifieke gebruiker.
* Een record van alle controlegebeurtenissen bevatten activiteitenlogboeken – in de afgelopen 24 uur, afgelopen 7 dagen of afgelopen 30 dagen en wijzigingen in groep activiteiten en activiteit voor wachtwoord opnieuw instellen en registratie.

Meer informatie:

* [Uw toegangs- en gebruiksrapporten weergeven](../active-directory/active-directory-view-access-usage-reports.md)
* [Aan de slag met Azure Active Directory-rapportage](../active-directory/active-directory-reporting-getting-started.md)
* [Handleiding voor rapportage van Azure Active Directory](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Identiteits- en toegangsbeheer van consumenten
Azure Active Directory B2C is een maximaal beschikbare, wereldwijde, identity management-service voor consumententoepassingen voor honderden miljoenen identiteiten. De service kan worden geïntegreerd in zowel mobiele als webplatforms. Uw klanten kunnen zich bij al uw toepassingen aanmelden via verschillende aanpasbare methoden met hun bestaande sociale accounts of door nieuwe aanmeldingsgegevens te maken.

Voorheen schreven toepassingsontwikkelaars die consumenten in hun toepassingen wilden registreren en aanmelden, hun eigen code. Bovendien gebruikten ze on-premises databases of systemen om gebruikersnamen en wachtwoorden op te slaan. Azure Active Directory B2C biedt uw organisatie een betere manier identiteitsbeheer van consumenten integreren in toepassingen met behulp van een veilige, op standaarden gebaseerd platform en een grote set uitbreidbare beleidsregels.

Wanneer u Azure Active Directory B2C gebruikt, uw consumenten zich registreren voor uw toepassingen met behulp van hun bestaande sociale accounts (Facebook, Google, Amazon, LinkedIn) of door het maken van nieuwe referenties (e-mailadres en wachtwoord of gebruikersnaam en wachtwoord).

Meer informatie:

* [Wat is Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Azure Active Directory B2C preview: aanmelden en aanmelden van gebruikers in uw toepassingen](../active-directory-b2c/active-directory-b2c-overview.md)
* [Azure Active Directory B2C Preview: Typen toepassingen](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Apparaatregistratie
Azure AD-apparaatregistratie vormt de basis voor apparaatgebaseerde [voorwaardelijke toegang](../active-directory/active-directory-conditional-access-device-registration-overview.md) scenario's. Wanneer een apparaat is geregistreerd, biedt Azure Active Directory-apparaatregistratie het apparaat met een identiteit die wordt gebruikt voor verificatie van het apparaat wanneer de gebruiker zich aanmeldt. Het geverifieerde apparaat en de kenmerken van het apparaat kunnen vervolgens worden gebruikt voor het afdwingen van voorwaardelijk toegangsbeleid voor toepassingen die in de cloud en on-premises worden gehost.

In combinatie met een oplossing voor mobiele apparaten management (MDM) zoals Intune, worden de apparaatkenmerken in Azure Active Directory bijgewerkt met aanvullende informatie over het apparaat. Hiermee kunt u extra regels voor voorwaardelijke toegang maken die toegang afdwingen van apparaten, zodat ze voldoen aan uw standaarden voor beveiliging en compliance

Meer informatie:

* [Aan de slag met Azure Active Directory-apparaatregistratie](../active-directory/active-directory-conditional-access-device-registration-overview.md)
* [Automatische apparaatregistratie met Azure Active Directory voor Windows-domein-apparaten](../active-directory/active-directory-conditional-access-automatic-device-registration.md)
* [Instellen van automatische registratie van Windows-apparaten met Azure Active Directory-domein](../active-directory/active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="privileged-identity-management"></a>Beschermde identiteitsbeheer
Met Azure Active Directory (AD) Privileged Identity Management kunt u bevoegde identiteiten en de toegang tot bronnen in Azure AD en andere Microsoft Online Services, zoals Office 365 of Microsoft Intune, beheren en controleren.

Gebruikers moeten soms bij het uitvoeren van bevoorrechte bewerkingen in de resources in Azure of Office 365 of andere SaaS-apps. Dit betekent vaak dat organisaties hebben om hem permanente bevoegde toegang in Azure AD. Dit is een groeiende beveiligingsrisico voor cloud-gebaseerde bronnen omdat organisaties voldoende kunnen niet controleren wat gebruikers doen met hun beheerdersbevoegdheden. Als een gebruikersaccount met uitgebreide toegang is aangetast, kan die een inbreuk bovendien invloed op de algehele beveiliging van de cloud. Azure AD Privileged Identity Management helpt bij het oplossen van dit risico.

Azure AD Privileged Identity Management kunt u:

* Zien welke gebruikers zijn Azure AD-beheerders
* On-demand 'just in time' beheerderstoegang toewijzen voor Microsoft Online Services, zoals Office 365 en Intune inschakelen
* Rapporten over beheerder toegang tot geschiedenis en wijzigingen in beheerderstoewijzingen ophalen
* Ontvang waarschuwingen over de toegang tot een bevoorrechte rol

Meer informatie:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Rollen in Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-roles.md)
* [Azure AD Privileged Identity Management: Het toevoegen of verwijderen van een gebruikersrol](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Identiteitsbeveiliging
Azure AD Identity Protection is een beveiligingsservice waarmee een geconsolideerde weergave risicogebeurtenissen en mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie. Identity Protection maakt gebruik van bestaande Azure Active Directory van afwijkingsdetectie-detectiemogelijkheden (beschikbaar via Azure AD-rapporten voor afwijkende activiteiten) en introduceert nieuwe risico gebeurtenistypen die afwijkingen in realtime kunnen detecteren.

Meer informatie:

* [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md)
* [Channel 9: Azure AD en Identity weergeven: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Beheer van hybride identiteit
Aanpak van Microsoft voor identiteit omvat on-premises en de cloud, het maken van de identiteit van een enkele gebruiker voor verificatie en autorisatie voor alle netwerkbronnen, ongeacht de locatie.

Meer informatie:

* [Witboek van hybride identiteit](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Active Directory-teamblog](https://blogs.technet.microsoft.com/ad/)
