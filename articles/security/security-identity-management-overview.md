---
title: Azure beveiligingsfuncties die helpen met identity management | Microsoft Docs
description: " Dit artikel bevat een overzicht van de belangrijkste functies van Azure-beveiliging die u bij het identiteitsbeheer helpen. Identiteits- en toegangsbeheer management solutions Help bij Microsoft IT toegang beveiligen tot toepassingen en bronnen in het datacenter van de zakelijke en naar de cloud, extra niveaus van validatie, zoals meervoudige verificatie en voorwaardelijke toegang inschakelen beleidsregels. "
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 5aa0a7ac-8f18-4ede-92a1-ae0dfe585e28
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2018
ms.author: terrylan
Customer intent: As a IT Pro or decision maker I am trying to learn about identity management capabilities in Azure
ms.openlocfilehash: b58cbcdccced3952c3171daa14f80986e473c5dd
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56111489"
---
# <a name="azure-identity-management-security-overview"></a>Overzicht van Azure identity management-beveiliging

 Identiteitsbeheer is het proces van verifiëren en autoriseren [beveiligings-principals](https://docs.microsoft.com/windows/security/identity-protection/access-control/security-principals). Dit omvat ook controle over informatie over de beveiligings-principals (identiteiten). Beveiligings-principals (identiteiten), omvat mogelijk services, toepassingen, gebruikers, groepen, enzovoort. Identiteits- en toegangsbeheer management solutions Help bij Microsoft IT de toegang tot toepassingen en bronnen beveiligen via het zakelijke datacentrum en in de cloud. Deze beveiliging zorgt ervoor dat extra niveaus van validatie, zoals multi-factor Authentication en beleid voor voorwaardelijke toegang. Verdachte activiteiten controleren tot en met geavanceerde beveiliging voor rapportages, controle en waarschuwingen helpt bij het beperken mogelijke beveiligingsproblemen. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) biedt eenmalige aanmelding (SSO) voor duizenden cloudsoftware as a service (SaaS)-apps en de toegang tot web-apps die u on-premises uitvoert.
 
Door te profiteren van de voordelen van de beveiliging van Azure Active Directory (Azure AD), kunt u het volgende doen:

* Maken en beheren van één identiteit voor elke gebruiker binnen uw onderneming hybride, synchroon houden van gebruikers, groepen en apparaten. 
* SSO-toegang bieden tot uw toepassingen, waaronder duizenden vooraf geïntegreerde SaaS-apps.
* Inschakelen van beveiliging van de toegang tot toepassingen door het afdwingen van multi-factor Authentication op basis van regels voor zowel on-premises en cloudtoepassingen.
* Veilige externe toegang tot on-premises webtoepassingen via Azure AD Application Proxy richt.

Het doel van dit artikel is voor een overzicht van de belangrijkste functies van Azure-beveiliging die u bij het identiteitsbeheer helpen. We bieden ook koppelingen naar artikelen met details van elke functie, zodat u kunt meer informatie.  

Het artikel richt zich op de volgende mogelijkheden van core Azure Identity management:

* Eenmalige aanmelding
* Omgekeerde proxy
* Multi-Factor Authentication
* Op rollen gebaseerd toegangsbeheer (RBAC)
* Beveiligingsbewaking, meldingen en machine learning gebaseerde rapporten
* Identiteits- en toegangsbeheer van consumenten
* Apparaatregistratie
* Privileged identity management
* Identiteitsbeveiliging
* Hybride identiteit management/Azure AD connect
* Azure AD-toegangsbeoordelingen

## <a name="single-sign-on"></a>Eenmalige aanmelding

Eenmalige aanmelding betekent toegang te hebben tot alle toepassingen en bronnen die u nodig hebt om zaken te doen, aanmeldt slechts eenmaal met behulp van één gebruikersaccount. Nadat u bent aangemeld, u toegang hebt tot alle van de toepassingen die u nodig hebt zonder vereist is om te verifiëren (bijvoorbeeld, typ een wachtwoord) een tweede keer.

Er zijn veel organisaties afhankelijk van SaaS-toepassingen zoals Office 365, Box en Salesforce voor productiviteit van de gebruiker. In het verleden, IT-personeel die nodig zijn voor het afzonderlijk maken en bijwerken van gebruikersaccounts in elke SaaS-toepassing en moesten gebruikers een wachtwoord voor elke SaaS-toepassing.

Azure AD is een uitbreiding on-premises Active Directory-omgevingen in de cloud, zodat gebruikers hun primaire organisatie-account aan te melden bij niet alleen op hun domein apparaten en resources van het bedrijf gebruiken, maar ook voor alle web- en SaaS-toepassingen moeten voor hun werk.

Niet alleen gebruikers hoeven niet te beheren van meerdere sets met gebruikersnamen en wachtwoorden, kunt u inrichten of inrichting ongedaan maken toegang tot toepassingen automatisch op basis van hun groepen in de organisatie en de status van de werknemer. Azure AD bevat beveiligings- en toegangsbeheer voor governance waarmee u de toegang van gebruikers centraal voor SaaS-toepassingen beheren kunt.

Meer informatie:

* [Overzicht van eenmalige aanmelding](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [What is application access and single sign-on with Azure Active Directory?](../active-directory/manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding Azure Active Directory integreren met SaaS-apps](../active-directory/manage-apps/configure-single-sign-on-portal.md)

## <a name="reverse-proxy"></a>Omgekeerde proxy

Azure AD-toepassingsproxy, kunt u on-premises toepassingen, zoals publiceren [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) sites, [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx), en [IIS](http://www.iis.net/)-gebaseerde apps binnen uw particuliere netwerk en veilige toegang verleent aan gebruikers buiten uw netwerk. Application Proxy biedt externe toegang en eenmalige aanmelding voor veel soorten on-premises webtoepassingen met de duizenden SaaS-toepassingen die ondersteuning biedt voor Azure AD. Werknemers kunnen zich aanmelden bij uw apps op hun eigen apparaten van herkomst en via deze proxy cloud-gebaseerde te verifiëren.

Meer informatie:

* [Azure AD-toepassingsproxy inschakelen](../active-directory/manage-apps/application-proxy-enable.md)
* [Toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](../active-directory/active-directory-application-proxy-publish.md)
* [Eenmalige aanmelding met Application Proxy](../active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
* [Werken met voorwaardelijke toegang](../active-directory/manage-apps/application-proxy-integrate-with-sharepoint-server.md)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Azure multi-factor Authentication is een verificatiemethode waarbij het gebruik van meer dan één verificatiemethode is vereist en wordt een essentiële tweede beveiligingslaag toegevoegd aan gebruikersaanmeldingen en transacties. Multi-factor Authentication helpt beschermen toegang tot gegevens en toepassingen aan de wensen van gebruikers voor een eenvoudige aanmeldprocedure. Het biedt een robuuste verificatie met een scala aan opties voor verificatie: telefonische oproepen, SMS-berichten of mobiele app-meldingen of verificatiecodes en OAuth-tokens van derde partij.

Meer informatie:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Wat is Azure Multi-Factor Authentication?](../active-directory/authentication/multi-factor-authentication.md)
* [De werking van Azure multi-factor Authentication](../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="rbac"></a>RBAC

RBAC is een autorisatiesysteem dat is gebouwd op Azure Resource Manager die verfijnd toegangsbeheer van bronnen in Azure biedt. RBAC kunt u granulair bepalen het niveau van toegang die gebruikers hebben. U kunt bijvoorbeeld een gebruiker voor het beheren van alleen virtuele netwerken en een andere gebruiker voor het beheren van alle resources in een resourcegroep beperken. Azure bevat diverse ingebouwde rollen die u kunt gebruiken. Hier volgen vier fundamentele ingebouwde rollen. De eerste drie zijn op alle resourcetypen van toepassing.

Meer informatie:

* [Wat is toegangsbeheer op basis van rollen (RBAC)?](../role-based-access-control/overview.md)
* [Ingebouwde rollen voor Azure-resources](../role-based-access-control/built-in-roles.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Beveiligingsbewaking, meldingen en machine learning gebaseerde rapporten

Beveiligingsbewaking, meldingen en machine learning gebaseerde rapporten die inconsistente toegangspatronen identificeren kunt u uw bedrijf te beveiligen. U kunt Azure AD-toegang en gebruiksrapporten op inzicht in de integriteit en beveiliging van de adreslijst van uw organisatie. Met deze informatie bepalen de directorybeheerder van een beter waar mogelijke beveiligingsrisico mogelijk liggen zodat ze voldoende plannen maken kunnen die risico's te beperken.

In de Azure-portal rapporten kunnen worden onderverdeeld in de volgende categorieën:

* **Anomaliedetectie rapporten**: Aanmeldingsgebeurtenissen waarop we gevonden worden afwijkende bevatten. Ons doel is het u op de hoogte van deze activiteit maken en kunt u bepalen of een gebeurtenis verdacht is.
* **Geïntegreerde toepassing rapporten**: Bieden inzicht in hoe cloud-toepassingen in uw organisatie worden gebruikt. Azure AD biedt integratie met duizenden cloudtoepassingen.
* **Foutenrapporten**: Fouten die zich voordoen kunnen bij het inrichten van accounts voor externe toepassingen aangeven.
* **Rapporten voor specifieke gebruikers**: Apparaat aanmeldingsactiviteiten gegevens weergeven voor een specifieke gebruiker.
* **Activiteitenlogboeken**: Een record van alle gecontroleerde gebeurtenissen binnen de afgelopen 24 uur, afgelopen 7 dagen of afgelopen 30 dagen en groep is gewijzigd en het wachtwoord opnieuw instellen en registratie activiteit bevatten.

Meer informatie:

* [Uw toegangs- en gebruiksrapporten weergeven](../active-directory/active-directory-view-access-usage-reports.md)
* [Aan de slag met Azure Active Directory-rapportage](../active-directory/active-directory-reporting-getting-started.md)
* [Azure Active Directory-rapportagegids](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Identiteits- en toegangsbeheer van consumenten

Azure AD B2C is een maximaal beschikbare, wereldwijde, identiteitsbeheerservice voor consumentgerichte toepassingen die kan worden opgeschaald naar honderden miljoenen identiteiten. De service kan worden geïntegreerd in zowel mobiele als webplatforms. Uw consumenten kunnen al uw toepassingen aanpasbare manier aanmelden met hun bestaande sociale accounts of door nieuwe referenties te maken.

In het verleden zou ontwikkelaars van toepassingen die klanten registreren en aanmelden met hun toepassingen wilden hun eigen code hebt geschreven. Bovendien gebruikten ze on-premises databases of systemen om gebruikersnamen en wachtwoorden op te slaan. Azure AD B2C biedt uw organisatie een betere manier om identiteitsbeheer van consumenten integreren in toepassingen met behulp van een beveiligde, op standaarden gebaseerd platform en een grote set uitbreidbare beleidsregels.

Wanneer u Azure AD B2C gebruikt, kunnen uw consumenten zich registreren voor uw toepassingen met behulp van hun bestaande sociale accounts (Facebook, Google, Amazon, LinkedIn) of door nieuwe referenties (e-mailadres en wachtwoord of gebruikersnaam en wachtwoord) te maken.

Meer informatie:

* [Wat is Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Azure Active Directory B2C preview: Registreren en aanmelden bij de consumenten in uw toepassingen](../active-directory-b2c/active-directory-b2c-overview.md)
* [Azure Active Directory B2C Preview: Soorten toepassingen](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Apparaatregistratie

Azure AD-apparaatregistratie vormt de basis voor apparaatgebaseerde [voorwaardelijke toegang](../active-directory/active-directory-conditional-access-device-registration-overview.md) scenario's. Wanneer een apparaat is geregistreerd, biedt Azure AD-apparaatregistratie het apparaat een identiteit die wordt gebruikt voor het verifiëren van het apparaat wanneer een gebruiker zich aanmeldt. Het geverifieerde apparaat en de kenmerken van het apparaat kunnen vervolgens worden gebruikt om het afdwingen van beleid voor voorwaardelijke toegang voor toepassingen die worden gehost in de cloud en on-premises.

In combinatie met een oplossing voor het beheer van mobiele apparaten, zoals Intune, worden de apparaatkenmerken in Azure AD bijgewerkt met extra informatie over het apparaat. Vervolgens kunt u regels voor voorwaardelijke toegang die toegang van apparaten afdwingen om te voldoen aan uw normen voor beveiliging en naleving.

Meer informatie:

* [Aan de slag met Azure AD-apparaatregistratie](../active-directory/active-directory-conditional-access-device-registration-overview.md)
* [Automatische apparaatregistratie bij Azure AD voor Windows-domein gekoppelde apparaten](../active-directory/active-directory-conditional-access-automatic-device-registration.md)
* [Instellen van automatische inschrijving van Windows-domein gekoppelde apparaten met Azure AD](../active-directory/active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="privileged-identity-management"></a>Privileged identity management

Met Azure AD Privileged Identity Management, kunt u beheren, controleren en bewaken van uw bevoorrechte identiteiten en toegang tot resources in Azure AD, evenals andere Microsoft online services, zoals Office 365 en Microsoft Intune.

Soms moeten gebruikers bevoorrechte bewerkingen in Azure of Office 365-resources, of in andere SaaS-apps uitvoeren. In deze behoefte betekent vaak dat organisaties hebben gebruikers permanente bevoorrechte om toegang te geven in Azure AD. Deze toegang is een groeiende beveiligingsrisico voor cloud-gebaseerde bronnen, omdat organisaties voldoende kunnen niet controleren wat gebruikers doen met de administrator-bevoegdheden. Ook als een gebruikersaccount met bevoegde toegang is geknoeid, die een inbreuk invloed kan zijn op de algehele cloud-beveiliging van de organisatie. Azure AD Privileged Identity Management helpt om dit risico te beperken.

Met Azure AD Privileged Identity Management, kunt u het volgende doen:

* Zie welke Azure AD-beheerders zijn.
* Op aanvraag, just-in-time (JIT) met beheerdersrechten toegang tot Microsoft-services zoals Office 365 en Intune inschakelen.
* Rapporten over beheerder beheerderstoeganggeschiedenis en wijzigingen in toewijzingen van beheerder ophalen.
* Ontvang waarschuwingen over de toegang tot een bevoorrechte rol.

Meer informatie:

* [Wat is Azure AD Privileged Identity Management?](../active-directory/privileged-identity-management/pim-configure.md)
* [Azure AD-directory-rollen in PIM toewijzen](../active-directory/privileged-identity-management/pim-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Identiteitsbeveiliging

Azure AD Identity Protection is een beveiligingsservice die een geconsolideerde weergave risicogebeurtenissen en mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie biedt. Identity Protection maakt gebruik van bestaande Azure AD-anomaliedetectie-mogelijkheden, die beschikbaar zijn via Azure AD-afwijkende activiteit rapporten. Identity Protection introduceert ook nieuwe typen risicogebeurtenissen die in realtime afwijkingen kunnen.

Meer informatie:

* [Azure AD Identity Protection](../active-directory/active-directory-identityprotection.md)
* [Channel 9: Azure AD en Identity weergeven: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-managementazure-ad-connect"></a>Hybride identiteit management/Azure AD connect

De identiteitsoplossingen van Microsoft omvatten mogelijkheden voor zowel on-premises als in de cloud. Er wordt één gebruikersidentiteit gemaakt voor verificatie en autorisatie bij alle resources, ongeacht hun locatie. We noemen dit hybride identiteit. Azure AD Connect is het programma van Microsoft dat is ontworpen om te voldoen aan uw doelstellingen voor een hybride identiteit. Hiermee kunt u uw gebruikers een algemene identiteit bieden voor Office 365, Azure en SaaS toepassingen die zijn geïntegreerd met Azure AD. Deze biedt de volgende functies:

* Synchronisatie
* AD FS en Federatie-integratie
* Verificatie doorgeven
* Statuscontrole

Meer informatie:

* [Whitepaper voor hybride identiteit](https://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Azure AD-teamblog](https://blogs.technet.microsoft.com/ad/)

## <a name="azure-ad-access-reviews"></a>Azure AD-toegangsbeoordelingen

Met Azure AD-toegangsbeoordelingen (Azure Active Directory) kunnen organisaties op efficiënte wijze groepslidmaatschappen, de toegang tot bedrijfstoepassingen en de toewijzing van bevoorrechte rollen beheren.

Meer informatie:

* [Azure AD-toegangsbeoordelingen](../active-directory/governance/access-reviews-overview.md)
* [Gebruikerstoegang beheren met Azure AD-toegangsbeoordelingen](../active-directory/governance/access-reviews-overview.md)
