---
title: Azure-beveiliging waarmee met identity management-functies | Microsoft Docs
description: " Dit artikel bevat een overzicht van de belangrijkste functies van Azure-beveiliging die u bij het identiteitsbeheer helpen. Microsoft identiteits- en toegangsbeheer management oplossingen zorgen ervoor dat IT beveiligen toegang tot toepassingen en bronnen in het zakelijke datacenter en in de cloud, extra niveaus validatie, zoals multi-factor Authentication en voorwaardelijke toegang inschakelen beleid. "
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
ms.openlocfilehash: 8763f1dca110a43586619c09f5d25c340c177b09
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300654"
---
# <a name="azure-identity-management-security-overview"></a>Overzicht van Azure identity management-beveiliging
Microsoft identiteits- en toegangsbeheer management oplossingen zorgen ervoor dat IT beveiligen van toegang tot toepassingen en bronnen in het zakelijke datacenter en in de cloud. Deze beveiliging kan extra niveaus validatie, zoals multi-factor Authentication en beleidsregels voor voorwaardelijke toegang. Bewaking verdachte activiteiten via geavanceerde beveiliging rapportage, controle en waarschuwingen helpt bij het verminderen mogelijke beveiligingsproblemen. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) biedt eenmalige aanmelding (SSO) tot duizenden cloudsoftware als een service (SaaS)-apps en toegang tot web-apps die u on-premises uitgevoerd.

Door gebruik te maken van de voordelen van de beveiliging van Azure Active Directory (Azure AD), kunt u het volgende doen:

* Maken en beheren van één identiteit voor elke gebruiker in uw onderneming hybride, gebruikers, groepen en apparaten synchroon te houden.
* Eenmalige aanmeldingstoegang bieden tot toepassingen, inclusief duizenden vooraf geïntegreerde SaaS-apps.
* Toepassingsbeveiliging toegang inschakelen door het afdwingen van multi-factor Authentication op basis van regels voor zowel on-premises en cloudtoepassingen.
* Veilige externe toegang tot on-premises webtoepassingen via Azure AD-toepassingsproxy inrichten.

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
Eenmalige aanmelding betekent wordt toegang tot alle toepassingen en bronnen die u nodig hebt om zaken te doen, wanneer u zich aanmeldt slechts één keer met behulp van één gebruikersaccount. Wanneer u bent aangemeld, kunt u alle toepassingen zonder verificatie openen (bijvoorbeeld: Typ een wachtwoord) een tweede keer.

Veel organisaties zijn afhankelijk van de SaaS-toepassingen zoals Office 365, het selectievakje en Salesforce voor de productiviteit van de gebruiker. In het verleden hebben IT-personeel nodig afzonderlijk maken en bijwerken van gebruikersaccounts in elke SaaS-toepassing en moesten gebruikers een wachtwoord voor elke SaaS-toepassing.

Azure AD breidt on-premises Active Directory-omgevingen in de cloud, zodat gebruikers hun primaire organisatie-account aan te melden niet alleen op hun apparaten domein en bedrijfsbronnen gebruiken, maar ook op het web en SaaS-toepassingen ze nodig hebben voor hun werk.

Niet alleen gebruikers hoeven niet te beheren van meerdere sets met gebruikersnamen en wachtwoorden, u kunt inrichten of ongedaan inrichten toegang tot toepassingen automatisch op basis van hun organisatie groepen en de status van de werknemer. Azure AD introduceert beveiligings- en toegangsbeheer van governance waarmee u de toegang van gebruikers centraal via de SaaS-toepassingen beheren kunt.

Meer informatie:

* [Overzicht van eenmalige aanmelding](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../active-directory/manage-apps/what-is-single-sign-on.md)
* [Eenmalige aanmelding Azure Active Directory integreren met SaaS-apps](../active-directory/manage-apps/configure-single-sign-on-portal.md)

## <a name="reverse-proxy"></a>Omgekeerde proxy
Azure AD-toepassingsproxy kunt u on-premises toepassingen, zoals publiceren [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) sites, [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx), en [IIS](http://www.iis.net/)-gebaseerde apps binnen uw particuliere netwerk en veilig toegang verleent aan gebruikers buiten uw netwerk. Toepassingsproxy biedt externe toegang en web-SSO voor veel soorten lokale toepassingen met de duizenden SaaS-toepassingen die ondersteuning biedt voor Azure AD. Werknemers kunnen aanmelden bij uw apps op hun eigen apparaten home en verifiëren via deze cloud-gebaseerde proxy.

Meer informatie:

* [Azure AD-toepassingsproxy inschakelen](../active-directory/manage-apps/application-proxy-enable.md)
* [Toepassingen publiceren met Azure AD-toepassingsproxy](../active-directory/active-directory-application-proxy-publish.md)
* [Eenmalige aanmelding met toepassingsproxy](../active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
* [Werken met voorwaardelijke toegang](../active-directory/manage-apps/application-proxy-integrate-with-sharepoint-server.md)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Azure multi-factor Authentication is een authenticatiemethode die vereist het gebruik van meer dan één verificatiemethode en een kritieke tweede beveiligingslaag wordt toegevoegd aan de gebruikersaanmeldingen en transacties. Multi-factor Authentication helpt beveiliging toegang tot gegevens en toepassingen en te voldoen aan de behoeften van de gebruiker voor een eenvoudig proces aanmelden. Levert sterke verificatie via een aantal opties voor verificatie: telefoongesprekken, SMS-berichten of mobiele app-meldingen of verificatiecodes en OAuth-tokens van derde partij.

Meer informatie:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Wat is Azure Multi-Factor Authentication?](../active-directory/authentication/multi-factor-authentication.md)
* [De werking van Azure multi-factor Authentication](../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Beveiligingsbewaking, waarschuwingen en machine learning gebaseerde rapporten
Beveiligingsbewaking, waarschuwingen en machine learning gebaseerde rapporten die inconsistente toegangspatronen aangeven kunt u uw bedrijf te beveiligen. U kunt toegang van Azure AD en gebruik rapporten naar meer inzicht verkrijgen in de integriteit en beveiliging van de directory van uw organisatie. Met deze informatie kunt kan directory-beheerder beter bepalen waar mogelijk beveiligingsrisico's mogelijk liggen zodat ze voldoende plannen kunnen die risico's te beperken.

In de Azure portal rapporten kunnen worden onderverdeeld in de volgende categorieën:

* **Rapporten voor afwijkingsdetectie**: aanmelden gebeurtenissen die we afwijkende worden gevonden. Ons doel is u rekening houden met deze activiteit en kunt u bepalen of een gebeurtenis verdacht is.
* **Rapporten van toepassingen geïntegreerde**: inzicht in hoe cloud-toepassingen in uw organisatie worden gebruikt. Azure AD biedt integratie met duizenden cloud-toepassingen.
* **Foutenrapporten**: fouten die zich voordoen kunnen bij het inrichten van accounts kunnen externe toepassingen aangeven.
* **Rapporten van de gebruiker gedefinieerde**: apparaat aanmeldingsactiviteiten gegevens weer voor een specifieke gebruiker.
* **Activiteitenlogboeken**: bevatten een record van alle controlegebeurtenissen in de afgelopen 24 uur, afgelopen 7 dagen of afgelopen 30 dagen en wijzigingen activiteit en activiteit voor wachtwoord opnieuw instellen en registratie.

Meer informatie:

* [Uw toegangs- en gebruiksrapporten weergeven](../active-directory/active-directory-view-access-usage-reports.md)
* [Aan de slag met Azure Active Directory-rapportage](../active-directory/active-directory-reporting-getting-started.md)
* [Azure Active Directory-rapportagegids](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Identiteits- en toegangsbeheer van consumenten
Azure AD B2C is een maximaal beschikbare, wereldwijde, identity management-service voor consumententoepassingen voor honderden miljoenen identiteiten. De service kan worden geïntegreerd in zowel mobiele als webplatforms. Uw consumenten zich aanmelden bij uw toepassingen via aanpasbare met behulp van hun bestaande sociale accounts of maken van nieuwe referenties.

In het verleden schreven toepassingsontwikkelaars die klanten aanmelden en aanmelden bij hun toepassingen wilden hun eigen code. Bovendien gebruikten ze on-premises databases of systemen om gebruikersnamen en wachtwoorden op te slaan. Azure AD B2C, biedt uw organisatie een betere manier identiteitsbeheer van consumenten integreren in toepassingen met behulp van een veilige, op standaarden gebaseerd platform en een grote set uitbreidbare beleidsregels.

Wanneer u Azure AD B2C, uw consumenten zich registreren voor uw toepassingen met behulp van hun bestaande sociale accounts (Facebook, Google, Amazon, LinkedIn) of door het maken van nieuwe referenties (e-mailadres en wachtwoord of gebruikersnaam en wachtwoord).

Meer informatie:

* [Wat is Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Azure Active Directory B2C preview: aanmelden en aanmelden van gebruikers in uw toepassingen](../active-directory-b2c/active-directory-b2c-overview.md)
* [Azure Active Directory B2C Preview: Typen toepassingen](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Apparaatregistratie
Azure AD-apparaatregistratie vormt de basis voor apparaatgebaseerde [voorwaardelijke toegang](../active-directory/active-directory-conditional-access-device-registration-overview.md) scenario's. Wanneer een apparaat is geregistreerd, biedt Azure AD-apparaatregistratie het apparaat met een identiteit die wordt gebruikt voor het verifiëren van het apparaat wanneer een gebruiker zich aanmeldt. Het geverifieerde apparaat en de kenmerken van het apparaat kunnen vervolgens worden gebruikt voor het afdwingen van beleid voor voorwaardelijke toegang voor toepassingen die worden gehost in de cloud en on-premises.

In combinatie met een beheersysteem voor mobiele apparaten zoals Intune, worden de apparaatkenmerken in Azure AD bijgewerkt met aanvullende informatie over het apparaat. Vervolgens kunt u regels voor voorwaardelijke toegang die toegang afdwingen van apparaten om te voldoen aan uw standaarden voor beveiliging en naleving.

Meer informatie:

* [Aan de slag met Azure AD-apparaatregistratie](../active-directory/active-directory-conditional-access-device-registration-overview.md)
* [Automatische apparaatregistratie met Azure AD voor Windows-domein-apparaten](../active-directory/active-directory-conditional-access-automatic-device-registration.md)
* [Instellen van automatische registratie van Windows-apparaten die lid zijn van een domein met Azure AD](../active-directory/active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="privileged-identity-management"></a>Beschermde identiteitsbeheer
Met Azure AD Privileged Identity Management, kunt u, beheren en bewaken van bevoegde identiteiten en toegang tot bronnen in Azure AD, evenals andere Microsoft online services, zoals Office 365 en Microsoft Intune.

Gebruikers moeten soms bij het uitvoeren van bevoorrechte bewerkingen in Azure of Office 365-bronnen, of in andere SaaS-apps. Deze behoefte betekent vaak dat organisaties hebben gebruikers permanente bevoegde om toegang te geven in Azure AD. Deze toegang is een groeiende beveiligingsrisico voor cloud-gebaseerde bronnen, omdat organisaties voldoende kunnen niet controleren wat gebruikers doen met de administrator-bevoegdheden. Bovendien, als een gebruikersaccount met uitgebreide toegang is geknoeid, kan die een inbreuk op de beveiliging beïnvloeden van de organisatie algehele cloud. Azure AD Privileged Identity Management helpt om dit risico te verkleinen.

U kunt met Azure AD Privileged Identity Management:

* Zien welke gebruikers Azure AD-beheerders.
* Op aanvraag, just in time (Just in time) beheerderstoegang toewijzen voor Microsoft-services zoals Office 365 en Intune inschakelen
* Rapporten over beheerder toegang tot geschiedenis en wijzigingen in beheerderstoewijzingen ophalen.
* Ontvang waarschuwingen over de toegang tot een bevoorrechte rol.

Meer informatie:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Rollen in Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-roles.md)
* [Azure AD Privileged Identity Management: Het toevoegen of verwijderen van een gebruikersrol](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Identiteitsbeveiliging
Azure AD Identity Protection is een beveiligingsservice waarmee een geconsolideerde weergave risicogebeurtenissen en mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie. Identity Protection maakt gebruik van de bestaande Azure AD-anomaliedetectie-mogelijkheden, die beschikbaar zijn via Azure AD afwijkende activiteit rapporten. Identity Protection introduceert bovendien nieuwe risico gebeurtenistypen die afwijkingen in realtime kunnen detecteren.

Meer informatie:

* [Azure AD Identity Protection](../active-directory/active-directory-identityprotection.md)
* [Channel 9: Azure AD en Identity weergeven: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Beheer van hybride identiteit
De aanpak van Microsoft identity omvat on-premises en de cloud, het maken van de identiteit van een enkele gebruiker voor verificatie en autorisatie voor alle netwerkbronnen, ongeacht de locatie.

Meer informatie:

* [Witboek van hybride identiteit](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Azure AD-teamblog](https://blogs.technet.microsoft.com/ad/)
